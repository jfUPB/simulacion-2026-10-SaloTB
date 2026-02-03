# Unidad 2

## Bitácora de proceso de aprendizaje

### Actividad 2

Suma de vectores: se suman las Componentes en x y componentes en y. 

position = position + velocity; No esta permitido, el operador no esta sobrecargado en la biblioteca. Se puede hacer por x o y, pero no la suma completa de vectores. 

### Actividad 3 
Vectorizar un random walker:

    // The Nature of Code
    // Daniel Shiffman
    // http://natureofcode.com
    
    let walker;
    
    function setup() {
      createCanvas(640, 240);
      walker = new Walker();
      background(255);
    }
    
    function draw() {
      walker.step();
      walker.show();
    }
    
    class Walker {
      constructor() {
        
        this.position = createVector(width/2, height/2);
      }
    
      show() {
        stroke(3);
        point(this.position);
      }
    
      step() {
        const choice = floor(random(4));
        if (choice == 0) {
         this.position.x++;
        } else if (choice == 1) {
           this.position.x--;
        } else if (choice == 2) {
           this.position.y++;
        } else {
          this.position.y--;
        }
      }
    }
    
### Actividad 4:
Aqui se esta modificando el vector por Referencia. Se guarda la dirección del vector para que se pueda modificar la posición. Asi no se modifica dircetamente el vector. 

Si se quiere tener los valores del vector, pero no se quiere modificarlo, se clona o copia. Esto se utiliza para manipular velocidades y aceleraciones. 

¿Qué resultado esperas obtener en el programa anterior?
Ver el vector con los nuevos valores establecidos en la función

¿Qué resultado obtuviste?
El vector antes de ser modificado en la función y el vector con los nuevos valores establecidos.

¿Qué tipo de paso se está realizando en el código?
Paso por referencia. Se esta llamando a la posición donde esta el vector y no al vector como tal. 

¿Qué aprendiste?
Es importante tener cuidado cuando se altera un vector y tener en cuenta que en P5js se utilizan los vectores por referencia. 

### Actividad 5 
Repasar interpretación geometrica. 

1. La magnitud dle vector: es el vector más largo. Se calcula ocn la raiz cuadrada de x al cuadrado más y al cuadrado.
Luego esta la MagSq(): que es x al cuadrado más y al cuadrado. Se utiliza para determinar la distancia entre particulas. se ahorra la riz cuadrada
2. Metodo Normalize(): Para que la magnitud del vector sea 1. Se utiliza para conseguir un sgeundo vector a partir de un primer vector. Lo unico que es relevante en este caso es la dirección.
3. Metodo dot(): (Porducto punto) Se utiliza para saber si son perpendiculares los dos vectores. Permite poryectar un vector sobre otro. 
4. dot estatica():
5. Producto cruz: Para crear un nuevo vector entre dos vectores. Su dirección es particular es que es perpendicular a ambos, se utiliza para calcular sombras y luces. Su magnitud es el area del espacio entre ambos vectores (Dentro del paralelogramo).
6. Metodo dist: para calcular una distancia. 
7. Metodo normalize() y limit():

### Actividad 6 
restar el vecto azul del vector rojo para encontrar el vector verde

## Bitácora de aplicación 



## Bitácora de reflexión
