

# Tarea FTP - despliegue de aplicación en VPS

> Trabajo realizado por Manuel Macho Calvín

[TOC]



# Objetivo

Desplegar una aplicación web `Full Stack` en un **VPS**, siguiendo las indicaciones de la `playlist` facilitada en el aula virtual (https://www.youtube.com/watch?v=ibIn4qIniv8&list=PL4bT56Uw3S4zIHpaNjz4OVAmXGkYgLpjo&ab_channel=LuigiCode). Como Servidor Privado Virtual (VPS) se utilizará el servidor remoto instalado en la nube privada del aula

# Descripción de la tarea

## VirtualBack

Lo primero que necesitamos es instalar una base de datos de `mysql`

```bash
sudo apt-get install mysql-server mysql-client
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/2.mysql.png)

Una vez descargado todos los paquetes probamos a entrar como *`root`*

```bash
sudo mysql
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/3.acces.png)



Realizamos la instalación segura:

```bash
sudo mysql_secure_installation
```

A continuación hacemos cambios para poder entrar con una contraseña propia:

```bash
alter user 'root'@'localhost' identified with mysql_native_password by 'tutorial';
```

Seguimos para crear la base de datos con el nombre '`test_virtual`' coincidiendo con la que aparece en el proyecto `virtualBACK`:

```bash
mysql -u root -p
create database test_virtual;
show databases;
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/4.create.png)

Creamos un usuario al que llamaremos *`user`* para poder tener acceso:

```bash
create user 'user'@'localhost' identified by 'user';
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/4.create.png)

Y le damos privilegios a `user`:

```bash
grant all privileges on test_virtual.* to 'user'@'localhost' with grant option;
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/6.grant.png)

Seguidamente tenemos que mirar que versión de `java`  y de `javac` tenemos:

```bash
java -version
javac -version
```

Al no tener versión de `jdk`, la instalaremos y seguidamente el `openjdk-8`:

```bash
sudo apt-get install maven
sudo apt-get install openjdk-8-jdk
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/7.maven.png)

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/8.jdk.png)

Comprobamos la version:

```bash
java -version
javac -version
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/9.version.png)

A continuación instalamos un servidor ftp, en nuestro caso utilizaremos **`vsftpd`**:

```bash
sudo apt-get install vsftpd
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/10.vsftd.png)



Lo habilitamos para escritura:

```bash
sudo nano /etc/vsftpd.conf
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/11.write.png)

Reiniciamos el servidor `vsftpd` y comprobamos si está habilitado:

```bash
sudo systemctl restart vsftpd.service
sudo systemctl is-enabled vsftpd.service
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/12.enabled.png)



En este punto desde el repositorio que nos referencia clonamos el proyecto `virtualBACK`  a una carpeta de nuestro escritorio. 

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/CAPTUR1.PNG)





```bash
git clone https://github.com/cavanosa/virtualBACK.git
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/1CLONA1.PNG)

Una vez clonado el proyecto desde el **FileZilla** cliente subimos el proyecto al servidor ftp, a nuestra carpeta `/home/master` .

<!--En este punto tengo explicar que tuve bastantes problemas para ejecutar el fichero .jar. Es por eso que tuve que repetir en diversas ocasiones el proceso,hasta en tres ocasiones, por lo cual muchas capturas del inicio del proceso no se corresponde en nombre con los nombres en el proceso final, ya que fueron nombradas de otra manera.-->

<!--Uno de los problemas tuve que solucionarlo eliminando una dependencia del pom.xl. que no me reconocia. Con eso lo pude solucionar.--> 

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/0200.PNG)

Comprobamos efectivamente que la transferencia mediante el ftp se ha realizado correctamente:

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/5COMPR1.PNG)



A continuación nos dirigimos a la carpeta `virtualBACK/` creada y ejecutamos para eliminar el target en el caso de que lo hubiera:

```bash
mvn clean
```



![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/13.clean.png)



![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/13.1.clean.png)

Editamos el pom.xml:

```bash
sudo nano pom.xml
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/14.pom.png)



Generamos el `.jar`

```bash
mvn install
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/81COMA1.PNG)

Ejecutamos el `.jar` creado:

```bash
java -jar target/virtual.jar
```



![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/8PROBA1.PNG)



La aplicación se desplegará en el puerto `8080`. Podemos comprobar que se está ejecutando el `spring`

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/001.PNG)



![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/002.PNG)



Hacemos comprobaciones y miramos que nos devuelva la lista, en este caso al estar vacía, nos devolverá un array vacio:

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/003.PNG)

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/004.PNG)



A continuación lo que haremos es convertirlo en ejecutable. Para que nada más arrancar el S.O. se ejecute el `.jar`

Para ello hay que crear el servicio y lo haremos creando un archivo para tal fin:

```bash
sudo nano /etc/systemd/system/spring.service
```



A continuación lo habilitamos:

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/005.PNG)

Lo iniciamos y comprobamos que efectivamente está activo

```bash
sudo systemctl is-enabled spring.service
sudo systemctl is-active spring.service
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/006.PNG)





![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/007.PNG)

Ya tenemos activa la aplicación. Con esto ya estaría configurado `spring` correctamente

Finalmente aplicamos la actualización correspondiente:

```bash
sudo apt-get update
```

![](Tarea%20FTP%20-%20despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/008.PNG)





## VirtualFRONT

Primeramente instalaremos el Apache

```bash
sudo apt-get install apache2
```

