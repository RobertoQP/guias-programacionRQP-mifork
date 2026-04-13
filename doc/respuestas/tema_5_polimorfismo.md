<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es un principio fundamental de la programación orientada a objetos que permite que objetos de diferentes clases respondan de manera específica a un mismo mensaje o método. Literalmente significa "muchas formas" y se basa en la capacidad de que una variable de un tipo superclase pueda referenciar objetos de cualquiera de sus subclases, y al invocar un método definido en la superclase, se ejecutará la versión correspondiente al tipo real del objeto, no al tipo declarado de la variable. Esto sirve para escribir código más genérico y reutilizable, ya que se pueden tratar objetos de distintos tipos de forma unificada, delegando en cada objeto la responsabilidad de implementar su comportamiento específico.

La sobreescritura de métodos (o "override") es el mecanismo que permite a una subclase proporcionar una implementación específica de un método que ya está definido en su superclase. Para que se produzca la sobreescritura, el método en la subclase debe tener el mismo nombre, el mismo tipo de retorno y los mismos parámetros que el método de la superclase. Cuando se invoca un método sobrescrito sobre un objeto de la subclase, se ejecuta la versión de la subclase, no la de la superclase. Este es el mecanismo que hace posible el polimorfismo, ya que permite que una misma acción (como "saludar" o "calcular area") se comporte de manera diferente según el tipo real del objeto. En Java, la sobreescritura se indica opcionalmente con la anotación `@Override`, que ayuda al compilador a verificar que efectivamente se está sobrescribiendo un método de la superclase.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** (o enlace tardío) es el mecanismo mediante el cual el compilador no determina qué método concreto se va a ejecutar hasta el momento de la ejecución del programa. Esto ocurre cuando se invoca un método sobrescrito a través de una referencia de un tipo superclase que apunta a un objeto de una subclase. El compilador solo verifica que el método exista en la superclase, pero en tiempo de ejecución el sistema determina cuál es la implementación real del método según el tipo dinámico del objeto. Este mecanismo es fundamental para que el polimorfismo funcione, ya que permite que una misma llamada se comporte de manera diferente dependiendo del objeto real que esté siendo referenciado.

La relación con el polimorfismo es directa: sin ligadura dinámica, no sería posible el comportamiento polimórfico. Cuando se escribe código como `Animal a = new Perro(); a.hacerSonido();`, la ligadura dinámica asegura que se ejecute `hacerSonido()` de `Perro` y no el de `Animal`. La necesidad de indicarlo explícitamente depende del lenguaje. En **C++**, los métodos deben declararse explícitamente como `virtual` para que se aplique ligadura dinámica; de lo contrario, se usa ligadura estática (en tiempo de compilación). En **Java**, todos los métodos no estáticos, no privados y no `final` son virtuales por defecto, por lo que la ligadura dinámica es automática sin necesidad de palabras clave adicionales. En **Python**, todo es dinámico por naturaleza: todos los métodos se resuelven en tiempo de ejecución, y no existe forma de obtener ligadura estática, lo que hace que el polimorfismo sea aún más flexible pero también potencialmente menos predecible. Esta diferencia refleja las distintas filosofías de los lenguajes: C++ busca eficiencia y control (el programador decide qué es virtual), Java busca equilibrio y seguridad, mientras que Python prioriza la flexibilidad y la simplicidad.


## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

A continuación se presenta un ejemplo sencillo en Java que ilustra el polimorfismo con una clase base `Soldado` y dos subclases que sobrescriben el método `saludar`:

```java
// Clase base
public class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
}

// Subclase Artillero (no sobrescribe saludar)
public class Artillero extends Soldado {
    private int cohetes;
    
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    
    // No sobrescribe saludar, usa el de Soldado
}

// Subclase Zapador (sobrescribe saludar)
public class Zapador extends Soldado {
    private int minas;
    
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    
    @Override
    public void saludar() {
        System.out.println("¡Soy el zapador " + nombre + "! Preparando minas...");
    }
}
```

**Demostración del polimorfismo:**

```java
public class Main {
    public static void main(String[] args) {
        // Array de tipo Soldado que contiene objetos de distintos subtipos
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Ana", 8);
        
        // Recorrido polimórfico: se ejecuta la versión correspondiente a cada objeto
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}
```

**Salida del programa:**

```
Soy el soldado Juan
¡Soy el zapador Pedro! Preparando minas...
Soy el soldado Ana
```

**Explicación del funcionamiento:**

El array está declarado como `Soldado[]`, pero contiene objetos de tipo `Artillero` y `Zapador`. Cuando se recorre el array y se llama a `s.saludar()`, el comportamiento es diferente según el tipo real del objeto. Para los objetos `Artillero` (que no sobrescriben `saludar`), se ejecuta el método heredado de `Soldado`, mostrando "Soy el soldado...". Para el objeto `Zapador`, que sí sobrescribe el método, se ejecuta su versión específica, mostrando el mensaje personalizado. La ligadura dinámica es la responsable de que, aunque la referencia sea de tipo `Soldado`, se ejecute el método correspondiente al objeto real. Esto demuestra cómo el polimorfismo permite tratar objetos de diferentes tipos de manera uniforme (todos son `Soldado`), mientras cada uno mantiene su comportamiento específico.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es posible invocar el método de la clase base dentro de la versión sobrescrita de la subclase. Esto se logra utilizando la palabra clave `super`, que permite acceder a miembros (métodos o atributos) de la superclase directa. Esta técnica es muy útil cuando se desea extender o modificar ligeramente el comportamiento heredado en lugar de reemplazarlo por completo, lo que favorece la reutilización de código y mantiene la coherencia con la funcionalidad base.

A continuación se muestra la modificación de la clase `Zapador` para que primero ejecute el saludo normal de `Soldado` y luego añada su mensaje específico:

```java
public class Zapador extends Soldado {
    private int minas;
    
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    
    @Override
    public void saludar() {
        super.saludar();  // Invoca el método saludar de la clase base Soldado
        System.out.println("¡ZAPADOR A SUS ÓRDENES!");
    }
}
```

**Demostración del funcionamiento:**

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        ejercito[2] = new Artillero("Ana", 8);
        
        for (Soldado s : ejercito) {
            s.saludar();
            System.out.println("---");
        }
    }
}
```

**Salida del programa:**

```
Soy el soldado Juan
---
Soy el soldado Pedro
¡ZAPADOR A SUS ÓRDENES!
---
Soy el soldado Ana
---
```

**Explicación:**

La palabra clave `super` es la que se utiliza en Java para invocar al método de la clase base. En este caso, `super.saludar()` llama al método `saludar` definido en `Soldado`, que imprime "Soy el soldado Pedro". Después de esa llamada, el método de `Zapador` continúa su ejecución y añade el mensaje específico "¡ZAPADOR A SUS ÓRDENES!". De esta forma, la subclase aprovecha y extiende el comportamiento de la superclase en lugar de reemplazarlo completamente, lo que resulta en un código más mantenible y menos redundante. Esta práctica es común cuando se quiere respetar el contrato establecido por la superclase y añadir funcionalidad adicional sin duplicar la lógica base.


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método en Java, existen restricciones importantes sobre la firma del método. Los parámetros deben ser exactamente los mismos en tipo, orden y número que en el método de la superclase; no se pueden cambiar. En cuanto al tipo de retorno, debe ser el mismo o un subtipo del tipo de retorno original (lo que se conoce como "covarianza de tipos de retorno"). Por ejemplo, si la superclase devuelve `Animal`, la subclase puede devolver `Perro` porque `Perro` es un subtipo de `Animal`. Además, el método sobrescrito no puede tener un nivel de acceso más restrictivo (no se puede hacer `public` en la superclase y `protected` en la subclase), aunque sí puede ser menos restrictivo (de `protected` a `public`). Tampoco se pueden sobrescribir métodos declarados como `final` o `static`.

La diferencia fundamental entre sobreescritura (overriding) y sobrecarga (overloading) es que la sobreescritura ocurre en la jerarquía de herencia entre clases diferentes (superclase y subclase) y redefine completamente el comportamiento de un método existente. La sobrecarga, en cambio, ocurre dentro de la misma clase (o entre clases relacionadas por herencia) y consiste en definir múltiples métodos con el mismo nombre pero con diferentes parámetros (diferente número, tipo u orden). La sobrecarga es un mecanismo de tiempo de compilación (ligadura estática), mientras que la sobreescritura es de tiempo de ejecución (ligadura dinámica). Por ejemplo, `saludar()` y `saludar(String mensaje)` están sobrecargados; mientras que `saludar()` en `Soldado` y `saludar()` en `Zapador` es sobrescritura.

La anotación `@Override` es opcional pero altamente recomendable. Sirve para indicar explícitamente al compilador que se pretende sobrescribir un método de la superclase. Su principal utilidad es detectar errores en tiempo de compilación: si se escribe mal el nombre del método, los parámetros no coinciden exactamente, o se está intentando sobrescribir un método `final` o `static`, el compilador mostrará un error. Sin `@Override`, el método sería tratado como un método nuevo y la equivocación pasaría desapercibida, causando errores lógicos difíciles de depurar. Por ejemplo, si se escribe `saludar()` en `Zapador` pero la superclase tiene `saludar()` (con 'd'), sin `@Override` se crearía un nuevo método en lugar de sobrescribir el existente. Usar siempre `@Override` es una buena práctica que mejora la legibilidad del código y previene errores sutiles.


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, cuando se sobrescribe `toString()` o `equals()` en Java, ya se está utilizando polimorfismo, aunque muchos estudiantes no sean conscientes de ello al principio. Estos métodos están definidos en la clase base `Object`, de la cual todas las clases heredan automáticamente. Al proporcionar una implementación específica en una clase propia, se está sobrescribiendo el comportamiento heredado, y cuando se utiliza una referencia de tipo `Object` (o se pasa el objeto a un método como `System.out.println()` que internamente llama a `toString()`), el sistema aplica ligadura dinámica para ejecutar la versión sobrescrita correspondiente al objeto real. Esto es polimorfismo puro: el mismo mensaje (`toString()` o `equals()`) produce comportamientos diferentes según el tipo concreto del objeto.

De hecho, este es uno de los primeros ejemplos de polimorfismo que se encuentran al aprender Java, aunque a menudo se enseña como "buena práctica sobrescribir `toString` para obtener una representación legible del objeto". Cada vez que se escribe `System.out.println(miObjeto)`, se está confiando en que el método `toString()` sobrescrito se ejecutará correctamente gracias al polimorfismo, sin necesidad de saber el tipo exacto del objeto. Lo mismo ocurre con `equals()` cuando se utilizan colecciones como `ArrayList.contains()` o `HashSet`, que dependen del comportamiento polimórfico de `equals()` para comparar objetos. Por tanto, el polimorfismo no es un concepto avanzado que se introduce tarde, sino que está presente desde los primeros programas Java, aunque su comprensión teórica y su uso explícito con jerarquías de clases propias suele abordarse más adelante en el aprendizaje.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una clase que no puede ser instanciada directamente, es decir, no se pueden crear objetos de ese tipo. Su propósito principal es servir como plantilla o base para otras clases (subclases) que heredarán de ella y completarán su funcionalidad. Una clase abstracta puede contener tanto métodos concretos (con implementación) como métodos abstractos (sin implementación). Se declara con la palabra clave `abstract` antes de `class`. Por otro lado, un **método abstracto** es un método que se declara pero no se implementa en la clase abstracta; solo se especifica su firma (nombre, parámetros y tipo de retorno), y las subclases concretas están obligadas a proporcionar una implementación (a menos que también sean abstractas). Los métodos abstractos no tienen cuerpo, terminan con punto y coma, y también se declaran con la palabra clave `abstract`.

No, no se pueden crear instancias de una clase abstracta. Intentar hacer `new Soldado()` produciría un error de compilación porque la clase está incompleta al tener métodos abstractos sin implementar. Las clases abstractas solo pueden ser utilizadas a través de sus subclases concretas, que deben implementar todos los métodos abstractos heredados. Esto garantiza que cualquier objeto creado tenga un comportamiento completo para todos los métodos definidos en la jerarquía.

A continuación se muestra el ejemplo redefiniendo `Soldado` como clase abstracta con un método abstracto `atacar`:

```java
// Clase abstracta
public abstract class Soldado {
    protected String nombre;
    
    public Soldado(String nombre) {
        this.nombre = nombre;
    }
    
    // Método concreto (ya implementado)
    public void saludar() {
        System.out.println("Soy el soldado " + nombre);
    }
    
    // Método abstracto (sin implementación)
    public abstract void atacar();
}

// Subclase concreta Artillero
public class Artillero extends Soldado {
    private int cohetes;
    
    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    
    @Override
    public void atacar() {
        System.out.println("¡" + nombre + " dispara " + cohetes + " cohetes!");
    }
}

// Subclase concreta Zapador
public class Zapador extends Soldado {
    private int minas;
    
    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    
    @Override
    public void saludar() {
        super.saludar();
        System.out.println("¡ZAPADOR A SUS ÓRDENES!");
    }
    
    @Override
    public void atacar() {
        System.out.println("¡" + nombre + " coloca " + minas + " minas explosivas!");
    }
}
```

**Demostración del polimorfismo con el método abstracto:**

```java
public class Main {
    public static void main(String[] args) {
        // Soldado s = new Soldado("Juan");  // ERROR: clase abstracta no se puede instanciar
        
        Soldado[] ejercito = new Soldado[2];
        ejercito[0] = new Artillero("Juan", 10);
        ejercito[1] = new Zapador("Pedro", 5);
        
        for (Soldado s : ejercito) {
            s.saludar();
            s.atacar();  // Cada soldado ataca según su tipo
            System.out.println("---");
        }
    }
}
```

**Salida del programa:**

```
Soy el soldado Juan
¡Juan dispara 10 cohetes!
---
Soy el soldado Pedro
¡ZAPADOR A SUS ÓRDENES!
¡Pedro coloca 5 minas explosivas!
---
```

La palabra clave `abstract` debe colocarse en dos lugares: en la declaración de la clase (`public abstract class Soldado`) y en la declaración de cada método abstracto (`public abstract void atacar();`). Las subclases concretas (`Artillero` y `Zapador`) están obligadas a implementar el método `atacar()`, de lo contrario el compilador mostraría un error. El uso de clases y métodos abstractos permite definir un contrato común (todos los soldados atacan) mientras se delega en cada subclase la implementación específica, lo que es una aplicación clara del polimorfismo y del principio de diseño "abierto/cerrado".


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave `final` en Java tiene efectos diferentes según dónde se aplique. Cuando se declara un método como `final`, significa que las subclases **no pueden sobrescribir** (override) ese método. Esto impide que el comportamiento de ese método sea modificado por herencia, garantizando que cualquier objeto de cualquier subclase se comporte exactamente igual que la superclase respecto a ese método. Cuando se declara una clase como `final`, implica que **no se puede heredar de ella**, es decir, ninguna clase puede extenderla. Una clase `final` no puede tener subclases, lo que cierra por completo la posibilidad de polimorfismo a través de herencia sobre esa clase.

La relación con el polimorfismo es directa: `final` restringe o elimina el polimorfismo. Si un método es `final`, se pierde la capacidad de que las subclases proporcionen comportamientos diferentes para ese método mediante sobrescritura. Si una clase es `final`, no se puede crear una jerarquía de herencia a partir de ella, por lo que el polimorfismo basado en subtipos no es posible. Esto se utiliza cuando se quiere asegurar que ciertos métodos críticos (por motivos de seguridad, rendimiento o corrección) no sean alterados, o cuando se diseña una clase que no debe ser extendida porque su comportamiento es completo y autosuficiente. En muchos casos, hacer un método `final` puede optimizar la ligadura dinámica, aunque en la práctica el compilador moderno ya realiza optimizaciones automáticas.

Un ejemplo muy conocido de clase `final` en la API estándar de Java es `String`. La clase `String` es `final` para garantizar su inmutabilidad y seguridad. Si cualquier clase pudiera heredar de `String`, podría modificar su comportamiento interno y romper contratos fundamentales (como el método `equals` o `hashCode`), lo que afectaría gravemente a la seguridad de los programas y al correcto funcionamiento de las colecciones que dependen de estos métodos. Otros ejemplos de clases `final` en Java son `Integer`, `Double` y todas las clases envoltorio de tipos primitivos, así como `System`, `Math` y `Scanner`. Estas clases están diseñadas para ser utilizadas tal cual, sin necesidad de extensión, y hacerlas `final` protege su integridad. En cuanto a métodos `final`, un ejemplo común es `Object.getClass()`, que es `final` para evitar que las subclases modifiquen el comportamiento de obtención de la información de tipo en tiempo de ejecución.


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

En Java, una **interfaz** es un tipo de referencia que define un contrato de comportamiento, es decir, una colección de métodos abstractos (y desde Java 8, también métodos `default` y `static`) que una clase debe implementar. Una interfaz no puede tener estado (atributos de instancia), aunque puede declarar constantes (`static final`). Su propósito principal es especificar "qué" debe hacer una clase, sin decir "cómo" hacerlo, lo que permite que clases no relacionadas entre sí puedan compartir comportamientos comunes. Por ejemplo, una interfaz `Nadador` podría ser implementada tanto por una clase `Pez` como por una clase `Deportista`, que no tienen ninguna relación de herencia, pero ambas pueden nadar.

Las interfaces no son exactamente como las clases abstractas, aunque comparten ciertas similitudes. La diferencia fundamental es que una clase abstracta puede tener estado (atributos) y métodos concretos (con implementación), mientras que una interfaz tradicionalmente no puede tener estado y sus métodos son abstractos por defecto (aunque desde Java 8 pueden tener métodos `default` con implementación). Además, una clase solo puede heredar de **una** clase (herencia simple), ya sea abstracta o concreta, pero puede implementar **múltiples** interfaces (herencia múltiple de comportamiento). Esto resuelve el problema de la herencia múltiple que Java no permite con clases. Una interfaz también puede extender a otras interfaces, formando jerarquías de interfaces. En esencia, las clases abstractas se usan para compartir código y estado entre clases relacionadas (relación "es-un"), mientras que las interfaces se usan para definir capacidades o roles que pueden ser implementados por clases no relacionadas (relación "puede-hacer").

Sí, una clase puede implementar **más de una interfaz**. La sintaxis es `class MiClase extends ClaseBase implements Interface1, Interface2, Interface3`. Por ejemplo, una clase `Soldado` podría implementar `Nadador` y `Corredor` simultáneamente, indicando que el soldado puede nadar y correr, independientemente de su jerarquía de herencia. Esto permite gran flexibilidad en el diseño. A continuación un ejemplo:

```java
public interface Nadador {
    void nadar();
}

public interface Corredor {
    void correr();
}

public abstract class Soldado {
    protected String nombre;
    public Soldado(String nombre) { this.nombre = nombre; }
    public abstract void atacar();
}

public class SoldadoEspecial extends Soldado implements Nadador, Corredor {
    public SoldadoEspecial(String nombre) { super(nombre); }
    
    @Override
    public void atacar() {
        System.out.println(nombre + " ataca con lanza");
    }
    
    @Override
    public void nadar() {
        System.out.println(nombre + " nada sigilosamente");
    }
    
    @Override
    public void correr() {
        System.out.println(nombre + " corre a toda velocidad");
    }
}
```

Esta capacidad de implementar múltiples interfaces es una de las características más potentes de Java, ya que permite diseñar sistemas flexibles donde los objetos pueden desempeñar múltiples roles sin las complicaciones de la herencia múltiple de clases.


## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

A continuación se presenta el diseño de las clases `Punto` (abstracta), `Punto2D`, `Punto3D` y `Linea`, demostrando el polimorfismo en acción:

## Clase abstracta Punto

```java
public abstract class Punto {
    // Método abstracto que cada subtipo debe implementar
    public abstract double calcularDistanciaA(Punto otro);
    
    // Método auxiliar para verificar compatibilidad de tipos
    protected void verificarCompatibilidad(Punto otro, Class<?> tipoEsperado) {
        if (!tipoEsperado.isInstance(otro)) {
            throw new IllegalArgumentException(
                "Se esperaba un " + tipoEsperado.getSimpleName() + 
                " pero se recibió un " + otro.getClass().getSimpleName()
            );
        }
    }
}
```

## Clase Punto2D

```java
public class Punto2D extends Punto {
    private double x;
    private double y;
    
    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    public double getX() { return x; }
    public double getY() { return y; }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verificar que el otro punto también es 2D
        verificarCompatibilidad(otro, Punto2D.class);
        
        Punto2D p2d = (Punto2D) otro;  // Downcasting seguro
        double dx = this.x - p2d.getX();
        double dy = this.y - p2d.getY();
        return Math.sqrt(dx * dx + dy * dy);
    }
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
}
```

## Clase Punto3D

```java
public class Punto3D extends Punto {
    private double x;
    private double y;
    private double z;
    
    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    
    public double getX() { return x; }
    public double getY() { return y; }
    public double getZ() { return z; }
    
    @Override
    public double calcularDistanciaA(Punto otro) {
        // Verificar que el otro punto también es 3D
        verificarCompatibilidad(otro, Punto3D.class);
        
        Punto3D p3d = (Punto3D) otro;  // Downcasting seguro
        double dx = this.x - p3d.getX();
        double dy = this.y - p3d.getY();
        double dz = this.z - p3d.getZ();
        return Math.sqrt(dx * dx + dy * dy + dz * dz);
    }
    
    @Override
    public String toString() {
        return "(" + x + ", " + y + ", " + z + ")";
    }
}
```

## Clase Linea (polimórfica)

```java
public class Linea {
    private Punto inicio;
    private Punto fin;
    
    public Linea(Punto inicio, Punto fin) {
        // Verificar que ambos puntos son del mismo tipo
        if (!inicio.getClass().equals(fin.getClass())) {
            throw new IllegalArgumentException(
                "Los puntos deben ser del mismo tipo (ambos 2D o ambos 3D)"
            );
        }
        this.inicio = inicio;
        this.fin = fin;
    }
    
    public double longitud() {
        // El polimorfismo hace su magia: no necesitamos saber qué tipo de punto es
        return inicio.calcularDistanciaA(fin);
    }
    
    public Punto getInicio() { return inicio; }
    public Punto getFin() { return fin; }
    
    @Override
    public String toString() {
        return "Línea desde " + inicio + " hasta " + fin + 
               " → longitud: " + longitud();
    }
}
```

## Demostración del polimorfismo

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("=== PUNTOS 2D ===");
        Punto2D p1 = new Punto2D(0, 0);
        Punto2D p2 = new Punto2D(3, 4);
        
        Linea linea2D = new Linea(p1, p2);
        System.out.println(linea2D);
        System.out.println("Distancia calculada: " + p1.calcularDistanciaA(p2));
        
        System.out.println("\n=== PUNTOS 3D ===");
        Punto3D q1 = new Punto3D(0, 0, 0);
        Punto3D q2 = new Punto3D(1, 2, 2);
        
        Linea linea3D = new Linea(q1, q2);
        System.out.println(linea3D);
        System.out.println("Distancia calculada: " + q1.calcularDistanciaA(q2));
        
        System.out.println("\n=== DEMOSTRACIÓN DE POLIMORFISMO ===");
        // Array de líneas con diferentes tipos de puntos
        Linea[] lineas = new Linea[2];
        lineas[0] = new Linea(new Punto2D(0, 0), new Punto2D(5, 12));
        lineas[1] = new Linea(new Punto3D(0, 0, 0), new Punto3D(1, 2, 2));
        
        for (Linea linea : lineas) {
            System.out.println(linea);
        }
        
        System.out.println("\n=== PRUEBA DE ERROR (tipos incompatibles) ===");
        try {
            Punto2D punto2D = new Punto2D(1, 1);
            Punto3D punto3D = new Punto3D(1, 1, 1);
            Linea lineaMixta = new Linea(punto2D, punto3D);
        } catch (IllegalArgumentException e) {
            System.out.println("Error esperado: " + e.getMessage());
        }
        
        try {
            Punto2D punto2D = new Punto2D(1, 1);
            punto2D.calcularDistanciaA(new Punto3D(1, 1, 1));
        } catch (IllegalArgumentException e) {
            System.out.println("Error esperado: " + e.getMessage());
        }
    }
}
```

**Salida del programa:**

```
=== PUNTOS 2D ===
Línea desde (0.0, 0.0) hasta (3.0, 4.0) → longitud: 5.0
Distancia calculada: 5.0

=== PUNTOS 3D ===
Línea desde (0.0, 0.0, 0.0) hasta (1.0, 2.0, 2.0) → longitud: 3.0
Distancia calculada: 3.0

=== DEMOSTRACIÓN DE POLIMORFISMO ===
Línea desde (0.0, 0.0) hasta (5.0, 12.0) → longitud: 13.0
Línea desde (0.0, 0.0, 0.0) hasta (1.0, 2.0, 2.0) → longitud: 3.0

=== PRUEBA DE ERROR (tipos incompatibles) ===
Error esperado: Los puntos deben ser del mismo tipo (ambos 2D o ambos 3D)
Error esperado: Se esperaba un Punto2D pero se recibió un Punto3D
```

**Explicación del diseño:**

1. **Clase abstracta `Punto`**: Define el contrato `calcularDistanciaA` que todos los puntos deben cumplir. Incluye un método auxiliar `verificarCompatibilidad` para validar tipos.

2. **Uso de `instanceof` (a través de `isInstance()`) y downcasting**: En cada subclase, se verifica que el otro punto sea del mismo tipo y luego se realiza downcasting para acceder a las coordenadas específicas.

3. **Polimorfismo en `Linea`**: La clase `Linea` trabaja con referencias de tipo `Punto` sin saber si son 2D o 3D. Al llamar a `calcularDistanciaA`, la ligadura dinámica ejecuta la versión correcta según el tipo real de los puntos.

4. **Seguridad**: Se verifica en el constructor de `Linea` que ambos puntos sean del mismo tipo, evitando cálculos imposibles (distancia entre un punto 2D y uno 3D).

Este diseño muestra cómo el polimorfismo permite que `Linea` sea completamente independiente de las dimensiones de los puntos, cumpliendo con el principio de abierto/cerrado: está abierta para trabajar con cualquier subtipo de `Punto`, pero cerrada para modificaciones cuando se añadan nuevos tipos (por ejemplo, `Punto4D`).


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

En Java, la **"herencia de interfaces"** es el mecanismo mediante el cual una interfaz puede extender a otra u otras interfaces, heredando así todos sus métodos abstractos. Esto se realiza con la palabra clave `extends` (al igual que en la herencia entre clases). La interfaz hija no tiene que implementar los métodos heredados (eso corresponde a la clase concreta que finalmente implemente la interfaz), simplemente los incorpora a su contrato, pudiendo además añadir nuevos métodos. Esto permite construir jerarquías de interfaces que organizan y especializan comportamientos de forma escalable, desde lo más general a lo más específico.

Sí, en Java existe la **herencia múltiple de interfaces**. Una interfaz puede extender **más de una interfaz** a la vez, separándolas por comas. Por ejemplo: `interface C extends A, B`. Esto es posible porque las interfaces solo declaran comportamientos (métodos sin implementación hasta Java 7, y desde Java 8 también pueden tener métodos `default`, pero los conflictos se resuelven con reglas específicas). La herencia múltiple de interfaces no tiene los problemas de la herencia múltiple de clases (como el "problema del diamante" con estado), porque las interfaces no tienen estado (atributos) y la ambigüedad entre métodos `default` se puede resolver explícitamente.

A continuación se muestra el ejemplo con `Fichero` y `FicheroEscribible`:

```java
// Interfaz base
public interface Fichero {
    String leerContenido();
}

// Interfaz que extiende la anterior (herencia de interfaces)
public interface FicheroEscribible extends Fichero {
    void escribirContenido(String contenido);
    void eliminar();
}

// Clase concreta que implementa la interfaz más específica
public class FicheroTexto implements FicheroEscribible {
    private String nombre;
    private String contenido;
    
    public FicheroTexto(String nombre) {
        this.nombre = nombre;
        this.contenido = "";
    }
    
    @Override
    public String leerContenido() {
        return contenido;
    }
    
    @Override
    public void escribirContenido(String contenido) {
        this.contenido = contenido;
        System.out.println("Escrito en " + nombre + ": " + contenido);
    }
    
    @Override
    public void eliminar() {
        this.contenido = null;
        System.out.println("Fichero " + nombre + " eliminado");
    }
}
```

**Ejemplo de herencia múltiple de interfaces:**

```java
public interface Serializable {
    String serializar();
}

public interface Compresible {
    void comprimir();
}

// Herencia múltiple de interfaces
public interface FicheroAvanzado extends Fichero, Serializable, Compresible {
    void cargarMetadatos();
}

// Clase que implementa todas las interfaces heredadas
public class FicheroConfiguracion implements FicheroAvanzado {
    private String datos;
    
    @Override
    public String leerContenido() {
        return datos;
    }
    
    @Override
    public String serializar() {
        return "XML:" + datos;
    }
    
    @Override
    public void comprimir() {
        System.out.println("Comprimiendo datos...");
    }
    
    @Override
    public void cargarMetadatos() {
        System.out.println("Cargando metadatos del fichero");
    }
}
```

**Demostración de uso:**

```java
public class Main {
    public static void main(String[] args) {
        // Polimorfismo con interfaces
        Fichero f = new FicheroTexto("documento.txt");
        System.out.println("Contenido: " + f.leerContenido());
        
        FicheroEscribible fe = new FicheroTexto("notas.txt");
        fe.escribirContenido("Hola mundo");
        System.out.println("Leído: " + fe.leerContenido());
        fe.eliminar();
        
        // Herencia múltiple
        FicheroAvanzado fa = new FicheroConfiguracion();
        fa.cargarMetadatos();
        fa.comprimir();
        System.out.println("Serializado: " + fa.serializar());
    }
}
```

**Salida:**
```
Contenido: 
Escrito en notas.txt: Hola mundo
Leído: Hola mundo
Fichero notas.txt eliminado
Cargando metadatos del fichero
Comprimiendo datos...
Serializado: XML:null
```

Como se observa, la herencia de interfaces permite construir jerarquías de comportamiento reutilizables, y la herencia múltiple de interfaces proporciona una flexibilidad que no existe con clases, siendo una de las características más potentes de Java para diseñar sistemas desacoplados y extensibles.
