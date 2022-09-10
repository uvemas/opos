# Tema 14 OOP: Herencia y Polimorfismo 

## Clases abstractas

[baeldung](https://www.baeldung.com/java-abstract-class)

Before diving into when to use an abstract class, let's look at their most relevant characteristics:

- We define an abstract class with the `abstract` modifier preceding the `class` keyword
- An abstract class can be subclassed, but it can't be instantiated
- If a class defines one or more abstract methods, then the class itself must be declared abstract
- An abstract class can declare both abstract and concrete methods
- A subclass derived from an abstract class must either implement all the base class's abstract methods or be abstract itself
- abstract methods must have the `abstract` modifier (diferencia con las interfaces)
- methods can be `protected` (diferencia con las interfaces)
- Classes in Java support single inheritance; they can't extend multiple classes.
- Also, note that in the absence of an extends keyword, a class implicitly inherits class java.lang.Object.
- A subclass class inherits the non-static protected and public members from the superclass class. In addition, the 
  members with default (package-private) access are inherited if the two classes are in the same package.
- On the other hand, the private and static members of a class are not inherited.
## Interfaces

[baeldung](https://www.baeldung.com/java-interfaces)

In Java, an interface is an *abstract type* that contains a collection of methods and constant variables.

~~~ java
public interface Electronic {
    // Constant variable
    String LED = "LED";

    // Abstract method
    int getElectricityUse();

    // Static method
    static boolean isEnergyEfficient(String electtronicType) {
        if (electtronicType.equals(LED)) {
            return true;
        }
        return false;
    }

    //Default method
    default void printDescription() {
        System.out.println("Electronic Description");
    }
}
~~~

Una interface puede contener:

- nada
- constantes (por definición las variables son `public static final` y esto no puede cambiarse)
- métodos abstractos
- métodos concretos que tengan el modificador `static` o `default`
- los métodos no pueden ser `protected` o `final` pero pueden ser `private` [baeldung](https://www.baeldung.com/java-interface-private-methods)

Las interfaces son tipos abstractos por definición: al crear una interface no necesitamos añadir el modificador 
`abstract`, el compilador lo hace por nosotros. Lo mismo ocurre con los métodos abstractos.

We can implement an interface in a Java class by using the `implements` keyword.

~~~ java
public class Computer implements Electronic {

    @Override
    public int getElectricityUse() {
        return 1000;
    }
}
~~~

Con interfaces podemos:

- implementar un determinado comportamiento
- conseguir herencia múltiple (implementando varias interfaces al mismo tiempo)
- conseguir polimorfismo (recordemos que las interfaces *son tipos*)

~~~ java
List<Shape> shapes = new ArrayList<>();  // Shape es una interface
Shape circleShape = new Circle();  // Circle es una clase que implementa Shape
Shape squareShape = new Square();  // Square es una clase que implementa Shape

shapes.add(circleShape);
shapes.add(squareShape);

for (Shape shape : shapes) {
    System.out.println(shape.name());  // BOOM! Polimorfismo en acción
}
~~~

```{note} An interface inherits other interfaces by using the keyword `extends`. Classes use the keyword `implements` to
inherit an interface.
```
## Clases abstractas vs Interfaces

A veces es difícil decidir si usar una clase abstracta o una interface.

Como regla de oro podemos decir que usaremos una clase abstracta cuando hay funcionalidad que queremos heredar. Si lo
que buscamos no es heredar sino implementar una determinada funcionalidad entonces usaremos interfaces.

Por ejemplo:

~~~ java
public abstract class BaseFileReader {
    protected Path filePath;
    protected BaseFileReader(Path filePath) {
        this.filePath = filePath;
    }
    
    public Path getFilePath() {
        return filePath;
    }

    public List<String> readFile() throws IOException {
        return Files.lines(filePath)
          .map(this::mapFileLine).collect(Collectors.toList());
    }

    protected abstract String mapFileLine(String line);
}
//
public class LowercaseFileReader extends BaseFileReader {
    public LowercaseFileReader(Path filePath) {
        super(filePath);
    }

    @Override
    public String mapFileLine(String line) {
        return line.toLowerCase();
    }   
}
//
public class UppercaseFileReader extends BaseFileReader {
    public UppercaseFileReader(Path filePath) {
        super(filePath);
    }

    @Override
    public String mapFileLine(String line) {
        return line.toUpperCase();
    }
}
~~~

Vemos como las subclases heredan una determinada funcionalidad (métodos concretos de la clase base) y la extienden con 
su propia implementación de los métodos abstractos.

En cambio con una interface podemos añadir un determinado comportamiento a clases que no están relacionadas entre ellas
(por lo que no heredan unas de otras y no tiene sentido usar clases abstractas). Por ejemplo, la interface
`Comparator` la implementan los tipos `Integer` y `String` (entre otros) y podemos usarla en nuestros propios tipos:

~~~ java
public class Employee {

    private double salary;

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }
}
//
public class EmployeeSalaryComparator implements Comparator<Employee> {

    @Override
    public int compare(Employee employeeA, Employee employeeB) {
        if (employeeA.getSalary() < employeeB.getSalary()) {
            return -1;
        } else if (employeeA.getSalary() > employeeB.getSalary()) { 
            return 1;
        } else {
            return 0;
        }
    }
}
~~~


## Herencia de tipos

[baeldung](https://www.baeldung.com/java-inheritance)

*When a class inherits another class or interfaces, apart from inheriting their members, it also inherits their type. 
This also applies to an interface that inherits other interfaces.*

This is a very powerful concept, which allows developers to program to an interface (base class or interface), rather 
than programming to their implementations.

For example, imagine a condition, where an organization maintains a list of the cars owned by its employees. Of course, 
all employees might own different car models. So how can we refer to different car instances? Here's the solution:

~~~ java
public class Employee {
    private String name;
    private Car car;
    
    // standard constructor
}
~~~

Because all derived classes of Car inherit the type Car, the derived class instances can be referred by using a variable of class Car:

~~~ java
Employee e1 = new Employee("Shreya", new ArmoredCar());
Employee e2 = new Employee("Paul", new SpaceCar());
Employee e3 = new Employee("Pavni", new BMW());
~~~
## Herencia, composición y dependency injection

[baeldung](https://www.baeldung.com/java-inheritance-composition)
[wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)
