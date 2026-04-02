# Tarea Consumo de APIs - Fase 2 (GraphQL) - Juan Pablo Barreto


### ¿Qué diferencia encontraste vs REST? 
La principal diferencia es la flexibilidad y la eficiencia. En REST, tuve que usar múltiples endpoints (URLs) dependiendo de lo que quería buscar, y muchas veces la API devuelve un montón de datos que no necesito (a esto se le llama *Overfetching*). Con GraphQL, usé **un único endpoint** (`https://countries.trevorblades.com/graphql`) siempre con el método **`POST`**, y la gran ventaja es que yo pude escribir en la consulta (Query) exactamente los campos específicos que quería recibir, logrando respuestas mucho más limpias.

### ¿Cuántos requests REST necesitarías para reemplazar tu query más compleja? 
Mi query más compleja fue la "Query combinada", donde pedí el continente Sudamérica ("SA") y dentro de esa misma petición, la lista de todos sus países con sus respectivas capitales. En una API REST tradicional, probablemente habría necesitado **al menos 2 peticiones (o más)**: una para obtener la información de la región, y otra para buscar los países filtrando por esa región. Además, en REST la respuesta incluiría datos irrelevantes (población, banderas, fronteras), mientras que en GraphQL lo resolví con **1 solo request** trayendo estrictamente lo necesario.

### ¿En qué proyecto real usarías GraphQL? 
Usaría GraphQL en proyectos donde el rendimiento de la red y el consumo de datos sean críticos, como en **aplicaciones móviles**. Al permitir pedir múltiples recursos relacionados en una sola llamada y evitar descargar datos de sobra, la app carga mucho más rápido. También sería excelente para aplicaciones de tipo *Dashboard* o redes sociales, donde una sola pantalla necesita mostrar información muy interconectada (por ejemplo, el perfil de un usuario, su lista de amigos y sus últimas publicaciones al mismo tiempo).

---

## Capturas de mis Peticiones y Tests en Postman

* **Request 1:** Query básica (Lista de todos los continentes)
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/6ab19a2d-f6ac-4402-8adf-a6f009c6d512" />

* **Request 2:** Query con filtro por argumento (Datos de Colombia con el código "CO")
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/38f9fb8d-e1cc-446a-858c-c5088a74b324" />

* **Request 3:** Query anidada (País México, su continente y lenguajes nativos)
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/d0869052-651f-423c-b82c-d84182ebd8d1" />

* **Request 4:** Query de lista completa (Todos los idiomas del mundo)
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/e421a91c-fc0b-4b95-bfd7-455835902350" />

* **Request 5:** Query combinada (Continente Sudamérica y la lista de todos sus países con sus capitales)
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/c63db858-6712-4843-ad28-aa603b77e48f" />
