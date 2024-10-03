
Problemas de HTTP 1.0 
- Un requsito por conexión.

HTTP 1.1 Lo soluciona permitiendo las conexiones persistentes.

Otras soluciones son pipelining o conexiones paralelas. Estas tienen sus desventajas:
- Muchas request = Mucha información repetida (Headers)

# HTTP2.0

Quiere resolver esto, incorpora los siguientes cambios:
- Protocolo binario en lugar de ASCII
- Multiplexa varios request en una petición, esto evita el overhead de cerrar/abrir conexión y que las peticiones se bloqueen entre si.
- Compresión de encabezado.
- Servidores pueden *pushear* datos a los clientes.
- TLS/SSL en la mayoría de implementaciones, aunque no se exija.

**Multiplexación**: Todos los streams en una misma conexión, donde son identificados y divididos en frames. Dentro de una misma conexión pueden haber varios streams. Un stream es una petición lógica (excluye la cuestión de abrir/cerrar la conexión).

**Flow-control**: Los streams dentro de una misma conexión tienen flow-control individual.

**Priorización**: El orden de los streams puede cambiar segun la prioridad asignada a cada uno, a diferencia de la manera secuencial en la que se daba en protocolos previos.

# DNS

**Esquema de nombres**: Labels separados por puntos, desplegando una estructura de arbol.

![[Pasted image 20240905095055.png]]


