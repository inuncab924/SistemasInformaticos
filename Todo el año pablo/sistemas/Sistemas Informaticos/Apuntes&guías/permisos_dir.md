# Permisos sobre directorios

## Enunciado

1. Rellena la siguiente tabla poniendo una `x` en las celdas en las que
   la acción está permitida, según los permisos del directorio

   | permisos dir     | --- | -w- | r-- | rw- | --x | -wx | r-x | rwx |
   |------------------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
   | octal            |  0  |  2  |  4  |  6  |  1  |  3  |  5  |  7  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | crear,           |     |     |     |     |     |     |     |     |
   | renombrar,       |     |     |     |     |     |  x  |     |  x  |
   | borrar ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | listar dir       |     |     |  *  |  *  |     |     |  x  |  x  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | leer             |     |     |     |     |     |     |     |     |
   | el contenido     |     |     |     |     |  x  |  x  |  x  |  x  |
   | de los ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | escribir         |     |     |     |     |     |     |     |     |
   | contenido        |     |     |     |     |  x  |  x  |     |  x  |
   | en los ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | cd al dir        |     |     |     |     |  x  |  x  |  x  |  x  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | cd a un subdir   |     |     |     |     |  x  |  x  |  x  |  x  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | listar un subdir |     |     |     |     |  x  |  x  |  x  |  x  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | acceder          |     |     |     |     |     |     |     |     |
   | a los ficheros   |     |     |     |     |  x  |  x  |  x  |  x  |
   | de un subdir     |     |     |     |     |     |     |     |     |

   Puedes usar los siguientes comandos antes de rellenar las columnas:

    ```bash
    for P in {0..7}; do
      mkdir -p /tmp/dir${P}00/subdir
      touch /tmp/dir${P}00/{f,subdir/g}
      chmod ${P}00 /tmp/dir${P}00
    done
    ```

2. ¿Qué conclusiones interesantes podemos sacar de la tabla una vez
   rellena?

   -Los permisos de ejecución 'x' son los que nos permiten acceder a los directorios, por lo que el usuario que no tenga permisos de ejecución "no podrá interactuar" con ese directorio. Por eso en los directorios con permisos de ejecución(1,3,5 y 7) podemos realizar muchas mas acciones que con los directorios que no tienen esos permisos.
   
   -La función de autocompletar del tabulador no funciona en los subdirectorios de las carpetas en las que, como usuarios, not enemos permisos de lectura. El autocompletar del directorio en sí funciona, pero no nos permite hacerlo para ver el subdirectorio.
   
   * En los directorios 4 y 6 no tenemos permisos de ejecución. Aun asi, nos permite listar los directorios y archivos que contiene el directorio aunque no podamos hacer nada con ellos, gracias a que tenemos permisos de lectura. Tenemos acceso a los archivos y directorios de la carpeta, pero a la vez no; podemos ver el contenido pero no hacer nada.
