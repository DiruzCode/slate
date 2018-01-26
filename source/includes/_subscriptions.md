# Suscripciones

Las suscripciones permiten cobrar a un cliente de manera recurrente. Una suscripcion vincula un cliente con un plan previamente creado.

## El objeto suscripción

> Ejemplo de respuesta

```json
{
  "id": "sub_HnKU4UmU5GtymRulcVOEow",
  "status": "active",
  "debt": 0,
  "start": "2017-05-17T19:12:57.185Z",
  "current_period_start": "2017-05-17T19:12:57.185Z",
  "current_period_end": "2017-06-17T19:12:57.185Z",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastad@thewatch.org"
  },
  "plan": {
    "id": "oro",
    "name": "Plan Oro",
    "price": "15000.0",
    "currency": "CLP",
    "interval": "month",
    "interval_count": 1
  },
  "transactions": [
    {
      "id": "trx_t0dM-kSdEXs0g3YHY4sASQ",
      "gateway": "webpay_oneclick",
      "amount": 15000,
      "currency": "CLP",
      "status": "successful",
      "created_at": "2017-05-17T19:12:57.185Z"
    }
  ],
  "tax_name": "IVA",
  "tax_percent": "19.0",
  "created_at": "2017-05-17T19:12:57.189Z",
  "updated_at": "2017-05-17T19:12:57.189Z"
}
```

### Atributos
|||
|---------: | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto |
| status<p class="attr-desc">string</p> | El estado de la suscripcion. Puede ser: `active`, `canceled`, `trialing`, `retrying`, `inactive` y `unpaid`. Una suscripción que está en periodo de prueba, se encuentra en `trialing` y se mueve a `active` cuando el periodo de prueba termina. Cuando se falla un cobro para renovar la suscripción, pasa al estado `retrying` donde se reintentarán los cobros por un periodo determinado. Una vez que acaban los reintentos pasará al estado `unpaid`. Cuando una suscripción tiene una fecha de inicio en el futuro, esta `inactive` y pasará a `active` o `trialing` cuando llege esa fecha. Cuando se cancela una suscripción, tiene el estado `canceled`. |
| debt<p class="attr-desc">integer</p> | Deuda asociada a al suscripción. |
| start<p class="attr-desc">datetime</p> | Fecha de inicio de la suscripción. |
| current_period_start<p class="attr-desc">datetime</p> | Fecha de inicio del ciclo de facturación. |
| current_period_end<p class="attr-desc">datetime</p> | Fecha de término del ciclo de facturación. Al final de este periodo se realizará un cobro. |
| customer<p class="attr-desc">[Customer](#el-objeto-cliente)</p> | El cliente asociado a la suscripción. |
| plan<p class="attr-desc">[Plan](#el-objeto-plan)</p> | El plan asociado a la suscripción. |
| transactions<p class="attr-desc">Array<[Transaction](#el-objeto-transacci-n)></p> | Las transacciones asociadas a la suscripción. |
| tax_name<p class="attr-desc">string</p> | Nombre del impuesto de la suscripción. |
| tax_percent<p class="attr-desc">decimal</p> | El porcentaje de impuesto de la suscripción. Si existe, cada cobro generado por esta suscripción se aplicará este porcentaje por sobre el total, aumentando el monto cobrado al cliente. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto |
| updated_at<p class="attr-desc">datetime</p> | Fecha de la última actualización del objeto |


## Crear una suscripción

> Ejemplo de llamada

````shell
curl --request POST "https://api.qvo.cl/subscriptions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d customer_id="cus_qos_6r3-4I4zIiou2BVMHg" \
  -d plan_id="oro"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/subscriptions', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    customer_id: "cus_qos_6r3-4I4zIiou2BVMHg",
    plan_id: "oro"
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result =
  RestClient.post 'https://api.qvo.cl/subscriptions', {
    customer_id: "cus_qos_6r3-4I4zIiou2BVMHg",
    plan_id: "oro"
  }, {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/subscriptions', params={
  'customer_id': "cus_qos_6r3-4I4zIiou2BVMHg",
  'plan_id': "oro"
}, headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

```php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('POST', 'https://api.qvo.cl/subscriptions', [
  'json' => [
    'customer_id' => 'cus_qos_6r3-4I4zIiou2BVMHg',
    'plan_id' => 'oro'
  ],
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
```

> Ejemplo de respuesta

```json
{
  "id": "sub_HnKU4UmU5GtymRulcVOEow",
  "status": "active",
  "debt": 0,
  "start": "2017-05-17T19:12:57.185Z",
  "current_period_start": "2017-05-17T19:12:57.185Z",
  "current_period_end": "2017-06-17T19:12:57.185Z",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastad@thewatch.org"
  },
  "plan": {
    "id": "oro",
    "name": "Plan Oro",
    "price": "15000.0",
    "currency": "CLP",
    "interval": "month",
    "interval_count": 1
  },
  "tax_name": null,
  "tax_percent": null,
  "created_at": "2017-05-17T19:12:57.189Z",
  "updated_at": "2017-05-17T19:12:57.189Z"
}
```

`POST /subscriptions`

Crea una nueva suscripción para un cliente existente.

Al momento de crear una suscripción, se cobrará automáticamente el costo del plan al medio de pago por defecto del cliente, a menos que el plan posea días de prueba, sea gratis (precio igual a 0) o tenga una fecha de inicio en el futuro.


### Parámetros
|||
|---------: | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de un cliente. |
| plan_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de un plan. |
| start<p class="attr-desc">datetime</p> | Fecha de inicio de la suscripción. Debe ser una fecha en el futuro. |
| tax_name<p class="attr-desc">string</p> | Nombre del impuesto que se mostrará en el detalle de la transacción. Por ejemplo: IVA. |
| tax_percent<p class="attr-desc">decimal</p> | Un decimal no negativo entre 0 y 100. Esto rerpesenta el porcentaje de impuesto que se aplicará en el cobro de esta suscripción. Por ejemplo para un plan que cobra $10.000 con un `tax_percent` de 19.0, se cobrará $11.900. |


### Respuesta

Retorna un objeto de suscripción si la llamada es exitosa. Si el cliente no posee un medio de pago asociado por defecto o el cobro falla, retornará [un error](#errores), a menos que el plan sea gratis o posea un periodo de prueba.



## Obtener una suscripción

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
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
  RestClient.get 'https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

```php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', [
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
```

> Ejemplo de respuesta

```json
{
  "id": "sub_HnKU4UmU5GtymRulcVOEow",
  "status": "active",
  "debt": 0,
  "start": "2017-05-17T19:12:57.185Z",
  "current_period_start": "2017-05-17T19:12:57.185Z",
  "current_period_end": "2017-06-17T19:12:57.185Z",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastad@thewatch.org"
  },
  "plan": {
    "id": "oro",
    "name": "Plan Oro",
    "price": "15000.0",
    "currency": "CLP",
    "interval": "month",
    "interval_count": 1
  },
  "tax_name": "IVA",
  "tax_percent": "19.0",
  "created_at": "2017-05-17T19:12:57.189Z",
  "updated_at": "2017-05-17T19:12:57.189Z"
}
```

`GET /subscriptions/{subscription_id}`

Obtiene los detalles de una suscripción existente. Se necesita proporcionar sólo el identificador único de la suscripción el cual fue retornado al momento de su creación.

### Parámetros
|||
|---------: | -----------|
| subscription_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la suscripción. |


### Respuesta

Retorna un objeto de suscripción si se provee de un identificador válido.




## Actualizar una suscripción

> Ejemplo de llamada

````shell
curl --request PUT "https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d plan_id="platino"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
  method: 'PUT',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    plan_id: "platino"
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result =
  RestClient.put 'https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
    plan_id: "platino"
  }, {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.put('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', params={
  'plan_id': 'platino'
}, headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

```php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('PUT', 'https://api.qvo.cl/subscriptions', [
  'json' => [
    'plan_id' => 'platino'
  ],
  'headers' => [
    'Authorization' => 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  ]
])->getBody();

$response = json_decode($body);

var_dump($response);
?>
```


> Ejemplo de respuesta

```json
{
  "id": "sub_HnKU4UmU5GtymRulcVOEow",
  "status": "active",
  "debt": 35000,
  "start": "2017-05-17T19:12:57.185Z",
  "current_period_start": "2017-05-17T19:12:57.185Z",
  "current_period_end": "2017-06-17T19:12:57.185Z",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastad@thewatch.org"
  },
  "plan": {
    "id": "platino",
    "name": "Plan Platino",
    "price": "50000.0",
    "currency": "CLP",
    "interval": "month",
    "interval_count": 1
  },
  "tax_name": "IVA",
  "tax_percent": "19.0",
  "created_at": "2017-05-17T19:12:57.189Z",
  "updated_at": "2017-05-17T19:12:57.185Z"
}
```


`PUT /subscriptions/{subscription_id}`

Actualiza la suscripción especificada con los parametros provistos. Cualquier parámetro que no se provea quedará inalterado.

Cuando se cambia un plan de mayor precio, es necesario cobrar la diferencia del uso del nuevo plan por el periodo restante, el cual va desde fecha de actualización hasta la fecha de término del ciclo de facturación. Esto se almacenará en la suscripcipción como una **deuda** o `debt` y se cobrará al término del ciclo de facturación.

En el caso contrario, en el que el plan es de menor precio, se le otorgará al cliente **créditos** que serán descontados del próximo cobro.

### Parámetros
|||
|---------: | -----------|
| subscription_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la suscripción. |
| plan_id<p class="attr-desc">string</p> | Identificador único de un plan existente. |


### Respuesta

Retorna el objeto de suscripción si la actualización es exitosa. Retorna [un error](#errores) si algun parámetro de actualización es inválido.



## Cancelar una suscripción

> Ejemplo de llamada

````shell
curl -x DELETE "https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
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
  RestClient.delete 'https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('DELETE', 'https://api.qvo.cl/subscriptions/sub_HnKU4UmU5GtymRulcVOEow', [
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
  "id": "sub_HnKU4UmU5GtymRulcVOEow",
  "status": "active",
  "start": "2017-05-17T19:12:57.185Z",
  "current_period_start": "2017-05-17T19:12:57.185Z",
  "current_period_end": "2017-06-17T19:12:57.185Z",
  "cancel_at_period_end": true,
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "dabastad@thewatch.org"
  },
  "plan": {
    "id": "oro",
    "name": "Plan Oro",
    "price": "15000.0",
    "currency": "CLP",
    "interval": "month",
    "interval_count": 1
  },
  "tax_name": "IVA",
  "tax_percent": "19.0",
  "created_at": "2017-05-17T19:12:57.189Z",
  "updated_at": "2017-05-17T19:12:57.189Z"
}
```


`DELETE /subscriptions/{subscription_id}`

Cancela una suscripción. Por defecto, esta se cancelará (pasará al estado `canceled`) en la fecha especificada por el final del periodo de facturación o `current_period_end`.

### Parámetros
|||
|---------: | -----------|
| subscription_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la suscripción. |

### Respuesta

Retorna la suscripción si la operación fue exitosa con el parámetro `cancel_at_period_end = true`. Si el identificador de la suscripción no es válido, retornará [un error](#errores).



## Obtener una lista de suscripciones

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/subscriptions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/subscriptions', {
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
  RestClient.get 'https://api.qvo.cl/subscriptions', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/subscriptions', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/subscriptions', [
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
    "id": "sub_HnKU4UmU5GtymRulcVOEow",
    "status": "active",
    "start": "2017-05-17T19:12:57.185Z",
    "current_period_start": "2017-05-17T19:12:57.185Z",
    "current_period_end": "2017-06-17T19:12:57.185Z",
    "customer": {
      "id": "cus_qos_6r3-4I4zIiou2BVMHg",
      "name": "Jon Snow",
      "email": "dabastad@thewatch.org"
    },
    "plan": {
      "id": "oro",
      "name": "Plan Oro",
      "price": "15000.0",
      "currency": "CLP",
      "interval": "month",
      "interval_count": 1
    },
    "tax_name": "IVA",
    "tax_percent": "19.0",
    "created_at": "2017-05-17T19:12:57.189Z",
    "updated_at": "2017-05-17T19:12:57.189Z"
  }
]
```

`GET /subscriptions`

Retorna una lista de suscripciones activas. Las suscripciones se encuentran ordenadas por defecto por la fecha de creación, donde las mas recientes aparecerán primero.

<aside class="notice">
Este endpoint admite <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a una suscripción con su respectiva información.


