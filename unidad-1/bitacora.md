# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 1
La aleatoriedad es la base que permite al arte generativo tener una influencia más directa en la obra, es uno de los factores que permite que la maquina y el artista puedan trabajar
en una pieza unica, que cambia y mejora con la influencia de la aleatoridad y el control más puntual del artista (Los sistemas/limites impuestos a la aleatoriedad). 

### Actividad 2
Modificacion: 

      step() {
        const choice = floor(random(4));
        if (choice == 0) {
          this.x--;
        } else if (choice == 1) {
          this.x--;
        } else if (choice == 2) {
          this.y--;
        } else {
          this.y--;
        }
      }
    }

Hipotesis: El punto se va a mover unicamente en las direcciones negativas del canvas 

Realidad: Si sucedio lo que esperaba, pero ademas siguio una ruta más definida a causa de que la porbabilidad de cambio se duplico en ambos casos. Avanzando siempre en diagonal hacia la parte derecha superior del canvas. 

  #### Notas
  La variable global es una direccion de memoria que se encarga de guardar el objeto. Cuanod se crea el objeto se lleva a la direccion donde se guarda. Se usa This para dteerminar que e sun atributo del objeto.  
  at varios metodos con diferentes responsabilidades. (cada metodo lo hace de forma independiente) (en este caso el dibujo con el movimeinto) 

### Actividad 3
Que sea uniforme significa que no existe una media. Osea un punto donde se concentra el resultado.

Modificaciones:

    step() {
        const choice = floor(random(4));
        if (choice == 0) {
          this.x++;
        } else if (choice == 0) {
          this.x--;
        } else if (choice == 0) {
          this.y++;
        } else {
          this.y--;
        }


  #### Notas
  Se utiliza una forma de randomizar, pero con una forma Normal (aumenta la probabilidad cerca de la media) Es no uniforme.

### Actividad 4 

Hipotesis: Si se cambia crea una variable que cree un color basada en la media y en la desviacion estandar.  

        function setup() {
      createCanvas(640, 240);
      background(255);
    }
    
    function draw() { //{!1} A normal distribution with mean 320 and standard deviation 60
      let x = randomGaussian(320, 60);
      let y = randomGaussian(120, 60);
      let a = randomGaussian(320, 100);
      noStroke();
      fill(255, a, 0)
      circle(x, y, 16); 
    }

<img width="1864" height="1007" alt="image" src="https://github.com/user-attachments/assets/e6131603-2f97-4f3b-a14a-8b2843920e94" />

https://editor.p5js.org/SaloTB/sketches/hwQRwqhnM 

### Actividad 5 
Saltos de Lévy (con un ejemplo anterior) 

https://editor.p5js.org/SaloTB/sketches/DhNL8YMAJ 

<img width="1864" height="1007" alt="image" src="https://github.com/user-attachments/assets/2e9c67cb-467a-41fe-b498-204d4aee51de" />

Utilice la teoria en el texto guia donde se muestra como un pequeño porcentaje de las posibilidades es utilizado como el salto, un movimiento con uan porbabilidad del 1% de ser más alejado a los mobimeintos base de 1 y -1. Al incio tube problemas, pues el movimeinto se sumaba a la posicion actual del circulo. Pero descubri que simplemente debia cambiar el "=" por un "+=" para que la posicion se sumara constantemente a la posicion actual del circulo.

### Actividad 6 

https://editor.p5js.org/SaloTB/sketches/ImP01cnQR 

<img width="1849" height="983" alt="image" src="https://github.com/user-attachments/assets/d4050233-e4dc-4074-bbae-d0e5f01bdd21" />

    let t = 0.0;
    
    
    function setup() {
      createCanvas(360, 240);
    }
    
    function draw() {
     
      background(255, 10);
      let xoff = t;
      noFill();
      stroke(0);
      strokeWeight(2);
      beginShape();
      for (let i = 0; i < width; i++) {
        let y = noise(xoff) * height;
        
        let r = random(1)
        
        if (r < 0.0001){
        xoff += random (-100, 100);
        } else {
          xoff += 0.01;
         
        }
        vertex(i, y);
      }
      endShape();
      t += 0.01;
    }

Esperaba poder crear un salto que cambiara una de las lineas emergentes del ruido perlin, sin emabrgo, al utilizar la base para el salto de levý termine encontrando una forma visualmente más llamativa en la que la que el ruido continua fluyendo como es normal, pero de vez en cuando una linea se sale de lo establecido. Sin emabrgo, incluso tras mis intentos no fui capaz de llevar a cabo mi primera idea. 

## Bitácora de aplicación 

### Actividad 7 

Es una obra generativa que muestra una expansion de puntos de forma normal y uniforme que contrastan la una con la otra atravez de los colores (con un random gausian se logra que el color dentro de cada una de las funciones tambien varie ligeramente), más un elemento cuadrado que contrasta en forma con el resto y esta realizando una caminata con saltos de levý para evitar el aburrimiento visual. La interactividad del mause es arrastarando, y depende de donde esté para que el usuario pueda descubrir que movimeintos causan que cosa y juegue con ello. 

https://editor.p5js.org/SaloTB/sketches/AFPNtjjs1

<img width="638" height="238" alt="image" src="https://github.com/user-attachments/assets/664c2f4b-9e25-453d-8082-4d08255f4c70" />


            let walker;
            
            function setup() {
              createCanvas(640, 240);
              walker = new Walker();
              background(255);
            }
            
            function crazy(){
              let x = randomGaussian(320, 100);
              let y = randomGaussian(120, 100);
              let a = randomGaussian(220, 100);
              noStroke();
              fill(155, a, 200)
              circle(x, y, 16); 
               
            }
            
            function crazy1(){
              let x = randomGaussian(320, 30);
              let y = randomGaussian(100, 30);
              let a = randomGaussian(220, 100);
              noStroke();
              fill(255, a, 200)
              circle(x, y, 16); 
              walker.step();
              walker.show(); 
            }
            
            function draw() { 
              let a = randomGaussian(220, 100);
              fill(255, a, 200)
            }
            
            class Walker {
              constructor() {
                this.x = width / 2;
                this.y = height / 2;
              }
            
              show() {
                stroke(0);
                square(this.x, this.y, 20);
              }
            
              step() {
             let r = random(1);
            if (r < 0.01) {
              this.x += random(-100, 100);
              this.y += random(-100, 100);
            } else {
              this.x += random(-1, 1);
              this.y += random(-1, 1);
            }
             }
              }
            
            function mouseDragged() {
              if (mouseX < 50) { setup();
              } else if (mouseY > 50)  { crazy1()
              }
              else {crazy();}
            
            }

## Bitácora de reflexión

### Actividad 8 

#### Diferencias de Random() y Noise: 

random(): Genera numeros completamente aleatorios sin valores consecutivos. Cada llamada es independiente de las anteriores. (Es más acotico). Se usaria para situaciones más aleatorias, como estrellas que titilan en el cielo.

noise() (Ruido Perlin): Genera secuencias que no son completamente aleatorias, son más orgánicas. Los valores cercanos producen otros valores similares (Sigue un patron a partir de la correlación). Se usa para movimeintos más organicos, como el ejemplo de la onda. 

#### Distribucion de porbabilidad, uniforme y normal:

La distribución de porbabilidad es una ecuación matematica que da a una variable un valor aleatorio. 

Distribucion uniforme: es la que da una amplitud más grande, no hay un punto de concentración.

Distribución normal: es en la que hay un punto de concentración, que es el punto donde es normal que todos aparezcan. (Campana de gaus) 

#### La aleatoriedad en el arte generativo: (Poner al menos dos funciones) 

1. Unicidad: Cada vez que se corre el programa se creara una version diferente de la misma obra, ninguna sera igual a la anterior.
2. Organico: Hace que el proceso se vea más organico a partir de sus variaciones e "imperfecciones"

#### Obra de aleatoriedad actividad 7:

Utilice una distribucion normal y una distribucion uniforme, haciendo que constrastace con gamas de colores diferentes, lo que queria lograr era un constraste y una especie de caos controlado. Ademas de eso añadi un objeto como un walker que de vez en cuando da saltos de levy para evitar lo montono de los conceptos anteriores. Asi el usuario sabe que si interactua en un lugar pasara algo concreto, pero tambien hay cierta sensación de insertidumbre.

#### Caminata y salto de levý: 

La caminata es un proceso de repeticion que tiene en cuenta la posicion anterior para detemrinar la posicion que sigue de forma aleatoria. 

El salto de levý combina pasos pequeños como en al caminata, pero despues da saltos muy largos (los cuales tienen una probabilidad mucho menor de ocurrir)



