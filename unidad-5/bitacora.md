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

En este sistema de particulas se ejemplifica de forma explisita, el nacimiento, la vida y la muerte de las "ideas" o "historias". Estas nacen de personas o en el caso de este sistema en particular de libros abiertos, quienes acumulan estas Ideas posteriormente viajan por el espacio para tocar a otros libros abierts o incluso libros cerrados, cuando se toca a otros libros abiertos estas "ideas" junto con las que este libro ya poseia se convierten en "ideas nuevas" represnetadas por particulas de un color dferente; por otro lado cuando estas ideas llegan con libros cerrados los convierten en libros abiertos que comeinzan a producir ideas. Para este sistema de particulas tambien se tiene una interacción del usuario donde este podra tomar el rol de la institucionalización del arte y las historias; en otras palabras actuando como una censura para las ideas. Esta interacción crea un campo de atraccion, "robando" las ideas a su alrededor para convertirlas en armas en contra de aquellos msimos quienes crearon las ideas. El proposito de este trabajo es que el usuario se de cuenta de lo dificil que es eliminar el poder de las historias y su influencia en su entorno cuando está lo sufcientemente esparcido, pero al msimo tiempo lo peligrosa que puede ser la censura en una sociedad aislada de nuevas ideas. 

Ciclo de vida de las particulas:
1. Libros cerrados: estos mueren cuando una idea llega y nacen cuando una particula "censura" los toca o cuando no "piensan" en una idea durante 5 segundos
2. Libros abiertos: Nacen cuando una idea les llega y mueren cuando una particula "Censura los toca" o cuando no "piensan" en una idea durante 5 segundos
3. Censura: Nacen cuando el usuario recolecta cierta cantidad de "Ideas" y tienen una probabilidad de morir cuando son tocadas por ideas
4. Ideas: Nacen de los libros abiertos y mueren tras cierto tiempo de existir (Se van desvaneciendo lentamente)
![WhatsApp Image 2026-03-25 at 10 01 31 PM](https://github.com/user-attachments/assets/31a948c3-28a1-4b0d-8600-0f02418875a9)


<img width="908" height="775" alt="image" src="https://github.com/user-attachments/assets/b26f5a7e-acf5-4e0c-918b-63bdad5f8f95" />
<img width="915" height="769" alt="image" src="https://github.com/user-attachments/assets/efa69ad0-3fc8-448f-8a48-6b5de600291e" />
<img width="917" height="789" alt="image" src="https://github.com/user-attachments/assets/582ff85b-45d9-470a-af27-63a3130a70c2" />

https://editor.p5js.org/SaloTB/sketches/1gKwpvFqj 

```javascript

    let personas = [];
    let historias = [];
    let opresiones = [];
    const NUM_PERSONAS = 70;
    const RADIO_SUCCION = 180; // Rango de atracción del sello
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      // Inicio: Máximo 2 libros abiertos, resto cerrados
      let cantBlancas = floor(random(1, 3)); 
      for (let i = 0; i < NUM_PERSONAS; i++) {
        let tipo = (i < cantBlancas) ? "CREADOR" : "CONSUMIDOR";
        personas.push(new Persona(random(width), random(height), tipo));
      }
      noCursor();
    }
    
    function draw() {
      background(10, 10, 15);
    
      // 1. EL SELLO DE CENSURA (Mouse)
      if (mouseIsPressed) {
        dibujarAreaSuccion(mouseX, mouseY);
      }
    
      // 2. OPRESIÓN (Cruces Rojas)
      for (let i = opresiones.length - 1; i >= 0; i--) {
        let o = opresiones[i];
        o.update();
        o.display();
        
        let victima = o.buscarVictima(personas);
        if (victima) {
          o.aplicarSeek(victima.pos);
          if (dist(o.pos.x, o.pos.y, victima.pos.x, victima.pos.y) < 15) {
            victima.serOprimido(); // Cierra el libro y borra sus historias
          }
        }
        if (o.isDead()) opresiones.splice(i, 1);
      }
    
      // 3. HISTORIAS (Partículas de Colores)
      for (let i = historias.length - 1; i >= 0; i--) {
        let h = historias[i];
        h.update();
        h.display();
        
        // Atracción al Sello
        if (mouseIsPressed) {
          let d = dist(h.pos.x, h.pos.y, mouseX, mouseY);
          if (d < RADIO_SUCCION) {
            h.atraerAlVortice(mouseX, mouseY);
            // Transformación SOLO al tocar el centro del sello
            if (d < 15) {
              opresiones.push(new Opresion(h.pos.x, h.pos.y));
              historias.splice(i, 1);
              continue;
            }
          }
        }
    
        if (h.estado === "VOLANDO") {
          // Resistencia: Historias vs Cruces
          for (let j = opresiones.length - 1; j >= 0; j--) {
            if (dist(h.pos.x, h.pos.y, opresiones[j].pos.x, opresiones[j].pos.y) < 20) {
              if (random(1) < 0.80) opresiones.splice(j, 1); 
              h.registrarImpacto();
              break;
            }
          }
    
          // Buscar Libros (60% Abiertos / 40% Cerrados)
          let obj = h.buscarObjetivo(personas);
          if (obj) {
            h.aplicarSeek(obj.pos);
            if (dist(h.pos.x, h.pos.y, obj.pos.x, obj.pos.y) < 12) {
              obj.recibirHistoria(h);
              h.registrarImpacto();
            }
          }
        }
        if (h.isDead()) historias.splice(i, 1);
      }
    
      // 4. PERSONAS (Libros)
      for (let p of personas) {
        p.comportamientoMultitud(personas);
        p.update();
        p.display();
      }
    
      dibujarSello(mouseX, mouseY, mouseIsPressed);
    }
    
    // --- VISUALES DEL SELLO ---
    function dibujarAreaSuccion(x, y) {
      noFill();
      strokeWeight(1);
      for (let r = 0; r < RADIO_SUCCION; r += 20) {
        let alpha = map(r, 0, RADIO_SUCCION, 100, 0);
        stroke(255, 0, 0, alpha);
        ellipse(x, y, r * 2 * sin(frameCount * 0.05 + r));
      }
    }
    
    function dibujarSello(x, y, presionado) {
      push();
      translate(x, y);
      if (presionado) scale(0.85);
      
      // Cuerpo del sello
      fill(120, 30, 30);
      stroke(255, 50);
      strokeWeight(2);
      rectMode(CENTER);
      rect(0, -20, 20, 40, 5); // Mango
      
      // Base lacrada
      fill(presionado ? color(255, 0, 0) : color(180, 20, 20));
      ellipse(0, 0, 50, 50);
      ellipse(0, 0, 40, 40);
      
      // Texto
      fill(255);
      noStroke();
      textAlign(CENTER, CENTER);
      textSize(8);
      textStyle(BOLD);
      text("CENSURA", 0, 0);
      pop();
    }
    
    // --- CLASE PERSONA (LIBROS) ---
    class Persona {
      constructor(x, y, tipo) {
        this.pos = createVector(x, y);
        this.vel = p5.Vector.random2D().mult(0.5);
        this.acc = createVector(0, 0);
        this.tipo = tipo;
        this.timerVida = millis();
        this.historiasAcumuladas = [];
        this.miColor = color(random(100, 255), random(100, 255), random(100, 255));
        this.historiasRecibidas = []; // Guardar colores de historias externas
      }
    
      update() {
        this.vel.add(this.acc);
        this.vel.limit(0.7);
        this.pos.add(this.vel);
        this.acc.mult(0);
    
        if (this.pos.x < 0) this.pos.x = width;
        if (this.pos.x > width) this.pos.x = 0;
        if (this.pos.y < 0) this.pos.y = height;
        if (this.pos.y > height) this.pos.y = 0;
    
        if (this.tipo === "CREADOR") {
          if (random(1) < 0.015 && this.historiasAcumuladas.length < 5) {
            this.crearHistoria(this.miColor);
          }
          if (millis() - this.timerVida > 5000) {
            this.tipo = "CONSUMIDOR";
            this.liberarHistorias();
          }
        }
      }
    
      crearHistoria(col) {
        let h = new Historia(this, col);
        this.historiasAcumuladas.push(h);
        historias.push(h);
        this.timerVida = millis();
      }
    
      serOprimido() {
        if (this.tipo === "CREADOR") {
          this.tipo = "CONSUMIDOR";
          // Borrar historias de la existencia
          for (let h of this.historiasAcumuladas) {
            let idx = historias.indexOf(h);
            if (idx > -1) historias.splice(idx, 1);
          }
          this.historiasAcumuladas = [];
          this.historiasRecibidas = [];
        }
      }
    
      recibirHistoria(h) {
        this.timerVida = millis();
        if (this.tipo === "CONSUMIDOR") {
          this.tipo = "CREADOR";
          this.miColor = h.color;
        } else {
          // Lógica de "Nuevas Ideas" (Hibridación)
          if (h.color.toString() !== this.miColor.toString()) {
            this.historiasRecibidas.push(h.color);
            
            // Al recibir 2 historias diferentes, crea una mezcla híbrida
            if (this.historiasRecibidas.length >= 2) {
              let c1 = this.historiasRecibidas[0];
              let c2 = this.historiasRecibidas[1];
              let colorHibrido = lerpColor(c1, c2, 0.5);
              this.crearHistoria(colorHibrido);
              this.historiasRecibidas = []; 
            }
          }
        }
      }
    
      liberarHistorias() {
        for (let h of this.historiasAcumuladas) h.lanzar();
        this.historiasAcumuladas = [];
      }
    
      comportamientoMultitud(otros) {
        let sep = createVector(0, 0);
        for (let otro of otros) {
          let d = dist(this.pos.x, this.pos.y, otro.pos.x, otro.pos.y);
          if (d > 0 && d < 40) {
            let diff = p5.Vector.sub(this.pos, otro.pos);
            sep.add(diff.normalize().div(d));
          }
        }
        this.acc.add(sep.limit(0.1));
      }
    
      display() {
        push();
        translate(this.pos.x, this.pos.y);
        rotate(this.vel.heading() + PI / 2);
        if (this.tipo === "CREADOR") {
          // LIBRO ABIERTO
          fill(255);
          stroke(this.miColor);
          strokeWeight(2);
          rect(-4, 0, 8, 12); 
          rect(4, 0, 8, 12);
        } else {
          // LIBRO CERRADO
          fill(30);
          stroke(100);
          strokeWeight(1);
          rect(0, 0, 8, 12);
        }
        pop();
      }
    }
    
    // --- CLASE HISTORIA ---
    class Historia {
      constructor(padre, col) {
        this.padre = padre;
        this.pos = createVector(padre.pos.x, padre.pos.y);
        this.vel = createVector(0, 0);
        this.acc = createVector(0, 0);
        this.color = col;
        this.lifespan = 255;
        this.estado = "ORBITANDO";
        this.capacidad = 3;
        this.offset = p5.Vector.random2D().mult(random(10, 20));
        this.preferenciaBlanca = random(1) < 0.6; // 60% Blancas, 40% Negras
      }
    
      update() {
        if (this.estado === "ORBITANDO") {
          this.pos.x = this.padre.pos.x + this.offset.x;
          this.pos.y = this.padre.pos.y + this.offset.y;
          this.offset.rotate(0.06);
        } else {
          this.vel.add(this.acc);
          this.pos.add(this.vel);
          this.acc.mult(0);
          this.lifespan -= 0.7;
        }
      }
    
      atraerAlVortice(mx, my) {
        let d = p5.Vector.sub(createVector(mx, my), this.pos);
        let fuerza = map(d.mag(), 0, RADIO_SUCCION, 2, 0.1);
        d.setMag(fuerza);
        this.acc.add(d);
        this.estado = "VOLANDO";
      }
    
      lanzar() {
        this.estado = "VOLANDO";
        this.vel = p5.Vector.random2D().mult(random(2, 4));
      }
    
      aplicarSeek(target) {
        let d = p5.Vector.sub(target, this.pos);
        d.setMag(4.5);
        let s = p5.Vector.sub(d, this.vel);
        s.limit(0.25);
        this.acc.add(s);
      }
    
      buscarObjetivo(lista) {
        let buscado = this.preferenciaBlanca ? "CREADOR" : "CONSUMIDOR";
        let masCerr = null;
        let dMin = 600;
        for (let p of lista) {
          if (p.tipo === buscado && p !== this.padre) {
            let d = dist(this.pos.x, this.pos.y, p.pos.x, p.pos.y);
            if (d < dMin) { dMin = d; masCerr = p; }
          }
        }
        return masCerr;
      }
    
      registrarImpacto() {
        this.capacidad--;
        this.vel.mult(-0.8);
      }
    
      display() {
        noStroke();
        fill(red(this.color), green(this.color), blue(this.color), this.lifespan);
        ellipse(this.pos.x, this.pos.y, 6);
        if (random(1) < 0.1) { // Brillo sutil
          fill(255, this.lifespan);
          ellipse(this.pos.x, this.pos.y, 2);
        }
      }
    
      isDead() { return this.lifespan <= 0 || this.capacidad <= 0; }
    }
    
    // --- CLASE OPRESIÓN (CRUCES) ---
    class Opresion {
      constructor(x, y) {
        this.pos = createVector(x, y);
        this.vel = p5.Vector.random2D();
        this.acc = createVector(0, 0);
        this.lifespan = 400;
      }
    
      update() {
        this.vel.add(this.acc);
        this.pos.add(this.vel);
        this.acc.mult(0);
        this.lifespan -= 0.5;
      }
    
      aplicarSeek(target) {
        let d = p5.Vector.sub(target, this.pos);
        d.setMag(2.8);
        let s = p5.Vector.sub(d, this.vel);
        s.limit(0.2);
        this.acc.add(s);
      }
    
      buscarVictima(lista) {
        let masCerr = null;
        let dMin = 10000;
        for (let p of lista) {
          if (p.tipo === "CREADOR") {
            let d = dist(this.pos.x, this.pos.y, p.pos.x, p.pos.y);
            if (d < dMin) { dMin = d; masCerr = p; }
          }
        }
        return masCerr;
      }
    
      display() {
        push();
        translate(this.pos.x, this.pos.y);
        stroke(255, 0, 0, this.lifespan);
        strokeWeight(3);
        line(-7, -7, 7, 7);
        line(7, -7, -7, 7);
        pop();
      }
    
      isDead() { return this.lifespan <= 0; }
    }
    
    function windowResized() {
      resizeCanvas(windowWidth, windowHeight);
    }

```
## Bitácora de reflexión

### Una partícula es una entidad con estado.
Esto significa que es un elemento que tiene guardado en si mismo varias cualidades como la posicion, la velocidad o la aceleración, el color, el tamaño...etc. No es simplemente un dibujo en la pantalla. 
### Una partícula tiene ciclo de vida.
Las particulas para eviatr la saturación de la meoria tiene un Lifespan, lo que permite que la particula tenga un nacimeinto y una muerte dentro del programa. Esta muerte para cumplir su proposito debe de ser eliminada completamente de la memoria, no uncimaente desaparecer de la pantalla.
### Un sistema de partículas gestiona colecciones dinámicas de elementos.
Es una forma de "pasar lista" de las particulas permitiendo que se dibujen y actualicen.
### La creación y eliminación de partículas no es un detalle técnico menor, sino parte central del modelo.
Estó es mas por el lado de la narrativa y el como cada elmento tiene un ciclo de vida que esta relacionado consigo mismo y ocn el entorno.
### Debe haber separación entre la lógica de una partícula individual y la lógica del sistema/emisor.
Cada particula sabe sus caracteristicas, pero el emisor tiene un concimeinto más general, osea cantidad de particulas, sin necesariamente saber como funcionan, esto aydua a poder hacer cambios sin dañar la logica y a trabajar en equipos de varios programadores. 
### Un emisor o particle system es una abstracción importante.
Ayuda a que no se tengan que mover las particulas individualmente sino que se puedan mover todas desde una base.
### Pueden existir sistemas de sistemas.
esto es gracias a la herencia, donde un sistema de particulas podria lanza otro sistema de particulas, en mi caso una particula "libro abierto" que lanza una particula "idea"
### Puede haber heterogeneidad usando herencia y polimorfismo.
Cone sta logica se pueden tener particulas diferentes que compartan elementos de la misma logica lo que permite mayor variedad sin tener que reescribir el codigo base para todas. 
### Las partículas pueden responder a fuerzas globales y locales.
En esta logica o este "mundo de particulas" como en el mundo real, existen fuerzas propias o fuerzas externas que se apliucan a su entorno para generar las interacciones.
### La representación visual puede variar sin cambiar el principio algorítmico de fondo.
Esto viene desde la base del arte generativo, nosotros solo establecemos las reglas base, pero el sistema, cada vez que se ejecuta, sera de una forma completamente diferente haciendo que las visuales sean unicas cada vez. 
