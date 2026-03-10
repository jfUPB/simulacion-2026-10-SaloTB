# Unidad 4

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
https://editor.p5js.org/SaloTB/sketches/yAEhJG3q9
<img width="794" height="591" alt="image" src="https://github.com/user-attachments/assets/62b7f6e5-5eac-4d14-a24f-307111828a9b" />


´´´js
/**
 * Nature of Code: Depredadores con Doble Campo de Fuerza
 * Núcleo: Atracción extrema (Comida) | Aura: Repulsión (Miedo)
 */

let esferas = [];
let particulas = [];
const numParticulas = 350;

function setup() {
  createCanvas(800, 600);

  // Crear 8 esferas (4 arriba, 4 abajo)
  for (let i = 0; i < 4; i++) {
    let x = (width / 5) * (i + 1);
    esferas.push(new Esfera(x, height, random() > 0.5 ? "negra" : "roja", "bottom"));
    esferas.push(new Esfera(x, 0, random() > 0.5 ? "negra" : "roja", "top"));
  }

  // Inicializar partículas
  for (let i = 0; i < numParticulas; i++) {
    let r = random();
    let tipo = r < 0.33 ? "azul" : (r < 0.66 ? "morado" : "naranja");
    particulas.push(new Particula(random(width), random(height), tipo));
  }
}

function draw() {
  background(245, 245, 250);

  // Actualizar depredadores (Resortes y Oscilación)
  for (let e of esferas) {
    e.aplicarResorte();
    if (e.tipo === "roja") e.comportamientoSalto();
    e.update();
    e.display();
  }

  // Actualizar presas (Comunidades y Reacción al peligro)
  for (let p of particulas) {
    p.aplicarInteraccionesSociales(particulas);
    p.reaccionDepredadorDoble(esferas);
    p.limites();
    p.update();
    p.display();
  }
}

// --- INTERACCIÓN ---
function mousePressed() {
  for (let e of esferas) {
    if (e.tipo === "negra") {
      let d = dist(mouseX, mouseY, e.pos.x, e.pos.y);
      if (d < e.radio) e.arrastrando = true;
    }
  }
}

function mouseReleased() {
  for (let e of esferas) e.arrastrando = false;
}

// --- CLASE ESFERA (DEPREDADOR) ---
class Esfera {
  constructor(x, y, tipo, lado) {
    this.anchor = createVector(x, y);
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.tipo = tipo;
    this.lado = lado;
    this.radio = random(25, 40); 
    this.k = (tipo === "roja") ? 0.02 : 0.08; // Rigidez del resorte
    this.arrastrando = false;
  }

  aplicarResorte() {
    if (!this.arrastrando) {
      let f = p5.Vector.sub(this.pos, this.anchor);
      f.mult(-this.k); // Ley de Hooke
      this.acc.add(f);
    }
  }

  comportamientoSalto() {
    if (random(1) < 0.01) {
      let jump = (this.lado === "bottom") ? -random(30, 65) : random(30, 65);
      this.acc.add(createVector(random(-15, 15), jump));
    }
  }

  update() {
    if (this.arrastrando) {
      this.pos.set(mouseX, mouseY);
      this.vel.mult(0);
    } else {
      this.vel.add(this.acc);
      this.vel.mult(0.92); // Amortiguación (Fricción)
      this.pos.add(this.vel);
      this.acc.mult(0);
    }
  }

  display() {
    stroke(0, 40);
    line(this.anchor.x, this.anchor.y, this.pos.x, this.pos.y);
    noStroke();
    
    // Cuerpo del depredador
    fill(this.tipo === "negra" ? 10 : color(220, 20, 50));
    ellipse(this.pos.x, this.pos.y, this.radio * 2);
    
    // Núcleo visual (boca)
    fill(255, 50);
    ellipse(this.pos.x, this.pos.y, this.radio * 0.8);
  }
}

// --- CLASE PARTÍCULA ---
class Particula {
  constructor(x, y, tipo) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector(0, 0);
    this.tipo = tipo;
    this.maxSpeed = 3.8;
    this.maxForce = 0.25;
    this.personalSpace = 18; 
    this.atrapada = false;
  }

  aplicarInteraccionesSociales(otros) {
    if (this.atrapada) return;

    let cohesion = createVector(0, 0);
    let separacion = createVector(0, 0);
    let socialRepulsion = createVector(0, 0);
    let countC = 0;

    for (let o of otros) {
      if (o === this || o.atrapada) continue;
      let d = dist(this.pos.x, this.pos.y, o.pos.x, o.pos.y);

      if (d > 0 && d < 65) {
        // Separación mínima (límite de acercamiento)
        if (d < this.personalSpace) {
          let diff = p5.Vector.sub(this.pos, o.pos);
          diff.normalize().div(d);
          separacion.add(diff);
        }

        // Reglas de tipo
        if (this.tipo === "naranja") {
          cohesion.add(o.pos); // Naranja atraída por todos
          countC++;
        } else if (o.tipo === "naranja") {
          let diff = p5.Vector.sub(this.pos, o.pos); // Todos repelen naranjas
          socialRepulsion.add(diff);
        }

        if (this.tipo === "azul") {
          if (o.tipo === "azul") { cohesion.add(o.pos); countC++; }
          if (o.tipo === "morado") socialRepulsion.add(p5.Vector.sub(this.pos, o.pos));
        } else if (this.tipo === "morado") {
          if (o.tipo === "morado") { cohesion.add(o.pos); countC++; }
          if (o.tipo === "azul") socialRepulsion.add(p5.Vector.sub(this.pos, o.pos));
        }
      }
    }

    if (countC > 0) {
      cohesion.div(countC);
      this.seek(cohesion);
    }
    this.acc.add(separacion.setMag(this.maxForce * 1.8));
    this.acc.add(socialRepulsion.limit(this.maxForce));
  }

  reaccionDepredadorDoble(esferas) {
    this.atrapada = false; // Resetear cada frame

    for (let e of esferas) {
      let d = dist(this.pos.x, this.pos.y, e.pos.x, e.pos.y);
      let coreRange = e.radio + 10;   // Zona de "comida"
      let auraRange = e.radio + 160;  // Zona de "miedo"

      if (d < coreRange) {
        // 1. ATRACCIÓN EXTREMA (Núcleo)
        let fAtraccion = p5.Vector.sub(e.pos, this.pos);
        fAtraccion.setMag(1.5); // Fuerza muy superior al maxForce
        this.acc.add(fAtraccion);
        this.atrapada = true; 
        this.vel.mult(0.5); // Ralentizar porque está siendo "masticada"
      } 
      else if (d < auraRange) {
        // 2. EXPULSIÓN (Aura)
        let fExpulsion = p5.Vector.sub(this.pos, e.pos);
        // La fuerza de escape es inversamente proporcional a la distancia
        let intensidad = map(d, coreRange, auraRange, 1.2, 0.1);
        fExpulsion.setMag(intensidad);
        this.acc.add(fExpulsion);
      }
    }
  }

  seek(target) {
    let desired = p5.Vector.sub(target, this.pos);
    desired.setMag(this.maxSpeed);
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxForce);
    this.acc.add(steer);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.atrapada ? 1.5 : this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  limites() {
    if (this.pos.x < 10 || this.pos.x > width - 10) this.vel.x *= -1;
    if (this.pos.y < 10 || this.pos.y > height - 10) this.vel.y *= -1;
    this.pos.x = constrain(this.pos.x, 10, width - 10);
    this.pos.y = constrain(this.pos.y, 10, height - 10);
  }

  display() {
    noStroke();
    if (this.tipo === "azul") fill(0, 160, 255, 200);
    else if (this.tipo === "morado") fill(160, 60, 255, 200);
    else fill(255, 130, 0, 200);
    
    // Si está atrapada, brilla o cambia ligeramente de tamaño
    let sz = this.atrapada ? 4 : 6;
    ellipse(this.pos.x, this.pos.y, sz);
  }
}

´´´

## Bitácora de reflexión
