### Arquitectura de microservicios
- Division de aplicaciones en componentes mas pequeños, independientes entre si.
- Un conjunto de microservicios lleva a cabo las mismas tareas que una aplicación monolítica.
- La escalabilidad se ve facilitada con microservicios.
- El mantenimiento se ve simplificado
	- Se mantienen o actualizan los componentes individualmente.

### Comunicación entre servicios
- API HTTP/REST, con formato json

### Frameworks js
- Nace **Jquery** para facilitar el desarrollo en javascript.
- Los frameworks buscan facilitar la sincronización del estado y la interfaz de la aplicación web.

### VUE.js
*Framework de js para front-end*
- Progresivo
- Two way data-binding
- Patrón Model-View-Viewmodel

### Two Way Data Binding
*Permite que los cambios en la interfaz de usuario se reflejen automáticamente en el modelo de datos y viceversa.*

```html
<template>
  <div>
    <input v-model="message" placeholder="Escribe algo...">
    <p>El mensaje es: {{ message }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "" // Variable enlazada bidireccionalmente con el input
    };
  }
};
</script>
```

*Ejemplo en un formulario*
```html
<template>
  <div>
    <form>
      <label>
        Nombre:
        <input v-model="user.name" placeholder="Nombre" />
      </label>
      <label>
        Correo:
        <input v-model="user.email" placeholder="Correo" />
      </label>
    </form>
    <p>Nombre ingresado: {{ user.name }}</p>
    <p>Correo ingresado: {{ user.email }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {
        name: "",
        email: ""
      }
    };
  }
};
</script>

```