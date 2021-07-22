
Esse projeto consiste na utilização da função ```analogRead```  juntamente com o sensor de temperatura visando aplicações do mundo real.

O sensor de temperatura  é um componente eletrônico capaz de medir a temperatura ao seu redor. Este dispositivo é composto por diodos sensíveis à temperatura. Assim, quando há uma mudança nessa grandeza, a tensão que passa pelo sensor é modificada. Com isso, é possível criar uma escala para converter a tensão que o NodeMCU recebe em uma escala de temperatura, como Celsius, Fahrenheit e Kelvin. O sensor LM35, por exemplo, apresenta uma taxa de variação conhecida de 10mV/°C e lê valores de -55°C até 150°C, com uma precisão de ± 0.5°C a 25°C.

A seguir, temos algumas das principais aplicações do sensor:
* Regulação da temperatura de um ar-condicionado;
* Medição da temperatura corpórea nos termômetros;
* Auxílio em processos industriais dependentes de temperatura;
* Regulação da temperatura de _freezers_;


Seja criativo para pensar em outras aplicações, como regular temperatura do carro/abrir as janelas caso precise deixar seu _pet_ sozinho!

O circuito envolvendo o sensor de temperatura envolve as seguintes competências trabalhadas no módulo 1:

- [x] Leitura Analógica
- [x] Escrita Analógica

> Nesse projeto você irá aprender a utilizar o sensor de temperatura, juntamente com o processo de escrita analógica com a função  ```analogWrite```.

## Conteúdo
- [Materiais Necessários](#materiais-necessários)
- [Montagem do Circuito](#montagem-do-circuito)
- [O Código do Circuito](#o-c&oacute;digo-do-circuito)

## Materiais Necessários
1. NodeMCU
2. 1 Sensor de temperatura LM35
3. 1 LED RGB
4. 3 Resistores de 220Ω
5. Protoboard
6. Jumpers

## Montagem do Circuito
O circuito deve ser montado como mostra a figura abaixo, representado na protoboard.

![Protoboard](assets/protoboard.png)

É necessário conectar os 3 terminais menores do LED em saídas PWM do NODEMCU, com um resistor de 220Ω entre as conexões (para limitar a corrente), pois através dessa conexão, é possível, por conta do pino ser PWM, controlar o envio de diferentes valores de tensão para os terminais do LED, assim modificando sua cor.

Já o terminal maior, deve ser conectado ao VCC caso o LED seja ânodo comum, ou ao GND caso seja cátodo comum.

O sensor de temperatura é uma entrada que gera um sinal analógico e por esse mesmo motivo o terminal do meio do sensor deve ser conectado a um pino PWM do ESP. Os outros terminais devem ser conectados no VCC e no GND, conforme a figura abaixo:

![Terminais](assets/terminais.png)


## O código do Circuito

Use o código que está em [code](code/code.ino) ou copie o código abaixo:
 
```C++
//Pino do sensor de temperatura
const int LM35 = 17;      //A0

//Pinos das cores
const int redPin    = 4;  //D2
const int greenPin  = 5;  //D1
const int bluePin   = 16; //D0

//Pino do led
const int led = 2;        //D4


float temperatura;

void ledColor(int temperatura) {
  if(temperatura < 20){
    digitalWrite(redPin, 0);
    digitalWrite(greenPin, 0);
    digitalWrite(bluePin, 255);
  }else if(temperatura >= 20 and temperatura <= 70){
    digitalWrite(redPin, 0);
    digitalWrite(greenPin, 255);
    digitalWrite(bluePin, 0);
  }else if(temperatura > 70){
    digitalWrite(redPin, 255);
    digitalWrite(greenPin, 0);
    digitalWrite(bluePin, 0);
  }
}


void setup() {
  pinMode(redPin,OUTPUT);
  pinMode(greenPin,OUTPUT);
  pinMode(bluePin,OUTPUT);
  
  pinMode(led,OUTPUT);

  pinMode(LM35, INPUT);
  
  Serial.begin(115200);
}

 
void loop() {
  temperatura = map(analogRead(LM35), 0, 1023, 0, 150);

  ledColor(temperatura);
  
  Serial.print("Temperatura: ");
  Serial.println(temperatura);
  
  delay(2000);
}
```
Diferentemente dos outros circuitos que foram mostrados nesse módulo, esse circuito precisa de uma biblioteca em específico: [``Ultrasonic.h``](library/Ultrasonic.zip) **Clique no link ao lado e realize o download da biblioteca antes de prosseguir para os próximos passos.**

No entanto você deve estar se perguntando como posso adicionar uma nova biblioteca à IDE do arduino. Para adicionar uma nova biblioteca na IDE do arduíno basta clicar em Sketch>Incluir Biblioteca e Adicionar biblioteca.ZIP (A biblioteca pode estar em .ZIP ou em uma pasta), como pode ser visualizado na imagem abaixo:

![TEXTO](assets/library.png)

Após adicionar a biblioteca, provavelmente você vai querer abrí-la. Para realizar esta ação basta clicar em Arquivo, depois em exemplos, procurar a biblioteca requerida e clicá-la. O conjunto das ações descritas anteriormente podem ser visualizadas na imagem a seguir:

![TEXTO](assets/lib.png)

A partir dessa biblioteca podemos fazer uso do sensor HCSR-04.

O código acima começa com a declaração e associação das saídas e entradas utilizadas. O pino "echo" foi associado à constante 4 (D2) e o pino "trigger" à constante 5 (D1). Feito isso, partimos para o ```void setup``` onde é necessário iniciar a comunicação serial através do comando ``Serial.begin`` e declarar as entradas e saídas por meio do ``pinMode``.

**Nota:** O pino trigger do sensor é uma saída, uma vez que emite o sinal de onda mecânica, enquanto que o echo recebe esse pulso após refletir no objeto e por esse mesmo motivo é uma entrada.

Posteriormente no ```void loop``` declaramos uma outra função ```hcsr04()``` que vai ser responsável pela ativação do funcionamento do sensor. Além disso, configuramos o monitor serial para mostrar um texto(Distância) seguido do resultado e um outro texto (cm)

Por fim e não menos importante, temos a função ```hcsr04``` que é chamada dentro da ```void loop```. Essa função começa enviando um pulso de onda mecânica pelo trigger:

```
    digitalWrite(trigPin, LOW); 
    delayMicroseconds(2); 
    digitalWrite(trigPin, HIGH); 
    delayMicroseconds(10); 
    digitalWrite(trigPin, LOW);
```
Em seguida utiliza-se a função ``Ranging`` responsável por converter uma medida de tempo (tempo que levou desde o envio do pulso até o recebimento no echo) em uma medida de distância em cm. Depois, armazena-se essa distância em cm, na variável distância.
Por fim, declara-se a variável result que como podemos visualizar abaixo, nada mais é que a conversão da variável distância em uma string.
```result = String(distancia); ```

Caso tenha encontrado algum problema abra uma _issue_ clicando [aqui](https://github.com/PETEletricaUFBA/IoT/issues/new)

![Circuit](assets/circuit.png)

> Pense na utilização do sensro ultrassônico na sua casa ou em outras aplicações do seu cotidiano. 
