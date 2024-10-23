# Instalaciones desatendidas con FAI

## Resultados de aprendizaje trabajados en la actividad

- :SIINF.2: Instala sistemas operativos planificando el proceso e
  interpretando documentación técnica

## Enunciado

Hacer una grabación `asciicast` en la que se muestre cómo se monta una
arquitectura FAI de instalaciones desatendidas

Usaremos el programa `tmux` para mostrar varias terminales en pantalla,
pudiendo usar los siguientes atajos de teclado:

- `C-b %` : para dividir horizontalmente
- `C-b "` : para dividir verticalmente
- `C-b <UP DOWN RIGHT LEFT>` : para movernos por los paneles
- `C-b z` : para maximizar/restaurar el tamaño que ocupa un panel

Inicialmente tendremos:

- Izquierda: log de `apt-cacher-ng` en máquina anfitriona con el comando
  `tail -f /var/log/apt-cacher-ng/apt-cacher.log`
- Derecha: instalación de servidor FAI con debian bullseye

Posteriormente, una vez se haya instalado el servidor FAI, se crearán
dos consolas más para acabar teniendo:

- Arriba Izquierda: log de `apt-cacher-ng` en máquina anfitriona
- Arriba Derecha: consola del servidor `faiserver` y monitorización con
  `fai-monitor` (mejor acceder por `ssh` que por puerto serie)
- Abajo Izquierda: log de sistema del servidor `faiserver` con el
  comando `sudo journalctl -f` (acceder también por `ssh`)
- Abajo Derecha: instalación de cliente FAI

**IMPORTANTE**: Para grabar varios terminales simultáneamente es fundamental
ejecutar el comando de grabación **ANTES** de ejecutar el multiplexor de
terminales `tmux`. También es importante terminar correctamente la
sesión de grabación saliendo ordenadamente de todos los terminales
abiertos

**NOTA**: Si hemos hecho varias instalaciones de prueba de `faiserver` y
al intentar conectarnos a él por `ssh` obtenemos un error de
discordancia de claves, podemos solventarlo borrando el fichero
`known_hosts` haciendo `rm .ssh/known_hosts` en nuestra máquina física

## Creación de máquina virtual para alojar el servidor FAI

```
    virt-install \
        --name faiserver \
        --memory 1024 --disk size=4 --graphics none \
        --network bridge=virbr1 \
        --location /srv/SIINF/debian-11.5.0-amd64-netinst.iso \
        --console pty,target.type=serial \
        --extra-args "console=ttyS0 theme=dark"
```

La red en `virbr1` no tiene servidor DHCP, con lo que hay que configurar
manualmente la interfaz de red de la máquina `faiserver`:

- IP: 192.168.123.2
- Máscara de red: 255.255.255.0
- Default Gateway: 192.168.123.1
- DNS: 192.168.123.1

Utilizar como nombre de usuario a la hora de instalar el servidor fai el
que hemos utilizado otras veces formado por nuestros apellidos y nombre

Instalamos el paquete `fai-quickstart`, que será el que nos permita
montar el servidor FAI para instalaciones desatendidas:



Configuramos la creación de un usuario `fai` para acceso remoto a los
logs en el fichero `/etc/fai/fai.conf`:



Configuramos el servidor FAI con `fai-setup`, pero antes también
configuramos `debootstrap` para que use nuestro proxy caché de paquetes
de la máquina anfitriona, editando el fichero `/etc/fai/nfsroot.conf` y
añadiendo la IP y el puerto correspondiente en la variable
`FAI_DEBOOTSTRAP`



Podemos ver el resultado de la configuración que ha hecho `fai-setup` en
el fichero de log `/var/log/fai/fai-setup.log`: se ha creado el usuario
`LOGUSER` y se ha configurado `nfsroot` añadiendo algunas líneas al
fichero `/etc/exports`. Así, todas las máquinas que sean instaladas con
FAI podrán montar algunos directorios fundamentales vía NFS (ver más
detalles en el apartado de observaciones)

A continuación configuramos el servidor DHCP. Para ello copiamos el
fichero de configuración de ejemplo `dhcpd.conf` a la carpeta
`/etc/dhcp`:

```
    sudo cp /usr/share/doc/fai-doc/examples/etc/dhcpd.conf /etc/dhcp/
```

y modificamos la dirección de subred, la dirección del router y la
dirección del servidor DNS. Aprovechamos también para generar 10
entradas con el one-liner en perl que aparece comentado en el fichero o
lo hacemos manualmente

También editamos `/etc/default/isc-dhcp-server` para añadir la interfaz
de red `enp1s0` a la variable `INTERFACESv4`

Una vez configurado el servidor DHCP lo reiniciamos:

```
    sudo systemctl restart isc-dhcp-server.service
```

*ALTERNATIVO*: Igualmente podríamos haber usado `dhcpd-edit` para añadir
cada línea necesaria a `/etc/dhcp/dhcpd.conf` y reiniciar al mismo
tiempo el servicio DHCP con el comando:

```
    sudo dhcp-edit client-1XX 52:54:00:11:23:XX
```

A continuación hay que crear la configuración PXELINUX para cada cliente
que queramos instalar con el comando `fai-chboot`:



Pero para una grabación detallada es importante que el kernel del
instalador FAI se inicie con soporte de puerto serie, con lo que es
conveniente añadir el parámetro correspondiente `-k "console=ttyS0"` al
ejecutar el comando `fai-chboot`:



Tambien es conveniente habilitar ese soporte de puerto serie en el GRUB
y en las sesiones de consola de los clientes FAI antes de la instalación
de los mismos. Para ello se editará el fichero
`/srv/fai/config/scripts/GRUB_PC/10-setup` en el servidor FAI añadiendo
las siguientes líneas:

```
    # enable serial console in kernel and agetty
    ainsl /etc/default/grub 'GRUB_CMDLINE_LINUX="console=ttyS0"'

    # enable serial console in GRUB
    ainsl /etc/default/grub 'GRUB_TERMINAL=serial'
    ainsl /etc/default/grub 'GRUB_SERIAL_COMMAND="serial --unit=0 --speed=9600 --stop=1"'
```

antes de la desactivación del os-prober de GRUB en:

```
    # disable os-prober because of #802717
    ainsl /etc/default/grub 'GRUB_DISABLE_OS_PROBER=true'
```

La configuración PXELINUX generada por `fai-chboot` se guarda en <br>
`/srv/tftp/fai/pxelinux.cfg/<ip_del_cliente_fai_en_hexadecimal>`

## Configuración de la instalación que se hará en los clientes FAI

El comando `fai-setup` acaba copiando como último paso el directorio
`/usr/share/doc/fai-doc/examples/simple` al destino `/srv/fai/config`.
Quizás interese más partir de una config inicial para `fai` como la que
se obtiene al generar una ISO en <https://fai-project.org/FAIme/>. Se
proporciona dicha configuración en el fichero `faiconfig.tar.gz` que
habrá que copiar (usando nuestro usuario de `faiserver`) y desempaquetar
en `faiserver`, borrando antes el directorio `/srv/fai/config`:

```
    daw1-1xx $ scp /srv/SIINF/faiconfig.tar.gz <usuario>@faiserver.fai:
    faiserver $ sudo rm -rf /srv/fai/config
    faiserver $ sudo tar vxf faiconfig.tar.gz -C /
```

Es bueno configurar un proxy caché de paquetes editando la variable
`APTPROXY=http://192.168.123.1:3142` en el fichero de configuración
`/srv/fai/config/class/DEBIAN.var`. De esta manera, la instalación de
cada cliente FAI no tendrá que bajarse los paquetes de internet

En `/srv/fai/config/class/FAIME.var` debemos cambiar el nombre del
usuario que se creará en la instalación (según las iniciales de nuestros
apellidos y nombre), así como su contraseña que se encuentra hasheada en
ese fichero, y se puede generar mediante el comando `mkpasswd` que
proporciona el paquete `whois`:

```
    echo <contraseña> | mkpasswd -m md5 -s
```



## Instalación desatendida de clientes FAI

A partir de aquí, podemos arrancar los clientes FAI y la instalación
tendrá lugar de forma totalmente desatendida. Para ello habrá que crear
una máquina virtual con soporte de arranque PXE, enganchada a la red
`fai` y con la MAC que hemos configurado en el servidor DHCP:



Para poder hacer una grabación detallada recuerda crear clientes
FAI sin hardware gráfico (usando el parámetro `--graphics none`) y
monitorizarlos e interactuar con ellos mediante una consola serie



## Observaciones

Podemos monitorizar el progreso de las instalaciones desatendidas
ejecutando `sudo fai-monitor` en la máquina `faiserver`

El paquete debian `ipxe-qemu` de nuestras máquinas anfitrionas no
habilita arranque PXE con soporte de consola serie, por lo que ha habido
que recompilarlo con esa opción habilitada en el fichero
`src/config/console.h`, descomentando la línea 37

Si no hemos habilitado el soporte de puerto serie, tanto en el propio
GRUB como en las sesiones de consola, y ya hemos instalado la máquina
cliente, se puede habilitar el mismo editando `/etc/default/grub` y
poniendo `GRUB_CMDLINE_LINUX="console=ttyS0"` y `GRUB_TERMINAL=serial`,
y acto seguido actualizar grub con el comando `sudo update-grub`

Si tenemos habilitado el usuario `fai` en `faiserver`, una vez terminada
con éxito la instalación, el propio instalador `fai` copiará los logs de
instalación en el servidor `faiserver` y desabilitará el arranque PXE
para la máquina recién instalada añadiendo el prefijo `.disable` al
fichero correspondiente en el directorio `/srv/tftp/fai/pxelinux.cfg/`

Podemos volver a habilitar el arranque PXE de esa máquina haciendo:

```
    $ sudo fai-chboot -e client-1XX
```

En la práctica y al estar usando máquinas virtuales creadas con el
comando `virt-install` y el parámetro `--pxe`, no es importante
deshabilitar el arranque PXE de los clientes, pues estos lo tendrán
activo sólo en el primer arranque antes de que se haya instalado nada en
la máquina virtual. En una máquina real sí que tendría sentido que se
produjera la deshabilitación para no instalar el sistema operativo cada
vez que se arranca la máquina

Por último, es importante observar que el servidor `faiserver` exporta
los directorios `/srv/fai/nfsroot` y `/srv/fai/config` a los clientes
FAI por NFS, usándose el primero como sistema de ficheros raíz del
instalador FAI y el segundo como directorio de configuración de la
instalación. Ambos directorios se han creado al ejecutar el comando
`fai-setup`


