<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Las cuatro características fundamentales de la Programación Orientada a Objetos (POO) son la **Abstracción**, la **Encapsulación**, la **Herencia** y el **Polimorfismo**. Estas características permiten un paradigma de programación que se asemeja más a cómo se organizan las cosas en el mundo real, facilitando la creación de software más modular, reutilizable y mantenible.

La **Abstracción** consiste en modelar las características esenciales de una entidad, ignorando los detalles irrelevantes para el contexto. En la práctica, se crean clases que definen atributos y métodos que representan el comportamiento clave del objeto, sin especificar cómo se implementa internamente cada operación. Por ejemplo, para un objeto "Coche", se abstraen propiedades como "velocidad" y métodos como "acelerar()", sin detallar inicialmente los complejos mecanismos del motor.

La **Encapsulación** es el mecanismo que agrupa los datos (atributos) y los métodos que operan sobre ellos en una única unidad (la clase), y restringe el acceso directo a los componentes internos. Esto se logra principalmente mediante modificadores de acceso como `private` y `public`. Un atributo declarado como `private` solo puede ser modificado o leído a través de métodos públicos (getters y setters), protegiendo la integridad de los datos y ocultando la complejidad interna. Por ejemplo, el nivel de combustible de un coche podría ser un atributo privado, y solo se podría modificar de forma controlada mediante un método público `repostar()`.

La **Herencia** permite crear nuevas clases (clases hijas o subclases) a partir de clases existentes (clases padres o superclases), heredando automáticamente sus atributos y métodos. Esto fomenta la reutilización de código y establece una relación jerárquica de tipo "es-un". Por ejemplo, una clase `Vehiculo` con atributos como "ruedas" podría ser la superclase de `Coche` y `Moto`. Las subclases heredan estos atributos y pueden añadir otros específicos, como `numeroPuertas` para el coche.

Finalmente, el **Polimorfismo** (que significa "muchas formas") permite que una misma operación o método se comporte de manera diferente según el objeto que lo ejecute. Esto se manifiesta comúnmente cuando una subclase redefine (sobrescribe) un método heredado de su superclase. Así, aunque se tenga una referencia genérica de tipo `Vehiculo`, al invocar su método `mover()`, este ejecutará la implementación específica de `Coche`, `Moto` o cualquier otra subclase. El polimorfismo permite escribir código más genérico y flexible que trabaje con familias de objetos relacionados.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### **Java, Python, C++ y JavaScript** son cuatro lenguajes populares que permiten la programación orientada a objetos.

**Java** es un lenguaje puramente orientado a objetos donde casi todo se estructura en clases, siendo fundamental para el desarrollo empresarial y de aplicaciones Android. **Python**, conocido por su simplicidad, implementa la POO de forma flexible y legible, usándose ampliamente en campos como la inteligencia artificial y el desarrollo web.

**C++** es una extensión del lenguaje C que añade soporte para clases y objetos, manteniendo la eficiencia del código de bajo nivel, lo que lo hace esencial para videojuegos y sistemas operativos. **JavaScript**, el lenguaje de la web, está basado en prototipos, un modelo diferente pero que cumple con los principios de la POO, permitiendo crear aplicaciones web interactivas y dinámicas.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?
Programación Estructurada
La programación estructurada es un paradigma que organiza el código usando solo tres estructuras de control: secuencia (instrucciones en orden), selección (como if/else) e iteración (bucles como while). Su objetivo principal es evitar el uso del salto incondicional (goto), que generaba código enmarañado y difícil de seguir ("código espagueti").

Al imponer esta disciplina, se logra un flujo de programa más claro, predecible y fácil de depurar. Lenguajes como C o Pascal se basan en este paradigma, el cual supuso un gran avance para la claridad del software, aunque el programa seguía siendo esencialmente una unidad monolítica.

Programación Modular
La programación modular es una evolución que divide un programa grande en partes más pequeñas e independientes llamadas módulos. Cada módulo agrupa un conjunto de funciones y datos relacionados con una tarea específica (por ejemplo, manejo de archivos o cálculos matemáticos).

Este enfoque fomenta la reutilización del código y la separación de responsabilidades, ya que los módulos se pueden desarrollar y probar por separado. Facilita el trabajo en equipo y el mantenimiento, ya que un cambio suele aislarse a un solo módulo. Este concepto de "caja negra" fue un paso directo hacia la idea de encapsulación en la POO.

## 4. Programación Estructurada
La programación estructurada es un paradigma que organiza el código usando solo tres estructuras de control: secuencia (instrucciones en orden), selección (como if/else) e iteración (bucles como while). Su objetivo principal es evitar el uso del salto incondicional (goto), que generaba código enmarañado y difícil de seguir ("código espagueti").

Al imponer esta disciplina, se logra un flujo de programa más claro, predecible y fácil de depurar. Lenguajes como C o Pascal se basan en este paradigma, el cual supuso un gran avance para la claridad del software, aunque el programa seguía siendo esencialmente una unidad monolítica.

Programación Modular
La programación modular es una evolución que divide un programa grande en partes más pequeñas e independientes llamadas módulos. Cada módulo agrupa un conjunto de funciones y datos relacionados con una tarea específica (por ejemplo, manejo de archivos o cálculos matemáticos).

Este enfoque fomenta la reutilización del código y la separación de responsabilidades, ya que los módulos se pueden desarrollar y probar por separado. Facilita el trabajo en equipo y el mantenimiento, ya que un cambio suele aislarse a un solo módulo. Este concepto de "caja negra" fue un paso directo hacia la idea de encapsulación en la POO.

### Respuesta

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Una **clase** es una plantilla o un plano que define las características (atributos) y comportamientos (métodos) comunes para un tipo de objeto. Se puede pensar como un molde abstracto. Un **objeto**, en cambio, es una **instancia concreta** creada a partir de esa clase. Es la materialización del molde, que ocupa memoria y tiene sus propios valores en los atributos. Por ejemplo, la clase `Coche` define los atributos `color` y `velocidad`; un objeto sería `miCoche` de color rojo y velocidad 0 km/h.

Una **instancia** es sinónimo de objeto. Se refiere específicamente al proceso de creación (*instanciación*) de un objeto único a partir de una clase. Por tanto, objeto e instancia son dos términos para la misma entidad concreta, siendo "instancia" el término que enfatiza su relación de pertenencia a una clase.

No todos los lenguajes orientados a objetos manejan el concepto de clase. Existen lenguajes basados en **prototipos**, como **JavaScript**, donde los objetos se crean directamente a partir de otros objetos (prototipos) y no a partir de clases abstractas. En estos lenguajes, la herencia se logra delegando en un objeto prototipo ya existente.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Los objetos se almacenan en una región de memoria llamada **heap** o **montón**. A diferencia de la **pila** (stack), que guarda variables locales primitivas y referencias de forma ordenada y automática, el heap es un área de memoria dinámica y de mayor tamaño donde se alojan los objetos en sí, con un ciclo de vida menos predecible.

El uso del heap para objetos **no es igual en todos los lenguajes**. En lenguajes como Java, C# o Python, esta gestión es automática y el programador no maneja direcciones de memoria directamente. En cambio, en C++, el programador puede decidir explícitamente si un objeto se crea en el heap (con `new`) o en la stack (como variable local), asumiendo también la responsabilidad de liberar esa memoria.

La **recolección de basura (Garbage Collection)** es un mecanismo automático de gestión de memoria que libera los objetos del heap que ya no son accesibles o referenciados por ninguna parte del programa. Un recolector de basura, presente en Java o Python, monitorea y reclama esta memoria, evitando fugas y simplificando el desarrollo. Lenguajes como C o C++ no lo incluyen, requiriendo una gestión manual con `malloc/free` o `new/delete`.


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Un **método** es una función o procedimiento definido dentro de una clase que describe un comportamiento o acción que los objetos de esa clase pueden realizar. Operan sobre los datos (atributos) del objeto y definen la interfaz pública a través de la cual se interactúa con él. Por ejemplo, un método `acelerar()` en una clase `Coche` modificaría el atributo `velocidad`.

La **sobrecarga de métodos (overloading)** es la capacidad de definir múltiples métodos dentro de una misma clase que compartan el **mismo nombre**, pero que se diferencien por su **lista de parámetros** (número, tipo o orden). El compilador decide cuál ejecutar en función de los argumentos usados en la llamada. Por ejemplo, una clase `Calculadora` podría tener varios métodos `sumar(int a, int b)` y `sumar(double a, double b)`. Esto mejora la legibilidad al usar un nombre único para operaciones conceptualmente similares.

La sobrecarga es un tipo de polimorfismo en tiempo de compilación (polimorfismo estático) y es común en lenguajes como Java y C++. No debe confundirse con la **sobrescritura (overriding)**, que implica redefinir un método heredado en una subclase.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### ```java
public class Punto {
    // Atributos con visibilidad de paquete (por defecto)
    double x;
    double y;

    // Método que calcula la distancia al origen (0,0)
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

**Ejemplo de uso**:
```java
public class Main {
    public static void main(String[] args) {
        // Crear una instancia (objeto) de Punto
        Punto miPunto = new Punto();
        
        // Asignar valores a los atributos
        miPunto.x = 3.0;
        miPunto.y = 4.0;
        
        // Usar el método y mostrar el resultado
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("Distancia al origen: " + distancia); // Muestra 5.0
    }
}
```

La clase `Punto` define dos atributos `x` e `y` con visibilidad de paquete. El método `calculaDistanciaAOrigen()` utiliza el teorema de Pitágoras para calcular la distancia desde el punto actual hasta el origen de coordenadas (0,0). 

En el ejemplo de uso, se crea un objeto `miPunto` con las coordenadas (3.0, 4.0). Al llamar al método `calculaDistanciaAOrigen()`, se obtiene 5.0 como resultado, que es la distancia euclidiana desde ese punto al origen.


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### El **punto de entrada** en un programa Java es el método `public static void main(String[] args)`. La máquina virtual de Java (JVM) busca y ejecuta este método específico para iniciar el programa.

La palabra clave **`static`** indica que un miembro (método o atributo) **pertenece a la clase misma**, no a ninguna instancia particular de la clase. Esto significa que se puede acceder directamente usando el nombre de la clase (por ejemplo, `Clase.metodo()`), sin necesidad de crear un objeto. En el `main`, `static` es esencial porque la JVM debe poder invocarlo antes de que exista ningún objeto.

**`static` no se emplea solo para `main`**. Se usa comúnmente para:
1. **Atributos compartidos**: Como contadores o constantes (combinado con `final`).
2. **Métodos utilitarios**: Que no necesitan estado de objeto, como `Math.sqrt()`.
3. **Bloques de inicialización**: Que se ejecutan al cargar la clase.

La combinación **`static final`** se usa para definir **constantes a nivel de clase**. El atributo es único (`static`) e inmutable (`final`). Por convención, sus nombres son en mayúsculas. Por ejemplo: `public static final double PI = 3.1416;`. Esto agrupa constantes relacionadas lógicamente en una clase y garantiza un único valor accesible globalmente.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Se puede compilar y ejecutar un programa Java desde la línea de comandos usando `javac` y `java`. Primero, se escribe el código en un archivo con extensión `.java` (por ejemplo, `MiPrograma.java`). Luego, desde la terminal en el mismo directorio, se ejecuta `javac MiPrograma.java`. Si no hay errores, esto genera un archivo **`.class`** con el mismo nombre (`MiPrograma.class`). Finalmente, para ejecutar el programa, se usa `java MiPrograma` (sin la extensión `.class`). El comando `java` inicia la Máquina Virtual de Java (JVM).

Java es un lenguaje **tanto compilado como interpretado**. El compilador `javac` traduce el código fuente (`.java`) a un código intermedio llamado **byte-code**, que se guarda en archivos `.class`. Este byte-code no es código máquina nativo de un sistema operativo específico, sino un conjunto de instrucciones optimizadas para la **Máquina Virtual de Java (JVM)**. La JVM es un programa que actúa como una "computadora virtual", capaz de **interpretar y ejecutar** este byte-code en cualquier sistema donde esté instalada. Este proceso es el que permite el principio de "escribir una vez, ejecutar en cualquier lugar".

Los archivos `.class` contienen el byte-code, que es un conjunto de instrucciones compactas y neutrales para la plataforma. La JVM carga estos archivos, verifica el byte-code por seguridad y luego lo ejecuta, normalmente mediante un compilador **JIT (Just-In-Time)** que convierte partes del byte-code a código máquina nativo en tiempo de ejecución para mayor velocidad.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### La palabra clave **`new`** es el operador en Java que **solicita memoria en el heap** para crear una nueva instancia (objeto) de una clase. Su ejecución desencadena la llamada al **constructor** de dicha clase, que es un método especial encargado de **inicializar el estado** del objeto recién asignado.

Un **constructor** es un método especial que tiene el **mismo nombre que la clase** y **no tiene tipo de retorno** (ni siquiera `void`). Su propósito principal es asignar valores iniciales a los atributos del objeto al ser creado. Si no se define explícitamente, el compilador proporciona un **constructor por defecto** sin parámetros. Es común definir constructores con parámetros para obligar a una inicialización específica.

Ejemplo de clase `Empleado` con constructor:
```java
public class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

Uso del constructor con `new`:
```java
// 'new' reserva memoria y llama al constructor
Empleado emp = new Empleado("12345678A", "Ana", "García López");
```
En este ejemplo, `new Empleado(...)` asigna memoria para un nuevo objeto y el constructor asigna los valores de los parámetros a los atributos `this.dni`, `this.nombre` y `this.apellidos`.


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### La referencia **`this`** es una **palabra clave** en Java que, dentro de un método de instancia o un constructor, hace referencia al **objeto actual** en el que se está ejecutando el código. Su función principal es eliminar ambigüedades cuando un parámetro o variable local tiene el mismo nombre que un atributo de la clase, permitiendo distinguir entre ellos. Por ejemplo, en un constructor `public Punto(double x, double y)`, usar `this.x = x` asigna el valor del parámetro `x` al atributo `x` del objeto que se está construyendo.

**No se llama igual en todos los lenguajes**. Otros lenguajes orientados a objetos utilizan palabras clave diferentes para el mismo propósito: en **C++** y **C#** también se usa `this`, aunque en C++ es un puntero. En **Python**, la referencia al objeto actual es el primer parámetro explícito de los métodos, convencionalmente llamado `self`. En **JavaScript**, se utiliza `this`, pero su comportamiento y valor pueden variar más dependiendo del contexto de ejecución.

Ejemplo del uso de `this` en la clase `Punto`, añadiendo un constructor y un método que compare distancias:
```java
public class Punto {
    double x;
    double y;

    // Constructor usando 'this' para diferenciar parámetros de atributos
    public Punto(double x, double y) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
        this.y = y;
    }

    // Método que usa 'this' para pasar el objeto actual como argumento
    public boolean estaMasLejosQue(Punto otroPunto) {
        double distThis = this.calculaDistanciaAOrigen(); // Distancia de ESTE objeto
        double distOtro = otroPunto.calculaDistanciaAOrigen();
        return distThis > distOtro;
    }

    public double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y); // 'this' es opcional aquí
    }
}
```

En este ejemplo, `this` es **obligatorio** en el constructor para evitar la ambigüedad de nombres, y es **opcional** (pero aclaratorio) en `calculaDistanciaAOrigen()`. En el método `estaMasLejosQue`, `this` se usa explícitamente para invocar un método sobre el propio objeto y compararlo con otro.


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### ```java
public class Punto {
    double x;
    double y;

    // ... (constructores y otros métodos previos)

    // Método que calcula la distancia entre este punto (this) y otro punto dado
    public double distanciaA(Punto otro) {
        double diffX = this.x - otro.x;     // Diferencia en coordenada X
        double diffY = this.y - otro.y;     // Diferencia en coordenada Y
        return Math.sqrt(diffX * diffX + diffY * diffY); // Teorema de Pitágoras
    }
}
```

**Ejemplo de uso:**
```java
public class Main {
    public static void main(String[] args) {
        Punto p1 = new Punto(3.0, 4.0);
        Punto p2 = new Punto(7.0, 1.0);
        
        double distancia = p1.distanciaA(p2);
        System.out.println("Distancia entre p1 y p2: " + distancia); // Aproximadamente 5.0
    }
}
```

El método `distanciaA` aplica la fórmula de distancia euclidiana entre dos puntos en un plano. La palabra clave `this` permite acceder a las coordenadas (`x`, `y`) del objeto desde el que se invoca el método, mientras que el parámetro `otro` proporciona acceso a las coordenadas del punto con el que se compara. El cálculo se realiza mediante la diferencia de coordenadas en cada eje y la aplicación del teorema de Pitágoras.


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función?

### En Java, el paso de parámetros **es siempre por valor**, pero su comportamiento varía según el tipo.

Cuando se pasa un objeto de tipo `Punto` como parámetro, se copia **el valor de la referencia** (la dirección de memoria) al objeto. Dentro del método, se está trabajando con una **copia de esa referencia**, que sigue apuntando al **mismo objeto original** en el heap. Por tanto, si dentro del método se modifican los atributos del objeto recibido (ej: `otro.x = 10;`), esos cambios **afectan directamente al objeto original** fuera del método, porque se ha accedido al mismo espacio de memoria. No obstante, si dentro del método se reasigna la propia referencia (ej: `otro = new Punto();`), esa reasignación solo afecta a la copia local de la referencia, **no al objeto original ni a la referencia externa**.

Cuando se pasa un tipo primitivo como un `int`, se copia directamente **su valor numérico**. Cualquier modificación dentro del método (ej: `valor = 20;`) se realiza sobre esa copia local y **no tiene ningún efecto** sobre la variable original fuera del método. Esto se debe a que no existe un concepto de referencia para los tipos primitivos; se copia únicamente el dato.

En resumen: en Java, el paso es siempre **por valor**, pero para objetos ese valor es una **referencia**, lo que permite modificar su estado interno. Para tipos primitivos, el valor es el dato en sí, por lo que las modificaciones son locales.


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### El método **`toString()`** en Java es un método definido en la clase base `Object` que devuelve una **representación en cadena de texto (String)** de un objeto. Su propósito principal es proporcionar una descripción legible del estado del objeto, útil para depuración, logging o visualización. Cuando se concatena un objeto con un String (ej: `"Punto: " + miPunto`) o se pasa a `System.out.println()`, Java llama automáticamente a su método `toString()`.

**Sí existe en otros lenguajes** con nombres y mecanismos similares: en **C#** existe `ToString()` como método virtual en la clase base `Object`. En **Python**, el método equivalente es `__str__()` para representaciones informales y `__repr__()` para representaciones técnicas. En **JavaScript**, todo objeto tiene un método `toString()`, aunque su implementación por defecto suele ser poco informativa.

Ejemplo de implementación de `toString()` en la clase `Punto`:
```java
public class Punto {
    double x;
    double y;
    
    @Override
    public String toString() {
        return "Punto [x=" + x + ", y=" + y + "]";
    }
}
```

Uso automático del método:
```java
Punto p = new Punto(5.0, 3.0);
System.out.println(p); // Imprime automáticamente: Punto [x=5.0, y=3.0]
System.out.println("Coordenadas: " + p); // Concatena: Coordenadas: Punto [x=5.0, y=3.0]
```
La anotación `@Override` indica explícitamente que se está sobrescribiendo el método de la clase padre `Object`. Esta implementación personalizada es mucho más útil que la versión por defecto de `Object` (que devuelve algo como `Punto@15db9742`).


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### La comparación es acertada: una **clase** en Java/POO es conceptualmente similar a un **`struct` en C**, ya que ambos son **plantillas o tipos de datos compuestos** que agrupan varios datos (atributos/campos) bajo un mismo nombre. Sin embargo, un `struct` tradicional de C carece de elementos fundamentales que convierten a una clase en una entidad de la programación orientada a objetos.

Para que un `struct` se asemeje a una clase y sus variables sean verdaderas instancias, le faltan tres pilares esenciales:
1.  **Comportamiento (métodos)**: Un `struct` en C solo contiene **datos** (campos). Una clase define no solo el estado (atributos), sino también el **comportamiento** a través de métodos que operan sobre esos datos.
2.  **Encapsulación**: En un `struct`, todos los campos son accesibles directamente desde cualquier parte del programa. Una clase implementa **encapsulación** mediante modificadores de acceso (`private`, `protected`), ocultando los datos internos y exponiendo una interfaz controlada de métodos públicos para interactuar con ellos.
3.  **Mecanismos de abstracción y relación**: Un `struct` no tiene herencia, polimorfismo o constructores. Una clase permite **abstraer** conceptos, definir **constructores** para una inicialización controlada, establecer relaciones de **herencia** con otras clases y aprovechar el **polimorfismo**.

En resumen, un `struct` es principalmente un **contenedor de datos pasivo**, mientras que una clase es una **unidad activa** que combina estado y comportamiento, protegida por encapsulación y capaz de relacionarse mediante herencia y polimorfismo. Lenguajes como C++ evolucionaron este concepto al permitir que sus `struct` tuvieran métodos, herencia y modificadores de acceso, borrando prácticamente la diferencia con una `class` (salvo la visibilidad por defecto).


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Se puede emular una clase `Punto` en C usando un `struct` para los datos y funciones que reciban un puntero a ese struct como primer parámetro. Esto simula el estado y comportamiento de un objeto.

```c
// "Clase" Punto (struct con datos)
struct Punto {
    double x;
    double y;
};

// "Método" que calcula la distancia al origen
double Punto_calculaDistanciaAOrigen(struct Punto* self) {
    return sqrt((self->x * self->x) + (self->y * self->y));
}

// Constructor manual (función de inicialización)
void Punto_inicializar(struct Punto* self, double x, double y) {
    self->x = x;
    self->y = y;
}
```

Ejemplo de uso (instanciación y llamada al "método"):
```c
int main() {
    struct Punto p1;                    // "Instancia" en el stack
    Punto_inicializar(&p1, 3.0, 4.0);   // Llamada al constructor

    double dist = Punto_calculaDistanciaAOrigen(&p1); // Llamada al "método"
    printf("Distancia: %f\n", dist);   // Muestra 5.0
    return 0;
}
```

**¿Qué ha pasado con `this`?** En este código, el parámetro **`self`** (de tipo `struct Punto*`) cumple **exactamente la misma función** que la referencia `this` en Java. Es un puntero explícito que se pasa manualmente a cada función para indicar sobre qué "instancia" (qué struct concreto) debe operar. Mientras que en Java `this` es implícito y automático, en C se debe pasar explícitamente como primer argumento.

Esta emulación revela la mecánica subyacente de los objetos: **un objeto es esencialmente un bloque de datos (struct) junto con funciones que operan sobre él, recibiendo una referencia a esos datos como contexto**. Lo que la POO y lenguajes como Java añaden es **azúcar sintáctico** y **gestión automática** para unificar datos y funciones en una entidad (clase), hacer que `this` sea implícito, y gestionar la memoria y el ciclo de vida de forma más segura.
