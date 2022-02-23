---
title: Tarea FTP - Despliegue de Aplicación Web Fullstack en VPS
author: Javier Uría 
---

# Ejercicio - Despliegue de Aplicación Web Fullstack en VPS

> Tarea realizada por Javier Uría



En primer lugar, instalo mysql de forma segura y creo una base de datos llamada test_virtual que usaré para el backend de la aplicación.

Luego, instalo maven "sudo apt-get install maven" ya que con ello me instalará también el jdk (el openjdk-11). A continuación instalo el openjdk-8 "sudo apt-get install openjdk-8-jdk". Una vez instalado, compruebo la versión de java y la de su máquina virtual.

![image-20220215201653821](C:/Users/vespertino/AppData/Roaming/Typora/typora-user-images/image-20220215201653821.png)

![image-20220215201708255](C:/Users/vespertino/AppData/Roaming/Typora/typora-user-images/image-20220215201708255.png)



El siguiente paso es hacerse con la aplicación. En primer lugar, hago un git clone al repositorio de github del backend de la aplicación en mi equipo real

![image-20220216154826257](tarea%20video.assets/image-20220216154826257.png)



Una vez tengo la carpeta del back, abro el filezilla para subirla al servidor, que ya tiene instalado y configurado el vsftpd para poder realizar la transferencia

![image-20220216155127983](tarea%20video.assets/image-20220216155127983.png)

![image-20220216155148391](tarea%20video.assets/image-20220216155148391.png)

![image-20220216155310956](tarea%20video.assets/image-20220216155310956.png)



Una vez tengo el proyecto, genero el .jar mediante el comando "mvn install", que me genera una nueva carpeta llamada target

![image-20220216155820338](tarea%20video.assets/image-20220216155820338.png)



Donde tengo el .jar generado

![image-20220216155838275](tarea%20video.assets/image-20220216155838275.png)



Ahora, pruebo a ejecutar dicho .jar

![image-20220216160155583](tarea%20video.assets/image-20220216160155583.png)



Con ello, tendré la aplicación desplegada ejecutándose en el puerto 8080. Para comprobarlo, accederé desde el equipo real a la ip del servidor con dicho puerto (cabe recordar que puede ser necesario abrir dicho puerto en el cortafuegos -ufw allow-)

![image-20220216162217343](tarea%20video.assets/image-20220216162217343.png)

![image-20220217163859095](tarea%20video.assets/image-20220217163859095.png)

Y vemos que la aplicación está desplegada

Sabiendo que la aplicación funciona correctamente, lo que vamos a hacer ahora es convertirlo en ejecutable, de modo que nada más arrancar el sistema se ejecute el jar para no tener que hacerlo a mano. Para ello, primero hay que crear el servicio

![image-20220217165736926](tarea%20video.assets/image-20220217165736926.png)



Y luego, habilitarlo

![image-20220217165754677](tarea%20video.assets/image-20220217165754677.png)



Y por ultimo, iniciarlo y comprobar que esté activo

![image-20220217165924841](tarea%20video.assets/image-20220217165924841.png)



Y con esto, tendremos la aplicación corriendo siempre que el servicio esté activo. Con esto, ya estaría configurado spring correctamente. Ahora toca la parte del frontend.

El primer paso sería instalar apache. Luego, al igual que hicimos con el back de la aplicación, clonamos el front al equipo local y, por filezilla, lo subimos al vps. No obstante, antes de subirlo, habrá que realizar unos cambios al proyecto a través del visual studio. 

En primer lugar abrimos la terminal y ejecutamos un npm update para crear unos node modules que faltan.

![image-20220222193933120](tarea%20video.assets/image-20220222193933120.png)



Luego ejecutamos un ng serve -o para que nos abra el navegador con localhost:4200

![image-20220222194916691](tarea%20video.assets/image-20220222194916691.png)



Y veo que tengo la aplicación corriendo

![image-20220222195034926](tarea%20video.assets/image-20220222195034926.png)



Aunque todavía presenta un error, y es que no encuentra un recurso en localhost:8080

![image-20220222195212895](tarea%20video.assets/image-20220222195212895.png)



Esto es porque necesito cambiar la dirección que usa para acceder a ese recurso a la del VPS

![image-20220222195422549](tarea%20video.assets/image-20220222195422549.png)

![image-20220222195447650](tarea%20video.assets/image-20220222195447650.png)



Y ahora ya puedo ver que el error ha desaparecido

![image-20220222195529313](tarea%20video.assets/image-20220222195529313.png)



Y, si pruebo por ejemplo a crear un nuevo registro, funciona correctamente

![image-20220222195632877](tarea%20video.assets/image-20220222195632877.png)



Ahora que sé que este proyecto, el front, funciona correctamente, voy a subirlo a producción. Para ello, abro una terminal nueva y ejecuto el ng build --prod

![image-20220222195852501](tarea%20video.assets/image-20220222195852501.png)



Y con ello, nos genera una carpeta llamada dist, que a su vez tiene una carpeta con el nombre del proyecto, que es la que, ya sí, queremos subir al VPS junto con el back.

![image-20220222200046857](tarea%20video.assets/image-20220222200046857.png)



Esta vez, la carpeta (o mejor dicho, sus contenidos) la meteremos en /var/www/html/ en vez de donde teníamos la del back.

![image-20220222200804357](tarea%20video.assets/image-20220222200804357.png)



Y ya tendré el front de la aplicación desplegado en el VPS

![image-20220222200918984](tarea%20video.assets/image-20220222200918984.png)



Y voy probando las distintas páginas del sitio, viendo que funcionan todas (ej: detalle de un tio)

![image-20220222201016613](tarea%20video.assets/image-20220222201016613.png)



No obstante, todavía hay un fallo. La aplicación solo tiene un html (el index) ya que es una Single Page Application y el resto de páginas que vemos realmente no existen, por lo que, si recargásemos la página estando en una de estas páginas "inexistentes", nos daría un 404.

![image-20220222201528304](tarea%20video.assets/image-20220222201528304.png)



Para corregir esto, tenemos que volver al proyecto en el visual studio, y en un archivo llamado app-routing.module.ts, añadimos lo siguiente

![image-20220222201818214](tarea%20video.assets/image-20220222201818214.png)



Ahora, toca volver a producir la carpeta de dist y volver a subir su contenido al VPS. Con esto, el problema de recargar la páginas que no son index.html queda resuelto.

Y habiendo llegado hasta aquí, ya tenemos desplegada la aplicación al completo (back y front) en el VPS, completamente funcional.



