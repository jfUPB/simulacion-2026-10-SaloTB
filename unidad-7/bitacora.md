# Unidad 7

## Bitácora de proceso de aprendizaje

### Actividad 1 

#### Analiza 3 o 4 ejemplos de Ji Lee y analiza su manipulación tipografica
<img width="399" height="385" alt="image" src="https://github.com/user-attachments/assets/65ce925c-b58b-4e9b-829c-62c7f9290453" />

Este ejemplo me gsuta mucho porque tiene muy en cuenta la linea de vición del lector y la pone en pracica para que incluso si las letras estan esparcidad y puestas una sobre otra se pueda leer de manera intuitiva lo que realmente está dciendo, por supuesto tambien es porprendente como la dirección y posición de las letras puede referenciar un juego entero. (Aqui tambien jugaria con el color  a sabiendas qu ele juego se carcateriza por los bloques de colores y haria cada letra de diferentes colores)

<img width="385" height="375" alt="image" src="https://github.com/user-attachments/assets/b23927aa-d1a3-40ea-916d-8ba76b8e0eda" />

Esté es similar al anterior, pero me gusta más es por la creatividad de poner las pocas letras en una posición exacta que de la sensación de estar viendo una imagen. Utilizar el simplismo de las letras para construir algo que se pueda interpetar, com alguien acostado en una cama y al mismo tiempo se pueda referenciar con lo que es la palabra. (Incluso fuera del video donde la i tose se puede inferir el concepto) (Aqui se me ocurre que incluso se podria jugar con el color y poner el punto de la i en verde para ejemplificar su enfermedad)

<img width="386" height="390" alt="image" src="https://github.com/user-attachments/assets/3a25ced6-b0c3-4a92-9dfd-c825cf747ec0" />

Esté me gusta porque juega con un concepto, y utiliza algo tan simple como la opacidad de las letras para evidenciar como funciona este proceso para todos, lo más cercano, lo ultimo que se escribio, está completamente en engrilla y de alli para atras se va desvaneciendo simulando como funciona la memoria.

#### Propón 2 o 3 palabras propias y esboza cómo podrían representarse visualmente.

Inefable --- Es una palabra contradictoria en si misma, ademas de que esta cargada con implicaciones de fuerte emocionalidad, por lo que imagino que es una palabra cambiante, que represente el vacio del estomago y el aceleramiento del corazon ante algo inexplicable, me la imagino cambiando de color, tamaño y posicion en la pantalla, revoloteando sobre si misma tratando de encontrarse un orden. Abrian letras que se mantendrian en su lugar, mientras otras se mueven de arriba abajo y otras crecen en tamaño, con una "ola" de color que las tintura a todas por partes como esa "ola de emocion".

Historia --- En este caso la historia se estaria contando a si misma, cada una de sus letras con posibilidades infinitas, una "a" que cambia en un planeta, una "H" que se vuelve libro, una "s" que se vuelve serpiente y se escapa de su palabra. Cada letra cambia y se aleja de la palabra para volver, diferente. Pero las historias no son solo las que se viven sino las que se cuentan, asi que me imagino una lluvia de "interferencias" de todo tipo, cuadrados que vienen de aventura, circulos que van de paso, todos pasan y tocan de alguna forma a la historia que se tranforma con cada toque diferente (un cambio de color, un cambio de tamaño) y la historia sigue vivienod sus propias historias pero ahora influenciado por su entorno.

Hambre --- Me imagno al ambre persguiendose a si misma, como Oroburos, comiendo su propia cola y haciendo su boca cada vez más grande entre más tempo pasa, entre más hambre tenga. Se persigue por partes, hasta que se come toda y solo queda la H, vacia, insonora. 

#### Indica cuál de esas palabras te interesa más y por qué.

Me gusta mucho inefable por su significado contradictorio, pero "historia" tiene un mayor significado en mi vida, es practicamente mi proposito, contar historias, asi que me gusta la idea de que la historia se cunete a si misma y demuestre su impacto al mismo timepo.

### Actividad 2 

#### Explica con tus palabras qué hace cada uno de esos conceptos.

Engine: Gestiona la simulación y ayuda a mantener las actualizaciones del mismo funcioanndo. En otras palabras, corre el programa. 
World: El lugar donde habitan los cuerpos. 
Bodies: Son como agentes, tienen su propia velocidad y posición en un mundo.
Constraint: Actua como la coneccion entre diferentes cuerpos, lo relaciono con los constrains utilizandos en el riggin, conecta dos puntos para que puedan "interferir" con el otro de alguna forma
MouseConstraint: Seria como el Maouse puede interferir directamente en el comportamiento de los cuerpos

#### Replica al menos dos experimentos básicos integrando Matter.js con p5.js con enlaces y capturas


#### Describe qué tipo de comportamiento físico te interesa explorar en tu palabra.

Me inetresa utilizar:

1. Composite manipulation: me permite jugar con el tamaño de los objetos al igual que su ubicación, se siente como si la aglomeración de objetos estuviera respirando o incluso como si se estuvieran alejando y acercandose lo que podria ser muy util para materializar la idea de la palabra
2. Collition filtering: Puedo hacer qu eobjetos tengan colición con objetos especificos lo que podria ayudar mucho a llevar la narrativa a niveles mucho más interesantes y complejos
3. Constrainsts: es una formainteresante de mantener a una palabra que se mueve constanetmente en su lugar haciendo ademas que sus movimeintos no sean tan predecibles y reaciconen al otro objeto al que se encuentra atado.

## Bitácora de aplicación 

https://editor.p5js.org/SaloTB/sketches/McHToRAV_  

Palabra elegida: Inefable

Elegi está palabra pues es la representación de todo aquello que es inexplicable y por tanto visualmente se puede representar de todas las maneras posibles de forma que se confunda el sentido de si misma. Una vista inexplicable ademas se transmite usualmente en los sueños que se traduce despues a las pinturas surrealistas de muchos artistas.



<img width="1024" height="768" alt="Purple and Green Minimalist Color Blocks Concept Map Chart (1)" src="https://github.com/user-attachments/assets/53e62383-990b-4f56-a874-d83a42cd0a45" />


<img width="736" height="920" alt="descargar (13)" src="https://github.com/user-attachments/assets/4816ff6f-70bf-475e-a7ea-3db2cd5519bb" />

<img width="600" height="745" alt="Hallway  - Manuel Zapata" src="https://github.com/user-attachments/assets/1fa81986-2d62-4981-b963-822a1c0e7c03" />


```javascript

// --- CONFIGURACIÓN DE MATTER.JS ---
const { Engine, World, Bodies, Constraint, Body } = Matter;
let engine, world;

// --- VARIABLES ---
let agents = [];
let word = "I N E F A B L E";
let font;
let spheres = [];
let pendulums = []; 
let fft, amplitude;
let song;
let started = false;
let currentHue = 0;
let prevCentroid = 0; // Para detectar saltos de pitch

function preload() {
  font = loadFont('https://cdnjs.cloudflare.com/ajax/libs/ink/3.1.10/fonts/Roboto/roboto-bold-webfont.ttf');
  song = loadSound('paganini.mp3'); 
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100); 
  noCursor(); 

  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 0.5;

  initWordPoints();

  fft = new p5.FFT();
  amplitude = new p5.Amplitude();
}

function initWordPoints() {
  agents = [];
  let fontSize = width * 0.12;
  let bounds = font.textBounds(word, 0, 0, fontSize);
  let pts = font.textToPoints(word, width/2 - bounds.w/2, height/2 + bounds.h/2, fontSize, {
    sampleFactor: 0.18
  });

  for (let p of pts) {
    agents.push(new Agent(p.x, p.y));
  }
}

function draw() {
  background(0, 0, 95); 

  if (!started) {
    drawStartScreen();
    return;
  }

  Engine.update(engine);

  // 1. ANÁLISIS DE AUDIO AVANZADO
  fft.analyze();
  let vol = amplitude.getLevel();
  let centroid = fft.getCentroid();
  let treble = fft.getEnergy("treble");

  // Detección de salto de Pitch (Punto 1)
 
  let pitchJump = centroid - prevCentroid;
  if (pitchJump > 600 || (centroid > 5500 && frameCount % 60 == 0)) {
    spawnPendulum();
  }
  prevCentroid = centroid;

  currentHue = lerp(currentHue, map(centroid, 1000, 7000, 0, 360), 0.8);

  // 2. ACTUALIZAR ELEMENTOS FÍSICOS
  drawSpheres();
  updateAndDrawPendulums(vol);

  // 3. ACTUALIZAR AGENTES
  for (let a of agents) {
    a.behaviors(vol, treble, currentHue, pendulums); 
    a.update();
    a.show(vol);
  }

  drawCustomCursor(vol, currentHue);
}

// --- SISTEMA DE PÉNDULO CON RETRACCIÓN (Punto 2) ---
function spawnPendulum() {
  // Solo permitimos 3 péndulos a la vez para no saturar
  if (pendulums.length > 4) return;

  let anchorX = random(width);
  let anchorPos = { x: anchorX, y: -20 };
  
  let ball = Bodies.circle(anchorX + random(-100, 100), 100, 50, {
    restitution: 0.8,
    density: 0.08,
    frictionAir: 0.005
  });

  let targetLen = random(200, height * 0.7);
  let constraint = Constraint.create({
    pointA: anchorPos,
    bodyB: ball,
    length: targetLen,
    stiffness: 0.04
  });

  let pObj = { 
    ball, 
    constraint, 
    timer: 180, // 3 segundos activo
    hue: currentHue, 
    state: "ACTIVE",
    originalLen: targetLen
  };
  
  pendulums.push(pObj);
  World.add(world, [ball, constraint]);
}

function updateAndDrawPendulums(vol) {
  for (let i = pendulums.length - 1; i >= 0; i--) {
    let p = pendulums[i];
    let posB = p.ball.position;
    let posA = p.constraint.pointA;

    // Balanceo rítmico
    let swing = map(vol, 0, 0.5, 0, 1.2);
    Body.applyForce(p.ball, p.ball.position, { x: swing * cos(frameCount * 0.15), y: 0 });

    // Lógica de Retracción (Punto 2)
    if (p.timer > 0) {
      p.timer--;
    } else {
      p.state = "RETRACTING";
      // El hilo se acorta agresivamente
      p.constraint.length = lerp(p.constraint.length, 0, 0.08);
      // El anclaje sube para sacarlo de escena
      p.constraint.pointA.y -= 5;
    }

    // Dibujo
    stroke(p.hue, 30, 60, 40);
    strokeWeight(2);
    line(posA.x, posA.y, posB.x, posB.y);
    
    noStroke();
    fill(p.hue, 20, 80, 60);
    ellipse(posB.x, posB.y, 80);

    // Eliminar cuando se ha retraído totalmente
    if (p.state === "RETRACTING" && p.constraint.length < 5) {
      World.remove(world, [p.ball, p.constraint]);
      pendulums.splice(i, 1);
    }
  }
}

// --- CLASE AGENTE CON VIBRACIÓN INDIVIDUAL ---
class Agent {
  constructor(tx, ty) {
    this.target = createVector(tx, ty);
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(random(-3, 3), random(-3, 3));
    this.acc = createVector();
    this.maxSpeedBase = 5;
    this.maxForce = 0.45;
    this.size = random(4, 10);
    this.baseSize = this.size;
    this.shapeType = floor(random(3));
    this.lockTimer = 0;
    this.jitterFactor = random(0.8, 6.0); // Niveles de "locura"
  }

  behaviors(vol, energy, h, pends) {
    this.maxSpeed = map(vol, 0, 0.5, this.maxSpeedBase, this.maxSpeedBase * 9);
    this.h = h;

    // Revelado por Mouse
    if (dist(mouseX, mouseY, this.pos.x, this.pos.y) < 110) {
      this.lockTimer = 60 * 7; 
    }

    if (this.lockTimer > 0) {
      this.applyForce(this.arrive(this.target));
      this.size = lerp(this.size, this.baseSize * 2.2, 0.2);
      this.lockTimer--;
    } else {
      this.applyForce(this.wander());
      let water = createVector(sin(frameCount * 0.05 + this.pos.y * 0.02), 0);
      water.mult(map(energy, 0, 255, 1, 5));
      this.applyForce(water);
      this.size = lerp(this.size, this.baseSize, 0.1);
    }

    // REPELENTE DE PÉNDULO (Potente)
    for (let p of pends) {
      let d = dist(this.pos.x, this.pos.y, p.ball.position.x, p.ball.position.y);
      if (d < 300) {
        let repel = p5.Vector.sub(this.pos, createVector(p.ball.position.x, p.ball.position.y));
        let strength = map(d, 0, 300, 15, 0); 
        repel.setMag(strength);
        this.applyForce(repel);
        this.lockTimer = 0; 
      }
    }
  }

  applyForce(f) { this.acc.add(f); }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
    
    // Bordes infinitos
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  arrive(t) {
    let desired = p5.Vector.sub(t, this.pos);
    let d = desired.mag();
    let s = (d < 100) ? map(d, 0, 100, 0, this.maxSpeed) : this.maxSpeed;
    desired.setMag(s);
    let steer = p5.Vector.sub(desired, this.vel);
    steer.limit(this.maxForce * 2);
    return steer;
  }

  wander() {
    let angle = noise(this.pos.x * 0.01, this.pos.y * 0.01, frameCount * 0.02) * TWO_PI * 4;
    return p5.Vector.fromAngle(angle).mult(0.6);
  }

  show(vol) {
    let s = (this.lockTimer > 0) ? 100 : 50;
    let b = (this.lockTimer > 0) ? 40 : 85;
    
    // Vibración individual musico-reactiva
    let vX = random(-1, 1) * this.jitterFactor * vol * 12;
    let vY = random(-1, 1) * this.jitterFactor * vol * 12;

    fill(this.h, s, b, 90);
    noStroke();
    push();
    translate(this.pos.x + vX, this.pos.y + vY);
    if (this.shapeType === 0) rect(0, 0, this.size, this.size);
    else if (this.shapeType === 1) ellipse(0, 0, this.size);
    else triangle(-this.size/2, this.size/2, 0, -this.size/2, this.size/2, this.size/2);
    pop();
  }
}

// --- INTERFAZ Y AUXILIARES ---
function mousePressed() {
  if (!started) {
    userStartAudio();
    song.play();
    fullscreen(true);
    started = true;
  }
}

function drawStartScreen() {
  textAlign(CENTER);
  fill(0, 0, 20);
  textSize(24);
  text("PAGANINI: INEFABLE", width/2, height/2 - 20);
  textSize(12);
  text("PULSA PARA PANTALLA COMPLETA", width/2, height/2 + 20);
}

function drawCustomCursor(vol, h) {
  push();
  translate(mouseX, mouseY);
  rotate(frameCount * 0.1);
  noFill();
  stroke(h, 100, 50);
  strokeWeight(3);
  let p = map(vol, 0, 0.5, 30, 90);
  triangle(-p/2, p/2, 0, -p/2, p/2, p/2);
  pop();
}

function spawnSphere() {
  let s = Bodies.circle(random(width), -50, 20, { restitution: 0.7 });
  spheres.push(s);
  World.add(world, s);
}

function drawSpheres() {
  fill(0, 0, 10, 10);
  for (let i = spheres.length - 1; i >= 0; i--) {
    let s = spheres[i];
    ellipse(s.position.x, s.position.y, 40);
    if (s.position.y > height + 100) {
      World.remove(world, s);
      spheres.splice(i, 1);
    }
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  initWordPoints();
}

```

## Bitácora de reflexión
