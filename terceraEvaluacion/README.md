## Objetivos
Que el programa aprenda nuevos animales, en caso de que no acierte el animal que había pensado el usuario. 

1. Documentación en el reporte de cada una de las funciones que implementes en el programa.


    * Explicación del bloque de código 

_Se define una regla llamada start, que es el punto de entrada del programa. Se le pide al usuario que escriba "start" para comenzar el juego. Si el usuario escribe "start", se llama a la regla main, de lo contrario, se muestra un mensaje indicando que el comando no es reconocido y se vuelve a pedir al usuario que escriba "start"._
```
    start :-
    write('Escribe "start" para comenzar la adivinanza: '),
    read(Input),
    (Input == start ->
        main;
        write('Comando no reconocido. Escribe "start" para comenzar.'), nl).
```
_Se define una regla main, que es donde ocurre la lógica principal del juego. En esta regla, se llama a `hypothesis(Animal)`, que intentará adivinar el animal. Luego, 
definirmos a `preguntarAsertar(Animal)` que recibe como parametro al animal final seleccionado por el programa, el cual va a confirmar si el programa asertó o realiza la dinamica de aprender, seguido de la llamada a undo, que permite al usuario jugar nuevamente o terminar el juego._
```
main :-
    hypothesis(Animal),
    write('El animal es: '),
    write(Animal),
    nl,
    preguntarAsertar(Animal),
    undo.
```
_Esta regla define las posibles hipótesis (animales) que el programa intentará adivinar. Cada hipótesis tiene un conjunto de características asociadas que se verificarán._
```
/* Hipótesis que van a ser puestas a prueba */
hypothesis(leon):- leon, !.
hypothesis(jirafa):- jirafa, !.
hypothesis(tortuga):- tortuga,!.
hypothesis(serpiente):- serpiente,!.
hypothesis(rana):- rana, !.
hypothesis(axolote):- axolote, !.
hypothesis(garza):- garza, !.
hypothesis(pato):- pato,!.
hypothesis(pezpayaso):- pezpayaso, !.
hypothesis(pezbetta):- pezbetta, !.
hypothesis("Animal desconocido").

/* Identificación de cada hipótesis */
leon:-
    verify("es mamífero"),
    verify("es vertebrado"),
    verify("es terrestre"),
    verify("es no volador"),
    verify("tiene pelaje"),
    verify("tiene pulmones"),
    verify("es vivíparo"),
    verify("es endotermico"),
    verify("es carnivoro"),
    verify("es castaño claro"),
    verify("tiene una melena").

jirafa:-
    verify("es mamífero"),
    verify("es vertebrado"),
    verify("es terrestre"),
    verify("es no volador"),
    verify("tiene pelaje"),
    verify("tiene pulmones"),
    verify("es vivíparo"),
    verify("es endotermico"),
    verify("es mamífero"),
    verify("es herbívoro"),
    verify("viven en manadas"),
    verify("tiene manchas oscuras"),
    verify("tiene cuello largo").

tortuga:-
    verify("es reptil"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es no volador"),
    verify("tiene escamas"),
    verify("tiene pulmones"),
    verify("es ovíparo"),
    verify("es ectotermico"),
    verify("es herbívoro"),
    verify("tiene un caparazón").

serpiente:-
    verify("es reptil"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es no volador"),
    verify("tiene escamas"),
    verify("tiene pulmones"),
    verify("es ovíparo"),
    verify("es ectotermico"),
    verify("es flexible"),
    verify("es carnivoro"),
    verify("puede llegar a medir hasta 2m de largo").
    
rana:-
    verify("es anfibio"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es no volador"),
    verify("tiene piel desnuda"),
    verify("tiene pulmones y/o branquias"),
    verify("es ovíparo"),
    verify("es ectotermico"),
    verify("tiene una gran capacidad de salto").
axolote:-
    verify("es anfibio"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es no volador"),
    verify("tiene piel desnuda"),
    verify("tiene pulmones y/o branquias"),
    verify("es ovíparo"),
    verify("es ectotermico"),
    verify("es originario de Xochimilco, México").
    
pato:-
    verify("es un ave"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es volador"),
    verify("tiene plumas"),
    verify("tiene pulmones"),
    verify("es ovíparo"),
    verify("es endotermico"),
    verify("grazna").

garza:-
    verify("es un ave"),
    verify("es vertebrado"),
    verify("es terrestre y/o acuatico"),
    verify("es volador"),
    verify("tiene plumas"),
    verify("tiene pulmones"),
    verify("es ovíparo"),
    verify("es endotermico"),
    verify("tiene pico en forma de lanza y patasmuy largas y delgadas").

pezpayaso:-
    verify("es un pez"),
    verify("es vertebrado"),
    verify("es acuatico"),
    verify("es no volador"),
    verify("tiene escamas"),
    verify("tiene branquias"),
    verify("es ovíparo"),
    verify("es ectotérmico"),
    verify("sale en la película <buscando a Nemo>").

pezbetta:-
    verify("es un pez"),
    verify("es vertebrado"),
    verify("es acuatico"),
    verify("es no volador"),
    verify("tiene escamas"),
    verify("tiene branquias"),
    verify("es ovíparo"),
    verify("es ectotérmico"),
    verify("son dificiles de aparearse").

```
_Esta regla se utiliza para verificar si una característica S es verdadera (yes) o falsa (no). Si la característica ya ha sido afirmada o negada por el usuario, retorna true o false respectivamente. Si no se ha verificado todavía, el programa pregunta al usuario si la característica es verdadera o falsa y la almacena en la base de conocimientos._
```
/* Verificación */
verify(S) :- (yes(S) -> true ;
               (no(S)  -> fail ;
               ask(S))).
``` 
_Esta regla se utiliza para preguntar al usuario si una cierta característica Question es verdadera o falsa. El usuario responde con "yes" o "no" y su respuesta se almacena en la base de conocimientos._
```
   /* Crear una pregunta */
ask(Question) :-
    write('El animal '),
    write(Question),
    write('? '),
    read(Response),
    nl,
    ( (Response == yes ; Response == y ; Response == si) ->
       assert(yes(Question)) ;
       assert(no(Question)), fail).

```
_Este predicado se encarga de preguntar al jugador si desea continuar jugando después de que se haya completado una ronda del juego. Primero, muestra un mensaje preguntando al jugador si desea seguir jugando. Luego, lee la respuesta del jugador y la almacena en Respuesta3. Después, verifica si la respuesta es "yes", "y" o "si" y, si es así, llama al predicado start/0 para comenzar un nuevo juego. Si la respuesta no es una de esas opciones, muestra un mensaje de despedida y termina el programa._
```
seguir_jugando :- 
write('¿Quieres seguir jugando? '), 
read(Respuesta3), 
( (Respuesta3 == yes ; Respuesta3 == y ; Respuesta3 == si) 
-> 
start ; 
nl, 
nl, 
write('Gracias por jugar conmigo, espero te hayas divertido'), 
nl, 
nl, 
write('Bye'), 
nl). 
```
_ Este predicado se encarga de preguntar al jugador si el animal identificado por el programa es el que había pensado. Recibe como entrada el animal identificado por el programa. Primero, muestra un mensaje preguntando al jugador si el animal es el que había pensado. Luego, lee la respuesta del jugador y la almacena en Respuesta. Si la respuesta es "yes", "y" o "si", significa que el programa acertó, así que muestra un mensaje de felicitación y llama a seguir_jugando/0 para continuar jugando. Si la respuesta no es una de esas opciones, significa que el programa no acertó, así que muestra un mensaje indicando que no acertó y llama a aprender_de_error/1 para aprender del error._
```
preguntarAsertar(Animal) :-
    write('¿Es este el animal que estás pensando? (s/n) '),
    read(Respuesta),
    nl,
    (
        (Respuesta == yes ; Respuesta == y ; Respuesta == si) ->
            % El programa acertó
            write('¡Acertaste! '),
            nl,
            nl,
            seguir_jugando;
        % El programa no acertó
        write('¡No he acertado! '),
        nl,
        nl,
        aprender_de_error(Animal)
    ).
```
_Este predicado se encarga de aprender del error cuando el programa no ha acertado el animal que pensaba el jugador. Recibe como entrada el animal correcto que había pensado el jugador. Primero, pregunta al jugador cuál era el animal correcto que había pensado. Luego, lee la respuesta del jugador y la almacena en AnimalCorrecto. Después, pregunta al jugador por una característica que diferencie al animal correcto del animal identificado por el programa. Lee la característica del jugador y la almacena en Caracteristica. Luego, actualiza la base de conocimientos con la nueva hipótesis que diferencia al animal correcto del animal identificado por el programa. Finalmente, muestra un mensaje de agradecimiento y llama a seguir_jugando/0 para continuar. _
```
aprender_de_error(Animal) :-
    write('¿Cuál es el animal que estás pensando? '),
    read(AnimalCorrecto),
    nl,
    write('Dime una característica que diferencie a: '),
    write(AnimalCorrecto),
    write(' de: '),
    write(Animal),
    write(': '),
    read(Caracteristica),
    nl,
    nl,

    % Actualizar la base de conocimientos
    assertz((hypothesis(AnimalCorrecto) :- Animal, verify(Caracteristica))),
    write('Gracias por tu ayuda, ahora sé más sobre animales.'),
    nl,
    nl,
    seguir_jugando.
```
_Definimos las funciones dinámicas_
```
:- dynamic yes/1, no/1.
```
_Esta regla se utiliza para eliminar las cláusulas creadas dinámicamente (características afirmadas o negadas) cuando se reinicia el juego o se termina. Es un mecanismo de limpieza para preparar el programa para una nueva sesión de juego._
```  
/* Eliminar las cláusulas creadas */
undo :- retract(yes(_)),fail.
undo :- retract(no(_)),fail.
undo.
```



