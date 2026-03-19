# Unidad 5
## Bitácora de proceso de aprendizaje

### Actividad 1
¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?
Su estado vital esta definido por Lifespan que es la carateristica que le permite determinar si esta muerta o no. 
Tiene una fuerza de gravedd que hace que la particula caiga. 
Tiene una posicion y una velocidad que sirven para el 101

¿Qué condición determina que una partícula “muere”? ¿Es una muerte instantánea o gradual?
Es una muerte gradual determinada por que tan vieja es la particula

¿Cómo se actualiza la partícula en cada frame? Identifica el patrón motion 101 dentro de la partícula.
el patron de Motion 101 esta en update, esto le permite a la particula cambiar u posicion ne cada frame, junto con la fuerza de gravedad aplicada a la misma

### Actividad 2 
¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?
La vida de las particulas, el array que se recorre para determinar que particulas eliminar.

¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?
Se tiene muhco mas control sobre las particulas, facilita el trabajo en equipo sobre el codigo y esta mucho más organizado

### Actividad 3 

¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?
Las subclases de poarticula, en este caso confeti, tienen en comun todo lo que heredan, en este caso la diferencia primordial es el metodo de Show

¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.
Para que se pueda utilizar correctamente el polimorfismo que permite posteriromente trabajar con más facilidad en equipo creando diferentes clases de forma separada, pero aplicandolas todas de la misma manera al final

Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?
No tendria que modificar la particula base, en cabio crearia una tercera clase particula que heredaria de esta primera particula padre. Crearia nuevos metodos que se puedan postriormente llamar ocn polimorfismo desde un Run

Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?
En el ejemplo 4.2 no habia como tal un emitter, en cmabio estaba todo puesto en el sketch en el frente. La logica de muerte no cambio como tal, pero ahora es parte de la logica de Run para la muerte de las diferentes particulas, s le puso en el polimorfismo. Permanecio intacta la logica de la particula, se cambio el sketch y se añadieron otras dos clases, una subclase particula y una de emitter.

En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?
Hay un Array de emitters en el 4.4. 


## Bitácora de aplicación 


## Bitácora de reflexión
