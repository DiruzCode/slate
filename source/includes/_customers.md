# Clientes

Los objetos de clientes permiten realizar cobros recurrentes y tener un registro de los mútliples cobros que estan asociados a un mismo cliente. El API permite crear, actualizar y eliminar clientes. Se puede obtener un cliente en particular, así como también una lista de clientes.

## El objeto cliente

> Ejemplo de respuesta

```json
{
  "id": "cus_qos_6r3-4I4zIiou2BVMHg",
  "default_payment_method": {
    "id": "opc_m_c3zyh5BEl8EITxvLbMzw",
    "last_4_digits": "4242",
    "card_type": "VISA",
    "payment_type": "CD"
  },
  "name": "John Snow",
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
        "price": "3000.0",
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
      "id": "opc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    }
  ],
  "transactions": [
    {
      "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
      "amount": "3000.0",
      "gateway": "olpays",
      "fee": "371.07",
      "credits": "0.0",
      "status": "successful",
      "created_at": "2017-05-17T19:12:57.759Z"
    }
  ]
}
```

### Atributos
|||
|--------- | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto |
| default_payment_method<p class="attr-desc"><a href="#el-objeto-tarjeta">Card</a></p> | Medio de pago por defecto del cliente. Describe una <a href="#el-objeto-tarjeta">Tarjeta</a>. |
| name<p class="attr-desc">string</p> | Nombre del cliente |
| email<p class="attr-desc">string</p> | Dirección email del cliente |
| subscriptions<p class="attr-desc">Array<<a href="#el-objeto-suscripci-n">Subscription</a>></p> | Subscripciones activas del cliente. |
| cards<p class="attr-desc">Array<<a href="#el-objeto-tarjeta">Card</a>></p> | Tarjetas activas del cliente. |
| transactions<p class="attr-desc">Array<<a href="#el-objeto-transacci-n">Transaction</a>></p> | Transacciones vinculadas al cliente. |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto |
| updated_at<p class="attr-desc">datetime</p> | Fecha de la última actualización del objeto |


## Crear cliente

> Ejemplo de llamada

````shell
curl --request POST "https://api.qvo.cl/customers" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d email="theimp@lannistercorp.gov" \
  -d name="Tyrion Lannister"
````


````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers', { method: 'POST'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.post 'https://api.qvo.cl/customers', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers', params={
  # TODO
})

print r.json()
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
|--------- | -----------|
| email<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Dirección email del cliente. |
| name<p class="attr-desc">string</p> | Nombre del cliente. |

### Respuesta

Retorna un objeto de cliente si la llamada es exitosa. Si existe otro cliente con el mismo email, retornará <a href="#errores">un error</a>.


## Obtener un cliente

> Ejemplo de llamada

```shell
curl -X get "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/{customer_id}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/{customer_id}', params={
  # TODO
})

print r.json()
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
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |

### Respuesta

Retorna un objeto de cliente si se provee de un identificador válido.



## Actualizar cliente

> Ejemplo de llamada

````shell
curl --request PUT "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d email="theimp@lannistercorp.gov" \
  -d name="Tyrion Lannister"
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}', { method: 'PUT'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.put 'https://api.qvo.cl/customers/{customer_id}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.put('https://api.qvo.cl/customers/{customer_id}', params={
  # TODO
})

print r.json()
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
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| nombre<p class="attr-desc">string</p> | Nombre. |
| email<p class="attr-desc">string</p> | Dirección email. |
| default_payment_method_id<p class="attr-desc">string</p> | Identificador del medio de pago por defecto. Debe ser perteneciente a la lista de tarjetas del cliente. |

### Respuesta

Retorna el objeto del cliente si la actualización es exitosa. Retorna <a href="#errores">un error</a> si algun parámetro de actualización es inválido (ej: identificador de medio de pago inválido).




## Eliminar cliente

> Ejemplo de llamada

````shell
curl -x DELETE "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}', { method: 'DELETE'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.delete 'https://api.qvo.cl/customers/{customer_id}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/customers/{customer_id}', params={
  # TODO
})

print r.json()
````

`DELETE /customers/{customer_id}`

Elimina permanentemente un cliente. Esta acción es irreversible. Si existieran suscripciones activas asociadas al cliente, estas se cancelarán inmediatamente.

### Parámetros
|||
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |


### Respuesta

Retorna una respuesta sin contenido si el cliente fue eliminado con éxito. Si el identificador del cliente no es válido, retornará <a href="#errores">un error</a>.



## Obtener una lista de clientes

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/customers" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers', params={
  # TODO
})

print r.json()
````

> Ejemplo de respuesta

```json
[
  {
    "id": "cus_qos_6r3-4I4zIiou2BVMHg",
    "default_payment_method": {
      "id": "opc_m_c3zyh5BEl8EITxvLbMzw",
      "last_4_digits": "4242",
      "card_type": "VISA",
      "payment_type": "CD"
    },
    "name": "John Snow",
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
          "price": "3000.0",
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
        "id": "opc_m_c3zyh5BEl8EITxvLbMzw",
        "last_4_digits": "4242",
        "card_type": "VISA",
        "payment_type": "CD"
      }
    ],
    "transactions": [
      {
        "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
        "amount": "3000.0",
        "gateway": "olpays",
        "fee": "371.07",
        "credits": "0.0",
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
Este endpoint puede ser utilizado con <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a un cliente con su respectiva información.


