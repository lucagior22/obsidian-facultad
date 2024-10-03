# Práctica 1 - Variables Compartidas

### 1.

### a) Verdadero. P1 -> P2 -> P3

### b) Verdadero

    - x = 0 : P1 pasa el if
    - x = 0 : P3 calcula (x*3)
    - x = 10 : P1 termina
    - x = 21 : P3 termina
    - x = 22 : P2 termina

### c) Verdadero

	- x = 0 : P1 pasa el if
	- x = 0 : P3 calcula (x*3)
	- x = 10 : P1 termina
	- x = 11 : P2 termina
	- x = 23 : P3 termina

## 2.  
```java
int arreglo[M]
int N = ?
int cantidadExistencias = 0

Process verificarExistencia [i : 0..M-1]
	if (arreglo[i] == N) <cantidadExistencias++>
```


## 3.

En un momento donde cant = 0 y el buffer se encuentra vacio, puede ocurrir que el productor haga cant++ y mientras que este guarda el elemento en el arreglo, el consumidor quiere retirarlo, ocasionando asi una inconsistencia, por lo que todo aquello relacionado al manejo del arreglo deberia ser exlusión mutua.

### a)

```java
int cant = 0; int pri_ocupada = 0; int pri_vacia = 0; int buffer[N];

Process Productor:: {  
	while (true) {
		//produce elemento
		<await (cant < N); cant++
		buffer[pri_vacia] = elemento;
		pri_vacia = (pri_vacia + 1) mod N; >
	}
}

Process Consumidor:: {  
	while (true) {
		<await (cant > 0); cant--
		elemento = buffer[pri_ocupada];
		pri_ocupada = (pri_ocupada + 1) mod N; >
		//consume elemento
	}
}
```
  

### b)

```java
int cant = 0; int pri_ocupada = 0; int pri_vacia = 0; int buffer[N];  
Process Productor::[id : 0..P-1] {  
	while (true) {
		//produce elemento
		<await (cant < N); cant++
		buffer[pri_vacia] = elemento;
		pri_vacia = (pri_vacia + 1) mod N; >
	}
}  

Process Consumidor::[id : 0..C-1] {  
	while (true) {
		<await (cant > 0); cant--
		elemento = buffer[pri_ocupada];
		pri_ocupada = (pri_ocupada + 1) mod N; >
		//consume elemento
	}
}  
```

## 4.

```java
Recurso recursos[5]; int recursosLibres = 5;
Process proceso::[0..N-1] {
	while (true) {
		<await (recursosLibres > 0); recurso = recursos.pop(); recursosLibres-- >
		// Utiliza recurso
		<recursos.push(recurso); recursosLibres++>
	}
}
```

## 5.

### a)

La de abajo fue la primera solución que me salió, pero es incorrecto o innecesario el uso de un boolean ya que el <> ya aplica exclusión mutua

```java
Impresora impresora; boolean enUso = false;
Process persona::[0..N-1] {
	Documento documento;
	<await (!enUso); enUso = true;>
	impresora.imprimir(documento)
	<enUso = false>
}  
```

Esta es la respuesta correcta

```java
Impresora impresora;
Process persona::[0..N-1] {
	Documento documento;
	<impresora.imprimir(documento)>
}  
```

### b)
Suponiendo que contamos con una estructura de cola

```java
Impresora impresora; int siguiente = -1; Cola cola;
Process persona::[id : 0..N-1] {
	Documento documento;
	<await (siguiente == -1 || siguiente == id);>
	impresora.imprimir(documento)
	if (cola.empty()) siguiente = -1
	else <siguiente = cola.pop()>
}  
```

Con algoritmo Ticket:

```java
Impresora impresora;
int siguienteNumero = 1; // Es el siguiente numero de ticket que retire el proceso que quiere acceder a la SC
int turnoActual = 1; // Es el turno que puede acceder a la SC
turno[1:n] = ([n] = 0); // Arreglo de n posiciones donde cada proceso almacena su numero de turno
Process persona::[id : 1..N] {
	Documento documento;
	<turno[i] = siguienteNumero; siguienteNumero = siguienteNumero + 1;>
	<await (turno[i] == turnoActual)>
	impresora.imprimir(documento)
	turnoActual = turnoActual + 1
}  
```

### c)

Solucion dudosa

```java
Impresora impresora;
int siguienteNumero = 1;
int turnoActual = 1;
turno[1:n] = ([n] = n+1);
Process persona::[id : 1..N] {
	Documento documento;
	<turno[i] = i>
	<await (min(turno) == i)>
	impresora.imprimir(documento)
	<turno[i] = n+1 >
}  
```

### d)

```java
Impresora impresora;
int siguienteNumero = 1; 
int turnoActual = 1;
turno[1:n] = ([n] = n + 1); 
continuar[1:n] = ([n] = false)
boolean listo = false 

Process persona::[id : 1..N] {
	Documento documento;
	<turno[i] = siguienteNumero; siguienteNumero = siguienteNumero + 1;>
	<await (continuar[i])>
	impresora.imprimir(documento)
	<turno[i] = n + 1>
	<listo = true>
}  

Process coordinador:: {
	while (true) {
		int siguiente = min(turno) 
		continuar[siguiente] = true
		<await (listo); continuar[siguiente = false]>
		listo = false
	}
}
```

Intento, falta ser revisado...

## 6)

```java
int turno = 1;
Process SC1:: { 
    while (true) { 
        while (turno == 2) 
            skip;
        SC;
        turno = 2;
        SNC;
    }
}

Process SC2:: { 
    while (true) { 
        while (turno == 1) 
            skip;
        SC;
        turno = 1;
        SNC;
    }
}

```

**Propiedades a cumplir:**
1. Exclusión mutua
2. Ausencia de Deadlock
3. Ausencia de demora innecesaria
4. Eventual entrada

1. Se cumple, ya que la variable que se utiliza para entrar al SC solo tiene un valor posible y cada proceso requiere que esta variable tome un valor diferente del otro.
2. No existe deadlock ya que la variable de la que depende la entrada al SC toma 2 valores y en ninguno de esos genera una traba.
3. El algoritmo es simple, cuando el valor corresponde entonces entran al SC, no hay demora innecesaria.
4. Dado que el SC y SNC son segmentos finitos entonces se garantiza que eventualmente cada proceso va a poder entrar al SC.
## 7)

```java
int actual = -1;
boolean listo = false;

Process SC[i : 1..n] {
	while (true) {
		while (actual <> i) skip;
		// SC
		listo = true
		while (listo) skip
	}
}

Process Coordinador:: {
	while(true) {
		for i in 1..n {
			actual = i
			while (!listo) skip;
			listo = false
		}
	}
}

```
