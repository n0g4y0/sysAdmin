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

- 

