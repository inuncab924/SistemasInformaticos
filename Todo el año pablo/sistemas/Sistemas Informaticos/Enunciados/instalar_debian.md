# SIINF

## Instalación de debian bullseye con `libvirt`

### Resultados de aprendizaje trabajados en la actividad

- :SIINF.2: Instala sistemas operativos planificando el proceso e
  interpretando documentación técnica

### Objetivo

Instalar el sistema operativo linux (distribución debian) en una máquina
virtual KVM creada con las herramientas de `libvirt`, usando una consola
serie para interactuar con el instalador

Manejar los comandos básicos de `virsh` para gestionar máquinas
virtuales

### Enunciado

Haz una grabación asciicast en un fichero `instalar_debian.cast` en la
que se grabe cómo se llevan a cabo las siguientes acciones y se
responda a las preguntas propuestas:

Instala debian bullseye en una máquina virtual creada con `virt-install`
de 1GB de RAM, 4GB de disco duro, sin hardware gráfico y con consola
serie conectada al puerto `ttyS0`. Llama `bullseye` a la máquina tanto a
nivel de `libvirt` como a nivel de sistema operativo

    $ virt-install \
        --name bullseye \
        --memory 1024 \
        --disk size=4 \
        --graphics none \
        --location /srv/SIINF/debian-11.5.0-amd64-netinst.iso \
        --console pty,target.type=serial \
        --extra-args "console=ttyS0 theme=dark"

Observa que para hacer la grabación asciicast no vamos a tener hardware
gráfico, pero vamos a emplear una consola serie que hay que añadir al
hardware de la máquina virtual creada y cuyo uso hay que habilitar en el
kernel del instalador mediante el argumento extra correspondiente

Explica las diferencias que hay en el nombre de máquina tanto a nivel de
dominio `libvirt` como a nivel de sistema operativo

No usaremos cuenta para el usuario administrador `root`. Explica las
implicaciones que eso conlleva

Como "Full Name" emplearemos nuestro nombre completo con ambos apellidos
y como "User Name" emplearemos las tres primeras letras de nuestro
primer apellido, seguidas de las tres primeras letras de nuestro segundo
apellido, seguidas de las dos primeras letras de nuestro nombre, si es
un nombre simple, o la primera letra de cada nombre si es un nombre
compuesto. En total tendremos 8 caracteres. Explica las diferencias
entre ambos nombres de usuario

Usa tu nombre de usuario como contraseña para evitar que se te olvide

Haremos un particionado guiado, usando todo el disco y con todo el
sistema de ficheros en la misma partición. Analiza qué otras opciones de
particionado puedes utilizar desde el instalador, y compara unas con
otras

¿Qué consecuencias se te ocurre que puede tener la selección de un uso
horario concreto en el instalador?

Vamos a configurar un proxy/caché de paquetes. Debes explicar para qué
se utiliza y qué repercusión tiene su uso en el aula

No instalaremos interfaz gráfica, pero sí instalaremos un servidor de
acceso remoto seguro SSH. ¿En qué contextos es útil este tipo de
instalaciones? ¿Por qué? También deberías dejar que se instalen las
utlidades estándar del sistema

Uno de los últimos pasos que lleva a cabo el instalador está relacionado
con el gestor de arranque. ¿En qué consiste exactamente ese paso? ¿Qué
ocurriría si no lo ejecutásemos?

<!--

Copia también el fichero `/srv/SIINF/fix-term` de tu máquina
anfitriona a la máquina virtual haciendo uso de `netcat`. Si ejecutas
ese fichero nada más iniciar una sesión de consola por el puerto serie
evitarás problemas de corrupción de terminal. Para ello puedes añadir lo
siguiente al final de tu fichero `.profile`, usando el editor `nano`:

    if [ -f "$HOME/fix-term" ]; then
        . "$HOME/fix-term"
    fi

El copiado del fichero lo puedes hacer de la siguiente manera:

    virtual> nc 10.0.2.2 1234 > fix-term
    anfitriona> nc -q 0 -l -p 1234 < /srv/SIINF/fix-term

-->

¿Qué ocurre cuando inicias una sesión de consola e intentas usar la
letra "eñe" o los acentos? Intenta documentarte para arreglarlo,
explicando en todo momento lo que estás haciendo. ¿Ocurría esto en la
primera instalación que hicimos en clase?. ¿Sabrías decir por qué?

Por último, averigua el número total de paquetes que has acabado
instalando, así como el espacio total de disco que se está usando. ¿Cómo
lo compararías con las instalaciones de otros sistemas operativos a las
que estés habituado?  ¿Qué ventajas e incovenientes encuentras en la
instalación realizada respecto a la de esos otros sistemas operativos?

Termina la instalación saliendo de la sesión "mediante tirón" del cable
serie y apagando el dominio `libvirt` mediante el uso del subcomando
`virsh` correspondiente

**IMPORTANTE**: Haz la grabación en una sesión de consola, y justo
después de empezar a grabar utiliza `tmux` para dividir la pantalla
verticalmente en dos paneles. En el panel de la izquierda deja en forma
de comentarios la descripción de lo que estás haciendo y las respuestas
a las preguntas planteadas. Utiliza el panel de la derecha para hacer la
instalación
