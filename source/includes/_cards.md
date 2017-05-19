# Tarjetas

## getCustomerCards

> Ejemplo de llamada

````shell
# You can also use wget
curl -X get https://api.qvo.cl/customers/{customer_id}/cards
````

````http
GET https://api.qvo.cl/customers/{customer_id}/cards HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
````

````html
<script>
  $.ajax({
    url: 'https://api.qvo.cl/customers/{customer_id}/cards',
    method: 'get',
    success: function(data) {
      console.log(JSON.stringify(data));
    }
  })
</script>
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/{customer_id}/cards', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/{customer_id}/cards', params={
  # TODO
})

print r.json()
````

````java
URL obj = new URL("https://api.qvo.cl/customers/{customer_id}/cards");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());
````

`GET /customers/{customer_id}/cards`

*Gets a customers cards*

Returns a list containing all active cards from a customer.

### Parameters

Parameter|In|Type|Required|Description
---|---|---|---|---|
customer_id|path|string|true|The customer's id



### Responses

Status|Meaning|Description
---|---|---|
200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Card list
404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Customer not found
default|Default|unexpected error

> Example responses

````json
[
  {
    "id": "woc_1Flhy3a-5UnARb7S4Ho3FQ",
    "last_4_digits": "0789",
    "card_type": "Visa",
    "payment_type": "CD",
    "created_at": "2017-05-19T01:37:44Z"
  }
]
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````


## getCustomerCardById

> Ejemplo de llamada

````shell
# You can also use wget
curl -X get https://api.qvo.cl/customers/{customer_id}/cards/{card_id}
````

````http
GET https://api.qvo.cl/customers/{customer_id}/cards/{card_id} HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
````

````html
<script>
  $.ajax({
    url: 'https://api.qvo.cl/customers/{customer_id}/cards/{card_id}',
    method: 'get',
    success: function(data) {
      console.log(JSON.stringify(data));
    }
  })
</script>
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', { method: 'GET'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.get 'https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', params={
  # TODO
})

print r.json()
````

````java
URL obj = new URL("https://api.qvo.cl/customers/{customer_id}/cards/{card_id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());
````

`GET /customers/{customer_id}/cards/{card_id}`

*Gets a card*

Returns a customer's single card for its card_id.

### Parameters

Parameter|In|Type|Required|Description
---|---|---|---|---|
customer_id|path|string|true|The customer's id
card_id|path|string|true|The card's id



### Responses

Status|Meaning|Description
---|---|---|
200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A Card
404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Customer or card not found.
default|Default|unexpected error

> Example responses

````json
{
  "id": "woc_1Flhy3a-5UnARb7S4Ho3FQ",
  "last_4_digits": "0789",
  "card_type": "Visa",
  "payment_type": "CD",
  "created_at": "2017-05-19T01:37:44Z"
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````


## deleteCustomerCard

> Ejemplo de llamada

````shell
# You can also use wget
curl -X delete https://api.qvo.cl/customers/{customer_id}/cards/{card_id}
````

````http
DELETE https://api.qvo.cl/customers/{customer_id}/cards/{card_id} HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
````

````html
<script>
  $.ajax({
    url: 'https://api.qvo.cl/customers/{customer_id}/cards/{card_id}',
    method: 'delete',
    success: function(data) {
      console.log(JSON.stringify(data));
    }
  })
</script>
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', { method: 'DELETE'})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});
````

````ruby
require 'rest-client'
require 'json'

result = RestClient.delete 'https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.delete('https://api.qvo.cl/customers/{customer_id}/cards/{card_id}', params={
  # TODO
})

print r.json()
````

````java
URL obj = new URL("https://api.qvo.cl/customers/{customer_id}/cards/{card_id}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("DELETE");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());
````

`DELETE /customers/{customer_id}/cards/{card_id}`

*Deletes a customer's card*

Deletes a single customer card

### Parameters

Parameter|In|Type|Required|Description
---|---|---|---|---|
customer_id|path|string|true|The customer's id
card_id|path|string|true|The card's id



### Responses

Status|Meaning|Description
---|---|---|
204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|Card deleted
404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Card not found
default|Default|unexpected error

> Example responses

````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````


## createCustomerCardInscription

> Ejemplo de llamada

````shell
# You can also use wget
curl -X post https://api.qvo.cl/customers/{customer_id}/cards/inscriptions
````

````http
POST https://api.qvo.cl/customers/{customer_id}/cards/inscriptions HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
````

````html
<script>
  $.ajax({
    url: 'https://api.qvo.cl/customers/{customer_id}/cards/inscriptions',
    method: 'post',
    success: function(data) {
      console.log(JSON.stringify(data));
    }
  })
</script>
````

````javascript
const request = require('node-fetch');
fetch('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions', { method: 'POST'})
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
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.post('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions', params={
  # TODO
})

print r.json()
````

````java
URL obj = new URL("https://api.qvo.cl/customers/{customer_id}/cards/inscriptions");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("POST");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());
````

`POST /customers/{customer_id}/cards/inscriptions`

*create a card inscription*

create a card inscription for a customer

### Parameters

Parameter|In|Type|Required|Description
---|---|---|---|---|
customer_id|path|string|true|The customer's id
body|body|object|true|inscription parameters



> Body parameter

````json
{}
````
### Responses

Status|Meaning|Description
---|---|---|
201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Inscription succesfully created.
422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Parameter error
default|Default|unexpected error

> Example responses

````json
{}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````


## getCustomerCardInscription

> Ejemplo de llamada

````shell
# You can also use wget
curl -X get https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}
````

````http
GET https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid} HTTP/1.1
Host: localhost:3000
Content-Type: application/json
Accept: application/json
````

````html
<script>
  $.ajax({
    url: 'https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}',
    method: 'get',
    success: function(data) {
      console.log(JSON.stringify(data));
    }
  })
</script>
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

result = RestClient.get 'https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}', params:
  {
    # TODO
  }

p JSON.parse(result)
````

````python
import requests

r = requests.get('https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}', params={
  # TODO
})

print r.json()
````

````java
URL obj = new URL("https://api.qvo.cl/customers/{customer_id}/cards/inscriptions/{inscription_uid}");
HttpURLConnection con = (HttpURLConnection) obj.openConnection();
con.setRequestMethod("GET");
int responseCode = con.getResponseCode();
BufferedReader in = new BufferedReader(
    new InputStreamReader(con.getInputStream()));
String inputLine;
StringBuffer response = new StringBuffer();
while ((inputLine = in.readLine()) != null) {
    response.append(inputLine);
}
in.close();
System.out.println(response.toString());
````

`GET /customers/{customer_id}/cards/inscriptions/{inscription_uid}`

*Gets card inscription*

Gets a customer card inscription on its inscription_uid

### Parameters

Parameter|In|Type|Required|Description
---|---|---|---|---|
customer_id|path|string|true|The customer's id
inscription_uid|path|string|true|The inscription's uid



### Responses

Status|Meaning|Description
---|---|---|
200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|A customer's card inscription
404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Inscription not found
default|Default|unexpected error

> Example responses

````json
{}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````
````json
{
  "error": {
    "message": "name already taken",
    "param": "name",
    "type": "invalid_request_error",
    "code": "string"
  }
}
````