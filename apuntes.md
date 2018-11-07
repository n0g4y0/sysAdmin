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
  - **(CTRL + b)  c :** -> crea una nueva instancia de terminal.
  - **(CTRL + b) n :** -> es el numero de instancia que quiero ir.
  - **man tmux :** mas informacion acerca de los atajos de teclado.
  - **(CTRL +b) ":** divide la ventana actual _VERTICALMENTE_  
- **(CTRL +b) %:** divide la ventana actual _HORIZONTALMENTE_

- **(CTRL +b) o :** selecciona la siguiente terminal cortada, en la actual ventana.

- **(CTRL +b) d :** desconecta la sesion de tmux, y la guarda temporalmente.

- **tmux attach :** restaura la session de terminal guardada.
