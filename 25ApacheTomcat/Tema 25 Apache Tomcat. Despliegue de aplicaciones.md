# Tema 25 Apache Tomcat. Despliegue de aplicaciones

[Basic tutorial](https://www.tutorialkart.com/apache-tomcat/apache-tomcat-tutorial/)

[Web Containers](https://www.knowprogram.com/servlet/servlet-container/)

[Referencia](https://tomcat.apache.org/tomcat-10.1-doc/introduction.html)

De la referencia hay que leer los apartados:

1) Introduction
2) First webapp

## Introduction

Throughout the documentation, there are references to the two following properties:

- `CATALINA_HOME`: Represents the root of your Tomcat installation, for example /home/tomcat/apache-tomcat-9.0.10
- `CATALINA_BASE`: Represents the root of a runtime configuration of a specific Tomcat instance. If you want to have multiple Tomcat 
  instances on one machine, use the `CATALINA_BASE` property.

If you set the properties to different locations, the `CATALINA_HOME` location contains static sources, such as .jar files, or binary 
files. The `CATALINA_BASE` location contains configuration files, log files, deployed applications, and other runtime requirements.

Before you start using `CATALINA_BASE`, first consider and create the directory tree used by `CATALINA_BASE`. Note that if you do not 
create all the recommended directories, Tomcat creates the directories automatically. If it fails to create the necessary directory, 
for example due to permission issues, Tomcat will either fail to start, or may not function correctly.

Consider the following list of directories:

- `/bin` directory with the setenv.sh, setenv.bat, and tomcat-juli.jar files, startup, shutdown, and other scripts.
  Order of lookup: `CATALINA_BASE` is checked first; fallback is provided to `CATALINA_HOME`.
- `/lib` directory with further resources to be added on classpath.
  Order of lookup: `CATALINA_BASE` is checked first; `CATALINA_HOME` is loaded second.
- `/logs` directory for instance-specific log files.
- `/webapps` directory for automatically loaded web applications (this is where your webapps go).
  Order of lookup: `CATALINA_BASE` only.
- `/work` directory that contains temporary working directories for the deployed web applications.
- `/temp` directory used by the JVM for temporary files.
  We recommend you not to change the tomcat-juli.jar file. However, in case you require your own logging implementation, you can replace the tomcat-juli.jar file in a `CATALINA_BASE` location for the specific Tomcat instance.

We also recommend you copy all configuration files from the `CATALINA_HOME/conf` directory into the `CATALINA_BASE/conf/` directory. In 
case a configuration file is missing in `CATALINA_BASE`, there is no fallback to `CATALINA_HOME`. Consequently, this may cause failure.

At minimum, `CATALINA_BASE` must contain:

- `conf/server.xml`
- `conf/web.xml`

That includes the conf directory. Otherwise, Tomcat fails to start, or fails to function properly.
For advanced configuration information, see the RUNNING.txt file. 
## Tomcat vs full application servers

Desde un punto de vista lógico un servidor de aplicaciones tiene esta estructura:

```{thumbnail} images/arquitectura.png
```

El Tomcat es una versión supersimplificada de un servidor de aplicaciones. Solo tiene la capa de web container. En Tomcat esa capa
se llama Catalina.

Un servidor "completo" como Wildfly proporciona muchos servicios:

- tiene una capa de middleware con servicios como seguridad, inyección de dependencias, transacciones, pool de conexiones a BBDD...
- puede ejecutar simultáneamente varias instancias de la misma aplicación
- tiene un balanceador para repartir la carga entre las distintas instancias

Tomcat no dispone de esas funcionalidades, simplemente sirve una instancia de cada una de las aplicaciones web que tiene alojadas.

Las aplicaciones web en el fondo son servlets (server components) que sirven contenido dinámico (JSPs, JSFs, etc.), pero ya no se 
desarrollan como cuando se creó esta tecnología.

Al principio desarrollar servlets era muy complejo, se hacía usando directamente la clase Servlet y compañía. Hoy en día se utilizan
frameworks como Spring y SpringBoot que hacen mucho más simple el desarrollo a base de enterrar los servlets bajo un montón de capas.
## First webapp

A web application is defined as a hierarchy of directories and files in a standard layout. Such a hierarchy can be accessed in its 
"unpacked" form, where each directory and file exists in the filesystem separately, or in a "packed" form known as a **Web ARchive**, 
or WAR file. The former format is more useful during development, while the latter is used when you distribute your application to be 
installed.

The top-level directory of your web application hierarchy is also the document root of your application. Here, you will place the HTML 
files and JSP pages that comprise your application's user interface. When the system administrator deploys your application into a 
particular server, they assign a context path to your application. 
Thus, if the system administrator assigns your application to the context path /catalog, then a request URI referring to 
/catalog/index.html will retrieve the index.html file from your document root.

To facilitate creation of a Web Application Archive file in the required format, it is convenient to arrange the "executable" files of 
your web application (that is, the files that Tomcat actually uses when executing your app) in the same organization as required by the 
WAR format itself. To do this, you will end up with the following contents in your application's "document root" directory:

- *.html, *.jsp, etc. - The HTML and JSP pages, along with other files that must be visible to the client browser (such as JavaScript, 
  stylesheet files, and images) for your application. In larger applications you may choose to divide these files into a subdirectory 
  hierarchy, but for smaller apps, it is generally much simpler to maintain only a single directory for these files.
- /WEB-INF/web.xml - The Web Application Deployment Descriptor for your application. This is an XML file describing the servlets and 
  other components that make up your application, along with any initialization parameters and container-managed security constraints 
  that you want the server to enforce for you. This file is discussed in more detail in the following subsection.
- /WEB-INF/classes/ - This directory contains any Java class files (and associated resources) required for your application, including 
  both servlet and non-servlet classes, that are not combined into JAR files. If your classes are organized into Java packages, you 
  must reflect this in the directory hierarchy under /WEB-INF/classes/. For example, a Java class named 
  com.mycompany.mypackage.MyServlet would need to be stored in a file named /WEB-INF/classes/com/mycompany/mypackage/MyServlet.class.
- /WEB-INF/lib/ - This directory contains JAR files that contain Java class files (and associated resources) required for your 
  application, such as third party class libraries or JDBC drivers.
  When you install an application into Tomcat (or any other 2.2 or later Servlet container), the classes in the WEB-INF/classes/ 
  directory, as well as all classes in JAR files found in the WEB-INF/lib/ directory, are made visible to other classes within your 
  particular web application. Thus, if you include all of the required library classes in one of these places (be sure to check 
  licenses for redistribution rights for any third party libraries you utilize), you will simplify the installation of your web 
  application -- no adjustment to the system class path (or installation of global library files in your server) will be necessary.

Much of this information was extracted from Chapter 9 of the Servlet API Specification, version 2.3, which you should consult for more 
details.

```{thumbnail} images/Captura.PNG
```
