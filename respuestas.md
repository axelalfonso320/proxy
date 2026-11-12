# Ejercicio: Patrón Proxy en Java

**Alumno:** García Núñez Axel Alfonso

## Preguntas de análisis

### 1. Carga perezosa: ¿Cuándo se ejecuta el constructor de `RealImage` en el código original vs en el código con Proxy?

En el código original, el constructor de `RealImage` se ejecuta en el momento de crear el objeto (`new RealImage("photo.jpg")` en `Main`), por lo que la imagen se carga desde el disco aunque nunca se muestre.

Con el Proxy, el constructor de `RealImage` se ejecuta hasta la primera llamada a `display()`, cuando `ProxyImage` detecta que `realImage` es `null` y lo crea. Al crear el `ProxyImage` solo se guarda el nombre del archivo.

### 2. Transparencia: ¿El código cliente necesita saber si está trabajando con un Proxy o con el objeto real? ¿Por qué?

No. El cliente declara la variable como `Image` y solo llama a `display()`. Como `ProxyImage` y `RealImage` implementan la misma interfaz, el cliente no distingue entre uno y otro, y el cambio no requiere modificar la forma en que se usa el objeto.

### 3. Rendimiento: ¿Qué ventajas de rendimiento ofrece el patrón Proxy en este escenario?

- Si la imagen nunca se muestra, nunca se realiza la carga costosa.
- El inicio de la aplicación es más rápido, porque la carga se pospone hasta que es necesaria.
- Si `display()` se llama varias veces, la imagen se carga una sola vez y se reutiliza el objeto ya creado.

### 4. Control de acceso: Además de la carga perezosa, ¿qué otros tipos de control podríamos implementar en el Proxy?

- **Proxy de protección:** verificar permisos del usuario antes de cargar o mostrar la imagen.
- **Proxy de caché:** almacenar las imágenes ya cargadas para no volver a leerlas del disco.
- **Proxy de logging:** registrar cada acceso a una imagen.
- **Proxy remoto:** acceder a imágenes ubicadas en servidores remotos como si fueran locales.

### 5. Principio SOLID: ¿Qué principio SOLID se aplica al permitir que el Proxy y el `RealImage` implementen la misma interfaz?

Se aplica el **Principio de Sustitución de Liskov (LSP)**, porque `ProxyImage` puede sustituir a `RealImage` en cualquier lugar donde se espera un `Image` sin alterar el comportamiento esperado del programa.

También se relaciona con el **Principio de Inversión de Dependencias (DIP)**, ya que el cliente depende de la abstracción (`Image`) y no de una clase concreta.

## Pruebas realizadas

- **Sin Proxy:** el mensaje "Loading image..." aparece al crear el objeto.
- **Con Proxy:** el mensaje "Loading image..." aparece solo al llamar a `display()`.
- **Múltiples llamadas:** la carga ocurre una sola vez, aunque se llame varias veces a `display()`.

Salida obtenida:

```
Application started.
Proxy created, but RealImage not loaded yet.
Loading image (very expensive operation): photo.jpg
Displaying image: photo.jpg
Second call:
Displaying image: photo.jpg
```