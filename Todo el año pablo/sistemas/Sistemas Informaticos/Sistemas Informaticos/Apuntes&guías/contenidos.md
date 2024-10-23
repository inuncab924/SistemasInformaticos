# Trabajo de investigación. Fundamentos de los sistemas informáticos

## Índice
1. Arquitectura del modelo Von Neumann
1. Estructura física del sistema informático (hardware)
1. Controladores de dispositivo
1. Componentes y estructura del software del equipo


## Arquitectura del modelo _Von Neumann_

El modelo *Von Neumann* es un tipo de arquitectura de ordenadores que fue creado en 1945 por el matemático, físico e ingeniero hungaro-estadounidense *John Von Neumann*. La gran mayoría de los ordenadores que usamos hoy en día son una versión refinada y mejorada de este mismo modelo, que ha superado el paso del tiempo.  
Una arquitectura *Von Neumann* está compuesta por:

1. Una **unidad de procesamiento (CPU)**, que a su vez contiene una **unidad aritmética lógica** (ALU) y registros del procesador.
1. Una **unidad de control**.
1. Una **memoria**.
1. Una **unidad de almacenamiento**.
1. Mecanismos de **entrada** y **salida**.

## Estructura física (_hardware_) de un sistema informático

El _hardware_ es la parte física de un ordenador, las piezas que lo componen tanto internas como periféricas.  
Los ordenadores modernos que podemos encontrar en cualquier hogar y oficina son una versión modernizada del modelo _Von Neumann_ creado en 1945, y las piezas que los componen son las siguientes:

#### Microprocesador (CPU)
La parte central de un ordenador. Su función es la de ejecutar todos los programas y procesos, desde los necesarios para que el ordenador esté encendido hasta las tareas que el usuario quiere realizar.

#### Memoria principal
Se divide en memoria **RAM** y memoria **ROM**. La memoria RAM es la parte de la memoria del ordenador que se encarga de almacenar los datos de los programas que la CPU está ejecutando en ese mismo momento. Es una memoria muy rápida pero volátil, lo que quiere decir que solo almacena los datos de los procesos de forma temporal y, una vez el ordenador se apague, estos datos se borran de la memoria RAM.  
La memoria ROM es una memoria de solo lectura, por lo que no permite que el usuario modifique la información que se encuentra dentro de esta. Por esta razón, se utiliza para almacenar toda la información necesaria para el arranque y correcto funcionamiento del ordenador.

#### Placa base
Es el esqueleto del ordenador, la parte en la que van conectadas todas las demás piezas que componen el sistema informático y permite que estas estén interconectadas entre sí. Una de las partes más importantes de la placa base es el _socket_ (o zócalo) donde va encajado el procesador (CPU), ya que determina si dicho procesador es compatible con la propia placa base. Si no lo son, el ordenador no podrá realizar ninguna función y se tendría que reemplazar por una CPU compatible. En caso de que el _socket_ se dañe, la placa base quedará completamente inservible y se deberá reemplazar, ya que no permitirá la conexión con ninguna CPU, compatible o no.  
La placa base también cuenta con conectores internos como:

1. **Ranuras DIMM** en las que se insertan las tarjetas de memoria RAM
1. **Puertos PCIe** donde se conectan las llamadas tarjetas de expansión (como por ejemplo una tarjeta gráfica)
1. **Conectores de energía** en los que va conectada la fuente de alimentación.

Los conectores externos de los que dispone la placa base son:

1. **Puertos USB** que permiten la conexión con periféricos como ratones, teclados o memorias flash USB como _pendrives_.
1. **Puerto HDMI** que permite la conexión de la placa base con un monitor donde mostrar la información de video y audio procesada por el sistema. Antiguamente se usaban conectores **VGA**, que hoy en dia están en desuso y solo mostraban vídeo.
1. **Fibra** que conecta el ordenador mediante cable a internet, haciendo que sea una conexión mas rápida y fiable.


#### Almacenamiento secundario
Los dispositivos de almacenamiento secundario son los que permiten almacenar información por separado del procesador. Estos datos guardados no se pierden cuando el ordenador se apaga y se pueden intercambiar entre dispositivos de almacenamiento. Estos dispositivos pueden estar integrados en la placa base o se pueden insertar y extraer libremente mediante puertos USB o lectores de CD, por ejemplo.  
Un tipo de dispositivo de almacenamiento secundario que está integrado en la propia placa base sería una memoria **SSD** o **HDD**, que permiten el almacenamiento de datos directamente dentro del ordenador sin necesidad de conectar ningún dispositivo de forma externa.  
Algunos ejemplos de dispositivos de almacenamiento secundario extraibles son **CDs**, _pendrives_ e incluso **SSD** o **HDD** externos que se pueden conectar al sistema mediante USB.

#### Fuente de alimentación
La fuente de alimentación es la pieza que se encarga de dar corriente eléctrica a la placa base y por ende al resto de piezas que componen el sistema informático.

#### Periféricos
Los periféricos son todos los componentes del sistema informático que no forman parte del equipo principal (CPU y memoria), pero que son necesarios para facilitar las operaciones de entrada y salida y asi realizar una mejor interacción con el sistema. Estos periféricos se dividen en:

1. **Periféricos de entrada**: Son los periféricos utilizados por el usuario para interactuar con el sistema y enviarle información que pueda procesar. Por ejemplo, el teclado que se utiliza para escribir o el ratón que se utiliza para acceder a carpetas o ejecutar procesos, son periféricos de entrada.
1. **Periféricos de salida**: Son los periféricos con los que el sistema envia información al exterior. Esta información se puede mostrar de forma directa al usuario, como un monitor en el que se muestra el texto que el usuario está redactando con un teclado, o puede ser enviada a otro periférico para que este la procese de nuevo y sea legible por el usuario, como por ejemplo, una impresora. El sistema envía la información a la impresora, estos datos se procesan y se imprimen en papel. El usuario puede ver el resultado impreso, pero no puede ver directamente la información que el sistema ha enviado a la impresora para la realización de estos datos.
1. **Periféricos de entrada/salida o de almacenamiento**: Son los periféricos que sirven tanto para guardar información como para enviarla. Encontramos dispositivos de almacenamiento como _pendrives_, **discos duros** o **SSD**, que se utilizan para recoger y guardar la información procesada por el sistema y que asi pueda ser usada mas adelante, por el mismo usuario o por otro sistema informático.
1. **Periféricos de comunicación**: Son los periféricos necesarios para la comunicación entre varios sistemas informáticos, como una tarjeta de red o router. Permiten al sistema conectarse con otros mediante internet y asi poder compartir información.

## Controladores de dispositivo (drivers)
Los controladores de dispositivo, o _drivers_, son el componente que permite al hardware estar conectado con el sistema operativo mediante una serie de líneas de código. Hacen que el sistema operativo reconozca el hardware y se enlace con las distintas piezas que lo componen, permitiendo su uso y correcto funcionamiento.  
Las distintas piezas de hardware tienen sus propios drivers que se deben instalar previamente de ser usadas y se deben mantener actualizados.  
Sin los drivers, el sistema operativo no sabrá que clase de hardware se ha instalado y no se podrá utilizar, y si están desactualizados, no sabrá como interactuar correctamente.

## Componentes y estructura del _software_ de un sistema informático

#### Software de sistema
El software de sistema es el que nos permite interactuar con el sistema informático mediante el hardware. Es principalmente una combinación de los drivers y el sistema operativo, que juntos permiten el correcto funcionamiento del sistema.  
Los tipos de software de sistema que existen son:

1. **Sistema operativo**. Es el software que enlaza los controladores con el hardware. Define que se puede y que no se puede hacer.
1. **Drivers**. Permite al sistema operativo identificar el hardware para que se reconozca y pueda ser usado. Cada pieza tiene su propio controlador que se puede actualizar.
1. **Librerías**. Es lo que permite al sistema operativo interpretar los distintos tipos de archivos y el como debe abrirlos.
1. **Gestor de arranque**. Es lo que nos permite escoger que sistema operativo usar, en caso de que contemos con varios. Si solo disponemos de uno, iniciará este automáticamente.
1. **BIOS**. Es un software de dispositivo que permite configurar ciertas características del mismo, a la vez que determina por donde se va a iniciar el ordenador, por el sistema operativo o por el gestor de arranque.

#### Software de aplicación
Es el software que no tiene que ver con el arranque y funcionamiento del ordenador y que utiliza el usuario para realizar las tareas necesarias usando el sistema informático. Programas como un editor de texto, una hoja de cálculo, un reproductor de vídeo, un programa de dibujo o diseño, son todos ejemplos de este tipo de software. 

#### Proceso de arranque del sistema operativo
El arranque se divide en distintas fases:

1. El procesador ejecuta las instrucciones iniciales.
1. Comprobación de memoria, pantalla, teclado, reloj, etc.
1. Se determina de donde se carga el sistema operativo.
1. El sistema operativo se carga y se inician los drivers, el sistema de archivos y los procesos.
1. Se carga la interfaz gráfica y el proceso de log-in.



## Webgrafía

<https://www.xataka.com/historia-tecnologica/john-von-neumann-genio-que-diseno-arquitectura-nuestros-ordenadores-hizo-hace-75-anos-este-solo-uno-sus-logros>

<https://www.lifeder.com/arquitectura-von-neumann/>

<https://www.geeksforgeeks.org/difference-between-von-neumann-and-harvard-architecture/>

<https://apen.es/glosario-de-informatica/hardware/>

<https://aratecnia.es/que-es-el-hardware/>

<https://www.xataka.com/basics/partes-placa-base-te-explicamos-sus-componentes-forma-sencilla-entiendas-que-tiene>

<https://www.geeknetic.es/Placa-base/que-es-y-para-que-sirve>

<https://www.profesionalreview.com/2018/12/03/conectores-externos-placa-base/>

<http://www.juntadeandalucia.es/educacion/cga/mediawiki/index.php/Perif%C3%A9ricos>

<https://www.areatecnologia.com/informatica/perifericos.html>

<https://tecnomagazine.net/software-de-sistema/>

<https://www.ibm.com/docs/es/aix/7.2?topic=startup-boot-process>