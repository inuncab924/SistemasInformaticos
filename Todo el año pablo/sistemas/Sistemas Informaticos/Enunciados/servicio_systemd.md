# Creando un servicio de eco con `systemd`

## Enunciado

Se propone crear una unidad `systemd` de usuario que implemente un
servicio de eco que transforme letras minúsculas en mayúsculas.

Para ello se deben seguir los pasos que se indican a continuación, y
generar un fichero `servicio_eco.cast` con `asciinema` que muestre que
se ha logrado configurar el servicio.

La tarea debe realizarse en la máquina virtual debian que tenemos
instalada y que hemos usado para las actividades anteriores.

## Unidades de usuario y unidades de tipo socket

Hemos visto que `systemd` sirve para, entre otras muchas cosas,
gestionar servicios del sistema mediante las llamadas *unidades*.

También se pueden crear unidades a nivel de *usuario*, y no sólo a nivel
de sistema, con lo que no es necesario ser administrador para poder
tener nuestros propios servicios que se van a ejecutar cuando iniciamos
una sesión.

Hemos mencionado que hay muchos tipos de *unidades*, pero sólo hemos
visto las que son de tipo `service`. En esta actividad crearemos una
unidad de tipo `service` y otra de un tipo nuevo, llamado `socket`.

Las unidades de tipo `socket` estarán asociadas a las de tipo `service`,
de modo que cuando se active la primera porque un cliente externo accede
al puerto configurado en la unidad de tipo `socket`, inmediatamente se
va a iniciar la segunda. Son por tanto una buena manera de lanzar
servicios bajo demanda.

## Servicio de eco

Se proporciona un pequeño programa en `python` que implementa un
servicio de eco. Su objetivo es actuar como el fenómeno acústico del
mismo nombre y devolver una copia (casi) idéntica de los datos que
recibe.

    #!/usr/bin/python3

    import sys
    sys.stdout.write(sys.stdin.readline().strip().upper() + '\r\n')

Este programa, cuando se ejecuta, lee una cadena de texto de la entrada
estándar, transforma las letras minúsculas en mayúsculas y las devuelve
como resultado por la salida estándar.

Deberemos copiarlo en el fichero `eco.py` en la carpeta `~/bin/` de
nuestro directorio personal, que habrá que crear si no existe. También
deberemos darle permisos de ejecución a ese fichero.

## Creación de unidades para `systemd`

Vamos a crear dos unidades de usuario para `systemd`, una de tipo
`socket` y otra de tipo `service`.

La unidad de tipo `socket` nos va a permitir escuchar en el puerto
`6666` y lanzar nuestro servicio cada vez que un cliente intente
conectarse a dicho puerto.

La crearemos con el siguiente comando:

    systemctl edit --user --force --full eco.socket

Se nos abrirá un editor de texto en el que hay que introducir el texto
que se muestra a continuación, que sirve como descripción de la unidad:

    [Unit]
    Description=Servidor de eco

    [Socket]
    ListenStream=6666
    Accept=yes

    [Install]
    WantedBy=sockets.target

La unidad de tipo `service` ejecutará el código `python` de nuestro
servicio de eco y la crearemos, igualmente, de la siguiente forma:

    systemctl edit --user --force --full eco@.service

Atención al nombre de la unidad, que es el mismo que el de la unidad socket
anterior, pero con un carácter `@` al final (y luego la coletilla `.service`)

Se nos vuelve a abrir el editor para introducir el texto que describe la
unidad:

    [Unit]
    Description=Servicio para el servidor de eco

    [Service]
    ExecStart=%h/bin/eco.py
    StandardInput=socket

Atentos al código de esta unidad: no tiene sección `[Install]` y por
tanto no puede ser habilitada, con lo que siempre estará en estado
`static`.

## Habilitado de la unidad en el arranque

Si queremos que la unidad `eco.socket` creada se ejecute cada vez que
iniciamos sesión habrá que habilitarla con:

    systemctl --user enable eco.socket

Si no hacemos esto, será necesario lanzar la unidad manualmente cada vez
que queramos que nuestro servicio de eco funcione:

    systemctl --user start eco.socket

## Comprobación del funcionamiento del servicio

Para comprobar que el servicio ha sido configurado correctamente se
pueden usar comandos del siguiente tipo:

    $ echo hola mundo | nc localhost 6666
    HOLA MUNDO

Con esto se puede ver que el servicio de eco realiza su tarea y nos
devuelve como salida, los caracteres que introducimos como entrada, pero
con las letras minúsculas transformadas en mayúsculas. La cadena de
caracteres que sirve como entrada se le pasa a nuestro servicio de eco
mediante el programa `nc` (netcat) usando el puerto `6666`

## Observaciones

Las *unidades de usuario*, se manejan igual que las de sistema que hemos
visto en clase, pero añadiendo el parámetro `--user` a todos los
comandos `systemctl`.

La *unidad* de tipo `service` asociada a la *unidad* de tipo `socket` no
hay que iniciarla, pues ya se encargará esta última de usarla de forma
correcta (aparece como `static` si hacemos un listado de los ficheros de
unidad de usuario). La unidad de tipo `socket` sí que tiene que ser
iniciada para que todo funcione.
