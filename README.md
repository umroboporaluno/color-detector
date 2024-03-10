<h1 align="center">🤖 Color detector</h1>
<h4 align="center">Um código para um sensor de cor.</h4>
<br />

# :pushpin: Tabela de Conteúdos

- [Como usar](#gear-como-usar)
- [DIY - Do It Yourself](#construction_worker-do-it-yourself)
- [Licença](#page_facing_up-licença)

# :gear: Como usar
```shell
> Abra o Arduino IDE

> No menu do Arduino IDE, Clique em File > Open > File.

> Selecione o arquivo color_detector.ino

> Certifique-se de ter a biblioteca Adafruit_NeoPixel instalada!

> E pronto, já está pronto para usar!

```

# :construction_worker: Do It Yourself
```shell
> Comece criando o arquivo color_detector.ino
```

```c++
> color_detector.ino

// O código do arduino é lido de cima para baixo, então iremos seguir esse padrão no tutorial!

/**
* Vamos começar incluindo a biblioteca Adafruit_NeoPixel que nos permite controlar o componente neopixel.
* Tutorial de uso e documentação da biblioteca: https://github.com/adafruit/Adafruit_NeoPixel
**/
#include <Adafruit_NeoPixel.h>

/**
* Agora vamos iniciar os pinos para o circuito!
* Defina algum pino digital como o pino do NeoPixel, neste exemplo usamos o pino 3 na variável PINNP.
* Em seguida defina algum pino analógico como o pino do LDR, nos usamos o pino A0 na variável PINLDR.
**/

#define PINNP 3
#define PINLDR A0

/**
* Aqui, nos vamos iniciar o NeoPixel usando uma função da biblioteca que nós adicinamos anteriormente, 
* a partir desse momento, neste exemplo o NeoPixel começa a ser referenciado como "led".
**/
Adafruit_NeoPixel led = Adafruit_NeoPixel(1, PINNP, NEO_RGB + NEO_KHZ800);

/**
* Então, vamos declarar algumas váriáveis que vamos usar na detecção, como a vaíável de leitura, que se chama leitura e
* pela lógica do sensor, também precisamos de intervalos de valores que vão ser definidos por váriáveis, como:
* "maxR" que armazena o maior valor lido na calibração na cor vermelha, por isso o prefixo "max" e o sufixo "R"
* que se refera "red"(vermelho, em inglês) e o menor valor é guardado na minR. A mesma lógica é aplicada para as outras cores.
* 
* ATENÇÃO: Note que a váriável de mínimo começa com um valor alto porque se fosse inicidada com 0, não seria atualiada.
**/
int leitura = 0;
int maxR = 0, minR = 2000;
int maxG = 0, minG = 2000;
int maxB = 0, minB = 2000;

/**
* Agora vamos a nossa função void setup(), nela vamos ligar o neopixel e fazer a calibração
* das cores para a detecção.
**/
void setup() {
  //Preparando sensores
  led.begin();
  led.show();
  Serial.begin(9600);

  /**
  * A calibração é simles, primeiro o led acenderá nna cor a qual ele vai calibrar, 
  * e após isso aguarda 1,5 segundos para iniciar a leitura e armazenar os valores nas variáveis.
  * Esse processo é repetido três vezes, um para cada cor.
  * 
  * Nota: Esse código seria mais legível e otimizado usando mais funções,
  * porém os utilizadores não haviam dominado o conceito ainda, então seu uso foi minimizado.
  **/
  //Calibrando Verde
  led.setPixelColor(0, 255, 0, 0); //Indicando que a cor Verde será calibrada
  led.show();
  delay(1500);
  led.setPixelColor(0, 255, 255, 255); 
  led.show();
  calibraLDR(minG, maxG);
  delay(1500);
  //Calibrando Vermelho
  led.setPixelColor(0, 0, 255, 0); //Indicando que a cor Vermelho será calibrada
  led.show();
  delay(1500);
  led.setPixelColor(0, 255, 255, 255); 
  led.show();
  calibraLDR(minR, maxR);
  delay(1500);
  //Calibrando azul
  led.setPixelColor(0, 0, 0, 255); //Indicando que a cor azul será calibrada
  led.show();
  delay(1500);
  led.setPixelColor(0, 255, 255, 255); 
  led.show();
  calibraLDR(minB, maxB);
  delay(1500);
}

/**
* Após inciar os nossos componentes e calibrar a leitura, vamos para a função void loop(),
* nela, o LDR será lido e esse valor passará por laços de seleção que vão verificar se
* o ele está em algum dos intervalos que foram obtidos na calibração.
**/
void loop() {
  //
  
  //inciando leitura
  leitura = analogRead(PINLDR);

  Serial.println(leitura);

  //Detectando cores
  if(leitura < maxR && leitura > minR){
  Serial.println("Vermelho");
  
  }else if(leitura < maxG && leitura > minR){
  Serial.println("Verde");
  
  }else if(leitura < maxB && leitura > minB){
  Serial.println("Azul");
  }

  delay(500);

}

/**
* Essa é a função que faz a leitura de calibração de cada cor, nessa leitura o utilizador deve
* deixar o sensor logo acima da cor a ser calibrada, e após 100 leituras, o menor e o maior valor
* vai ser enviado pras váriáveis que declaramos anteriormente.
**/
void calibraLDR(int &minVal, int &maxVal){
  minVal = 2000;
  maxVal = 0;
  
  for (int i = 0; i < 100; i++) {

    leitura = analogRead(PINLDR);
    
    if (leitura < minVal) { //verificando se a leitua é menor que o valor atual
      minVal = leitura; 
    }

    if (leitura > maxVal) { //Verificando se a leitura é maior que o valor atual
      maxVal = leitura;
    }

    delay(100);
  }
} 
```
Para mais informações de como o código funciona, <a href="/color_detector.ino">Acesse o código aqui</a>

# :page_facing_up: Licença
Licença MIT. Para mais informações sobre a licença, <a href="/LICENSE">Clique aqui</a>

<img src="https://github.com/umroboporaluno/.github/blob/main/profile/ura-logo.png" alt="URA Logo" width="100" align="right" />
