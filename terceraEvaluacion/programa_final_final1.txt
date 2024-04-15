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
    undo,
    preguntarAsertar(Animal).

/* Hipótesis que van a ser puestas a prueba */
hypothesis(leon):- leon.
hypothesis(jirafa):- jirafa.
hypothesis(tortuga):- tortuga.
hypothesis(serpiente):- serpiente.
hypothesis(rana):- rana.
hypothesis(axolote):- axolote.
hypothesis(garza):- garza.
hypothesis(pato):- pato.
hypothesis(pezpayaso):- pezpayaso.
hypothesis(pezbetta):- pezbetta.
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

/* Verificación */
verify(S) :- (yes(S) -> true ;
               (no(S)  -> fail ;
               ask(S))).


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

/*Ofrecemos la oportunidad de volver a jugar otra vez*/ 

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

:- dynamic yes/1, no/1,hypothesis/1.
    
/* Eliminar las cláusulas creadas */
undo :- retract(yes(_)),fail.
undo :- retract(no(_)),fail.

undo.