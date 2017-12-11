# Transacciones

Las transacciónes representan movimientos en el sistema en torno a pagos. Estos pueden ser transaccionales o vinculados a un "transable", como una [suscripción](#suscripciones). Estos almacenan información como pagos, reembolsos y comisiones.

## El objeto transacción

```json
{
  "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
  "created_at": "2017-05-17T19:12:57.759Z",
  "amount": 3000,
  "currency": "CLP",
  "description": "For the Watch",
  "gateway": "webpay_oneclick",
  "fee": 372,
  "credits": 0,
  "status": "successful",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "lordcommander@thewatch.org"
  },
  "payment": {
    "amount": 3000,
    "gateway": "webpay_oneclick",
    "payment_type": "credit",
    "installments": 0,
    "payment_method": {
      "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    }
  },
  "refund": null,
  "transable": {
    "type": "Subscription",
    "id": "sub_HnKU4UmU5GtymRulcVOEow",
    "status": "active",
    "current_period_start": "2017-05-17T19:12:57.185Z",
    "current_period_end": "2017-06-17T19:12:57.185Z",
    "plan": {
      "id": "oro",
      "name": "Plan Oro",
      "price": "3000.0",
      "currency": "CLP"
    },
    "created_at": "2017-05-17T19:12:57.189Z",
    "updated_at": "2017-05-17T19:12:57.189Z"
  },
  "gateway_response": {
    "status": "success",
    "message": "successful transaction"
  }
}
```

### Atributos
|||
|---------: | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto. |
| amount<p class="attr-desc">integer</p> | Monto de la transacción. |
| currency<p class="attr-desc">string</p> | Código correspondiente a la moneda. Puede ser: `CLP` o `USD`. |
| description<p class="attr-desc">string</p> | Corresponde a la descripción de la transacción. Puede ser útil para identificar un producto u orden de compra del comercio. |
| gateway<p class="attr-desc">string</p> | Corresponde a la vía de pago por la cual se efectuó la transacción. Puede ser: `webpay_plus`, `webpay_oneclick` o `olpays`. |
| fee<p class="attr-desc">integer</p> | Comisión de la transacción. Corresponde a lo cobrado por QVO. |
| credits<p class="attr-desc">integer</p> | Créditos utilizados en la transacción. |
| status<p class="attr-desc">string</p> | Estado de la transacción. Puede ser: `successful`, `rejected`, `unable_to_charge`, `refunded` o `waiting_for_response`. Una transacción que está esperando el pago está `waiting_for_response` y pasa a `successful` si el pago es exitoso. Si no es exitoso, puede pasar a `unable_to_charge` si existió un problema con la vía de pago o `rejected` si fue rechazado por la misma. Por último, una transacción está en `refunded` si se ha reembolsado la totalidad del monto. |
| customer<p class="attr-desc">[Customer](#el-objeto-cliente)</p> | Cliente asociado a la transacción. |
| payment<p class="attr-desc">[Payment](#el-objeto-pago)</p> | Pago asociado a la transacción. Puede ser `null` |
| refund<p class="attr-desc">[Refund](#el-objeto-reembolso)</p> | Reembolzo asociado a la transacción. Puede ser `null` |
| transable<p class="attr-desc">hash</p> | Objeto transado en la transacción. Puede ser [Suscripción](#el-objeto-suscripci-n). |
| gateway_response<p class="attr-desc">hash</p> | Respuesta obtenida de la vía de la transacción. Contiene `status` y `message`. Es acá donde se expondrán detalles en caso de un error en la transacción. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto. |
| updated_at<p class="attr-desc">datetime</p> | Fecha de la última actualización del objeto. |

## El objeto pago

```json
{
  "amount": 3000,
  "gateway": "webpay_oneclick",
  "payment_type": "credit",
  "installments": 0,
  "payment_method": {
    "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
    "last_4_digits": "4242",
    "card_type": "VISA",
    "payment_type": "CD"
  }
}
```

### Atributos
|||
|---------: | -----------|
| amount<p class="attr-desc">integer</p> | Monto del pago. |
| gateway<p class="attr-desc">string</p> | Vía de pago. Puede ser: `webpay_plus`, `webpay_oneclick` o `olpays`. |
| payment_type<p class="attr-desc">string</p> | Tipo de pago. Puede ser: `credit` (crédito) o `debit` (débito) |
| installments<p class="attr-desc">integer</p> | Número de cuotas (sólo aplica a crédito). |
| payment_method<p class="attr-desc">[Card](#el-objeto-tarjeta)</p> | Medio de pago utilizado. Describe una [tarjeta](#el-objeto-tarjeta). |

## El objeto reembolso

```json
{
  "amount": 3000,
  "created_at": "2017-05-17T19:12:57.189Z"
}
```

### Atributos
|||
|---------: | -----------|
| amount<p class="attr-desc">integer</p> | Monto del reembolso. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del reembolso. |


## Obtener una transacción

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw', {
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
  RestClient.get 'https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````

````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw', [
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
  "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
  "created_at": "2017-05-17T19:12:57.759Z",
  "amount": 3000,
  "currency": "CLP",
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
      "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    }
  },
  "refund": null,
  "transable": {
    "type": "Subscription",
    "id": "sub_HnKU4UmU5GtymRulcVOEow",
    "status": "active",
    "current_period_start": "2017-05-17T19:12:57.185Z",
    "current_period_end": "2017-06-17T19:12:57.185Z",
    "plan": {
      "id": "oro",
      "name": "Plan Oro",
      "price": "3000.0",
      "currency": "CLP"
    },
    "created_at": "2017-05-17T19:12:57.189Z",
    "updated_at": "2017-05-17T19:12:57.189Z"
  },
  "gateway_response": {
    "status": "success",
    "message": "successful transaction"
  }
}
```

`GET /transactions/{transaction_id}`

Obtiene los detalles de una transacción existente.

### Parámetros
|||
|---------: | -----------|
| transaction_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la transacción. |


### Respuesta

Retorna un objeto de transacción si se provee de un identificador válido. De lo contrario, retornará [un error](#errores).



## Reembolsar una transacción

> Ejemplo de llamada

````shell
curl --request POST "https://api.qvo.cl/tramsactions/trx_Vk7WJYL-wYi4bjXmAaLyaw/refund" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=1000
````


````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw/refund', {
  method: 'POST',
  headers: {
    'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  },
  body: {
    amount: 1000
  }
}).then(function(response) {
  console.log(response);
});
````

````ruby
require 'rest-client'
require 'json'

result =
  RestClient.post 'https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw/refund', {
    amount: 1000
  }, {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw/refund', params={
  'amount': 1000
}, headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````


````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('POST', 'https://api.qvo.cl/transactions/trx_Vk7WJYL-wYi4bjXmAaLyaw/refund', [
  'json'=> [
    'amount' => 1000
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
  "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
  "created_at": "2017-05-17T19:12:57.759Z",
  "amount": 3000,
  "currency": "CLP",
  "gateway": "webpay_oneclick",
  "fee": 372,
  "credits": 0,
  "status": "refunded",
  "customer": {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "name": "Jon Snow",
    "email": "lordcommander@thewatch.org"
  },
  "payment": {
    "amount": 3000,
    "gateway": "webpay_oneclick",
    "payment_type": "credit",
    "installments": 0,
    "payment_method": {
      "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    }
  },
  "refund" : {
    "amount": 3000,
    "created_at": "2017-05-17T19:12:57.189Z"
  },
  "transable": {
    "type": "Subscription",
    "id": "sub_HnKU4UmU5GtymRulcVOEow",
    "status": "active",
    "current_period_start": "2017-05-17T19:12:57.185Z",
    "current_period_end": "2017-06-17T19:12:57.185Z",
    "plan": {
      "id": "oro",
      "name": "Plan Oro",
      "price": "3000.0",
      "currency": "CLP"
    },
    "created_at": "2017-05-17T19:12:57.189Z",
    "updated_at": "2017-05-17T19:12:57.189Z"
  },
  "gateway_response": {
    "status": "success",
    "message": "successful transaction"
  }
}
```

`POST /transactions/{transaction_id}/refund`

Crea un reembolso para una transacción existente. Se reembolzará la totalidad del monto pagado y los créditos correspondientes. Sólo se soportan reembolosos para los pagos en crédito de los gateway `webpay_plus` y `webpay_oneclick`

### Parámetros
|||
|---------: | -----------|
| transaction_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de una transacción. |

### Respuesta

Retorna un objeto de transacción con el objeto `refund` si la llamada es exitosa. Si la vía de pago no soporta reembolsos, retornará [un error](#errores).


## Obtener una lista de transacciones

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/transactions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const fetch = require('node-fetch-json');

fetch('https://api.qvo.cl/transactions', {
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
  RestClient.get 'https://api.qvo.cl/transactions', {
    Authorization: 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/transactions', headers={
  'Authorization': 'Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
})

print r.json()
````


````php
<?php
require 'guzzle.phar';

$client = new GuzzleHttp\Client();

$body = $client->request('GET', 'https://api.qvo.cl/transactions', [
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
    "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
    "created_at": "2017-05-17T19:12:57.759Z",
    "amount": 3000,
    "currency": "CLP",
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
        "id": "woc_m_c3zyh5BEl8EITxvLbMzw",
        "last_4_digits": "4242",
        "card_type": "VISA",
        "payment_type": "CD"
      }
    },
    "refund": null,
    "transable": {
      "type": "Subscription",
      "id": "sub_HnKU4UmU5GtymRulcVOEow",
      "status": "active",
      "current_period_start": "2017-05-17T19:12:57.185Z",
      "current_period_end": "2017-06-17T19:12:57.185Z",
      "plan": {
        "id": "oro",
        "name": "Plan Oro",
        "price": "3000.0",
        "currency": "CLP"
      },
      "created_at": "2017-05-17T19:12:57.189Z",
      "updated_at": "2017-05-17T19:12:57.189Z"
    },
    "gateway_response": {
      "status": "success",
      "message": "successful transaction"
    }
  }
]
```

`GET /transactions`

Retorna una lista de transacciones. Las transacciones se encuentran ordenadas por defecto por la fecha de creación, donde las mas recientes aparecerán primero.

<aside class="notice">
Este endpoint admite <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a una transacción con su respectiva información.

