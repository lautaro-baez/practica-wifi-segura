# Informe de Auditoría de Red Wi-Fi Insegura

## 1. Introducción
El presente informe documenta el análisis de seguridad realizado sobre el tráfico web transmitido a través de redes inalámbricas públicas y no cifradas. Como parte de las auditorías de ciberseguridad para redes de acceso abierto (como cafeterías, aeropuertos y plazas), este documento expone los vectores de riesgo asociados a la interceptación de datos en texto plano y demuestra el rol crítico de las soluciones de cifrado moderno para mitigar la exposición de información sensible.

---

## 2. Sitio Analizado
Para fines de esta demostración práctica de auditoría, se analizó el comportamiento de red al interactuar con un sitio web que no implementa seguridad en la capa de transporte:

* **Sitio analizado / Host visitado:** `http://neverssl.com`
* **URL Solicitada:** `http://neverssl.com/`
* **Método HTTP:** `GET`
* **Protocolo utilizado:** `HTTP/1.1` (Inseguro)

---

## 3. Evidencia Observada (Análisis de Tráfico)
A través de las herramientas de inspección del navegador (F12 > Network), se identificaron los siguientes elementos visibles durante la transacción inicial:

* **Cabeceras de Solicitud (Headers):** Se observa el campo `Host: neverssl.com`, junto con el `User-Agent` que detalla explícitamente el sistema operativo, versión del navegador y arquitectura del cliente.
* **Falta de Cifrado (Texto Plano):** Al no existir un certificado SSL/TLS válido en el servidor de origen, la petición viaja de manera totalmente legible por el medio inalámbrico ("el aire").
* **Exposición de Sesión:** Cualquier cookie de sesión o parámetro enviado en la URL (`Query Parameters`) quedaría expuesto a la captura pasiva.

---

## 4. Riesgos Encontrados
Navegar mediante el protocolo HTTP desde una red Wi-Fi pública expone al usuario a las siguientes vulnerabilidades críticas:

1. **Intercepción de Datos (Sniffing):** Un atacante conectado a la misma red inalámbrica puede utilizar analizadores de paquetes (como Wireshark) para escuchar y registrar todo el tráfico que fluye desde el dispositivo hacia el router, leyendo credenciales, tokens o datos personales si estos viajan sin cifrar.
2. **Ataques de Intermediario (Man-in-the-Middle - MitM):** Al no haber validación criptográfica de la identidad del servidor, un agente malicioso puede posicionarse entre el cliente y el punto de acceso, alterando el contenido de las páginas o capturando credenciales al vuelo.
3. **Redes Gemelas Malignas (Evil Twins):** El uso de puntos de acceso falsos con nombres idénticos a redes legítimas atrae conexiones desprevenidas, centralizando todo el tráfico directamente bajo el control del atacante.

---

## 5. Cómo Ayuda una VPN (Protección Técnica)
La implementación de una Red Privada Virtual (VPN) neutraliza los riesgos de las redes abiertas aplicando los siguientes principios arquitectónicos:

* **Cifrado Extremo a Extremo:** Toda la información generada por el dispositivo se somete a un riguroso proceso de **cifrado** antes de abandonar la tarjeta de red inalámbrica, convirtiendo los datos legibles en ruido criptográfico indescifrable para terceros en la misma red Wi-Fi.
* **Túnel Seguro y Encapsulamiento:** La VPN establece un **túnel seguro** virtual mediante **encapsulamiento** de paquetes. De este modo, los datos viajan encapsulados hacia un servidor VPN de confianza, ocultando el destino real y el contenido de las peticiones a cualquier observador externo en la red local.
* **Privacidad y Protección del Tráfico:** Incluso si el sitio web visitado utiliza el protocolo heredado HTTP, el **túnel seguro** de la VPN protege el tráfico desde el dispositivo hasta la salida del servidor VPN, resguardando la **privacidad** operativa del usuario frente a escuchas pasivas o activas en la red pública.

---

## 6. Las 3 Reglas de Oro para Navegar en Redes Wi-Fi Públicas

1. **Utiliza siempre una VPN de confianza:** Antes de enviar cualquier tipo de información o autenticarte en una red pública, activa un servicio de VPN para garantizar que todo tu tráfico viaje cifrado y encapsulado.
2. **Verifica el uso de HTTPS:** Asegúrate de que las páginas web que visites utilicen el protocolo seguro (`https://` y el icono del candado). Evita ingresar contraseñas o datos personales en sitios que sigan operando exclusivamente bajo HTTP.
3. **Desactiva la conexión automática:** Configura tus dispositivos para que no se conecten automáticamente a redes Wi-Fi abiertas o desconocidas, evitando así caer en trampas de redes gemelas malignas (*Evil Twins*).