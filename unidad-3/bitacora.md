# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 3 

#### Fricción

<img width="600" height="382" alt="image" src="https://github.com/user-attachments/assets/30bea7c1-783c-44b5-979a-b1ce52fd4500" />

```javascript
        // Obra 1: Fricción - El rastro del cansancio
        let movers = [];
        let mu = 0.1; // Coeficiente de fricción
        
        function setup() {
          createCanvas(600, 400);
          background(255);
          for (let i = 0; i < 10; i++) {
            movers[i] = new Mover(0, random(height), random(1, 5));
            movers[i].vel = createVector(random(5, 15), 0); // Velocidad inicial
          }
        }
        
        function draw() {
          for (let m of movers) {
            // Calcular Fricción
            let friction = m.vel.copy();
            friction.normalize();
            friction.mult(-1);
            let normal1 = 1; 
            friction.setMag(mu * normal1);
            
            m.applyForce(friction);
            m.update();
            m.display();
          }
        }
        
        class Mover {
          constructor(x, y, m) {
            this.pos = createVector(x, y);
            this.vel = createVector(0, 0);
            this.acc = createVector(0, 0);
            this.mass = m;
          }
          applyForce(f) {
            let force = p5.Vector.div(f, this.mass);
            this.acc.add(force);
          }
          update() {
            this.vel.add(this.acc);
            this.pos.add(this.vel);
            this.acc.mult(0);
          }
          display() {
            stroke(0, 50);
            fill(0, 10);
            circle(this.pos.x, this.pos.y, this.mass * 4);
          }
        }
```

#### Resistencia de fluidos

<img width="633" height="398" alt="image" src="https://github.com/user-attachments/assets/22efde19-7cd3-4d8a-8d38-17b2185da418" />

```javascript
        let movers = [];
        let liquids = []; // Ahora es un arreglo para tener 4
        
        function setup() {
          createCanvas(640, 400);
          reset();
        
          // Creamos 4 líquidos con diferentes densidades (coeficientes) y colores
          // Liquid(x, y, ancho, alto, coeficiente de arrastre, color)
          let w = width / 4;
          liquids[0] = new Liquid(0, 0, w, height, 0.05, color(200, 255, 255, 100));     // Aire (muy suave)
          liquids[1] = new Liquid(w, 0, w, height, 0.1, color(100, 200, 255, 150));    // Agua (medio)
          liquids[2] = new Liquid(w * 2, 0, w, height, 0.25, color(50, 100, 255, 180)); // Aceite (fuerte)
          liquids[3] = new Liquid(w * 3, 0, w, height, 0.5, color(20, 50, 150, 200));   // Lodo (muy fuerte)
        }
        
        function draw() {
          background(255);
        
          // Dibujar los 4 líquidos
          for (let l of liquids) {
            l.show();
          }
        
          for (let m of movers) {
            // Revisar cada líquido para ver si la pelota está dentro
            for (let l of liquids) {
              if (l.contains(m)) {
                let dragForce = l.calculateDrag(m);
                m.applyForce(dragForce);
              }
            }
        
            // Gravedad escalada por masa
            let gravity = createVector(0, 0.2 * m.mass);
            m.applyForce(gravity);
        
            m.update();
            m.show();
            m.checkEdges();
          }
        }
        
        function mousePressed() {
          reset();
        }
        
        function reset() {
          movers = [];
          for (let i = 0; i < 9; i++) {
            // Posición inicial aleatoria en la parte superior
            movers[i] = new Mover(random(width), 20, random(1, 4));
          }
        }
        
        // --- CLASE MOVER ---
        class Mover {
          constructor(x, y, mass) {
            this.mass = mass;
            this.radius = mass * 8;
            this.position = createVector(x, y);
            this.velocity = createVector(0, 0);
            this.acceleration = createVector(0, 0);
            // COLOR ALEATORIO para cada esfera
            this.color = color(random(255), random(255), random(255));
          }
        
          applyForce(force) {
            let f = p5.Vector.div(force, this.mass);
            this.acceleration.add(f);
          }
        
          update() {
            this.velocity.add(this.acceleration);
            this.position.add(this.velocity);
            this.acceleration.mult(0);
          }
        
          show() {
            stroke(0);
            strokeWeight(2);
            fill(this.color); // Usar el color asignado
            circle(this.position.x, this.position.y, this.radius * 2);
          }
        
          checkEdges() {
            if (this.position.y > height - this.radius) {
              this.velocity.y *= -0.9;
              this.position.y = height - this.radius;
            }
          }
        }
        
        // --- CLASE LIQUID (Fluido) ---
        class Liquid {
          constructor(x, y, w, h, c, col) {
            this.x = x;
            this.y = y;
            this.w = w;
            this.h = h;
            this.c = c;   // Coeficiente de arrastre
            this.col = col; // Color del fluido
          }
        
          contains(m) {
            let p = m.position;
            return p.x > this.x && p.x < this.x + this.w &&
                   p.y > this.y && p.y < this.y + this.h;
          }
        
          calculateDrag(m) {
            // Magnitud es el coeficiente * velocidad al cuadrado
            let speed = m.velocity.mag();
            let dragMagnitude = this.c * speed * speed;
        
            // Dirección es opuesta a la velocidad
            let dragForce = m.velocity.copy();
            dragForce.mult(-1);
            dragForce.normalize();
            dragForce.mult(dragMagnitude);
            return dragForce;
          }
        
          show() {
            noStroke();
            fill(this.col);
            rect(this.x, this.y, this.w, this.h);
            
            // Etiqueta de texto para identificar la fuerza
            fill(0, 100);
            textAlign(CENTER);
            text("Drag: " + this.c, this.x + this.w/2, height - 10);
          }
        }
```

#### Atraccion gravitacional

<img width="595" height="597" alt="image" src="https://github.com/user-attachments/assets/16843521-5dc2-415c-9b67-0c51abed19cf" />

```javascript
        let movers = [];
        let attractors = [];
        let G = 1; // Constante de gravedad
        
        function setup() {
          createCanvas(600, 600);
          
          // 1. Creamos las 3 esferas grandes (Attractors)
          // Posiciones: Arriba centro, abajo izquierda y abajo derecha
          attractors.push(new Attractor(width / 2, 80, 40, color(255, 204, 0))); // Arriba
          attractors.push(new Attractor(100, height - 80, 40, color(255, 100, 0))); // Izquierda
          attractors.push(new Attractor(width - 100, height - 80, 40, color(255, 100, 0))); // Derecha
        
          // 2. Creamos muchas esferas pequeñas en el centro
          for (let i = 0; i < 40; i++) {
            movers.push(new Mover(width / 2 + random(-20, 20), height / 2 + random(-20, 20), random(1, 3)));
          }
        }
        
        function draw() {
          background(20, 20, 30);
        
          // Dibujar los atractores
          for (let a of attractors) {
            a.show();
          }
        
          for (let m of movers) {
            // A. Atracción hacia las 3 esferas grandes
            for (let a of attractors) {
              let force = a.calculateAttraction(m);
              m.applyForce(force);
            }
        
            // B. Repulsión entre esferas pequeñas (se repelen a sí mismas)
            for (let other of movers) {
              if (m !== other) {
                let repulsion = m.calculateRepulsion(other);
                m.applyForce(repulsion);
              }
            }
        
            m.update();
            m.show();
          }
        }
        
        // --- CLASE ATRACTOR (Esferas Grandes) ---
        class Attractor {
          constructor(x, y, m, col) {
            this.pos = createVector(x, y);
            this.mass = m;
            this.col = col;
          }
        
          calculateAttraction(mover) {
            let force = p5.Vector.sub(this.pos, mover.pos); // Dirección
            let distance = constrain(force.mag(), 5, 50);  // Limitar distancia para evitar fuerzas infinitas
            force.normalize();
            let strength = (G * this.mass * mover.mass) / (distance * distance); // Fórmula de Newton
            force.mult(strength);
            return force;
          }
        
          show() {
            noStroke();
            fill(this.col);
            ellipse(this.pos.x, this.pos.y, this.mass * 2);
          }
        }
        
        // --- CLASE MOVER (Esferas Pequeñas) ---
        class Mover {
          constructor(x, y, m) {
            this.pos = createVector(x, y);
            this.vel = p5.Vector.random2D(); // Inician con un poco de movimiento
            this.acc = createVector(0, 0);
            this.mass = m;
            this.color = color(0, 255, 255, 150);
          }
        
          applyForce(force) {
            let f = p5.Vector.div(force, this.mass);
            this.acc.add(f);
          }
        
          // Fuerza de repulsión: igual a la gravedad pero en dirección opuesta
          calculateRepulsion(other) {
            let force = p5.Vector.sub(this.pos, other.pos); // Dirección hacia afuera
            let distance = force.mag();
            
            if (distance < 40) { // Solo se repelen si están cerca
              force.normalize();
              let strength = (G * this.mass * other.mass) / (distance * distance);
              force.mult(strength * 0.5); // Ajustar intensidad de repulsión
              return force;
            } else {
              return createVector(0, 0);
            }
          }
        
          update() {
            this.vel.add(this.acc);
            this.vel.limit(4); // Evitar que salgan disparadas demasiado rápido
            this.pos.add(this.vel);
            this.acc.mult(0);
          }
        
          show() {
            noStroke();
            fill(this.color);
            ellipse(this.pos.x, this.pos.y, this.mass * 6);
          }
        }
```

## Bitácora de aplicación 

Esta obra gnerativa busca simular un ecosistema donde cada una de las particulas tiene una funcion en su "sociedad", estan las esferas azules que se encargan de recolectar esferas moradas, los cuadrados rojos que son quienes protegen los alrededores y se encargan de eliminar las esferas naranjas que buscan causar caos sobre el orden establecido por las otras esferas. Este ecosistema esta creado a partir de diferentes reglas, ente ellas la fuerza de gravedad que limita el movimeinto de las esferas naranjas quienes caen ante la interacción del usuario y causan un caos lineal. La otra fuerza más notoria es la de la atraccion y repulsion por la que todas las esferas son afectadas, el ejemplo más claro de esto son las esferas moradas quienes son atraidas por las azules y las naranjas y ademas expulsadas por las rojas. Fuera de estas fuerzas se utilizan los principios de movimiento a tavez del cambio en la aceleracion (por parte de las azules que se mueven aleatoriamente) 

https://editor.p5js.org/SaloTB/sketches/T66rBfnpZ 
<img width="796" height="595" alt="image" src="https://github.com/user-attachments/assets/6b06d106-d342-406f-9c9a-5c38dd654183" />

```javascript
    let particlesBlue = [];
    let particlesPurple = [];
    let particlesOrange = [];
    let redSquares = [];
    
    function setup() {
      createCanvas(800, 600);
      
      // Crear partículas azules (aleatorias)
      for (let i = 0; i < 15; i++) {
        particlesBlue.push(new Particle(random(width), random(height), 7, color(0, 150, 255), "blue"));
      }
      
      // Crear partículas moradas (pequeñas)
      for (let i = 0; i < 300; i++) {
        particlesPurple.push(new Particle(random(width), random(height), 3, color(150, 50, 255), "purple"));
      }
    
      // Crear exactamente dos cuadrados rojos
      redSquares.push(new RedSquare(0, 0, 1)); // Uno empieza arriba izquierda
      redSquares.push(new RedSquare(width, height, -1)); // Otro abajo derecha
    }
    
    function mousePressed() {
      // Al cliquear, caen naranjas desde la parte superior
      particlesOrange.push(new Particle(mouseX, 0, 10, color(255, 150, 0), "orange"));
    }
    
    function draw() {
      background(20, 20, 30);
    
    
      // 1. Gravedad para las naranjas (Fuerza constante)
      let gravity = createVector(0, 6);
    
      // 2. Lógica de Interacciones y Actualización
      
      // CUADRADOS ROJOS
      for (let rs of redSquares) {
        rs.update();
        rs.display();
      }
    
      // AZULES: Movimiento aleatorio
      for (let b of particlesBlue) {
        let randomForce = p5.Vector.random2D().mult(0.5);
        b.applyForce(randomForce);
        
        // Repelidas por las naranjas
        for (let o of particlesOrange) {
          let repel = b.calculateAttraction(o, -20, 100); // Fuerza negativa = repulsión
          b.applyForce(repel);
        }
        
        b.update();
        b.edges1();
        b.display();
      }
    
      // NARANJAS: Caen y atraen
      for (let o of particlesOrange) {
        o.applyForce(gravity);
        
        // Empujadas por cuadrados rojos
        for (let rs of redSquares) {
          let push = o.calculateAttraction(rs, -15, 150);
          o.applyForce(push);
        }
        
        o.update();
        o.edges();
        o.display();
      }
    
      // MORADAS: Atraídas por azules y más por naranjas, expulsadas por rojos
    for (let p of particlesPurple) {
      
      // 2. Atracción por Azules (Enjambre)
      for (let b of particlesBlue) {
        let attractBlue = p.calculateAttraction(b, 20, 200);
        let expel = p.calculateAttraction(b, -50, 10); // Buffer de espacio personal
        p.applyForce(attractBlue);
        p.applyForce(expel);
      }
    
      // 3. Atracción fuerte por Naranjas
      for (let o of particlesOrange) {
        let attractOrange = p.calculateAttraction(o, 60, 200);
        let expel = p.calculateAttraction(o, -80, 30);
        p.applyForce(attractOrange);
        p.applyForce(expel);
      }
    
      // 4. Expulsadas por Rojos
      for (let rs of redSquares) {
        let expel = p.calculateAttraction(rs, -150, 100);
        p.applyForce(expel);
      }
    
      // 5. Integración física
      p.update();
      p.edges1();   // Aquí es donde se ejecuta el rebote que definimos arriba
      p.display();
    }
    
    
    }
    // --- CLASE BASE PARA PARTÍCULAS (Basada en Mover de Nature of Code) ---
    class Particle {
      constructor(x, y, m, c, type) {
        this.pos = createVector(x, y);
        this.vel = createVector(0, 0);
        this.acc = createVector(0, 0);
        this.mass = m;
        this.color = c;
        this.type = type;
        this.maxSpeed = (type === "blue") ? random(2, 5) : 4; 
      }
    
      applyForce(force) {
        // Segunda Ley de Newton: A = F / M
        let f = p5.Vector.div(force, this.mass);
        this.acc.add(f);
      }
    
      update() {
        this.vel.add(this.acc);
        this.vel.limit(this.maxSpeed);
        this.pos.add(this.vel);
        this.acc.mult(0); // Limpiar aceleración en cada frame
      }
    
      // Cálculo de fuerza de atracción/repulsión (Capítulo 2.9 - 2.10)
      calculateAttraction(target, strength, range) {
        let force = p5.Vector.sub(target.pos, this.pos);
        let distance = force.mag();
        distance = constrain(distance, 5, range);
        
        if (distance < range) {
          force.normalize();
          let m = (strength * this.mass * (target.mass || 10)) / (distance * distance);
          force.mult(m);
          return force;
        }
        return createVector(0, 0);
      }
    
      edges() {
        if (this.pos.y > height) {
          this.vel.y *= -1;
          this.pos.y = height;
        }
      }
      
     edges1() {
      // Lógica para bordes laterales (Izquierda y Derecha)
      if (this.pos.x > width) {
        this.pos.x = width;  // Reposiciona en el borde
        this.vel.x *= -1;    // Invierte dirección
      } else if (this.pos.x < 0) {
        this.pos.x = 0;
        this.vel.x *= -1;
      }
    
      // Lógica para bordes superiores e inferiores (Arriba y Abajo)
      if (this.pos.y > height) {
        this.pos.y = height;
        this.vel.y *= -1;
      } else if (this.pos.y < 0) {
        this.pos.y = 0;
        this.vel.y *= -1;
      }
    }
    
      display() {
        noStroke();
        fill(this.color);
        ellipse(this.pos.x, this.pos.y, this.mass * 2);
      }
    }
    
    // --- CLASE PARA LOS CUADRADOS ROJOS (Movimiento en bordes) ---
    class RedSquare {
      constructor(x, y, dir) {
        this.pos = createVector(x, y);
        this.dir = dir; // 1 horario, -1 antihorario
        this.side = 0; // 0: top, 1: right, 2: bottom, 3: left
        this.speed = 4;
        this.mass = 20; // Masa grande para influir más
      }
    
      update() {
        // Lógica simple para recorrer el borde del canvas
        if (this.side === 0) { // Arriba
          this.pos.x += this.speed * this.dir;
          if (this.pos.x > width || this.pos.x < 0) this.side = (this.dir > 0) ? 1 : 3;
        } else if (this.side === 1) { // Derecha
          this.pos.y += this.speed * this.dir;
          if (this.pos.y > height || this.pos.y < 0) this.side = (this.dir > 0) ? 2 : 0;
        } else if (this.side === 2) { // Abajo
          this.pos.x -= this.speed * this.dir;
          if (this.pos.x < 0 || this.pos.x > width) this.side = (this.dir > 0) ? 3 : 1;
        } else if (this.side === 3) { // Izquierda
          this.pos.y -= this.speed * this.dir;
          if (this.pos.y < 0 || this.pos.y > height) this.side = (this.dir > 0) ? 0 : 2;
        }
        this.pos.x = constrain(this.pos.x, 0, width);
        this.pos.y = constrain(this.pos.y, 0, height);
      }
    
      display() {
        fill(255, 0, 0);
        rectMode(CENTER);
        rect(this.pos.x, this.pos.y, 30, 30);
      }
    }
```


## Bitácora de reflexión

### Actividad 5

Motion 101: En motion 101 se trabaja el movimeinto, por ejemplo de una particula en base a un vector de movimiento que tiene dirección y magnitud, estos dos factores afectando que tanto se mueve el objeto y en que direccion se mueve. Ademas de esto se trabaja la aceleración que tiene la capacidad de alterar la velocidad y por concecuente la posición de la particula. 


```javascript
        let esferas = [];
        let numEsferas = 4;
        
        function setup() {
          createCanvas(windowWidth, windowHeight);
          
          // Paleta clásica de Calder
          let colores = ["#DD0A1A", "#618AC4", "#F8B100", "#111111"];
        
          for (let i = 0; i < numEsferas; i++) {
            let x = random(width * 0.3, width * 0.7);
            let y = random(height * 0.3, height * 0.7);
            let m = random(30, 60);
            
            // Gravedad única: cada pieza cae a una velocidad distinta
            // Incluso algunas podrían tener gravedad negativa (flotar hacia arriba)
            let gravPersonal = createVector(0, random(5, -6)); 
            
            esferas.push(new Esfera(x, y, m, color(colores[i]), gravPersonal));
          }
        }
        
        function draw() {
          background(200, 200, 200); // Fondo color lienzo
        
          // 1. Conexiones (Atracción/Repulsión interna entre esferas)
          // Conectamos en cadena: 0-1, 1-2, 2-3, 3-0
          for (let i = 0; i < esferas.length; i++) {
            let proxima = (i + 1) % esferas.length;
            vincularEsferas(esferas[i], esferas[proxima]);
          }
        
          for (let e of esferas) {
            // 2. Aplicar Gravedad Propia
            e.applyForce(e.gravedadPropia);
        
            // 3. REPELSIÓN DEL MOUSE (Interacción)
            if (mouseIsPressed) {
              let mousePos = createVector(mouseX, mouseY);
              let fuerzaRepulsion = p5.Vector.sub(e.pos, mousePos); // Dirección: del mouse hacia la esfera
              let distancia = fuerzaRepulsion.mag();
              
              // Solo aplicar si el mouse está relativamente cerca (opcional, para realismo)
              if (distancia < 500) {
                fuerzaRepulsion.normalize();
                // Cuanto más cerca, más fuerte es el empujón (fuerza inversamente proporcional)
                let potencia = 20; 
                fuerzaRepulsion.mult(potencia);
                e.applyForce(fuerzaRepulsion);
              }
            }
        
            // 4. Rozamiento (Fricción) para suavizar el movimiento
            let friccion = e.vel.copy();
            friccion.mult(-0.08);
            e.applyForce(friccion);
        
            e.update();
            e.display();
          }
        
          // Dibujar las líneas de conexión "alambres"
          dibujarAlambres();
        }
        
        // Función para crear la tensión del hilo (Hooke's Law simplificado)
        function vincularEsferas(a, b) {
          let fuerza = p5.Vector.sub(b.pos, a.pos);
          let distanciaActual = fuerza.mag();
          let distanciaDeseada = 250; // Longitud del alambre
          
          let diferencia = distanciaActual - distanciaDeseada;
          
          // Fuerza de resorte
          fuerza.normalize();
          fuerza.mult(diferencia * 0.05); 
          
          a.applyForce(fuerza);
          b.applyForce(fuerza.mult(-1));
        }
        
        function dibujarAlambres() {
          stroke(0);
          strokeWeight(1.5);
          noFill();
          beginShape();
          for (let e of esferas) {
            vertex(e.pos.x, e.pos.y);
          }
          endShape(CLOSE);
        }
        
        class Esfera {
          constructor(x, y, m, col, grav) {
            this.pos = createVector(x, y);
            this.vel = createVector(0, 0);
            this.acc = createVector(0, 0);
            this.masa = m;
            this.color = col;
            this.gravedadPropia = grav;
          }
        
          applyForce(f) {
            // A = F / M
            let fuerza = p5.Vector.div(f, this.masa);
            this.acc.add(fuerza);
          }
        
          update() {
            this.vel.add(this.acc);
            this.pos.add(this.vel);
            this.acc.mult(0);
            
            // Bordes: Rebotar sutilmente
            if (this.pos.x < 0 || this.pos.x > width) this.vel.x *= -0.5;
            if (this.pos.y < 0 || this.pos.y > height) this.vel.y *= -0.5;
          }
        
          display() {
            // Dibujar la masa
            noStroke();
            fill(this.color);
            ellipse(this.pos.x, this.pos.y, this.masa, this.masa);
            
            // Detalle estético central (estilo Calder)
            fill(0);
            ellipse(this.pos.x, this.pos.y, 4, 4);
          }
        }

```

<img width="926" height="798" alt="image" src="https://github.com/user-attachments/assets/699052b1-b51d-4cf1-a3c7-d44bf4fde71a" />
