# Unidad 1

## Bitácora de proceso de aprendizaje
###Actividad 1
La aleatoriedad es la base que permite al arte generativo tener una influencia más directa en la obra, es uno de los factores que permite que la maquina y el artista puedan trabajar
en una pieza unica, que cambia y mejora con la influencia de la aleatoridad y el control más puntual del artista (Los sistemas/limites impuestos a la aleatoriedad). 

### Actividad 2
Hipotesis: 


  #### Notas
  La variable global es una direccion de memoria que se encarga de guardar el objeto. Cuanod se crea el objeto se lleva a la direccion donde se guarda. Se usa This para dteerminar que e sun atributo del objeto.  
  at varios metodos con diferentes responsabilidades. (cada metodo lo hace de forma independiente) (en este caso el dibujo con el movimeinto) 

### Actividad 3


  #### Notas
  Se utiliza una forma de randomizar, pero con una forma Normal (aumenta la probabilidad cerca de la media) Es no uniforme

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

### Actividad 5 
Saltos de Lévy (con un ejemplo anterior) 

## Bitácora de aplicación 



## Bitácora de reflexión


