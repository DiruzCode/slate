# Webhooks

Los [Webhooks](https://es.wikipedia.org/wiki/Webhook) son callbacks personalizados que cumplen la función de notifiación cuando ocurren [eventos](#eventos) en tu cuenta. 

Interactuar con una API de tercero como la de QVO puede introducir dos problemas:

- Servicios no directamente responsables de hacer una llamada a la API pueden que aún necesiten saber su respuesta.
- Algunos eventos, como los producidos por pagos recurrentes, no son resultados directos de llamadas a la API.

Los Webhooks resuelven estos problemas, al permitir registrar una URL a la cual notificaremos cada vez que un evento ocurra en tu cuenta. Cuando un evento ocurre, por ejemplo, cuando se realiza un pago exitoso de una suscripción, QVO crea un objeto de [evento](#eventos). Este objeto contiente toda la información relevante de lo ocurrido, incluido el tipo y los datos asociados a él. QVO luego envía el objeto de evento a la URL registrada para webhooks registrada en tu cuenta mediante una llamada HTTP POST. Puedes encontrar la lista completa de eventos [aquí](#tipos-de-evento).

Algunos casos de uso son:

- Actualizar la membresía de un cliente en tu base de datos cuando el pago de una suscripción es exitoso.
- Enviar un email a un cliente cuando el pago de su suscripción falla.
- Registrar una entrada en contabilidad cuando se realiza una transacción.

Sólo necesitas utilizar Webhooks para operacioens que ocurren "tras-bambalinas". El resultado de la mayoría de las llamadas de QVO, son reportados de manera síncrona a tu código y no necesitan de un Webhook para su verificación. Por ejemplo, cuando se cobra una tarjeta, una vez realizado el pago existosamente, se retornará inmediatamente la transacción, o si falla, se retornará un error.

### Recibiendo una notificación webhook

```javascript
// Using Express
app.post("/my/webhook/url", function(request, response) {
  // Retrieve the request's body and parse it as JSON
  var event_json = JSON.parse(request.body);

  // Do something with event_json

  response.send(200);
});
```

```ruby
require "json"

# Using Sinatra
post "/my/webhook/url" do
  # Retrieve the request's body and parse it as JSON
  event_json = JSON.parse(request.body.read)

  # Do something with event_json

  status 200
end
```

```python
import json
from django.http import HttpResponse

# Using Django
def my_webhook_view(request):
  # Retrieve the request's body and parse it as JSON
  event_json = json.loads(request.body)

  # Do something with event_json

  return HttpResponse(status=200)
```

Crear un endpoint para recibir webhooks, no es distinto a crear cualquier otra página en tu sitio. Basta con crear una nueva ruta con la URL deseada.

Los datos del webhook son enviado en formato JSON en el body o cuerpo de la llamada POST. Todos los detalles del evento están incluidos y pueden ser usados diréctamente (luego de parsear el JSON).

### Respondiendo a un webhook

Para acusar recibo del webhook, tu endpoint debe retornar un estado HTTP `200`. Cualquier otra información retornada en la cabecera o cuerpo de la llamada será ignorada. Cualquier respuesta que no sea código `200`, incluyendo códigos en el rango `3xx`, indicará a QVO que no se recibió el webhook.

Si un webhook no es recibido con éxito por cualquier razón, QVO continuará tratando de enviar el webhook cada hora por 7 días. Luego de este periodo, los webhooks no podrán volver a enviarse, pero se puede consultar la lista de eventos para reconciliar la información por eventos faltantes.

### Buenas prácticas

Antes de ir a producción, prueba que tu webhook esta funcionando de manera adecuada.

Si tu endpoint para webhooks ejecuta lógica compleja o realiza llamadas HTTP, es posible que se produzca un timeout antes de que QVO pueda ser notificado de la recepción. Por esta razón, es mejor acusar recibo inmediatamente del webhook retornando un código HTTP `200` y luego realizar el resto de las tareas.

Los endpoints de webhook pueden ocacionalmente recibir los mismos eventos más de una vez. Aconsejamos considerar estos casos para evitar la duplicación de los eventos, lo que puede provocar resultados inesperados.

Por razones de seguridad, se aconseja siempre verificar que la información proporcionada en el evento realmente existe y el evento ocurrió. Para ello simplemente basta con enviar un request a nuestra API a la ruta correspondiente del evento para corroborar que el recurso señalado se modificó. Este chequeo sirve para verifcar que la información viene desde QVO, evitando que potenciales atacantes envíen webhooks con información falsa.