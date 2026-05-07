# Laboratorio Fase 3: API y SSH

## 1) Clase

### Objetivo
Consumir la API localmente desde la terminal de Windows (PowerShell) y validar los endpoints de autenticación y gestión de tareas.

### Requests realizados

| # | Request | Endpoint | Status Code | Observación |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Register | `/auth/register` | 201 | Crea usuario y devuelve `userId` |
| 2 | Login | `/auth/login` | 200 | Devuelve `token` JWT |
| 3 | Create task | `/tasks` | 201 | Crea tarea con `status: pending` |
| 4 | Update task | `/tasks/:id` | 200 | Cambia estado a `completed` vía PUT |
| 5 | Delete task | `/tasks/:id` | 204 | Elimina la tarea y retorna sin contenido |

### Resultados

**Register**
Se registra un nuevo usuario y la API devuelve un mensaje de éxito con el identificador único.
* **Request:** `POST /auth/register`
* **Status code:** 201
* **Respuesta (JSON):**
```json
{
  "message": "Usuario creado",
  "userId": "1778120215703"
}
```

**Login**
Se autentica el usuario ("estudiante") y se obtiene el token JWT para los endpoints protegidos.
* **Request:** `POST /auth/login`
* **Status code:** 200
* **Respuesta (JSON):**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiIxNzc4MTIwMjE1NzAzIiwidXNlcm5hbWUiOiJlc3R1ZGlhbnRlIiwiZXhwIjoxNzc4MTIzODI1NzY0fQ.Tj7bzYQf36owb-NJ7ZMtRxWnR2JFfw8CcCV6XDqZB1w"
}
```

**Create task**
Se crea una tarea asociada al usuario autenticado usando el token en el Header.
* **Request:** `POST /tasks`
* **Status code:** 201
* **Respuesta (JSON):**
```json
{
  "id": "1778120280708",
  "title": "Tarea de prueba",
  "description": "Para probar PUT y DELETE",
  "status": "pending",
  "userId": "1778120215703",
  "createdAt": "2026-05-07T02:18:00.708Z"
}
```

**Update task (PUT)**
Se actualiza el estado de la tarea creada a `completed` usando su `id`.
* **Request:** `PUT /tasks/1778120280708`
* **Status code:** 200
* **Respuesta (JSON):**
```json
{
  "id": "1778120280708",
  "title": "Tarea de prueba",
  "description": "Para probar PUT y DELETE",
  "status": "completed",
  "userId": "1778120215703",
  "createdAt": "2026-05-07T02:18:00.708Z"
}
```

**Delete task**
Se elimina la tarea creada de la base de datos en memoria. El servidor responde sin contenido.
* **Request:** `DELETE /tasks/1778120280708`
* **Status code:** 204
* **Respuesta:** *No Content* (Consola limpia)

### Evidencia (capturas)
* **Register:** <img width="1918" height="230" alt="image" src="https://github.com/user-attachments/assets/522e70c5-23d3-4488-9c55-f92645452774" />
* **Login:** <img width="1918" height="222" alt="image" src="https://github.com/user-attachments/assets/c3bb8bc4-8146-4b54-ad80-2824dda7acd9" />
* **Create task:** <img width="1918" height="348" alt="image" src="https://github.com/user-attachments/assets/5174729f-dcf8-4fef-a4be-e72299d5b556" />
* **Update task:** <img width="1918" height="302" alt="image" src="https://github.com/user-attachments/assets/c0a0a2a4-a62e-4333-be94-12d8fcc6156f" />
* **Delete task:** <img width="1918" height="156" alt="image" src="https://github.com/user-attachments/assets/0fe04b45-6fb6-4377-a8e3-fc15a7f180c0" />


---

## 2) SSH

### Objetivo
Consumir la API desde otro dispositivo (Máquina Virtual con Kali Linux) simulando un entorno remoto mediante el uso de un túnel SSH inverso (Reverse Port Forwarding).

### Teoría breve
* **SSH** crea un canal cifrado y seguro entre dos equipos.
* El **Reverse Port Forwarding** (Túnel Inverso) toma un puerto de la máquina remota (Kali en este caso, puerto 8080) y redirige todo su tráfico hacia un puerto de la máquina local (Windows, puerto 3000).
* Esto permite ejecutar peticiones desde Kali hacia `http://localhost:8080` y que estas sean procesadas por la API en Node.js que corre en el host anfitrión.

### Requests realizados (Remotos)

| # | Request | Endpoint | Status Code | Observación |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Create task | `/tasks` | 201 | Tarea creada remotamente desde Kali a través del túnel |
| 2 | Update task | `/tasks/:id` | 200 | Actualización de estado desde Kali vía cURL |

### Resultados

**Túnel SSH**
Se identificó la IP de la máquina virtual (Red Host-Only: `192.168.56.101`) y se abrió el túnel desde Windows usando el comando:
```powershell
ssh -R 8080:localhost:3000 kali@192.168.56.101
```

A partir de este momento, las peticiones ejecutadas en la terminal de Kali contra el puerto `8080` viajan por el túnel hacia la API local en el puerto `3000`.

**Validación de endpoints remotos**
Se confirmó que el token JWT viaja correctamente por el túnel SSH y permite el acceso. Se creó una tarea remota mediante cURL desde la terminal de Kali.
* **Request desde Kali:** `POST http://localhost:8080/tasks`
* **Status code:** 201
* **Respuesta (JSON) recibida en Kali:**
```json
{
  "id": "1778120853489",
  "title": "Tarea final desde Kali",
  "description": "Túnel SSH funcionando al 100%",
  "status": "pending",
  "userId": "1778120215703",
  "createdAt": "2026-05-07T02:27:33.489Z"
}
```

### Evidencia (capturas)
* **Conexión SSH:** <img width="1915" height="407" alt="image" src="https://github.com/user-attachments/assets/913754dc-0bfb-450d-a7d1-ddcaeab126e5" />
* **Create Task Remoto:** <img width="1918" height="420" alt="image" src="https://github.com/user-attachments/assets/d2daef08-06b9-4046-a741-2d0ff99d45a7" />
* **Updaate Task Remoto:** <img width="1918" height="373" alt="image" src="https://github.com/user-attachments/assets/d85ac336-b2d6-4346-950b-3458d8d179ba" />

