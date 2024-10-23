# Informe John the Ripper

## Grupo: Jorge Coronil Villalba, John Cordoba, José Ramírez Sánchez

## Funcionamiento del programa

Es una herramienta creada con el propósito de crackear (adivinar) contraseñas de usuarios a partir de sus hashes (contraseña encriptada).
Consiste en adivinar las contraseñas mediante fuerza bruta, es decir, probando combinaciones hasta dar con la correcta. Hay varias formas de obtener estas combinaciones.
Podría parecer un programa para usos maliciosos, pero se utiliza como método de seguridad para comprobar la potencia y seguridad de las contraseñas de los usuarios. Cuanto mas tarde el programa en adivinar estas contraseñas, mejores y mas seguras serán.

## John the Ripper versiones

Existen dos versiones del programa, la versión de paquete Linux, que es mas simple y con menos funcionalidades, y la versión jumbo, la versión completa con todas las funcionalidades que ofrece el programa.

### Cifrados y parametros de john the ripper modos de roturas

El método de cifrado usado hoy en día es 'yescrypt' aunque en versiones anteriores se utilizaba 'sha512'. Para realizar la rotura de contraseñas se debe utilizar uno de los tres modos:


    -Single: Realiza combinaciones con la información del usuario (nombre, apellidos, direcciñon, edad, fecha de nacimiento, etc.).
*El modo single es el que se suele usar primero, ya que muchos usuarios, por comodidad, prefieren usar una contraseña que combine nombre, apellidos y fecha de nacimiento, por ejemplo, que es algo fácil de recordar y escribir pero extremadamente inseguro.*


    -Wordlist: Realiza combinaciones con las palabras y caracteres dados por un diccionario.
*Para realizar búsquedas en el modo 'wordlist', hemos creado nuestro propio diccionario con todas las letras que se encuentran en el teclado, incluyendo algunos caracteres que no se encuentran en el modo ASCII. También existen varios diccionarios que se puede descargar.*


    -Incremental: Realiza todas las combinaciones posibles según el tipo de código que se le introduzca. Normalmente, se utiliza el ASCII (que tiene 95
     caracteres en total). También se puede introducir el formato internacional (UTF8) que contiene todos los carácteres de todos los idiomas.
*La mejor forma de crackear las contraseñas de estilo PIN (un código numérico) es con el formato incremental, porque va probando diferentes combinaciones de dígitos.*


### Mascaras

Las máscaras son similares a las expresiones regulares. Consisten en generar contraseñas candidatas mediante el uso de una descripción de como podría ser la contraseña. Se puede definir la longitud de la contraseña, en que lugar poner que caracter o digito, el rango de caracteres en cada posición, si está en mayúscula o minúscula, etc.


    --mask=?u[A-Za-z0-9][A-Za-z0-9][A-Za-z0-9][A-Za-z0-9]?d


En este ejemplo de máscara, se determina que la contraseña tiene seis caracteres de longitud, de los cuales el primero debe ser una letra mayúscula (?u), los caracteres dos, tres, cuatro y cinco pueden ser cualquier caracter de la 'a' a la 'z' (tanto en mayúscula como en minúscula) o un número del 1 al 9, y el último caracter debe ser un dígito. Las máscaras se suelen usar cuando se tiene cierta información sobre el formato de las contraseñas.

Una vez se domina el programa, el uso de máscaras es la forma mas óptima de crackear contraseñas.


### Tipos de contraseñas

Se recomienda siempre que las contraseñas sean cuanto mas complicadas mejor. Mayúsculas, dígitos, caracteres, caracteres especiales... Eso es porque cuantos mas caracters distintos tenga una contraseña, mayor es la cantidad de combinaciones necesarias para adivinarla. Una contraseña simple como '123456', podría necesitar apenas unos pocos cientos de intentos, mientras que otra como 'BhkñJ23N_+Hs5jh9d' podría necesitar unos cuantos varios millones de intentos.


Para adivinar las contraseñas, primero hemos utilizado el modo single para averiguar contraseñas con posibles combinaciones con la informaciñon del usuario. No dimos con ninguna contraseña de esta forma.

    ./john --format=crypt --single --user="usuario" /srv/SIINF/daw1yescrypt

El comando se compone del tipo de formato del hash, el modo con el que se va a realizar el crackeado de la contraseña, el nombre del usuario de dentro del fichero de hashes, y la ruta del fichero en sí.


Luego, utilizamos el modo wordlist para realizar todas las combinaciones posibles con los caracteres que no se encuentra en el modo incremental (como la ñ o la ç).
El uso de estos caracteres es muy recomendado a la hora de crear contraseñas, ya que normalmente se utiliza el modo ASCII. Al tener menos caracteres, la cantidad de combinaciones que tiene que realizar es mucho menor, crackear una contraseña en UTF8 llevaría muchisimo mas tiempo.

Finalmente, utilizamos el modo incremental en UTF8 sin diccionario ni máscara. De esta forma obtuvimos contraseñas formadas por dígitos, aunque tardara su tiempo.

    ./john --format=crypt --incremental=UTF8 --user="usuario" /srv/SIINF/daw1yescrypt

    ./john --format=crypt --incremental=digits --user="usuario" /srv/SIINF/daw1yescrypt


Windows utiliza un formato de encriptado NTLM, que es mucho mas inseguro que el método que utiliza Linux (yescrypt). Utilizando el comando ./john --format=nt /srv/SIINF/daw1ntlm obtuvimos un total de 11 contraseñas en una cantidad ínfima de tiempo, mientras que utilizando los comandos de ./john para adivinar las contraseñas de Linux, apenas pudimos obtener tres contraseñas en un período de varios días, de las cuales todas eran secuencias de números.


Los hashes de contraseña de Windows son tan poco seguros por el tipo de 'sal' que utilizan.
La sal en los hashes de contraseñas consiste en una secuencia de caracteres que se añaden a la contraseña antes de que se realice la operación de hasheado.
La sal es distinta para cada contraseña, de manera que si, por ejemplo, dos usuarios tienen la misma contraseña, el hash de los dos va a ser distinto gracias a la sal. Por ejemplo:


	Usuario1: contraseña123 --sal--> contraseña123(h276Yja29. --hash--> qiyh4XPJGsOZ2MEAyLkfWqeQ
	Usuario2: contraseña123 --sal--> contraseña123>P971,aw!23 --hash--> Os80yXhTurpBMUbA5uyq841K


Aunque ambos usuarios tengan la misma contraseña, al añadir la sal, los hashes son completamente diferentes. Si la sal es inexistente, como ocurre en Windows, con adivinar una contraseña también se sabe la otra. En Windows, los hashes de estas dos contraseñas serían exactamente las mismas, por lo que no tiene esta segunda capa de seguridad.


	Usuario 1: contraseña123 --hash--> $NT$7f8fe03093cc84b267b109625f6bbf4b
	Usuario 2: contraseña123 --hash--> $NT$7f8fe03093cc84b267b109625f6bbf4b


Al no existir la sal, ambos hashes van a ser los mismos. A la hora de adivinar las contraseñas con los hashes de windows en clase, encontró tres de estas de forma casi instantánea. Esto es porque esas contraseñas eran las mismas, '1DAW_2223'. Los tres hashes eran idénticos, asi que con adivinar uno también se adivinan los otros. Si un usuario usa la misma contraseña, o una similar, para todas sus cuentas, con adivinar una en Windows, sería realmente fácil adivinar el resto. Microsoft ha intentado arreglar esto añadiendo los PINs al inicio de sesión, haciendo que se necesite añadir un código numérico simple al iniciar sesión.


## Tablas arcoíris

Las tablas arcoíris se utilizan para crackear las contraseñas. Consiste en hashear X número de contraseñas, guardando el hash generado y la contraseña original en la tabla. A la hora de crackear las contraseñas, en lugar de realizar cálculos para adivinar las contraseñas a partir de los hashes, se comparan los hashes de los usuarios con los que se encuentran en la tabla para ver si coinciden.


Estas tablas arcoíris contienen cantidades ingentes de hashes con sus respectivas contraseñas, las tablas más livianas pesando alrededor de 30GB de texto. Las tablas más complejas y completas pueden llegar a pesar incluso 200GB. Estas tablas requieren de tiempo y un gran espacio de almacenamiento para su uso, pero resultan realmente efectivas para crackear contraseñas comunes y débiles.


Es posible proteger las contraseñas de estas tablas arcoíris realizándoles un hash antes de su almacenamiento para evitar asi que puedan ser usadas por personas sin la información para resolver los hashes. Esto añade una segunda capa de seguridad: Se tendrían que descifrar los hashes de las contraseñas que se encuentran en las tablas, que potencialmente pueden llegar a ser millones, para luego poder usar estos hashes para compararlos con los que se han obtenido de los usuarios anteriormente y crackear asi sus contraseñas.
