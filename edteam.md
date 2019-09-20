# Administrador de Servidores - EDTEAM

## QUE ES UN SERVIDOR WEB
- Es un programa informatico, que procesa informacion, del lado del servidor.
- la respuesta obtenida final, se renderiza en un navegador web, del lado del cliente.

### EJEMPLOS:
- apache, NGINX, apache tomcat, cherokee, IIS7 (de microsoft, bastante facil de configurar).
- Glasfish (usado para aplicaciones java, actualmente descontinuado.)
- apache (apache2) es el programa de servidor mas utilizado.


### NOTA APARTE: EJECUTAR UN SCRIPT EN POWERSHELL EN WINDOWS 10:

- se debe entrar a la consola de powershell, en modo administrador.
- ejecutar el comando **Set-ExecutionPolicy remotesigned**, y poner como respuesta: *_S_*, esto para permitir la ejecucion de scripts.

- guardar un script como ejemplo: ejm: habilita y deshabilita interfaces de red, cada cierto tiempo, entre ei ethernet y el wifi.

´´´

sleep 5
netsh interface set interface "Ethernet" disabled

sleep 900
netsh interface set interface "Wi-Fi" disabled
netsh interface set interface "Ethernet" enabled

sleep 5

netsh interface set interface "Ethernet" enabled

sleep 5

´´´

- para ejecutar: en modo consola ejecutar: **.\ejemplo.ps1** // suponiendo que el script se llama ejemplo.


# SERVIDOR APACHE
- de codigo abierto, fundado en 1995.
- **Beneficios:**
    - Modular
    - Multiplataforma
    - Extensible
    - codigo Abierto
    - Popular

    - NOTA: cuando tienes mucho trafico, o muchas cosas concurrentes, es mejor usar *NGINX*.
        - NGINX no es capaz de procesar codigo PHP, por eso necesita implementar FPM o fastCGI para procesar php.
        - APACHE es como un tanque,tiene muchas configuraciones y modulos, es lento, pero ROBUSTO.
- **opciones para la instalacion:**
    - desde los repositorios de la distribucion:
        - **# apt-get install apache2**
    - desde su repositorio particular:
        - **# add-apt-repository ppa:ondrej/apache2**
        - luego ejecutar: *apt-get udpate*

- los sitios de una pagina, una vez instalado apache, se los almacena en la ruta: **/var/www/html**


# MODOS MULTIPROCESAMIENTO APACHE

- por defecto, en versiones anteriores de apache (version 14 hacia atras), viene la configuracion por defecto. (multiprocesamiento Prefork)

    - **MULTIPROCESAMIENTO PREFORK (mpm-prefork)**
        - mas compatibilidad
        - usa procesos en lugar de hilos (o llamados THREADS)
        - mayor estabilidad
        - mayor consumo de recursos (no thread safe - mod-php).
        - este PREFORK, no es acto para la optimizar el servidor.
        - la mayoria de instalaciones, ejm WORPRESS o cualquier SITE, generalmente se utilizar el MOD-PHP

    - **MULTIPROCESAMIENTO WORKER (mpm-worker)**
        - menor compatibilidad
        - usa procesos y threads
        - menor consumo de recursos (solo soporta thread-safe)
        - NO SOPORTA MOD-PHP (usar php-fpm, similar a fastCGI)
    
    - NOTA: CGI es una interface comun, cuando quieres conectar algo, utilizas el modulo CGI, la version mas moderna es el fastCGI.


    - **MULTIPROCESAMIENTO EVENT (mpm-event)**
        - a partir de las versiones de ubuntu 16, ya viene por defecto esta version.
        - basado en mpm-worker
        - mejoras en rendimiento en peticiones con KEEP-ALIVE (mantener una conexion viva por un determinado tiempo) que son muy comunes.
        - NO SOPORTA EL MOD-PHP (usar en vez el PHP-FPM)


# ARCHIVOS DE CONFIGURACION APACHE

    - existe una documentacion sobre el tema de configuraciones, (aqui)[https://httpd.apache.org/docs2.4] 
    - en el directorio **/etc/apache2/** encontraremos los archivos de configuracion de apache:
        - *apache2.conf*
        - *ports.conf*  
    - dentro de la configuracion de **/etc/apache2/apache.conf**:
        - modificar el **timeout**, es una de las modificaciones mas importantes para ataques de los servidores, dejarlo entre 30 y 60 segundos.

# VIRTUALHOST en APACHE.
- apache tiene la facilidad de procesar varios subdominios en un mismo servidor.
- *como tener varios subdominios, en un mismo servidor?*: facil, con VIRTUALHOST.
- **PARA ESTE EJEMPLO:**
    - suponemos crear el sitio: *apache1.nativa.tech*
    - creamos la configuracion dentro de */etc/apache2/sites-available* con el nombre *apache1.nativa.tech.conf* , ahi modificamos el directorio donde se encuentra el proyecto.
        - habilitamos tambien el ServerName: *apache1.nativa.tech*.
    - ahora debemos poner el sitio en la carpeta *sites-enabled*, dentro de apache, es decir, habilitamos el proyecto con el comando: 
        - **# a2ensite apache1.nativa.tech.conf** // previamente, debe haber la configuracion en la carpeta */etc/apache2/sites-available*.
    - finalmente, recargar el servicio apache: 
        - **# service apache2 reload**

# QUE ES NGINX e INSTALACION

- es un servidor web, la competencia de APACHE, tambien utilizado como proxy reverso.
- es un servidor de archivos estaticos.
- NGINX por defecto, no soporta PHP, para solucionar este tema, se utilizan algunos modulos,como FASTCGI.
- **BENEFICIOS:**
    - Servidor de archivos estaticos
    - Proxy inverso
    - Soporta HTTP y HTTP2 sobre SSL
    - es de codigo abierto.
    - Soporte para FASTCGI.
- para la instalacion:
    - **# apt-get install nginx**
- para agregar un repositorio particular:
    - **add-apt-repository ppa:nginx/stable**
- **NOTA:** instalar la herramienta **tree** dentro de los archivos, para ver un resumen de los archivos.
- el archivo mas importante se encuenta en **/etc/nginx/nginx.conf**


# CONFIGURACION NGINX

- NGINX solo puede procesar archivos estaticos.
- para archivos dinamicos(es decir, manipulacion de codigo) se debera instalar modulos adicionales.
- las maneras recomendadas de calcular las siguientes variables son:
    - **worker_processes**
        - **# grep processor /proc/cpuinfo | wc -l** // numero de CORES que tiene el servidor.
    - **worker_connections** 
        - **ulimit -n** // cuantos procesos por cada THREAD puede manejar el servidor web.

- Es muy importante saber las directrices para configurar de buena manera el servidor, para asi evitar que colapse dicho servidor.
- entre esas directrices esta:
    - **keepalive_timeout**:
        - Asigna el valor que las conexiones permaneceran activas.
        - Posterior a ese Intervalo de tiempo, NGINX cerrara las conexiones establecidas.
    - **Gzip**:
        - esta directiva puede reducir la cantidad de trafico de red, pero hay que ser cautelosos, ya que incrementar mucho este valor, puede reducir el rendimiento del servidor WEB.

- **NOTA**: ya no se utiliza mucho el protocolo SSL, para cuestiones de seguridad,se utiliza actualmente el **TLS**. que es mas robusto y seguro.


# NGINX VIRTUALHOST

- las configuraciones de virtual host en NGINX son muy parecidas a las de apache.
- Solo que es necesario establecer el enlace simbolico entre las carpetas de *AVAILABLE* y *ENABLE* para el correcto funcionamiento.
- para crear una virtualHOST, haremos los siguientes pasos:
    - crear el archivo de configuracion en **/etc/nginx/sites-availables** con el nombre del proyecto: ejm: *nginx1.nativa.tech*
    - ejemplo del archivos de configuracion:
        ´´´
        server{
                listen 80;
                server_name nginx1.nativa.tech;
                root /var/www/html/nginx1;
                index index.html;

                error_log /var/log/nginx/nginx1.nativa.tech_error.log;

                access_log /var/log/nginx/nginx1.nativa.tech_error.log;




        }


        ´´´
    - finalmente,crear un enlace simbolico hacia la carpeta **/etc/nginx/sites-enabled**:
        - **ln -s /etc/nginx/sites-available/nginx1.nativa.tech /etc/nginx/sites-enabled/nginx1.nativa.tech**
    - reiniciar el servicio :
        - **# service nginx restart**

# STACK LAMP


- LAMP quiere decir: Linux, Apache, Mysql, PHP.



- eliminamos nginx, si es que tuvieramos instalado:
    - **# apt-get remove nginx**
    - **# apt-get purge nginx**
- El comando **# apt autoremove** elimina alguna dependencia colgada.
- **NOTA:** no eliminar los directorio o archivos de la ruta: **/var/www/html/**.
- instalamos el **mysql server**:
    - **# apt-get install mysql-server-5.7**
- una vez instalado, podemos ejecutar la consola con la instruccion:
    - **# mysql -u root -p**. (por defecto la contraseña esta en blanco.)
- pasamos a instalar ahora el PHP 7.0:
    - **# apt-get install php7.2**

- AHORA, pasamos a instalar un modulo de apache, para integrar las anteriores herramientas, para que me permita trabajar con PHP:

 - **# apt-get install libapache2-mod-php7.2**

 - finalmente, necesitamos un modulo mas, para que PHP pueda ver a MYSQL:

    **# apt-get install php7.2-mysql**


# PONIENDO A PRUEBA UNA APLICACION:

- bajaremos una aplicacion de ejemplo, de GITHUB, EJM: 
    - git clone https://github.com/doncadavona/ana.git

- hacemos la copia del proyecto hacia el servidor, mediante el comando scp (linux, y con la ayuda de cmder desde windows):
    - **# scp -r /tmp/proyecto/* root@192.168.1.9:/var/www/html/proyecto/** (varia un poco desde windows).

- luego modificamos los db_conect.php del proyecto(segun su documentacion en git), e importamos el archivo *database.sql* del proyecto, del siguiente modo:
    - primero ingresamos a la consola de mysql:
       
        - **# mysql -u root -p**
        
    - luego creamos la base de datos:

        - **> create database anna;**
    - nos cambiamos a la nueva base de datos:
        - **> use anna;**
    - luego cargamos el archivo de base de datos del proyecto:

        - **> source database.sql;**

    - ya finalizando, salimos de la consola de base de datos:
        - **>exit**


- **NOTA:** , un ejemplo para loguearme con otro usuario:

    - **# mysql -u prueba -p -h localhost**



- como estamos dejando como proyecto por defecto, y tenemos un archivo **index.php**, debemos modificar el archivo de configuracion: **/etc/apache2/mods-available/dir.conf**, y luego del termino: *_DirectoryIndex_* debemos colocar como primera opcion: **index.php**.

- finalmente, reiniciamos el servicio apache:
    - **#service apache2 restart**
- accedemos a la ip del servidor, y deberia mostrar el proyecto.



# SERVICIOS (demonios) EN LINUX 

- Es proceso informatico no interactivo, se ejecuta en segundo plano.
- No disponen de interfaz grafica.
- No hacen uso de las entradas y salidas STANDARD, para comunicar los errores Y/O registrar su funcionamiento.

## GESTORES DE SERVICIOS (demonios)


- SysV Init: gestor de servicios inicial, denominado INIT.
- Upstart: mejoras en comparacion con INIT.
- **Systemd**: gestor de servicios actual.





### SysV INIT

- Es el primer proceso en ser ejecutado, posterior a la carga del kernel.
- Es ejecutado como demonio, y generalmente asume el PID 1.
- Estilo BSD, sin **run level**. (usados en slackware)
- Estilo estilo SystemV, con RUN LEVEL **/etc/rc.d**
- **PROBLEMA:** estrictamente sincrono, es decir, espera que se termine un proceso, para ejecutar otro.

### Upstart

- ofrece un modelo, basado en eventos.
- mezclaba de manera beta, INIT y SYSTEMD
- totalmente en DESUSO.


## NIVELES DE EJECUCION.

- Se crearon para definir que cosas se levantan en diferentes etapas del sistema operativo.

- **RUNLEVELS**
    - **0 : apagar**-> definimos que cosas se van a para cuando apagamos el sistema.
    - **1 : monousuario**-> se utiliza para tareas de rescate.
    - **2 : multiusuario**-> varios usuarios, se ejecutan procesos bastante limitados.
    - **3 : multiusuario con network**-> nos podemos conectar a traves de internet, la mayoria de los procesos (demonios se encuentran aqui).
    - **5 : modo grafico con multiusuario.**-> nos podemos conectar a traves de internet, es el nivel **NORMALMENTE** usado, es el nivel **NORMALMENTE** usado.
    - **6 : reinicio.**-> utilizado para reiniciar o RELOAD los servicios.
- **NOTA**:
    - **NO SE USA el RUNLEVEL 4:** no se utiliza.


- **EN QUE RUNLEVEL estoy ACTUALMENTE?**

- averiguamos con el comando:
    - **# runlevel**, o tambien el comando **# who -r**

### Systemd


- sustituto natural para SysV INIT.
- tiene la capacidad de expresar las dependencias de los servicios.
- permite ejecutar mas trabajo paralelo al inicio de la carga del sistema.
- cada uno de los runlevel tienen un directorio de configuracion en la ruta: **/etc/rcX.d/** 
    - algunos archivos, inician con **K**, significa *KILL* ciertos procesos.
    - algunos archivos, inician con **S**, significa *START* ciertos procesos.
    - los numeros indican el numero de prioridad, van del 0 al 99.

- generalmente, el RUNLEVEL, mas usado en un servidor linux, es el nivel 3.
    - se lo puede invocar de varias maneras:
        - 2,4 (si utilizamos **SysV INIT**)
        - **runlevel3.target** o **multi-user.target** (si utilizamos **Systemd**).


 
### Systemd - Unidades de servicio

- **Systemd**: se encarga de la inicializacion y supervision de todo el sistema, que basicamente, son los demonios en si.
    -      MiDemonio.service 
    -   **(unit_name.type_extension)**

- Existen 7 tipos de unidades de servicio: 
    - **Service:** demonio (Start,Stop,Restart).
    - **Socket:** Encapsula un socket dentro del sistema de archivos, enlazados por servicios (conexion a proyectos).
    - **Device:** Encapsula un dispositivo, dentro del arbol de dispositivos de linux.
    - **Mount:** Encapsula un punto de montaje, dentro de la jerarquia del sistema de archivos.
    - **Automount:** Encapsula un punto de montaje automatico.
    - **Target:**: Utilizada para la agrupacion logica de unidades (multi-user.target).
    - **Snapshot:**: Similar a las unidades target.


## SYSTEMCTL

- Es el comando que nos proporciona LINUX, para poder interactuar de manera directa con SYSTEMD.
- Permite realizar diferentes operaciones con los diferentes servicios.
- Es una Buena practica trabajar con Systemctl para habilitar o deshabilitar servicios.
- ejemplos de los comandos mas utilizados con *systemctl*:
    - **# systemctl list-dependencies [service]** // comando que te muestra todas las dependencias que tiene ese servicio.
    - **# systemctl show [service]** // muestra todo lo que tiene configurado ese servicio.


## SYSTEMCTL - OPERACIONES CON SERVICIOS

- tenemos 2 opciones para manejar los servicios:

    - **service** **[servicio]** **[accion]** // utilizado en sistemas SysV.
    - **systemctl** **[accion]** **[servicio]** // utilizado en sistemas Systemd.

- el archivo de configuracion de servicios de **Systemd** es mucho mas clara que los servicios de **init**, ejemplo de archivos de systemd, los podemos encontrar en la ruta: **/etc/systemd/**.



## DESAFIO - CREANDO NUESTRO PROPIO SERVICIO

- El desafio es crear un demonio, que vaya creando un archivo dentro de la carpeta del usuario **eduser**.
- primero creamos un archivo bash, llamado: **edbash.sh**, dentro de la ruta: **/home/eduser**, con lo siguiente:
    - '''
        #!/bin/bash

        touch /home/eduser/ficherocreado.txt

    '''
- Le damos permiso de ejecucion, con el comando:
    - **# chmod +x edbash.sh**.


- Se crea el servicio, dentro de la ruta: **/etc/systemd/system**.
    - ejemplo, creare el servicio, con el editor vim: *ed.service*, que tiene lo siguiente:
        - '''
            [Unit]
            Description=demonio para crear un fichero en el /home de eduser
            After=network.target

            [Service]
            User=root
            ExecStart=/home/eduser/edbash.sh

            [Install]
            WantedBy=multi-user.target

            '''
- Ahora debemos activar el servicio, para eso debemos ejecutar el comando:
    - **# systemctl enable [nombre-servicio-creado]**
- luego iniciamos el servicio creado:
    - **# systemctl start [nombre-servicio-creado]**



# CERTIFICADOS SSL

- **QUE ES ?**
    - un titulo digital, que autentifica la identidad de un sitio web, y cifra con tecnologia SSL la informacion que se envia al servidor.
- un certificado SSL contiene:
    - Nombre del titular del certificado
    - Numero de serie del certificado
    - Una copia de la clave publica del titular del certificado.
    - la firma digital de la autoridad que emite el certificado.

- algunos terminos:
    - **HTTPS:** es http sobre SSL o TLS, es un protocolo de comunicacion de  tranferencia de hipertexto, de manera eficiente y segura.
    - **SSL:** es la capa de seguridad para los puertos, actualmente existe la version SSL v3.0
    - **TLS:** es la nueva capa de seguridad actual, a raiz de la vulnerabilidad de SSL, TSL significa la seguridad en la capa de transporte.


- actualmente, es recomendado utilizar certificacion TLS, y se encuentra en su version v1.3.

## TIPOS DE CERTIFICADOS

- **dominio unico:**   solo para un tipo de dominio, es de paga.
- **multiDominio SAN :** funciona para dominio y contados subdominios, es de paga.
- **WILDCARD :** cuando queremos publicar varios subdominios, dentro el dominio principal.
- **ORGANIZACION :** se deben pasar varias validaciones, para comprobar que ese dominio es tuyo.
- **EXTENDIDO :** aparece el nombre de la empresa, en la URL, es mas costoso, lo usan los bancos principalmente.
- **GRATUITO :** let's Encrypt nos brinda certificados gratuitos y auto renovables.


### LET'S ENCRYPT

- Es una autoridad de certificacion
- OBJETIVOS:
    - crear conexiones cifradas
    - brindar certificados X.509
    - suprimir los pagos.
    - reducir la configuracion para HTTPS.


### CERTBOT

- Es un cliente automatico y facil de usar que recupera y despliega de manera automatica los certificados SSL/TLS en nuestro servidor web.
- entrar a la pagina web de **certbot.org** y seguir los pasos, es muy facil.
- inicialmente desarrollado para trabajar con _let's encrypt_ pero trabaja con cualquier otra autoridad certificadora que soporte el protocolo ACME.
- por ahora no soporta WILDCARDS, ya que solo funciona para un dominio unico, tendriamos que generar para cada subdominio.





