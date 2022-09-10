# Tema 13 OOP: Clases y Objetos 

## Top-level clases

Este término puede llevar a confusión, porque parece que haga referencia a la posición de una clase dentro de la
jerarquía de clases, lo cual no es cierto.

En [la especificación de Java](http://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html) se define una top-level
class como aquella que no está anidada:

> A top level class is a class that is not a nested class.
>
> A nested class is any class whose declaration occurs within the body of another class or interface.

Por lo tanto el término top-level hace referencia a la posición de la clase en el código, *no* en la jerarquía de
clases.
## Modificadores de acceso

[Reference](https://www.baeldung.com/java-access-modifiers)

As the name suggests access modifiers in Java helps to restrict the scope of a class, constructor, variable or method,
o sea, ayudan a encapsular el estado de un objeto. 
There are four types of access modifiers available in java: 

- `default` (no keyword required)
- `private`
- `protected`
- `public`

Una top-level class solo puede utilizar los modificadores `public` y `default`. Los miembros de una clase (fields and
methods) pueden usar los cuatro modificadores.

Cuando se aplican a una clase, los modificadores definen si la clase se puede importar o no, y su significado es el 
siguiente:

- `public`: la clase es visible desde cualquier parte del código
- `default`: la clase solo es visible desde el paquete al que pertenece

Para usar una clase `public` desde otro paquete hay que importar la clase. Para usar una clase desde el mismo paquete
al que pertenece no hace falta importarla, se puede usar directamente.

Para los miembros de una clase, los modificadores permiten restringir el acceso garantizado por el modificador de 
la clase.

- `public`: el miembro es accesible desde cualquier lugar del código
- `protected`: el miembro solo es accesible desde el paquete al que pertenece la clase y sus subclases visibles (aunque 
  estén en otro paquete)
- `default`: el miembro solo es accesible desde el paquete al que pertenece la clase
- `private`: el miembro solo es accesible desde dentro de la clase en que está definida

Lógicamente si la clase es `default` los modificadores `public` o `protected` en un miembro no tienen sentido. En ese 
caso se puede [usar una
interfaz](https://stackoverflow.com/questions/18758317/how-do-i-access-a-public-method-of-a-default-class-outside-the-package)

En el caso de variables accesible significa que la variable se puede leer y modificar.

En el caso de métodos accesible significa que el método se puede invocar.
## Modificadores de no-acceso

Proporcionan información a la JVM acerca de una clase, mètodo o variable. Hay siete:

- final: it indicates that the specific class cannot be extended or a method cannot be overridden.
- static: it means that the entity to which it is applied is available outside any particular instance of the class. 
  That means the static methods or the attributes are a part of the class and not an object.
- abstract: it is used to declare a class as partially implemented means an object cannot be created directly from that 
  class. Any subclass needs to be either implement all the methods of the abstract class, or it should also need to be 
  an abstract class. The abstract keyword cannot be used with static, final, or private keywords because they prevent 
  overriding, and we need to override methods in the case of an abstract class
- synchronized: synchronized keyword prevents a block of code from executing by multiple threads at once. It is very 
  important for some critical operations.
- volatile: the volatile keyword is used to make a class thread-safe. That means if a variable is declared as volatile, 
  then that can be modified by multiple threads at the same time without any issues. The volatile keyword is only 
  applicable to a variable. 
- transient: This needs prior knowledge of serialization in Java. You can refer to the following article for that:- 
  serialization in java. The transient keyword may be applied to member variables of a class to indicate that the 
  member variable should not be serialized when the containing class instance is serialized. Serialization is the ​
  process of converting an object into a byte stream. When we do not want to serialize the value of a variable, then we 
  declare it as transient.
- native: The native keyword may be applied to a method to indicate that the method is implemented in a language other 
  than Java.
  
## Clases y objetos

[baeldung](https://www.baeldung.com/java-classes-objects)
