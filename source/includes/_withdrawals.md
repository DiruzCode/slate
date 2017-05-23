# Retiros

Los retiros permiten extraer dinero del sistema a la cuenta especificada por el comercio en la configuración. Al momento de ser aprovados, existe un plazo de 48 horas hábiles en que se realizará la transferencia la cuenta del comercio.

## El objeto retiro

> Ejemplo de respuesta

```json
{
  "id": "wdl_nPe9BeVau5rQyV2h7Env0A",
  "amount": "100.0",
  "status": "waiting_for_approval",
  "created_at": "2017-05-21T22:55:14.975Z",
  "updated_at": "2017-05-21T22:55:14.975Z"
}
```

### Atributos
|||
|--------- | -----------|
| id<p class="attr-desc">string</p> | Identificador único del objeto |
| amount<p class="attr-desc">number</p> | Monto de retiro. |
| status<p class="attr-desc">string</p> | Estado del retiro. Puede ser: `waiting_for_approval`, `approved` y `transfered` |
| created_at<p class="attr-desc">datetime</p> | Fecha de creación del objeto |
| updated_at<p class="attr-desc">datetime</p> | Fecha de la última actualización del objeto |


## Crear un retiro

> Ejemplo de llamada

````shell
curl --request POST "https://api.qvo.cl/withdrawals" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA" \
  -d amount=100
````


````javascript
const request = require('node-fetch');
fetch('http://api.qvo.cl/withdrawals', { method: 'POST'}, {
  amount: 100
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

result = RestClient.post 'http://api.qvo.cl/withdrawals', params:
  {
    amount: 100
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('http://api.qvo.cl/withdrawals', params={
  amount: 100
})

print r.json()
````

> Ejemplo de respuesta

```json
{
  "id": "wdl_nPe9BeVau5rQyV2h7Env0A",
  "amount": "100.0",
  "status": "waiting_for_approval",
  "created_at": "2017-05-21T22:55:14.975Z",
  "updated_at": "2017-05-21T22:55:14.975Z"
}
```

`POST /withdrawals`

Crea un nuevo objeto retiro.

### Parámetros
|||
|--------- | -----------|
| id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del plan. **Único**. |
| name<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Nombre de despliegue del plan. |
| price<p class="attr-desc warning">Requerido</p><p class="attr-desc">number</p> | Precio del plan. |
| currency<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Código de [3 dígitos ISO de moneda](https://www.iso.org/iso-4217-currency-codes.html). Puede ser: `CLP` o `USD`. |
| trial_period_days<p class="attr-desc">integer</p> | Especifica el número de días de prueba del plan. Si incluyes un periodo de prueba, al cliente no se le cobrará hasta que termine este periodo. |


### Respuesta

Retorna un objeto de retiro si la llamada es exitosa.



## Obtener un retiro

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/withdrawals/wdl_nPe9BeVau5rQyV2h7Env0A" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const request = require('node-fetch');
fetch('http://api.qvo.cl/withdrawals/wdl_nPe9BeVau5rQyV2h7Env0A', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'http://api.qvo.cl/withdrawals/wdl_nPe9BeVau5rQyV2h7Env0A'
p JSON.parse(result)
````

````python
import requests

r = requests.get('http://api.qvo.cl/withdrawals/wdl_nPe9BeVau5rQyV2h7Env0A')

print r.json()
````

> Ejemplo de respuesta

```json
{
  "id": "wdl_nPe9BeVau5rQyV2h7Env0A",
  "amount": "100.0",
  "status": "waiting_for_approval",
  "created_at": "2017-05-21T22:55:14.975Z",
  "updated_at": "2017-05-21T22:55:14.975Z"
}
```

`GET /withdrawals/{withdrawal_id}`

Obtiene los detalles de un retiro existente. Se necesita proporcionar sólo el identificador único del retiro el cual fue retornado al momento de su creación.

### Parámetros
|||
|--------- | -----------|
| withdrawal_id<p class="attr-desc warning">Requerido</p><p class="attr-desc">string</p> | Identificador único del retiro. |


### Respuesta

Retorna un objeto de retiro si se provee de un identificador válido.




## Obtener una lista de retiros

> Ejemplo de llamada

````shell
curl --request GET "https://api.qvo.cl/withdrawals" \
  -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/withdrawals', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/withdrawals'
p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/withdrawals')

print r.json()
````

> Ejemplo de respuesta

```json
[
  {
    "id": "wdl_nPe9BeVau5rQyV2h7Env0A",
    "amount": "100.0",
    "status": "waiting_for_approval",
    "created_at": "2017-05-21T22:55:14.975Z",
    "updated_at": "2017-05-21T22:55:14.975Z"
  }
]
```

`GET /withdrawals`

Retorna una lista de retiros. Los retiros se encuentran ordenados por defecto por la fecha de creación, donde los mas recientes aparecerán primero.

<aside class="notice">
Este endpoint puede ser utilizado con <a href="#paginaci-n-filtros-y-orden">paginación, filtros y orden</a>
</aside>

### Respuesta

Un arreglo donde cada entrada representa a un retiro con su respectiva información.


