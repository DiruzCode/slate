# Transbank Webpay Plus

## Overview

**TODO**

## Crear Transacción

> Definición

```shell
POST https://api.qvo.cl/api/webpay_plus/create_transacton
```

> Ejemplo de Llamada:

```shell
curl --request POST "https://api.qvo.cl/api/webpay_plus/create_transacton" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=2000 \
  -d return_url="http://www.example.com/return"
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('token');

let amount = 2500
let return_url = 'http://www.example.com/return'

let transaction = api.webpayPlus.createTransaction(amount, return_url);

```
> Ejemplo de Respuesta:

```json
{
  "transaction_uid": "NTCBq4nCn2GQoqi1_RERVw",
  "redirect_url": "http://api.qvo.cl/api/webpay_plus/init_transaction/NTCBq4nCn2GQoqi1_RERVw",
  "expiration_date": "2017-01-16T13:09:05.828Z"
}
```

<!-- Revisar -->
Este endpoint permite crear una transacción utilizando Transbank Webpay Plus. Esta retorna una url a la cual se debe redirigir al usuario para iniciar la transacción. Estas podrán ser "canjeadas" sólo una vez por el usuario.

<aside class="notice">
Las transacciones poseen una fecha de expiración por defecto de 10 minutos después de creadas.
</aside>

Luego de realizada la transacción por el usuario, se le redireccionará realizando un `GET` a una `return_url` del comercio:

`GET http://www.example.com/return?uid=NTCBq4nCn2GQoqi1_RERVw`

Esta tendrá el query parameter `uid`, el cual debe ser utilizado para verificar el resultado de la transacción por el comercio.


### Argumentos
|||
|--------- | -----------|
| amount<p class="attr-desc warning">Requerido</p><p class="attr-desc">integer</p> | El monto de la transacción|
| return_url<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Url (válida) de retorno de la transacción |

## Obtener resultado de transacción

```ruby
require 'qvo'

api = QVO::APIClient.authorize!('meowmeowmeow')
api.transactions.get('NTCBq4nCn2GQoqi1_RERVw')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://api.qvo.cl/api/webpay_plus/transaction/NTCBq4nCn2GQoqi1_RERVw"
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('token');

let transaction_uid = 'NTCBq4nCn2GQoqi1_RERVw';

let transaction_result = api.transactions.get(transaction_uid);
```

> Ejemplo de Respuesta:

```json
{
  "uid": "NTCBq4nCn2GQoqi1_RERVw",
  "status": "paid",
  "created_at": "2017-01-12T14:23:16.193Z",
  "updated_at": "2017-01-12T14:24:23.233Z",
  "payment": {
    "id": 4,
    "amount": 1000,
    "commerce_id": 1,
    "details": "pago de ejemplo",
    "status": "ok",
    "gateway_type": "webpay_plus",
    "created_at": "2017-01-12T14:24:23.221Z",
    "updated_at": "2017-01-12T14:24:23.221Z",
    "payment_type": "credit"
  }
}
```

Este endpoint obtiene el resultado de una transacción específica.

### HTTP Request

`GET http://api.qvo.cl/api/webpay_plus/transaction/{transaction_uid}`

### URL Parameters

Parameter | Description
--------- | -----------
transaction_uid | El uid de la transacción a obtener

## Anulación de transacción

```shell
curl "https://api.qvo.cl/api/webpay_plus/nullify/NTCBq4nCn2GQoqi1_RERVw"
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('token');

let transaction_uid = 'NTCBq4nCn2GQoqi1_RERVw'; // uid de transacción
let nullify_amount = 1500 // monto a anular

let transaction_result = api.transactions.nullify(transaction_uid, nullify_amount);
```

> Este comando retorna la siguiente estructura JSON:

```json
{
  "uid": "NTCBq4nCn2GQoqi1_RERVw",
  "status": "nullified",
  "balance": 0,
  "created_at": "2017-01-12T14:23:16.193Z",
  "updated_at": "2017-01-16T14:52:16.193Z",
  "payment": {
    "id": 4,
    "amount": 1000,
    "commerce_id": 1,
    "details": null,
    "status": "ok",
    "gateway_type": "webpay_plus",
    "created_at": "2017-01-12T14:24:23.221Z",
    "updated_at": "2017-01-12T14:24:23.221Z",
    "payment_type": "credit"
  },
  "nullifications": [
    {
      "id": 4,
      "amount": 1000,
      "webpay_plus_transaction_id": 8,
      "authorization_code": "919062",
      "created_at": "2017-01-16T14:52:16.170Z",
      "updated_at": "2017-01-16T14:52:16.170Z"
    }
  ]
}
```

Este endpoint permite anular un monto determinado de una transacción específica.

<aside class="notice">
Es posible anular un monto inferior al total. Se podrán realizar anulaciones hasta completar el monto total de la transacción.
</aside>

### HTTP Request

`POST https://api.qvo.cl/api/webpay_plus/nullify/{transaction_uid}`

### URL Parameters

Parameter | Description
--------- | -----------
transaction_uid | El uid de la transacción a obtener

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ----------- | ----------- | -----------
nullify_amount | Yes | integer | El monto a anular de la transacción
