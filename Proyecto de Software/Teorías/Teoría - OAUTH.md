# Conceptos

- **Identificación**: Secuencia de caracteres que identifica **unívocamente** al usuario
- **Autenticación**: La **verificación** que realiza el sistema sobre la identificación. Realizable a través de:
	- Algo que se conoce: clave de acceso.
	- Algo que se posee: tarjeta o token.
	- Algo que se es: huella digital, retina, voz
- **Autorización**: Los **permisos asociados** a un usuario autenticado.

### JWT
*Jason Web Token*

- Define una forma segura de transmitir información entre partes como un objeto JSON.
- El contenido se firma para garantizar confiabilidad.
- Estructura del token:
	- header.payload.signature
		- Header: Objeto JSON, incluye el algoritmo de la firma (obligatorio), cuando no se requiere encriptación entonces alg = "None" y el tipo del token.
		- Payload: Objeto JSON, incluye los datos que se quieren transmitir.
		- Signature: Permite establecer la autenticidad del token, es decir, que los datos no han sido alterados.
### OAUTH

- Protocolo de autorización o de delegación de acceso.
	- Permite definir como un tercero accede a nuestros recursos.
- Funcionamiento:
	- Usuario quiere acceder a mi aplicación.
	- Mi aplicación requiere a un servicio de un token de un uso.
	- El servicio responde al requerimiento con un token de un solo uso y su clave secreta.
	- Mi aplicación arma el link de autorización y redirige al usuario a ese link.
	- El usuario completa la autenticación con el servicio en el link armado por mi aplicación.
	- El servicio envia a mi aplicación un access token para ese usuario junto con su clave secreta.
	- Mi aplicación almacena el access token para ese usuario.

### OpenID Connect

- Capa de identificación construida sobre **OAUTH 2.0**.
- Permite al cliente verificar la identidad del usuario final basndose en la autenticación realizada por el servidor de autorización (OAUTH).