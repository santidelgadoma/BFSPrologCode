# Código BFS en Prolog
> Este programa hace un recorrido de arboles de la manera Breadth-First-Search (BFS). Prolog es un lenguaje lógico que trabaja como un árbol. Implementar códigos para realizar la búsqueda BFS es posible con Prolog. De hecho, este lenguaje ayuda a entender el funcionamiento de el árbol como estructura. 

## Tabla de Contenido
* [¿Qué es BFS?](#¿qué-es-bfs?)
* [¿Cómo uso Prolog?](#¿cómo-uso-prolog?)
* [¿Qué necesito saber antes de intentar BFS en Prolog?](#¿qué-necesito-saber-antes-de-intentar-bfs-en-prolog?)
* [¿Cómo funciona el código?](#¿cómo-funciona-el-código?)

## ¿Qué es BFS?
Una búsqueda de árbol en BFS recorré el árbol de manera horizontal, es decir una búsqueda por nivel. Para lograr la búsqueda tiene que guardar los hijos de cada hoja para poder cambiar de nivel. Este tipo de búsqueda es rápida ya que va por nivel. Sin embargo, al guardar tantos hijos se vuelve un poco pesado para la memoria. Cada búsqueda tiene su intratabilidad pero el BFS es una gran herramienta para arboles medianos o pequeños.

![alt text](https://github.com/santidelgadoma/BFSPrologCode/blob/main/ArbolBFS.png)

## ¿Cómo uso Prolog?
[Prolog](http://swish.swi-prolog.org) es una herramienta de programación online.

1. Escoja la opción "crear un nuevo programa".
2. Seleccione en la barra superior 'File' para después picar 'Save'.
3. Llene los parámetros con la información adecuada y pique el botón 'Save program'.

Ya que tienes el nombre de tu programa es necesario importar una base de conocimiento. Dentro del repositorio encontrarás la base de conocimiento que usaremos, un documento llamado BDHijos. Al copiar y pegar la información de BDHijos en tu programa, en el cuadro de texto de lado derecho, le estas dando a Prolog conocimiento. Le estas indicando quien es hijo de quien a través de generaciones (niveles). Ya que Prolog tiene conocimiento debemos usarlo a nuestro favor para realizar la búsqueda.


## ¿Qué necesito saber antes de intentar BFS en Prolog?

Antes de realizar una búsqueda dentro de la base de conocimiento necesitas predicados (funciones). Los predicados sirven para modificar la información y checar características del recorrido. Los predicados que necesitamos para navegar de manera horizontal a base de conocimiento son:
 
* `cabeza([X|_],X).` Toma la cabeza de la lista que le entregues.
* `descabezar([_|L],L).` Entrega la cola de la lista que le entregues
* `insertar(X,L,[X|L]).` Inserta el objeto que le des en la lista que le entregues
* `concat([],L2,L2).` La cláusula base que junta dos listas en una. En el momento que la lista a juntar se vacié se pasan sus valores a una nueva lista.
* `concat([X1|L1],L2,[X1|L3]):- concat(L1,L2,L3).` La cláusula recursiva que descabeza la lista a juntar. Guarda los valores para agregarlos a la lista final.
* `bagof(Hijos,hijo(Hijos,O),L1).`Hace una lista con todos los hijos de una hoja del árbol.

Los códigos mencionados en la sección anterior se encargan de manipular las listas. Los nombres de estos predicados explican su función; pedir el primer objeto, pedir la lista sin el primer objeto, insertar un objeto al principio de la lista. El predicado `concat` se encarga de juntar dos listas en una, algo muy ventajoso para la búsqueda BFS. El predicado `bagof` es parte de la inteligencia de Prolog se utiliza en la búsqueda BFS para checar los hijos de una hoja. Por lo tanto, guardarlos en una lista los prepara para ser revisados.
 

## ¿Cómo funciona el código?

`objetivoBfs1(harry).
solucionBfs1(Obj,Sol):- 
    				bfs1([],[],Obj,Sol).`
            
* La primera parte del código se encarga de definir el objetivo, en este caso 'harry'. Para llamar a la búsqueda se implementa `SolucionBfs1(Obj,Sol)`. Recibe el valor con el que va a empezar la busqueda `Obj` y `Sol` la variable de retorno. Cuando se encuentre el objetivo `Sol` regresara una lista con el camino recorrido.
Para iniciar la búsqueda se llama el predicado `bfs1([],[],Obj,Sol)` que empieza con dos listas vacías y el valor de inicio.

`bfs1(C,A,O,[O|C]):- objetivoBfs1(O).`

* El predicado `bfs1(C,A,O,[O|C])` se encarga de identificar al objetivo checando con su base de conocimiento `ObjetivoBfs1(O)`. La variable 'O' entrega el objetivo actual, y si no es el objetivo (la ruta) vuelve a intentar. Ya que encuentra otro camino para recorrer intenta solucionarlo con ese predicado de mismo nombre. La variable escrita `[O|C]`se activa si el objetivo ha sido encontrado. Por lo que, al lograr el objetivo la solución es una lista que encabeza el objetivo y su cola es el camino recorrido. 
 
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
          
* El predicado `bfs1(C,A,O,S)` es el encargado de realizar el recorrido. Como es una búsqueda BFS el siguiente en checarse depende de la posición. La primer operación es `insertar(O,C,C1)` que se encarga de agregar al valor que no fue el objetivo en una lista. En la siguiente llamada `bagof(Hijos,hijo(Hijos,O),L1)` se agregan todos los hijos de la hoja que acabamos de checar en la lista L1. Cada que una hoja es revisada se tiene que guardar a sus hijos. La lista que dicta las hojas que se revisan se forma con `concat(A,L1,L2)`, esta conformada por 'A', los hijos aún no revisados. Los valores L1 y L2 corresponden a los hijos recién agregados 'L1', y la unión de esas listas 'L2'.
 
* Si la hoja que no contenía el objetivo tiene una cantidad de hijos nula se realiza un salto. El indicador `!` después de `bagof()`se encarga de realizar el salto. El salto termina en `;` y el programa realiza las operaciones de `cabeza()`y `descabezar()`. La razón por la cual se necesita el salto es porque el programa ya llegó a la última hoja de ese recorrido. Solo vuelve a llamar a la búsqueda con la siguiente hoja de la lista ordenada.
 
* Ya sea que tenga hijos o no la hoja que acabamos de revisar siempre tenemos que revisar la siguiente en orden. Para que no se repita el objeto en la búsqueda el predicado `descabezar(L2,L3).`se encarga de borrar el elemento ya revisado. 

* Las variables del predicado `bfs1(C,A,O,S)` están divididas en dos tipos. 'A' y 'C' son listas auxiliares usadas temporalmente en el código. La variable 'A' se encarga de almacenar la lista ordenada por lo cual solo se usa al buscar. La lista 'C' va guardando todas las hojas que se hayan revisando con el propósito de regresar la lista del camino recorrido. Cuando el objetivo se encuentra se agrega el objetivo al principio de la lista del camino recorrido.

* Por ejemplo si quieres intentar la solución `solucionBfs1(jacqueline,Y)`con el objetivo como `objetivoBfs1(monica)'.
- `Y=[monica,obama,jacqueline]` Esta sería la lista con el camino recorrido hasta el objetivo (monica). 


![alt text](https://github.com/santidelgadoma/BFSPrologCode/blob/main/Diagrama.png)



