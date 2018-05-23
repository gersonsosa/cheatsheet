pliegue remoto con Maven
=========================

## Maven ##
Maven es una herramienta que permite manejar un proyecto Java, facilitando la administración de dependencias y muchas tareas como compilar, empaquetar, correr pruebas unitarias y desplegar o instalar un proyecto Java.

> **Tip:** Descargar maven desde [<i class="icon-download"></i>Descarga maven](https://maven.apache.org/download.cgi).


----------


Compilar
-------------------

Para compilar un proyecto se debe ir en consola hasta la carpeta donde esta ubicada la raíz del proyecto, usualmente donde esta ubicad el archivo pom.xml

Este archivo especifica la configuración del proyecto como las dependencias y parámetros de despliegue entre otros.

Una vez ubicado se debe ejecutar el siguiente comando

    mvn compile
Si la compilación es exitosa maven dará un mensaje como el siguiente
```
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building my_project 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[WARNING] The POM for com.jidesoft:jide-oss:jar:3.5.0 is invalid, transitive dependencies (if any) will not be available, enable debug logging for more details
[INFO] 
[INFO] --- maven-resources-plugin:2.3:resources (default-resources) @ my_project ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] Copying 1 resources
[INFO] Copying 1 resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.1:compile (default-compile) @ my_project ---
[INFO] Nothing to compile - all classes are up to date
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.504s
[INFO] Finished at: Fri Sep 11 10:45:07 COT 2015
[INFO] Final Memory: 9M/152M
[INFO] ------------------------------------------------------------------------
```
De otra forma mostrará los errores.

#### <i class="icon-upload"></i> Desplegar remotamente a Jboss

Una vez se compila el proyecto es posible asegurarnos que el despliegue a un servidor de la aplicación va a ser exitoso.

Antes de desplegar es necesario hacer todas las operaciones en archivos por ejemplo en algunos proyectos java de aplicaciones WEB la base de datos puede estar ubicada en otro servidor y es necesario especificar esta información en algún archivo de propiedades.

Para desplegar remotamente a Jboss es necesario especificar en el archivo pom.xml la dirección **IP** del servidor y las credenciales del usuario *Managment*  de Jboss al que queremos desplegar así:

```
...
    </plugin>
    <plugin>
        <groupId>org.jboss.as.plugins</groupId>
        <artifactId>jboss-as-maven-plugin</artifactId>
        <version>7.7.Final</version>
        <configuration>
            <name>my_project.war</name>
            <hostname>xxx.xxx.xxx.xxx</hostname>
            <username>user_name</username>
            <password>password</password>
        </configuration>
    </plugin>
</plugins>
```
Y posteriormente ejecutamos el comando 

    mvn jboss-as:deploy
Esto desplegará la aplicación en el servidor remoto.

Luego de desplegar es necesario regresar el archivo pom.xml a su estado anterior y todos los archivos de propiedades ya que maven utiliza estos archivos cada vez que compila el proyecto al igual que Eclipse, si dejamos estos archivos configurados así Eclipse o Maven no podran desplegar por ejemplo en la misma maquina de desarrollo.

>* Gerson Sosa Arquitecto de software, Helio4
