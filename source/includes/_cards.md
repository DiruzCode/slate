# Tarjetas

Es posible guardar múltiples tarjetas en un cliente para luego cobrar.

## El objeto tarjeta

> Ejemplo de respuesta

```json
{
  "id": "opc_m_c3zyh5BEl8EITxvLbMzw",
  "last_4_digits": "4242",
  "card_type": "VISA",
  "payment_type": "CD"
}
```

### Atributos
|||
|--------- | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto |
| last_4_digits<p class="attr-desc">string</p> | Los últimos 4 dígitos de la tarjeta. |
| card_type<p class="attr-desc">string</p> | Tipo de tarjeta. Puede ser: `VISA` o `MASTERCARD` |
| payment_type<p class="attr-desc">string</p> | Tipo de pago de la tarjeta. Puede ser: `CD` (crédito) o `DB` (débito) |

## Crear una inscripción de tarjeta

> Ejemplo de llamada

````shell
# You can also use wget
curl -X post https://api.qvo.cl/customers/{customer_id}/cards/inscriptions
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions', { method: 'POST'}, {
  return_url: "http://example.com/return"
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.post 'https://api.qvo.cl/customers/{customer_id}/cards/inscriptions', params:
  {
    return_url: "http://example.com/return"
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions', params={
  return_url: "http://example.com/return"
})

print r.json()
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

Esta inscripción permite al cliente inscribir su tarjeta mediante la interfaz de Webpay Oneclick. Para esto, es necesario **redirigir** al cliente a `redirect_url`, para iniciar el proceso de inscripción. 

Una vez terminado el proceso de inscripción, se retornará el cliente al `return_url` proporcionado. Esta llamada, irá acompañada de un **query param** llamado `uid` (ubicado en la ruta) que representa el identificador único de la inscripción de tarjeta. 

Por ejemplo, para `return_url = "http://www.example.com/return` se retornará el cliente a:

`GET http://www.example.com/return?uid=woi_WZa9DgYQPzPtUqMsQdoNhQ`

donde `uid` es igual a `woi_WZa9DgYQPzPtUqMsQdoNhQ` 

Luego, para obtener el resultado de la inscripción, se debe realizar una llamada a <a href="obtener-inscripci-n-de-tarjeta">obtener inscripción de tarjeta</a>, utilizando el identificador único de inscripción `uid`.

<aside class="warning">
La inscripción posee una fecha de expiración de <b>10 minutos</b> luego de su fecha de creación. Si se intenta acceder a <code>redirect_url</code> luego de este tiempo, retornará <a href="errores">un error</a>.
</aside>

### Parámetros
|||
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| return_url<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | URL válida de retorno de la inscripción. |


### Respuesta

Retorna los parámetros para iniciar una inscripción de tarjeta.


## Obtener una inscripción de tarjeta

> Ejemplo de llamada

````shell
curl -X get https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}'
p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}')

print r.json()
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
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| inscription_uid<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la inscripción. |

### Respuesta

Retorna el estado de la inscripción y una <a href="el-objeto-tarjeta">tarjeta</a> de haber creado una.

Esta puede tener `status`:
 - `succeeded`: La inscripción ha sido un éxito y se adjunta `card` con la información de la tarjeta resultante
 - `failed`: La inscripción ha fracasado y se adjunta `error` con la información del error.



## Obtener una tarjeta

> Ejemplo de llamada

````shell
# You can also use wget
curl -X get https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ
````


````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', params={
  # TODO
})

print r.json()
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
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta. |

### Respuesta

Retorna un objeto de tarjeta si se provee de un identificador válido.


## Eliminar una tarjeta

> Ejemplo de llamada

````shell
curl -X DELETE "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ', { method: 'DELETE'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.delete 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ'
p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards/woc_1Flhy3a-5UnARb7S4Ho3FQ')

print r.json()
````

`DELETE /customers/{customer_id}/cards/{card_id}`

Elimina permanentemente una tarjeta de un cliente. Esta acción es irreversible.

### Parámetros
|||
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta. |


### Respuesta

Retorna una respuesta sin contenido si la tarjeta fue eliminada con éxito. Si el identificador de la tarjeta no es válido, retornará <a href="#errores">un error</a>.



## Obtener una lista de tarjetas

> Ejemplo de llamada

````shell
curl -X get "https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````


````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards'
p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/cus_I-ZNs9TlY2FmdOUByQ5Ieg/cards')

print r.json()
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
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |

### Respuesta

Un arreglo donde cada entrada representa a una tarjeta del cliente con su respectiva información.


