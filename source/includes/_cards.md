# Tarjetas

<img src="images/webpay_oneclick_banner.jpg" class="full-width-image" />

Es posible inscribir múltiples tarjetas para un cliente para luego cobrar.


## El objeto tarjeta

> Ejemplo de respuesta

```json
{
  "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
  "last_4_digits": "4242",
  "card_type": "VISA",
  "payment_type": "CD",
  "created_at": "2017-05-17T19:12:57.108Z"
}
```

### Atributos
|||
|---------: | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto. |
| last_4_digits<p class="attr-desc">string</p> | Los últimos 4 dígitos de la tarjeta. |
| card_type<p class="attr-desc">string</p> | Tipo de tarjeta. Puede ser: `VISA` o `MASTERCARD` |
| payment_type<p class="attr-desc">string</p> | Tipo de pago de la tarjeta. Puede ser: `CD` (crédito) o `DB` (débito) |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto. |




## Crear una inscripción de tarjeta

> Ejemplo de llamada

````shell
curl -X POST "https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d return_url="http://example.com/return"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    return_url: "http://example.com/return"
  }
})
.then(function(response) {
  console.log(response)
});
````

````ruby
require 'rest-client'
require 'json'

result =
  RestClient.post 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions', {
    return_url: "http://example.com/return"
  }, {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions', params={
  'return_url': 'http://example.com/return'
}, headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('POST', 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions', [
  'json' => [
    'return_url' => "http://example.com/return",
  ],
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````

> Ejemplo de respuesta

```json
{
  "inscription_uid": "woi_WZa9DgYQPzPtUqMsQdoNhQ",
  "redirect_url": "https://api.qvo.cl/webpay_oneclick/init_inscription/woi_WZa9DgYQPzPtUqMsQdoNhQ",
  "expiration_date": "2017-05-19T17:15:20.832Z"
}
```

`POST /customers/{customer_id}/cards/inscriptions`

Crea una inscripción de tarjeta para el cliente.

Permite a un cliente inscribir su tarjeta mediante la interfaz de Webpay Oneclick. Esto se divide en 3 pasos:

#### 1. Realizar la llamada y **redirigir** al cliente

Una vez realizada la llamada de inscripción de tarjeta, es necesario **redirigir** al cliente a el `redirect_url` retornado por la llamada, para iniciar el proceso de inscripción.

#### 2. Capturar la respuesta

Una vez terminado paso 1, se retornará el cliente con una llamada `GET` a el `return_url` proporcionado como parámetro en el paso 1. Esta llamada, irá acompañada de un parámetro llamado `uid` ubicado en la ruta, que representa el identificador único de la inscripción de tarjeta.

Por ejemplo, para `return_url = "http://www.example.com/return` se retornará el cliente a:

`GET http://www.example.com/return?uid=woi_WZa9DgYQPzPtUqMsQdoNhQ`

donde `uid` es igual a `woi_WZa9DgYQPzPtUqMsQdoNhQ`

#### 3. Obtener el resultado

Para verificar si la inscripción fue exitosa, se debe realizar una llamada a [obtener inscripción de tarjeta](#obtener-una-inscripci-n-de-tarjeta), utilizando el identificador único de inscripción `uid` retornado en el paso 2.

<aside class="warning">
La inscripción posee una fecha de expiración de <b>10 minutos</b> luego de su fecha de creación. Si se intenta acceder a <code>redirect_url</code> luego de este tiempo, retornará <a href="#errores">un error</a>.
</aside>

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| return_url<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | URL de retorno de la inscripción. |


### Respuesta

Retorna los parámetros para iniciar una inscripción de tarjeta.


## Obtener una inscripción de tarjeta

> Ejemplo de llamada

````shell
curl -X GET "https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions/woi_WZa9DgYQPzPtUqMsQdoNhQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions/woi_WZa9DgYQPzPtUqMsQdoNhQ', {
  method: 'GET',
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

result =
  RestClient.get 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions/woi_WZa9DgYQPzPtUqMsQdoNhQ', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions/woi_WZa9DgYQPzPtUqMsQdoNhQ', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/inscriptions/woi_WZa9DgYQPzPtUqMsQdoNhQ', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````

> Ejemplo de respuesta

```json
{
  "uid": "woi_WZa9DgYQPzPtUqMsQdoNhQ",
  "status": "succeeded",
  "card": {
    "id": "woc_bMz2iAH1mJ8M4cvv0b7IMA",
    "last_4_digits": "6623",
    "card_type": "Visa",
    "payment_type": "CD",
    "created_at": "2017-05-19T17:07:01.924Z"
  },
  "created_at": "2017-05-19T17:05:20.820Z",
  "updated_at": "2017-05-19T17:07:01.953Z"
}
```

`GET /customers/{customer_id}/cards/inscriptions/{inscription_uid}`


Obtiene el resultado de una inscripción de tarjeta para un cliente.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| inscription_uid<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la inscripción. |

### Respuesta

Retorna el estado de la inscripción y una [tarjeta](#el-objeto-tarjeta) de haber creado una.

Esta puede tener `status`:

  - `succeeded`: La inscripción ha sido un éxito y se adjunta `card` con la información de la tarjeta resultante
  - `failed`: La inscripción ha fracasado y se adjunta `error` con la información del error.



## Obtener una tarjeta

> Ejemplo de llamada

````shell
curl -X GET "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', {
  method: 'GET',
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

result =
  RestClient.get 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````


> Ejemplo de respuesta

````json
{
  "id": "woc_1Flhy3a-5UnARb7S4Ho3FQ",
  "last_4_digits": "0789",
  "card_type": "Visa",
  "payment_type": "CD",
  "created_at": "2017-05-19T01:37:44Z"
}
````

`GET /customers/{customer_id}/cards/{card_id}`

Obtiene los detalles de una tarjeta existente para el ciente. Se necesita proporcionar sólo el identificador único de la tarjeta el cual fue retornado al momento de su creación en la inscripción.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta. |

### Respuesta

Retorna un objeto de tarjeta si se provee de un identificador válido.




## Cobrar a una tarjeta


> Ejemplo de llamada

```shell
curl -X POST "https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/charge" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=3000 \
  -d description="For the watch"
```

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/charge', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    amount: 3000,
    description: 'For the watch'
  }
}).then(function(resposne) {
  console.log(resposne);
});
````

````ruby
require 'rest-client'
require 'json'

result =
  RestClient.post 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/charge', {
    amount: 3000,
    description: 'For the watch'
  }, {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/charge', params={
  'amount': 3000,
  'description': 'For the watch'
}, headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('POST', 'https://api.qvo.cl/customers/cus_qos_6r3-4I4zIiou2BVMHg/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/charge', [
  'json' => [
    'amount' => 3000,
    'description' => 'For the watch'
  ],
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````

> Ejemplo de respuesta:

```json
{
  "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
  "created_at": "2017-05-17T19:12:57.759Z",
  "amount": 3000,
  "currency": "CLP",
  "description": "For the watch",
  "gateway": "webpay_oneclick",
  "fee": 372,
  "credits": 0,
  "status": "successful",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastard@winterfell.com"
  },
  "payment": {
    "amount": 3000,
    "gateway": "webpay_oneclick",
    "payment_type": "credit",
    "installments": 0,
    "payment_method": {
      "id": "woc_bMz2iAH1mJ8M4cvv0b7IMA",
      "last_4_digits": "6623",
      "card_type": "Visa",
      "payment_type": "CD"
    }
  },
  "refund": null,
  "gateway_response": {
    "status": "success",
    "message": "successful transaction"
  }
}
```

`POST /customers/{customer_id}/cards/{card_id}/charge`

Este endpoint permite autorizar un cobro para una tarjeta en específico.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta del cliente. |
| amount<p class="attr-desc warning">Requerido</p><p class="attr-desc">integer</p> | Monto a cobrar. |
| description<p class="attr-desc">string</p> | Descripción de la transacción. |


### Respuesta

De ser exitosa la llamada, retorna [una transacción](#el-objeto-transacci-n). De lo contrario, retornará [un error](#errores).





## Eliminar una tarjeta

> Ejemplo de llamada

````shell
curl -X DELETE "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', {
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

result =
  RestClient.delete 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }
p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('DELETE', 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````


`DELETE /customers/{customer_id}/cards/{card_id}`

Elimina permanentemente una tarjeta de un cliente. Esta acción es irreversible.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta. |


### Respuesta

Retorna una respuesta sin contenido si la tarjeta fue eliminada con éxito. Si el identificador de la tarjeta no es válido, retornará [un error](#errores).



## Obtener una lista de tarjetas

> Ejemplo de llamada

````shell
curl -X get "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards', {
  method: 'GET',
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

result =
  RestClient.get 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
````

> Ejemplo de respuesta

```json
[
  {
    "id": "woc_1Flhy3a-5UnARb7S4Ho3FQ",
    "last_4_digits": "0789",
    "card_type": "Visa",
    "payment_type": "CD",
    "created_at": "2017-05-19T01:37:44Z"
  }
]
```

`GET /customers/{customer_id}/cards`

Retorna una lista de tarjetas para un cliente.

### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |

### Respuesta

Un arreglo donde cada entrada representa a una tarjeta del cliente con su respectiva información.


