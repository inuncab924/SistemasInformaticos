# Los fines de semana los pingüinos dicen la hora y los ñúes la piensan

## Enunciado

Se propone configurar la máquina virtual utilizada en prácticas
anteriores mediante el planificador de tareas `cron` primero, y mediante
una unidad `systemd` de usuario de tipo `timer` después, para que el
fichero `hora_animal.txt` de nuestra carpeta personal, cambie de
contenido **CADA HORA Y MEDIA**, a partir de las doce de la noche del
sábado, y lo vaya haciendo durante **TODO EL FIN DE SEMANA** (sábado y
domingo), alternando entre los dibujos mostrados más abajo en los que
además aparece **la hora a la que se ha producido el cambio**.

A las doce de la noche del sábado el pingüino dice la hora, a la una y
media el ñu piensa la hora, a las tres de la madrugada el pingüino dice
la hora, a las cuatro y media de la mañana el ñu piensa la hora, ... y
así durante todo el fin de semana.

Los **LUNES** a las **DOCE DE LA NOCHE** el fichero debe ser eliminado,
para no volver a aparecer hasta que no llegue de nuevo el fin de semana
siguiente.

Tened en cuenta que los días empiezan a las `0:00` de la noche, es
decir, pasamos de viernes a las `23:59` a sábado (y por tanto fin de
semana) a las `0:00` (es decir, al minuto siguiente)

Hay que generar un fichero `pinguinos_y_nues.cast` que muestre y
que explique mediante comentarios en consola (texto que empieza con `#`)
cómo se ha configurado cada planificador de tareas (primero `cron` y
después `systemd.timer`) para lograr el objetivo final.

## Hora animal

1. Contenido del fichero `hora_animal.txt` a las *... en punto*

    ```asciiart
     --------------------
    < Hablé a las 0:00 >
     --------------------
       \
        \
            .--.
           |o_o |
           |:_/ |
          //   \ \
         (|     | )
        /'\_   _/`\
        \___)=(___/
    ```

2. Contenido del fichero `hora_animal.txt` a las *... y media*

    ```asciiart
     --------------------
    ( Pensé a las 1:30 )
     --------------------
        o               ,-----._
      .  o         .  ,'        `-.__,------._
     //   o      __\\'                        `-.
    ((    _____-'___))                           |
     `:='/     (alf_/                            |
     `.=|      |='                               |
        |)   O |                                  \
        |      |                               /\  \
        |     /                          .    /  \  \
        |    .-..__            ___   .--' \  |\   \  |
       |o o  |     ``--.___.  /   `-'      \  \\   \ |
        `--''        '  .' / /             |  | |   | \
                     |  | / /              |  | |   mmm
                     |  ||  |              | /| |
                     ( .' \ \              || | |
                     | |   \ \            // / /
                     | |    \ \          || |_|
                    /  |    |_/         /_|
                   /__/
    ```

## Observaciones

Recordad que en `linux` hay un comando para *casi* todo y que *Google* y
`man` son vuestros mejores aliados. Si hay que instalar algún paquete en
vuestras máquina, ¡hacedlo, que para eso sois los administradores de las
mismas!
