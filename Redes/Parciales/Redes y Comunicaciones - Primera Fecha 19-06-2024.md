# 1
a)
##### Primer paso
Se debe resolver la IP del nombre www.redes.unlp.edu.ar, para esto se consulta recursivamente al DNS local, este puede contar con la ip del dominio en su caché y este resuelve la IP retornandola al cliente. Caso contrario, el DNS local comienza a desglosar el dominio y asi busca iterativamente la ip consultando en un principio al servidor ROOT, este redirecciona al DNS local al siguiente "escalón" del dominio ".ar.", este redirecciona hacia "edu.ar." y asi consecutivamente se redirecciona al *resolver* (DNS local) hasta el servidor DNS que contenga el registro NS con el valor de la IP asociada al dominio "www.redes.unlp.edu.ar".  
##### Segundo paso
Se realiza la consulta HTTP:
```http
GET contacto.html HTTP/1.1
Host: www.redes.unlp.edu.ar
```
Se realiza una petición de tipo GET, solicitando el recurso *contacto.html* al dominio www.redes.unlp.edu.ar. 
##### Tercer paso
Se recibe la respuesta HTTP desde el servidor:
```http
HTTP/1.1 200 OK 
Date: Mon, 10 Jun 2024 01:13:42 GMT 
Server: Apache/2.4.41 (Ubuntu) 
Last-Modified: Mon, 10 Jun 2024 01:13:42 GMT 
Accept-Ranges: bytes 
Content-Length: 139 
Vary: Accept-Encoding 
Content-Type: text/html 
Connection: keep-alive
```

b) 
No es autoritativa dado que el DNS local no responde al servidor autoritativo del nombre buscado, es decir, este debe realizar la búsqueda para obtener la IP requerida.

No es iterativa, esta es recursiva ya que el cliente consulta y espera la respuesta completa, mientras que en una consulta iterativa, el "buscador" tiene participación activa en cuanto a seguir redirecciones o armar la respuesta en base a recibir la respuesta en porciones (el DNS local va construyendo la respuesta a partir de las redirecciones que los servidores DNS le entregan).

c)
	i) 1.1, esto se ve en el header `Connection: keep-alive
	ii)  ```
```http	 
	GET contacto.html HTTP/1.1
	Host: www.redes.unlp.edu.ar
```

d)
	i) Se utiliza aquel de mayor prioridad, el 6, a menos que este no se encuentre disponible entonces se utilizaría el 5.
	ii) No, no es necesario.

e)
	Lo que se comunica con esta respuesta, es que el servidor no almacena mas el recurso en la dirección ingresada en la Request sino que este se encuentra en la dirección que indica el header Location de la Response.

# 2
**Teniendo en cuenta el punto anterior, usted es el administrador del dominio de DNS de ``redes.unlp.edu.ar.`` 
Considere agregar registros para servidores web, correo electrónico y las delegaciones de dominio necesarias. 
¿Cómo quedarían los registros DNS en redes.unlp.edu.ar? 
*NOTA: Indicar tipo de registro y contenido (nombre y valor) usando valores ilustrativos donde sea necesario***

```clike
// Registro NS que define el NAME SERVER al cual se debe consultar para resolver una consulta DNS
redes.unlp.edu.ar. in NS ns1.redes.unlp.edu.ar.

// Registro SOA que define los datos importantes sobre el manejo del dominio (NS principal, mail de administrador, datos para las consultas)
redes.unlp.edu.ar. in SOA ns1.redes.unlp.edu.ar. admin@redes.unlp.edu.ar. (
    2024050601 ; Serial (AAAAMMDDNN)
    3600       ; Refresh (1 hora)
    1800       ; Retry (30 minutos)
    604800     ; Expire (7 días)
    86400      ; Minimum TTL (1 día)
)

// Registro MX que define el servidor de mail al que se consulta para resolver consultas de mail
redes.unlp.edu.ar in MX 5 mail.redes.unlp.edu.ar

// Registros A que definen las IP correspondientes para los distintos dominios
mail.redes.unlp.edu.ar in A 120.36.25.10
ns1.redes.unlp.edu.ar in A 120.36.25.15
www.redes.unlp.edu.ar in A 126.0.115.1
```
# 3
