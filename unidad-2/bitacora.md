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

### Actividad 7
Marco de movimeinto 101, necesita el movimeinto garfico y para eso se necita la aceleración. Encontrar la psoicion dad ala velocidads y la velocidad dada al aceleración. 

el objetivo es predecir donde al siguiente frame se va a pintar el elemento.

Tecnica de integración utilizada: Integración de Euler implicita. Se usa en videojuegos por la simplicidad y la eficiencia (aunque no este acertado a la realidad) 

1. ¿Que es el concepto de motion 101?: Es el cambio en la velocidad atravez de la aceleración que permite determinar (estimar) la siguiente posiición del elemetno.
2. ¿Como se aplica 101 en el ejemplo?: Se puede ver como la volita tiene una velocidad constante, por lo que tiene una misma a celeración todo el tiempo y con esto se puede determinar hacia donde se va a mover que en este caso es siempre en la misma dirección. 


### Actividad 8 
si se deriva la posicion en x con respecto al tiempo, esto da la rapidez. (Magnitud de la velocidad) (porque esta en la dirección del eje x) 

Si la velocidad se deriva con respecto al tiempo esto da la celeridad (magnitud de la aceleración) 

La aceleración es un vector que se le puede variar la magnitud y dirección. 

Aceleración constante: La velocidad aumenta constantemente en la misma dirección 

Aceleración aleatoria: La velocidad y la dirección tambien se vuelven aleatorias. 

Aceleración hacia el mause: 

## Bitácora de aplicación 

### Actividad 9

1. Esta obra es una esfera que funciona como un vector al que el usuario, utilizando las flechas dle teclado, le puede alterar tanto la magnitud como la dirección. Se juega en escenacia ocn la acelración que cmabia la velocidad y la velocidad que cmabia la posicion. Y se evidencia este cambio a partir del tamaño y los colores del objeto, que en este caso funciona como un pincel en constante movimeinto que el usuario puede manipular para pintar el canvas de formas unicas. 

<img width="939" height="807" alt="image" src="https://github.com/user-attachments/assets/84abd092-31a2-4123-9f9f-0b3c3b2d727c" />

```js
    // Obra generativa interactiva
    // El objeto pinta el canvas. Usa las flechas del teclado.
    
    let position;
    let velocity;
    let angle = 0;      // dirección del vector
    let magnitude = 2;  // magnitud del vector
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      background(10);
      colorMode(HSB, 360, 100, 100, 100);
    
      position = createVector(width / 2, height / 2);
      // Inicializamos la velocidad
      velocity = p5.Vector.fromAngle(angle);
      velocity.setMag(magnitude);
    }
    
    function draw() {
      // Actualizamos la velocidad en cada frame por si angle o magnitude cambiaron
      velocity = p5.Vector.fromAngle(angle);
      velocity.setMag(magnitude);
    
      // Movimiento
      position.add(velocity);
    
      // Rebote en bordes
      if (position.x < 0 || position.x > width) {
        angle = PI - angle; // Refleja el ángulo horizontalmente
      }
      if (position.y < 0 || position.y > height) {
        angle = -angle;     // Refleja el ángulo verticalmente
      }
    
      // Color depende de la dirección (normalizamos el ángulo para el tono)
      // Usamos angle % TWO_PI para mantenerlo en el rango de color
      let hue = map(angle % TWO_PI, -PI, PI, 0, 360);
      if (hue < 0) hue += 360; 
    
      stroke(hue, 80, 100, 40);
      fill(2,2,2);
    
      // Tamaño depende de la magnitud
      let brushSize = map(magnitude, 0.5, 10, 4, 60);
    
      // Dibujo del trazo
      ellipse(position.x, position.y, brushSize, brushSize);
    }
    
    function keyPressed() {
      // Cambiar magnitud 
      if (keyCode === UP_ARROW) {
        magnitude += 0.5;
      } else if (keyCode === DOWN_ARROW) {
        magnitude -= 0.5;
      }
      
      // Limitar la velocidad para que no se detenga ni vuele
      magnitude = constrain(magnitude, 0.5, 15);
    
      // Cambiar dirección 
      if (keyCode === LEFT_ARROW) {
        angle -= 0.2;
      } else if (keyCode === RIGHT_ARROW) {
        angle += 0.2;
      }
    
      // Importante: return false al FINAL para evitar scroll del navegador
      return false;
    }   
```

https://editor.p5js.org/SaloTB/sketches/Mc0l0F1o8 

## Bitácora de reflexión

### Actividad 10 

1. Mi obra generativa es una simulación de "particle life" donde cada una de los peuqeños puntos tiene una atracción y una repulsión diferente con otros puntos dependiendo de su color (esto lso incluye a ellos mismos).

<img width="908" height="835" alt="image" src="https://github.com/user-attachments/assets/404dfda8-7ea5-4f5e-8b28-6198afe5e70d" />

```js
    let particles = [];
    const numParticles = 100; // Cantidad por cada color
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      
      // Crear partículas de forma aleatoria
      for (let i = 0; i < numParticles; i++) {
        particles.push(new Particle('purple', color(160, 32, 240)));
        particles.push(new Particle('blue', color(0, 100, 255)));
        particles.push(new Particle('green', color(50, 255, 50)));
      }
    }
    
    function draw() {
      background(10);
    
      for (let p of particles) {
        // Aplicar las reglas de interacción
        p.applyBehaviors(particles);
        // Motion 101
        p.update();
        p.edges();
        p.display();
      }
    }
    
    class Particle {
      constructor(type, col) {
        this.pos = createVector(random(width), random(height));
        this.vel = createVector(0, 0);
        this.acc = createVector(0, 0);
        this.type = type;
        this.color = col;
        this.maxSpeed = 3;
        this.friction = 0.95; // Para que no sea un caos total
      }
    
      // Lógica de Motion 101: La aceleración cambia velocidad, la velocidad cambia posición
      update() {
        this.vel.add(this.acc);
        this.vel.limit(this.maxSpeed);
        this.pos.add(this.vel);
        
        // Limpiamos la aceleración en cada frame 
        this.acc.mult(0);
        // Aplicamos fricción constante
        this.vel.mult(this.friction);
      }
    
      // Aplicar una fuerza (F = M * A, como la masa es 1, la fuerza es la aceleración)
      applyForce(force) {
        this.acc.add(force);
      }
    
      applyBehaviors(others) {
        for (let other of others) {
          if (other === this) continue;
    
          let force = p5.Vector.sub(other.pos, this.pos);
          let d = force.mag();
    
          if (d > 0 && d < 200) { // Radio de influencia
            force.normalize();
            let strength = 0;
    
            // REGLAS
            
            // MORADOS: Atracción a Verdes y Azules. Repelen a sí mismos.
            if (this.type === 'purple') {
              if (other.type === 'green' || other.type === 'blue') strength = 0.5;
              if (other.type === 'purple') strength = -0.5;
            }
    
            // AZULES: Repelidos por Morados. Atraídos por el resto (Azul y Verde).
            if (this.type === 'blue') {
              if (other.type === 'purple') strength = -0.7;
              if (other.type === 'green' || other.type === 'blue') strength = 0.4;
            }
    
            // VERDES: Repelidos por todo. Atraídos por sí mismos.
            if (this.type === 'green') {
              if (other.type === 'purple' || other.type === 'blue') strength = -0.8;
              if (other.type === 'green') strength = 0.6;
            }
    
            // Si están demasiado cerca, siempre se repelen un poco (colisión física básica)
            if (d < 30) strength = -2;
    
            force.mult(strength);
            this.applyForce(force);
          }
        }
      }
    
      edges() {
        // Rebote simple en los bordes
        if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -1;
        if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -1;
        this.pos.x = constrain(this.pos.x, 0, width);
        this.pos.y = constrain(this.pos.y, 0, height);
      }
    
      display() {
        strokeWeight(5);
        stroke(this.color);
        point(this.pos.x, this.pos.y);
      }
    }

```

[Enlace a la simulación](https://editor.p5js.org/SaloTB/sketches/I21pnywBY) 



