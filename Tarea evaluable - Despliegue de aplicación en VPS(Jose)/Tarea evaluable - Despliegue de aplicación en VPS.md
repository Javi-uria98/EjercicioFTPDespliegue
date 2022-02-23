author: Jose Ramón López Marrón

[TOC]

# Instalación y administración de servidores de transferencia de archivos

## VirtualBack

Instalamos mysql en el servidor

![image-20220215202700065](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220215202700065.png)

 Y lo configuramos

![image-20220215202439687](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220215202439687.png)

Creando una base de datos

![image-20220215202539326](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220215202539326.png)

Instalo Maven y el jdk

![image-20220215202940009](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220215202940009.png)

![image-20220215203030966](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220215203030966.png)

Activo el `vsftpd`, cambiando la configuración del `vaftpd.conf` ya que lo habia alterado en la prueba anterior, u reiniciando el `vsftpd`

![image-20220216155756150](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216155756150.png)

Descargo un proyecto de github, haciendo un `bash` en una carpeta y clonando el repositorio en el mismo

![image-20220216155954830](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216155954830.png)

Tras esto lo subo al servidor con Filezilla

![image-20220216160020560](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216160020560.png)

Ejecuto `mvn clean` para elminar el `target`

![image-20220216160140700](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216160140700.png)

Ejecuto `mvn instal`l para generar el `.jar`

![image-20220216160526728](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216160526728.png)

Vemos que se ha creado el `tarjet`, por lo que procedo a ejecutar el `.jar`, abriendo el puerto 8080

![image-20220216162232613](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216162232613.png)

![image-20220216160633114](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216160633114.png)

En el anfitrión accedemos a la ip, miramos la lista de tíos que hay, y en concreto el que tiene id 4 (que no existe)

![image-20220216162440552](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216162440552.png)

![image-20220216162453165](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216162453165.png)

![image-20220216162515927](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220216162515927.png)

Ahora convertimos en ejecutable el `.jar`, de modo que al arrancar el servidor virtual todos los programas en los que corre la aplicación se inicie al arrancar el sistema

![image-20220217164642676](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220217164642676.png)

![image-20220217164907523](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220217164907523.png)

![image-20220217165001877](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220217165001877.png)

Descargo la carpeta de back en mi escritorio, mediante un ftp al servidor `172.16.0.100`

![image-20220217171155366](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220217171155366.png)







## VirtualFront

Abro los puertos 80 `y` 4200 para poder acceder a la aplicación web desde el navegador

![image-20220222204607135](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220222204607135.png)

Tuve algunos problemas con los permisos, a causa de la tarea del despliegue de una aplicación en VPS, por lo que no podía acceder a los ficheros del `/var/www/html` para subir la aplicación

![image-20220222204637020](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220222204637020.png)

Para subir los diferentes ficheros, tuve que darle permiso en la carpeta.

![image-20220222204704213](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220222204704213.png)

Mediante la ejecución de la instrucción `code` en GitBash, me abre el VisualStudio donde puedo editar y ejecutar comandos para desplegar la aplicación. Para ello ejecuto el comando `npm run ng serve -o`

![image-20220223155214599](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223155214599.png)

![image-20220222204729986](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220222204729986.png)

Tras crear el archivo necesario para el despliegue lo despliego con `npm run ng build --prod`, que me crea la  carpeta dist. El comando `npm run` es un comando que me resulto necesario para ejecutar las instrucciones `ng`

![image-20220222204802914](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220222204802914.png)

![image-20220223155546651](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223155546651.png)

Comprobamos que al acceder al servidor mediante Google Chrome, podemos acceder a la aplicación de forma correcta, donde tenemos un usuario con mi nombre

![image-20220223155721893](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223155721893.png)

De igual modo, al acceder a la pestaña de "Ver", sección destacada del usuario, podemos comprobar los detalles del usuario.

![image-20220223160649302](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160649302.png)



Así como podemos editar tanto el nombre como el correo

![image-20220223160734280](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160734280.png)

Por último, creamos un nuevo usuario, que será el que utilicemos para borrar, y como vemos en las siguientes capturas, nos salen los datos debajo del anterior "tío". Al eliminarlo, nos salta una alerta que nos dice que ha sido eliminado, por lo que ya no aparece mas en la tabla

![image-20220223160811801](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160811801.png)

![image-20220223160831654](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160831654.png)

![image-20220223160933244](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160933244.png)

![image-20220223160945394](Tarea%20evaluable%20-%20Despliegue%20de%20aplicaci%C3%B3n%20en%20VPS.assets/image-20220223160945394.png)

