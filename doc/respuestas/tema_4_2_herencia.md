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
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia es un mecanismo fundamental en orientación a objetos que permite definir una nueva clase a partir de una clase existente, estableciendo una relación "A es-un B" donde la subclase (A) es un caso particular o especialización de la superclase (B). Por ejemplo, un `Artillero` es un `Soldado`, y un `Zapador` también es un `Soldado`. Esta relación implica que todo lo que puede hacer un soldado genérico también lo puede hacer el artillero y el zapador, aunque estos últimos añadan comportamientos o características adicionales específicas.

La herencia tiene dos implicaciones principales. La primera es la **compatibilidad de tipos** (polimorfismo), que permite tratar a un objeto de una subclase como si fuera de su superclase. Esto significa que una variable declarada del tipo `Soldado` puede referenciar tanto a objetos `Artillero` como `Zapador`, lo que facilita la creación de código genérico y reutilizable. La segunda implicación es la **herencia de estado y comportamiento**, donde la subclase automáticamente posee todos los atributos y métodos de la superclase que no sean privados, pudiendo añadir nuevos o modificar los existentes (sobrescritura). Así se evita duplicar código y se favorece la reutilización.

Composicion: "tiene un/ tiene varios" vs Herencia: "es un"

A continuación se muestra un ejemplo sencillo en Java que ilustra estos conceptos:

```java
// Superclase
public class Soldado {
    private String nombre;  // Atributo privado
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
    
    protected String getNombre() {  // Getter para que las subclases puedan acceder
        return nombre;
    }
}

// Subclase Artillero
public class Artillero extends Soldado {
    private int numCohetes;
    
    public Artillero(String nombre, int numCohetes) {
        super(nombre);  // Llama al constructor de Soldado
        this.numCohetes = numCohetes;
    }
    
    public int getNumCohetes() {
        return this.numCohetes;
    }
}

// Subclase Zapador
public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public int getNumMinas() {
        return numMinas;
    }
}

// Demostración de compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        // Array de tipo Soldado que contiene objetos de distintos subtipos
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Ana", 8);
        
        // Recorrido polimórfico: todos saludan, aunque sean de distintos tipos
        for (Soldado s : ejercito) {
            s.saludar();  // Compatibilidad de tipos en acción
        }
    }
}
```

La salida del programa mostraría:
```
Soy el soldado Juan
Soy el soldado Pedro
Soy el soldado Ana
```

Como se observa, el array declarado como `Soldado[]` puede contener objetos de cualquier subclase de `Soldado`. Al recorrerlo y llamar al método `saludar()`, cada objeto responde según su implementación heredada, demostrando la compatibilidad de tipos. Los métodos específicos como `getNumCohetes()` o `getNumMinas()` no son accesibles desde la variable de tipo `Soldado`, pero sí lo serían si se realizara un casting explícito al tipo concreto, manteniendo así la seguridad de tipos que proporciona la herencia.


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al crear un objeto de una subclase (por ejemplo, `new Artillero("Juan", 10)`), se ejecutan **dos constructores** en un orden específico: primero el constructor de la superclase (`Soldado`) y después el constructor de la subclase (`Artillero`). Esto ocurre porque la subclase hereda el estado de la superclase, y dicho estado debe inicializarse antes de que la subclase pueda añadir o modificar sus propios atributos. Java garantiza esta secuencia de ejecución para asegurar que los atributos heredados estén correctamente inicializados antes de que el constructor de la subclase comience a ejecutar su propio código.

La palabra clave `super` dentro de un constructor se utiliza para invocar explícitamente al constructor de la superclase. Cuando se escribe `super(nombre)`, se está llamando al constructor de `Soldado` que recibe un parámetro `String`. Si no se escribe ninguna llamada a `super`, el compilador inserta automáticamente una llamada al constructor sin parámetros de la superclase (`super()`). Sin embargo, si la superclase no tiene un constructor sin parámetros accesible (ya sea porque no existe o porque es privado), entonces es **obligatorio** llamar explícitamente a `super` con los argumentos adecuados. En el ejemplo, `Soldado` no define un constructor sin parámetros, solo uno con `String`, por lo que la primera línea de los constructores de `Artillero` y `Zapador` debe ser `super(nombre)`; de lo contrario, el compilador mostraría un error. Esta llamada a `super` debe ser la primera instrucción dentro del constructor de la subclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Cuando se crea un objeto `Artillero`, este contiene dentro de sí todos los atributos definidos en `Soldado` (como el `nombre`), además de los suyos propios (como `numCohetes`). Esto ocurre porque la subclase hereda el estado de la superclase, aunque dicho estado esté declarado como privado. Visualmente, la memoria del objeto `Artillero` incluye una región que almacena el `nombre`, aunque este atributo no sea directamente accesible desde el código de `Artillero`. La privacidad afecta únicamente a la visibilidad, no a la existencia física del atributo en el objeto.

Sin embargo, que el atributo privado esté presente en memoria **no implica** que pueda ser utilizado directamente desde el código de la subclase. En el ejemplo, `Artillero` no puede acceder a `nombre` mediante `this.nombre` porque está declarado como `private` en `Soldado`. Para que la subclase pueda manipular ese valor heredado, la superclase debe proporcionar métodos públicos o protegidos (getters/setters) que permitan acceder de forma controlada. En el código mostrado, `Soldado` define un método `protected String getNombre()` que devuelve el nombre, y es llamado implícitamente en el constructor de `Artillero` mediante `super(nombre)`. Esta combinación entre herencia (el atributo existe en memoria) y encapsulación (no se accede directamente) es una característica clave del diseño orientado a objetos: la subclase se beneficia del estado heredado sin violar la privacidad de la superclase, manteniendo así la integridad del objeto.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos que proporciona la herencia implica una **alta extensibilidad** del código, permitiendo añadir nuevos subtipos sin modificar el código existente que opera sobre la superclase. Esto es posible porque el código que trabaja con referencias de tipo `Soldado` puede manejar cualquier objeto de cualquier subclase futura sin necesidad de cambios, siempre que dicha subclase respete el contrato definido por la superclase. Esta propiedad, conocida como **principio de abierto/cerrado** (abierto a la extensión, cerrado a la modificación), es uno de los beneficios fundamentales de la orientación a objetos, ya que facilita el crecimiento y mantenimiento del software.

A continuación se añade un nuevo tipo de `Soldado` (el `Francotirador`) y se demuestra que el código que saluda a todos los soldados del array no necesita ninguna modificación:

```java
// Nuevo subtipo añadido sin modificar el código existente
public class Francotirador extends Soldado {
    private int alcance;
    
    public Francotirador(String nombre, int alcance) {
        super(nombre);
        this.alcance = alcance;
    }
    
    public int getAlcance() {
        return alcance;
    }
}

// El código de demostración sigue siendo exactamente el mismo
public class Main {
    public static void main(String[] args) {
        // Se añade el nuevo tipo al array sin cambiar nada más
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Ana", 8);
        ejercito[3] = new Francotirador("Carlos", 500);  // Nuevo tipo añadido
        
        // El código para saludar NO se modifica
        for (Soldado s : ejercito) {
            s.saludar();  // Sigue funcionando con el nuevo tipo
        }
    }
}
```

La salida del programa ahora mostraría:
```
Soy el soldado Juan
Soy el soldado Pedro
Soy el soldado Ana
Soy el soldado Carlos
```

Como se observa, el bucle que recorre el array y llama a `saludar()` funciona perfectamente con el nuevo `Francotirador` sin haber sido modificado. Esto es posible porque el compilador verifica que `Francotirador` es un subtipo de `Soldado` (cumple la relación "es-un") y, por tanto, es compatible a nivel de tipos. Esta capacidad de extender el sistema añadiendo nuevas clases sin alterar el código existente que depende de la superclase es lo que hace que la herencia y el polimorfismo sean herramientas tan poderosas para construir sistemas flexibles y mantenibles. Cualquier código futuro que opere sobre objetos de tipo `Soldado` (como colecciones, métodos de búsqueda, algoritmos de ordenación, etc.) podrá manejar automáticamente todos los subtipos que se creen posteriormente.


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, en Java una referencia del supertipo puede apuntar a objetos reales de un subtipo, lo que se conoce como **upcasting** implícito. Por ejemplo, `Soldado s = new Artillero("Juan", 10);` es perfectamente válido porque un `Artillero` es un `Soldado`. Sin embargo, con una referencia del supertipo solo se pueden invocar aquellos métodos que estén definidos en la superclase (como `saludar()`), pero no los métodos específicos del subtipo (como `getNumCohetes()`). Esto es así porque el compilador solo garantiza que la referencia conoce los métodos de `Soldado`, aunque en tiempo de ejecución el objeto real sea un `Artillero`. Para acceder a los métodos específicos del subtipo es necesario realizar un **downcasting** explícito.

El **upcasting** es la conversión implícita de un subtipo a su supertipo, que siempre es segura y no requiere sintaxis especial. El **downcasting** es la conversión explícita de un supertipo a un subtipo, que requiere una sintaxis como `(Artillero) referencia` y puede lanzar una `ClassCastException` si la referencia no apunta realmente a un objeto del subtipo indicado. Para evitar errores, se utiliza el operador `instanceof`, que comprueba si un objeto es de un tipo determinado o de algún subtipo del mismo. Su sintaxis es `objeto instanceof Clase` y devuelve `true` si el objeto real es una instancia de esa clase o de sus subclases.

A continuación se muestra un ejemplo que recorre un array de `Soldado`, verifica con `instanceof` cuáles son `Artillero`, realiza downcasting y accede al método específico `getNumCohetes()`:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 10); //upcasting
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Ana", 8);
        ejercito[3] = new Francotirador("Carlos", 500);
        
        for (Soldado s : ejercito) {
            s.saludar();  // Método común a todos
            
            // Comprobar si el objeto real es un Artillero
            if (s instanceof Artillero) {
                // Downcasting seguro
                Artillero a = (Artillero) s;
                System.out.println("   Tiene " + a.getNumCohetes() + " cohetes");
            }
        }
    }
}
```

La salida sería:
```
Soy el soldado Juan
   Tiene 10 cohetes
Soy el soldado Pedro
Soy el soldado Ana
   Tiene 8 cohetes
Soy el soldado Carlos
```

Como se observa, `instanceof` evita errores de conversión, ya que solo se realiza downcasting cuando se está seguro de que el objeto real es del tipo esperado. Este patrón es muy útil cuando se necesita tratar de forma específica a ciertos subtipos dentro de una colección heterogénea de objetos del supertipo, combinando la flexibilidad del polimorfismo con la capacidad de acceder a comportamientos especializados cuando sea necesario.


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **"protegido"** en el contexto de la herencia es un nivel de visibilidad intermedio entre `private` y `public`, diseñado específicamente para facilitar la reutilización mediante herencia mientras se mantiene cierto grado de encapsulación. Un miembro (atributo o método) declarado como protegido es accesible desde cualquier clase que se encuentre en el mismo paquete y, lo que es más relevante, desde **todas las subclases**, incluso si estas se encuentran en paquetes diferentes. Esto permite que las subclases accedan y modifiquen el estado heredado de la superclase sin necesidad de métodos públicos (getters/setters), pero sin exponer dicho estado al resto del código que no forme parte de la jerarquía de herencia.

En Java, el acceso protegido se implementa mediante la palabra clave `protected`. Cuando un atributo o método se declara con este modificador, las subclases pueden utilizarlo directamente como si fuera propio, mientras que las clases no relacionadas (que no heredan) no pueden acceder a él. Esto resuelve el problema de tener que hacer públicos ciertos métodos o atributos solo para que las subclases los utilicen, lo que rompería la encapsulación frente a clases externas no relacionadas. Es importante destacar que el acceso protegido sigue siendo más restrictivo que el público, pero menos que el privado, logrando un equilibrio entre encapsulación y extensibilidad.

A continuación se muestra un ejemplo modificado donde el atributo `nombre` de `Soldado` se declara como `protected` en lugar de `private`, permitiendo que `Zapador` lo utilice directamente en su método específico:

```java
// Superclase con atributo protegido
public class Soldado {
    protected String nombre;  // Accesible para subclases
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

// Subclase que accede directamente al atributo protegido
public class Zapador extends Soldado {
    private int numMinas;
    
    public Zapador(String nombre, int numMinas) {
        super(nombre);
        this.numMinas = numMinas;
    }
    
    public void ponerMinas() {
        // Acceso directo al atributo protegido de la superclase
        System.out.println("El zapador " + nombre + " está poniendo " + numMinas + " minas");
    }
    
    public int getNumMinas() {
        return numMinas;
    }
}

// Demostración del acceso protegido
public class Main {
    public static void main(String[] args) {
        Zapador zapador = new Zapador("Pedro", 5);
        zapador.saludar();    // Método heredado
        zapador.ponerMinas(); // Usa el atributo nombre directamente
        
        // Desde fuera de la jerarquía NO se puede acceder a nombre
        // System.out.println(zapador.nombre); // Error de compilación
    }
}
```

La salida sería:
```
Soy el soldado Pedro
El zapador Pedro está poniendo 5 minas
```

Como se observa, el método `ponerMinas()` de `Zapador` puede acceder directamente al atributo `nombre` porque está declarado como `protected` en `Soldado`. Esto evita tener que llamar a un getter público (como `getNombre()`) y mantiene el código más limpio. Sin embargo, desde el método `main` (que no forma parte de la jerarquía de herencia), intentar acceder a `zapador.nombre` produciría un error de compilación, demostrando que la encapsulación se mantiene frente a clases externas no relacionadas. Esta decisión de diseño debe tomarse con cuidado: usar `protected` es apropiado cuando se sabe que las subclases necesitarán acceso directo al estado heredado, pero también puede acoplar más estrechamente la jerarquía de herencia.


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

En los lenguajes orientados a objetos no existe una regla universal que establezca la presencia de una clase base única para todos los objetos; esta característica varía según el diseño específico de cada lenguaje. Algunos lenguajes, como C++, no tienen una clase raíz común, lo que significa que las jerarquías de herencia son independientes y no existe un tipo que unifique a todos los objetos. Otros lenguajes, como Smalltalk, fueron pioneros en incluir una clase base universal (normalmente llamada `Object`) de la cual descienden todas las demás, proporcionando así un conjunto mínimo de operaciones comunes a cualquier objeto. Esta decisión de diseño influye en aspectos como la implementación de colecciones polimórficas, la reflexión o la capacidad de realizar operaciones genéricas sobre cualquier objeto.

En Java, existe una **clase base única para todos los objetos**: la clase `java.lang.Object`. Cualquier clase que se escriba en Java, ya sea definida explícitamente por el programador o implícitamente por el compilador, hereda directa o indirectamente de `Object`. Si una clase no extiende explícitamente a ninguna otra, el compilador la hace extender automáticamente de `Object`. Esto significa que todas las instancias de cualquier clase en Java son compatibles con el tipo `Object`, lo que permite escribir código polimórfico de propósito general, como colecciones que almacenan `Object` y, por tanto, cualquier tipo de objeto. La clase `Object` define métodos fundamentales como `equals()`, `hashCode()`, `toString()` y `getClass()`, que están disponibles en cualquier objeto y pueden ser sobrescritos por las subclases para proporcionar comportamientos específicos. Por ejemplo, aunque `Soldado` no se declare explícitamente como subclase de `Object`, lo es automáticamente, y por tanto puede ser tratado como `Object` en cualquier contexto que lo requiera, como al almacenarlo en un `ArrayList<Object>`.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** es un mecanismo presente en algunos lenguajes orientados a objetos mediante el cual una clase puede heredar directamente de **dos o más clases base**. Esto permite que una subclase combine el estado y el comportamiento de múltiples superclases, lo que puede ser útil para modelar conceptos que realmente son una combinación de varias categorías. Por ejemplo, un `Anfibio` podría heredar tanto de `AnimalTerrestre` como de `AnimalAcuatico`. Sin embargo, la herencia múltiple introduce problemas complejos, como el **"problema del diamante"** (cuando dos superclases heredan de una misma clase base y la subclase no sabe cuál de las dos versiones de un método heredado debe utilizar), además de aumentar la complejidad del compilador y la posibilidad de ambigüedades en el código.

**Java no permite la herencia múltiple de clases**. Es decir, una clase en Java solo puede extender a una única superclase, usando la palabra clave `extends`. Esta decisión de diseño fue tomada por los creadores del lenguaje precisamente para evitar los problemas de ambigüedad y complejidad asociados con la herencia múltiple, simplificando el modelo de objetos y haciéndolo más seguro y predecible. No obstante, Java ofrece un mecanismo alternativo que permite alcanzar muchos de los beneficios de la herencia múltiple sin sus inconvenientes: las **interfaces**. Una clase en Java puede implementar múltiples interfaces (usando `implements`), heredando así contratos de comportamiento de varias fuentes. Las interfaces no contienen estado (atributos) y, hasta Java 8, solo podían tener métodos abstractos; a partir de Java 8 pueden tener métodos `default` con implementación, lo que acerca aún más la funcionalidad a la herencia múltiple, pero sin los conflictos de estado. En el ejemplo de los soldados, si se quisiera que un `Artillero` fuera también `Conducible` (porque maneja un vehículo), se podría definir una interfaz `Conducible` y que `Artillero` la implemente, manteniendo así la herencia simple de `Soldado` mientras se adquieren múltiples comportamientos adicionales.


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

Las excepciones en los lenguajes orientados a objetos son objetos que encapsulan información sobre una situación anómala ocurrida durante la ejecución de un programa. En Java, al igual que cualquier otra clase, se pueden crear excepciones personalizadas extendiendo de `Exception` (excepciones controladas) o de `RuntimeException` (excepciones no controladas). Las excepciones no controladas no obligan a quien llama al método a declararlas o capturarlas explícitamente, lo que las hace adecuadas para errores que podrían ocurrir en cualquier lugar y de los que quizás no se pueda recuperar fácilmente, como errores de lógica o violaciones de precondiciones.

A continuación se muestra la implementación de una excepción personalizada no controlada `UsuarioNoEncontradoException`, que compone un objeto `Usuario` para proporcionar información detallada sobre qué usuario causó el problema, y que permite también incluir la causa subyacente mediante sobrecarga de constructores:

```java
// Clase Usuario que se compone en la excepción
public class Usuario {
    private String id;
    private String nombre;
    
    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }
    
    public String getId() {
        return id;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    @Override
    public String toString() {
        return "Usuario{id='" + id + "', nombre='" + nombre + "'}";
    }
}

// Excepción personalizada no controlada (hereda de RuntimeException)
public class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;  // Composición: el usuario que causó el problema
    
    // Constructor sin causa
    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario);
        this.usuario = usuario;
    }
    
    // Constructor con causa (permite encadenar excepciones)
    public UsuarioNoEncontradoException(Usuario usuario, Throwable cause) {
        super("Usuario no encontrado: " + usuario, cause);
        this.usuario = usuario;
    }
    
    // Getter para acceder al usuario que causó la excepción
    public Usuario getUsuario() {
        return usuario;
    }
}

// Ejemplo de uso
public class ServicioUsuarios {
    private List<Usuario> usuarios = new ArrayList<>();
    
    public Usuario buscarPorId(String id) {
        for (Usuario u : usuarios) {
            if (u.getId().equals(id)) {
                return u;
            }
        }
        Usuario buscado = new Usuario(id, "desconocido");
        throw new UsuarioNoEncontradoException(buscado);
    }
    
    public void eliminarUsuario(Usuario usuario) {
        if (!usuarios.contains(usuario)) {
            // Lanzar con causa si se quiere encadenar
            throw new UsuarioNoEncontradoException(usuario, 
                new IllegalStateException("Usuario no estaba en la lista"));
        }
        usuarios.remove(usuario);
    }
}
```

Como se observa, la excepción `UsuarioNoEncontradoException` compone un objeto `Usuario`, permitiendo que el código que captura la excepción pueda acceder mediante `getUsuario()` a la información de qué usuario no fue encontrado. Esto es más informativo que simplemente pasar un mensaje de texto, ya que proporciona un objeto estructurado con todos los datos relevantes. Además, la sobrecarga del constructor con `Throwable cause` permite encadenar excepciones, manteniendo la traza completa del error cuando una excepción es provocada por otra (por ejemplo, si el fallo se origina por un problema de base de datos subyacente). Al heredar de `RuntimeException`, es una excepción no controlada, por lo que los métodos que lanzan esta excepción no están obligados a declararla con `throws`, aunque pueden hacerlo si se desea documentar explícitamente.


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

La herencia no debe emplearse como primera opción para la reutilización de código porque establece un **acoplamiento muy fuerte** entre la superclase y la subclase, mucho más rígido que el que se obtiene mediante composición. Cuando una clase hereda de otra, la subclase queda permanentemente ligada a la implementación interna de la superclase, incluso a sus detalles privados a través de métodos protegidos. Esto significa que cualquier cambio en la superclase (como modificar un método que la subclase sobrescribe o añadir un nuevo método que coincide con uno de la subclase) puede romper el comportamiento de la subclase de manera inesperada, un problema conocido como "fragilidad de la clase base". Además, la herencia viola el encapsulamiento al exponer parte de la implementación de la superclase a las subclases, lo que hace que el código sea más difícil de mantener y evolucionar.

La composición, por el contrario, consiste en que una clase contenga instancias de otras clases como atributos, delegando en ellas parte de su funcionalidad. Este enfoque produce un **acoplamiento más débil** porque la clase solo depende de la interfaz pública de las clases que compone, no de su implementación interna. Si una de las clases compuestas cambia, mientras su interfaz pública se mantenga, la clase que la compone no se ve afectada. Además, la composición ofrece mayor flexibilidad en tiempo de ejecución: se pueden cambiar dinámicamente los objetos compuestos, cosa que no es posible con la herencia que se define estáticamente en tiempo de compilación. Por estas razones, el principio de diseño conocido como "Preferir composición sobre herencia" (Favor composition over inheritance) es ampliamente aceptado en ingeniería de software. La herencia debería reservarse para situaciones donde realmente exista una relación "es-un" genuina y estable en el dominio del problema, no solo como un mecanismo para aprovechar código ya escrito. Por ejemplo, un `Artillero` realmente es un `Soldado`, por lo que la herencia es apropiada; pero si solo se quisiera reutilizar el código de `saludar()` en una clase `Robot`, sería más adecuado componer un objeto `Saludador` dentro de `Robot` en lugar de hacer que `Robot` herede de `Soldado`, ya que un robot no es un soldado.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

El principio de "favorecer la composición frente a la herencia" se basa en que la composición produce un diseño más flexible, mantenible y menos propenso a errores que la herencia cuando el objetivo principal es la reutilización de código. La herencia crea un acoplamiento muy fuerte entre la superclase y la subclase, ya que la subclase queda expuesta a los detalles internos de la superclase, incluyendo sus métodos protegidos y su funcionamiento interno. Cualquier cambio en la superclase (como modificar un método, añadir un nuevo método que coincida con uno privado de la subclase, o cambiar el comportamiento de un método heredado) puede romper silenciosamente a todas las subclases que dependen de ella, un problema conocido como "fragilidad de la clase base". Además, la herencia se define estáticamente en tiempo de compilación, lo que impide cambiar el comportamiento heredado durante la ejecución del programa.

La composición, en cambio, consiste en que una clase contenga referencias a objetos de otras clases y delegue en ellos parte de su funcionalidad. Esto produce un acoplamiento mucho más débil porque la clase solo depende de la interfaz pública de las clases que compone, no de su implementación interna. Si la clase compuesta cambia internamente pero mantiene su interfaz, la clase contenedora no se ve afectada. Además, la composición permite una mayor flexibilidad dinámica: se pueden cambiar los objetos compuestos en tiempo de ejecución (por ejemplo, un `Coche` puede cambiar su comportamiento de `Motor` asignando una instancia diferente), cosa que es imposible con la herencia. La composición también respeta mejor el principio de encapsulamiento, ya que la clase contenedora decide qué partes de sus componentes exponer, mientras que la herencia expone toda la interfaz pública de la superclase, incluso aquellos métodos que no deberían ser accesibles.

Por estas razones, la herencia debería reservarse únicamente para situaciones donde exista una auténtica relación "es-un" en el dominio del problema, no como un mecanismo genérico de reutilización de código. Por ejemplo, un `Artillero` realmente es un `Soldado`, por lo que la herencia es conceptualmente correcta. Sin embargo, si se quisiera reutilizar el código de un método `calcularDistancia()` entre una `Torreta` y un `Enemigo`, sería mejor crear una clase `CalculadoraDistancia` independiente y que tanto `Torreta` como `Enemigo` la compongan, en lugar de hacer que hereden de una hipotética clase `ConCapacidadDeCalcularDistancias`. La composición permite construir sistemas a partir de piezas pequeñas e independientes que se ensamblan dinámicamente, mientras que la herencia crea jerarquías rígidas que tienden a volverse frágiles con el tiempo. Esta es la razón por la que muchos patrones de diseño (como Strategy, Decorator o Composite) prefieren la composición para lograr una mayor flexibilidad.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

La afirmación de que "la herencia rompe la encapsulación" se refiere a que una subclase puede depender de los detalles internos de implementación de su superclase, no solo de su interfaz pública. Cuando una clase hereda de otra, la subclase tiene acceso a los miembros `protected` de la superclase y, en muchos casos, también puede sobrescribir métodos públicos o protegidos. Esto significa que el comportamiento de la subclase queda ligado a cómo está implementada internamente la superclase, no solo a lo que promete su interfaz pública. Si la superclase cambia su implementación interna (por ejemplo, modificando un método `protected` que la subclase utiliza o sobrescribe), la subclase puede dejar de funcionar correctamente sin que haya cambiado su propio código ni la interfaz pública de la superclase. Esta violación del encapsulamiento es inherente a la herencia porque expone partes del interior de la superclase que normalmente estarían ocultas para otras clases.

En una relación de composición, en cambio, el encapsulamiento se mantiene intacto. Cuando una clase compone otra, la clase contenedora solo conoce la interfaz pública de la clase contenida (sus métodos públicos), pero no tiene acceso a sus atributos privados ni a sus métodos internos. La clase contenida puede cambiar completamente su implementación interna sin afectar a la clase que la compone, siempre que su interfaz pública permanezca estable. Además, la clase contenedora decide explícitamente qué métodos de la clase contenida quiere delegar, pudiendo ocultar aquellos que no le interesen. Por el contrario, la herencia expone automáticamente todos los métodos públicos y protegidos de la superclase en la subclase, aunque algunos de ellos puedan no tener sentido en el contexto de la subclase o puedan ser peligrosos si se utilizan incorrectamente. Por ejemplo, si `Soldado` tiene un método `recibirDaño(int cantidad)` público, cualquier subclase como `Artillero` automáticamente hereda ese método, aunque tal vez no debería ser modificable externamente. Con composición, se podría tener un atributo `salud` dentro de `Soldado` y decidir si se expone o no un método para modificarla, manteniendo el control total sobre la interfaz que se ofrece. Esta es una de las razones principales por las que se recomienda favorecer la composición sobre la herencia: la composición respeta y preserva el encapsulamiento, mientras que la herencia tiende a erosionarlo.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

A continuación se presentan las dos alternativas para modelar la situación donde `Estudiante` y `Trabajador` comparten datos como el DNI y el nombre. La primera utiliza herencia con una superclase `Persona`, y la segunda utiliza composición con una clase `DatosPersonales`.

## Alternativa 1: Herencia (relación "es-un")

```java
// Superclase con los datos comunes
public class Persona {
    private String dni;
    private String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() {
        return dni;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    @Override
    public String toString() {
        return "DNI: " + dni + ", Nombre: " + nombre;
    }
}

// Subclase que hereda los datos comunes
public class Estudiante extends Persona {
    private String carrera;
    
    public Estudiante(String dni, String nombre, String carrera) {
        super(dni, nombre);  // Reutiliza el constructor de Persona
        this.carrera = carrera;
    }
    
    public String getCarrera() {
        return carrera;
    }
    
    @Override
    public String toString() {
        return super.toString() + ", Carrera: " + carrera;
    }
}

// Subclase que hereda los datos comunes
public class Trabajador extends Persona {
    private String empresa;
    
    public Trabajador(String dni, String nombre, String empresa) {
        super(dni, nombre);
        this.empresa = empresa;
    }
    
    public String getEmpresa() {
        return empresa;
    }
    
    @Override
    public String toString() {
        return super.toString() + ", Empresa: " + empresa;
    }
}
```

## Alternativa 2: Composición (relación "tiene-un")

```java
// Clase que encapsula los datos personales
public class DatosPersonales {
    private String dni;
    private String nombre;
    
    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    
    public String getDni() {
        return dni;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    @Override
    public String toString() {
        return "DNI: " + dni + ", Nombre: " + nombre;
    }
}

// Estudiante compone sus datos personales
public class Estudiante {
    private DatosPersonales datosPersonales;  // Composición
    private String carrera;
    
    public Estudiante(DatosPersonales datosPersonales, String carrera) {
        this.datosPersonales = datosPersonales;
        this.carrera = carrera;
    }
    
    public String getDni() {
        return datosPersonales.getDni();  // Delega en el objeto compuesto
    }
    
    public String getNombre() {
        return datosPersonales.getNombre();
    }
    
    public String getCarrera() {
        return carrera;
    }
    
    @Override
    public String toString() {
        return datosPersonales.toString() + ", Carrera: " + carrera;
    }
}

// Trabajador compone sus datos personales
public class Trabajador {
    private DatosPersonales datosPersonales;  // Composición
    private String empresa;
    
    public Trabajador(DatosPersonales datosPersonales, String empresa) {
        this.datosPersonales = datosPersonales;
        this.empresa = empresa;
    }
    
    public String getDni() {
        return datosPersonales.getDni();
    }
    
    public String getNombre() {
        return datosPersonales.getNombre();
    }
    
    public String getEmpresa() {
        return empresa;
    }
    
    @Override
    public String toString() {
        return datosPersonales.toString() + ", Empresa: " + empresa;
    }
}
```

## Comparación de ambas alternativas

| Aspecto | Herencia | Composición |
|---------|----------|-------------|
| Relación | `Estudiante` es una `Persona` | `Estudiante` tiene unos `DatosPersonales` |
| Acoplamiento | Fuerte (subclase ligada a superclase) | Débil (solo depende de la interfaz pública) |
| Reutilización | Automática (hereda todos los métodos) | Explícita (delegación manual) |
| Flexibilidad | Rígida (relación fija en compilación) | Alta (se puede cambiar el objeto compuesto) |
| Encapsulación | Se rompe parcialmente (acceso a protected) | Se preserva completamente |

## Ejemplo de uso

```java
public class Main {
    public static void main(String[] args) {
        // Con herencia
        Estudiante estudianteHerencia = new Estudiante("12345678A", "Ana", "Informática");
        Trabajador trabajadorHerencia = new Trabajador("87654321B", "Luis", "TechCorp");
        
        System.out.println("=== Herencia ===");
        System.out.println(estudianteHerencia);
        System.out.println(trabajadorHerencia);
        
        // Con composición
        DatosPersonales dp1 = new DatosPersonales("12345678A", "Ana");
        DatosPersonales dp2 = new DatosPersonales("87654321B", "Luis");
        
        Estudiante estudianteComposicion = new Estudiante(dp1, "Informática");
        Trabajador trabajadorComposicion = new Trabajador(dp2, "TechCorp");
        
        System.out.println("\n=== Composición ===");
        System.out.println(estudianteComposicion);
        System.out.println(trabajadorComposicion);
        
        // Ventaja de composición: se puede reutilizar el mismo DatosPersonales
        DatosPersonales dpCompartidos = new DatosPersonales("11111111C", "Carlos");
        Estudiante e1 = new Estudiante(dpCompartidos, "Matemáticas");
        Trabajador t1 = new Trabajador(dpCompartidos, "OpenAI");
        
        System.out.println("\n=== Mismo DatosPersonales compartido ===");
        System.out.println(e1);
        System.out.println(t1);
    }
}
```

Como se observa, la composición ofrece mayor flexibilidad (por ejemplo, permite compartir la misma instancia de `DatosPersonales` entre un estudiante y un trabajador que son la misma persona), mantiene un mejor encapsulamiento y produce un acoplamiento más débil. La herencia, por su parte, es más directa y requiere menos código repetitivo (no hay que escribir métodos delegados), pero crea una dependencia más rígida entre las clases.
