<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a función es una variable que almacena la dirección de memoria de una función, permitiendo invocarla de manera indirecta. En C, las funciones no son valores en sí mismos, pero sí pueden ser referenciadas mediante punteros, lo que permite tratarlas como datos: pasarlas como argumentos, almacenarlas o seleccionarlas dinámicamente en tiempo de ejecución.

Este mecanismo resulta útil cuando se desea desacoplar la lógica que ejecuta una operación de la propia operación concreta, por ejemplo en algoritmos genéricos, callbacks o tablas de funciones. El tipo del puntero a función debe coincidir exactamente con la firma de la función a la que apunta.

A continuación se muestra un ejemplo en C donde se define una función que convierte una cadena a mayúsculas y se utiliza un puntero a dicha función:

```c
#include <stdio.h>
#include <ctype.h>

void aMayusculas(char *str) {
    for (int i = 0; str[i] != '\0'; i++) {
        str[i] = toupper(str[i]);
    }
}

int main() {
    char texto[] = "hola mundo";

    void (*aMayusculasPtr)(char *) = aMayusculas;

    aMayusculasPtr(texto);

    printf("%s\n", texto);

    return 0;
}
```

En este caso, `aMayusculasPtr` es el puntero a función que apunta a `aMayusculas`, y la invocación `aMayusculasPtr(texto)` ejecuta la función de forma indirecta, produciendo la conversión de la cadena a mayúsculas.

Las funciones son "ciudadanos de primera clase". Una función es un tipo más:
- Se puede asignar una variable.
- Se puede pasar como parámetro.
- Se puede devolver una función como retorno de otra.
- Closure. 
- Expresiones lambda (no tiene nombre).
- En lenguajes con comprobación estática de tipos: ¿Qué tipo tienen?

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda es una función anónima, es decir, una función que no tiene nombre explícito y que puede definirse directamente en una expresión. Este tipo de funciones se utilizan para representar comportamiento como si fuera un valor, lo que permite pasarlas como argumentos, almacenarlas en variables o devolverlas desde otras funciones.

Las lambdas son especialmente comunes en lenguajes con soporte funcional o multiparadigma, ya que facilitan la programación más declarativa y reducen la necesidad de definir métodos completos para operaciones simples. Suelen capturar variables del entorno (cierres o *closures*) y permiten expresar lógica de forma más compacta.

En JavaScript, una lambda puede definirse mediante una *arrow function*:

```javascript
let aMayusculas = (str) => str.toUpperCase();

console.log(aMayusculas("hola mundo"));
```

En este caso, `aMayusculas` es una variable que almacena una función anónima que recibe una cadena y devuelve su versión en mayúsculas, utilizando el método `toUpperCase()`.

En Java, las funciones lambda se representan mediante interfaces funcionales. Usando `Function<String, String>`, se puede escribir:

```java id="lambda1"
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {

        Function<String, String> aMayusculas = (str) -> str.toUpperCase();

        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

Aquí `aMayusculas` es una referencia a una función lambda que transforma una cadena a mayúsculas, y se invoca mediante el método `apply`, ya que `Function` define ese contrato funcional.

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El paradigma funcional es un modelo de programación en el que el cálculo se basa en la evaluación de funciones matemáticas, evitando en la medida de lo posible el uso de estado mutable y efectos secundarios. En este enfoque, las funciones se tratan como transformaciones de datos: reciben entradas y producen salidas sin depender de un contexto externo cambiante, lo que favorece la predictibilidad y la facilidad de razonamiento del código.

Lenguajes como Java a partir de la versión 8 se consideran multi-paradigma porque, además de su modelo orientado a objetos tradicional, incorporan características propias del paradigma funcional, como expresiones lambda, interfaces funcionales y operaciones sobre colecciones con estilo declarativo (por ejemplo, `map`, `filter` o `reduce`). Esto permite combinar clases y objetos con programación funcional dentro del mismo lenguaje, eligiendo el enfoque más adecuado según el problema.

Se dice que las funciones son *ciudadanos de primera clase* cuando pueden tratarse como cualquier otro valor del lenguaje. Esto implica que pueden asignarse a variables, pasarse como parámetros a otras funciones, devolverse como resultado de una función y almacenarse en estructuras de datos. En lenguajes tradicionales como C esto se logra mediante punteros a función, mientras que en lenguajes modernos como Java o JavaScript se realiza mediante lambdas o referencias funcionales.

Esta propiedad es fundamental para el paradigma funcional, ya que permite construir abstracciones de alto nivel basadas en comportamiento. En lugar de centrarse únicamente en datos y objetos, se puede manipular directamente la lógica como si fuera un dato más, lo que facilita la composición de funciones y la creación de programas más modulares y reutilizables.

Function <E,S>: apply(E): S
BiFunction<E1, E2, S>: apply(E1, E2): S
Supplier<T>: get(); T
Consumer<T>: acept(T): void
Predicate<T>: test(T): bool

## 4. Explica la sintaxis básica de una función lambda en Java.

Una función lambda en Java es una forma compacta de representar una implementación de una interfaz funcional (es decir, una interfaz con un único método abstracto). Su sintaxis se introdujo en Java 8 para permitir tratar el comportamiento como un valor, evitando la necesidad de clases anónimas completas cuando solo se necesita implementar un método.

La sintaxis básica de una lambda en Java sigue el patrón:

```java
(parámetros) -> expresión
```

o bien, si el cuerpo es más complejo:

```java
(parámetros) -> {
    // bloque de código
    return valor;
}
```

El símbolo `->` separa la lista de parámetros del cuerpo de la función. Si el compilador puede inferir el tipo de los parámetros, no es necesario declararlo explícitamente. Además, si solo hay un parámetro, los paréntesis pueden omitirse.

Un ejemplo simple sería una función que convierte una cadena a mayúsculas usando `Function<String, String>`:

```java id="lam_basic"
import java.util.function.Function;

Function<String, String> aMayusculas = (s) -> s.toUpperCase();
```

En este caso, `(s) -> s.toUpperCase()` es la expresión lambda: recibe un parámetro `s` y devuelve el resultado de aplicar `toUpperCase()`. Si el cuerpo tuviera más instrucciones, se usarían llaves y una sentencia `return` explícita.

Este mecanismo permite reducir código “boilerplate” asociado a clases anónimas y facilita el uso de programación funcional dentro de Java de forma más directa y legible.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

En el paradigma funcional, es habitual que una función se reciba como parámetro de otra función para aplicar transformaciones de forma genérica. En este caso, el método `transformar` actúa como una función de orden superior, ya que no solo trabaja con datos (`String`), sino también con comportamiento (otra función que transforma ese `String`).

La idea es separar el “qué se transforma” del “cómo se transforma”, de forma que `transformar` no dependa de una operación concreta como pasar a mayúsculas, sino que reciba esa lógica desde fuera y la ejecute dinámicamente.

En JavaScript, esto se puede expresar directamente pasando funciones como parámetros:

```javascript id="js_transform"
let aMayusculas = (str) => str.toUpperCase();

function transformar(texto, funcion) {
    return funcion(texto);
}

console.log(transformar("hola mundo", aMayusculas));
```

En este caso, `transformar` recibe una cadena y una función, y simplemente aplica la función recibida al texto. Esto permite reutilizar el mismo método con distintas transformaciones sin modificar su implementación.

En Java, el mismo concepto se implementa utilizando interfaces funcionales, como `Function<String, String>`:

```java id="java_transform"
import java.util.function.Function;

public class Main {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {

        Function<String, String> aMayusculas = s -> s.toUpperCase();

        System.out.println(transformar("hola mundo", aMayusculas));
    }
}
```

Aquí `transformar` recibe tanto el dato como la función transformadora, y la ejecuta mediante `apply`. Este enfoque permite desacoplar completamente la lógica de transformación del método que la ejecuta, favoreciendo reutilización y flexibilidad.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

El uso de funciones lambda permite definir comportamiento “al vuelo”, es decir, directamente en el punto donde se necesita, sin tener que crear una variable intermedia. Esto es especialmente útil cuando la función solo se va a utilizar una vez, como ocurre al pasar una transformación puntual a un método de orden superior como `transformar`.

En este caso, se puede invocar `transformar` pasando directamente una lambda que invierta la cadena. La inversión se realiza recorriendo el `String` en orden inverso y construyendo un nuevo resultado. La función se define en el propio argumento de la llamada, sin necesidad de almacenarla previamente.

En JavaScript, la invocación quedaría así:

```javascript id="js_reverse_lambda"
function transformar(texto, funcion) {
    return funcion(texto);
}

console.log(transformar("hola mundo", (str) => str.split("").reverse().join("")));
```

Aquí la lambda `(str) => str.split("").reverse().join("")` se define directamente en la llamada a `transformar`, convirtiendo la cadena en un array de caracteres, invirtiéndolo y volviéndolo a unir. Esto evita crear una variable intermedia como `aInvertir`.

En Java, el mismo concepto se expresa con una lambda inline de tipo `Function<String, String>`:

```java id="java_reverse_lambda"
import java.util.function.Function;

public class Main {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {

        System.out.println(
            transformar("hola mundo", s ->
                new StringBuilder(s).reverse().toString()
            )
        );
    }
}
```

En este caso, la función lambda `s -> new StringBuilder(s).reverse().toString()` se define directamente como argumento de `transformar`. Se utiliza `StringBuilder` para invertir la cadena de forma eficiente, y el resultado se devuelve inmediatamente, manteniendo el estilo funcional y evitando cualquier definición previa de la función.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un *closure* (cierre) es una función que “captura” variables del entorno donde ha sido definida, de forma que puede seguir accediendo a ellas incluso cuando se ejecuta fuera de ese contexto inmediato. En el caso de las funciones lambda, esto implica que pueden referenciar variables locales del método en el que se crean, siempre que dichas variables sean efectivamente finales o “como si fueran finales” (*effectively final*) en Java.

Este mecanismo permite que una lambda no dependa únicamente de sus parámetros, sino también de información externa al momento de su creación. De esta forma, la función “recuerda” el entorno léxico donde fue definida, lo que es la base del concepto de cierre.

En el siguiente ejemplo en Java, se define una variable local fuera de la lambda y luego se accede a ella dentro de la función anónima para concatenarla con la cadena de entrada:

```java id="closure1"
import java.util.function.Function;

public class Main {

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }

    public static void main(String[] args) {

        String sufijo = "!!!";

        Function<String, String> concatenar = s -> s + sufijo;

        System.out.println(transformar("hola mundo", concatenar));
    }
}
```

En este caso, la lambda `s -> s + sufijo` forma un closure porque utiliza la variable local `sufijo`, definida fuera de su cuerpo. Aunque `sufijo` no es un parámetro de la función, la lambda puede acceder a ella gracias al mecanismo de captura del entorno.

Esto es posible porque Java no copia la variable, sino que permite su acceso siempre que no sea modificada después de su captura. De este modo se garantiza consistencia y seguridad en el acceso a variables externas dentro de expresiones funcionales.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La principal diferencia entre una función lambda y un puntero a función en C está en que una lambda no es solo una referencia a una función, sino una *estructura de comportamiento que puede capturar su entorno*. En C, un puntero a función únicamente almacena la dirección de una función global o estática, sin ninguna información adicional. En cambio, una lambda puede llevar asociado estado (variables del entorno), lo que la convierte en algo más expresivo.

En C, el puntero a función es puramente “nominal”: apunta a código ejecutable, pero no puede acceder a variables locales del contexto donde se usa salvo que se le pasen explícitamente como parámetros. Por ejemplo, no existe el concepto de closure en C estándar, por lo que no hay captura automática de variables externas.

En Java o JavaScript, una lambda sí puede capturar variables del contexto léxico donde se define, formando un *closure*. Esto permite que la función “recuerde” el entorno incluso cuando se ejecuta en otro lugar o momento. Esa capacidad no existe en punteros a función en C, que son estáticos en su comportamiento.

Otra diferencia importante es que las lambdas forman parte de un modelo de tipado más alto. En Java, una lambda implementa una interfaz funcional y se integra con el sistema de tipos genérico, mientras que en C el puntero a función es simplemente una dirección con una firma fija. Esto hace que las lambdas sean más seguras y flexibles, mientras que los punteros a función son más simples pero también más limitados.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

Una función que devuelve otra función es una función de orden superior. En este caso, `crearDescuento` no calcula directamente un resultado, sino que construye y devuelve una nueva función que aplica un descuento a cualquier valor que reciba. Esto es típico del paradigma funcional, donde el comportamiento se puede generar dinámicamente a partir de parámetros.

Se utiliza `Function<Double, Double>` porque la función de descuento recibe un número (el precio o cantidad) y devuelve otro número ya transformado. El porcentaje de descuento se fija en el momento de creación de la función, pero la aplicación del descuento ocurre después, cuando la función devuelta se invoca.

En Java, la implementación sería:

```java id="discount1"
import java.util.function.Function;

public class Main {

    public static Function<Double, Double> crearDescuento(double porcentaje) {
        return precio -> precio - (precio * porcentaje / 100.0);
    }

    public static void main(String[] args) {

        Function<Double, Double> descuento10 = crearDescuento(10);
        Function<Double, Double> descuento20 = crearDescuento(20);

        double precio = 100.0;

        System.out.println(descuento10.apply(precio)); // 90.0
        System.out.println(descuento20.apply(precio)); // 80.0
    }
}
```

Aquí `crearDescuento` devuelve una lambda que utiliza la variable `porcentaje`, aunque esta variable ya ha salido del ámbito de la función. Esto es posible gracias al concepto de closure.

El closure en este caso consiste en que cada función de descuento “captura” el valor de `porcentaje` en el momento en que es creada. Aunque `crearDescuento` ya ha terminado su ejecución, la lambda sigue teniendo acceso a ese valor. Esto permite que `descuento10` y `descuento20` sean funciones distintas, cada una con su propio entorno capturado, manteniendo el porcentaje asociado sin necesidad de almacenarlo en una estructura adicional.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una interfaz funcional en Java es una interfaz que contiene **exactamente un método abstracto**. Puede contener múltiples métodos `default` o `static`, así como métodos heredados de la clase `Object` (como `equals`, `toString` o `hashCode`), ya que estos no cuentan como métodos abstractos a efectos de la definición de interfaz funcional. Este único método abstracto define el contrato que podrá ser implementado mediante una expresión lambda o una referencia a método, permitiendo tratar funciones como ciudadanos de primera clase en el lenguaje. El propósito fundamental de las interfaces funcionales es habilitar la programación funcional dentro del paradigma orientado a objetos de Java, proporcionando un sistema de tipos seguro para las expresiones lambda.

El requisito principal es tener un único método abstracto, aunque Java proporciona la anotación opcional `@FunctionalInterface` para marcar explícitamente que una interfaz está diseñada para ser funcional. Esta anotación no es obligatoria, pero si se utiliza, el compilador generará un error si la interfaz no cumple con el requisito de tener exactamente un método abstracto. Además, una interfaz funcional puede heredar métodos abstractos de otras interfaces; si la suma total de métodos abstractos heredados es uno, sigue siendo funcional. Ejemplos comunes de interfaces funcionales en la API estándar de Java son `Consumer<T>` (recibe un valor y no devuelve nada), `Function<T,R>` (transforma un valor en otro), `Predicate<T>` (prueba una condición y devuelve un booleano) y `Supplier<T>` (proporciona un valor sin recibir argumentos). Sin estas interfaces funcionales, las expresiones lambda no podrían existir, ya que necesitan un tipo al que asociarse.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

A continuación se define la interfaz funcional `Transformador`, que cumple con el requisito de tener exactamente un método abstracto. Dicho método recibe una cadena de texto y devuelve otra cadena transformada. Para mayor claridad y seguridad, se utiliza la anotación `@FunctionalInterface`, que permite al compilador verificar que la interfaz cumple con las condiciones para ser utilizada con expresiones lambda:

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String texto);
}
```

Esta interfaz funcional puede ser implementada de diversas formas. Por ejemplo, mediante una expresión lambda que convierta el texto a mayúsculas, que invierta el orden de los caracteres o que elimine los espacios en blanco. Al ser una interfaz funcional, el compilador permite asignar directamente una expresión lambda a una variable de tipo `Transformador`, lo que facilita la escritura de código más conciso y expresivo. A continuación se muestra un ejemplo de uso:

```java
public class EjemploTransformador {
    public static void main(String[] args) {
        // Implementación con lambda que convierte a mayúsculas
        Transformador mayusculas = texto -> texto.toUpperCase();
        
        // Implementación con lambda que invierte el texto
        Transformador inversor = texto -> new StringBuilder(texto).reverse().toString();
        
        // Uso de los transformadores
        String original = "Hola Mundo";
        System.out.println(mayusculas.transformar(original));  // HOLA MUNDO
        System.out.println(inversor.transformar(original));    // odnuM aloH
    }
}
```

La interfaz `Transformador` es un ejemplo sencillo pero ilustrativo de cómo Java permite tratar comportamientos como datos, pasándolos como argumentos a métodos o almacenándolos en variables. Gracias a este enfoque, es posible escribir código más modular y reutilizable, delegando la lógica de transformación en objetos funcionales sin necesidad de crear clases anónimas verbosas. Este patrón es ampliamente utilizado en la API de Streams de Java, donde operaciones como `map`, `filter` o `reduce` reciben interfaces funcionales para personalizar su comportamiento.

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

A continuación se redefine la interfaz funcional `Transformador` utilizando genéricos, de modo que pueda transformar un tipo de entrada `T` en un tipo de salida `R`. Esta generalización permite reutilizar el mismo concepto para múltiples propósitos, manteniendo la seguridad de tipos que caracteriza a Java. La interfaz sigue siendo funcional al tener un único método abstracto, y se mantiene la anotación `@FunctionalInterface` para garantizar que el compilador valide su correcta definición:

```java
@FunctionalInterface
public interface Transformador<T, R> {
    R transformar(T entrada);
}
```

Un ejemplo concreto de uso de esta interfaz genérica es un transformador que convierte un número de tipo `Double` en su valor entero redondeado más cercano, produciendo un `Integer`. La expresión lambda correspondiente utiliza el método estático `Math.round()`, que devuelve `long`, por lo que es necesario aplicar un casting explícito a `int` o utilizar `Long.intValue()`. Este transformador puede ser almacenado en una variable, pasado como argumento a un método o aplicado sobre una colección de datos, demostrando la flexibilidad que aporta la combinación de genéricos e interfaces funcionales:

```java
public class EjemploTransformadorGenerico {
    public static void main(String[] args) {
        // Transformador que redondea un Double a Integer
        Transformador<Double, Integer> redondeador = numero -> (int) Math.round(numero);
        
        // Aplicación del transformador
        Double valor = 3.67;
        Integer resultado = redondeador.transformar(valor);
        
        System.out.println("Original: " + valor);    // 3.67
        System.out.println("Redondeado: " + resultado); // 4
        
        // También puede usarse con Streams
        java.util.List<Double> precios = java.util.Arrays.asList(10.49, 20.99, 30.25);
        precios.stream()
               .map(redondeador::transformar)
               .forEach(System.out::println);  // 10, 21, 30
    }
}
```

Esta versión genérica de `Transformador` es análoga a la interfaz `Function<T, R>` que proporciona la API estándar de Java en el paquete `java.util.function`. De hecho, `Function<T, R>` tiene el mismo propósito: transformar un valor de tipo `T` en un valor de tipo `R`. La ventaja de definir la propia interfaz funcional, aunque no sea necesaria para este caso concreto, reside en comprender el mecanismo subyacente y en poder crear abstracciones específicas del dominio que aporten semántica adicional al código (por ejemplo, `Serializador<T>` o `Validador<T>`). Los genéricos permiten que la interfaz sea reutilizable para cualquier par de tipos, manteniendo la expresividad y la seguridad que caracterizan al lenguaje Java.

  ## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Efectivamente, la interfaz genérica `Function<T, R>` que proporciona Java en el paquete `java.util.function` es exactamente equivalente al `Transformador` definido anteriormente. Su método abstracto es `R apply(T t)`. Java incluye un conjunto de interfaces funcionales predefinidas para cubrir los casos más comunes de programación funcional, clasificadas según el número y tipo de parámetros que reciben y el valor que devuelven. Las principales familias son: `Function<T,R>` (recibe un parámetro y devuelve un resultado), `Consumer<T>` (recibe un parámetro y no devuelve nada), `Supplier<T>` (no recibe parámetros y devuelve un resultado), `Predicate<T>` (recibe un parámetro y devuelve un booleano) y `UnaryOperator<T>` (especialización de Function donde el tipo de entrada y salida coinciden). También existen versiones especializadas para tipos primitivos como `IntFunction<R>`, `ToIntFunction<T>`, `DoubleConsumer` o `LongSupplier`, que evitan el auto-boxing y mejoran el rendimiento cuando se trabaja con tipos básicos.

Las interfaces funcionales se distinguen por la aridad de sus métodos y los tipos que manejan. `BiFunction<T,U,R>` recibe dos parámetros y devuelve un resultado, mientras que `BiConsumer<T,U>` recibe dos parámetros y no devuelve nada. `BinaryOperator<T>` es una especialización de `BiFunction` donde los dos parámetros y el resultado son del mismo tipo. Para la generación de valores, existen `IntSupplier`, `DoubleSupplier` y `BooleanSupplier`. Para la transformación de tipos primitivos, se dispone de interfaces como `IntToDoubleFunction` o `LongToIntFunction`. Esta amplia variedad permite expresar operaciones funcionales de forma muy precisa y eficiente, evitando la creación de tipos intermedios innecesarios. A continuación se muestra una tabla resumen con las interfaces funcionales más utilizadas:

| Interfaz | Método abstracto | Propósito |
|----------|-----------------|-----------|
| `Function<T,R>` | `R apply(T t)` | Transformar un valor de tipo T en otro de tipo R |
| `Consumer<T>` | `void accept(T t)` | Realizar una acción con un valor sin devolver resultado |
| `Supplier<T>` | `T get()` | Proveer un valor sin recibir argumentos |
| `Predicate<T>` | `boolean test(T t)` | Evaluar una condición sobre un valor |
| `UnaryOperator<T>` | `T apply(T t)` | Aplicar una operación que devuelve el mismo tipo |
| `BinaryOperator<T>` | `T apply(T t1, T t2)` | Combinar dos valores del mismo tipo en uno |
| `BiFunction<T,U,R>` | `R apply(T t, U u)` | Transformar dos valores en un resultado |

El conocimiento de estas interfaces funcionales es fundamental para trabajar con la API de Streams y con las expresiones lambda en Java. En lugar de definir interfaces personalizadas como `Transformador`, se recomienda utilizar las predefinidas siempre que sea posible, ya que aportan semántica reconocible universalmente y se integran perfectamente con el resto de la biblioteca estándar. Por ejemplo, el redondeo de `Double` a `Integer` se expresaría como `Function<Double, Integer> redondeador = d -> (int) Math.round(d);`, evitando así la creación de un tipo adicional innecesario. Solo cuando se necesita un nombre de dominio específico que aporte claridad al código (como `ValidadorDeUsuario` o `SerializadorDeDocumento`) tiene sentido definir interfaces funcionales personalizadas.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método `forEach` de la interfaz `List` es una operación terminal que recibe un `Consumer<? super T>` y lo aplica a cada elemento de la lista. Un `Consumer` es una interfaz funcional que representa una operación que acepta un argumento y no devuelve ningún resultado (efecto lateral). Esta aproximación funcional permite expresar el recorrido de una colección de manera más declarativa que el bucle `for` tradicional, separando claramente la lógica de iteración de la acción a realizar sobre cada elemento.

A continuación se muestra cómo recorrer una lista de `Integer` utilizando `forEach` y mostrar un mensaje solo para los números positivos. La expresión lambda que se pasa al método `forEach` contiene la lógica de verificación y la impresión por consola:

```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-3, 5, 0, -1, 8, -4, 2);
        
        // Versión funcional con forEach
        numeros.forEach(numero -> {
            if (numero > 0) {
                System.out.println(numero + " es positivo");
            }
        });
    }
}
```

La salida del programa sería:
```
5 es positivo
8 es positivo
2 es positivo
```

Para comparar con el enfoque imperativo tradicional, el mismo comportamiento con un bucle `for-each` clásico requeriría más código boilerplate, donde la mecánica de iteración y la acción están mezcladas:

```java
// Versión imperativa tradicional
for (Integer numero : numeros) {
    if (numero > 0) {
        System.out.println(numero + " es positivo");
    }
}
```

El método `forEach` también puede combinarse con referencias a métodos cuando la lógica ya está encapsulada en un método existente. Por ejemplo, si se tiene un método `void mostrarSiPositivo(Integer n)`, se podría escribir `lista::mostrarSiPositivo`. La ventaja principal del enfoque funcional es que permite componer operaciones y pasar comportamientos como argumentos, lo que resulta especialmente útil cuando la acción a realizar no está fijada de antemano y puede variar según el contexto. Internamente, `forEach` itera sobre la lista y aplica el `Consumer` recibido, sin que el programador deba preocuparse por los detalles de la iteración. Este estilo de programación, que trata las operaciones como ciudadanos de primera clase, es característico de los lenguajes funcionales y ha sido hábilmente integrado en Java a partir de su versión 8.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

El uso de `Consumer<? super T>` en lugar de `Consumer<T>` responde al principio **PECS** ("Producer Extends, Consumer Super"), un mnemotécnico para recordar cuándo usar `? extends T` y cuándo usar `? super T` en genéricos con wildcards. Este principio establece que si una estructura produce elementos de tipo T (los proporciona hacia afuera), se debe declarar como `? extends T`; si consume elementos de tipo T (los recibe para procesarlos), se debe declarar como `? super T`. En el caso de `Consumer`, su método `accept` recibe un argumento de tipo T, por lo que es un **consumidor** de T. Al declarar `Consumer<? super T>`, se indica que puede aceptar cualquier `Consumer` que sea capaz de consumir elementos de tipo T o de algún supertipo de T. Esto proporciona mayor flexibilidad: un `Consumer<Number>` puede procesar tanto `Number` como `Integer` o `Double` (ya que estos son subtipos de Number), permitiendo pasar consumidores más genéricos al método `forEach`. Si se declarase como `Consumer<T>`, solo se aceptarían consumidores exactamente del tipo T, reduciendo la reutilización. Por ejemplo, si se tiene una `List<Integer>` y un `Consumer<Number>` que imprime números, con `Consumer<? super Integer>` se puede pasar dicho consumidor, mientras que con `Consumer<Integer>` no sería posible.

Aplicando PECS al ejemplo del transformador, si se quiere definir un método que transforme una lista de elementos mediante una función y devuelva una nueva lista con los resultados, se debe considerar la dirección del flujo de tipos. La función transformadora **produce** un resultado de tipo R a partir de un valor de tipo T, por lo tanto, en la posición de producción (el tipo de salida de la función), se debe usar `? extends R`. Sin embargo, en la firma típica del método `map` (como el de Stream), se declara como `Function<? super T, ? extends R>`: el primer wildcard `? super T` indica que la función puede consumir cualquier supertipo de T (si se tiene una función que trabaja con `Number`, puede aplicarse a una lista de `Integer`), y el segundo wildcard `? extends R` indica que la función puede producir cualquier subtipo de R (por ejemplo, una función que devuelve `Integer` puede ser usada donde se espera `Number`). Si se invierte el uso de wildcards violando PECS, se perdería flexibilidad. Para ilustrarlo, un método `transformarLista(List<T> lista, Function<T, R> funcion)` es menos flexible que `transformarLista(List<? extends T> lista, Function<? super T, ? extends R> funcion)`. En el primer caso, no se podría pasar una `List<Integer>` con una `Function<Number, Double>`; en el segundo, sí, porque `Integer` es subtipo de `Number` (producido por `? super T` en el consumo) y `Double` es subtipo de `Number` (producido por `? extends R` en la salida). Este es un ejemplo de cómo PECS guía un diseño de API más reutilizable y expresivo en Java.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

A continuación se muestra cómo obtener referencias a métodos en JavaScript y Java, utilizando una clase `Persona` con un método `saludar`. En ambos lenguajes, las referencias a métodos permiten tratar funciones como ciudadanos de primera clase, pudiendo almacenarlas en variables, pasarlas como argumentos o invocarlas posteriormente.

## Ejemplo en JavaScript

JavaScript, al ser un lenguaje con funciones de primera clase, permite asignar directamente métodos de objetos a variables manteniendo el contexto de `this` mediante `bind` o utilizando funciones flecha. El siguiente ejemplo muestra cómo obtener una referencia al método `saludar` de una instancia de `Persona` y cómo invocarlo desde una variable:

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

// Crear una instancia
const persona = new Persona("Ana");

// Obtener referencia al método saludar (manteniendo el contexto con bind)
const referenciaSaludar = persona.saludar.bind(persona); 

// O usando una función flecha que preserva el contexto
const referenciaSaludar2 = () => persona.saludar();

// Invocar a través de la referencia
referenciaSaludar();  // Hola, soy Ana
referenciaSaludar2(); // Hola, soy Ana
```

## Ejemplo en Java

Java implementa referencias a métodos mediante el operador `::`, que permite referenciar métodos de instancias específicas (referencia a método de un objeto concreto), métodos estáticos de clases, o métodos de instancia de cualquier tipo (contexto de recepción). Para referenciar el método `saludar` de una instancia particular de `Persona`, se utiliza la sintaxis `objeto::nombreMetodo`. La variable que almacena la referencia debe ser de un tipo de interfaz funcional compatible con la firma del método. A continuación se muestra el código Java completo:

```java
import java.util.function.Supplier;

class Persona {
    private String nombre;
    
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

public class Main {
    public static void main(String[] args) {
        Persona persona = new Persona("Ana");
        
        // Referencia al método saludar de la instancia específica
        Runnable referenciaSaludar = persona::saludar;
        
        // Invocar a través de la referencia (Runnable.run())
        referenciaSaludar.run();  // Hola, soy Ana
    }
}
```

## Explicación de las diferencias fundamentales

En **JavaScript**, la asignación directa `const ref = persona.saludar` perdería el contexto de `this`, por lo que es necesario usar `bind` para fijar el objeto receptor. Esto se debe a que en JavaScript, `this` se determina dinámicamente según cómo se invoca la función, no por dónde fue definida. En **Java**, no existe tal problema porque las referencias a métodos de instancia capturan implícitamente el objeto receptor en el momento de crear la referencia. La interfaz funcional `Runnable` se utiliza porque `saludar` no recibe parámetros y devuelve `void`, coincidiendo con el método `run()`. Si el método tuviera parámetros, se usaría `Consumer<T>` o `Function<T,R>`. Java impone un sistema de tipos estático, por lo que la referencia a método debe ser asignada a una variable cuyo tipo de interfaz funcional sea compatible con la firma del método referenciado. JavaScript, al ser de tipado dinámico, no impone esta restricción. Ambos lenguajes demuestran cómo el paradigma funcional permite tratar métodos como valores, aunque con enfoques diferentes derivados de la naturaleza estática o dinámica de sus sistemas de tipos.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

En Java, el operador `::` permite cuatro tipos diferentes de referencias a métodos. El primer tipo son las **referencias a métodos estáticos**, cuya sintaxis es `Clase::metodoEstatico`. Son útiles cuando se dispone de un método estático cuya firma coincide con la interfaz funcional deseada. El segundo tipo son las **referencias a constructores**, con sintaxis `Clase::new`, que permiten crear objetos de manera funcional, delegando en el constructor apropiado según los parámetros que reciba la interfaz funcional. El tercer tipo son las **referencias a métodos de instancia sobre una instancia concreta**, con sintaxis `instancia::metodoInstancia`, que capturan un objeto específico y permiten invocar su método en cualquier momento. El cuarto tipo son las **referencias a métodos de instancia sobre cualquier instancia del tipo**, con sintaxis `Clase::metodoInstancia`, donde el método se aplicará al primer parámetro de la interfaz funcional cuando este sea del tipo de la clase. Este último tipo es especialmente útil para transformar un objeto aplicándole un método sin necesidad de una lambda explícita.

A continuación se muestran ejemplos de los cuatro tipos de referencias a métodos en Java, utilizando una clase `Persona` con atributos y métodos, y una clase `Main` para demostrar su uso con interfaces funcionales predefinidas:

```java
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Supplier;
import java.util.Arrays;
import java.util.List;

class Persona {
    private String nombre;
    
    public Persona() {
        this.nombre = "Anónimo";
    }
    
    public Persona(String nombre) {
        this.nombre = nombre;
    }
    
    public static Persona crearPorDefecto() {
        return new Persona("Por Defecto");
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
    
    @Override
    public String toString() {
        return "Persona{" + nombre + "}";
    }
}

public class Main {
    public static void main(String[] args) {
        // 1. Referencia a método estático: Clase::metodoEstatico
        Supplier<Persona> proveedor = Persona::crearPorDefecto;
        Persona p1 = proveedor.get();
        System.out.println(p1);  // Persona{Por Defecto}
        
        // 2. Referencia a constructor: Clase::new (constructor sin parámetros)
        Supplier<Persona> constructorVacio = Persona::new;
        Persona p2 = constructorVacio.get();
        System.out.println(p2);  // Persona{Anónimo}
        
        // Referencia a constructor con parámetro: Function<String, Persona>
        Function<String, Persona> constructorConNombre = Persona::new;
        Persona p3 = constructorConNombre.apply("Carlos");
        System.out.println(p3);  // Persona{Carlos}
        
        // 3. Referencia a método de instancia sobre una instancia concreta
        Persona ana = new Persona("Ana");
        Runnable saludarAna = ana::saludar;  // Captura la instancia 'ana'
        saludarAna.run();  // Hola, soy Ana
        
        // 4. Referencia a método de instancia sobre cualquier instancia del tipo
        // Equivalente a (persona) -> persona.getNombre()
        Function<Persona, String> obtenerNombre = Persona::getNombre;
        System.out.println(obtenerNombre.apply(p3));  // Carlos
        
        // Uso práctico con Stream: transformar cada persona en su nombre
        List<Persona> personas = Arrays.asList(p1, p2, p3);
        personas.stream()
                .map(Persona::getNombre)  // Referencia a método sobre cualquier instancia
                .forEach(System.out::println);  // Referencia a método sobre instancia concreta (System.out)
    }
}
```

**Explicación de cada tipo:**

1. **Método estático**: `Persona::crearPorDefecto` se asigna a un `Supplier<Persona>` porque el método no recibe parámetros y devuelve una `Persona`. Al invocar `get()` se ejecuta el método estático.

2. **Constructor**: `Persona::new` tiene dos variantes según la interfaz funcional. Con `Supplier<Persona>` se invoca el constructor sin parámetros; con `Function<String, Persona>` se invoca el constructor que recibe un `String`. El compilador infiere qué constructor utilizar según el contexto.

3. **Método de instancia sobre instancia concreta**: `ana::saludar` captura el objeto `ana` concreto. La interfaz `Runnable` es compatible porque `saludar` no recibe parámetros y devuelve `void`. Al ejecutar `run()`, se invoca el método sobre la instancia capturada.

4. **Método de instancia sobre cualquier instancia**: `Persona::getNombre` significa que el método `getNombre` se aplicará al primer argumento que reciba la interfaz funcional. En `Function<Persona, String>`, el primer parámetro es la `Persona` sobre la que se invoca `getNombre()`, y el resultado es el `String` devuelto. Es equivalente a la lambda `(Persona p) -> p.getNombre()`.

Estas referencias a métodos son azúcar sintáctico sobre las expresiones lambda que hacen el código más conciso y legible, especialmente cuando se trabaja con Streams y APIs funcionales. Cada tipo de referencia tiene su utilidad específica: las referencias a constructores son ideales para fábricas funcionales, las referencias a métodos estáticos para operaciones auxiliares, y las referencias a métodos de instancia (tanto sobre instancias concretas como sobre cualquier instancia) permiten extraer o transformar datos de objetos de forma muy expresiva.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

A continuación se muestra cómo ordenar una lista de `Persona` utilizando `Collections.sort` con expresiones lambda y con `Comparator`, aplicando los criterios solicitados: primero por edad, y en caso de empate, por orden alfabético del nombre.

## Clase Persona

```java
public class Persona {
    private String nombre;
    private int edad;
    
    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    
    public String getNombre() {
        return nombre;
    }
    
    public int getEdad() {
        return edad;
    }
    
    @Override
    public String toString() {
        return nombre + " (" + edad + " años)";
    }
}
```

## Versión 1: Comparación manual dentro de la lambda

En esta versión, la expresión lambda implementa la lógica completa de comparación directamente:

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class OrdenacionLambda {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Carlos", 25),
            new Persona("Ana", 30),
            new Persona("Beatriz", 25),
            new Persona("David", 20)
        );
        
        System.out.println("Original: " + personas);
        
        // Ordenar con lambda manual
        Collections.sort(personas, (p1, p2) -> {
            int comparacionEdad = Integer.compare(p1.getEdad(), p2.getEdad());
            if (comparacionEdad != 0) {
                return comparacionEdad;
            }
            return p1.getNombre().compareTo(p2.getNombre());
        });
        
        System.out.println("Ordenada: " + personas);
    }
}
```

**Salida:**
```
Original: [Carlos (25 años), Ana (30 años), Beatriz (25 años), David (20 años)]
Ordenada: [David (20 años), Beatriz (25 años), Carlos (25 años), Ana (30 años)]
```

## Versión 2: Empleando Comparator y sus métodos encadenados

Utilizando `Comparator` y sus métodos estáticos y por defecto, se obtiene una solución más declarativa y reutilizable:

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class OrdenacionComparator {
    public static void main(String[] args) {
        List<Persona> personas = Arrays.asList(
            new Persona("Carlos", 25),
            new Persona("Ana", 30),
            new Persona("Beatriz", 25),
            new Persona("David", 20)
        );
        
        System.out.println("Original: " + personas);
        
        // Construir Comparator encadenado
        Comparator<Persona> comparador = Comparator
            .comparingInt(Persona::getEdad)
            .thenComparing(Persona::getNombre);
        
        Collections.sort(personas, comparador);
        
        System.out.println("Ordenada: " + personas);
    }
}
```

**Salida:**
```
Original: [Carlos (25 años), Ana (30 años), Beatriz (25 años), David (20 años)]
Ordenada: [David (20 años), Beatriz (25 años), Carlos (25 años), Ana (30 años)]
```

## Explicación de las diferencias

**Versión manual con lambda**: La lógica de comparación está explícitamente escrita dentro de la lambda. Primero se compara la edad usando `Integer.compare`, que devuelve un entero negativo, cero o positivo. Si la edad es diferente, ese resultado se devuelve directamente. Si las edades son iguales, se procede a comparar los nombres alfabéticamente con `compareTo`. Este enfoque es más verboso y propenso a errores si se olvida gestionar correctamente los casos de empate.

**Versión con `Comparator`**: Se utiliza el método estático `comparingInt` que recibe una `Function` que extrae la clave de ordenación (en este caso, la edad). El resultado es un `Comparator` que ordena según esa clave. Luego, el método `thenComparing` encadena una segunda clave de ordenación (el nombre) para resolver empates. Este estilo es más declarativo, legible y menos propenso a errores. Además, los métodos de `Comparator` están optimizados y manejan correctamente casos como valores nulos si se usan las versiones adecuadas (por ejemplo, `nullsFirst` o `nullsLast`).

Ambas versiones producen el mismo resultado: la lista queda ordenada primero por edad ascendente, y dentro de cada grupo de edad, por orden alfabético ascendente del nombre. La segunda versión es la más recomendada en la práctica porque expresa claramente la intención de "ordenar por edad y luego por nombre" sin necesidad de escribir la lógica de comparación manualmente, además de que es más fácil de modificar (por ejemplo, añadiendo un tercer criterio de ordenación es tan simple como agregar otro `thenComparing`).
