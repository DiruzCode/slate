# Autenticación

```ruby
require 'base64'
api_token = 'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA'
# => "YTcxODNlMWI3ZTlhYjA5YjhhNWNmYTg3ZDE5MzRjM2M6"

headers = {
  "Authorization" => "Bearer " + api_token
}
```

```python
import qvo

api = qvo.authorize('api_key')
```

```shell
$ curl "https://api.qvo.cl/v1/payments"
      -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA"
      
...

> GET /v1/payments/ HTTP/1.1
> Host: api.qvo.cl
> Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA

...
```

```javascript
const qvo = require('qvo');

let api = qvo.authorize('api_key');
```

QVO utiliza [Token Based Authentication](https://www.w3.org/2001/sw/Europe/events/foaf-galway/papers/fp/token_based_authentication/) sobre HTTPS para la autenticación. Puedes solicitar un API token en nuestro [portal de desarrolladores](http://qvo.cl/developers). Los request no autenticados retornarán una respuesta HTTP 401. Las llamadas sobre HTTP simple también fallarán.

### Header de autenticación

> El header `Authorization` debe verse así:

```
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiVGVzdCBjb21tZXJjZSIsImFwaV90b2tlbiI6dHJ1ZX0.AXt3ep_r23w9rSPTv-AnK42s2m-1O0okMYrYYDlRyXA
```

Este tiene el siguiente formato:

`Authorization: Bearer <api_token>`

Donde `api_token` es el asociado a la cuenta del comercio.

<aside class="warning">
<b>Importante</b>: Usa HTTPS para todos los requests. Requests hechos mediante HTTP retornarán respuestas HTTP 403. <b>¡Mantén tu API token en secreto!</b>
</aside>