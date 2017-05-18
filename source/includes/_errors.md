# Errores

> Ejemplo

```json
{
  "error": {
    "type": "invalid_request_error",
    "message": "amount parameter required",
    "param": "amount"
  }
}
```


QVO usa respuestas HTTP convencionales para indicar el éxito o fracaso de un request. En general, códigos en el rango de los 2xx indican éxito, códigos en el rango 4xx indican un errir qye falló debido a la información proporcionada (ej: un parámetro requerido fue omitido, un pago falló, etc.), y códigos en el rango de los 5xx indican un error con los servidores de QVO (estos son raros).

### Atributos
|||
|--------- | -----------|
| type<p class="attr-desc">string</p> | El tipo de error. Puede ser: `api_error`, `authentication error`, `invalid_request_error` o `rate_limit_error`. |
| message<p class="attr-desc">string</p><p class="attr-desc">opcional</p> | Un mensaje legible que provee mas detalles acerca del error. |
| param<p class="attr-desc">string</p><p class="attr-desc">opcional</p> | El parámetro al cual se relaciona el error. |


El API de qvo usa los siguientes códigos de error:

Código | Significado
---------- | -------
400 | Bad Request -- Hay un problema con tu request 🙈
401 | Unauthorized -- Tu api key es incorrecta 🔐
403 | Forbidden -- No tienes permiso para ver esta página 🚫
404 | Not Found -- El recurso especificado no fue encontrado 😔
405 | Method Not Allowed -- Trataste de ingresar a un recurso con un método inválido
406 | Not Acceptable -- Solicistaste un formato que no es json 😣
410 | Gone -- El recurso solicitado fue removido de nuestros servidores 🏃🏻
418 | Soy una tetera 😗☕️
429 | Too Many Requests -- Estas solicitando muchos recursos! Detente! 😱
500 | Internal Server Error -- Tuvimos un problema con nuestro servidor. 😰 Inténtalo nuevamente mas tarde (estos son raros)
503 | Service Unavailable -- Estamos offline por mantenimiento. Inténtalo nuevamente mas tarde 🛠
