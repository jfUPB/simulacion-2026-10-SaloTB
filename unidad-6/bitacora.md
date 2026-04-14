# Unidad 6

## Bitácora de proceso de aprendizaje

### Actividad 2 
Explica con tus propias palabras qué es un agente autónomo.
Un objeto que tiene sus propias fuerzas y en base a su entorno interactua 

Explica qué es una steering force.
Ua fuerza que media el deseo que tiene el agente para que el movimiento no sea directo sino que se sienta más natural

Compara una steering force con una fuerza externa como la gravedad.
El stering force ayuda a regular la fuerza porpia del agente, mientras que la fuerza de gravedad es una fuerza externa aplicada a este objet, no su propio "deseo"

Describe por qué estas ideas son útiles para diseñar comportamiento visual y no solo para simular movimiento.
Porque el agente tiene en cuenta su entorno y esto ayuda agenerar reaccion y accion que permite un comportamiento por parte del mismo y no solo un movimeinto causado por fuerzas externas

### Actividad 3

tipo de movimiento que producen:

nivel de control visual:

nivel de emergencia:

tipo de atmósfera o sensación,
relación posible con una pieza musical,
ventajas y limitaciones de cada uno.

## Bitácora de aplicación 

### Actividad 6 

https://editor.p5js.org/SaloTB/sketches/BPncqGIVc 

Prompt:

Crea el codigo para una obra generativa audioreactiva basada en "The nature of code" hasta el capitulo de autonomus agents. A continuación pondre las acracteristicas que debe de tener esta obra generativa:

Descripción visual base: Debe de verse como un ojo que ocupe el 80% de la pantalla, este ojo estara compuesto por dos lineas redondeadas (es una forma simple e interpretativa de un ojo) dentro de este ojo hay una pipula de colo negro y por fuera se veran las lineas carcateristicas de los ojos. Estas lineas en realidad son un monton de agentes que siguen un campo de flujo que va cambiando entre más se acelera la canción. La esfera del entro del ojo actua como un objeto de deseo para los agentes, quienes al tocarla desaparecen. 

Agentes: Los agentes tienen dos deseos, el cambo de flujo y la esfera dentro del ojo (pupila). Además de esto tienen un floking, donde desearan acercarse a sus compañeras dejando un espacio entre ellas de apenas 0.5, buscaran alinearse con el movimeinto de estas mismas si se desvian de su grupo y se uniran a la dirección que la mayoria esten siguiendo. Los agentes ademas cambiaran de color al "nacer" (Nacen desde las lineas de los ojos, desde dentro) cuando la musica llegue a puntos altos o de highpitch

Campo de Flujo: Este es inviisble y tiene 4 cambios que se van aplicando dependiendo de la velocidad de la musica. Cuando la mucisa va lento son lineas sicsagueantes que generan hondas ocn los agentes. Cuando va un poco más rapido estas lineas se vuelven menos como hondas y más como lineas rectas. Cuando sube un poco más comienzan a volverse más puntiagudos en el movimiento y cuando esta en el punto más rapido se vuelven un espiral en su totalidad. 

Cancion a utilizar: My time del juego Omori (no agregues la canción,pero señalame dond ey en que formato la debo colocar en el codigo)

    let song;
    let fft;
    let amp; 
    let flowfield;
    let agents = [];
    let pupilPos;
    let started = false;
    let pupilSize = 90;
    
    // Paleta de la imagen (Magenta, Borgoña, Lavanda, Cyan, Oro)
    const palette = ['#b5014d', '#640d33', '#d386a7', '#00c2c7', '#c7a33a'];
    let currentGlobalColor;
    
    // Geometría del ojo
    let eyeW, eyeH;
    
    function preload() {
      song = loadSound('my_time.mp3');
    }
    
    function setup() {
      createCanvas(windowWidth, windowHeight);
      eyeW = width * 0.8;
      eyeH = height * 0.5;
      pupilPos = createVector(width / 2, height / 2);
      currentGlobalColor = color(palette[0]);
      
      fft = new p5.FFT(0.8, 512);
      amp = new p5.Amplitude(); 
      flowfield = new FlowField(40); 
    
      for (let i = 0; i < 500; i++) {
        spawnAgent();
      }
    }
    
    function draw() {
      if (!started) {
        showStartScreen();
        return;
      }
    
      // Fondo basado en la oscuridad de la pintura (azul medianoche profundo)
      background(15, 15, 25, 60);
    
      fft.analyze();
      let vol = amp.getLevel(); 
      let bass = fft.getEnergy("bass");
      let treble = fft.getEnergy("treble"); 
      let highMids = fft.getEnergy(1000, 3000);
    
      // 1. Cambio de color rítmico (Cada vez que hay un pico de agudos o ritmo fuerte)
      if (treble > 185 || highMids > 170) {
        currentGlobalColor = color(random(palette));
      }
    
      // Control de velocidad y cortes abruptos
      let globalSpeed = map(vol, 0, 0.4, 0, 3.5);
      if (vol < 0.005) globalSpeed = 0; 
    
      // Interacción del iris
      let targetPupil = createVector(
        constrain(mouseX, width/2 - eyeW/4, width/2 + eyeW/4),
        constrain(mouseY, height/2 - eyeH/6, height/2 + eyeH/6)
      );
      pupilPos = p5.Vector.lerp(pupilPos, targetPupil, 0.1);
      pupilSize = map(bass, 0, 255, 80, 140);
    
      // Campo de flujo
      flowfield.update(bass, highMids);
      flowfield.display();
    
      // Dibujar estructura del ojo
      drawSharpEye(bass);
    
      // Gestión de agentes
      for (let i = agents.length - 1; i >= 0; i--) {
        let a = agents[i];
        
        a.applyForce(a.follow(flowfield).mult(1.0));
        a.applyForce(a.flock(agents).mult(1.8)); 
        a.applyForce(a.seek(pupilPos).mult(0.5));
        a.applyForce(a.boundaries().mult(5.0)); 
    
        a.update(globalSpeed);
        a.display(treble); // Reacciona al tamaño con los agudos
    
        // Desaparición y respawn
        if (p5.Vector.dist(a.pos, pupilPos) < pupilSize/2) {
          agents.splice(i, 1);
          spawnAgent();
        }
      }
    
      // Dibujar Pupila
      fill(0);
      noStroke();
      ellipse(pupilPos.x, pupilPos.y, pupilSize);
    }
    
    function drawSharpEye(bass) {
      stroke(255, map(bass, 0, 255, 40, 100));
      strokeWeight(2);
      noFill();
      
      let xL = width/2 - eyeW/2;
      let xR = width/2 + eyeW/2;
      let yC = height/2;
    
      beginShape();
      vertex(xL, yC);
      bezierVertex(width/2 - eyeW/4, yC - eyeH, width/2 + eyeW/4, yC - eyeH, xR, yC);
      endShape();
      
      beginShape();
      vertex(xL, yC);
      bezierVertex(width/2 - eyeW/4, yC + eyeH, width/2 + eyeW/4, yC + eyeH, xR, yC);
      endShape();
    }
    
    function isInsideEye(p) {
      let xL = width/2 - eyeW/2;
      let xR = width/2 + eyeW/2;
      if (p.x < xL || p.x > xR) return false;
      let normX = map(p.x, xL, xR, -1, 1);
      let curveHeight = (1 - normX * normX) * (eyeH); 
      return (p.y > height/2 - curveHeight && p.y < height/2 + curveHeight);
    }
    
    class Agent {
      constructor(x, y) {
        this.pos = createVector(x, y);
        this.vel = p5.Vector.random2D();
        this.acc = createVector(0, 0);
        this.maxSpeed = random(2, 5);
        this.maxForce = 0.5;
        this.baseSize = 5;
      }
    
      applyForce(f) { this.acc.add(f); }
    
      boundaries() {
        if (!isInsideEye(this.pos)) {
          let steer = p5.Vector.sub(createVector(width/2, height/2), this.pos);
          steer.setMag(this.maxSpeed).sub(this.vel).limit(this.maxForce * 3);
          return steer;
        }
        return createVector(0, 0);
      }
    
      update(speedMult) {
        this.vel.add(this.acc);
        this.vel.limit(this.maxSpeed * speedMult);
        this.pos.add(this.vel);
        this.acc.mult(0);
      }
    
      follow(ff) {
        let desired = ff.lookup(this.pos).mult(this.maxSpeed);
        return p5.Vector.sub(desired, this.vel).limit(this.maxForce);
      }
    
      seek(target) {
        let desired = p5.Vector.sub(target, this.pos).setMag(this.maxSpeed);
        return p5.Vector.sub(desired, this.vel).limit(this.maxForce * 0.5);
      }
    
      flock(others) {
        let sep = createVector(0, 0);
        let count = 0;
        for (let i = 0; i < others.length; i += 15) { 
          let d = p5.Vector.dist(this.pos, others[i].pos);
          if (d > 0 && d < 15) { 
            let diff = p5.Vector.sub(this.pos, others[i].pos).normalize().div(d);
            sep.add(diff);
            count++;
          }
        }
        if (count > 0) sep.div(count).setMag(this.maxSpeed).sub(this.vel).limit(this.maxForce);
        return sep;
      }
    
      display(treble) {
        // El tamaño pulsa con los agudos (estética glitch)
        let dynamicSize = this.baseSize;
        if (treble > 210) dynamicSize = random(10, 20);
    
        stroke(currentGlobalColor);
        strokeWeight(dynamicSize);
        point(this.pos.x, this.pos.y);
      }
    }
    
    class FlowField {
      constructor(r) {
        this.res = r;
        this.cols = ceil(width / r);
        this.rows = ceil(height / r);
        this.field = new Array(this.cols * this.rows);
      }
    
      update(bass, highMids) {
        for (let i = 0; i < this.cols; i++) {
          for (let j = 0; j < this.rows; j++) {
            let x = i * this.res;
            let y = j * this.res;
            let v = p5.Vector.sub(pupilPos, createVector(x, y));
            let angle = v.heading();
            if (bass > 190) angle += HALF_PI;
            else if (highMids > 150) angle += sin(frameCount * 0.1 + i) * 0.5;
            this.field[i + j * this.cols] = p5.Vector.fromAngle(angle);
          }
        }
      }
    
      display() {
        // Líneas del campo muy sutiles con el color actual
        let c = color(currentGlobalColor);
        c.setAlpha(15);
        stroke(c);
        strokeWeight(1);
        for (let i = 0; i < this.cols; i++) {
          for (let j = 0; j < this.rows; j++) {
            let x = i * this.res;
            let y = j * this.res;
            let v = this.field[i + j * this.cols];
            if (isInsideEye(createVector(x, y))) {
              line(x, y, x + v.x * 10, y + v.y * 10);
            }
          }
        }
      }
    
      lookup(p) {
        let col = floor(constrain(p.x / this.res, 0, this.cols - 1));
        let row = floor(constrain(p.y / this.res, 0, this.rows - 1));
        return this.field[col + row * this.cols].copy();
      }
    }
    
    // 8 ENTRADAS DE AGENTES (Cardinal y Diagonal)
    function spawnAgent() {
      let x, y;
      let type = floor(random(8));
      let w = eyeW / 2;
      let h = eyeH / 1.5;
    
      switch(type) {
        case 0: x = width/2 - w; y = height/2; break; // Oeste
        case 1: x = width/2 + w; y = height/2; break; // Este
        case 2: x = width/2; y = height/2 - h; break; // Norte
        case 3: x = width/2; y = height/2 + h; break; // Sur
        case 4: x = width/2 - w*0.7; y = height/2 - h*0.7; break; // Noroeste
        case 5: x = width/2 + w*0.7; y = height/2 - h*0.7; break; // Noreste
        case 6: x = width/2 - w*0.7; y = height/2 + h*0.7; break; // Suroeste
        case 7: x = width/2 + w*0.7; y = height/2 + h*0.7; break; // Sureste
      }
      agents.push(new Agent(x, y));
    }
    
    function showStartScreen() {
      background(15, 15, 25);
      fill(255);
      textAlign(CENTER);
      textSize(18);
      text("MY TIME - OMORI\n\n[ Clic para abrir el ojo ]", width/2, height/2);
    }
    
    function mousePressed() {
      if (!started) {
        userStartAudio();
        song.play();
        started = true;
      }
    }

## Bitácora de reflexión
