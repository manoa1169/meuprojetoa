// Variáveis da bolinha
let xBolinha = 300;
let yBolinha = 200;
let diametro = 20;
let raio = diametro / 2;

// Variáveis do oponente
let xRaqueteOponente = 585;
let yRaqueteOponente = 150;

// Velocidade da bolinha
let velocidadeXBolinha = 6;
let velocidadeYBolinha = 6;

// Variáveis da raquete
let xRaquete = 5;
let yRaquete = 150;
let raqueteComprimento = 10;
let raqueteAltura = 90;

// Placar do jogo
let meusPontos = 0;
let pontosDoOponente = 0;

// Controle de colisão
let colidiu = false;

function setup() {
  createCanvas(600, 400);
}

function draw() {
  background(0);

  // Mostrar elementos do jogo
  mostraBolinha();
  mostraRaquete(xRaquete, yRaquete);
  mostraRaquete(xRaqueteOponente, yRaqueteOponente);

  // Movimentos e colisões
  movimentaBolinha();
  verificaColisaoBorda();
  movimentaMinhaRaquete();
  verificaColisaoRaquete(xRaquete, yRaquete);
  verificaColisaoRaquete(xRaqueteOponente, yRaqueteOponente);
  movimentaRaqueteOponente();

  // Placar
  incluiPlacar();
  marcaPonto();

  // Verifica fim do jogo
  verificaFimDeJogo();
}

function mostraBolinha() {
  fill(random(100, 255), random(100, 255), random(100, 255));
  circle(xBolinha, yBolinha, diametro);
}

function movimentaBolinha() {
  xBolinha += velocidadeXBolinha;
  yBolinha += velocidadeYBolinha;
}

function verificaColisaoBorda() {
  if (xBolinha + raio > width || xBolinha - raio < 0) {
    velocidadeXBolinha *= -1;
  }
  if (yBolinha + raio > height || yBolinha - raio < 0) {
    velocidadeYBolinha *= -1;
  }
}

function mostraRaquete(x, y) {
  fill(255);
  rect(x, y, raqueteComprimento, raqueteAltura);
}

function movimentaMinhaRaquete() {
  if (keyIsDown(UP_ARROW)) {
    yRaquete -= 10;
  }
  if (keyIsDown(DOWN_ARROW)) {
    yRaquete += 10;
  }
}

function verificaColisaoRaquete(x, y) {
  colidiu = collideRectCircle(x, y, raqueteComprimento, raqueteAltura, xBolinha, yBolinha, diametro);
  if (colidiu) {
    velocidadeXBolinha *= -1;
  }
}

function movimentaRaqueteOponente() {
  // Oponente segue a bolinha, mas com um atraso
  let velocidadeYOponente = yBolinha - yRaqueteOponente - raqueteComprimento / 2 - 30;
  yRaqueteOponente += velocidadeYOponente * 0.3; // Atraso no movimento
}

function incluiPlacar() {
  textAlign(CENTER);
  textSize(16);
  fill(255, 140, 0);
  rect(150, 10, 40, 20);
  fill(255);
  text(meusPontos, 170, 26);
  fill(255, 140, 0);
  rect(450, 10, 40, 20);
  fill(255);
  text(pontosDoOponente, 470, 26);
}

function marcaPonto() {
  if (xBolinha > 590) {
    meusPontos += 1;
    resetBolinha();
  }
  if (xBolinha < 10) {
    pontosDoOponente += 1;
    resetBolinha();
  }
}

function resetBolinha() {
  xBolinha = 300;
  yBolinha = 200;
  velocidadeXBolinha *= -1;
}

function verificaFimDeJogo() {
  if (meusPontos === 10 || pontosDoOponente === 10) {
    noLoop();
    textSize(32);
    fill(255);
    textAlign(CENTER, CENTER);
    if (meusPontos === 10) {
      text("Você Venceu!", width / 2, height / 2);
    } else {
      text("Oponente Venceu!", width / 2, height / 2);
    }
  }
}
