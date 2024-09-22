# Montagem Básica com LEDs
## Descrição
Este projeto demonstra uma montagem básica utilizando um Arduino Uno e dois LEDs. O objetivo é aprender como controlar LEDs através do Arduino, ligando e desligando-os de forma sequencial simulando luzes de sinalizaçao de garagem.

## Materiais Necessários
+ Arduino Uno

+ 2 LEDs

+ 2 Resistores de 220Ω (para limitar a corrente dos LEDs)

+ Fios de conexão (jumpers)

+ Protoboard

## Montagem do Circuito

**1.Conecte os LEDs:**

+ O primeiro LED deve ter o anodo (terminal positivo) conectado ao pino digital 2 do Arduino, e o cátodo (terminal negativo) conectado a um resistor de 220Ω.

+ O segundo LED deve ter o anodo conectado ao pino digital 3 do Arduino, e o cátodo conectado a outro resistor de 220Ω.

+ Os outros terminais dos resistores devem ser conectados ao GND no Arduino.

**2.Conexões com a Protoboard:**

+ Use a protoboard para facilitar as conexões entre os LEDs, resistores e o Arduino.


Utilize os fios de conexão (jumpers) para ligar os pinos do Arduino aos componentes montados na protoboard.

```
const int ledPin1 = 4;
const int ledPin2 = 5;
const int ledPin3 = 6;

void setup() {
// Define os pinos dos LEDs como saídas
pinMode(ledPin1, OUTPUT);
pinMode(ledPin2, OUTPUT);
pinMode(ledPin3, OUTPUT);
}

void loop() {
// Acende o primeiro LED e espera 1 segundo
digitalWrite(ledPin1, HIGH);
delay(1000);
digitalWrite(ledPin1, LOW);


digitalWrite(ledPin2, HIGH);
delay(1000);
digitalWrite(ledPin2, LOW);


digitalWrite(ledPin3, HIGH);
delay(1000);
digitalWrite(ledPin3, LOW);
}
```

+ ![Figura](https://github.com/user-attachments/assets/faa4167d-cc3b-4587-bd64-24a5f8646cf4)
