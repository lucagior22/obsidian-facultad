*Simil AWS S3*

*Object storage que brinda funcionalidades para el manejo de archivos.*

# Configuración inicial
- Instalar Minio en la computadora.
	- `poetry add minio`
- Se ofrece una página de configuración en la url `minio.localhost:9001` (*así lo tiene configurado el profesor*) o `localhost:9001` 
- Crear un bucket.

# Access policy
*Los archivos tienen propiedades de acceso **Public** o **Private**.*
- Para el trabajo integrador, la mayoria de los archivos estarán configurados en privado.
- Se puede configurar una sección donde se almacenen archivos públicos y otro donde vayan aquellos que requieran ser privados.
	- Carpeta `/public`

**IMPORTANTE**: La cátedra va a avisar si se requiere alguna configuración de este estilo.

# Integración con nuestra aplicación
```python title=storage.py
from minio import Minio

class Storage:
	def __init__(self, app=None):
		self._client = None
		if app is not None:
			self.init_app(app)
	
	def init_app(self, app):
		# Obtengo los datos de configuración, depende del ambiente donde se esté ejecutando la aplicación (Producción || local).
		minio_server = app.config.get("MINIO_SERVER")
		access_key = app.config.get("MINIO_ACCESS_KEY")
		secret_key = app.conig.get("MINIO_SECRET_KEY")
		secure = app.config.get("MINIO_SECURE", False) # HTTPS? Debe estar en true en entorno de producción.
		
		# Inicializo el cliente de Minio.
		self._client = Minio(minio_server, access_key=access_key, secret_key=secret_key, secure=secure)
		
		# Adjunto el cliente Minio al contexto de la app Flask.
		app.storage = self
		
		return app

	# Getter y setter del cliente
	
	@property
	def client(self):
		return self._client
	
	@client.setter
	def client(self, value):
		self._client = value

# Instancio el storage VACIO 
storage = Storage()
```

### Desde el __init__.py (*El principal de nuestra aplicación*)
```python title=__init__.py
from src.web.storage import storage

def create_app(...):
	app = Flask(...)
	...
	# Inicializo la aplicación con la app de flask
	storage.init_app(app)
```

### Credenciales
- Desarrollo
	- Access key se crea desde la interfaz de Minio.
		- Apartado **Access Keys**.
	- MINIO_SERVER
		- `localhost:9000`
- Producción
	- Todo se maneja con environ.get(<nombre_variable>).
	- Secure es **True** por default.

# Update
```python
def update(id):
	params = request.form.copy()

	if "avatar" in request.files:
		avatar = request.files["avatar"]
		client = current_app.storage.client
		size = fstat(avatar.line.fileno()).st_size

		client.put_object("grupo00", avatar.filename, avatar, size, content_type=avatar.content_type)
		params["avatar"] = avatar.filename

	auth.update_user(id, **params)
	
	return redirect(url_for(users.index))
```
*Ejemplo de una respuesta a una petición post de un formulario de edición de un usuario.*

- La función put_object permite subir archivos a Minio.
	- Parámetros:
		- bucket_name
		- filename
		- file
		- file_length
		- file_content_type
- Consideraciones:
	- Si el nombre del archivo se repite entonces se sobreescribe el archivo en el bucket.
		- Pueden agregarse valores específicos al nombre del archivo para identificarlo.
			- ID usuario
			- ULID (*número random irrepetible*)
			  

# Get
```python
def avatar_url(avatar):
	client = current_app.storage.client
	
	if avatar is None:
		return ""

	return client.presigned_get_object("grupo00", avatar)
```
*El parámetro avatar es el nombre del archivo que se quiere obtener, sacado de la tabla del usuario.*

# Configuración de producción
*Desde Vault*
- Definir las variables esperadas por el config.py



