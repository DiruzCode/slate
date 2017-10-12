# Eventos

Los eventos nos permiten comunicarte cambios relevantes en el sistema. Cuando ocurre un evento interesante, se crea un objeto evento. Por ejemplo, cuando se cobra una suscripción, se crea un evento `transaction.payment_succeeded`; cuando un pago falla, se crea un objeto `transaction.payment_failed`. Cabe mencionar que una llamada a la API puede generar 1 o más eventos. Por ejemplo, cuando se crea una nueva suscripción para un cliente, se crearán los eventos `customer.subscription.created` y `transaction.payment_succeeded`.

Los eventos pueden ser enviados diréctamente a tu servidor a través de la utilización de [webhooks](#webhooks). Puedes adminstrarlos en la configuración de tu cuenta.

## El objeto evento

> Ejemplo de respuesta

```json
{
  "id": "evt_fdyXA5uWS9F8mEwMXBPcNg",
  "type": "customer.updated",
  "data": {
    "customer": {
      "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
      "default_payment_method": null,
      "name": "No One",
      "email": "arya@stark.com",
      "created_at": "2017-05-22T20:39:10.370Z",
      "updated_at": "2017-05-22T20:55:47.860Z",
      "subscriptions": [],
      "cards": [],
      "transactions": []
    }
  },
  "previous": {
    "customer": {
      "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
      "default_payment_method": null,
      "name": "Arya Stark",
      "email": "arya@stark.com",
      "created_at": "2017-05-22T20:39:10.370Z",
      "updated_at": "2017-05-22T20:39:10.370Z",
      "subscriptions": [],
      "cards": [],
      "transactions": []
    }
  },
  "created_at": "2017-05-22T20:55:47.872Z"
}
```

### Atributos
|||
|---------: | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto. |
| type<p class="attr-desc">string</p> | Descripción de el evento (ej: `transaction.payment_succeeded`, `customer.card.created`, etc.). |
| data<p class="attr-desc">hash</p> | Objeto que contiene la información de los objetos asociados al evento. |
| previous<p class="attr-desc">hash</p> | Objeto que contiene la información previa de los objetos asociados al evento. Se encuentra en eventos donde se produce la actualización de un objeto. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto. |

### Tipos de evento
|||
|---------: | -----------|
| `customer.created` | Se creó un objeto de cliente. |
| `customer.updated` | Se actualizó un objeto de cliente. |
| `customer.deleted` | Se eliminó un objeto de cliente. |
| `plan.created` | Se creó un objeto de plan. |
| `plan.updated` | Se actualizó un objeto de plan. |
| `plan.deleted` | Se eliminó un objeto de plan. |
| `customer.card.created` | Se creó un objeto de tarjeta para un cliente. |
| `customer.card.deleted` | Se eliminó un objeto de tarjeta para un cliente. |
| `customer.subscription.created` | Se creó un objeto de suscripción para un cliente. |
| `customer.subscription.updated` | Se actualizó un objeto de suscripción para un cliente. |
| `customer.subscription.deleted` | Se eliminó un objeto de suscripción para un cliente. |
| `transaction.payment_succeeded` | Pago exitoso para una transacción. |
| `transaction.payment_failed` | Ocurrió un error en el pago de una transacción. |
| `transaction.refunded` | Ocurrió un reembolso en una trasacción. |




## Obtener un evento

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/events/evt_fdyXA5uWS9F8mEwMXBPcNg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/events/evt_fdyXA5uWS9F8mEwMXBPcNg', {
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/events/evt_fdyXA5uWS9F8mEwMXBPcNg'
p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/events/evt_fdyXA5uWS9F8mEwMXBPcNg')

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$response = $client->request('GET', 'https://api.qvo.cl/events/evt_fdyXA5uWS9F8mEwMXBPcNg', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
]);

var_dump($response->json());
?>
````

> Ejemplo de respuesta

```json
{
  "id": "evt_fdyXA5uWS9F8mEwMXBPcNg",
  "type": "customer.updated",
  "data": {
    "customer": {
      "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
      "default_payment_method": null,
      "name": "No One",
      "email": "arya@stark.com",
      "created_at": "2017-05-22T20:39:10.370Z",
      "updated_at": "2017-05-22T20:55:47.860Z",
      "subscriptions": [],
      "cards": [],
      "transactions": []
    }
  },
  "previous": {
    "customer": {
      "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
      "default_payment_method": null,
      "name": "Arya Stark",
      "email": "arya@stark.com",
      "created_at": "2017-05-22T20:39:10.370Z",
      "updated_at": "2017-05-22T20:39:10.370Z",
      "subscriptions": [],
      "cards": [],
      "transactions": []
    }
  },
  "created_at": "2017-05-22T20:55:47.872Z"
}
```

`GET /events/{event_id}`

Obtiene los detalles de un evento existente.

### Parámetros
|||
|---------: | -----------|
| event_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del evento. |


### Respuesta

Retorna un objeto de evento si se provee de un identificador válido. De lo contrario, retornará [un error](#errores).




## Obtener una lista de eventos

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/events" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/events', {
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/events'
p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/events')

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$response = $client->request('GET', 'https://api.qvo.cl/events', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
]);

var_dump($response->json());
?>
````

> Ejemplo de respuesta

```json
[
  {
    "id": "evt_fdyXA5uWS9F8mEwMXBPcNg",
    "type": "customer.updated",
    "data": {
      "customer": {
        "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
        "default_payment_method": null,
        "name": "No One",
        "email": "arya@stark.com",
        "created_at": "2017-05-22T20:39:10.370Z",
        "updated_at": "2017-05-22T20:55:47.860Z",
        "subscriptions": [],
        "cards": [],
        "transactions": []
      }
    },
    "previous": {
      "customer": {
        "id": "cus_hi9ycVF8p9WYLrvWkCCilg",
        "default_payment_method": null,
        "name": "Arya Stark",
        "email": "arya@stark.com",
        "created_at": "2017-05-22T20:39:10.370Z",
        "updated_at": "2017-05-22T20:39:10.370Z",
        "subscriptions": [],
        "cards": [],
        "transactions": []
      }
    },
    "created_at": "2017-05-22T20:55:47.872Z"
  }
]
```

`GET /events`

Retorna una lista de eventos. Los eventos se encuentran ordenadas por defecto por la fecha de creación, donde las mas recientes aparecerán primero.

<aside class="notice">
Este endpoint admite <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a un evento con su respectiva información.

