
## 1. ![[Pasted image 20240901174857.png]]
 **Capa de aplicación:** Su función principal es la de conectar al usuario con la red. Contiene a las aplicaciones que requieren de conectarse a la red.

## 2. ![[Pasted image 20240901174843.png]]

### a)
De encontrarse en diferentes máquinas, los procesos deben comunicarse mediante la red haciendo uso de protocolos basados en TCP/IP como HTTP, FTP o SMTP. 

### b)
Los procesos pueden cuentan con diversos mecanismos para comnunicarse con sus semejantes dentro de la misma computadora.

- Pipes: Flujo de datos unidireccional entre procesos.
- Memoria compartida: Un espacio de memoria es asignado para un grupo de procesos, desde donde van a poder leer y escribir información para asi comunicarse.
- Sockets locales: Similar al sistema de direccionamiento de la red, pero aplicado dentro de la misma computadora.
- Signals: Mecanismo  asíncrono que permite notificar a otro proceso de un determinado suceso. 

## 3. ![[Pasted image 20240901184724.png]]

El modelo de cliente/servidor establece dos roles que pueden tomar los diferentes "usuarios finales", el cliente y el servidor. El cliente es el encargado de requerir los archivos y objetos mientras que el servidor es el que almacena, procesa y provee esos datos. 

Ejemplo:
- Vida cotidiana: Una tienda. Muchos clientes se acercan a la tienda requiriendo productos que solo pueden obtener de la misma tienda. 
- Sistema informático: Página web. Las aplicaciones web cuentan con un servidor que actua como almacen de los datos del negocio, procesador de peticiones de los clientes quienes son los visitantes del sitio.

Los demás modelos de comunicación que existen son:
- P2P : Peer to peer, donde la red toma forma de grafo donde todos los nodos tienen el mismo rol.
- Mainframe Centralizado: La carga cae completamente en el servidor, dejando al cliente encargado únicamente de presentar la interfaz al usuario.

## 4. ![[Pasted image 20240901191447.png]]

Un agente de usuario se encarga de representar a un usuario y actuar en su nombre al momento de interactuar con otros servicios de red.

## 5.![[Pasted image 20240901192015.png]]

Hyper Text Markup Language: Es un lenguaje que se utiliza para estructurar y presentar contenido en la web.

Hyper Text Transfer Protocol: Es un protocolo de comunicación utilizado en la transferencia de información web.

Las diferencias: HTML es una manera de estructurar páginas web, desarrolla la interfaz al usuario, mientras que HTTP establece los contenidos, encabezados y demas objetos que se deben enviar en la comunicación en la red.

## 6.![[Pasted image 20240902084638.png]]
### a) 
Es en la primera línea de los mensajes HTTP que se evidencia el tipo del mismo. 
Para los requerimientos : 
``<METODO> <URL> <VERSIONHTTP>``
Para las respuestas:
``<VERSIONHTTP> <STATUSCODE> <STATUSMSG>``

Las cabeceras proveen información sobre el mensaje, desde el tipo de dato que se envia "content-type", cuestiones de seguridad "allow..." y demás metadatos.

![[Pasted image 20240902085236.png]]

### b)
Las cabeceras tienen formato de *key-value*, donde primero se establece la clave de la cabecera y luego su correspondiente valor. 
### c) 
```
GET /index.html HTTP/1.1
Host: www.misitio.com
User-Agent: curl/7.74.0
Accept: */*
```

## 7) ![[Pasted image 20240902090011.png]]

- -I: Head, muestra únicamente la información del documento.
- -H: Header, permite establecer un header (o mas) personalizado
- -X: Request, permite especificar el método de request a utilizar
- -s: Silent, modo silencioso, donde se muestran mensajes fundamentales únicamente.
## 8)![[Pasted image 20240902090622.png]]

### a) 
Se realizó un solo requerimiento. 

### b)
El parámetro *href* toma un valor del tipo string que contiene una dirección. 
- Para el tag `<Link>` este significa la fuente de donde se obtiene el objeto que se va a aplicar a la propiedad del link, por ejemplo, la hoja de estilos a aplicar.
- Para `<img>` el parámetro *href* no existe, sino que se utiliza el parámetro *src*, que puede cumplir un rol similar, al especificar la URL de la imagen a presentar.

### c)
No, dado que en el primer requerimiento se obtiene únicamente el archivo .html que contiene las referencias pero no los datos de los recursos deseados, por lo que se requieren mas solicitudes.

### d)
La cantidad de solicitudes se puede pensar como 1 (requerimiento del .html principal) + 1 por cada objeto extra, lo que resultaría en: 
``1 + 2 + 2 + 3 = 8``

Un navegador realiza la petición del html principal y, luego de analizarlo, ejecuta automáticamente los requerimientos necesarios para aquellas referencias encontradas.

## 9.![[Pasted image 20240902092832.png]]

`curl -v -s www.redes.unlp.edu.ar > /dev/null`
![[Pasted image 20240902092954.png]]
`curl -I -v -s www.redes.unlp.edu.ar`
![[Pasted image 20240902093527.png]]

### a)

| Primer comando                                                          | Segundo comando                                 |
| ----------------------------------------------------------------------- | ----------------------------------------------- |
| Breve                                                                   | Los headers de la respuesta están duplicados    |
| Solicitud GET                                                           | Solicitud HEAD                                  |
| El cuerpo de la respuesta se descarta redireccionandolo hacia /dev/null | No hay cuerpo en la respuesta, solo encabezados |

### b)
En caso de hacer ese cambio, el cuerpo de la respuesta no se perdería y podriamos acceder al html requerido. 
En el segundo comando se especifica que el método de requerimiento es HEAD lo que implica que en la respuesta solo viajen los encabezados y no se incluya un cuerpo, es decir el mensaje carece de cuerpo por lo que no es necesario hacerlo uno mismo.

### c)
- Cabeceras del requerimiento:
	- Primer comando: 4
	- Segundo comando: 4
- Cabeceras de la respuesta:
	- Primer comando: 8
	- Segundo comando: 8

## 10)
La cabecera *Date* muestra la fecha y hora donde se generó y envió la respuesta.

### 11)
![[Pasted image 20240902101401.png]]

- **HTTP/1.0**: El servidor cierra la conexión con el cliente cuando termina de enviar los datos, por lo que de cesar la conexión entonces significa que el servidor envió todos los datos requeridos.
- **HTTP/1.1**: Se incorporaron headers como *content-length* que indica el tamaño del objeto enviado, por lo que el cliente puede comparar los bytes que tiene con los que deberia recibir.

## 12)![[Pasted image 20240902102105.png]]

- **2XX** (Éxito):
	- 200 (ok)
	- 201 (Creado)
	- 202 (Aceptado): Se recibió la respuesta y se comunica que se aceptó mas no incluye la respuesta esperada ya que esta puede ocurrir asíncronamente.
	- 204 (Sin contenido)
- **3XX** (Redirección):
	- 300 (Multiples respuestas posibles)
	- 301 (La url del recurso requerido fue movida permanentemente)
	- 304 (No se modificó la respuesta por lo que se puede acceder a la respuesta del caché)
- **4XX** (Error de cliente):
	- 400 (Mal requerimiento)
	- 401 (No autorizado, sin autenticación)
	- 403 (Prohibido, se sabe la identidad del cliente)
	- 404 (No encontrado)
	- 405 (Método no permitido)
	- 408 (Se terminó el tiempo de requerimiento)
	- 429 (Rate-Limiting)
- **5XX** (Error de servidor):
	- 500 (Error interno del servidor)
	- 501 (Sin implementar)
	- 503 (Servicio no disponible)
	- 505 (Version de HTTP no permitida)

## 13) ![[Pasted image 20240905182617.png]] ![[Pasted image 20240905191526.png]]

### a. 
La primera linea muestra el protocolo HTTP utilizado, el código de status y el mensaje de status.
	`HTTP/1.1 200 OK`
### b.
Se reciben 7 encabezados: Date, server, Last-Modified, ETag, Accept-Ranges, Content-Length, Content-Type
### c.
El servidor es de tipo `Apache/2.4.56`
### d.
El acceso fue exitoso, esto se ve con el código y mensaje de status `200 OK`
### e.
Esto se ve en el header `Last-Modified`, fue el domingo 19 de marzo del 2023.
### f.
Para solicitar la página solo si la misma fue modificada luego de la que efectivamente fue modificada se utiliza el parámetro `-z`.  En el método GET ejecutado con el filtro de `Last-Modified` se recibe `304 Not Modified`.

## 14) ![[Pasted image 20240905184136.png]]

![[Pasted image 20240905185405.png]]

### 15) ![[Pasted image 20240905185424.png]]

### b. 
Se ejecuta la petición al recurso requerido y se cierra la conexión.
### c.
La diferencia con el inciso previo es que el protocolo HTTP1.1 soporta conexiones persistentes, donde la conexión no se cierra luego de cada petición.

## 16) ![[Pasted image 20240905190216.png]]

### a.
El comando telnet abre una conexión con el host (URL) en el puerto ingresado, permitiendo luego hacer peticiones manualmente.

### b.
Para el inciso *b* se utiliza el protocolo HTTP1.0, mientras que para el inciso *c* se envian mensajes mediante el protocolo HTTP1.1. Se solicitaron los archivos .txt prueba-http-1-0.txt y prueba-http-1-1.txt.

### c.
La diferencia principal entre ambas solicitudes es la persistencia de la conexión del protocolo HTTP1.1, donde las solicitudes no cierran su conexión inmediatamente.
### d.
La eficiencia depende del grado de paralelismo de las conexiones, dado que si este es alto, permanecer todas las conexiones podria ser muy costoso.

## 17) ![[Pasted image 20240905191455.png]]![[Pasted image 20240905191623.png]]

### d.
La URL cambia, dado que el método `GET` incluye los datos que se quieren enviar en la misma, mientras que el método `POST` estos se incluyen en el body.
### e.
Lo mismo que el punto anterior, la diferencia visible es la URL.

## 18) ![[Pasted image 20240905192856.png]]

Estas cabeceras se utilizan generalmente para simular un estado o sesión, cualidad de la cual el protocolo HTTP carece. Con Set-Cookie se envia al cliente para que este cree la cookie requerida por la aplicación.

## 19)![[Pasted image 20240905193042.png]]

Cuando se habla de protocolo binario o basado en texto, se está hablando sobre el formato de los datos enviados mediante un mensaje. El protocolo basado en texto corresponde a HTTP1.0 y HTTP1.1, mientras que HTTP2.0 implementa mensajes binarios. Las diferencias principales se encuentran en la eficiencia dado que los mensajes binarios son mas comprimidos.

## 20)![[Pasted image 20240905193345.png]]
![[Pasted image 20240905193356.png]]

### a.
La cabecera host segun el protocolo:
- **HTTP/1.0**: No existe
- **HTTP/1.1**: Es obligatoria e indica el dominio del servidor al que se realiza la petición.
- **HTTP/2.0**: La cabecera se reemplaza por :`authority`
### b.
No es correcto dado que falta el encabezado obligatorio `Host`
### c.
```
:method: GET 
:path: /index.php 
:scheme: https 
:authority: www.info.unlp.edu.ar
```
