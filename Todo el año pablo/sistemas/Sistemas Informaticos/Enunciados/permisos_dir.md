# Permisos sobre directorios

## Enunciado

1. Rellena la siguiente tabla poniendo una `x` en las celdas en las que
   la acción está permitida, según los permisos del directorio

   | permisos dir     | --- | -w- | r-- | rw- | --x | -wx | r-x | rwx |
   |------------------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
   | octal            |  0  |  2  |  4  |  6  |  1  |  3  |  5  |  7  |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | crear,           |     |     |     |     |     |     |     |     |
   | renombrar,       |     |     |     |     |     |     |     |     |
   | borrar ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | listar dir       |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | leer             |     |     |     |     |     |     |     |     |
   | el contenido     |     |     |     |     |     |     |     |     |
   | de los ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | escribir         |     |     |     |     |     |     |     |     |
   | contenido        |     |     |     |     |     |     |     |     |
   | en los ficheros  |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | cd al dir        |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | cd a un subdir   |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | listar un subdir |     |     |     |     |     |     |     |     |
   |------------------|-----|-----|-----|-----|-----|-----|-----|-----|
   | acceder          |     |     |     |     |     |     |     |     |
   | a los ficheros   |     |     |     |     |     |     |     |     |
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
