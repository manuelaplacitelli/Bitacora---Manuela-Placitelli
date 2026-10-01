# Semana 8 - Uso de IA mediante APIs

## De una imagen a datos estructurados

En esta clase se trabajo con un ejemplo en el que se envía la foto de un ticket de compra a un modelo de inteligencia artificial y se obtiene como respuesta una tabla con los productos y sus montos.

El flujo general fue:

```text
Imagen del ticket
→ convertir imagen a un formato enviable
→ enviar solicitud a una API de IA
→ recibir respuesta
→ extraer el contenido
→ guardar como CSV
→ cargar como tabla
→ validar el resultado
```

La idea principal fue ver cómo un modelo de IA puede formar parte de un proceso de ciencia de datos y transformar información no estructurada, como una imagen, en datos tabulares.

## Uso de una API

Para comunicarnos con el modelo no utilizamos directamente una interfaz de chat, sino una **API**.

La API permite que un programa envíe una solicitud a un servicio externo y reciba una respuesta que luego puede seguir procesando automáticamente.

Para realizar la solicitud necesitamos:

- una dirección o endpoint;
- una clave de acceso;
- el modelo que queremos utilizar;
- una instrucción o prompt;
- los datos que queremos enviar.

## API Keys

La clave de API funciona como una credencial que identifica y autoriza el acceso al servicio.

Un punto importante de la clase fue que:

**las claves de API no deben subirse a GitHub.**

Por eso se guardan en un archivo separado y el programa las lee desde allí.

Esto permite separar las credenciales del código que sí puede compartirse.

## Conversión de la imagen a Base64

La imagen del ticket originalmente está almacenada como un archivo binario.

Para incluirla dentro de una solicitud JSON se convierte a **Base64**, que representa los bytes de la imagen mediante texto.

El proceso puede pensarse como:

```text
Imagen → bytes → Base64 → JSON
```

Base64 no analiza ni modifica la imagen: solamente cambia la forma en que se representa para poder transportarla dentro de la solicitud.

## El prompt como parte del proceso

También vimos que la instrucción enviada al modelo debe especificar claramente cuál es la salida esperada.

En este caso se pidió:

- transcribir los ítems del ticket;
- devolver solamente dos columnas: descripción y monto;
- utilizar formato CSV;
- normalizar los números;
- excluir subtotal, IVA, total, efectivo, cambio y tarjeta;
- no inventar información cuando algo no se puede leer.

Esto muestra que el prompt no sirve solamente para hacer una pregunta: también puede definir la **estructura y las reglas de los datos que queremos obtener**.

## JSON

La información enviada a la API se organiza utilizando **JSON**.

Dentro del cuerpo de la solicitud se incluyen:

- la instrucción;
- la imagen codificada;
- parámetros del modelo.

La API también devuelve su respuesta en JSON.

Por lo tanto, el flujo incluye dos conversiones:

```text
Objetos del programa → JSON → API
API → JSON → objetos del programa
```

## Solicitud HTTP POST

La información se envió mediante una solicitud HTTP de tipo `POST`.

A diferencia de una consulta simple, `POST` permite enviar un cuerpo con información, en este caso el prompt y la imagen.

Después de enviar la solicitud se revisa el **código de estado HTTP**.

Por ejemplo:

```text
200 → solicitud exitosa
429 → límite o cuota
503 → servicio temporalmente no disponible
```

Esto permite detectar si el problema está en nuestro código, en la autenticación, en los límites del servicio o en el servidor.

## Procesar la respuesta

Una vez recibida la respuesta, se extrae el texto generado por el modelo.

Como se solicitó que la respuesta estuviera en CSV, ese texto se puede guardar directamente en un archivo y luego cargar como una tabla.

El flujo final queda:

```text
Respuesta de IA
→ texto CSV
→ archivo .csv
→ DataFrame
```

Esto permite seguir trabajando con el resultado mediante las herramientas habituales de ciencia de datos.

## Validación de la respuesta

Aunque la IA genere una tabla correctamente estructurada, el resultado no debe asumirse automáticamente como correcto.

En el ejemplo se sumaron los montos extraídos y se compararon con el total impreso en el ticket.

Esto funciona como un control de calidad sencillo para detectar posibles errores de lectura o transcripción.

La idea importante es que **automatizar una extracción no elimina la necesidad de validar los datos obtenidos**.

## Tokens

También vimos que las APIs registran la cantidad de **tokens** utilizados.

Se distingue entre:

- tokens de entrada: imagen + instrucción;
- tokens de salida: respuesta generada.

El consumo de tokens es relevante porque puede estar relacionado con límites de uso, velocidad y costo del servicio.

## Comparación entre proveedores

Trabajamos con dos proveedores distintos para realizar prácticamente la misma tarea.

El flujo conceptual se mantiene:

```text
imagen
→ prompt
→ API
→ modelo
→ respuesta
→ CSV
```

Lo que cambia entre proveedores es principalmente:

- la URL de la API;
- la forma de autenticación;
- el nombre del modelo;
- la estructura del JSON;
- la ubicación de la respuesta dentro del JSON.

Esto muestra que el concepto de trabajar con una API es general, aunque cada servicio tenga una implementación diferente.

## Manejo de errores y reintentos

En uno de los ejemplos también se incorporaron reintentos automáticos frente a errores temporales como límites de uso o servidores saturados.

En lugar de detener inmediatamente el proceso, el programa puede:

```text
recibir error
→ esperar
→ volver a intentar
```

Esto permite construir procesos más robustos cuando se trabaja con servicios externos.

## Idea principal de la clase

La principal idea de esta clase fue entender cómo incorporar inteligencia artificial dentro de un flujo programado de datos.

No se utilizó la IA solamente para obtener una respuesta, sino como una etapa dentro de un proceso:

```text
dato no estructurado
→ IA
→ dato estructurado
→ validación
→ análisis
```

El ejemplo del ticket mostró cómo una imagen puede convertirse automáticamente en una tabla que luego puede seguir analizándose con R o Python.
