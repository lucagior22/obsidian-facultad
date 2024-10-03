# 1
> Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

El sistema de nombres de dominio (DNS) es el directorio telefónico de Internet. Las personas acceden a la información en línea a través de nombres de dominio como nytimes.com o espn.com. Los navegadores web interactúan mediante direcciones de Protocolo de Internet (IP). El DNS traduce los nombres de dominio a direcciones IP para que los navegadores puedan cargar los recursos de Internet.

---
# 2
> ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

Un **Root Server** es un servidor DNS que es responsable de la primera resolución de la busqueda de nombres, resolviendo una parte de la consulta y derivando a otro servidor de jerarquia menor. Se habla de 13 root servers, nombrados de la A a la M.

Un **Top-Level Domain** es un dominio que se encuentra en la cima de la estructura de dominios DNS. Los **gTLD**, son los dominios *genéricos* que no pertenecen a ningun país y que no están orientados a un propósito específico.

---
# 3
> ¿Qué es una respuesta del tipo autoritativa?

Una respuesta autoritativa es aquella que proviene de un servidor que cuenta con el recurso original buscado. No se puede recibir una respuesta autoritativa de un servidor que responde con un recurso de la caché.

---
# 4
> ¿Qué diferencia una consulta DNS recursiva de una iterativa?

Recursiva | Iterativa
-- | --
El servidor que recibe la consulta se encarga de resolver toda la consulta por si mismo| El servidor que recibe la consulta resuelve la parte que puede resolver 
Retorna la dirección IP (o un error) | Retorna la mejor respuesta, si no la completa responde con una dirección DNS que puede seguir la solicitud

---
# 5
> ¿Qué es el resolver?

Es un componente del sistema DNS que recibe una consulta y se encarga de responder a la misma, ya sea iterativa o recursivamente.

---
# 6
> Describa para qué se utilizan los siguientes tipos de registros de DNS: 
> 	1. A 2. NS 3. MX 4. CNAME 5. PTR 6. SOA 7. AAAA 8. TXT 9. SRV

1. **A**: Registra la dirección ipv4. 
2. **NS**: Registra los servidores de nombres que son autoritativos para un dominio.
3. **MX**: Registra los servidores de correo para un dominio.
4. **CNAME**: Registra un alias para un dominio. Utilizado para redireccionar varios nombres a un dominio.
5. **PTR**: Registra el puntero utilizado para la resolución inversa de dirección IP a dominio.
6. **SOA**: Registra la información de autoridad sobre la zona del dominio. 
7. **TXT**: Registra información de texto arbitriaria asociada a un dominio.
8. **SRV**: Define los servicios específicos (protocolo y puerto) ofrecidos por un dominio.

---
# 7
> En Internet, un dominio suele tener más de un servidor DNS, ¿por qué cree que esto es así?

Distribución de carga y redundancia. Puede mejorar el tiempo de respuesta y es más seguro frente a fallos.

---
# 8
> Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son secundarios (o esclavos). ¿Cuál es la razón de que sea así?

Proporciona redundancia de datos y facilita la administración.

---
# 9
> Explique brevemente en qué consiste el mecanismo de transferencia de zona y cuál es su finalidad.

La transferencia de zona permite que un servidor secundario tenga una copia de los datos que los servidores primarios tienen. Las zonas se definen segun una porción de la estructura de nombres, `unlp.edu.ar` es una zona.

---
# 10
> Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar). Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio. ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios). Indicar qué registros de DNS se deberían agregar.

Se deben agregar los registros **NS** correspondientes a los servidores de `redes.unlp.edu.ar.` como `ns1.redes.unlp.edu.ar.` y `ns2.redes.unlp.edu.ar.`. Además, se configura la nueva zona de ``redes.unlp.edu.ar`` para reconocer a sus servidores como autoritativos, logrando esto medianto los registros **SOA** y**A**.

---
# 11
