# Webpay Oneclick

TODO

## Crear una inscripción de tarjeta webpay oneclick

> Ejemplo de llamada

```shell
curl --request POST "https://api.qvo.cl/api/webpay_plus/transactions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d email="theimp@kingslanding.gov" \
  -d return_url="http://www.example.com/return"
```

```javascript
// TODO

```

```ruby
# TODO

```

```python
// TODO

```


> Ejemplo de respuesta

```json
{
  "inscription_uid": "-paHbu0Bnaj5aLWwqYx4TQ",
  "redirect_url": "https://api.qvo.cl/webpay_oneclick/init_inscription/-paHbu0Bnaj5aLWwqYx4TQ",
  "expiration_date": "2017-01-16T15:21:39.363Z"
}
```

`POST /webpay_oneclick/inscriptions`

Crea una inscripción de tarjeta Webpay Oneclick.

Esta inscripción permite inscribir una tarjeta mediante la interfaz de Webpay Oneclick. Para esto, es necesario **redirigir** al cliente a `redirect_url`, para iniciar el proceso de inscripción. 

Una vez terminado el proceso de inscripción, se retornará el cliente al `return_url` proporcionado. Esta llamada, irá acompañada de un **query param** llamado `uid` (ubicado en la ruta) que representa el identificador único de la inscripción de tarjeta. 

Por ejemplo, para `return_url = "http://www.example.com/return` se retornará el cliente a:

`GET http://www.example.com/return?uid=woi_WZa9DgYQPzPtUqMsQdoNhQ`

donde `uid` es igual a `woi_WZa9DgYQPzPtUqMsQdoNhQ` 

Luego, para obtener el resultado de la inscripción, se debe realizar una llamada a <a href="obtener-inscripci-n-de-tarjeta-webpay-oneclick">obtener inscripción de tarjeta Webpay Oneclick</a>, utilizando el identificador único de inscripción `uid`.

<aside class="warning">
La inscripción posee una fecha de expiración de <b>10 minutos</b> luego de su fecha de creación. Si se intenta acceder a <code>redirect_url</code> luego de este tiempo, retornará <a href="errores">un error</a>.
</aside>

### Parámetros
|||
|--------- | -----------|
| customer_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del cliente. |
| return_url<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | URL válida de retorno de la inscripción. |


## Obtener inscripción de tarjeta Webpay Oneclick

> Ejemplo de llamada

```shell
curl -X GET "https://api.qvo.cl/webpay_oneclick/inscriptions/WuNMovWou2G9_Sxb686deQ" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
```

```javascript
// TODO
```

```ruby
#TODO
```

```python
// TODO
```

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

`GET /webpay_oneclick/inscriptions/{inscription_uid}`


Obtiene el resultado de una inscripción de tarjeta Webpay Oneclick.

### Parámetros
|||
|--------- | -----------|
| inscription_uid<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la inscripción. |

### Respuesta

Retorna el estado de la inscripción y una <a href="el-objeto-tarjeta">tarjeta</a> de haber creado una.

Esta puede tener `status`:
 - `succeeded`: La inscripción ha sido un éxito y se adjunta `card` con la información de la tarjeta resultante
 - `failed`: La inscripción ha fracasado y se adjunta `error` con la información del error.





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
