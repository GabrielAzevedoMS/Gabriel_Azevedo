# Jogo de Tempo de Reação

## Como funciona

O jogo consiste em um LED aceso e dois botões. Cada jogador tem o objetivo de apertar o botão correspondente ao seu lado assim que o LED apagar. O jogador que apertar o botão mais rápido ganha 1 ponto. O jogo continua indefinidamente, somando pontos até que os jogadores decidam parar. Caso nenhum jogador aperte o botão em até 5 segundos após o LED apagar, o jogo termina.

## Componentes Necessários

- 1 protoboard
- 2 botões
- 1 LED
- 1 display LCD I2C
- 1 servo motor

## Código do Projeto

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

// Configurações do Servo
Servo servo;
const int ledPin = 9;         // Pino do LED
const int buttonRightPin = 2; // Pino do botão da direita
const int buttonLeftPin = 3;  // Pino do botão da esquerda
const int servoPin = 4;       // Pino do servo

// Configurações do LCD
LiquidCrystal_I2C lcd(0x27, 16, 2);
bool gameActive = false;       // Controle do estado do jogo
int scoreLeft = 0;             // Placar do jogador da esquerda
int scoreRight = 0;            // Placar do jogador da direita

void setup() {
  pinMode(ledPin, OUTPUT);
  pinMode(buttonRightPin, INPUT_PULLUP); 
  pinMode(buttonLeftPin, INPUT_PULLUP);  
  
  servo.attach(servoPin);
  servo.write(90); // Posição inicial do servo (central)

  lcd.begin(16, 2);  // Configura o LCD com 16 colunas e 2 linhas
  lcd.backlight();
  lcd.print("Placar:");
  atualizarPlacar(); // Atualiza o placar no LCD
  digitalWrite(ledPin, HIGH); // LED começa aceso
}

void loop() {
  if (!gameActive) {
    gameActive = true; // O jogo está ativo
    delay(random(2000, 5000)); // Espera um tempo aleatório entre 2 e 5 segundos
    digitalWrite(ledPin, LOW); // Apaga o LED
    esperarVencedor();
    gameActive = false; // O jogo terminou
    digitalWrite(ledPin, HIGH); // Acende o LED novamente para o próximo jogo
  }
}

void esperarVencedor() {
  unsigned long startWait = millis(); // Marca o início da espera

  while (true) {
    if (digitalRead(buttonRightPin) == LOW) {
      delay(1000); // Delay para garantir que o botão foi pressionado
      servo.write(0); // Levanta o servo para a direita
      delay(1000);      // Mantém por 1 segundo
      servo.write(90);  // Retorna à posição central
      scoreRight++;     // Incrementa o placar do jogador da direita
      atualizarPlacar(); // Atualiza o placar no LCD
      break;            // Sai do loop
    } else if (digitalRead(buttonLeftPin) == LOW) {
      delay(1000); // Delay para garantir que o botão foi pressionado
      servo.write(180);   // Levanta o servo para a esquerda
      delay(1000);      // Mantém por 1 segundo
      servo.write(90);  // Retorna à posição central
      scoreLeft++;      // Incrementa o placar do jogador da esquerda
      atualizarPlacar(); // Atualiza o placar no LCD
      break;            // Sai do loop
    }

    // Se passou mais de 5 segundos sem resposta, encerra a espera
    if (millis() - startWait > 5000) {
      break; // Sai do loop se ninguém pressionou
    }
  }
}

void atualizarPlacar() {
  lcd.setCursor(0, 1); // Move o cursor para a segunda linha
  lcd.print("E:"); 
  lcd.print(scoreLeft); 
  lcd.print(" D:"); 
  lcd.print(scoreRight);
}
```
## Esquema 
![Esquema](https://github.com/user-attachments/assets/269f0bea-2242-45f2-8592-9a65be884678)


---

## Bibliotecas Necessárias

Antes de começar, certifique-se de que as seguintes bibliotecas estão instaladas no seu Arduino IDE:

1. **Wire.h** (já incluída no Arduino IDE)
   - Biblioteca usada para comunicação I2C com o display LCD.

2. **LiquidCrystal_I2C.h**
   - Esta biblioteca é usada para controlar o display LCD I2C.
   - Instale-a através do Gerenciador de Bibliotecas do Arduino IDE:
     - No Arduino IDE, vá em **Sketch** > **Incluir Biblioteca** > **Gerenciar Bibliotecas...**
     - Pesquise por **LiquidCrystal I2C** e instale a biblioteca de **Marco Schwartz**.

3. **Servo.h** (já incluída no Arduino IDE)
   - Esta biblioteca é necessária para controlar o servo motor.

---
## Passo a passo para montar o circuito

1. **Conectar o LED:**
   - Conecte o ânodo (perna longa) do LED ao **pino digital 9** do Arduino.
   - Conecte o cátodo (perna curta) do LED a um **resistor** de 220 ohms, e depois conecte o resistor ao **GND**.

2. **Conectar os Botões:**
   - Para o botão da direita:
     - Conecte uma extremidade do botão ao **pino digital 2** do Arduino.
     - A outra extremidade do botão deve ser conectada ao **GND**.
   - Para o botão da esquerda:
     - Conecte uma extremidade do botão ao **pino digital 3** do Arduino.
     - A outra extremidade do botão deve ser conectada ao **GND**.

3. **Conectar o Servo Motor:**
   - Conecte o fio de sinal do servo (geralmente amarelo ou laranja) ao **pino digital 4** do Arduino.
   - Conecte o fio de alimentação (vermelho) ao **5V** do Arduino.
   - Conecte o fio de terra (preto ou marrom) ao **GND**.

4. **Conectar o Display LCD I2C:**
   - Conecte os pinos **SDA** e **SCL** do display LCD aos pinos correspondentes do Arduino:
     - **SDA** → A4 (no Arduino UNO)
     - **SCL** → A5 (no Arduino UNO)
   - Conecte o pino **VCC** do display ao **5V** do Arduino.
   - Conecte o pino **GND** do display ao **GND** do Arduino.

5. **Alimentação:**
   - Certifique-se de que todos os componentes estão conectados ao **GND** e ao **5V** corretos no Arduino.

6. **Copiar e Enviar o Código para o Arduino:**
   - Verifique se você copiou o código do projeto corretamente e sem erros.
   - Conecte o Arduino ao computador usando um cabo USB.
   - No **Arduino IDE**, selecione a porta correta em **Ferramentas** > **Porta**.
   - Clique no botão de **Upload** no Arduino IDE para enviar o código ao Arduino.
---
## Instruções para Jogar o Jogo de Tempo de Reação

### Durante o Jogo

1. Quando o LED apagar, os dois jogadores precisam apertar seus respectivos botões o mais rápido possível.
2. O jogador que apertar o botão mais rápido após o LED apagar, ganha 1 ponto.
   - O placar do jogador da esquerda aparece como **"E:"**.
   - O placar do jogador da direita aparece como **"D:"**.
3. Se nenhum dos jogadores apertar o botão dentro de 5 segundos após o LED apagar, o jogo termina e o loop acaba.

### Como Vencer

- O jogo pode continuar indefinidamente, ou até que os jogadores decidam parar.
- O jogador com mais pontos vence!

---

## Dicas Adicionais

- **Modificação do Tempo de Reação:** No código, a função `random()` controla o tempo que o LED permanece aceso. Se desejar, você pode ajustar os limites do tempo para alterar a dificuldade.
- **Debug:** Se o display LCD não estiver funcionando corretamente, verifique as conexões SDA e SCL, e certifique-se de que o endereço I2C no código está correto. O endereço padrão geralmente é `0x27`, mas pode variar conforme o modelo do seu display.

---
## Divirta-se! :)
