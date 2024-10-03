### Cookies
*Tokens que almacenan información del cliente*

Se encuentran en el cliente y se envian en cada petición.
Permiten simular un *estado* al protocolo *stateless* HTTP

***En Flask***
Para obtenerla: `request.cookies.get("algo")`
Para settear una: `resp.set_cookie("nombre", "valor")`

### Usos de las cookies
- Gestión de sesiones: Sesión, carritos de compras, puntajes de juegos, etc.
- Personalización: Idioma, etc.
- Rastreo: Recolección de información sobre los usuarios.

### Seguridad de las cookies
- Secure: Solo transferible por HTTPS.
- HttpOnly: Inaccesibles desde Javascript.
- Same-Site: Requiere que una cookie sea enviada desde el mismo dominio.

***Manejo de sesiones tradicional***
Se almacena una session ID en la cookie (Puede resultar inseguro).

### Sesiones en Flask
*Client Side*
- Sesion cookie
- La cookie se almacena en el cliente en una cookie firmada con una secret key.
- Es menos segura.

*Server Side*
- Flask Session.
- Se elige donde se almacena la sesión en el servidor.
- El session ID almacenado en la cookie pasa por un *hashing* que hace inaccessible a la información de la misma sin la clave *secret*.
- El seteo, get y demás acciones se realizan automáticamente por Flask.

```python
	from flask_session import Session

	app.config["SESSION_TYPE"] = "filesystem"
	Session(app)
```
