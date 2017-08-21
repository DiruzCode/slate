# Pruebas y Sandbox

Para facilitar el proceso de integración y permitirte hacer pruebas de las diferentes acciones que puedes realizar con QVO, puedes utilizar un ambiente de prueba. El entorno de pruebas funciona exactamente igual que el de producción, sin embargo <strong>ninguna de las operaciones que realices generarán un cargo real</strong>. Esto significa que las transacciones realizadas en este entorno de prueba son únicamente válidas en este ambiente.

El ambiente de sandbox se encuentra ubicado en la URL

`https://playground.qvo.cl`.

Luego, para realizar pruebas, basta con apuntar las llamadas a esa URL.

### Tarjetas de prueba
|||
| ----------: | ------- |
| **VISA**<p class="attr-desc warning">Débito</p><p class="attr-desc">Aprobar o rechazar</p> | Num: 4051885600446623<br>Emisor: Test Commerce Bank |
| **MasterCard**<p class="attr-desc warning">Débito</p><p class="attr-desc">Siempre rechaza</p> | Num: 5186059559590568<br>Emisor: Test Commerce Bank |
| **VISA**<p class="attr-desc warning">Crédito</p><p class="attr-desc">Aprobar o rechazar</p> | Num: 4051885600446623<br>Fechas: Cualquiera<br>CVV: 123 |
| **MasterCard**<p class="attr-desc warning">Crédito</p><p class="attr-desc">Siempre rechaza</p> | Num: 5186059559590568<br>Fechas: Cualquiera<br>CVV: 123 |

### Credenciales de prueba

|||
| ----------: | ------- |
| **Interfaz Webpay**<p class="attr-desc">Banco prueba</p> | RUT : 11.111.111-1<br>Password  : 123<br>|
