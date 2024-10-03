
![[Pasted image 20241003110440.png]]

```c
array grupos[10] = ([10] 0);
array muebles_a_armar[10] = ([10] M)
sem barrera[10] = ([10] 0)
sem mutex = 1
sem mutex_muebles = 1
int i = 0

process empleado[id:0..49]{
    int mi_grupo
    P(mutex)
    mi_grupo = i
    grupos[i]++
    if (grupos[i] == 5) {
        for j:= 1 to 4 {
            v(barrera[i])
        }
        i++
        V(mutex)
    }
    else {
        V(mutex)
        P(barrera[i])
    }
    
    P(mutex_muebles)
    while (muebles_a_armar[mi_grupo] > 0) {
        muebles_a_armar[mi_grupo]--
        V(mutex_muebles)
        //armarMueble
        P(mutex_muebles)
    }
    V(mutex_muebles)
}

```

---

![[Pasted image 20241003115145.png]]

```c
sem hayGenteCola[7] = ([7] 0)
sem turnoVehiculo[150] = ([150] 0);
sem colaEstacion[7] = ([7] 1)
sem mutexCoordinador = 1;
sem mutexCola[7] = ([7] 1);
sem mutexEsperando = 1;
sem hayGenteColaCoordinador = 0;
Queue colaCoordinador; 
array<int> esperando[7] = ([7] 0);
sem mutexCola = 1;
array<Queue> cola[7];

process vehiculo[id: 0 .. 149]{
    P(mutexColaCoordinador)
    colaCoordinador.push(id)
    V(hayGenteColaCoordinador)
    V(mutexColaCoordinador)
    p(turnoVehiculo[id])
}

process estacion::[id : 0..6] {
    int vehiculo;
    while (true){
        p(hayGenteCola[id])
        p(mutexCola[id])
        vehiculo = cola[id].pop()
        v(mutexCola[id])
        
        //se hace la revision
        
        v(turnoVehiculo[vehiculo])
        P(mutexEsperando)
        esperando[id] -= 1
        V(mutexEsperando)
    }
}

process coordinador{
    int id; int colaMinima;
    for (int i = 0; i < 150; i++) {
        // Espero que haya alguien en la cola
        P(hayGenteColaCoordinador)
        
        // Saco vehiculo de la cola
        P(mutexColaCoordinador)
        id = colaCoordinador.pop()
        V(mutexColaCoordinador)
        
        // Decido a donde va
        P(mutexEsperando)
        colaMinima = min(esperando);
        V(mutexEsperando)
        
        P(mutexCola[colaMinima])
        cola[colaMinima].push(id);
        V(mutexCola[colaMinima])
        
        P(mutexEsperando)
        esperando[colaMinima] += 1;
        v(hayGenteCola[colaMinima])
        V(mutexEsperando)
    }
}
```