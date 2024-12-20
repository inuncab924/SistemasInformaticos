## Juego WORDLE


Empezamos suponiendo que la palabra es ALERO.
Con el comando:

    for C in {a..z}; do echo $(grep -c $C /srv/SIINF/dict) $C; done | sort -rn | head

...salen las 10 letras mas comunes del diccionario. Como podemos ver, las letras que componen la palabra "alero" se encuentran entre las 6 primeras y ninguna letra se repite. Es una buena estrategia empezar con esta palabra porque es muy probable que al menos una de las cinco letras esté en la palabra que buscamos.

Ponemos ALERO, suponemos que el resultado es: A, R no están; L, E están pero en otro espacio; la O está en el espacio correcto.

Usando el comando:

                    grep '....o' /srv/SIINF/dict | grep -v [ar] | grep l | grep e | sed -n '1p;$='

'grep '....o' /srv/SIINF/dict' -> Busca todas las palabras que tengan cinco letras, siendo la última una 'o'.
'grep -v [ar]' -> La tubería redirecciona el comando a esta salida, que hace que, de todas las palabras obtenidas en el comando anterior, haga una busqueda inversa con las letras 'a' y 'r', es decir, que elimine todas las palabras que contengan esas letras, ya que sabemos que no están.
'grep l | grep e' -> La tubería vuelve a redireccionar la salida a estos comandos, con los que le decimos que la palabra debe contener la 'l' y la 'e'.
| sed -n '1p;$=' -> Nos da un número que se corresponde a la cantidad de palabras que cumplen los requisitos que hemos establecido con los filtros, además de la primera palabra de esta lista. Esta palabra es la que vamos a usar para seguir jugando.


En este caso de ejemplo, la palabra es BELFO. Las letras 'e', 'l' y 'o' están en espacios correctos, pero la 'b' y la 'f' no están en la palabra. De esta forma, podemos reducir la lista aún mas:

                    grep '.el.o' /srv/SIINF/dict | grep -v [arbf] | sed -n '1p;$='

La estrategia se basa en ir mejorando el filtro con cada intento, añadiendo las letras acertadas y eliminando las que no estén. Cuantas mas letras acertadas tengamos, mas reduciremos las posibilidades.
En caso de que nos proponga una palabra que no nos convenga, como por ejemplo CELLO, sería conveniente cambiar sed -n '1p;$=' por 'less' para ver la lista mas extensa. CELLO sería una mala palabra para continuar porque se repite la LL, perdiendo una letra para adivinar.


[····················][····················][····················][····················][····················][····················]


En caso de que tengamos una o varias letras correctas, pero ninguna en el sitio correcto, debemos seguir otra estrategia.
Supongamos que, de nuevo, escribimos la palabra ALERO para empezar: Vemos que la A y la R están en la palabra, pero no en el sitio correcto, ninguna de las otras letras están:
                    grep '[^a]..[^r].' /srv/SIINF/dict | grep r | grep a | grep -v [leo] | sed -n '1p;$='


grep '[^a]..[^r].' -> Le decimos que la palabra tiene 5 letras, pero que en la posición 1 no está la A y en la 4 no está la R.
| grep r | grep a | -> Le decimos que las letras R y A se encuentran en la palabra. De esta forma, va a buscar las palabras que tengan A y R en cualquiera de las otras posiciones.
grep -v [leo] -> De nuevo, le decimos que las letras L, E y O no se encuentran en la palabra.

Nos dice la palabra BAGAR. No es del todo buena, ya que la letra A se repite y perdemos una letra, pero la vamos a poner igual: La A y la R se encuentran en el lugar correcto, la G está en la palabra, pero no en el lugar correcto:

                    grep '.[^a][^g]ar' /srv/SIINF/dict | grep g | grep -v [leob] | sed -n '1p;$='

Le decimos que en esas posiciones no están la A ni la G. Debemos ponerlo asi porque no podemos meter esas letras dentro del -v [], ya que las tenemos que buscar.
La siguiente palabra propuesta es GAFAR: La G está en el lugar correcto, por lo que sabemos que la palabra tiene que ser 'G...AR':

                    grep 'g[^a].ar' /srv/SIINF/dict | grep -v [leobuf] |sed -n '1p;$='

Usando este comando, nos devuelve la palabra GIRAR, que es la correcta.