# WORDLE con grep

## Resultados de aprendizaje trabajados en la actividad

- :SIINF.3: Gestiona la información del sistema identificando las
  estructuras de almacenamiento y aplicando medidas para asegurar la
  integridad de los datos

## Enunciado

Las reglas del juego de moda WORDLE son las siguiente:

- Hay que adivinar una palabra secreta en seis o menos intentos
- Cada intento debe ser una palabra válida de 5 letras
- Después de cada intento el color de las letras cambia para mostrar qué
  tan cerca estás de acertar la palabra:
  - El color verde indica que la letra está en la palabra y en la
    posición correcta
  - El color amarillo indica que la letra está en la palabra pero en la
    posición incorrecta
  - El color gris indica que la letra no está en la palabra
- Puede haber letras repetidas

Diseña una estrategia sencilla para jugar al WORDLE apoyándote
únicamente en el comando `grep`, explicando en qué consiste tal
estrategia. Deberías usar al menos tres tipos de filtros asociados a los
tres colores de las letras

Puedes jugar usando el comado `wordle.sh`, y las posibles palabras a
acertar están disponibles en el fichero `/srv/SIINF/dict` (se ha
prescindido de las palabras con acentos, diéresis y eñes). El juego
seleccionará aleatoriamente, para que la adivines, una de las 3904
palabras que aparecen en dicho fichero

Graba un asciicast llamado `wordle.cast` en el que juegues tres partidas
del juego, y las ganes tras realizar tres o más intentos

Muestra los comandos `grep` que has usado para filtrar el espacio de
soluciones según la estrategia diseñada, explicando detalladamente cómo
funcionan los filtros que estás empleando

Utiliza `tmux` y divide la pantalla verticalmente en un panel izquierdo
con el juego y otro derecho con los comandos `grep` que utilices
debidamente comentados

Para mostrar cómo se reduce el tamaño del espacio de soluciones debes
calcular en cada momento el número de palabras que cumplen las reglas de
tus filtros para cada suposición, y luego seleccionar la primera de
ellas. Puedes usar para ello la *coletilla* `| sed -n '1p;$='` al final
del filtro. ¿Eres capaz de explicar cómo funciona esta *coletilla*?

Empieza siempre suponiendo que la palabra a adivinar es "ALERO". ¿Por
qué crees que es buena opción empezar por esa palabra? Razónalo
estadísticamente con la ayuda del siguiente comando, documentándote para
explicar cómo funciona:

```bash
for C in {a..z}; do \
  echo $(grep -c $C /srv/SIINF/dict) $C; \
done | sort -rn | head
```
