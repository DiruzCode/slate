# Errores

El API de qvo usa los siguientes códigos de error:

Error Code | Meaning
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
