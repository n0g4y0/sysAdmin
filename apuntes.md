# Administrador de Servidores - Platzi
- **GNU/LINUX** es el sistema operativo que es la mas usada en cuanto a los servidores.

### Manejo de Sesiones Remotas con Tmux y SSH.

- lo mas recomendable es guardar las llaves SSH en la carpeta:
 > .ssh/

- En vez de conectarnos **asi**:
    _$ ssh -i ~/.ssh/platzi.pem ubuntu@54.196.172.97_

    quisiera conectarme **asi**:
    _$ ssh platzi_

    deberias configurar **ASI**:


- *Ademas que es la manera mas segura de conectarse mediante llaves ssh :*
  - crear un archivo config, dentro de .ssh/
  - Dentro de **config** escribimos:
    ```
    Host Platzi
        Hostname 54.196.172.97 (cualquier ip)
        Port 22
        User ubuntu
        IdentityFile ~/.shh/platzi.pem
    ```
  - una vez guardado, podemos ejecutar:
    - _$ ssh platzi_

### Tmux

- atajos de teclado para TMUX, [aqui](http://www.sromero.org/wiki/linux/aplicaciones/tmux).

- Comandos rapidos:
  - **(CTRL + b)  c :** -> crea una nueva instancia de terminal.|
  - **(CTRL + b) n :** -> es el numero de instancia que quiero ir.
  - **man tmux :** mas informacion acerca de los atajos de teclado.
  - **(CTRL +b) " :** divide la ventana actual _VERTICALMENTE_  
  - **(CTRL +b) % :** divide la ventana actual _HORIZONTALMENTE_
  - **(CTRL +b) o :** selecciona la siguiente terminal cortada, en la actual ventana.
  - **(CTRL +b) d :** desconecta la sesion de tmux, y la guarda temporalmente.
  - **tmux attach :** restaura la session de terminal guardada.

### Directorio de archivos en linux

- todo en el mundo linux (GNU-LINUX) esta representado como un archivo.

```
/bin, archivos binarios de los usuarios del sistema.
/boot, guarda los archivos del arranque del sistema.
/dev, guarda las definiciones de todos los dispositivos.
/etc, archivos de configuración del sistema.
/home, se almacenan los archivos de cada usuario.
/lib, almacena las librerías del sistema.
/lib64, almacena las librerias del sistema de 64 bits.
/lost+found, espacio temporal donde se guardan datos que se recuperan despues de la caida del sistema.
/media, cuando montamos en el sistema dispositivos, los podemos ver en esta carpeta.
/mnt, cuando montamos en el sistema dispositivos, los podemos ver en esta carpeta.
/opt, almacenas los programas instalados de terceros.
/proc, sistema de archivo virtual que lo crea y destruye el sistema. Contiene informacion del mismo. (forma anarquica)
/root, almacena los archivos del super usuario Root.
/run, datos variables en tiempo de ejecucion. Informacion del sistema desde el ultimo booteo.
/sbin, archivos binarios del administrador.
/srv, archivos de datos especificos para cada servicio instalado en el sistema.
/sys, evolucion de /proc pero localizado de forma jerarquica.
/tmp, almacenamiento de archivos temporales .
/usr, programas instalados por defecto.
/var, se utiliza para guardar archivos de logs, backups, servidor web.

```

- El sistema Linux, **esta compilado de forma dinamica**, es decir, las librerias son necesarias para correr cualquier Binario.
  - ejm: _FIREFOX necesita los binarios minimos para instalarse, sino, simplemente no se va a poder instalar_
- El sistema windows tiene una forma de compilacion estatica, ya que los instaladores tienen todo el programa ahi, incluido instaladores, eso incrementa el tamaño del instalador.
- el directorio **/root** carga con el sistema base inicial, por eso tambien no se encuentra en /home.
- cual es la diferencia entre **/bin** y **/sbin** ? es el PATH, el **/sbin** esta listo para poder ser utilizado para un usuario ROOT, o hacerse pasar por ROOT al ejecutar un determinado comando (ejm):
> /sbin/ifconfig (para un usuario sin privilegios de poder utilizar ese comando).
- para saber donde esta instalado un determinado PAQUETE (PROGRAMA) (ejm):
```
# dpkg -L firefox

```
- **/var/mail** -> se envian los correos del sistema localmente.
  - **/var/log** -> todos los reportes del sistema. (logs.)

- el directorio mas importante : **/etc**
  - estan todos los archivos de configuracion del sistema.
  - No es el directorio mas pesado, pero el mas importante.

- **/boot** se configura el sector de arranque, es la pieza mas importante para arrancar el sistema, (UEFI,GRUB, aqui) sin esto el sistema queda irreparable.
  - este directorio es el unico, que no se encuentra encriptado cuando todo el sistema lo esta.

### Repositorios

- son la fuente de Software oficial de una determinada distribucion, ademas se puede agregar manualmente otros No oficiales.

- con la opcion: **_# apt-get update_** actualiza toda la lista de configuraciones de paquetes.

- cada uno de los repositorios, existe con una llave (GPG).
- la llave GPG permite saber si un repositorio es confiable o no.
- **# apt-cache search keyring** busca los anillos de seguridad, por lo general estan instalados por defecto, sino, instalarlos, en un servidor es preferible instalar las **versiones estables**.

### Como instalar paquetes

- los paquetes en la mayoria de las distribuciones, ya estan compilados, lo que hace uno es simplemente `descargarlos`.

- **_# apt-get_** me permite instalar paquetes o administrar los paquetes.
  - **# apt-get remove nombre-paquete** : elimina el paquete nombrado.
  - **# dpkg -P nombre-paquete** : elimina todo el programa mas archivos relacionados, pero no quita los programas instalados por _DEPENDENCIA._

- **aptitute:** es un programa que me permite administrar o instalar componentes (similar a synaptic).
    - **/** para buscar componentes.
    - **+** para marcar el paquete instalar paquetes (tambien todo lo que se necesita instalar).

### Empaquetar y comprimir Archivos.

- *Empaquetar:* es agarrar muchos archivos y directorios y dejarlos todos en un solo archivo.

- *Compresion:* Agarrar ese archivo (empaquetado) y comprimirlo.

- ejm:

  - para empaquetar la carpeta doc/, llamandolo _doc.tar_

    >**# tar cvf doc.tar doc/**

  - para compresion, utilizo gzip (le agrega un .gz al archivo):

    >**# gzip -9r doc.tar**    

- para descomprimir:

    >**# tar xvfz doct.tar.gz**

- el comando **tar** me mantiene los permisos de los archivos descomprimidos.

- el sistema automaticamente crea un punto _.gz_ despues de **X** tiempo (ya sea Ngix,apache,postgresql) en el directorio **/var/log**
(siempre pone los _logs_ comprimidos).

### Compilar

- en debian y ubuntu, estan precompilados los paquetes, solo hay que bajarse los binarios.

1. se tiene que instalar:
  - **_# apt-get install module-assistant_** (me permite realizar algunas configuraciones).

  - - **_# m-a --help** (es una ayuda para compilar librerias o modulos que van directamente al kernel).

  - - **_# m-a prepare** (Busca, dependiendo del kernel que tengamos, y va a instalar una serie de paquetes necesarios para compilar.

2. Agregar la funte de datos, en el **/etc/apt/sources.list**:

3. descargamos la fuente:

  -   - **_# apt-get source paquete-a-descargar_** (me permite descargar todo lo relacionado a ese programa).

  -   - **_# apt-get built-dep paquete_** (me permite descargar todas las dependencias que le falta al determinado programa).

  -   - **_# apt-get source -b paquete-a-descargar_** (me permite compilar el determinado programa).

### Documentacion.

- **# zless archivo.gz** comando que me permite visualizar un archivo comprimido sin descomprimirlo.
- los comandos **man** e **info** me permiten saber la Documentacion de un determinado programa.
  - para busqueda, apretar el signo **_/_** dentro del comando  ´man.´

  - Todos los programas deben tener Documentacion.
### Administrar Discos y particiones en Linux.

- Las particiones aparecen para segmentar un disco.

- Las particiones y discos duros en linux **se pueden montar** en caliente, similar a un pendrive USB.

- Comandos:
  - `dmseg` **->** lista el buffer del núcleo.
  - `fdisk -l` **->** lista de las particiones del disco, tambien me permite **particionar** un disco.
    - este comando actua sobre un disco duro (/dev/sdX), **NO** sobre una particion.

Notas importantes
Parámetros fdisk.
n-> nueva partición
d-> eliminar partición.
t-> modificar tipo de partición.
q-> salir sin guardar.
w-> salir y guardar los cambios. (**guarda los cambios!!**)

**NOTA**: mientras no se guarden los cambios, no afecta al disco.
- dentro de una particion logica **(extendida)**, se pueden crear mas particiones (_actua como un contenedor_).
