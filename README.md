# Código BFS en Prolog
> Este repositorio fue creado con la intención de enseñar el recorrido de arboles. Prolog es un lenguaje lógico que trabaja como un árbol. Implementar códigos para realizar la búsqueda BFS es posible con Prolog. De hecho, este lenguaje ayuda a entender el funcionamiento de el árbol como estructura. 

## Table of contents
* [¿Qué es BFS?](#¿qué-es-bfs?)
* [¿Cómo uso Prolog?](#¿cómo-uso-prolog?)
* [¿Qué necesito saber antes de intentar BFS en Prolog?](#¿qué-necesito-saber-antes-de-intentar-bfs-en-prolog?)
* [¿Cómo funciona el código?](#¿cómo-funciona-el-código?)
* ¿Qué he aprendido de esto?

## ¿Qué es BFS?
La busqueda dentro de arboles es una herramienta muy importante. La estructura del árbol sigue una relación de padre con hijos. Por ejemplo, un
padre con tres hijos y cada uno de ellos con dos más. Cada hoja del árbol (hijo o padre) contiene información del tipo que sea. Una busqueda del árbol en BFS
recorré este árbol de maner horizontal. En otras palabras, una búsqueda por nivel. Para lograr esto tiene que guardar los hijos de cada hoja para poder cambiar de nivel. Este tipo de búsqueda es rápida ya que va por nivel. Sin embargo, al guardar tantos hijos se vuelve un poco pesado para la memoria. Cada búsqueda tiene su intratabilidad pero el BFS es una gran herramienta para arboles medianos o pequeños.

! [alt text](https://github.com/santidelgadoma/BFSPrologCode/main/arbolbfs.png)

## ¿Cómo uso Prolog)
Prolog es una herramienta que cuenta con una versión online. La puedes encontrar usando el siguiente [hipervínculo](http://swish.swi-prolog.org).

1. Al abrir el link aparecerá la opción de crear un programa, escojá esa opción.
2. En la barra superior seleccione 'File' para después picar 'Save'
3. Llené los parámetros con la información adecuada y usé el botón 'Save program'

Ya que tienes el nombre de tu programa es necesario importar una base de conocimiento. Dentro del repositorio encontrarás la base de conocimiento que usaremos, un documento llamado BDHijos. Al copiar y pegar la información de BDHijos en tu programa le estas dando a Prolog conocimiento. Le estas indicando quien es hijo de quien a través de generaciones (niveles). Ya que Prolog tiene conocimiento debemos usarlo a nuestro favor para realizar la busqueda.


## ¿Qué necesito saber antes de intentar BFS en Prolog?
Antes de realizar una busqueda dentro de la base de conocimiento necesitas predicados (funciones). Nos apoyamos en los predicados para dirigir la busqueda.
Los predicados sirven para modificar la información y checar características de el recorrido. Los predicados que necesitamos para navegar de manera horizontal a base de conocimiento son:
 
* `cabeza([X|_],X).` Toma la cabeza de la lista que le entregues.
* `descabezar([_|L],L).` Entrega la cola de la lista que le entregues
* `insertar(X,L,[X|L]).` Inserta el objeto que le des en la lista que le entregues
* `concat([],L2,L2).` La claúsula base que junta dos listas en una. En el momento que la lista a juntar se vacié se pasan sus valores a una nueva lista.
* `concat([X1|L1],L2,[X1|L3]):- concat(L1,L2,L3).` La claúsula recursiva que descabeza la lista a juntar. Guarda los valores para agregarlos a la lista final.
* `bagof(Hijos,hijo(Hijos,O),L1).`Hace una lista con todos los hijos de una hoja del árbol.

Las listas son estructuras que guardan y sacan elementos normalmente al descabezar. Descabezar una lista es tomar el primer valor. Los códigos mencionados en la sección anterior se encargan de manipular las listas. Los nombres de estos predicados explican su función; pedir el primer objeto, pedir la lista sin el primer objeto, insertar un objeto al principio de la lista. El predicado concat se encarga de juntar dos listas en una, algo muy ventajoso para la busqueda BFS. El predicado bagof es parte de la inteligencia de Prolog. Su uso es útil ya que en la búsqueda BFS se necesita checar los hijos de una hoja. Por lo tanto, guardarlos en una lista los prepara para ser revisados.
 

## ¿Cómo funciona el código?

`objetivoBfs1(harry).
solucionBfs1(Obj,Sol):- 
    				bfs1([],[],Obj,Sol).`
            
* La primera parte del código se encarga de definir el objetivo, en este caso 'harry'. Para llamar a la busqueda se implementa `SolucionBfs1(Obj,Sol)`. Recibe el valor con el que va a empezar la busqueda `Obj` y `Sol` la variable de retorno. Cuando se encuentré el objetivo `Sol` regresara una lista con el camino recorrido.
Para iniciar la busqueda se llama el predicado `bfs1([],[],Obj,Sol)` que empieza con dos listas vacías y el valor de inicio.

`bfs1(C,A,O,[O|C]):- objetivoBfs1(O).`

* El predicado `bfs1(C,A,O,[O|C])` se encarga de identificar al objetivo checando con su base de conocimeinto `ObjetivoBfs1(O)`. La variable 'O' entrega el objetivo actual, y si no es el objetivo (la ruta) vuelve a intentar. Ya que encuentra otro camino para recorrer intenta solucionarlo con ese predicado de mismo nombre. La variable escrita `[O|C]`se activa si el objetivo ha sido encontrado. Por lo que, al lograr el objetivo la solución es una lista que encabeza el objetivo y su cola es el camino recorrido. 
 
`bfs1(C,A,O,S):-
    			insertar(O,C,C1),
    			(bagof(Hijos,hijo(Hijos,O),L1),!,
    			concat(A,L1,L2),
    			cabeza(L2,X),
    			descabezar(L2,L3),
    			bfs1(C1,L3,X,S); 
				  cabeza(A,Y),
          descabezar(A,L4),
				  bfs1(C1,L4,Y,S)).`
          
* El predicado `bfs1(C,A,O,S)` es el encargado de realizar el recorrido. Cada vez que el objetivo no es encontrado el programa tiene que checar el siguiente. Como es una búsqueda BFS el siguiente en checarse depende de la posición. La primer operación es `insertar(O,C,C1)` que se encarga de agregar al valor que no fue el objetivo en una lista. Guardar esta información es importante porque es el camino que realiza el código. En la siguiente llamada `bagof(Hijos,hijo(Hijos,O),L1)` se agregan todos los hijos de la hoja que acabamos de checar en la lista L1. Cada que una hoja es checada tiene que guardar sus hijos. Esto se debe a que esta lista se usa como el orden de chequeo. La lista que dicta las hojas que se revisan se forma con `concat(A,L1,L2),` esta conformada por 'A', los hijos aún no revisados. Los valores L1 y L2 corresponden a los hijos recien agregados 'L1' y la unión de esas listas 'L2'. La lista L2 va formando la fila ordenada de hojas por revisar.
 
* Si la hoja que no contenía el objetivo tiene una cantidad de hijos nula se realiza un salto. Por un salto me refiero a ignorar parte del código. El indicador `!`después de `bagof()`se encarga de realizar el salto. El salto termina en `;` y el programa realiza las operaciones de `cabeza()`y `decabezar()`. La razón por la cual se necesita el salto es porque el programa ya llegó a la última hoja de ese recorrido. Por lo qué no agrega más hijos a la lista con la fila ordenada (L2). Solo vuelve a llamar a la busqueda con la siguiente hoja de la lista ordenada.
 
* Ya sea que tenga hijos o no la hoja que acabamos de revisar siempre tenemos que revisar la siguiente en orden. Para que no se repita el objeto en la busqueda el predicado `descabezar(L2,L3).`se encarga de borrar el elemento ya revisado. 

* Las variables del predicado `bfs1(C,A,O,S)` estan divididas en dos tipos. 'A' y 'C' son listas auxiliares usadas temporalmente en el código. La variable 'A' se encarga de almacenar la lista ordenada por lo cual solo se usa al buscar. La lista 'C' va guardando todas las hojas que se hayan revisando con el proposito de regresar la lista del camino recorrido. Cuando el objetivo se encuentra se agrega el objetivo al principio de la lista del camino recorrido.






## Code Examples
Show examples of usage:
`put-your-code-here`

## Features
List of features ready and TODOs for future development
* Awesome feature 1
* Awesome feature 2
* Awesome feature 3
