# Instalando debian con particionado LVM

## Resultados de aprendizaje trabajados en la actividad

- :SIINF.3.d: Se han creado diferentes tipos de particiones y unidades
  lógicas

## Enunciado

Vamos a instalar un sistema mínimo debian en una máquina virtual con las
siguientes características:

- Dos discos duros de 4G cada uno
- Memoria RAM: 1G
- Nombre de la máquina debian: deblvm
- Nombre de usuario: tres letras del primer apellido, tres letras del
  segundo apellido, dos letras del nombre si es simple y una de cada
  nombre si es compuesto

La instalación se hará a partir de la ISO de debian que hay en la
carpeta `/srv/SIINF` usando el primer disco duro completo y configurando
el LVM desde el instalador. Inicialmente no hay que tocar nada del
segundo disco duro

Se usará particionado guiado y separaremos la partición `/home`.
Instalaremos GRUB en el MBR del primer disco duro

Graba un asciicast llamado `deblvm.cast` en el que se muestre cómo se
realizan los pasos que se muestran a continuación. Deberás utilizar
`tmux` para dividir la pantalla verticalmente en dos paneles, usando el
panel de la izquierda para explicar mediante comentarios lo que estás
haciendo y el de la derecha para hacer la instalación:

- Realizar una instalación usando LVM en la que expliques qué está
  ocurriendo en cada una de las pantallas correspondientes a la fase del
  particionado
- Analizar cuidadosamente las particiones físicas creadas, los grupos de
  volúmenes, así como los volúmenes lógicos, indicando claramente qué
  tamaño tienen y para qué se usan en cada caso
- El instalador crea una partición que monta en `/boot`. ¿Para qué sirve
  esa partición?
- ¿Existe algún motivo por el que lo haga de esa manera?
- Explica las diferencias entre el particionado que hay en tu máquina
  física y el que ha hecho el instalador en la máquina virtual

Posteriormente, y una vez esté instalado el sistema, graba también como
se llevan a cabo las siguientes acciones:

- Crea un PV que use el segundo disco duro en su totalidad
- Añade ese PV al único VG que hay creado ahora mismo
- Extiende en 3G adicionales el tamaño del LV usado para la raíz del
  sistema de ficheros. No olvides redimensionar ese sistema de ficheros
  con el comando `resize2fs`
- Apaga la máquina virtual creada, añade 2G al fichero imagen del
  segundo de los discos duros, vuelve a encender la máquina y comprueba
  con `fdisk` que el tamaño de ese disco ha crecido en consonancia.
  Redimensiona el PV asociado a este dispositivo y comprueba que el VG
  también crece en consonancia
- Vuelve a apagar la máquina, y vuelve a añadir 2G, pero esta vez al
  fichero imagen del primero de los discos duros (el dispositivo en el
  que se hizo la instalación)
- Enciende de nuevo la máquina, y comprueba que el tamaño de ese primer
  disco ha crecido en consonancia. Crea una nueva partición física
  primaria que ocupe todo ese espacio nuevo (comando `fdisk`)
- Añade la partición recién creada al LVM, es decir, crea un nuevo PV en
  esa partición. Añade ese PV al VG con el que estamos trabajando y
  comprueba que su tamaño vuelve a crecer en consonancia
- Añade todo el espacio que te queda en el VG al LV usado para `/home`
  (ya no es necesario recordar que tienes que redimensionar el sistema
  de ficheros correspondiente con `resize2fs`, ¿verdad?). ¿Hacer este
  último paso sería una buena práctica? ¿Por qué?

## Más info

- <https://blog.inittab.org/administracion-sistemas/lvm-para-torpes-i/>
- <https://blog.inittab.org/administracion-sistemas/lvm-para-torpes-ii/>
- <https://blog.inittab.org/administracion-sistemas/lvm-para-torpes-iii-ampliando-espacio/>
- <https://linuxconfig.org/how-to-resize-a-qcow2-disk-image-on-linux>
