# Tema 24 Spring Boot

## SpringBoot

En el diccionario boot strapping tiene muchos significados, uno de ellos es: to start up with minimum resources. 

Esa definición parece que se adapta al espíritu de lo que es SpringBoot:

[Referencia muy interesante](https://www.ibm.com/cloud/learn/java-spring-boot)

Java Spring Framework (Spring Framework) is a popular, open source, enterprise-level framework for creating standalone, 
production-grade applications that run on the Java Virtual Machine (JVM).

Java Spring Boot (Spring Boot) is a tool that makes developing web application and microservices with Spring Framework faster and 
easier through three core capabilities:

- Autoconfiguration
- An opinionated approach to configuration
- The ability to create standalone applications

These features work together to provide you with a tool that allows you to set up a Spring-based application with minimal configuration 
and setup.

So the trick with Spring Boot is that many things happen implicitly. For instance, we use the [@SprinBootApplication](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-using-springbootapplication-annotation.html) annotation, but 
it's a combination of three annotations:

~~~ java
@Configuration
@EnableAutoConfiguration
@ComponentScan
~~~
## Dependency injection and IoC

[IoC y DI](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)

**Inversion of Control** is a principle in software engineering which transfers the control of objects or portions of a program to a 
container or framework. We most often use it in the context of object-oriented programming.

In contrast with traditional programming, in which our custom code makes calls to a library, IoC enables a framework to take control of 
the flow of a program and make calls to our custom code. To enable this, frameworks use abstractions with additional behavior built in.

- Programación tradicional: el flujo del programa lo controla nuestro código haciendo llamadas a las bibliotecas/framework.
- IoC: el flujo del programa lo controla el framework haciendo llamadas a nuestro código

En la IoC una parte fundamental es el contenedor IoC. In the Spring framework, the interface 
`ApplicationContext` represents the IoC container. The Spring container is responsible for instantiating, configuring and assembling 
objects known as **beans**, as well as managing their life cycles.

In order to assemble beans, the container uses configuration metadata, which can be in the form of XML configuration or annotations.

Para crear los beans solo necesitamos proporcionale al IoC los metadatos necesarios. Esto se puede hacer de tres maneras:

- en un fichero de configuración (Java o XML)
- anotando las clases con las anotaciones adecuadas (por ejemplo @Component y @Autowired)
- combinando los dos métodos anteriores

We can achieve Inversion of Control through various mechanisms such as: Strategy design pattern, Service Locator pattern, Factory 
pattern, and **Dependency Injection (DI)**.

Dependency injection is a pattern we can use to implement IoC, where the control being inverted is setting an object's dependencies.

O sea, en la DI el control que se invierte es el de las dependencias de un objeto. En lugar de ser el objeto el que controla sus
dependencias cede ese control al contenedor IoC.

Connecting objects with other objects, or **injecting** objects into other objects, is done by an assembler rather than by the objects 
themselves.

Dependency Injection in Spring can be done through constructors, setters or fields.

En programación tradicional si un objeto A depende de otro B, tenemos que instanciar B y A explícitamente:

~~~ java
public class ItemImpl1 implement Item {
    public ItemImpl1() {
      // do something
    }
}
~~~

~~~ java
public class Store {
    private Item item;
 
    public Store(Item item) {
        this.item = item;    
    }
}
~~~

~~~ java
Item item1 = new ItemImpl1();
Store tienda = new Store(item1);
~~~

If we use constructor-based dependency injection, the container will invoke a constructor with arguments each representing a 
dependency we want to set. En este ejemplo usamos un fichero de configuración para definir los beans.

~~~ java
@Configuration
public class AppConfig {

    @Bean
    public Item item1() {
        return new ItemImpl1();  // un bean sin dependencias
    }

    @Bean
    public Store store() {
        return new Store(item1());  // un bean con una dependencia
    }
}
~~~

Ahora ya no tenemos que instanciar en el código las dependencias porque el framework se encarga de crearlas (en el caso de Spring usando
metadatos para ensamblar los beans en tiempo de ejecución) usando el contexto de la aplicación:

~~~ java
ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
Store tienda = context.getBean("store", Store.class);  // no hace falta instanciar Store ni Item
System.out.println(tienda.item);
~~~

For setter-based DI, the container will call setter methods of our class after invoking a no-argument constructor or no-argument static 
factory method to instantiate the bean. Let's create this configuration using annotations:

~~~ java
@Bean
public Item item1() {
    return new ItemImpl1();  // un bean sin dependencias
}

@Bean
public Store store() {
    Store store = new Store();
    store.setItem(item1());
    return store;
}
~~~

Y el código original quedaría:

~~~ java
public class Store {
    private Item item;
    public Store() {
      // do something
    }

    public setItem(Item item) {
      this.item = item;
    }
}
~~~

In case of Field-Based DI, we can inject the dependencies by marking them with an @Autowired annotation. Esto no se hace en el fichero
de configuración, sino directamente en el código.

~~~ java
@Component
public class Store {
    @Autowired
    private Item item; 
}
~~~

Como ni el bean Item ni el bean Store se han definido en el fichero de configuración the 
container will use reflection to inject Item into Store, which is costlier than constructor-based or setter-based injection.

Wiring allows the Spring container to automatically resolve dependencies between collaborating beans by inspecting the beans that have 
been defined.

Summary:

When a class encapsulates other objects, the referenced objects become a dependency for the outer class. In other words, these other 
objects must be created so the outer class can use it. The container takes a look at our classes and, depending on the method you 
choose, instantiates beans from the referenced objects (RaceTrack and Driver) before it instantiates a bean from the outer classes 
(RaceRound). Spring implements a way for the objects needed by another object to be provided as beans for others to reference. This 
process is known as dependency injection; Our classes no longer have to instantiate their own dependencies. Therefore, we can say the 
control of dependencies has been inverted back to the container, and this is why we call it an Inversion of Control (IoC) container.
## Spring Beans

[Beans](https://www.baeldung.com/spring-bean)
Spring Framework offers a dependency injection feature that lets objects define their own dependencies that the Spring container later 
injects into them. This process is called Inversion of Control (IoC). This enables developers to create modular applications consisting 
of loosely coupled components that are ideal for microservices and distributed network applications.

Again, Inversion of Control (IoC) is a process in which an object defines its dependencies without creating them. This object 
delegates the job of constructing and managing such dependencies to an IoC container (in our case the Spring environment/context
where application is executed).

*A Spring bean is every object constructed/instantiated and managed by the IoC container. When working with Spring, we can annotate our 
classes in order to make them into Spring beans.*

*The Spring IoC container's management of beans includes several responsibilities. Perhaps the most significant of which include bean 
instantiation/assembly and the management of dependency injections.*

Veamos la IoC con un ejemplo. Usaremos esta clase:

~~~ java
public class Company {
    private Address address;

    public Company(Address address) {
        this.address = address;
    }

    // getter, setter and other properties
}
~~~

Vemos que depende de la clase `Address`:

~~~ java
public class Address {
    private String street;
    private int number;

    public Address(String street, int number) {
        this.street = street;
        this.number = number;
    }

    // getters and setters
}
~~~

En el código la dependencia se maneja de manera explícita invocando un constructor específico para crearla:

~~~ java
Address address = new Address("High Street", 1000);
Company company = new Company(address);
~~~

Pero si hay muchas dependencias, o las dependencias son muy complejas o hay muchos casos de uso, entonces es mejor usar la IoC para la 
gestión de dependencias. Instead of instantiating dependencies by itself, an object can retrieve its dependencies from an IoC container. 
All we need to do is to provide the container with appropriate configuration metadata.

Veamos el ejemplo anterior de nuevo, esta vez usando inyección de dependencias:

~~~ java
@Component
public class Company {
    // this body is the same as before
}
~~~

Este objeto sigue dependiendo de la clase `Address`:

~~~ java
public class Address {
    // this body is the same as before
}
~~~

Pero ahora esta dependencia la gestionamos con un fichero de configuración:

~~~ java
@Configuration
@ComponentScan(basePackageClasses = Company.class)
public class Config {
    @Bean
    public Address getAddress() {
        return new Address("High Street", 1000);
    }
}
~~~

*Las clases con anotaciones que el contenedor IoC sabe como interpretar se llaman beans*.

@Configuration significa que la clase es una clase de configuración que, en este ejemplo, se usa para instanciar beans de tipo 
`Address`.

[@ComponentScan](https://www.baeldung.com/spring-component-scanning) le dice al contenedor IoC que busque beans en la ruta especificada.

Ya tenemos los beans definidos. Ahora necesitamos un IoC container, para instanciar y gestionar los beans. El contenedor IoC se 
crea a partir de la configuración:

~~~ java
ApplicationContext context = new AnnotationConfigApplicationContext(Config.class);
~~~

y comprobamos que todo funciona:

~~~ java
Company company = context.getBean("company", Company.class);
assertEquals("High Street", company.getAddress().getStreet());
assertEquals(1000, company.getAddress().getNumber());
~~~

La inyección de dependencias también se puede hacer *automáticamente* usando anotaciones, siendo 
[@Autowired](https://www.baeldung.com/spring-autowire) la más común.  In other words, by declaring all the bean dependencies in a 
Spring configuration file, Spring container can autowire relationships between collaborating beans. This is called Spring bean 
autowiring.

Necesitaremos un fichero de configuración que busque automáticamente los beans por toda la aplicación. Lo mejor es que el fichero esté 
en el paquete principal para asegurarnos de que encuentra todos los beans (porqué se busca en el paquete donde está el fichero y en los 
subpaquetes). Normalmente como fichero de configuración usaremos la clase principal de la aplicación:

~~~ java
@SpringBootApplication
public class App {
    public static void main(String[] args) {
        SpringApplication.run(App.class, args);
    }
}
~~~

porque `@SpringBootApplication` equivale a `@Configuration` + `@ComponentScan` + `@EnableAutoConfiguration`. As a result, when we run 
this Spring Boot application, it will automatically scan the components in the current package and its sub-packages. Thus it will 
register them in Spring's Application Context, and allow us to inject beans using `@Autowired`.

After enabling annotation injection, we can use autowiring on properties, setters, and constructors.
## Crear un proyecto SpringBoot con VSCode

[Referencia](https://code.visualstudio.com/docs/java/java-spring-boot)

Como ejemplo vamos a crear un proyecto web. Como gestor del proyecto usaremos Maven.

```{thumbnail} images/sb_maven.png
```

al seleccionar el tipo de proyecto se inicia un wizard en el que vamos respondiendo preguntas: seleccionamos la versión 
estable más reciente de Maven, Java como lenguaje del proyecto, com.opos como ID del grupo (o sea, nombre del paquete), sbweb como ID del 
artefacto, jar como tipo de empaquetado y la versión de Java más reciente disponible.

Después nos pide que seleccionemos las dependencias del proyecto. Elegimos Spring Web

```{thumbnail} images/sb_dependencias.png
```

Por último nos pregunta en que carpeta vamos a guardar el proyecto.

```{thumbnail} images/sb_project_folder.png
```

Una vez hecho esto ya podemos ver el proyecto en el panel izquierdo del VSCode.

```{thumbnail} images/sb_project_vscode.png
```

El fichero pom.xml resultante contiene toda la información que hemos introducido al crear el proyecto.

Además nos ayuda a entender la diferencia entre groupID y artifactID. Por ejemplo, en la sección de dependencias vemos que un
mismo groupID, org.springframework.boot, cuelgan dos artefactos, spring-boot-starter-web y spring-boot-starter-test. Es una
especie de estructura jerárquica en la que los componentes tienen funciones más específicas a medida que nos alejamos de la raíz.

El starter spring-boot-starter-web contiene los módulos necesarios para el desarrollo de la aplicación web MVC y también un starter 
Tomcat para que podamos ejecutar la aplicación sin tener que instalar un Tomcat.

El esqueleto de la aplicación ya está creado y además funciona. Si vamos a la clase anotada con @SpringBootApplication vemos en la
terminal como se ejecuta la aplicación:


      .   ____          _            __ _ _
    /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
    \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
    =========|_|==============|___/=/_/_/_/
    :: Spring Boot ::                (v2.7.3)

    2022-09-11 11:31:34.462  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : Starting SbwebApplication using Java 18.0.2.1 on Perseus with PID 32855 (/home/vicent/Programming/Java/GettingStarted/SpringBoot/sbweb/target/classes started by vicent in /home/vicent/Programming/Java/GettingStarted/SpringBoot)
    2022-09-11 11:31:34.465  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : No active profile set, falling back to 1 default profile: "default"
    2022-09-11 11:31:35.217  INFO 32855 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
    2022-09-11 11:31:35.228  INFO 32855 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
    2022-09-11 11:31:35.228  INFO 32855 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.65]
    2022-09-11 11:31:35.318  INFO 32855 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
    2022-09-11 11:31:35.318  INFO 32855 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 812 ms
    2022-09-11 11:31:35.631  INFO 32855 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
    2022-09-11 11:31:35.641  INFO 32855 --- [           main] com.opos.sbweb.SbwebApplication          : Started SbwebApplication in 1.437 seconds (JVM running for 1.699)
    2022-09-11 11:33:57.338  INFO 32855 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
    2022-09-11 11:33:57.338  INFO 32855 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
    2022-09-11 11:33:57.339  INFO 32855 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
## Desarrollo del proyecto

Ahora tenemos un proyecto SpringBoot creado con Maven, así que tiene la misma estructura que todos los proyectos Maven, incluída una
clase principal, que SpringBoot ha anotado con 
[@SprinBootApplication](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/using-boot-using-springbootapplication-annotation.html). 
Pero la aplicación todavía no hace nada. El siguiente paso es crear el fichero de configuración.

Next, for Java-based configuration of Spring beans, we need to create a config class and
annotate it with @Configuration annotation. This annotation is the main artifact used by the Java-based Spring configuration; it is itself
meta-annotated with @Component, which makes the annotated classes standard beans and as
such, also candidates for component-scanning.
The main purpose of @Configuration classes is to be sources of bean definitions for the Spring
IoC Container.

## Notas

**Ejecutar una aplicación springboot con argumentos**
Si queremos usar la terminal:

$ mvn spring-boot:run -Dspring-boot.run.arguments="Hello world"

le pasa a la aplicación dos argumentos. Si no ponemos las comillas dará un error del tipo:

[ERROR] Unknown lifecycle phase "world". You must specify a valid lifecycle phase or a goal in the format <plugin-prefix>:<goal> or 
<plugin-group-id>:<plugin-artifact-id>[:<plugin-version>]:<goal>. Available lifecycle phases are: validate, initialize, 
generate-sources, process-sources, generate-resources, process-resources, compile, process-classes, generate-test-sources, 
process-test-sources, generate-test-resources, process-test-resources, test-compile, process-test-classes, test, prepare-package, 
package, pre-integration-test, integration-test, post-integration-test, verify, install, deploy, pre-clean, clean, post-clean, 
pre-site, site, post-site, site-deploy.

Si queremos usar VSCode:
- crear en .vscode un fichero launch.json y abrirlo
- clic en Añadir configuración y seleccionar la que queremos

Ejemplo de configuración "lanzar pidiendo argumentos":

{
  "configurations": [
  {
    "type": "java",
    "name": "Launch with Arguments Prompt",
    "request": "launch",
    "mainClass": "com.opos.sbweb.SBwebApplication",
    "args": "${command:SpecifyProgramArgs}"
  }
  ]
}

Ahora cuando lanzamos la aplicación en la parte superior de la ventana se abre una caja de texto para introducir los argumentos.

**Como consumir servicios REST proporcionados por un webservice**

- manualmente
  - desde el navegador
  - usando curl
  - usando postman
- programáticamente
  - con RestTemplate

