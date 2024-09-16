// Sensor ultrasónico
const int echo = 2;
const int trig = 3;
volatile unsigned int distancia;
const int cerca = 15;
const unsigned long duracionMaxPulso = 5000;

// Controlador de motores
const int ENA = 11;
const int IZQ1 = 9;
const int IZQ2 = 8;
const int DER1 = 6;
const int DER2 = 7;
const int ENB = 10;

void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);
  pinMode(ENA, OUTPUT);
  pinMode(IZQ1, OUTPUT);
  pinMode(IZQ2, OUTPUT);
  pinMode(DER1, OUTPUT);
  pinMode(DER2, OUTPUT);
  pinMode(ENB, OUTPUT);
}

void loop() {
  medir();
  if (distancia < cerca) {
    obstaculo();
  } else {
    adelante();
  }
}

void medir() {
  digitalWrite(trig, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);
  distancia = pulseIn(echo, HIGH, duracionMaxPulso) / 29.1 / 2;
  if (distancia == 0) {
    distancia = duracionMaxPulso / 29.1 / 2;  // Corrige el cálculo de la distancia cuando no hay retorno
  }
}

void adelante() {
  digitalWrite(IZQ1, HIGH);
  digitalWrite(IZQ2, LOW);
  digitalWrite(DER1, HIGH);
  digitalWrite(DER2, LOW);
  analogWrite(ENA, 90);
  analogWrite(ENB, 90);
}

void derecha() {
  digitalWrite(IZQ1, HIGH);
  digitalWrite(IZQ2, LOW);
  digitalWrite(DER1, LOW);
  digitalWrite(DER2, HIGH);
  analogWrite(ENA, 150);
  analogWrite(ENB, 200);
}

void detenido() {
  digitalWrite(IZQ1, LOW);
  digitalWrite(IZQ2, LOW);
  digitalWrite(DER1, LOW);
  digitalWrite(DER2, LOW);
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
}

void obstaculo() {
  detenido();
  delay(300);
  derecha();
  delay(230);  // Corregido, se eliminó una coma sobrante
  detenido();
  delay(100);
}
