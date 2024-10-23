# SIINF

## John the Ripper

### Resultados de aprendizaje trabajados en la actividad

- :SIINF.4: Gestiona sistemas operativos utilizando comandos y
  herramientas gráficas y evaluando las necesidades del sistema

### Criterios de evaluación

- :SIINF.4.b: Se ha asegurado el acceso al sistema mediante el uso de
  directivas de cuenta y directivas de contraseñas

### Objetivo

Comprobar la facilidad con la que pueden adivinarse las contraseñas
débiles con programas como **John the Ripper**. Es importante que las
contraseñas sean largas, y con caracteres variados, conteniendo
mayúsculas, dígitos y símbolos, no sólo letras minúsculas

### Enunciado

Se propone adivinar, con ayuda del comando `john`, la contraseña de
login de algunos usuarios linux a partir del hash de la misma, leyendo
la información que se ofrece en los siguientes apartados, así como la
documentación adicional referenciada, explicando en todo momento el
procedimiento llevado a cabo mediante la redacción de un informe en
formato markdown (fichero a entregar `jtr.md`)

Se proporciona un fichero de partida con hashes de contraseñas Unix
generado mediante el comando:

    sudo unshadow /etc/passwd /etc/shadow > mypasswd

Este fichero une en la misma línea la información de los usuarios (y sus
datos GECOS) junto al hash de la contraseña (obtenido mediante la
función `crypt(3)` en alguna de sus variantes)

Es importante hacer notar que a partir de debian bullseye se utiliza
`yescrypt`, en lugar de `sha512`, como función hash aplicada a la
contraseña de usuario. Esto proporciona una mayor seguridad contra los
ataques de rotura de contraseñas basados en diccionarios, tanto en
términos de complejidad espacial como temporal del ataque

En la página man `crypt(5)` se pueden ver los distintos métodos de hash
soportados por la función `crypt(3)`, y hay que tener en cuenta que
`john` no detecta actualmente de forma automática el tipo de hash
`yescrypt`, por lo que hay que indicárselo usando el parámetro
`--format=crypt`

### Línea de comandos de `john`

Para ver un resumen general de uso del comando `john` basta invocarlo
sin argumentos, aunque es mejor añadirle el argumento `--help` para
obtener un listado de todas las opciones disponibles

Las opciones reconocidas por `john` se pueden especificar mediante un
único guión (`-`), aunque es más recomendable usar los dos guiones
(`--`) propios de los formatos largos. Si dichas opciones necesitan un
parámetro adicional lo habitual es usar el símbolo `=`, aunque también
se reconoce el símbolo `:`

Más detalles en doc/OPTIONS

### Paquetes, diccionarios y versión jumbo

Debian incorpora un paquete oficial con el software de rotura de
contraseñas `john`. Este paquete tiene una dependencia del paquete
`john-data` que incorpora el diccionario `/usr/share/john/password.lst`,
con contraseñas muy comunes proporcionadas por el autor del software
(habituales en usuarios de habla inglesa)

El paquete `john` también incorpora scripts para avisar a los usuarios
del uso de contraseñas débiles (mediante una tarea cron)

Cuidado con el diccionario incorporado que trae el paquete debian, pues
puede dar sensación de falsa seguridad, ya que las contraseñas dependen
fuertemente de la lengua materna y de cuestiones culturales en general y
podríamos creer que, al no ser adivinada por `john`, la contraseña es
fuerte. También hay que tener en cuenta que el diccionario es bastante
pequeño, con unos pocos miles de contraseñas

Una fuente de diccionarios adicional puede ser la página:

<https://wiki.skullsecurity.org/index.php/Passwords>

aunque está algo desactualizada. Los diccionarios que aparecen en el
paquete `seclists` de la distribución **Kali Linux** son más completos y
están más actualizados. Se pueden descargar también de la página:

<https://github.com/danielmiessler/SecLists>

Otra fuente de diccionarios interesante referenciada en el README.md de
SecLists es la siguiente:

<https://weakpass.com>

Los formatos de cifrado soportados por `john` son múltiples y variados.
Se puede obtener un listado de los mismos, junto a una prueba de
rendimiento mediante el comando:

    /usr/sbin/john --test

Observamos que el ejecutable `john` se encuentra en la carpeta
`/usr/sbin` que habitualmente no está en la ruta de los usuarios
ordinarios de una máquina linux

Existe una versión más completa de `john` llamada versión *jumbo* que no
sólo incorpora muchos más formatos de cifrado, sino que es mucho más
eficiente en las pruebas de rendimiento y tiene una documentación más
abundante que la versión de `john` que empaqueta debian. Es la versión
que incorpora la distribución basada en debian **Kali Linux**. También
incorpora un diccionario `password.lst` mucho más extenso que el
incorporado en el paquete debian, con casi dos millones de contraseñas

### Compilación en debian de la versión jumbo de John de Ripper

Más detalles en el fichero `doc/INSTALL-UBUNTU`

### Modos de rotura de contraseñas

Existen tres modos básicos de uso del programa `john`:

- *single*: parámetro `--single`
- *wordlist*: parámetro `--wordlist=FICHERO`
- *incremental*: parámetro `--incremental=MODO`

Si no se especifica ninguno de ellos se ejecutan los tres, uno a
continuación del otro y en el orden enumerado anteriormente:

- el modo `single` utiliza la información GECOS del usuario para
  intentar romper la contraseña
- el modo `wordlist` utiliza un diccionario de posibles contraseñas y
  habilita además un conjunto de "reglas de mezcla" definidas en el
  fichero de configuración de `john`
- el modo `incremental` utiliza el modo `ASCII` para la mayoría de
  hashes (juego de 95 caracteres imprimibles ASCII y longitud de
  contraseña entre 0 y 13) o el modo `LM_ASCII` para hashes LM de
  Windows (ambos también definidos en el fichero de configuración de
  `john`)

El fichero de configuración de `john` se encuentra en `john.conf`. Ahí
podemos ver los modos de rotura que se pueden utilizar y las reglas
adicionales para todos ellos (más detalles en el fichero `doc/CONFIG`)

Cada vez que `john` encuentra una contraseña la pinta por pantalla y la
guarda en el fichero `john.pot`, que se lee siempre que `john` se
reinicia para evitar tener que volver a adivinar contraseñas ya
descubiertas

Para ver las contraseñas ya adivinadas podemos hacer:

    john --show mypasswd

Podemos lanzar un modo de rotura particular usando los nombres que
aparecen en el fichero de configuración:

    john --format=crypt --incremental=digits --users=usuario mypasswd

Con este comando seleccionamos el modo incremental que usa sólo dígitos

Más detalles en los ficheros `doc/EXAMPLES` y `doc/MODES`

### Sesiones de rotura de contraseñas

Se puede dar nombre a las distintas sesiones de rotura de contraseñas
mediante el parámetro `--session=NOMBRE`. Si no se especifica este
parámetro se utiliza un nombre de sesión por defecto llamado `john` que
se guarda en los ficheros `john.log` y `john.rec`

Los ficheros de sesión `.log` y `.rec` para todas las sesiones que no
sean la de por defecto se guardan en la carpeta desde la que se invoca
el programa

Cualquier sesión de rotura se puede abortar pulsando la combinación
de teclas `Ctrl-C` (o `q`) y luego reanudarse con el comando:

    john --restore

o el comando:

    john --restore=NOMBRE

### Interactuando con una sesión de rotura de contraseñas

Mientras se intentan adivinar las contraseñas, se puede pulsar la tecla
`h` y `john` nos mostrará una pequeña ayuda con las teclas que reconoce
y que podemos pulsar para interactuar con la sesión de rotura

Ahí podemos leer que la pulsación de casi cualquier tecla muestra una
línea simple de estado con la siguiente información:

- recuento de aciertos (**g**)
- duración de la sesión (en formato **D:HH:MM:SS** para días, horas,
  minutos y segundos)
- indicador de progreso (porcentaje realizado y, opcionalmente, número
  de pase del número total de pasadas)
- hasta cuatro métricas de velocidad (**g/s**, **p/s**, **c/s** y
  **C/s**)
- la contraseña candidata actual (o rango de las mismas) que se está(n)
  probando en estos momentos (`john` a menudo es capaz de probar
  múltiples contraseñas candidatas en paralelo para un mejor
  rendimiento, de ahí el rango)

Las cuatro métricas de velocidad son las siguientes:

- **g/s** son los aciertos por segundo (así que se mantendrá en 0 hasta que
  se descifre al menos una contraseña)
- **p/s** es el número de contraseñas candidatas probadas por segundo
- **c/s** es el número de "crypts" (hash de contraseñas o cálculos de
  cifrado) por segundo
- **C/s** son las combinaciones de contraseña candidata y hash objetivo por
  segundo

Más detalles en el fichero `doc/FAQ`

### Reglas

Más detalles en el fichero `doc/RULES`

### Máscaras

Más detalles en el fichero `doc/MASK`

### Rotura de contraseñas Windows

Las contraseñas de usuario de Windows se almacenan en las colmenas
(ficheros) del registro de Windows denominadas `SYSTEM` y `SAM`:

- `C:\Windows\System32\config\SAM`
- `C:\Windows\System32\config\SYSTEM`

En concreto, los hashes de las contraseñas se almacenan en la colmena
SAM; sin embargo, se cifran con la clave de arranque del sistema
(bootkey), que se almacena en la colmena `SYSTEM`

En un sistema en ejecución, no se pueden copiar dichos archivos de forma
directa. Para hacer una copia de ambas colmenas se puede utilizar la
utilidad `reg`, en una consola de administrador:

    reg save HKLM\SYSTEM SystemBkup.hiv
    reg save HKLM\SAM SamBkup.hiv

Aunque si tenemos acceso a estas colmenas desde otro sistema operativo,
podremos usarlas directamente para extraer los hashes de las contraseñas
de usuario que buscamos empleando utilidades como `samdump2`:

    sudo ntfs-3g /dev/<device> /mnt
    cd /mnt/Windows/System32/config/
    samdump2 SYSTEM SAM -o /tmp/hashes

o `pwdump.py`:

    sudo ntfs-3g /dev/<device> /mnt
    cd /mnt/Windows/System32/config/
    /usr/share/creddump7/pwdump.py SYSTEM SAM > /tmp/hashes

**NOTA 1**: El comando `samdump2` está disponible en el paquete debian del
mismo nombre y el comando `pwdump.py` lo tenemos en el paquete
`creddump7`

**NOTA 2**: El comando `samdump2` sólo es válido para sistemas Windows
2k/NT/XP/Windows7 o para los Windows10 a los que no se haya aplicado la
actualización "Windows10 anniversary update". En este último caso
parece que todo funciona correctamente pero sólo obtenemos un conjunto
de usuarios con contraseñas en blanco de la forma:

admin:1001:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::

Para adivinar las contraseñas usando `john` emplearemos el formato `nt`:

    ./john --format=nt /tmp/hashes

### Uso de tablas arcoíris

Una tabla arcoíris es una tabla de búsqueda utilizada en criptografía
para almacenar hashes de contraseñas. Se llama "arcoíris" porque está
compuesta por una serie de colores que se asemejan a un arcoíris

La tabla arcoíris se utiliza para acelerar el proceso de cracking de
contraseñas mediante ataques de fuerza bruta. El proceso implica el hash
de una contraseña y la búsqueda del hash en la tabla arcoíris para
encontrar una coincidencia. Si se encuentra una coincidencia, se puede
recuperar la contraseña original

Para construir una tabla arcoíris, se generan cadenas de caracteres
aleatorios y se aplican funciones de hash a cada cadena. Estos hashes se
utilizan para buscar coincidencias con las contraseñas hash que se
quieren crackear

La idea tras el uso de las tablas arcoíris es que los algoritmos de
búsqueda son más veloces que los algoritmos de generación de hashes, por
lo que va a ser más rápido encontrar un hash precalculado de entre el
enorme conjunto de todos ellos, que calcular una serie potencialmente
enorme de hashes hasta que se produzca la coincidencia

Las tablas arcoíris se pueden generar si disponemos de la suficiente
potencia de cómputo, e incluso se puede hacer de forma distribuida y
colaborativa, o se pueden descargar de internet. Para usarlas tendremos
que emplear herramientas como `RainbowCrack` o `rtcracki_mt`, que
incluso están empaquetados en distribuciones como **Kali Linux**

### Generación de hashes desde la línea de comandos

Para generar el hash de una contraseña se puede utilizar el programa
`mkpasswd` del paquete `whois`

Por ejemplo, para generar el hash de 10 contraseñas aleatorias de seis
dígitos que usen el método `sha256crypt` se puede usar un script como el
siguiente:

    #!/usr/bin/env bash
    for i in {1..10}; do
      N=$(printf '%06d' $(shuf -i 0-999999 -n 1))
      mkpasswd --method=sha256crypt --stdin <<< ${N}  >> hashes.txt
    done

Estos hashes se pueden emplear para hacer pruebas de adivinación con el
comando `john`

### Conteo de combinaciones con `maskprocessor`

El comando `mp64` del paquete `maskprocessor` es un generador de listas
de palabras, que enumera todas las combinaciones de un espacio de claves
definido por el usuario. Se puede emplear para generar claves candidatas
y para contar las mismas con el parámetro `--combinations`

Por ejemplo, podemos contar el número de palabras diferentes de 8
cararacteres donde los 5 primeros son letras minúsculas y los 3 últimos
son dígitos decimales mediante el siguiente comando:

    $ mp64 --combinations '?l?l?l?l?l?d?d?d'
    11881376000

Observa que en este caso hablamos de decenas de miles de millones de
combinaciones o diez órdenes de magnitud (un uno seguido de diez ceros)

### Uso de hardware gráfico para rotura de contraseñas

`john` permite usar la GPU en el proceso de adivinación de contraseñas,
empleando para ello el estándar OpenCL. Podemos ver una lista de todos
los dispositivos OpenCL disponibles ejecutando el comando:

    ./john --list=opencl-devices

También podemos obtener un listado de todos los algoritmos soportados
por `john` mediante el siguiente comando:

    ./john --list=formats

Estos formatos pueden acabar con el sufijo `-opencl`, que indica que
pueden ser rotos usando el hardware gráfico. También podemos listar
estos formatos usando el siguiente comando:

    ./john --list=formats --format=opencl

### JtR versus hashcat

Existe otro software de rotura de contraseñas también muy utilizado
llamado `hashcat` y orientado sobre todo al uso de hardware específico
como GPUs, FPGAs y ASICs en el proceso de adivinación de contraseñas

El siguiente post de la lista de correos de John the Ripper hace una
espléndida comparativa entre `JtR` y `hashcat`, con los puntos fuertes y
débiles de cada uno de ellos:

<https://www.openwall.com/lists/john-users/2020/05/26/1>

### Miscelánea

El hash de las contraseñas que aparece en `/etc/shadow` utiliza una
variante de codificación especial del tipo Base64

La codificación Base64 usa el siguiente conjunto de caracteres:

    ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/

No obstante, la codificación que hace la función `crypt(3)` emplea este
otro conjunto:

  ./0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz

Al contrario que en Base64, no hay ningún relleno con el carácter `=`, y
el orden en el que se guardan los bytes no es secuencial (?)

### Más info

- <https://www.openwall.com/john/>
- <https://wiki.skullsecurity.org/index.php/Passwords>
- <https://github.com/danielmiessler/SecLists>
- <https://miloserdov.org/?p=4961>
- <https://www.openwall.com/lists/john-users/2020/05/26/1>
- <https://en.wikipedia.org/wiki/Solar_Designer>
- <https://www.debian.org/releases/stable/amd64/release-notes/ch-information.html#pam-default-password>
- <https://www.vidarholen.net/contents/blog/?p=33>
- <https://www.secureauth.com/labs/open-source-tools/impacket/>
- <https://miloserdov.org/?p=4129>
- <https://resources.infosecinstitute.com/topic/ethical-hacking-breaking-windows-passwords/>
- <https://freerainbowtables.com/>
- <https://sourceforge.net/projects/rcracki/files/rcracki_mt/>
- <http://project-rainbowcrack.com/>
