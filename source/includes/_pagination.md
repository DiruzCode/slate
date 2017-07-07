# Paginación, filtros y orden

> Ejemplo

```shell
  curl -x GET https://api.qvo.cl/transactions \
    -d 'page=1' \
    -d 'per_page=10' \
    -d 'where={"amount": {">": 2000}}' \
    -d 'order_by=amount DESC'
```

Todos los recursos que retornen un arreglo o lista, soportan paginación, filtros y orden. Por defecto, los valores se ordenan por fecha de creación, donde los mas recientes se ubicarán primero en el arreglo.

#### Paginación

La paginación se realiza definiendo el número de entradas por página a través del parámetro `per_page` y se navega mediante el parámetro `page`.

Por ejemplo, para obtener la segunda página desplegando 20 entradas por página:

`GET /recurso&page=1&per_page=20`

Para facilitar la navegación y el despliegue de número de páginas, en la respuesta existen los headers `X-Page`, `X-Per-Page` y `X-Total` que representan la **página actual**, la **cantidad de entradas por página** y el **total de entradas** respectivamente.

Por ejemplo si `X-Page = 2`, `X-Per-Page = 10` y `X-Total = 45`, es fácil definir que se trata de 5 páginas en total.

#### Filtros

> Ejemplo de filtro

```json
{
  "amount" : {
    ">=" : 1000,
    "<" : 8000
  },
  "status": {
    "=": "successful"
  }
}
```

El parámetro `where` acepta un hash para filtrar el contenido con respecto a una serie de reglas. La sintáxis corresponde a un objeto JSON de la siguiente manera:

`{ ":atributo": { ":operador": ":variable" } }`

- `:atributo` pertenece a los atributos del recurso u objeto.
- `:operador` puede ser:
  - `<` menor a
  - `<=` menor o igual a
  - `>` mayor a
  - `>=` mayor o igual a
  - `=` igual a
  - `like` parecido a
  - `!` distinto a
- `:variable` representa al valor con el cual se aplicará el operador

Cabe mencionar que múltiples atributos y múltiples operadores por atributo son soportados.

#### Orden

Se puede definir el orden de despligue de las entradas mediante el parámetro `order_by`. Este acepta un string con la siguiente estructura:

`:atributo :orden`

- `:atributo` pertenece a los atributos del recurso u objeto.
- `:order` puede ser:
    - `DESC` orden descendente
    - `ASC` orden ascentente

El órden sólo se puede aplicar sobre un argumento y no soporta órdenes anidados.

Por ejemplo, si deseamos ordenar un recurso por fecha de creación de manera ascendente, realizamos:

`created_at ASC`


### Atributos
|||
|---------: | -----------|
| page<p class="attr-desc">integer</p> | Número de página |
| per_page<p class="attr-desc">integer</p> | Número de entradas por página. |
| where<p class="attr-desc">hash</p> | Corresponde a un conjunto de reglas (ej: `{"amount": {">": 2000}}`) |
| order_by<p class="attr-desc">string</p> | Orden de despliegue de las entradas |