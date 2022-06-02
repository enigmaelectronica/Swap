# Swap
Vaciado de Swap en equipos y Script que lo automatiza
¿Por qué debemos minimizar el uso de la swap en nuestros Servidores y equipos GNU/Linux?
En equipos PC con prestaciones normales, o incluso de prestaciones reducidas como mi Servidor, seguro que notarán que en el momento que se activa la memoria Swap todo empieza a ir más lento. ¿Por qué en el momento de activarse la swap el servidor se ralentiza?

Simplemente porque las librearías y procesos necesarios para poder ejecutar algunas de las aplicaciones se hallan en una parte del disco duro denominado swap en vez de a la memoria RAM. Cómo todos saben la velocidad de ejecución de la información almacenada en la memoria RAM es infinitamente superior a la que tenemos en nuestro disco duro. Por lo tanto es normal que todo vaya mucho más lento.

Otro motivo de la lentitud que experimentamos es la posible aparición de hiperpaginación. La hiperpaginación causará que haya una serie de librerías que estén intercambiándose continuamente de la memoria RAM a la Swap y de la Swap a la RAM causando lentitud y sobrecarga en el sistema. Los motivos de la hiperpaginación pueden ser que nuestro ordenador tenga muy poca RAM o que los algoritmos de intercambio de procesos de un sitio a otro estén mal implementados.

Pero tenemos que tener en cuenta que no en todos los equipos es interesante minimizar el uso de la Swap. Hay casos particulares que incluso pueden ser necesario no minimizar el uso de la memoria Swap. Estos casos particulares pueden ser los siguientes:

1) Servidores en los que exista una alta carga de trabajo. Hay que tener en cuenta que los servidores acostumbran a lidiar con bases de datos y archivos de gran tamaño.
2) En el caso que dispongamos de un servidor que tenga muy poca memoria RAM disponible. Este caso si estamos hablando de ordenadores relativamente actuales nunca se dará.

3) Servidores o equipos, que aunque dispongan de mucha memoria RAM, tengan que ejecutar aplicaciones que hagan un gran consumo de RAM. Por lo tanto tenemos que ir con cuidado en el caso que tengamos que usar aplicaciones que consuman gran cantidad de RAM.

4) Usuarios que tengan que compilar aplicaciones que sean muy grandes.

Nota: En prácticamente el 99% de los equipos actuales podemos minimizar el uso de la memoria swap. Los 4 casos que acabamos de citar prácticamente no se dan en la mayoría de usuarios. Además hoy en día los ordenadores tienen mucha RAM en comparación con los ordenadores de años atrás.

Cantidad de memoria Swap que es aconsejable asignar
En internet encontrareis multitud de opiniones sobre la memoria swap que se debe asignar para que nuestro sistema operativo funcione adecuadamente. Una teoría muy extendida es la que se presenta a continuación:

En el caso que tengamos 1 GB de RAM o menos deberemos tener un tamaño igual a nuestra RAM. Por lo tanto si tenemos 512 Mb de RAM deberemos tener 512 Mb de Swap
En el caso que tengamos entre 2 y 4 GB de RAM la swap deberá ser la mitad de nuestra memoria RAM. Así por lo tanto si tenemos 2Gb de RAM deberíamos tener 1Gb de Swap
En el caso de equipos más modernos con más de 4Gb de RAM como máximo deberíamos tener 2 Gb de memoria de intercambio.
No obstante según mi punto de vista el criterio que deberíamos seguir es el siguiente:

Primero tenemos que saber la memoria RAM que tiene nuestro Servidor. Por ejemplo pongamos que tenga 1 Gb de RAM.
A continuación tenemos que tener una idea del consumo máximo de RAM que tendremos con nuestros equipo en la peor de las situaciones. Por ejemplo considero que el uso máximo de memoria RAM se dará cuando use simultáneamente el navegador, Libreoffice, el gestor de correo y el reproductor de música. Estimo que con todas estas aplicaciones abiertas mi consumo total Será de 2 GB.

Considerando que tenemos un 1Gb de RAM y en el peor de los casos vamos a llenar 2 Gb implica que tenemos que asignar 1 Gb de Swap más un margen de seguridad de 512Mb. Por lo tanto en el caso presentado 1.5 Gb de Swap es suficiente.

Para finalizar hay que tener claro si tendremos la necesidad de poner nuestro equipo en hibernación. En el momento que hibernamos nuestro equipo lo que estamos haciendo es volcar la totalidad de imágenes de procesos de nuestra memoria RAM a nuestra memoria Swap. Por lo tanto en el caso de querer hibernar el equipo como mínimo necesitaremos nan memoria Swap igual a nuestra RAM. En el caso que hemos planteado, en el caso que quiera hibernar mi equipo, incrementaría la partición de memoria Swap de 1.5 Gb a 2.5Gb.

Minimizar el uso de la memoria Swap
Como hemos visto en los puntos anteriores, en la gran mayoría de casos lo ideal es evitar o retardar el uso de la swap en nuestro sistema operativo y usar la memoria RAM siempre que sea posible. De está forma el funcionamiento de nuestro ordenador será mucho más rápido y fluido. Además los usuarios que dispongan de un disco duro SSD puede que no deseen que se escriba información en su disco duro ya que la vida de un disco duro SSD viene determinado por un número límite de escrituras.

Quien decide el momento en que se empiezan a trasvasar imágenes de procesos de la RAM a la Swap es el Kernel. Como Linux es un sistema operativo abierto podemos hacer que el Kernel de Linux esperé hasta cuando la memoria RAM este casi llena para que empezar el trasvase. Tan solo tenemos que modificar el valor de swappiness de nuestro sistema operativo para indicarle al kernel de linux que de más prioridad al uso de RAM que al uso de Swap.

El valor de swappiness puede comprender un valor entre 0 y 100. Si el valor es 0 se evitará el intercambio de información entre la RAM y la Swap, mientras que si elegimos un valor 100 el intercambio se estará realizando constantemente  La mayoría de distribuciones Linux vienen preconfiguradas con un valor de swappiness de 60. Este valor es probablemente correcto para el uso de servidores y superordenadores pero es claramente excesivo para usuarios domésticos como nosotros.

Por lo tanto en nuestro caso podemos recudir muy drásticamente el valor de la swappiness por defecto hasta valores de 10 o 5. Para ello lo primero que tenemos que hacer es abrir una terminal y teclear el siguiente comando:

cat /proc/sys/vm/swappiness

La salida será el valor actual de swappiness que tenemos asignado. Si no habéis tocado nunca este parámetro es probable que el valor que nos salga en pantalla sea 60, ya que es el valor por defecto que usan la mayoría de distros de Linux. Si es este vuestro caso y tenéis un valor de 60 podemos reducir drásticamente el uso de swap.

Si queremos reducir el valor de Swappiness de 60 a 10 tan solo tenemos que abrir una terminal y escribir el siguiente comando:

sudo sysctl -w vm.swappiness=10
 
En estos momentos ya tenemos fijado el valor de swappiness en 10 y nuestro sistema operativo tenderá a usar mucho menos swap que cuando teníamos fijado el valor en 60.

Una vez tenemos el nuevo valor, lo más aconsejable es usar el ordenador durante un periodo de tiempo razonable para asegurar que el rendimiento que obtenemos con el nuevo parámetro es aceptable y también para comprobar que el sistema sea estable.

Una vez que estamos seguro que nuestro sistema rinde mejor y no genera inestabibilidad en el sistema lo que haremos es hacer este cambio permanente. Así evitaremos que cada vez que arranquemos el ordenador tengamos que cambiar el valor de swappiness de 60 a 10.

Para hacerlo persistente el valor de swappiness 10 abrimos una terminal y tecleamos:

sudo gedit /etc/sysctl.conf

Justo al final del fichero introducimos el siguiente comando:

vm.swappiness=10
 
Guardamos el fichero y la próxima vez que arranquemos nuestro ordenador ya tendremos ajustado el valor de swappiness a 10. Si queremos realizar la comprobación tan solo tenemos que abrir una terminal y teclear el siguiente comando:

cat /proc/sys/vm/swappiness

Liberar la memoria Swap una vez está en uso
Una vez se activa nuestra memoria Swap nuestro sistema se acostumbra a saturar y ralentizar. Una solución para solucionar este problema es reiniciar el equipo pero existen soluciones para liberar la memoria Swap y no tener que apagar el ordenador. 

Para evitar apagar el equipo podemos proceder de la siguiente forma:

free

Lo que realizaremos es traspasar el contenido que tenemos almacenado en nuestra memoria Swap hacia la memoria RAM. Por lo tanto lo primero que realizaremos es asegurarnos que la memoria RAM tiene capacidad libre para poder albergar el contenido que está guardado en la Swap. Para hacer esto abrimos una terminal y tecleamos el comando: 

free

Por lo tanto podemos traspasar el contenido de la memoria Swap a la RAM sin tener ningún tipo de problema.

Para realizar el traspaso abrimos la terminal y escribimos el siguiente comando para transferir el contenido de un lado a otro:

sudo swapoff -a ; sudo swapon -a

Ahora volveremos a teclear el comando:

free

Y efectivamente podemos confirmar que la operación ha tenido éxito, ya que la memoria Swap ahora está completamente libre

El proceso que acabamos ver lo podemos automatizar mediante un script y ejecutarlo periódicamente a las 12 de la noche por ejemplo. 
Para ello tan solo tenemos que añadir la siguiente frase:

swapoff -a && swapon -a

Por lo tanto el contenido del script para limpiar la memoria cache de la RAM y traspasar el contenido de la memoria Swap a la RAM.

