# Webhooks

Los [Webhooks](https://es.wikipedia.org/wiki/Webhook) son callbacks que notifican cuando ocurren [eventos](#eventos)
en tu cuenta.

Por ejemplo: Cuando se genera un cobro recurrente de una suscripción, un Webhook te permite recibir una notificación para que puedas tomar una acción, como enviar un email de agradecimiento al usuario.

Los Webhooks son útiles en dos situaciones:

- Cuando se genera un evento que no es un resultado directo de una llamada a la API. Como por ejemplo el cobro de una suscripción.
- Cuando existen servicios o funcionalidades que necesitan saber la respuesta a una llamada, pero éstos no la realizan diréctamente. Como por ejemplo un servcio de contabilidad que necesita actualizar el registro cuando se genera una transacción.

Cuando un evento ocurre, QVO crea un objeto de [evento](#eventos). Este objeto contiente toda la información relevante de lo ocurrido. QVO luego envía el objeto de evento a través de una llamada HTTP POST a una URL que tú definas para ese propósito. Puedes registar la URL [aquí](https://dashboard.qvo.cl) y ver la lista completa de eventos [aquí](#tipos-de-evento).

Algunos casos de uso son:

- Actualizar la membresía de un cliente en tu base de datos cuando el pago de una suscripción es exitoso.
- Enviar un email a un cliente cuando el pago de su suscripción falla.
- Registrar una entrada en contabilidad cuando se realiza una transacción.

Sólo necesitas utilizar Webhooks para operaciones que ocurren "tras-bambalinas". El resultado de la mayoría de las llamadas de QVO, son reportados de manera síncrona a tu código y no necesitan de un Webhook para su verificación. Por ejemplo, cuando se cobra una tarjeta, una vez realizado el pago existosamente, se retornará inmediatamente la transacción, o si falla, se retornará un error.

### Recibiendo una notificación webhook

```javascript
// Using Express
app.post("/my/webhook/url", function(request, response) {
  // Retrieve the request's body and parse it as JSON
  const event_json = JSON.parse(request.body);

  // Do something with event_json

  response.send(200);
});
```

```ruby
require "json"

# Using Sinatra
post "/my/webhook/url" do
  # Retrieve the request's body and parse it as JSON
  event_json = JSON.parse request.body.read

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

````php
<?php
$body = @file_get_contents('php://input');
$event_json = json_decode($body);

// Do Something with $event_json

http_response_code(200);

// if PHP < 5.4
// header("HTTP/1.1 200 OK");
?>
````

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