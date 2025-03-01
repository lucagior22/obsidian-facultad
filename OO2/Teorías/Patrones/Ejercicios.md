### **Ejercicio 1: Sistema de Carga de Archivos**

**Enunciado:**  
Un equipo de desarrollo implementó un sistema que permite subir archivos a distintos servicios en la nube (Google Drive, Dropbox y OneDrive). Para manejar la carga, crearon una interfaz `Uploader` con un método `upload()`, y cada servicio tiene su propia clase (`GoogleDriveUploader`, `DropboxUploader`, etc.) que implementa la interfaz.  
Para que el código sea más flexible, usaron **Factory Method** para la creación de los distintos `Uploader`.

**Preguntas:**

1. ¿Es **Factory Method** el mejor patrón para este caso?
2. ¿Podría haberse utilizado otro patrón más adecuado? ¿Por qué?
3. **Aquí tienes la implementación utilizada. Analízala y encuentra posibles errores o mejoras:**

``` java
interface Uploader {     
	void upload(String file); 
}

class GoogleDriveUploader implements Uploader {     
	public void upload(String file) {         
		System.out.println("Uploading " + file + " to Google Drive...");     } }  
		
class DropboxUploader implements Uploader {     
	public void upload(String file) {         
		System.out.println("Uploading " + file + " to Dropbox...");
	 } }  
class OneDriveUploader implements Uploader {     
	public void upload(String file) {         
		System.out.println("Uploading " + file + " to OneDrive...");     } }  
		
// Aquí está la fábrica 
abstract class UploaderFactory {     abstract Uploader createUploader(); }  

class GoogleDriveUploaderFactory extends UploaderFactory {     
	public Uploader createUploader() {         
		return new GoogleDriveUploader();     
		} }  

class DropboxUploaderFactory extends UploaderFactory {     
	public Uploader createUploader() {         
		return new DropboxUploader();     
	} }  
	
class OneDriveUploaderFactory extends UploaderFactory {     
	public Uploader createUploader() {         
		return new OneDriveUploader();     
	} }  
// Cliente 
public class Main {     
	public static void main(String[] args) {         
		UploaderFactory factory = new GoogleDriveUploaderFactory();
		Uploader uploader = factory.createUploader();
		uploader.upload("document.pdf");     
} }
```
**Respuesta**: 
1. Si, Factory Method es el patrón apropiado para manejar la creación dinámicamente de los distintos Uploaders, delegando la responsabilidad de creación a los Factorys.
2. No, ya que al reducir las opciones a patrones creacionales únicamente resta el Builder, pero el proceso de creación de los Uploaders no los suficientemente complejo (pasos) para ameritar este patrón.
3. No encuentro cambios.

---
### **Ejercicio 2: Sistema de Edición de Documentos**

**Enunciado:**  
Un software de edición de documentos permite agregar funcionalidades extra a los documentos, como cifrado, compresión y firma digital. Para ello, cada documento (`TextDocument`, `PDFDocument`, `WordDocument`) extiende de una clase base `Document` e implementa estos métodos en cada subclase.  
Para permitir agregar múltiples funcionalidades sin modificar el código base, usaron el  patrón **Strategy**.

**Preguntas:**

4. ¿Es **Strategy** el patrón correcto para este caso?
5. Si no lo es, ¿qué patrón sería más adecuado y por qué?
6. **Analiza la siguiente implementación y encuentra posibles errores o mejoras:**

```java
interface ProcessingStrategy {
    void process(Document doc);
}

class EncryptionStrategy implements ProcessingStrategy {
    public void process(Document doc) {
        System.out.println("Encrypting document...");
    }
}

class CompressionStrategy implements ProcessingStrategy {
    public void process(Document doc) {
        System.out.println("Compressing document...");
    }
}

class SigningStrategy implements ProcessingStrategy {
    public void process(Document doc) {
        System.out.println("Signing document...");
    }
}

abstract class Document {
    ProcessingStrategy strategy;
    
    void setStrategy(ProcessingStrategy strategy) {
        this.strategy = strategy;
    }

    void applyProcessing() {
        strategy.process(this);
    }
}

class PDFDocument extends Document {}
class WordDocument extends Document {}

public class Main {
    public static void main(String[] args) {
        Document doc = new PDFDocument();
        doc.setStrategy(new EncryptionStrategy());
        doc.applyProcessing();
    }
}
```

1. No es correcto utilizar Strategy para este enunciado, dado que el mismo busca agregar responsabilidades dinámicamente y el Strategy no busca permitir eso sino que permite redefinir el funcionamiento de algoritmos según se busque.
2. El patrón adecuado sería el Decorator. El Decorator permite que dinámicamente se agreguen responsabilidades a un objeto, que es lo que se busca al querer sumar mas comportamiento. Además, permite que se sumen múltiples responsabilidades al mismo tiempo manteniendo la interfaz del objeto original intacta. 
3. No es correcto utilizar Strategy.

---

### **Ejercicio 3: Sistema de Gestión de Usuarios**

**Enunciado:**  
Se implementó un sistema donde cada usuario tiene diferentes roles (admin, moderador, usuario). Dependiendo del rol, el sistema muestra opciones distintas en la interfaz y permite realizar acciones específicas.  
Para manejar estos estados, se utilizó **State**, creando una clase para cada rol (`AdminState`, `ModeratorState`, `UserState`) que define las acciones permitidas.

**Preguntas:**

1. ¿El uso de **State** es correcto en este caso?
2. ¿Habría otra manera más simple de manejar estos roles sin necesidad de `State`?
3. **Observa la implementación y analiza si tiene errores o se puede mejorar:**

```java
interface UserState {
    void showOptions();
}

class AdminState implements UserState {
    public void showOptions() {
        System.out.println("Admin options: Manage users, Edit content, View stats...");
    }
}

class ModeratorState implements UserState {
    public void showOptions() {
        System.out.println("Moderator options: Edit content, View stats...");
    }
}

class UserStateImpl implements UserState {
    public void showOptions() {
        System.out.println("User options: View content...");
    }
}

class User {
    private UserState state;

    public void setState(UserState state) {
        this.state = state;
    }

    public void displayOptions() {
        state.showOptions();
    }
}

public class Main {
    public static void main(String[] args) {
        User user = new User();
        user.setState(new AdminState());
        user.displayOptions();
    }
}
```

**Respuesta**:
1. No, ya que el state sirve para cuando un objeto cambia su comportamiento según su estado y este cambia através de la vida del objeto. Además, el cambio de estado en State, es automanejado, no se cambia desde afuera. En cambio, en este caso, el objeto tiene un "tipo" para toda su vida.
2. La implementación correcta seria utilizar Type Object.

---

### **Ejercicio 4: Sistema de Autenticación**

**Enunciado:**  
Un sistema de autenticación permite a los usuarios iniciar sesión usando diferentes métodos (usuario/contraseña, autenticación con Google, autenticación con Facebook). Para encapsular estas opciones, se utilizó **Decorator**, creando clases como `PasswordAuthDecorator`, `GoogleAuthDecorator` y `FacebookAuthDecorator`, que se apilan sobre una clase base `Authenticator`.

**Preguntas:**

3. ¿Es **Decorator** el patrón correcto para este caso?
4. ¿Qué problemas podrían surgir al usar **Decorator** aquí?
5. **Analiza la siguiente implementación y encuentra errores o mejoras:**

```java
interface Authenticator {
    void authenticate();
}

class BasicAuth implements Authenticator {
    public void authenticate() {
        System.out.println("Authenticating with username and password...");
    }
}

class AuthDecorator implements Authenticator {
    protected Authenticator auth;

    public AuthDecorator(Authenticator auth) {
        this.auth = auth;
    }

    public void authenticate() {
        auth.authenticate();
    }
}

class GoogleAuthDecorator extends AuthDecorator {
    public GoogleAuthDecorator(Authenticator auth) {
        super(auth);
    }

    public void authenticate() {
        super.authenticate();
        System.out.println("Authenticating with Google...");
    }
}

class FacebookAuthDecorator extends AuthDecorator {
    public FacebookAuthDecorator(Authenticator auth) {
        super(auth);
    }

    public void authenticate() {
        super.authenticate();
        System.out.println("Authenticating with Facebook...");
    }
}

public class Main {
    public static void main(String[] args) {
        Authenticator auth = new GoogleAuthDecorator(new BasicAuth());
        auth.authenticate();
    }
}
```

---

### **Ejercicio 5: Motor de Renderizado de Video**

**Enunciado:**  
Un motor de renderizado de video debe manejar distintos formatos de salida (MP4, AVI, MKV). En lugar de escribir una lógica única con múltiples condiciones, se decidió usar **Template Method**, creando una clase base `VideoRenderer` con métodos abstractos `encode()`, `compress()` y `export()`, que luego son implementados por subclases como `MP4Renderer`, `AVIRenderer`, etc.

**Preguntas:**

6. ¿Es **Template Method** una buena elección en este caso? ¿Por qué?
7. ¿Podría haberse usado otro patrón más flexible?
8. **Evalúa la implementación y encuentra posibles errores o mejoras:**

```java
abstract class VideoRenderer {
    abstract void encode();
    abstract void compress();
    abstract void export();

    public void render() {
        encode();
        compress();
        export();
    }
}

class MP4Renderer extends VideoRenderer {
    public void encode() { System.out.println("Encoding MP4..."); }
    public void compress() { System.out.println("Compressing MP4..."); }
    public void export() { System.out.println("Exporting MP4..."); }
}

public class Main {
    public static void main(String[] args) {
        VideoRenderer renderer = new MP4Renderer();
        renderer.render();
    }
}
```

---

---

---
# Enunciados

## 1
Una compañía de software está desarrollando una aplicación de edición de imágenes. Cada imagen puede ser modificada mediante diferentes efectos visuales, como aplicar filtros, agregar bordes y sombras. Lo importante es que los efectos deben poder combinarse dinámicamente sin modificar la estructura base de la imagen.  
Por ejemplo, un usuario puede aplicar primero un filtro de blanco y negro, luego agregar un borde y después una sombra, o en otro orden según su preferencia. La solución debe permitir agregar nuevos efectos en el futuro sin alterar la implementación de los efectos existentes.

**Respuesta:**
Requerimientos:
- No modificar la estructura base de la imagen.
- Agregar responsabilidades dinámicamente.
- Múltiples responsabilidades al mismo tiempo.
- Responsabilidades intercambiables.

Patrón *Decorator*
```java
public interface IImagen {
	imprimirImagen()
}

// objeto base
public class Imagen implements IImagen{
	...
}

public abstract class DecoratorBase implements IImagen {
	IImagen imagen;

	public DecoratorBase(IImagen imagen) {
		this.imagen = imagen;
	}

	public void imprimirImagen() {
		imagen.imprimirImagen()
	}
}

public class DecoratorSombra extends DecoratorBase {
	public void imprimirImagen() {
		// agregar filtro
		super.imprimirImagen()
	}
}

public class DecoratorColor extends DecoratorBase {
	public void imprimirImagen() {
		// agregar filtro
		super.imprimirImagen()
	}
}

public class DecoratorBorde extends DecoratorBase {
	public void imprimirImagen() {
		// agregar filtro
		super.imprimirImagen()
	}
}

public static void main(String args[]) {
	IImagen miImagen = importarImagen() // Un metodo que obtiene una imagen X
	IImagen miImagenConFiltros = new DecoratorBorde( 
		new DecoratorSombra(
			miImagen
		)
	)

	miImagenConFiltros.imprimirImagen()
}

```

# 2
Un sistema de pagos en línea requiere una solución flexible para procesar transacciones mediante distintos métodos de pago. Actualmente, soporta tarjeta de crédito, PayPal y transferencia bancaria, pero en el futuro se podrían agregar más opciones sin modificar la lógica central del sistema.  
Cada procesador de pagos debe implementarse de forma independiente y debe poder ser instanciado dinámicamente según la elección del usuario. Además, todos los métodos de pago deben seguir una interfaz común para garantizar su compatibilidad con la plataforma.

**Respuesta:**
Requerimientos:
- Interfaz transparente para los métodos de pago.
- Creación de instancias de pago.
- No hay diferentes variantes de un mismo producto o un proceso de creación complejo.

Patrón Factory.
```java
public interface ProcesadorPago {
	public pagar(int monto);
}

public class Credito implements ProcesadorPago {
	public pagar(int monto) {
		// lógica
	}
}

public class Paypal implements ProcesadorPago {
	public pagar(int monto) {
		// lógica
	}
}

public class Transferencia implements ProcesadorPago {
	public pagar(int monto) {
		// lógica
	}
}

public abstract class ProcesadorFactory {
	public abstract ProcesadorPago obtenerProcesador();
}

public class CreditoFactory extends ProcesadorFactory {
	public ProcesadorPago obtenerProcesador() {
		return new Credito()
	}
}

public class PaypalFactory extends ProcesadorFactory {
	public ProcesadorPago obtenerProcesador() {
		return new Paypal()
	}
}

public class TransferenciaFactory extends ProcesadorFactory {
	public ProcesadorPago obtenerProcesador() {
		return new Transferencia()
	}
}

public static void Main(String args[]) {
	String procesadorDeseado = "Paypal"
	
	ProcesadorFactory factory;

	switch procesadorDeseado {
		case "Paypal" -> factory = new PaypalFactory(),
		case "Transferencia" -> factory = new TransferenciaFactory(),
		case "Credito" -> factory = new CreditoFactory()
	}

	ProcesadorPago procesador = factory.obtenerProcesador();

	procesador.pagar()
}
``` 

### **Enunciado 3:**

En una aplicación de reserva de viajes, los usuarios pueden crear paquetes personalizados que incluyan vuelos, hoteles, alquiler de coches y actividades adicionales, como excursiones o entradas a eventos.  
El sistema debe permitir construir estos paquetes de manera progresiva, agregando o eliminando elementos según las necesidades del usuario. Es importante que los pasos de la construcción sean flexibles para adaptarse a distintos tipos de paquetes sin que el código tenga que modificarse constantemente.

**Respuesta**
Puntos clave:
- Se busca crear (Factory o Builder)
- Existen diferentes variantes de un mismo objeto "Paquete" (no me dan las variantes asi que las creo yo)
	- Un **Paquete Económico** puede incluir solo vuelos y hospedaje básico.
	- Un **Paquete Premium** puede incluir vuelos, hoteles 5 estrellas, alquiler de autos y excursiones.

```java
public class Paquete {
	private String hotel;
	private String vuelos;
	private boolean alquilerAutos;
	private String actividades;

	public void setVuelo(String vuelo) { this.vuelo = vuelo; } 
	public void setHotel(String hotel) { this.hotel = hotel; } 
	public void setAlquilerAuto(boolean alquilerAuto) { this.alquilerAuto = alquilerAuto; } 
	public void setActividades(String actividades) { this.actividades = actividades; }
}

public abstract class PaqueteBuilder {
	protected Paquete paquete;

	public void reset() {
		this.paquete = new Paquete()
	}

	public abstract setVuelo();
	public abstract setHotel();
	public abstract setAlquilerAuto();
	public abstract setActividades();

	public Paquete getPaquete() {
		return paquete;
	}
}

public class PaqueteEconomicoBuilder extends PaqueteBuilder {
	public void setVuelo() {
		this.paquete.setVuelo("Clase económica.");
	}

	public void setHotel() {
		this.paquete.setHotel("2 estrellas.");
	}

	public void setAlquilerAuto() {
		this.paquete.setAlquilerAuto(false);
	}

	public void setActividades() {
		this.paquete.setActividades("Tour por la ciudad.")
	}
}

public class PaquetePremiumBuilder extends PaqueteBuilder {
	public void setVuelo() {
		this.paquete.setVuelo("Clase business.");
	}

	public void setHotel() {
		this.paquete.setHotel("5 estrellas.");
	}

	public void setAlquilerAuto() {
		this.paquete.setAlquilerAuto(true);
	}

	public void setActividades() {
		this.paquete.setActividades("Tour por la ciudad, parque de diversiones.")
	}
}

public class Director {
	PaqueteBuilder builder;

	public void setBuilder(PaqueteBuilder builder) {
		this.builder = builder;
	}
 
	public Paquete make() {
		builder.reset()
		builder.setVuelo()
		builder.setAlquilerAuto()
		builder.setActividades()
		return builder.getPaquete()
	}
}

public static void Main(String args[]) {
	Director director = new Director()

	director.setBuilder(new PaquetePremiumBuilder())

	Paquete miPaquete = director.make()
}
```

---

### **Enunciado 4:**

Una empresa de logística necesita integrar dos sistemas heredados: uno que gestiona el inventario de productos y otro que administra las órdenes de compra. Ambos sistemas fueron desarrollados en diferentes momentos y utilizan interfaces incompatibles.  
El nuevo sistema debe permitir que estos dos módulos se comuniquen sin que sea necesario modificar el código original de cada uno, actuando como un intermediario que traduce las llamadas de un sistema al otro.

```java
// 📌 Sistema de Inventario (Interfaz heredada 1)
public interface Inventario {
    void verificarDisponibilidad(String producto);
    void actualizarStock(String producto, int cantidad);
}

// 📌 Sistema de Órdenes de Compra (Interfaz heredada 2)
public interface OrdenesCompra {
    void realizarPedido(String producto, int cantidad);
    void cancelarPedido(String producto);
}

// 📌 Interfaz estándar que el nuevo sistema usará para interactuar con ambos
public interface SistemaLogistica {
    boolean hayStockDisponible(String producto);
    void procesarPedido(String producto, int cantidad);
}
```

**Respuesta**
Puntos clave:
- Necesidad de comunicar dos interfaces incompatibles.
- Sistemas externos (no se pueden modificar)

Patrón Adapter

---

### **Enunciado 5:**

Un desarrollador está creando un sistema de exportación de datos donde los usuarios pueden guardar la información en distintos formatos, como CSV, XML y JSON.  
El proceso de exportación sigue una serie de pasos bien definidos:

1. Obtener los datos a exportar.
2. Convertir los datos al formato deseado.
3. Aplicar validaciones específicas para cada formato.
4. Escribir el archivo resultante.  
    Algunos de estos pasos son comunes a todos los formatos, pero otros dependen del tipo de exportación seleccionada, por lo que deben ser personalizables sin alterar la estructura del proceso.

**Respuesta:**
Puntos clave:
- Se definen pasos.
- Se busca seguir una estructura pero cambiando pasos segun variantes.

Patrón Template Method
```java
public abstract class Archivo {
	public void exportar() {
		obtenerDatos()
		covertir()
		validar()
		escribir()
	}

	public abstract void obtenerDatos();
	public abstract void convertir();
	public abstract void validar();
	public abstract void escribir();
}
``` 
Las diferentes clases de CSV, JSON, etc, deben heredar de Archivo y redefinir los métodos abstractos.

---

### **Enunciado 6:**

En una aplicación para visualizar imágenes de alta resolución, cargar las imágenes completamente desde el inicio puede consumir muchos recursos y afectar el rendimiento del sistema.  
Para solucionar esto, se necesita un mecanismo que permita mostrar una versión previa o en baja calidad de la imagen y cargar la versión completa solo cuando el usuario la solicite explícitamente. Esta solución debe actuar como una representación de la imagen real, retrasando su carga hasta que sea absolutamente necesario.

**Respuesta:**
- Se necesita una representación de un objeto hasta que el original haya finalizado su carga.

Patron Proxy
```java
public interface IImagen {
	public void mostrar()
}

public class Imagen implements IImagen {
	public String nombreArchivo;

	public Imagen(String nombreArchivo) {
		this.nombreArchivo = nombreArchivo;
		cargar()
	}

	private void cargar() {
		// cargar imagen..
	}

	public void mostrar() {
		// mostrar
	}
}

public class ProxyImagen implements IImagen {
	private Imagen imagen;
	private String nombreArchivo;

	public ProxyImagen (String nombreArchivo) {
		this.nombreArchivo = nombreArchivo;
	}

	public void mostrar() {
		if (imagen == null) {
			this.imagen = new Imagen(nombreArchivo)
		}
		imagen.mostrar()
	}
}


```

---

### **Enunciado 7:**

Un sistema de gestión de usuarios debe manejar diferentes roles, como administrador, moderador y usuario estándar. Dependiendo del rol asignado, el sistema debe permitir diferentes acciones y mostrar interfaces distintas.  
Además, es importante que los usuarios puedan cambiar de rol en tiempo de ejecución sin afectar la estructura del sistema. Por ejemplo, un moderador puede ser promovido a administrador y recibir nuevas funcionalidades automáticamente.

---

### **Enunciado 8:**

Una máquina expendedora de snacks y bebidas cambia su comportamiento según su estado actual.

- Cuando está esperando una moneda, solo acepta billetes o monedas.
- Una vez que recibe el dinero, permite la selección del producto.
- Si el usuario elige un producto disponible, la máquina lo dispensa y da el cambio si es necesario.
- Si el producto seleccionado está agotado, muestra un mensaje de error y devuelve el dinero.
- En caso de error mecánico, la máquina entra en un estado de mantenimiento.  
    Cada uno de estos estados modifica la manera en que la máquina responde a las acciones del usuario, por lo que debe manejarse de forma estructurada.

---

### **Enunciado 9:**

Una biblioteca de algoritmos de ordenamiento debe permitir que los usuarios elijan, en tiempo de ejecución, entre distintos métodos como QuickSort, MergeSort o BubbleSort.  
El sistema debe ofrecer una interfaz común para todos los algoritmos y permitir que el usuario seleccione el más adecuado según el tamaño y características de la colección de datos. Además, deben poder añadirse nuevos algoritmos de ordenamiento en el futuro sin afectar la implementación existente.