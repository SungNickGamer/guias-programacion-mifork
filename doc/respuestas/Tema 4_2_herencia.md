<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

***

# ✅ **1. Herencia y relación “A es‑un B”**

### Respuesta

La herencia en orientación a objetos permite que una clase derive de otra, heredando tanto su estado como su comportamiento. La relación “A es‑un B” expresa que una subclase puede ser tratada como una instancia de su superclase, lo que proporciona compatibilidad de tipos. Esto posibilita almacenar objetos de clases distintas, pero relacionadas jerárquicamente, dentro de una misma estructura, siempre que compartan la superclase. La compatibilidad de tipos es fundamental para lograr polimorfismo y extensibilidad.

Además de la compatibilidad, la herencia permite reutilizar código mediante la transmisión automática de atributos y métodos. La subclase adquiere las características comunes de la superclase y puede añadir otras específicas. Así se evita duplicar código y se facilita la extensión del comportamiento. En el ejemplo propuesto, los soldados comparten el atributo nombre y el método saludar, y cada subtipo añade sus propias capacidades específicas.

```java
class Soldado {
    private String nombre;
    public Soldado(String nombre){ this.nombre = nombre; }
    public void saludar(){ System.out.println("Soy " + nombre); }
}

class Artillero extends Soldado {
    private int cohetes;
    public Artillero(String n, int c){ super(n); this.cohetes = c; }
    public int getCohetes(){ return cohetes; }
}

class Zapador extends Soldado {
    private int minas;
    public Zapador(String n, int m){ super(n); this.minas = m; }
    public int getMinas(){ return minas; }
}

public class Main {
    public static void main(String[] args){
        Soldado[] lista = {
            new Artillero("Ana", 5),
            new Zapador("Luis", 2),
            new Soldado("Eva")
        };
        for(Soldado s : lista) s.saludar();
    }
}
```

***

# ✅ **2. Constructores, orden y uso de `super`**

### Respuesta

En Java, al crear un objeto de una subclase, se ejecutan primero los constructores de la superclase y luego los de la subclase. Este proceso garantiza que la parte heredada esté inicializada antes de inicializar la parte específica del subtipo. El orden es siempre de arriba hacia abajo, recorriendo la jerarquía completa hasta llegar a `Object`. Esta secuencia evita estados incompletos y asegura la consistencia interna del objeto recién creado.

Dentro del constructor de una subclase, `super` se utiliza para invocar explícitamente al constructor de la clase base. Si la superclase no tiene constructor sin parámetros accesible, entonces es obligatorio llamar a `super(...)` proporcionando los argumentos necesarios. En ausencia de dicha llamada explícita, Java insertaría una llamada implícita a `super()`, que produciría un error si ese constructor no existe o no es visible.

***

# ✅ **3. Atributos privados de la superclase en la memoria de la subclase**

### Respuesta

Los atributos privados de la superclase forman parte de cada objeto de la subclase almacenado en memoria. La instancia de una subclase contiene, físicamente, tanto sus propios atributos como los heredados, independientemente de su visibilidad. Esto significa que el estado completo de la superclase está presente dentro del objeto del subtipo, garantizando la coherencia estructural de la herencia.

Sin embargo, el hecho de que esos atributos existan dentro de la instancia no implica que puedan usarse directamente desde el código de la subclase. La visibilidad privada impide su acceso directo, preservando la encapsulación. En el caso del ejemplo, aunque un `Artillero` contiene internamente el atributo `nombre` heredado de `Soldado`, no puede acceder a él directamente, debiendo utilizar los métodos públicos ofrecidos por la superclase.

***

# ✅ **4. Extensibilidad gracias a la compatibilidad de tipos**

### Respuesta

La compatibilidad de tipos permite que el código sea extensible sin necesidad de modificar las partes que usan la superclase. Esto implica que se pueden añadir nuevas subclases y seguir utilizando el mismo código que opera sobre la superclase sin realizar cambios. De esta forma, se consigue código abierto a extensión pero cerrado a modificación, siguiendo uno de los principios del diseño orientado a objetos.

Por ejemplo, si se añade un nuevo subtipo `Sanitario`, no es necesario modificar el fragmento que recorre la lista de soldados para pedirles que saluden. Como todos son compatibles con `Soldado`, basta con instanciar el nuevo tipo e incluirlo en el array. La estructura polimórfica se mantiene y el código consumidor permanece estable.

```java
class Sanitario extends Soldado {
    public Sanitario(String n){ super(n); }
}

Soldado[] lista = {
    new Artillero("Ana", 5),
    new Zapador("Luis", 2),
    new Sanitario("Marta")
};
for(Soldado s : lista) s.saludar();
```

***

# ✅ **5. Upcasting, downcasting y `instanceof`**

### Respuesta

En Java, es posible almacenar en una referencia de la superclase objetos reales de subclases gracias al upcasting, que es automático y seguro. Esto permite aprovechar el polimorfismo. Sin embargo, una referencia de la superclase no puede llamar a métodos específicos del subtipo, porque el tipo estático limita los miembros accesibles. Para acceder a los métodos propios de la subclase se requiere un downcasting, que convierte una referencia general en una referencia más específica.

El operador `instanceof` permite comprobar si un objeto apuntado por una referencia es de un tipo concreto antes de realizar un downcast. Esto previene errores de tiempo de ejecución. En un recorrido de soldados, puede verificarse si un elemento es un `Artillero` para consultar su número de cohetes.

```java
for(Soldado s : lista){
    s.saludar();
    if(s instanceof Artillero){
        Artillero a = (Artillero) s;
        System.out.println("Cohetes: " + a.getCohetes());
    }
}
```

***

# ✅ **6. Acceso protegido y ejemplo**

### Respuesta

El acceso protegido permite que los atributos y métodos sean visibles para las subclases, aunque sigan siendo ocultos para el resto del código externo. Se sitúa entre el acceso público y el privado, facilitando la reutilización de partes internas sin renunciar completamente a la encapsulación. Esto es útil en jerarquías donde las subclases necesitan emplear datos internos de la superclase.

En Java, se emplea la palabra clave `protected`. En el ejemplo, si se desea que el `Zapador` pueda usar el nombre del soldado para el método de poner minas, es necesario declarar el nombre como protegido en la clase `Soldado`.

```java
class Soldado {
    protected String nombre;
    public Soldado(String n){ nombre = n; }
}

class Zapador extends Soldado {
    public Zapador(String n){ super(n); }
    public void ponerMina(){
        System.out.println(nombre + " pone una mina");
    }
}
```

***

# ✅ **7. Clase base universal en lenguajes orientados a objetos**

### Respuesta

Algunos lenguajes orientados a objetos proporcionan una clase base común de la cual derivan todas las demás clases. Esto permite uniformidad en las operaciones básicas, como comparación, clonación o conversión a cadena. Sin embargo, no todos los lenguajes comparten este enfoque; algunos permiten clases sin una superclase explícita.

En Java, todas las clases heredan implícitamente de `Object`, lo que proporciona un conjunto común de métodos universales. Esto facilita la interoperabilidad entre bibliotecas y estructuras genéricas. Otros lenguajes, como C++, no requieren necesariamente una clase base universal, lo que da más flexibilidad a costa de una menor uniformidad.

***

# ✅ **8. Herencia múltiple y Java**

### Respuesta

La herencia múltiple consiste en permitir que una clase derive simultáneamente de varias superclases. Esta funcionalidad puede proporcionar gran flexibilidad, aunque conlleva riesgos como el conocido problema del “diamante”, donde se producen ambigüedades heredadas. Por esta razón, algunos lenguajes la restringen para evitar problemas de complejidad.

Java no permite herencia múltiple de clases para evitar estas ambigüedades. En su lugar, utiliza interfaces para ofrecer un mecanismo similar sin heredar estado, reduciendo así los conflictos. El diseño busca preservar la claridad y simplicidad del modelo jerárquico de clases.

***

# ✅ **9. Excepción personalizada con composición**

### Respuesta

Una excepción personalizada permite capturar situaciones específicas del dominio, proporcionando información adicional al error. En este caso, la excepción incluye un objeto de tipo `Usuario` para indicar claramente qué usuario provocó el fallo. Esta composición mejora la depuración y facilita el tratamiento de errores en capas superiores del sistema.

Para que la excepción sea no controlada, debe heredarse de `RuntimeException`. Además, es útil sobrecargar los constructores para permitir incluir una causa subyacente, lo que ayuda a encadenar errores y preservar su origen. El diseño sigue el patrón estándar de las excepciones en Java.

```java
class Usuario {
    private String nombre;
    public Usuario(String n){ nombre = n; }
    public String getNombre(){ return nombre; }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario u){
        super("Usuario no encontrado: " + u.getNombre());
        this.usuario = u;
    }

    public UsuarioNoEncontradoException(Usuario u, Throwable causa){
        super("Usuario no encontrado: " + u.getNombre(), causa);
        this.usuario = u;
    }

    public Usuario getUsuario(){ return usuario; }
}
```

***

# ✅ **10. Por qué no usar herencia solo para reutilizar código**

### Respuesta

Emplear herencia exclusivamente como mecanismo para reutilizar código suele generar diseños incorrectos donde las clases se relacionan jerárquicamente sin una verdadera relación conceptual “es‑un”. Esto provoca jerarquías artificiales difíciles de mantener y que pueden romperse al introducir nuevas funcionalidades. La herencia debe reflejar relaciones semánticas, no simplemente evitar duplicación.

Además, usar herencia de forma inapropiada puede propagar responsabilidades de una clase a otra sin necesidad, dificultando la encapsulación. La reutilización de código debe abordarse primero mediante composición, delegación o utilidades independientes, reservando la herencia para casos en los que exista una relación conceptual clara.

***

# ✅ **11. Por qué favorecer composición frente a herencia**

### Respuesta

La composición permite construir objetos complejos uniendo partes más simples, lo que facilita la reutilización sin comprometer la encapsulación. Este enfoque favorece la flexibilidad, ya que se pueden sustituir componentes internos sin afectar al resto de la jerarquía. La composición evita los problemas típicos del acoplamiento fuerte entre clases derivadas y clases base.

Además, la composición permite cambiar comportamientos en tiempo de ejecución mediante la sustitución de objetos internos. Esto resulta mucho más difícil con la herencia, cuyo comportamiento se determina en tiempo de compilación. Por ello, muchos principios de diseño recomiendan priorizar composición y usar herencia solo cuando existe una verdadera relación jerárquica.

***

# ✅ **12. Herencia y ruptura de encapsulación**

### Respuesta

La herencia rompe la encapsulación cuando una subclase obtiene acceso directo a partes internas de la superclase, incluso si no deberían ser visibles. El problema se acentúa cuando la superclase cambia su implementación interna, lo que puede romper el funcionamiento de las subclases dependientes. Esto genera un acoplamiento fuerte entre niveles de la jerarquía.

Incluso cuando las variables son privadas, los métodos protegidos pueden exponer detalles internos no diseñados para un uso genérico. Las subclases pueden confiar en comportamientos que la superclase no garantiza formalmente, comprometiendo así la estabilidad del sistema. Por esta razón, se prefiere la composición, que mantiene los límites más claros entre objetos.

***

# ✅ **13. Estudiante y Trabajador: herencia vs composición**

### Respuesta

Al modelar entidades con datos comunes, la herencia ofrece una solución en la que ambos tipos derivan de una superclase que contiene la información compartida. El enfoque es adecuado cuando existe una relación conceptual clara entre la superclase y sus derivados. En el ejemplo, `Persona` puede incluir el DNI y el nombre, que serán heredados por `Estudiante` y `Trabajador`. Este diseño aprovecha la capacidad de extender comportamientos comunes de forma coherente.

Por otro lado, la composición propone encapsular los datos compartidos en una clase independiente, `DatosPersonales`, que se incluye dentro de `Estudiante` y `Trabajador`. Este enfoque evita el acoplamiento jerárquico y permite utilizar los mismos datos en contextos distintos sin imponer una relación de “es‑un”. La composición resulta más flexible cuando no hay una jerarquía natural en los conceptos del dominio.

### Versión con herencia

```java
class Persona {
    private String dni;
    private String nombre;
    public Persona(String d, String n){ dni = d; nombre = n; }
}

class Estudiante extends Persona {
    public Estudiante(String d, String n){ super(d, n); }
}

class Trabajador extends Persona {
    public Trabajador(String d, String n){ super(d, n); }
}
```

### Versión con composición

```java
class DatosPersonales {
    private String dni;
    private String nombre;
    public DatosPersonales(String d, String n){ dni = d; nombre = n; }
}

class Estudiante {
    private DatosPersonales datos;
    public Estudiante(DatosPersonales dp){ datos = dp; }
}

class Trabajador {
    private DatosPersonales datos;
    public Trabajador(DatosPersonales dp){ datos = dp; }
}
```

***