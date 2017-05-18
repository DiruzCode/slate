
# Transbank Webpay Oneclick

## Overview

**TODO**

## Crear Inscripción de tarjeta

```shell
curl "https://api.qvo.cl/api/webpay_oneclick/create_inscription" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('token');

let return_url = 'http://www.example.com/return'; // url de retorno
let email = 'test@example.com' // email de usuario

let transaction_result = api.transactions.nullify(transaction_uid, nullify_amount);
```

> Este comando retorna la siguiente estructura JSON:

```json
{
  "inscription_uid": "-paHbu0Bnaj5aLWwqYx4TQ",
  "redirect_url": "https://api.qvo.cl/webpay_oneclick/init_inscription/-paHbu0Bnaj5aLWwqYx4TQ",
  "expiration_date": "2017-01-16T15:21:39.363Z"
}
```

<!-- Revisar -->
Este endpoint permite crear una inscripción de tarjeta para Transbank Oneclick. Esta retorna una url a la cual se debe redirigir al usuario para iniciar la inscripción de su tarjeta. Esta podrá ser "canjeada" sólo una vez por el usuario.

<aside class="notice">
La inscripción posee una fecha de expiración por defecto de 10 minutos después de creada.
</aside>

Luego de realizada la transacción por el usuario, se le redireccionará realizando un `GET` a una `return_url` del comercio:

`GET http://www.example.com/return?uid=NTCBq4nCn2GQoqi1_RERVw`

Esta tendrá el query parameter `uid`, el cual debe ser utilizado para verificar el resultado de la inscripción por el comercio.

### HTTP Request

`POST https://api.qvo.cl/api/webpay_oneclick/create_inscription`

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ----------- | ----------- | -----------
email | Yes | string | El email del usuario
return_url | Yes | string | Url (válida) de retorno de la inscripción


## Obtener resultado de inscripción

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
curl "https://api.qvo.cl/webpay_oneclick/inscription/WuNMovWou2G9_Sxb686deQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('token');

let inscription_uid = 'NTCBq4nCn2GQoqi1_RERVw';

let inscription_result = api.inscriptions.get(inscription_uid);
```

> Este comando retorna la siguiente estructura JSON:

```json
{
  "uid": "WuNMovWou2G9_Sxb686deQ",
  "status": "success",
  "created_at": "2017-01-16T19:52:53.559Z",
  "updated_at": "2017-01-16T19:53:36.264Z",
  "card": {
    "id": 1,
    "commerce_id": 1,
    "credit_card_type": "Visa",
    "last4_card_digits": "6623",
    "token": "qOG4Zsh7BFJKA4GjuBkFTA",
    "created_at": "2017-01-16T19:53:36.252Z",
    "updated_at": "2017-01-16T19:53:36.252Z",
    "active": true
  }
}
```

Este endpoint obtiene el resultado de una inscripción específica.

De resultar exitosa, esta retornará un objeto `card` que representa la tarjeta asociada al comercio.

### HTTP Request

`GET https://api.qvo.cl/webpay_oneclick/inscription/{inscription_uid}`

### URL Parameters

Parameter | Description
--------- | -----------
inscription_uid | El uid de la inscripción a obtener

## Autorizar pago

```shell
curl "https://api.qvo.cl/webpay_oneclick/authorize/qOG4Zsh7BFJKA4GjuBkFTA" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

> Este comando retorna la siguiente estructura JSON:

```json
{
  "transaction_uid": "l8qzt_v5Pcv-0t6tsKUMHQ",
  "status": "paid",
  "created_at": "2017-01-16T19:57:51.202Z",
  "updated_at": "2017-01-16T19:57:54.458Z",
  "payment": {
    "id": 5,
    "amount": 1000,
    "commerce_id": 1,
    "details": null,
    "status": "ok",
    "gateway_type": "webpay_oneclick",
    "created_at": "2017-01-16T19:57:54.453Z",
    "updated_at": "2017-01-16T19:57:54.453Z",
    "payment_type": "credit"
  }
}
```

<!-- Revisar -->
Este endpoint permite realizar autorizar un pago para una tarjeta en específico. Retorna el estado del pago.

### HTTP Request

`POST https://api.qvo.cl/webpay_oneclick/authorize/{card_token}`

### URL Parameters

Parameter | Description
--------- | -----------
card_token | El token de la tarjeta a cargar

### JSON Body Parameters

Parameter | Required | Type | Description
--------- | ----------- | ----------- | -----------
amount | Yes | integer | El monto a cargar a la tarjeta

## Reversar pago

```shell
curl "https://api.qvo.cl/webpay_oneclick/reverse/l8qzt_v5Pcv-0t6tsKUMHQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

> Este comando retorna la siguiente estructura JSON:

```json
{
  "transaction_uid": "l8qzt_v5Pcv-0t6tsKUMHQ",
  "status": "reversed",
  "created_at": "2017-01-16T19:57:51.202Z",
  "reverse_code": "7355785544020521896"
}
```

<!-- Revisar -->
Este endpoint permite realizar autorizar un pago para una tarjeta en específico. Retorna el estado del pago.

### HTTP Request

`POST https://api.qvo.cl/webpay_oneclick/reverse/{transaction_uid}`

### URL Parameters

Parameter | Description
--------- | -----------
transaction_uid | El uid de la transacción a anular

## Obtener tarjetas

```shell
curl "https://api.qvo.cl/webpay_oneclick/cards" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

> Este comando retorna la siguiente estructura JSON:

```json
[
  {
    "id": 1,
    "commerce_id": 1,
    "credit_card_type": "Visa",
    "last4_card_digits": "6623",
    "token": "qOG4Zsh7BFJKA4GjuBkFTA",
    "created_at": "2017-01-16T19:53:36.252Z",
    "updated_at": "2017-01-16T19:53:36.252Z",
    "active": true
  },
  ...
]
```

Este endpoint permite obtener una lista de las tarjetas asociadas al comercio.

### HTTP Request

`GET https://api.qvo.cl/webpay_oneclick/cards`

## Obtener tarjeta específica

```shell
curl "https://api.qvo.cl/webpay_oneclick/cards/qOG4Zsh7BFJKA4GjuBkFTA" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

> Este comando retorna la siguiente estructura JSON:

```json
  {
    "id": 1,
    "commerce_id": 1,
    "credit_card_type": "Visa",
    "last4_card_digits": "6623",
    "token": "qOG4Zsh7BFJKA4GjuBkFTA",
    "created_at": "2017-01-16T19:53:36.252Z",
    "updated_at": "2017-01-16T19:53:36.252Z",
    "active": true
  }
```

Este endpoint permite obtener una tarjeta específica asocioada comercio.

### HTTP Request

`GET https://api.qvo.cl/webpay_oneclick/cards/{card_token}`

### URL Parameters

Parameter | Description
--------- | -----------
card_token | El token asociado del a tarjeta

## Inactivar tarjeta

```shell
curl --request DELETE "https://api.qvo.cl/webpay_oneclick/cards/qOG4Zsh7BFJKA4GjuBkFTA" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

> Este comando retorna la siguiente estructura JSON:

```json
  {
    "msg" : "card succesfully inactivated"
  }
```

Este endpoint permite anular una tarjeta específica asociada al comercio.

### HTTP Request

`DELETE https://api.qvo.cl/webpay_oneclick/cards/{card_token}`

### URL Parameters

Parameter | Description
--------- | -----------
card_token | El token asociado del a tarjeta a eliminar
