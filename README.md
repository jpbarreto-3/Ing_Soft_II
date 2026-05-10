# Actividad: Patrones de Diseño en Python 🐍

Bienvenido a este repositorio. Aquí encontrarás una demostración práctica de cómo implementar y combinar diferentes patrones de diseño en Python, yendo desde implementaciones individuales hasta combinaciones arquitectónicas avanzadas.

---

## 📌 Propósito del proyecto

Este proyecto tiene como objetivo ilustrar de forma clara y directa cómo los patrones de diseño (creacionales, estructurales y de comportamiento) pueden resolver problemas comunes de software, y cómo su combinación permite crear sistemas altamente desacoplados y escalables.

---

# 🏗️ Estructura y Patrones Cubiertos

El proyecto está dividido en 4 niveles de complejidad:

1. **Patrón Creacional Base**
   - **Singleton:** Implementación pura para garantizar una única instancia global en la aplicación (ej. un sistema centralizado de Logging).

2. **Combinación de Creacionales**
   - **Singleton + Factory:** Un gestor centralizado de recursos (Singleton) que delega la creación de objetos complejos a una fábrica o constructor.

3. **Creacional + Estructural**
   - **Factory + Adapter:** Utilización de una fábrica para instanciar dinámicamente un adaptador, permitiendo integrar una librería externa o un sistema heredado sin acoplar el código cliente.

4. **Ecosistema Completo**
   - **Factory + Adapter + Strategy:** Un flujo avanzado donde se selecciona un algoritmo en tiempo de ejecución (Strategy), se adapta un servicio de un tercero (Adapter) y se centraliza la creación de las instancias requeridas (Factory).

---

# 1️⃣ Patrón Creacional: Singleton

## 📖 Definición breve

El patrón **Singleton** garantiza que una clase tenga una única instancia durante todo el ciclo de vida de la aplicación y proporciona un punto de acceso global a dicha instancia.

Es ideal para recursos compartidos como:

- Gestores de configuración
- Conexiones a bases de datos
- Sistemas de logging

---

## 💻 Ejemplo en Python

En este caso, crearemos un sistema centralizado de Logging. Cualquier parte de la aplicación que intente instanciar el `Logger` recibirá exactamente el mismo objeto.

```python
class Logger:
    _instancia = None

    def __new__(cls):
        # Si la instancia no existe, la creamos
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
            cls._instancia.historial = []  # Estado compartido

        # Retornamos la instancia existente o recién creada
        return cls._instancia

    def registrar(self, mensaje: str) -> None:
        self.historial.append(mensaje)
        print(f"[LOG] {mensaje}")


if __name__ == "__main__":
    # Simulamos dos partes distintas de la aplicación
    logger_modulo_a = Logger()
    logger_modulo_b = Logger()

    # Agregamos registros desde distintos módulos
    logger_modulo_a.registrar("Aplicación iniciada correctamente.")
    logger_modulo_b.registrar("Cargando variables de entorno...")

    print("\n--- Verificación del Singleton ---")
    print(f"¿Son el mismo objeto?: {logger_modulo_a is logger_modulo_b}")
    print(f"Historial compartido: {logger_modulo_b.historial}")
```
### 📝 Explicación



El método mágico __new__: En Python, este método es el verdadero constructor que asigna la memoria (a diferencia de __init__, que solo inicializa la instancia). Al sobrescribirlo, controlamos la creación del objeto.

El atributo de clase _instancia: Almacena la referencia a la única instancia permitida. Si es None, se crea y se asigna. Si ya tiene un valor, se devuelve esa misma referencia.

Comprobación: Al final del script, se demuestra que aunque creamos logger_modulo_a y logger_modulo_b, ambos apuntan al mismo espacio en memoria (is devuelve True) y comparten el mismo atributo historial.


---

## 📸 Evidencia 1

<img width="1918" height="978" alt="image" src="https://github.com/user-attachments/assets/5a04c7bf-a96c-43e7-b075-f0d069a415d6" />

---

# 2️⃣ Combinación de Creacionales: Singleton + Factory

## 📖 Definición breve

Esta combinación utiliza:

- **Singleton** para centralizar la gestión de un recurso global.
- **Factory** para delegar la creación de objetos específicos.

Esto mantiene el sistema organizado y desacoplado.

---

## 💻 Ejemplo en Python

Imaginemos un gestor de notificaciones centralizado que:

- Guarda un historial global.
- Utiliza una fábrica para crear notificaciones Email o SMS.

```python
from abc import ABC, abstractmethod

# --- 1. FACTORY ---
class Notificacion(ABC):
    @abstractmethod
    def enviar(self, mensaje: str) -> None:
        pass


class EmailNotificacion(Notificacion):
    def enviar(self, mensaje: str) -> None:
        print(f"[Email] Enviando: {mensaje}")


class SMSNotificacion(Notificacion):
    def enviar(self, mensaje: str) -> None:
        print(f"[SMS] Enviando: {mensaje}")


class NotificacionFactory:
    """Fábrica encargada de crear notificaciones."""

    @staticmethod
    def crear(tipo: str) -> Notificacion:
        if tipo.lower() == "email":
            return EmailNotificacion()

        elif tipo.lower() == "sms":
            return SMSNotificacion()

        raise ValueError("Tipo de notificación no soportado.")


# --- 2. SINGLETON ---
class GestorNotificaciones:
    """Punto de acceso global."""

    _instancia = None

    def __new__(cls):
        if cls._instancia is None:
            cls._instancia = super().__new__(cls)
            cls._instancia.historial = []

        return cls._instancia

    def procesar(self, tipo: str, mensaje: str) -> None:
        # Delegamos la creación al Factory
        notificacion = NotificacionFactory.crear(tipo)

        # Ejecutamos la acción
        notificacion.enviar(mensaje)

        # Guardamos en el historial global
        self.historial.append(f"{tipo.upper()}: {mensaje}")


if __name__ == "__main__":
    gestor_compras = GestorNotificaciones()
    gestor_seguridad = GestorNotificaciones()

    gestor_compras.procesar(
        "email",
        "Confirmación de tu compra."
    )

    gestor_seguridad.procesar(
        "sms",
        "Alerta: Nuevo inicio de sesión."
    )

    print("\n--- Verificación ---")
    print(
        f"¿Es el mismo gestor?: "
        f"{gestor_compras is gestor_seguridad}"
    )

    print(
        f"Historial centralizado: "
        f"{gestor_seguridad.historial}"
    )
```

### 📝 Explicación


Responsabilidades separadas: El GestorNotificaciones (Singleton) no sabe cómo se construye un Email o un SMS; su única responsabilidad es llevar el historial y coordinar. La lógica de creación pertenece exclusivamente a NotificacionFactory.

Escalabilidad: Si mañana necesitamos añadir notificaciones por WhatsApp, solo creamos la clase WhatsAppNotificacion y la añadimos al Factory. El GestorNotificaciones (Singleton) no sufre ninguna modificación.

Estado global seguro: Al ser un Singleton, podemos invocar a GestorNotificaciones() en cualquier archivo o módulo de nuestro proyecto y siempre estaremos añadiendo eventos a la misma lista de historial.


---

## 📸 Evidencia 2

<img width="1918" height="981" alt="image" src="https://github.com/user-attachments/assets/4e135a8c-7c9f-470b-a70e-d91fae3c9bab" />


---

# 3️⃣ Creacional + Estructural: Factory + Adapter

## 📖 Definición breve

El patrón **Adapter** permite que dos clases con interfaces incompatibles trabajen juntas.

Combinado con **Factory**, el sistema puede crear adaptadores dinámicamente sin que el cliente conozca los detalles internos.

---

## 💻 Ejemplo en Python

Usaremos un SDK externo de AWS S3 cuyo método no coincide con nuestra interfaz estándar.

El Adapter traducirá las llamadas de nuestro sistema al SDK externo.

```python
from abc import ABC, abstractmethod

# Interfaz estándar
class Almacenamiento(ABC):

    @abstractmethod
    def guardar(self, archivo: str) -> None:
        pass


# SDK externo
class AWSS3_SDK:

    def put_object(self, file_name: str) -> None:
        print(
            f"AWS S3: Subiendo '{file_name}' "
            f"a la nube de Amazon."
        )


# Adapter
class S3Adapter(Almacenamiento):

    def __init__(self, sdk: AWSS3_SDK) -> None:
        self.sdk = sdk

    def guardar(self, archivo: str) -> None:
        self.sdk.put_object(archivo)


# Factory
class AlmacenamientoFactory(ABC):

    @abstractmethod
    def crear(self) -> Almacenamiento:
        pass


class S3Factory(AlmacenamientoFactory):

    def crear(self) -> Almacenamiento:
        return S3Adapter(AWSS3_SDK())


if __name__ == "__main__":
    factory = S3Factory()

    storage = factory.crear()

    print("\n--- Procesando almacenamiento ---")

    storage.guardar("reporte_mensual.pdf")
```

### 📝 Explicación


* **Incompatibilidad resuelta:** El `AWSS3_SDK` es una librería externa que utiliza el método `put_object`, pero nuestra aplicación está diseñada para invocar siempre un método estándar llamado `guardar`.
* **El rol del Adapter:** La clase `S3Adapter` sirve como puente. Implementa nuestra interfaz estándar `Almacenamiento` y, en su interior, traduce esa llamada hacia el método específico del SDK.
* **El rol del Factory:** `S3Factory` se encarga de instanciar el adaptador ya configurado con el SDK. De esta forma, el código cliente queda completamente aislado de las dependencias externas; solo interactúa con la fábrica y pide guardar un archivo.
---

## 📸 Evidencia 3

<img width="1918" height="983" alt="image" src="https://github.com/user-attachments/assets/b302b826-b024-46af-b13b-fe3a0ede12e2" />

---


# 4️⃣ Ecosistema Completo: Strategy + Factory + Adapter

## 📖 Definición breve

Este ejemplo combina tres patrones:

- **Strategy** → Selecciona algoritmos dinámicamente.
- **Factory** → Centraliza la creación de objetos.
- **Adapter** → Integra servicios externos incompatibles.

El resultado es una arquitectura altamente modular y desacoplada.

---

## 💻 Ejemplo en Python

La aplicación:

1. Comprime un archivo utilizando una estrategia dinámica.
2. Crea un servicio de almacenamiento mediante una fábrica.
3. Usa un adaptador para subir el archivo a AWS S3.

```python
from abc import ABC, abstractmethod

# --- 1. STRATEGY ---
class EstrategiaCompresion(ABC):

    @abstractmethod
    def comprimir(self, archivo: str) -> str:
        pass


class CompresionZIP(EstrategiaCompresion):

    def comprimir(self, archivo: str) -> str:
        print(f"Comprimiendo '{archivo}' en ZIP...")
        return f"{archivo}.zip"


class CompresionGZIP(EstrategiaCompresion):

    def comprimir(self, archivo: str) -> str:
        print(f"Comprimiendo '{archivo}' en GZIP...")
        return f"{archivo}.gz"


# --- 2. FACTORY + ADAPTER ---
class Almacenamiento(ABC):

    @abstractmethod
    def guardar(self, archivo: str) -> None:
        pass


class AWSS3_SDK:

    def put_object(self, file_name: str) -> None:
        print(
            f"AWS S3: Archivo "
            f"'{file_name}' guardado exitosamente."
        )


class S3Adapter(Almacenamiento):

    def __init__(self, sdk: AWSS3_SDK) -> None:
        self.sdk = sdk

    def guardar(self, archivo: str) -> None:
        self.sdk.put_object(archivo)


class AlmacenamientoFactory(ABC):

    @abstractmethod
    def crear(self) -> Almacenamiento:
        pass


class S3Factory(AlmacenamientoFactory):

    def crear(self) -> Almacenamiento:
        return S3Adapter(AWSS3_SDK())


# --- 3. CLIENTE ---
class GestorArchivos:

    def __init__(
        self,
        estrategia: EstrategiaCompresion,
        factory: AlmacenamientoFactory
    ) -> None:

        self.estrategia = estrategia
        self.factory = factory

    def procesar_y_subir(self, archivo: str) -> None:

        # 1. Strategy
        archivo_comprimido = (
            self.estrategia.comprimir(archivo)
        )

        # 2. Factory + Adapter
        storage = self.factory.crear()

        # 3. Acción final
        storage.guardar(archivo_comprimido)


if __name__ == "__main__":

    print(
        "\n--- Ecosistema Completo ---"
    )

    gestor = GestorArchivos(
        CompresionGZIP(),
        S3Factory()
    )

    gestor.procesar_y_subir(
        "backup_db.sql"
    )
```

### 📝 Explicación


**Explicación**
* **Desacoplamiento total:** La clase principal `GestorArchivos` no sabe *cómo* se comprime un archivo ni *a qué nube* se va a subir. Su única responsabilidad es coordinar las interfaces abstractas.
* **Strategy en acción:** Al inyectar `CompresionGZIP()` en el gestor, definimos el comportamiento de compresión en tiempo de ejecución. Si mañana el cliente prefiere ZIP, solo pasamos `CompresionZIP()` sin modificar ni una sola línea de la clase `GestorArchivos`.
* **Integración fluida (El Ecosistema):** Tras transformar el archivo mediante la estrategia, el gestor utiliza la fábrica (`S3Factory`) para obtener el proveedor de almacenamiento (que internamente es el `S3Adapter`) y completa el flujo. Esto respeta el principio Abierto/Cerrado (Open/Closed Principle): podemos añadir nuevas estrategias o nuevos adaptadores de nube en el futuro sin romper el código existente.
---

## 📸 Evidencia 4

<img width="1918" height="987" alt="image" src="https://github.com/user-attachments/assets/f53a3c93-04ec-4d5c-8855-584597ac2c06" />

---

# ✅ Conclusión

Los patrones de diseño permiten construir aplicaciones:

- Más mantenibles
- Escalables
- Desacopladas
- Reutilizables

A medida que se combinan distintos patrones, es posible crear arquitecturas mucho más flexibles y preparadas para cambios futuros.

En este proyecto observamos cómo:

- **Singleton** centraliza recursos globales.
- **Factory** desacopla la creación de objetos.
- **Adapter** integra sistemas externos.
- **Strategy** permite intercambiar comportamientos dinámicamente.

La combinación de estos patrones demuestra cómo la programación orientada a objetos puede utilizarse para construir software limpio y profesional.

---
