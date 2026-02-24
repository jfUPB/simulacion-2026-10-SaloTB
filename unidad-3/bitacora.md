# Unidad 3

## Bitácora de proceso de aprendizaje
### Actividad 3 

Fricción
<img width="600" height="382" alt="image" src="https://github.com/user-attachments/assets/30bea7c1-783c-44b5-979a-b1ce52fd4500" />


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
Resistencia de fluidos

Atraccion gravitacional

## Bitácora de aplicación 

Esta obra gnerativa busca simular un ecosistema donde cada una de las particulas tiene una funcion en su "sociedad", estan las esferas azules que se encargan de recolectar esferas moradas, los cuadrados rojos que son quienes protegen los alrededores y se encargan de eliminar las esferas naranjas que buscan causar caos sobre el orden establecido por las otras esferas. Este ecosistema esta creado a partir de diferentes reglas, ente ellas la fuerza de gravedad que limita el movimeinto de las esferas naranjas quienes caen ante la interacción del usuario y causan un caos lineal. La otra fuerza más notoria es la de la atraccion y repulsion por la que todas las esferas son afectadas, el ejemplo más claro de esto son las esferas moradas quienes son atraidas por las azules y las naranjas y ademas expulsadas por las rojas. Fuera de estas fuerzas se utilizan ls principios de movimiento a tavez del cambio en la aceleracion (por parte de las azules que se mueven aleatoriamente) 

https://editor.p5js.org/SaloTB/sketches/T66rBfnpZ 
<img width="796" height="595" alt="image" src="https://github.com/user-attachments/assets/6b06d106-d342-406f-9c9a-5c38dd654183" />

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

## Bitácora de reflexión

