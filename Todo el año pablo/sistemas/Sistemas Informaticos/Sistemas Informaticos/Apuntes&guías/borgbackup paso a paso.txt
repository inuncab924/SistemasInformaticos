#ALT+Space -> Abrimos el perfil creado con las dimensiones correctas(que se creó previamente) -> eldeseo -> Enter -> asciinema rec borgbackup.cast -i2


##1º  → Instalamos el paquete BorgBackup → sudo apt install borgbackup → Si este está ya instalado, no debemos de volver a instalarlo a no ser que sean dos máquinas diferentes.

##2º  → Creamos un repositorio en la carpeta tmp → cd /tmp → mkdir repositorio → borg init --encryption=repokey /tmp/repositorio → Nos pedirá una contraseña, la escribimos y posterior pulsamos enter.

##3º  → creamos un archivo cualquiera, por ejemplo -> echo Hola > saludo.txt -> borg create /tmp/repositorio::<nombrecopia> saludo.txt (Para crear una copia)

	La primera ruta es la ruta donde se encuentra el repositorio. La segunda ruta es la ruta del archivo o directorio del que queremos guardar la copia. De esta forma hacemos copia del saludo.txt

Podemos hacer otro archivo y hacer otra copia para ver que se hacen bien

##4º  -> borg list /tmp/repositorio -> debería salir la copia que hemos hecho

Si hacemos borg list /tmp/repositorio::<nombre de la copia de seguridad> sale solo la copia de seguridad de ese archivo/directorio. Si hemos hecho saludo.txt y otro archivo distinto, podemos ver los dos.
También si hacemos un borg info /tmp/repositorio veremos información de este

##5º  → Realizamos cambios a los ficheros -> echo Hola de nuevo > saludo.txt -> Añadimos otro archivo con el echo, por ejemplo -> echo Adios > despedida.txt → borg create /tmp/repositorio::<nombrecopia> saludo.txt despedida.txt

##6º  → Ahora compararemos ambas copias
borg diff /tmp/repositorio::<nombre primera copia> <nombre segunda copia>

##7º  → Creamos otra carpeta en /tmp llamada “restored”. Estando en /tmp -> mkdir restored -> cd /tmp/restored -> Queremos extraer una de esas copias de seguridad y para ello tendremos que hacer un borg extract /tmp/repositorio::<nombrecopia>

	La copia se extrae en la carpeta en la que estamos, así que nos tenemos que asegurar que estamos en /tmp/restored

##8º Paso → Finalmente comprobamos que la restauración ha sido correcta mediante el ls en la carpeta tmp y mediante el diff comprobaremos las diferencias entre la copia y la extracción, los cuales deben ser iguales.