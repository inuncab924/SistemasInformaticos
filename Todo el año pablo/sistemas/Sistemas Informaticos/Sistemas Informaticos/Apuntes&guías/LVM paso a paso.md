LVM

Instalación de máquina pero usando volúmenes lógicos y a posterior hacer operaciones como redimensionado de ficheros …

## 1.-Principio

Aciinema rec deblvm.cast -i 2 (En la grabación se debe de explicar y hablar únicamente de la parte donde configuramos los volumenes lógicos)

Comienzo tmux (Izquierda se escribe y se usan comandos, derecha se crea la máquina virtual)

Para crear la máquina usamos este comando:

virt-install --name deblvm --ram=1024 --disk size=4 --disk size=4 --graphics none --location /srv/SIINF/debian-11.5.0-amd64-netinst.iso --console pty,target.type=serial --extra-args "theme=dark console=ttyS0"

Para el hostname de la máquina será deblvm y a posterior nombre de usuario igual a la anterior

A la hora de partición--> ‘Guided - use entire disk and set up LVM’ --> Escoger el primer disco que sale --> ‘Separate /home partition’-> Dejar ‘3.8GB’ (que está por defecto)

Para copiar fix-term:
Hay que poner primero en la máquina física (se queda expectante hasta que no se haga en la virtual)
nc -q 0 -l -p 1234 < /srv/SIINF/fix-term
…y luego en la virtual (entonces la física deja de estar esperando)
	nc 10.0.2.2 1234 > fix-term
^esto viene dentro del archivo fix-term en sí, que lo podéis abrir

En la virtual:
	sudo nano .profile
al final del todo pegar:

if [ -f "$HOME/fix-term" ]; then
. "$HOME/fix-term"
fi

<|CUESTIONES A EXPLICAR|>
-Realizar una instalación usando LVM en la que expliques qué está ocurriendo en cada una de las pantallas correspondientes a la fase del particionado

-Analizar cuidadosamente las particiones físicas creadas, los grupos de volúmenes, así como los volúmenes lógicos, indicando claramente qué tamaño tienen y para qué se usan en cada caso

-El instalador crea una partición que monta en /boot. ¿Para qué sirve esa partición?
¿Existe algún motivo por el que lo haga de esa manera?

-Explica las diferencias entre el particionado que hay en tu máquina física y el que ha hecho el instalador en la máquina virtual




## 2.- PV

Ponemos sudo fdisk -l y sale la lista de discos, ahí miramos cual es el segundo (que debería ser vdb) y es con el que tenemos que crear el PV.
	sudo pvcreate /dev/vdb -> Añade un PV en la segunda partición.
-Añade ese PV al único VG que hay creado ahora mismo
sudo vgextend deblvm-vg /dev/vdb  asi me ha salido sucesfully
Hacemos un sudo pvs para ver si está correctamente tendrían que salir 2.
Lo importante es que ambas tengan el mismo nombre en la columna VG porque eso significa que pertenecen al mismo grupo.

se hace sudo lvs para ver los LV y sudo vgs para ver los VG
sudo lvextend -L+3G  /dev/deblvm-vg/root (Para poder añadirle 3g mas)

sudo resize2fs /dev/deblvm-vg/root  (Redimensionado ese volumen lógico)

df -h /mnt/

si hacemos eso sale el giga y poco

df -h /mnt/ al hacerlo después del resize cambia



Apagamos la máquina virtual y ponemos
	virsh vol-list --pool default
Salen todas las máquinas virtuales que tenemos. Debemos añadir los 2GB a la segunda, que se debería llamar ‘deblvm-1.qcow2’
Si ponemos
	du -h /home/alumno/.local/share/libvirt/images/deblvm-1.qcow2
y luego
	du -h /home/alumno/.local/share/libvirt/images/deblvm.qcow2
podemos ver el tamaño de cada uno
Hacemos 
qemu-img resize /home/alumno/.local/share/libvirt/images/deblvm-1.qcow2  +2G

…y con eso añadimos 2GB. Volvemos a entrar en la máquina virtual y hacemos 
	sudo fdisk -l
y si lo hemos hecho bien, /dev/vdb debería salir con 6Gb en lugar de 4Gb como antes.
Para redimensionar el PV asociado:
sudo pvresize /dev/vdb
Con sudo pvs comprobamos que /dev/vdb tiene un tamaño de 6GB con 3GB libres

[···········]

Ahora hay que hacer lo mismo, pero con el primer disco. Apagamos la máquina de nuevo.
Ponemos virsh vol-list --pool default para ver la ruta.
qemu-img resize /home/alumno/.local/share/libvirt/images/deblvm.qcow2 +2G


sudo fdisk /dev/vda -> m -> n -> p -> enter -> enter -> enter -> se añaden los 2GB -> w
sudo pvcreate /dev/vda3 -> sudo vgextend deblvm-vg /dev/vda3 -> sudo lvextend -l +100%FREE /dev/deblvm-vg/home




 