
> Resolver con **SEMÁFOROS** el siguiente problema. En una fábrica de muebles trabajan **50 empleados**. Al llegar, los empleados forman **10 grupos de 5 personas** cada uno, de acuerdo al orden de llegada (los 5 primeros en llegar forman el primer grupo, los 5 siguientes el segundo grupo, y así sucesivamente). Cuando un grupo se ha terminado de formar, todos sus integrantes se ponen a trabajar. Cada grupo debe armar **M muebles** (cada mueble es armado por un solo empleado); mientras haya muebles por armar en el grupo, los empleados los irán resolviendo (cada mueble es armado por un solo empleado). **Nota:** Cada empleado puede tardar distinto tiempo en armar un mueble. Solo se pueden usar los procesos **"Empleado"**, y todos deben terminar su ejecución. Maximizar la concurrencia.

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

> Resolver con **SEMÁFOROS** el siguiente problema. En una planta verificadora de vehículos, existen **7 estaciones** donde se dirigen **150 vehículos** para ser verificados. Cuando un vehículo llega a la planta, el **coordinador** de la planta le indica a qué estación debe dirigirse. El coordinador selecciona la estación que tenga menos vehículos asignados en ese momento. Una vez que el vehículo sabe qué estación le fue asignada, se dirige a la misma y espera a que lo llamen para verificar. Luego de la revisión, la estación le entrega un comprobante que indica si pasó la revisión o no. Más allá del resultado, el vehículo se retira de la planta. **Nota:** maximizar la concurrencia.

*Esta solución no es correcta dado que no se respeta el enunciado. En el mismo se dice que los vehiculos **saben qué estación le fue asignada, se dirige a la misma y espera a que lo llamen a verificar**. *

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

---
> **Resolver con _MONITORES_ el siguiente problema.**  
    Hay una boletería virtual que vende en forma online **E** entradas para un partido de fútbol a **P** personas (**P > E**) de acuerdo con el orden de llegada. Cuando la boletería atiende a una persona, si aún quedan entradas disponibles le envía el número de entrada vendida, sino le indica que no hay más entradas.  
    **Nota**: suponga que existe la función `vender()` que simula la venta de la entrada.

```c
process persona[id: 0 .. p-1]{
    int nro_entrada;
    fila.entrar()
    garita.comprar(nro_entrada)
    salir.me_voy()
}

process boleteria{
    int cant = E
    while(cant<P){
        fila.siguiente()
        garita.vender(cant_res)
        cant--
        if(cant_res>0)
            nro = vender()
        else
            nro = -1
        garita.depositar(nro)
        salir.se_fue()
    }
}
monitor fila{
    cond esp;
    int esperando;

    procedure entrar(){
        signal(llegue)
        esperando++
        wait(esp)
      }

    procedure siguiente(){
        if(esperando<1){
            wait(llegue)
        }
        signal(esp)
    }
}

monitor garita{
    boolean llegue = false;
    cond hay_alguien;esperar_venta;
    int nro_boleto;
    int id_P = -1;

    procedure vender(out id){
        if(not llegue){
            wait(hay_alguien})
        }
        id = id_P
    
    }

      procedure depositar(nro, id){
        nro_boleto[id_p]=nro
      }

    procedure comprar(nro: out int){
        llegue = true
        signal(hay_alguien)
        id_P = id
        wait(espear_venta)
        nro = nro_boleto
    }
}

monitor salir{
    cond se_fue
    
    procedure me_voy(){
        signal(se_fue)
    }
    
    procedure se_fue(){
        wait(se_fue)
    }
}
```