---
title: Grabaciones en consola
author: Juan Carlos Romero
date: 2022-10-12
...

# Observaciones generales para las grabaciones asciicast

- Hay que explicar lo que se está haciendo en todo momento mediante
  comentarios en la consola (usando el carácter `#`)
- Hay que preocuparse de que no haya solapamiento de caracteres. Para
  ello es conveniente hacer la grabación desde `tmux` y cuando accedamos
  por la consola serie a la máquina virtual, ejecutar antes de la
  grabación el script `fix-term`. También es aconsejable hacer `reset`
  tan pronto veamos que la consola serie se desconfigura
- Hay que vigilar que todas las máquinas que se crean tienen el nombre
  de máquina que se indica en la actividad y el nombre de usuario que
  nos corresponde según nuestros nombres y apellidos y visible en la
  pantalla de login gráfico de nuestra máquina física
- Las grabaciones con continuos fallos al teclear quedan poco vistosas,
  así como aquellas en las que se hacen recorridos continuados por el
  historial de comandos
- Es muy importante visualizar la grabación antes de entregarla, y
  pensar que el profesor la va a evaluar según cómo esté hecha la misma
- Antes de grabar, deberías llevar a cabo la actividad al menos una vez,
  evitando entregar grabaciones en las que se está intentando sin éxito,
  una y otra vez, que algo funcione
- Si se utilizan scripts para lanzar las máquinas virtuales, estaría
  bien hacer un `cat` de los mismos antes de ejecutar el comando
- En ocasiones, es conveniente comprobar que los comandos han realizado
  con éxito la tarea que se supone que tienen que realizar
- No usar nunca una sesión de consola de `root`. Emplear `sudo` en su
  lugar, quedando visible en todo momento el nombre de usuario y el
  nombre de máquina que se están utilizando para hacer la actividad
- Cuidado con las faltas de ortografía. Todas son intolerables, pero
  algunas lo son mucho más que otras
- Si se copian comandos desde el enunciado de la actividad, mejor hacer
  el `copy` desde un editor de textos, no desde un visor de documentos
  como `okular`. Así evitaremos copiar caracteres "extraños"
  inicialmente no visibles
- Si se copia y pega en consola un comando que ocupa múltiples líneas no
  es necesario editarlo para que ocupe sólo una. Funciona igualmente e
  incluso queda más legible
- Antes de grabar un asciicast en el que necesitamos conectarnos por
  `ssh` a alguna máquina virtual, comprobar previamente que tenemos el
  fichero `.ssh/known_hosts` limpio, para no ensuciar la grabación con
  actividades innecesarias relacionadas con la conexión remota
- No copiar y pegar explicaciones de los enunciados de las actividades,
  explicar lo que se está haciendo con palabras propias
- Escribir todos los comentarios de una actividad en mayúsculas es
  símbolo de que se está gritando. Los comentarios deben escribirse en
  minúsculas, excepto cuando la ortografía dicte lo contrario

