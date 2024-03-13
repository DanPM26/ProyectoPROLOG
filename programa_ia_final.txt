start :-
    write('Escribe "start" para comenzar la adivinanza: '),
    read(Input),
    (Input == start ->
        main;
        write('Comando no reconocido. Escribe "start" para comenzar.'), nl).

main :-
    hypothesis(Animal),
    write('El animal es: '),
    write(Animal),
    nl,
    undo.

/* Hipótesis que van a ser puestas a prueba */
hypothesis(leon):- leon, !.
hypothesis(tigre):- tigre, !.
hypothesis(jirafa):- jirafa, !.
hypothesis(zebra):- zebra, !.
hypothesis(tortuga):- tortuga,!.
hypothesis(serpiente):- serpiente,!.
hypothesis(pato):- pato,!.
hypothesis(tlacuache):- tlacuache,!.
hypothesis("Animal desconocido").

/* Identificación de cada hipótesis */
leon:-
    verify("es mamífero"),
    verify("es carnivoro"),
    verify("es castaño claro"),
    verify("tiene una melena").

tigre:-
    verify("es mamífero"),
    verify("es carnivoro"),
    verify("es color ambar"),
    verify("tiene rayas negras").

zebra:-
    verify("es mamífero"),
    verify("es herbívoro"),
    verify("viven en manadas"),
    verify("tiene rayas blancas y negras").

jirafa:-
    verify("es mamífero"),
    verify("es herbívoro"),
    verify("viven en manadas"),
    verify("tiene manchas oscuras"),
    verify("tiene cuello largo").

tortuga:-
    verify("es tipo reptil"),
    verify("es herbívoro"),
    verify("es ovíparo"),
    verify("tiene un caparazón"),
    verify("es acuático").

serpiente:-
    verify("es reptil"),
    verify("es flexible"),
    verify("es ovíparo"),
    verify("es carnivoro"),
    verify("se pueden localizar en medios acuaticos o terrestres").

pato:-
    verify("es un ave"),
    verify("es acuático"),
    verify("es ovíparo"),
    verify("se pueden localizar en medios acuaticos o terrestres"),
    verify("grazna").

tlacuache:-
    verify("es un marsupial"),
    verify("es omnívoro"),
    verify("vive en madrigueras"),
    verify("tiene bigotes"),
    verify("es de México"),
    verify("se parece a un ratón").

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

:- dynamic yes/1, no/1.

/* Verificación */
verify(S) :- (yes(S) -> true ;
               (no(S)  -> fail ;
               ask(S))).

/* Eliminar las cláusulas creadas */
undo :- retract(yes(_)),fail.
undo :- retract(no(_)),fail.
undo.