# Webpay Oneclick

<img src="images/webpay_oneclick_banner.jpg" class="full-width-image" />

El sistema permite inscribir tarjetas y generar cobros con Webpay Oneclick.



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



## Obtener una tarjeta Webpay Oneclick

> Ejemplo de llamada

```shell
curl -X GET "https://api.qvo.cl/webpay_oneclick/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA" \
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
  "id": "woc_bMz2iAH1mJ8M4cvv0b7IMA",
  "last_4_digits": "6623",
  "card_type": "Visa",
  "payment_type": "CD",
  "created_at": "2017-05-19T17:07:01.924Z"
}
```

`GET /webpay_oneclick/cards/{card_id}`

Obtiene los detalles de una tarjeta Webpay Oneclick existente. Se necesita proporcionar sólo el identificador único de la tarjeta el cual fue retornado al momento de su inscripción.

### Parámetros
|||
|--------- | -----------|
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la tarjeta Webpay Oneclick. |


### Respuesta

Retorna un objeto de tarjeta si se provee de un identificador válido.





## Realizar un cobro a una tarjeta Webpay Oneclick

> Ejemplo de llamada

```shell
curl -X POST "https://api.qvo.cl/webpay_oneclick/cards/woc_bMz2iAH1mJ8M4cvv0b7IMA/authorize" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=3000
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

> Ejemplo de respuesta:

```json
{
  "id": "trx_Vk7WJYL-wYi4bjXmAaLyaw",
  "created_at": "2017-05-17T19:12:57.759Z",
  "amount": "3000.0",
  "currency": "CLP",
  "gateway": "webpay_oneclick",
  "fee": "371.07",
  "credits": "0.0",
  "status": "successful",
  "initial_balance": "0.0",
  "final_balance": "2628.93",
  "customer": {},
  "payment": {
    "amount": "3000.0",
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
  "gateway_response": {
    "status": "success",
    "message": "successful transaction"
  }
}
```

`POST /webpay_oneclick/cards/{card_id}/authorize`

<!-- Revisar -->
Este endpoint permite autorizar un cobro para una tarjeta Webpay Oneclick en específico.


### Parámetros
|||
|--------- | -----------|
| card_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único de la inscripción. |
| amount<p class="attr-desc warning">Requerido</p><p class="attr-desc">integer</p> | Monto a cobrar. |


### Respuesta

Retorna <a href="#el-objeto-transacci-n">una transacción</a> de ser existosa la transacción. De lo contrario, retornará <a href="#errores">un error</a>.






## Obtener una lista de tarjetas Webpay Oneclick

> Ejemplo de llamada

```shell
curl -X GET "https://api.qvo.cl/webpay_oneclick/cards" \
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
[
  {
    "id": "woc_bMz2iAH1mJ8M4cvv0b7IMA",
    "last_4_digits": "6623",
    "card_type": "Visa",
    "payment_type": "CD",
    "created_at": "2017-05-19T17:07:01.924Z"
  }
]
```


`GET /webpay_oneclick/cards`

Retorna una lista de tarjetas Webpay Oneclick asociadas al comercio. Las tarjetas se encuentran ordenadas por defecto por la fecha de creación, donde los mas recientes aparecerán primero.

### Respuesta

Un arreglo donde cada entrada representa a una tarjeta con su respectiva información.