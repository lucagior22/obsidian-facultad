### Características
- Programa compuesto únicamente de procesos.
- Los canales de tipo **link**.
	- Un único emisor y un único receptor.
	- Sincrónicos.
	- No se declaran.
- Exclusión mutua no es requerida.
	- No existen las variables compartidas.

# Sintaxis
- La comunicación se realiza nombrando al proceso.
### Envio de mensajes
- Bloqueante y sincrónica.
- Se demora hasta finalizada la recepción.
```c
destino!port (mensaje)
destino[i]!port (mensaje)
```

### Recepción de mensajes
- Bloqueante y sincrónica.
```c
origen?port (variable)
origen[i]?port (variable)

origen[*]?port (variable)
```

### Comunicación guardada
- Selección no determinística entre alternativas de comunicación.
	- Para la práctica, **solo recepciones**.
- Cada alternativa es una **guarda**:
	- `B; C -> S`
	- **B**: Condición booleana no obligatoria, que indica si el proceso está en condiciones o no de procesar el mensaje recibido en C.
	- **C**: Sentencia de comunicación.
		- En la práctica, **sólo recepción**.
	- **S**: Conjunto de sentencias que se ejecutarán en caso de ser elegida la guarda.
- Evaluación
	- **Éxito**: *B* es true o no existe y *C* se puede realizar sin producir demora.
		- La demora no existe dado que el emisor ya ejecutó la sentencia de envío.
	- **Fallo**: *B* es false y no importa lo que ocurra con *C*
	- **Bloqueo**: *B* es true o no existe y *C* no se puede realizar sin producir demora.
		- La demora se da porque el emisor no envió aún el mensaje.
- El **IF*** guardado:
	- Se evalúan todas las guardas.
	- Si una o mas son exitosas:
		- Se selecciona una de ellas en forma *no determinística*.
		- Se ejecuta *C*.
		- Se ejecuta *S*.
	- Si todas fallan:
		- Se sale del IF sin realizar ninguna acción.
	- Si no hay ninguna exitosa pero hay una o mas con estado bloqueo:
		- El proceso se demora en el IF hasta que haya una guarda exitosa.
			- A partir del exito se sigue el primer caso.