<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En lenguajes sin genericidad como C o en versiones básicas de Java, una forma de construir estructuras de datos “genéricas” consiste en almacenar referencias de tipo genérico (`void*` en C o `Object` en Java). De este modo, un mismo array puede contener elementos de distintos tipos, ya que todos ellos se tratan como referencias opacas. Esta técnica permite reutilizar código sin necesidad de duplicar la estructura para cada tipo, pero pierde seguridad de tipos: el compilador no puede comprobar si el tipo almacenado coincide con el esperado al recuperarlo.

En C, el uso de `void*` permite crear, por ejemplo, una lista basada en array capaz de almacenar cualquier dato. Sin embargo, el programador debe encargarse manualmente de convertir los tipos (casting) y de gestionar la memoria. Un ejemplo sencillo sería:

```c
#include <stdio.h>

#define MAX 10

typedef struct {
    void* data[MAX];
    int size;
} GenericArray;

void add(GenericArray* arr, void* elem) {
    arr->data[arr->size++] = elem;
}

void* get(GenericArray* arr, int index) {
    return arr->data[index];
}
```

En Java, se puede hacer algo equivalente utilizando `Object`, ya que todas las clases heredan de él. Esto permite crear una estructura basada en arrays capaz de almacenar cualquier objeto, aunque obliga a realizar conversiones explícitas al recuperar los datos. Por ejemplo:

```java
class GenericArray {
    private Object[] data;
    private int size;

    public GenericArray(int capacity) {
        data = new Object[capacity];
        size = 0;
    }

    public void add(Object elem) {
        data[size++] = elem;
    }

    public Object get(int index) {
        return data[index];
    }
}
```

El principal inconveniente de este enfoque es la falta de seguridad en tiempo de compilación, ya que los errores de tipo solo se detectan en ejecución. Esto justifica la aparición de la genericidad en Java (con `Generics`), que permite mantener la flexibilidad de estas estructuras pero añadiendo comprobación de tipos en compilación y evitando conversiones explícitas innecesarias.


## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La **programación genérica** consiste en escribir código (clases, métodos o estructuras de datos) que pueda trabajar con distintos tipos de datos sin necesidad de duplicarlo para cada uno de ellos. En lugar de fijar un tipo concreto, se utilizan parámetros de tipo (como los *generics* en Java) que permiten que el compilador verifique la coherencia de tipos en tiempo de compilación. De este modo se consigue reutilización de código, mayor claridad y, sobre todo, mayor seguridad frente a errores de tipo.

El ejemplo anterior basado en `void*` en C o en `Object` en Java **no es programación genérica en sentido estricto**, sino una aproximación previa. Aunque permite almacenar cualquier tipo de dato, lo hace perdiendo la comprobación de tipos en compilación, obligando a realizar conversiones explícitas y pudiendo provocar errores en tiempo de ejecución. La programación genérica moderna (por ejemplo, `List<T>` en Java) soluciona este problema al mantener la flexibilidad sin renunciar a la seguridad de tipos.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El uso de `void*` en C o `Object` en Java implica que el compilador pierde información sobre el tipo real de los datos almacenados. Como consecuencia, no puede realizar comprobaciones de tipo en tiempo de compilación, por lo que se permite insertar cualquier valor sin verificar si es coherente con el uso posterior de la estructura. Esto elimina una de las principales ventajas de los lenguajes tipados, que es detectar errores antes de ejecutar el programa.

Además, al recuperar los elementos es necesario realizar conversiones explícitas (*casting*). Estas conversiones son potencialmente peligrosas, ya que si se realiza un cast incorrecto (por ejemplo, interpretar un `String` como un `Integer`), el error solo se detectará en tiempo de ejecución, normalmente mediante excepciones o comportamientos indefinidos. Esto hace que los fallos sean más difíciles de localizar y depurar.

Otro problema es la pérdida de expresividad y claridad del código. Al no estar especificado el tipo de los elementos, resulta menos evidente cómo debe utilizarse la estructura de datos, aumentando la probabilidad de uso incorrecto. Además, se dificulta el mantenimiento, ya que otros programadores no pueden inferir fácilmente qué tipos son esperados en cada contexto.

En conjunto, estos inconvenientes justifican el uso de mecanismos de programación genérica con parámetros de tipo, que permiten mantener la flexibilidad de las estructuras de datos reutilizables, pero añadiendo verificación estática de tipos y evitando conversiones inseguras.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los **parámetros de tipo** son identificadores que se utilizan en clases, interfaces o métodos para representar tipos de datos de forma abstracta. En lugar de fijar un tipo concreto (como `String` o `Integer`), se define un parámetro (por ejemplo, `T`) que será sustituido por un tipo real cuando se utilice la clase o método. De este modo, se puede escribir código reutilizable que funciona con distintos tipos manteniendo la coherencia y la seguridad.

Este mecanismo permite que el compilador conozca el tipo concreto en cada uso y pueda realizar comprobaciones en tiempo de compilación. Así, se evitan conversiones explícitas (*casting*) y se reducen los errores en ejecución. Por ejemplo, una estructura como `List<T>` garantiza que solo se podrán añadir y recuperar elementos del tipo especificado, evitando mezclas incorrectas de datos.

En definitiva, los parámetros de tipo son la base de la programación genérica moderna, ya que combinan flexibilidad (una única implementación válida para múltiples tipos) con seguridad de tipos. Esto mejora la legibilidad del código, facilita su mantenimiento y reduce la aparición de errores difíciles de detectar.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En Java, la programación genérica se implementa mediante *generics*, que permiten parametrizar clases con tipos concretos. Al instanciar una colección indicando el tipo (`String` en este caso), el compilador garantiza que solo se podrán insertar elementos de ese tipo y que al recuperarlos no será necesario realizar conversiones explícitas. Esto aporta seguridad en tiempo de compilación y evita errores típicos asociados al uso de `Object`.

```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        List<String> lista = new ArrayList<>();

        lista.add("Hola");
        lista.add("Mundo");
        // lista.add(123); // Error de compilación

        for (String s : lista) {
            System.out.println(s.toUpperCase()); // Uso directo como String
        }
    }
}
```

En C++, la programación genérica se implementa mediante *templates*, que permiten definir clases o funciones parametrizadas por tipos. Al instanciar un `std::vector<std::string>`, el compilador genera una versión concreta del vector para ese tipo, garantizando igualmente que solo se puedan almacenar cadenas y que su uso sea seguro sin conversiones. Esto proporciona eficiencia y seguridad de tipos en tiempo de compilación.

```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    std::vector<std::string> vec;

    vec.push_back("Hola");
    vec.push_back("Mundo");
    // vec.push_back(123); // Error de compilación

    for (const std::string& s : vec) {
        std::cout << s << std::endl; // Uso directo como string
    }

    return 0;
}
```

En ambos casos, se observa cómo la programación genérica permite trabajar con estructuras reutilizables sin perder seguridad de tipos. El compilador impide insertar elementos de tipo incorrecto y permite tratar cada elemento como `String` sin necesidad de conversiones, lo que mejora la robustez y claridad del código.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

Cuando se instancia una clase con parámetros de tipo, el compilador debe transformar ese código genérico en algo ejecutable con tipos concretos. En esencia, se comprueba que los tipos utilizados son correctos y coherentes con las operaciones definidas en la clase o método genérico. Sin embargo, la forma en que se materializa ese código concreto depende del lenguaje, ya que no todos implementan la programación genérica de la misma manera.

En Java, el mecanismo utilizado es el llamado ***type erasure*** (borrado de tipos). Durante la compilación, la información de los parámetros de tipo (`T`, `String`, etc.) se elimina y se sustituye por su tipo límite (normalmente `Object` si no hay restricción). El compilador inserta automáticamente los *casts* necesarios para mantener la coherencia. Esto implica que en tiempo de ejecución no existe información real sobre el tipo genérico, lo que explica algunas limitaciones (por ejemplo, no se pueden crear arrays de tipos genéricos). Aun así, se mantiene la seguridad en compilación.

En C++, en cambio, se emplea la **instanciación de plantillas** (*template instantiation*). El compilador genera una versión específica del código para cada tipo concreto utilizado. Por ejemplo, `vector<int>` y `vector<string>` producen implementaciones distintas a nivel de código máquina. Esto significa que los tipos se conservan completamente, sin borrado, y permite mayor flexibilidad (como usar cualquier operación válida para ese tipo), aunque puede aumentar el tamaño del código generado.

En resumen, Java y C++ no hacen lo mismo: Java reutiliza una única implementación mediante *type erasure*, mientras que C++ genera múltiples versiones del código mediante instanciación de plantillas. Ambos enfoques logran seguridad de tipos en compilación, pero difieren en cómo gestionan los tipos en tiempo de ejecución y en sus implicaciones de rendimiento y expresividad.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Se puede definir una clase genérica `Par` utilizando dos parámetros de tipo (por ejemplo, `T` y `U`), de forma que permita almacenar dos valores potencialmente de tipos distintos. El compilador se encargará de comprobar que los tipos utilizados en cada instancia son correctos, evitando conversiones explícitas y errores en tiempo de ejecución. Esta clase es útil cuando se desea devolver o agrupar dos valores relacionados sin crear una clase específica para cada caso.

```java id="par123"
public class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```

Un ejemplo de uso sería definir una función que calcule la media y la desviación típica de un array de `double`, devolviendo ambos valores en un objeto `Par<Double, Double>`. Gracias a la programación genérica, se garantiza que ambos resultados son del tipo esperado y pueden utilizarse directamente sin conversiones.

```java id="uso123"
public static Par<Double, Double> calcularEstadisticas(double[] datos) {
    double suma = 0.0;

    for (double d : datos) {
        suma += d;
    }

    double media = suma / datos.length;

    double sumaCuadrados = 0.0;
    for (double d : datos) {
        sumaCuadrados += Math.pow(d - media, 2);
    }

    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}
```

El uso de esta función permite recuperar ambos valores de forma clara y segura, manteniendo la coherencia de tipos. Por ejemplo:

```java id="uso456"
public static void main(String[] args) {
    double[] datos = {1.0, 2.0, 3.0, 4.0, 5.0};

    Par<Double, Double> resultado = calcularEstadisticas(datos);

    System.out.println("Media: " + resultado.getPrimero());
    System.out.println("Desviación: " + resultado.getSegundo());
}
```

Este enfoque evita crear estructuras específicas para cada caso y mejora la reutilización del código, manteniendo la seguridad de tipos que proporciona la programación genérica.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

En Java, también es posible declarar **parámetros de tipo a nivel de método**, lo que permite definir operaciones genéricas sin necesidad de parametrizar toda la clase. Un método genérico declara sus parámetros de tipo antes del tipo de retorno (por ejemplo, `<T>`). Esto permite que el compilador infiera el tipo concreto en cada llamada, manteniendo seguridad de tipos y evitando conversiones explícitas.

Primero, una versión **sin genéricos**, usando `Object`:

```java
public static Object seleccionaUno(Object a, Object b) {
    if (Math.random() < 0.5) {
        return a;
    } else {
        return b;
    }
}
```

El problema de este enfoque es doble. Por un lado, al recuperar el valor devuelto es necesario hacer *downcasting*, ya que el tipo de retorno es `Object`. Por otro, no se fuerza que ambos parámetros sean del mismo tipo, por lo que se podrían pasar objetos heterogéneos sin que el compilador lo detecte:

```java
String s = (String) seleccionaUno("hola", "adios"); // requiere cast
Object o = seleccionaUno("hola", 123); // permitido, pero incoherente
```

Ahora, una versión **con método genérico**:

```java
public static <T> T seleccionaUno(T a, T b) {
    if (Math.random() < 0.5) {
        return a;
    } else {
        return b;
    }
}
```

En este caso, el compilador infiere automáticamente el tipo `T` en cada llamada. Esto evita el *downcasting*, ya que el valor devuelto ya tiene el tipo correcto, y además obliga a que ambos parámetros sean del mismo tipo o compatibles entre sí:

```java
String s = seleccionaUno("hola", "adios"); // sin cast
// seleccionaUno("hola", 123); // error de compilación
```

En conclusión, el uso de parámetros de tipo en métodos mejora la seguridad y claridad del código. Se eliminan conversiones explícitas y se garantiza que los argumentos cumplen ciertas restricciones de tipo, lo que reduce errores y hace el código más robusto y mantenible.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden establecer **restricciones en los parámetros de tipo** mediante *bounded generics*. Esto permite indicar que un parámetro genérico debe pertenecer a una cierta jerarquía de tipos, por ejemplo `<T extends Number>`, lo que garantiza que los objetos de ese tipo podrán tratarse como números. Gracias a esto, el compilador permite invocar métodos definidos en `Number` (como `doubleValue()`), manteniendo seguridad de tipos sin perder flexibilidad.

Una primera solución consiste en no usar genéricos y definir directamente las coordenadas como `Number`. Esto permite almacenar cualquier tipo numérico (`Integer`, `Double`, etc.), pero no asegura que ambos puntos trabajen con el mismo tipo concreto:

```java id="punto1"
public class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(Punto otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

La segunda solución utiliza genéricos con restricción, lo que permite trabajar con un tipo numérico concreto y mantener coherencia entre coordenadas:

```java id="punto2"
public class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    public double calcularDistanciaA(Punto<T> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En cuanto al *type erasure*, en ambos casos el tipo genérico se elimina durante la compilación. En la versión genérica, `T` se sustituye por su límite superior (`Number`), por lo que el tipo final tras compilación es equivalente a usar `Number`. Sin embargo, el uso de genéricos sigue siendo ventajoso porque el compilador realiza comprobaciones de tipo antes de aplicar ese borrado, evitando usos incorrectos en el código fuente.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

Ambas soluciones permiten trabajar con distintos tipos numéricos, pero no ofrecen el mismo nivel de control sobre los tipos. En la versión sin genéricos (usando Number), es perfectamente posible crear un punto con coordenadas de tipos distintos, por ejemplo una Integer y otra Double, ya que ambas son subclases de Number. El compilador no impide esta mezcla, lo que puede introducir incoherencias si se esperaba trabajar siempre con un tipo homogéneo.

En cambio, en la versión con genéricos (Punto<T extends Number>), se refuerza el chequeo de tipos. Al instanciar un punto, se fija un tipo concreto para T, por lo que ambas coordenadas deben ser de ese mismo tipo (por ejemplo, Punto<Integer> o Punto<Double>). Esto impide crear un punto con coordenadas de tipos distintos, ya que el compilador generará un error si se intenta mezclar tipos incompatibles. De esta forma, se garantiza mayor coherencia en el uso de la clase.

Respecto a los métodos getX, en la solución sin genéricos el tipo de retorno es Number, lo que obliga a realizar conversiones (casting) si se quiere trabajar con un tipo concreto como Integer o Double. En cambio, en la solución con genéricos, el método devuelve T, es decir, el tipo específico con el que se instanció el punto. Esto elimina la necesidad de conversiones y proporciona mayor seguridad y claridad en el código, ya que el tipo es conocido en tiempo de compilación.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

Para eliminar la necesidad de `instanceof` y el downcasting, puede introducirse un parámetro genérico en la propia interfaz `Punto`, de forma que el tipo del punto con el que se calcula la distancia quede fijado en tiempo de compilación. Esto evita que un `Punto2D` pueda recibir accidentalmente un `Punto3D`, ya que el sistema de tipos lo impedirá directamente.

Una forma habitual de modelarlo es utilizar un tipo genérico acotado (patrón *self-bounded generics*), donde cada implementación concreta “se referencia a sí misma” como parámetro de tipo:

```java
public interface Punto<T extends Punto<T>> {
    double distanciaA(T p);
}
```

Con esta definición, cada clase concreta fija su propio tipo como argumento genérico, garantizando la coherencia entre receptores y parámetros del método.

Las implementaciones quedan entonces tipadas de forma segura sin necesidad de comprobaciones en tiempo de ejecución:

```java
public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2));
    }
}
```

De este modo, el compilador asegura que `distanciaA` solo reciba objetos del mismo tipo (`Punto2D` con `Punto2D`, `Punto3D` con `Punto3D`), eliminando completamente la necesidad de `instanceof` y de downcasting, y trasladando la corrección al sistema de tipos en lugar de a la lógica de ejecución.


## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

No, `List<String>` **no es subtipo de** `List<Object>`, aunque `String` sí sea subtipo de `Object`. En cambio, `String[]` **sí es subtipo de** `Object[]`. Esta diferencia se debe a cómo Java implementa la *subtipificación* en genéricos frente a arrays: los genéricos son *invariantes*, mientras que los arrays son *covariantes*.

En el caso de los arrays, la covarianza permite que `String[]` se trate como `Object[]`. Sin embargo, esto abre un problema en tiempo de ejecución. Por ejemplo:

```java
Object[] arr = new String[1];
arr[0] = 10; // Compila, pero falla en ejecución
```

Aquí el compilador permite la asignación porque `arr` es un `Object[]`, pero en realidad apunta a un `String[]`. En tiempo de ejecución se lanza un `ArrayStoreException`, ya que no se puede almacenar un `Integer` dentro de un array de `String`. Este es el precio de permitir covarianza en arrays: la verificación de tipos se retrasa hasta ejecución.

En cambio, en los genéricos ocurre invariancia: aunque `String` sea subtipo de `Object`, no se permite que `List<String>` sea subtipo de `List<Object>`. Esto evita el problema anterior en compilación:

```java
List<Object> lista = new ArrayList<String>(); // Error de compilación
```

Si esto se permitiera, podría ocurrir algo similar al caso de los arrays, donde se insertaría un objeto de tipo incorrecto en una lista supuestamente de `String`. Por eso Java decide ser más estricto con genéricos y eliminar ese riesgo en tiempo de compilación.

A partir de estos ejemplos, se definen las relaciones de varianza así: un tipo genérico es **covariante** si mantiene la relación de subtipado del tipo interno (si `A` es subtipo de `B`, entonces `G<A>` es subtipo de `G<B>`). Es **contravariante** si invierte esa relación (si `A` es subtipo de `B`, entonces `G<B>` es subtipo de `G<A>`). Finalmente, es **invariante** si no existe relación de subtipado entre `G<A>` y `G<B>` aunque `A` y `B` sí estén relacionados.

En Java, los arrays son covariantes (con riesgo en ejecución), mientras que los genéricos son invariantes por defecto. La contravarianza en genéricos solo se expresa explícitamente mediante comodines, como `? super T`, que permiten mayor flexibilidad controlada en el sistema de tipos.


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un *wildcard* (`?`) en Java es un comodín que representa un tipo desconocido dentro de un parámetro genérico. Su objetivo es flexibilizar el sistema de tipos cuando no es necesario conocer exactamente el tipo concreto, pero sí su relación con otro tipo (por ejemplo, si es un subtipo o un supertipo). Esto permite expresar covarianza y contravarianza de forma segura sin perder el control del sistema de tipos.

`List<? extends T>` se utiliza cuando se quiere **leer** elementos de una estructura cuyo tipo es T o subtipo de T. Es decir, permite tratar la lista de forma covariante: una `List<Integer>` es válida como `List<? extends Number>`. Sin embargo, no se pueden añadir elementos (salvo `null`), porque el compilador no puede garantizar el tipo real de la lista. En cambio, `List<? super T>` se utiliza cuando se quiere **escribir** elementos de tipo T o subtipos, es decir, permite tratar la lista de forma contravariante: una `List<Number>` o `List<Object>` puede aceptar `Integer`.

Un ejemplo de uso de `? extends` para calcular la suma de números sería:

```java id="ext1"
public static double suma(List<? extends Number> lista) {
    double total = 0;
    for (Number n : lista) {
        total += n.doubleValue();
    }
    return total;
}
```

En este caso, se permite pasar listas como `List<Integer>`, `List<Double>`, etc., porque todas son subtipos de `Number`. Solo se realiza lectura, por lo que no existe riesgo de insertar un tipo incorrecto.

Un ejemplo de uso de `? super` para añadir elementos sería:

```java id="sup1"
public static void agregarEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}
```

Aquí se garantiza que la lista puede almacenar `Integer` o cualquier supertipo (como `Number` u `Object`), por lo que es seguro insertar enteros. Sin embargo, al leer elementos solo se puede tratar como `Object`, ya que no se conoce el tipo concreto almacenado.
