    systemd               (init)   <- PID 1
(proceso padre de       (programa)
todos los procesos)


  ·servicio: Proceso que ofrece al usuario realizar algo.

  ·systemctl: Administrador de sistema que permite examinar y controlar el systemd y el gestor de servicios.


$ systemctl | status <servicio> -> Permite ver el estado de un servicio concreto.
            |        --type=service --state=running -> Muestra solo los servicios que se estén ejecutando.
            |
            | start <servicio> -> Inicia el servicio.
            | stop <servicio> -> Para el servicio.
            | restart <servicio> -> Para el servicio y lo inicia (hace un 'stop' y luego un 'start').
            | reload <servicio> -> Recarga los ficheros de configuración del servicio. Se usa si se ha cambiado la configuración, para no tener que parar el servicio.
            | enable <servicio> -> Habilita un servicio para se se inicie con la máquina.
            | disable <servicio> -> Deshabilita un servicio para que se inicie con la máquina.
            | cat <servicio> ->
            | daemon-reload ->
            | edit -> Permite editar ficheros de configuración.
            | poweroff ->
            | reboot ->

*Si se deshabilita el servicio red de la máquina virtual, no se puede acceder a través del ssh, pero sí desde consola serie (usando virsh start).

$ ssh lorzaman@faiserver.fai -> Conectarse desde la maq. física a la virtual mediante ssh.

