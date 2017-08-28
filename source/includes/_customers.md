# Clientes

Los objetos de clientes permiten realizar cobros recurrentes y tener un registro de los mútliples cobros que estan asociados a un mismo cliente. El API permite crear, actualizar y eliminar clientes. Se puede obtener un cliente en particular, así como también una lista de clientes.

## El objeto cliente

> Ejemplo de respuesta

```json
{
  "id": "cus_qos_6r3-4I4zIiou2BVMHg",
  "default_payment_method": {
    "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
    "last_4_digits": "4242",
    "card_type": "VISA",
    "payment_type": "CD"
  },
  "name": "Jon Snow",
  "email": "dabastard@thewatch.org",
  "created_at": "2017-05-17T19:12:55.535Z",
  "updated_at": "2017-05-17T19:12:57.123Z",
  "subscriptions": [
    {
      "id": "sub_HnKU4UmU5GtymRulcVOEow",
      "status": "active",
      "plan": {
        "id": "test-plan-03",
        "name": "Test plan 03",
        "price": 3000,
        "currency": "CLP"
      },
      "current_period_start": "2017-05-17T19:12:57.185Z",
      "current_period_end": "2017-06-17T19:12:57.185Z",
      "created_at": "2017-05-17T19:12:57.189Z",
      "updated_at": "2017-05-17T19:12:57.189Z"
    }
  ],
  "cards": [
    {
      "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    }
  ],
  "transactions": [
    {
      "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
      "amount": 3000,
      "gateway": "Webpay Oneclick",
      "fee": 372,
      "credits": 0,
      "status": "successful",
      "created_at": "2017-05-17T19:12:57.759Z"
    }
  ]
}
```

### Atributos
|||
|---------: | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto |
| default_payment_method<p class="attr-desc">[Card](#el-objeto-tarjeta)</p> | Medio de pago por defecto del cliente. Describe una [tarjeta](#el-objeto-tarjeta). |
| name<p class="attr-desc">string</p> | Nombre del cliente |
| email<p class="attr-desc">string</p> | Dirección email del cliente |
| subscriptions<p class="attr-desc">Array<[Subscription](#el-objeto-suscripci-n)></p> | Subscripciones activas del cliente. |
| cards<p class="attr-desc">Array<[Card](#el-objeto-tarjeta)></p> | Tarjetas activas del cliente. |
| transactions<p class="attr-desc">Array<[Transaction](#el-objeto-transacci-n)></p> | Transacciones vinculadas al cliente. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto |
| updated_at<p class="attr-desc">datetime</p> | Fecha de la última actualización del objeto |


## Crear un cliente

> Ejemplo de llamada

````shell
curl --request POST "https://api.qvo.cl/customers" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d email="theimp@lannistercorp.gov" \
  -d name="Tyrion Lannister"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    email: "theimp@lannistercorp.gov",
    name: "Tyrion Lannister"
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.post 'https://api.qvo.cl/customers', params:
  {
    email: "theimp@lannistercorp.gov",
    name: "Tyrion Lannister"
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers', params={
  email: "theimp@lannistercorp.gov",
  name: "Tyrion Lannister"
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new Guzzle\Http\Client();

$response = $client->request('POST', 'https://api.qvo.cl/customers', [
  'json' => [
    'email' => 'theimp@lannistercorp.gov',
    'name' => 'Tyrion Lannister'
  ],
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
  "id": "cus_I-ZNs9TlY2FmdOUByQ5Ieg",
  "default_payment_method": null,
  "name": "Tyrion Lannister",
  "email": "theimp@lannistercorp.gov",
  "created_at": "2017-05-19T02:42:03.563Z",
  "updated_at": "2017-05-19T02:42:03.563Z",
  "subscriptions": [],
  "cards": [],
  "transactions": []
}
```

`POST /customers`

Crea un nuevo objeto cliente

### Parámetros
|||
|---------: | -----------|
| email<p class="attr-desc danger">Único</p><p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Dirección email del cliente. |
| name<p class="attr-desc">string</p> | Nombre del cliente. |

### Respuesta

Retorna un objeto de cliente si la llamada es exitosa. Si existe otro cliente con el mismo email, retornará [un error](#errores).


## Obtener un cliente

> Ejemplo de llamada

```shell
curl -X get "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', {
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

result = RestClient.get 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg'

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg')

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new Guzzle\Http\Client();

$response = $client->request('GET', 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', [
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
  "id": "cus_I-ZNs9TlY2FmdOUByQ5Ieg",
  "default_payment_method": null,
  "name": "Tyrion Lannister",
  "email": "theimp@lannistercorp.gov",
  "created_at": "2017-05-19T02:42:03.563Z",
  "updated_at": "2017-05-19T02:42:03.563Z",
  "subscriptions": [],
  "cards": [],
  "transactions": []
}
```

`GET /customers/{customer_id}`

Obtiene los detalles de un cliente existente. Se necesita proporcionar sólo el identificador único del cliente el cual fue retornado al momento de su creación.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |

### Respuesta

Retorna un objeto de cliente si se provee de un identificador válido.



## Actualizar un cliente

> Ejemplo de llamada

````shell
curl --request PUT "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d email="theimp@lannistercorp.gov" \
  -d name="Tyrion Lannister"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    email: "theimp@lannistercorp.gov",
    name: "Tyrion Lannister"
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.put 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', params:
  {
    email: "theimp@lannistercorp.gov",
    name: "Tyrion Lannister"
  }

p JSON.parse(result)
````

````python
import requests

r = requests.put('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', params={
  email: "theimp@lannistercorp.gov",
  name: "Tyrion Lannister"
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new Guzzle\Http\Client();

$response = $client->request('PUT', 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', [
  'json' => [
    'email' => 'theimp@lannistercorp.gov',
    'name' => 'Tyrion Lannister'
  ],
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
  "id": "cus_I-ZNs9TlY2FmdOUByQ5Ieg",
  "default_payment_method": null,
  "name": "Tyrion Lannister",
  "email": "theimp@lannistercorp.gov",
  "created_at": "2017-05-19T02:42:03.563Z",
  "updated_at": "2017-05-19T02:42:03.563Z",
  "subscriptions": [],
  "cards": [],
  "transactions": []
}
```

`PUT /customers/{customer_id}`

Actualiza el cliente especificado con los parametros provistos. Cualquier parámetro que no se provea quedará inalterado. Por ejemplo, si se provee el parametro **name**, este será el nuevo nombre del cliente.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| nombre<p class="attr-desc">string</p> | Nombre. |
| email<p class="attr-desc">string</p> | Dirección email. |
| default_payment_method_id<p class="attr-desc">string</p> | Identificador del medio de pago por defecto. Debe ser perteneciente a la lista de tarjetas del cliente. |

### Respuesta

Retorna el objeto del cliente si la actualización es exitosa. Retorna [un error](#errores) si algun parámetro de actualización es inválido (ej: identificador de medio de pago inválido).




## Eliminar un cliente

> Ejemplo de llamada

````shell
curl -x DELETE "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg', {
  method: 'DELETE',
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

result = RestClient.delete 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg'

p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg')

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new Guzzle\Http\Client();

$response = $client->request('DELETE', 'https://api.qvo.cl/customers.cus_I-ZNs9TlY2FmdOUByQ5Ieg', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
]);

var_dump($response->json());
?>
````

`DELETE /customers/{customer_id}`

Elimina permanentemente un cliente. Esta acción es irreversible. Si existieran suscripciones activas asociadas al cliente, estas se cancelarán inmediatamente.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |


### Respuesta

Retorna una respuesta sin contenido si el cliente fue eliminado con éxito. Si el identificador del cliente no es válido, retornará [un error](#errores).



## Obtener una lista de clientes

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/customers" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers', {
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

result = RestClient.get 'https://api.qvo.cl/customers'

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers')

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new Guzzle\Http\Client();

$response = $client->request('GET', 'https://api.qvo.cl/customers', [
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
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "default_payment_method": {
      "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    },
    "name": "Jon Snow",
    "email": "dabastard@thewatch.org",
    "created_at": "2017-05-17T19:12:55.535Z",
    "updated_at": "2017-05-17T19:12:57.123Z",
    "subscriptions": [
      {
        "id": "sub_HnKU4UmU5GtymRulcVOEow",
        "status": "active",
        "plan": {
          "id": "test-plan-03",
          "name": "Test plan 03",
          "price": 3000,
          "currency": "CLP"
        },
        "current_period_start": "2017-05-17T19:12:57.185Z",
        "current_period_end": "2017-06-17T19:12:57.185Z",
        "created_at": "2017-05-17T19:12:57.189Z",
        "updated_at": "2017-05-17T19:12:57.189Z"
      }
    ],
    "cards": [
      {
        "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
        "last_4_digits": "4242",
        "card_type": "VISA",
        "payment_type": "CD"
      }
    ],
    "transactions": [
      {
        "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
        "amount": 3000,
        "gateway": "Webpay Oneclick",
        "fee": 372,
        "credits": 0,
        "status": "successful",
        "created_at": "2017-05-17T19:12:57.759Z"
      }
    ]
  }
]
```

`GET /customers`

Retorna una lista de clientes. Los clientes se encuentran ordenados por defecto por la fecha de creación, donde los mas recientes aparecerán primero.

<aside class="notice">
Este endpoint admite <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a un cliente con su respectiva información.


