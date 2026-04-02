# Tarea Consumo de APIs - Fase 1 (API REST) - Juan Pablo Barreto

## 1. ¿Qué API elegiste y por qué?
Elegí la API de **iTunes Search**. Decidí utilizar esta API porque es muy robusta, cuenta con un catálogo inmenso de datos reales (música, videos, podcasts) y me permitió practicar el uso avanzado de *query params* (parámetros de consulta) para filtrar resultados sin necesidad de lidiar con configuraciones complejas de registro. Además, también intenté con las que estaban relacionadas con astronomía pero no me convenció la información que contenía. En vista de que soy una persona que escucha bastante música me pareció interesante esta API para poder realizar filtros por artistas.

## 2. ¿Qué datos devuelve?
La API devuelve información muy detallada en formato **JSON**. Entre los datos se incluyen metadatos de las canciones, álbumes y videos musicales, tales como el nombre del artista (`artistName`), el nombre de la pista o álbum (`trackName` / `collectionName`), URLs de las portadas (`artworkUrl`), precios, género musical y el contador total de resultados de la búsqueda (`resultCount`).

## 3. ¿Usa token o no? ¿Qué tipo?
**No**, esta API es completamente pública y abierta. Al ser de solo lectura para búsquedas de su catálogo, no requiere ningún tipo de token de autenticación (como Bearer Token o API Key) para realizar las consultas `GET`.

## 4. ¿Qué código de estado recibiste en cada request?
En las 5 peticiones que realicé (incluyendo búsquedas generales y consultas específicas por ID) recibí el código de estado **`200 OK`**. Esto fue validado automáticamente en Postman mediante los scripts de pruebas (`pm.response.to.have.status(200)`).

## 5. ¿Qué aprendiste diferente a JSONPlaceholder?
A diferencia de JSONPlaceholder (que es una API de prueba muy sencilla y predecible), con la API de iTunes aprendí a enfrentarme a un entorno real. Aprendí:
* Cómo usar múltiples *query params* encadenados con el símbolo `&` (por ejemplo, `?term=Nsqk&media=music` o `&limit=3`).
* A filtrar resultados por el tipo de entidad para evitar que la API me devolviera podcasts cuando yo solo quería canciones o álbumes (`entity=album`, `entity=musicVideo`).
* Que las APIs reales en producción utilizan rutas distintas para diferentes propósitos (usé `/search` para buscar texto y `/lookup` para buscar por ID específico, como en el caso del artista Feid).

---

## Capturas de mis Peticiones y Tests en Postman



* **Request 1:** GET Trae lista de canciones de Nsqk
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/45132df4-9e3c-4396-aee1-788926d4111b" />

* **Request 2:** GET Trae lista de álbumes de Tainy
 <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/44739fff-8949-4e90-a6c6-18c5fda2df79" />

* **Request 3:** GET Trae primeros 3 resultados de canciones de Álvaro Diaz
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/177c5a64-1644-4fdf-906a-b97fc5d95bcf" />

* **Request 4:** GET Trae exclusivamente videos musicales de José José
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/13214f23-55e2-4a16-9d92-baf6b5f68709" />

* **Request 5:** GET Trae información de Feid (por ID)
  <img width="1920" height="1200" alt="image" src="https://github.com/user-attachments/assets/fd16d2f0-fa63-41ab-9842-c1e4ee1e249f" />

