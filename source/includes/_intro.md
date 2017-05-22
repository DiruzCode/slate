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

> Tenemos disponibles varios SDK para distintos lenguajes que puedes encontrar <a href="#">aquí</a>.

Bienvenido a la API de QVO. Puedes usar nuestra API para acceder a los distintos endpoins de QVO, donde podrás efectuar pagos mediante distintos métodos y obtener información de ellos.

Tenemos bindings para **cURL**, **Ruby**, **Node.js** y **Python**! Puedes ver ejemplos de código en el área a la derecha, y puedes cambiar el lenguaje de los ejemplos arriba a la derecha.

El API esta organizado alrededor de <a href="https://es.wikipedia.org/wiki/Transferencia_de_Estado_Representacional">REST</a>. Nuestra API posee URLs predecibles y orientadas a recursos, y utiliza códigos de respuesta HTTP para indicar errores. Utilizamos características incluidas en el protocolo HTTP, como authenticación y verbos, los cuales son soportados por la gran mayoría de los clientes HTTP. Soportamos <a href="https://developer.mozilla.org/es/docs/Web/HTTP/Access_control_CORS">CORS</a>, lo cual permite interactuar de manera segura con nuestra API desde una aplicación web desde el lado del cliente. Todas las respuestas de la API retornan objetos JSON, incluyendo los errores, sin embargo, nuestros SDK convertirán las respuestas a objetos específicos de cada lenguaje.