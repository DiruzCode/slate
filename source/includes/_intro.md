# Introducción

```
 _          _ _
| |__   ___| | | ___
| '_ \ / _ \ | |/ _ \
| | | |  __/ | | (_) |
|_| |_|\___|_|_|\___/     _
__      _____  _ __| | __| |
\ \ /\ / / _ \| '__| |/ _` |
 \ V  V / (_) | |  | | (_| |
  \_/\_/ \___/|_|  |_|\__,_|
```



Bienvenido al API de QVO. Puedes usar nuestra API para acceder a los distintos endpoins de QVO, donde podrás generar y gestionar pagos mediante distintos métodos y obtener información de ellos.

El API esta organizado alrededor de [REST](https://es.wikipedia.org/wiki/Transferencia_de_Estado_Representacional). Nuestra API posee URLs predecibles y orientadas a recursos, y utiliza códigos de respuesta HTTP para indicar el resultado de la llamada. Todas las respuestas de la API retornan objetos JSON, incluyendo los errores, sin embargo, nuestros SDK convertirán las respuestas a objetos específicos de cada lenguaje.

Utilizamos características incluidas en el protocolo HTTP, como authenticación y verbos, los cuales son soportados por la gran mayoría de los clientes HTTP. Soportamos [CORS](https://developer.mozilla.org/es/docs/Web/HTTP/Access_control_CORS), lo cual permite interactuar de manera segura con nuestra API desde una aplicación web desde el lado del cliente.

Tenemos bindings para **cURL**, **PHP**, **Ruby**, **Node.js** y **Python**! Puedes ver ejemplos de código en el área a la derecha, y puedes cambiar el lenguaje de los ejemplos arriba a la derecha.