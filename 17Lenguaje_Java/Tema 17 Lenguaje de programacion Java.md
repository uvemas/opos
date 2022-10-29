# Tema 17 Lenguaje de programación Java: sintaxis ,tipos de datos, operadores, estructuras de control

## Get Started with Java

[Get started](https://www.baeldung.com/get-started-with-java-series)

[Back to basics](https://www.baeldung.com/java-tutorial)

Muy buena introducción al lenguaje Java.
## Basic syntax

[Baeldung](https://www.baeldung.com/java-syntax)

Primitive types are the basic data types that store simple data.

Reference types are objects that contain references to values and/or other objects.

An identifier is a name of any length, consisting of letters, digits, underscore, and dollar sign (cannot start with a 
number).

The main difference *in representing literal values* of char and String is the number of quotes that surrounds the 
values.

Arrays declaration:

~~~ java
type[] identifier = new type[length] // array indexes start at zero
~~~

The basic unit of a Java program is a Class.

For a Class to be executable, it must have a main method. The main method signifies the entry point of the program.
Executable means it can be executed via the `java` command.

## Java primitives

[Baeldung](https://www.baeldung.com/java-primitives)

The eight primitives defined in Java are:

- integers: byte, short, int, long. Default value: 0
- floating numbers: float, double. Default value: 0.0
- boolean: boolean (1 bit but padded to 8 bits): Default value: false
- character: char (16 bits). Default value: '/u0000'

Default type of floating numbers is `double` so

~~~ java
// trailing f is mandatory
float pi = "3.14f"
~~~

Primitive types aren't considered objects and represent raw values. They're stored directly on the 
[stack](https://www.guru99.com/stack-vs-heap.html).

The simplest primitive data type is boolean. It can contain only two values: true or false. It stores its value in a 
single bit. However, for convenience, Java pads the value and stores it in a single byte.

Overflow: The primitive data types have size limits. But what happens if we try to store a value that's larger than the 
maximum value? We run into a situation called *overflow*. When an integer overflows, it rolls over to the minimum value 
and begins counting up from there. Floating point numbers overflow by returning `Infinity`.

Underflow is the same issue except it involves storing a value smaller than the minimum value. When the numbers 
underflow, they return 0.0.
## Operators

[Reference](https://www.javatpoint.com/java-operator-precedence)

Operators in Java can be grouping, array indexing, member selecting, arithmetic, bitwise, relational and logical.

### Precedence

Precedence is the priority for grouping different types of operators with their operands. It is meaningful only if an expression has 
more than one operator with higher or lower precedence. The operators having higher precedence are evaluated first. If we want to 
evaluate lower precedence operators first, we must group operands by using parentheses and then evaluate.

### Associtivity

We must follow associativity if an expression has more than two operators of the same precedence. In such a case, an expression can be 
solved either left-to-right or right-to-left, accordingly.

In Java all operators have left-to-right associativity except:

- unary operators
- assignment operators: =, +=, -=, *=, /=, %=

### Arithmetic operators

We can perform all standard arithmetic operations on ints. Just be aware that decimal values will be chopped off when performing these 
on integers.

For instance the `/` operator makes an integer division if both operands are integer and a floating point division if any of the 
operands is a float number.

### The increment/decrement operators

The increment/decrement operators can be applied before (prefix) or after (postfix) the operand. The code `result++;` and `++result;` 
will both end in `result` being incremented by one. The only difference is that the prefix version (`++result`) evaluates to the i
ncremented value, whereas the postfix version (`result++`) evaluates to the original value. If you are just performing a simple 
increment/decrement, it doesn't really matter which version you choose. But if you use this operator in part of a larger expression, 
the one that you choose may make a significant difference.

~~~ java
int a = 3;
int result = ++a; // result = 4 and a = 4
result = a--; // result = 4 and a = 3
~~~
## Exceptions

Exceptions can be:

- checked/situational: is typically a way to say that something external to our code (on the network, filesystem, database...) failed
- unchecked/runtime: some unexpected usage makes the code fail or something is wrong in the logic of the code

`IOException` is a checked exception.

`ArrayIndexOutOfBoundsException`, `ClassCastException`, `IllegalArgumentException`, `IllegalStateException`, `NullPointerException`,
`NumberFormatException` are unchecked exceptions.

We *must* handle the checked exceptions, and we *may* handle the unchecked ones.

A **risky method** is a method that throws a checked exception. It is risky because as the method does not handle the exception by 
itself,passing it up the call stack instead, then anyone that calls the method needs to handle it!


~~~ java
public int getPlayerScore(String playerFile)
  throws FileNotFoundException {
    Scanner contents = null;
    try {
        contents = new Scanner(new File(playerFile));
        return Integer.parseInt(contents.nextLine());
    } finally {
        if (contents != null) {
            contents.close();
        }
    }
}
~~~

Because `FileNotFoundException` is a checked exception, we must handle it to satisfy the compiler (in this example it does mean that anyone that calls our method now needs to handle it too!)

`parseInt` can throw a `NumberFormatException`, but because it is unchecked, we aren't required to handle it.

Here, the `finally` block indicates what code we want Java to run regardless of what happens with trying to read the file.

*Even if a `FileNotFoundException` is thrown up the call stack, Java will call the contents of `finally` before doing that.*
## Interfaces

[Baeldung](https://www.baeldung.com/java-interfaces)

```{Note} In Java, an interface is an *abstract type* that contains a collection of methods and constant variables.
```

It is one of the core concepts in Java and is used to achieve abstraction, polymorphism and multiple inheritances.

In an interface, we're allowed to use:

- constants variables: interface variables are public, static, and final by definition; we're not allowed to change their visibility
- abstract methods
- static methods
- default methods

but

- we can't use the `final` word in the interface definition, as it will result in a compiler error
- all interface declarations should have the `public` or `default` access modifier; the `abstract` modifier will be added automatically 
  by the compiler
- an interface method can't be `protected` or `final`
- 

~~~ java
public interface Electronic {  // no abstract modifier

    // Constant variable final static public by definition
    String LED = "LED";

    // Abstract method but no abstract modifier
    int getElectricityUse();

    // Static method
    static boolean isEnergyEfficient(String electtronicType) {
        if (electtronicType.equals(LED)) {
            return true;
        }
        return false;
    }

    //Default method is public 
    default void printDescription() {
        System.out.println("Electronic Description");
    }
}
~~~

Because interfaces are abstract by definition we dont need to use the `abstract` modifier.
## Functional interfaces

Any interface with a SAM (Single Abstract Method) is a functional interface.

The problem: in Java there are many methods that need a function as a parameter or need to return a function. But Java is not a functional programming language so in
these cases we should create a SAM interface and a class `A` a class implementing it and providing the required functionality. Then when a method needs that function 
as a parameter we had to do:

~~~ java
interface SAM {
  Integer func(String s);
}

class A implements SAM {
  public Integer func(String s) {
    return s.length();
  }
}

System.out.println(new A().func("hola"));
~~~

It means a lot of boilerplate is needed just for using a function as a parameter.

The solution: Java 8 fixes this situtation providing lambda expressions and many new functional interfaces. Let's see how they work.

A lambda expression is an anonymous function that we can handle as a first-class language citizen. For instance, we can pass it to or return it from a method or assign 
it to a variable.

```{Note} 
Functional interfaces implementation may be treated as lambda expressions i.e. *we can use lambda expressions instead of functional interfaces* (`Function`, 
`BiFunction`, `Supplier`, `Consumer`, `Predicate`, `Operator`...).
```

As an example of functional interface we will use the `Function` interface which represents a function which takes in one argument and produces a result. Hence this functional interface takes in 2 generics namely as follows:

    T: denotes the type of the input argument
    R: denotes the return type of the function

The abstract method of `Function` is `apply()` which eventually applies the given function on the argument.

Let's see an example. The method `HashMap.computeIfAbsent` uses the `Function` functional interface as parameter:

~~~ java
public V 
       computeIfAbsent(K key,
             Function<? super K, ? extends V> remappingFunction)
~~~

So we need `remappingFunction` being a `Function`. And we need to implement the `Function.apply` abstract method. Since Java 8 we can achieve both goals at the same
time as follows:

~~~ java
public Function<String, Integer> remappingFunction(String name) {
  return name -> name.length();
}
~~~

as a result the returned lambda expression becomes the implementation of the `Function.apply` method:

~~~ java
public Integer apply(String name) {
  return name.length();
}
~~~

and we use it as follows:

~~~ java
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", remappingFunction);
~~~

We can do it shorter:

~~~ java
Function<String, Integer> remappingFunction = s -> s.length();
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", remappingFunction);
~~~

And even shorter as we can use directly a lambda expression:

~~~ java
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", s -> s.length());
~~~

or a reference to a method with the proper types of input and return values:

~~~ Java
Map<String, Integer> nameMap = new HashMap<>();
Integer value = nameMap.computeIfAbsent("John", String::length);
~~~

But the most readable choice seems to be lambda expressions..

Summary: thanks to functional interfaces and lambda expressions we can move from

~~~ java
interface SAM {
  Integer func(String s);
}

class A implements SAM {
  public Integer func(String s) {
    return s.length();
  }
}

System.out.println(new A().func("hola"));
~~~

to

~~~ java
Function<String, Integer> func = s -> s.length()
System.out.println(func.apply("hola"));
~~~
## The for loop

Currently `for` loops come in three flavours.

### Classic for

~~~ java
for (initialization; termination;
     increment) {
    statement(s)
}
~~~

When using this version of the for statement, keep in mind that:

- The initialization expression initializes the loop; it's executed only once, as the loop begins.
- When the termination expression evaluates to false, the loop terminates.
- The increment expression is invoked after each iteration through the loop; it is perfectly acceptable for this expression to 
  increment or decrement a value.


Notice how the code declares a variable within the initialization expression. The scope of this variable extends from its declaration 
to the end of the block governed by the for statement, so it can be used in the termination and increment expressions as well. If the 
variable that controls a for statement is not needed outside of the loop, it's best to declare the variable in the initialization 
expression. The names i, j, and k are often used to control for loops; declaring them within the initialization expression limits their 
life span and reduces errors.

So this is a typical example:

~~~ java
for(int i=1; i<11; i++){
  System.out.println("Count is: " + i);
}
~~~

Notice that `i` is initialised inside the loop so it is only visible in the loop block.

But this is allowed too:

~~~ java
int i = 1;
for(; i<11; i++){
  System.out.println("Count is: " + i);
}
~~~

We will use this form when the `i` variable is needed out of the loop.

However this will fail:

~~~ java
int i = 1;
for(int i = 0; i<11; i++){
  System.out.println("Count is: " + i);
}
~~~

because the outer `i` is visible inside the loop so the loop initialization is creating a variable that already exists.

### Enhanced for loop

Its use is recommended over the classic use because it has an easier syntax.

~~~ java
int[] numbers = 
    {1,2,3,4,5,6,7,8,9,10};
for (int item : numbers) {
    System.out.println("Count is: " + item);
}
~~~

### forEach loop

Since Java 8, we can leverage for-each loops in a slightly different way. We now have a dedicated `forEach()` method in the `Iterable`
interface that accepts a lambda expression representing an action we want to perform.

~~~ java
numbers.forEach(item -> System.out.println("Count is: " + item));
~~~
## String -bytes conversion

[baeldung](https://www.baeldung.com/java-string-to-byte-array)

A String is stored as an array of Unicode characters in Java. To convert it to a byte array, we translate the sequence of 
characters into a sequence of bytes. For this translation, we use an instance of Charset. **This class specifies a mapping between a 
sequence of chars and a sequence of bytes**.

We refer to the above process as encoding.

~~~ java
String inputString = "Hello World!";
byte[] byteArrray = inputString.getBytes();   // use the default charset which is system-dependent
byte[] byteArrray = inputString.getBytes("IBM01140");  // use a named charset
byte[] byteArrray = inputString.getBytes(Charset.forName("ASCII"));  // use an instance of Charset
byte[] byteArrray = inputString.getBytes(StandardCharsets.UTF_16);  // use a standard charset

String inputString = "Hello ਸੰਸਾਰ!";
Charset charset = StandardCharsets.US_ASCII;
byte[] byteArrray = charset.encode(inputString).array();

CharsetEncoder encoder = StandardCharsets.US_ASCII.newEncoder();  // this provides fine-grained control over the encoding process
encoder.onMalformedInput(CodingErrorAction.IGNORE)
  .onUnmappableCharacter(CodingErrorAction.REPLACE)
  .replaceWith(new byte[] { 0 });

byte[] byteArrray = encoder.encode(CharBuffer.wrap(inputString))
                      .array();
~~~

We refer to the process of converting a byte array to a String as decoding. Similar to encoding, this process requires a Charset.

However, we can't just use any charset for decoding a byte array. In particular, we should use the charset that encoded the String 
into the byte array.

~~~ java
String charsetName = "IBM01140";
byte[] byteArrray = { -56, -123, -109, -109, -106, 64, -26, -106,
  -103, -109, -124, 90 };

String string = new String(byteArrray, charsetName);
    
assertEquals("Hello World!", string);
...
~~~
## Java initialization

[Baeldung](https://www.baeldung.com/java-initialization)

All objects in Java are stored in our program's heap memory. In fact, the heap represents a large pool of unused 
memory, allocated for our Java application.
## Overloading vs Overriding

[baeldung](https://www.baeldung.com/java-method-overload-override)

Overloading: dentro de la misma clase definimos varios métodos con el mismo nombre pero diferente firma:

- número diferente de argumentos
- mismo número de argumentos pero distinto tipo
- no se permiten varios métodos que difieran solamente en el tipo del valor de retorno

```{note} Promoción de tipos: si se invoca un método y el tipo de los parámetros no coincide con el de los argumentos
entonces, si se puede, se hace promoción de tipos:

    byte -> short -> int -> long -> float -> double
    char -> int -> long -> float -> double
```

Un tipo puede promocionar a cualquiera de los tipos de su derecha, por ejemplo, si tengo un método:

~~~~ java
public String samplemethod(float index)
~~~~

lo puedo invocar como:

~~~~ java
samplemethod('f')
~~~~

y el argumento de tipo `char` se convertirá automáticamente en `float`.

```{note} Sustitución de tipos: el LSP (Liskov Substituion Principle) afirma que si una aplicación funciona correctamente con un tipo base también
debe hacerlo con cualquiera de sus subtipos.
```

Overriding: un método implementado en un clase base se reimplementa *no necesariamente con la misma firma* en un 
subclase para conseguir un comportamiento personalizado. *El overriding debe usarse con cuidado porque no siempre cumple
el LSP*.

Of course, it's valid to make an overridden method to accept arguments of different types and return a different type as
well, but with full adherence to these rules:

- If a method in the base class takes argument(s) of a given type, the overridden method should take the same type or a supertype (a.k.a. contravariant method arguments)
- If a method in the base class returns void, the overridden method should return void
- If a method in the base class returns a primitive, the overridden method should return the same primitive
- If a method in the base class returns a certain type, the overridden method should return the same type or a subtype (a.k.a. covariant return type)
- If a method in the base class throws an exception, the overridden method must throw the same exception or a subtype of the base class exception

```{note} Binding: el binding es la asociación de una llamada a un método con el cuerpo de dicho método. Se puede hacer
de manera estática (en tiempo de compilación) o dinámica (en tiempo de ejecución).
```

En el caso de sobrecarga de métodos el binding es estático. En el caso de sobreescritura, es dinámico, porqué implica
herencia y una jerarquía de tipos y el compilador no es capaz de determinar en tiempo de compilación que método hay
que llamar. Para saberlo el compilador tiene que comprobar el tipo del objeto que está invocando al método y esa
comprobación se hace en tiempo de ejecución.
## El modificador `static`

[baeldung](https://www.baeldung.com/java-static)

Dentro de una clase, un método no estático puede acceder *directamente* a cualquier variable/método de esa clase (ya sea 
estático o no). Pero los métodos estáticos solo pueden acceder *directamente* a variables/métodos estáticos. *Si quieren
acceder a miembros no estáticos tienen que hacerlo a través de una instancia de esa clase*.

Los métodos estáticos no se pueden sobreescribir porque se resuelven en tiempo de compilación.
## Modificador `final`

[baeldung](https://www.baeldung.com/java-final)

`final` se puede usar en:

- clases
- variables (estáticas y no estáticas)
- métodos
- argumentos de métodos
## Enums

## Scanner

`Scanner` class reads *input stream into **tokens*** (not lines). The token delimiter defaults to white space.

Constructor: `Scanner(input_stream)`

where input_stream can be any kind of stream: a String, a file, a byte array, the standard input stream...

- `Scanner("Some string")`
- `Scanner(new File(filepath))` or `Scanner(FileInputStream(filepath))`
- `Scanner(new ByteArrayInputStream(a_string.getBytes()))`
- `Scanner(System.in)`

Must know the `next()`, `nextInt()`, `nextDouble()` and `nextLine()` APIs:

### next() consumes the line separator but not the token delimiter

    AThis is the first line.\n
    This is the second line_And\n
    123Some more text\n
    45,7

`A` is the delimiter. From position (0,0) `next()` will return:

    This is the first line.\n
    This is the second line_

### nextLine() is a kind of buffer.readLine(). It consumes both the line separator and the token delimiter delimiter

and moves the current position of the Scanner to the next line.

So from the current position `nextLine()` will return:

    And\n

Position is (2, 0) now.

### nextInt() and nextDouble() will not consume the line separator nor the token delimiter.

So from the current position `nextInt()` will return:

    123

Position is (2, 4) now.

`nextLine()` will move cursor to position (3, 0). Finally `nextDouble()` will return:

    45.7

Also hasNext() for validating purposes.
## Streams

[Streams](https://www.baeldung.com/java-8-streams-introduction)

Los streams permiten trabajar con flujos/secuencias de elementos.

Un stream se crea a partir de un data provider (normalmente un array o una colección).

~~~ java
String[] arr = new String[] {"hola", "adios"};
Stream<String> stream = Arrays.stream(arr);  // Stream a partir de un array
stream = list.stream();                      // Stream a partir de una colección
stream = Stream.of("a", "b", "c");           // Stream a partir de un literal
stream = Stream.empty();                     // crea un stream vacío
~~~

Las operaciones con streams se agrupan en:

- intermedias: devuelven un Stream<T>, se pueden encadenar.
- terminales: devuelven un resultado de un determinado tipo. Como no devuelven un stream no se pueden seguir encadenando
  con operaciones intermedias.

~~~ java
long count = list.stream().distinct().count();
~~~

Los streams se pueden iterar con bucles for, foreach, while... pero tambien con métodos propios que ponen énfasis en
la operación que se realiza sobre los elementos en cada iteración, obviando los detalles de como se realiza la 
iteración (no hace falta usar índices, contadores...). Normalmente las operaciones sobre los elementos se hacen con
expresiones lambda. Por ejemplo:

~~~ java
/* Matching */
boolean isValid = list.stream().anyMatch(element -> element.contains("h")); // true
boolean isValidOne = list.stream().allMatch(element -> element.contains("h")); // false
boolean isValidTwo = list.stream().noneMatch(element -> element.contains("h")); // false

/* Mapping */
List<String> uris = new ArrayList<>();
uris.add("C:\\My.txt");
Stream<Path> stream = uris.stream().map(uri -> Paths.get(uri));      // convert Stream<String> to Stream<Path>

List<Detail> details = new ArrayList<>();
details.add(new Detail());
Stream<String> stream
  = details.stream().flatMap(detail -> detail.getParts().stream());

/* Filtering */
Stream<String> stream = list.stream().filter(element -> element.contains("d"));

/* Reduction */
List<Integer> integers = Arrays.asList(1, 1, 1);
Integer reduced = integers.stream().reduce(23, (a, b) -> a + b);

/* Collecting ??? */
~~~

Importante:

- Java NIO class `Files` allows us to generate a `Stream<String>` of a text file through the `lines()` method. Every 
line of the text becomes an element of the stream
- We can instantiate a stream, and have an accessible reference to it, as long as only intermediate operations are 
  called. Executing a terminal operation makes a stream inaccessible. This kind of behavior is logical. We designed 
  streams to apply a finite sequence of operations to the source of elements in a functional style, not to store 
  elements.




### Pipeline

[pipeline](https://www.baeldung.com/java-8-streams#pipeline)

To perform a sequence of operations over the elements of the data source and aggregate their results, we need three 
parts: the source, intermediate operation(s) and a terminal operation.

Intermediate operations return a new modified stream. If we need more than one modification, we can chain intermediate 
operations. 

A stream by itself is worthless; the user is interested in the result of the terminal operation, which can be a value 
of some type or an action applied to every element of the stream. We can only use one terminal operation per stream.

The correct and most convenient way to use streams is by a stream pipeline, which is a chain of the stream source, 
intermediate operations, and a terminal operation:

~~~ java
List<String> list = Arrays.asList("abc1", "abc2", "abc3");
long size = list.stream().skip(1)
  .map(element -> element.substring(0, 3)).sorted().count();
~~~

From the performance point of view, the right order is one of the most important aspects of chaining operations in the 
stream pipeline: intermediate operations which reduce the size of the stream should be placed before operations which 
are applying to each element. So we need to keep methods such as `skip()`, `filter()`, and `distinct()` at the 
[vertical] top of our stream pipeline.


In most of the code samples shown in this article, we left the streams unconsumed (we didn't apply the `close()` method 
or a terminal operation). In a real app, *don't leave an instantiated stream unconsumed, as that will lead to memory 
leaks*.
## Collectors

[Collectors](https://www.baeldung.com/java-8-collectors)

[Definiciones útiles](https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html#Reduction):

- A reduction operation (also called a fold) takes a sequence of input elements and combines them into a single summary 
  result by repeated application of a combining operation, such as finding the sum or maximum of a set of numbers. The 
  streams classes have a general reduction operation, called `reduce()`, as well as multiple specialized reduction 
  forms such as `sum()`, `max()`, or `count()`.
- A mutable reduction operation accumulates input elements into a mutable result container, such as a Collection or 
  StringBuilder, as it processes the elements in the stream. The streams classes have a general reduction operation, 
  called `collect()`

A collect operation requires three functions: a supplier function to construct new 
instances of the result container, an accumulator function to incorporate an input element into a result container, 
and a combining function to merge the contents of one result container into another. The three aspects of collect
 -- supplier, accumulator, and combiner -- are tightly coupled. We can use the abstraction of a Collector to capture 
 all three aspects.

```{note} Un colector es un método que convierte un Stream en algún tipo de colección (List, Set, Map) y, 
opcionalmente, realiza alguna operación sobre la colección.
```

All predefined implementations can be found in the Collectors class. It's common practice to use the following static 
import with them to leverage increased readability:

~~~ java
import static java.util.stream.Collectors.*;
//We can also use single import collectors of our choice:

import static java.util.stream.Collectors.toList;
import static java.util.stream.Collectors.toMap;
import static java.util.stream.Collectors.toSet;
~~~

Ejemplos de uso:

~~~ java
List<String> result = givenList.stream()
  .collect(toList());

Set<String> result = givenList.stream()
  .collect(toSet());

Map<String, Integer> result = givenList.stream()
  .collect(toMap(Function.identity(), String::length));
~~~

`Function.identity()` is just a shortcut for defining a function that accepts and returns the same value.

`givenList.stream()` transforma una lista en un stream. `stream.collect(toList())` convierte el stream en una
colección usando el colector `toList()`. When using the `toSet` and `toList` collectors, we can't make any assumptions 
of their implementations. If we want to use a custom implementation, we'll need to use the toCollection collector with 
a provided collection of our choice.

~~~ java
List<String> result = givenList.stream()
  .collect(toCollection(LinkedList::new))
~~~

With the collect() method we can [non-mutable] reduction operations too using Collectors methods (`joining`, 
`averagingInt`).
## Java IO: concepts

[baeldung](https://www.baeldung.com/java-outputstream)

The Java IO API which defines classes required to perform I/O operations in Java. These are all packaged in the java.io namespace. This 
is one of the core packages available in Java since version 1.0.

Starting Java 1.4, we also have Java NIO packaged in the namespace java.nio which enables non-blocking input and output operations. 

Details related to Java IO and Java NIO can be found [here](https://docs.oracle.com/javase/8/docs/technotes/guides/io/index.html).

*Java IO basically provides a mechanism to read data from a source and write data to a destination. Input represents the source while 
output represents the destination here.*

These sources and destinations can be anything from Files, Pipes to Network Connections.

Java IO provides the concept of **streams** which basically represents a *continuous flow of data*. Streams can support many different 
types of data like bytes, characters, objects, etc.

Moreover, *connection to a source or a destination is what a stream represents*. They hence come as either InputStream or OutputStream 
respectively.

### Buffering

[Como funciona el buffering de escritura?](https://www.baeldung.com/java-outputstream#outputstream-buffering)

Input and output operations typically involve relatively expensive operations like disk access, network activity, etc. Performing this 
often can make a program less efficient.

We have “buffered streams” of data in Java to handle these scenarios. BufferedOutputStream writes data to a buffer instead which is 
flushed to the destination less often, when the buffer gets full (default buffer size is 8K), or the method flush() is called.

BufferedOutputStream extends FilterOutputStream discussed earlier and wraps an existing OutputStream to write to a destination:

~~~ java
public static void bufferedOutputStream(
  String file, String ...data) throws IOException {
 
    try (BufferedOutputStream out = new BufferedOutputStream(new FileOutputStream(file))) {
        for(String s : data) {
            out.write(s.getBytes());
            out.write(" ".getBytes());
        }
    }
}
~~~

The critical point to note is that every call to write() for each data argument *only writes to the buffer* and does not result in a 
potentially expensive call to the File.

In the case above, if we call this method with data as “Hello”, “World!”, this will only result in data being written to the File when 
the code exits from the try-with-resources block which calls the method close() -which instead calls flush()- on the 
BufferedOutputStream.

This results in an output file with the following text:

Hello World!

[Como funciona el buffering de lectura?](https://www.baeldung.com/java-buffered-reader)

In general, BufferedReader comes in handy if we want to read text from any kind of input source whether that be files, sockets, or 
something else.

Simply put, it enables us to minimize the number of I/O operations by reading chunks of characters and storing them in an internal 
buffer. While the buffer has data, the reader will read from it instead of directly from the underlying stream. Por defecto, 
BufferedReader usa un tamaño de buffer de 8K.

BufferedReader utiliza el patrón Decorator lo que significa que cuando se usa el constructor BufferedReader(Reader) automáticamente se
añade a Reader la capacidad de hacer buffering.

Ejemplos de BufferedReader(Reader):

~~~ java
BufferedReader reader = 
  new BufferedReader(new FileReader("src/main/resources/input.txt"));

BufferedReader reader = 
  new BufferedReader(new InputStreamReader(System.in));

BufferedReader reader = 
           new BufferedReader(new StringReader("1__2__3__4__5"));

BufferedReader reader = 
  Files.newBufferedReader(Paths.get("src/main/resources/input.txt"))
~~~
## Java IO: Consola

[baeldung](https://www.baeldung.com/java-console-input-output)

La entrada estándar en Java es el stream System.in, y la salida estándar es el stream System.out. Por defecto, usamos el teclado para
escribir datos en System.in y la pantalla para leer datos de System.out.

Para leer datos (que hemos introducido previamente con el teclado) de System.in usamos la clase Scanner:
- Scanner.nextLine() lee una línea entera de datos y la almacena en un String
- Scanner.nextXXX() lee un token e intenta almacenarlo en una variable de tipo XXX

La idea es que entramos datos a System.in con el teclado y los recuperamos con la clase Scanner.

~~~ java
// Java program to read data of various types using Scanner class.
import java.util.Scanner;
public class ScannerDemo1
{
	public static void main(String[] args)
	{
		// Declare the object and initialize with
		// predefined standard input object
		Scanner sc = new Scanner(System.in);

		// String input
		String name = sc.nextLine();

		// Character input
		char gender = sc.next().charAt(0);

		// Numerical data input
		// byte, short and float can be read
		// using similar-named functions.
		int age = sc.nextInt();
		long mobileNo = sc.nextLong();
		double cgpa = sc.nextDouble();

		// Print the values to check if the input was correctly obtained.
		System.out.println("Name: "+name);
		System.out.println("Gender: "+gender);
		System.out.println("Age: "+age);
		System.out.println("Mobile Number: "+mobileNo);
		System.out.println("CGPA: "+cgpa);
    sc.close();
	}
}
~~~

Input correcto:
John Doe
Female 20 645645645 5.6 pepe

Input erróneo:
Pedro Picapiedra
Female 20.1 645645645 60

Después de leer la primera línea necesitamos al menos cuatro tokens. Los podemos introducir todos en una línea (como hemos hecho 
arriba) o usando varias líneas (cuatro líneas de un token, dos líneas de dos tokens, etc.).

Para escribir al System.out usamos println() o print().

En general el mecanismo es: 

- escribo datos a un stream (con el teclado escribo en System.in, con println() escribo en System.out)
- recupero datos del stream (con los métodos nextXXX() de System.in, con la pantalla de System.out)

Otra posibilidad es usar la clase System.Console.

- Console.readline(msg): escribe el mensaje msg a System.out y lee una línea de System.in.
- Console.readPassword(msg)
- Console.printf(msg)
## Java IO: files

Podemos crear un fichero de muchas maneras:

~~~ java
private final String FILE_NAME = "src/test/resources/fileToCreate.txt";

// Method 1 uses Java IO File objects
File newFile = new File(FILE_NAME);
boolean success = newFile.createNewFile();  //Devuelve false si el fichero ya existe

// Method 2 uses Java NIO Files objects
Path newFilePath = Paths.get(FILE_NAME);
Files.createFile(newFilePath);  // FileAlreadyExistsException si el fichero ya existe

// Method 3 uses Stream objects
try(FileOutputStream fileOutputStream = new FileOutputStream(FILE_NAME)){  // Si el fichero ya existe lo sobreescribe
    }
~~~

El método 2 es el que mejor maneja las excepciones.

Podemos leer y escribir un fichero de muchas maneras. Algunas de ellas son:

[leer](https://www.baeldung.com/reading-file-in-java)
[escribir](https://www.baeldung.com/java-write-to-file)


Si queremos leer/escribir texto lo más sencillo es usar BufferedReader y BufferedWriter:

~~~ java
String file ="src/test/resources/fileTest.txt";

BufferedReader reader = new BufferedReader(new FileReader(file, "UTF-8"));
String currentLine = reader.readLine();
reader.close();

// write to a new file
String str = "Hello";
BufferedWriter writer = new BufferedWriter(new FileWriter(fileName));
writer.write(str);
writer.close();

// write to an existing file
String str = " world!";
BufferedWriter writer = new BufferedWriter(new FileWriter(fileName, true));
writer.append(str);
writer.close();
~~~

Si queremos escribir texto con formato usamos PrintWriter:

~~~ java
FileWriter fileWriter = new FileWriter(fileName);
PrintWriter printWriter = new PrintWriter(fileWriter);  // can use FileWriter, BufferedWriter or System.out
printWriter.print("Some String");
printWriter.printf("Product name is %s and its price is %d $", "iPhone", 1000);
printWriter.close();
~~~

Si queremos leer/escribir secuencias de bytes (i.e. ficheros binarios, como una imagen), lo más sencillo es usar
FileInputStream y FileOutputStream:

~~~ java
String str = "Hello";
FileInputStream inputStream = new FileInputStream(fileName);
int ch;
while(ch = inputStream.read() != -1) {System.out.println(ch)}
inputStream.close();

String str = "Hello";
FileOutputStream outputStream = new FileOutputStream(fileName);
byte[] strToBytes = str.getBytes();
outputStream.write(strToBytes);
outputStream.close();
~~~

Con Java NIO (fichero pequeño, lo guardamos entero en una variable):

~~~ java
Path path = Paths.get("src/test/resources/fileTest.txt");

List<String> lines = Files.readAllLines(path);
~~~

Con Java NIO (fichero grande, usamos un buffer):

~~~ java
Path path = Paths.get("src/test/resources/fileTest.txt");

BufferedReader reader = Files.newBufferedReader(path);
String line = reader.readLine();
~~~

Con Java NIO Files.lines():

~~~ java
Path path = Paths.get(getClass().getClassLoader()
  .getResource("fileTest.txt").toURI());
      
Stream<String> lines = Files.lines(path);
String data = lines.collect(Collectors.joining("\n"));
lines.close();
~~~

También podemos leer con una instancia de Scanner:

~~~ java
String file = "src/test/resources/fileTest.txt";
Scanner scanner = new Scanner(new File(file));
scanner.useDelimiter(" ");
String word;
while(scanner.hasNext()){
  word = scanner.next();
}
scanner.close();
~~~

Aún quedan cosas que ver: random access para I/O, creación de ficheros temporales, bloqueo de ficheros, NIO API...

Un FileChannel es un canal de bytes conectado a un fichero. Un canal mantiene una posición (la posición actual) y permite cambiarla.
