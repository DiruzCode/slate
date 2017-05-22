# Webpay Plus

<img src="images/webpay_plus_banner.jpg" class="full-width-image" />

El sistema permite generar transacciones con Webpay Plus.




## Generar un cobro webpay plus

> Ejemplo de llamada

```shell
curl --request POST "https://api.qvo.cl/api/webpay_plus/transactions" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=2000 \
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
  "transaction_id": "trx_fzmNpXvJJZBWwGbH5fW8cw",
  "redirect_url": "http://api.qvo.cl/webpay_plus/init_transaction/wpt_y7CUkd3EqiLB8TV1O7fhGQ",
  "expiration_date": "2017-05-21T23:43:41.332Z"
}
```

`POST /webpay_plus/transactions`

Crea una transacción utilizando Webpay Plus.

Permite realizar un cobro mediante la interfaz de Webpay Plus. Para esto, es necesario **redirigir** al cliente a `redirect_url` para iniciar el proceso. 

Una vez terminado el proceso de transacción, se retornará el cliente al `return_url` proporcionado. Esta llamada, irá acompañada de un **query param** llamado `transaction_id` (ubicado en la ruta) que representa el identificador único de la transacción. 

Por ejemplo, para `return_url = "http://www.example.com/return` se retornará el cliente a:

`GET http://www.example.com/return?transaction_id=trx_fzmNpXvJJZBWwGbH5fW8cw`

donde `transaction_id` es igual a `trx_fzmNpXvJJZBWwGbH5fW8cw` 

Luego, para obtener el resultado de la transacción, se debe realizar una llamada a <a href="#obtener-una-transacci-n">obtener una transacción</a>, utilizando el identificador único de inscripción `transaction_id`.


<aside class="warning">
La transacción posee una fecha de expiración de <b>10 minutos</b> luego de su fecha de creación. Si se intenta acceder a <code>redirect_url</code> luego de este tiempo, retornará <a href="errores">un error</a>.
</aside>


### Parámetros
|||
|--------- | -----------|
| amount<p class="attr-desc warning">Requerido</p><p class="attr-desc">integer</p> | El monto de la transacción|
| return_url<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Url (válida) de retorno de la transacción |


### Respuesta

Retorna los parámetros para iniciar una transacción con Webpay Plus.