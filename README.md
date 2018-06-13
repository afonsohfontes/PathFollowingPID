# PathFollowingPID

TEXTO ESCRITO POR: @JorgeRenan

# 1. INTRODUÇÃO

Segundo SANTOS (2015), o ser humano é atraído em desenvolver cada vez mais máquinas tecnologicamente complexas, onde possuem a capacidade de substituir as pessoas que são expostas a situações de risco de vida, com insalubridade ou repetitivas. 

Para SANTOS (2015), essas máquinas tecnológicas, ou robôs, tem seu funcionamento dependente dos comandos de controle para que o seu desempenho seja eficaz na execução das atividades, cujas podem ser autônomas ou não. O tipo de atividade em que o robô será submetido é umas das principais informações necessárias para saber qual o tipo de controle poderá ser desenvolvido e como o mesmo irá atuar.

De acordo com ROMERO (2014), as máquinas ou robôs que antes eram apenas vistos através da ficção cientifica passaram ser inseridas em nosso cotidiano. Alguns deles, como um robô aspirador de pó do chão, possuem maior quantidade e precisão de informações obtidas no meio em que reside permitindo assim o desenvolvimento de sistemas de controles para controle de trajetória, para tomada de decisão e processamento de imagens e entre outros. 

# 1.1. Objetivo Geral

O objetivo deste trabalho é desenvolver um sistema de controladores que possam conduzir um robô móvel em uma trajetória planejada baseando-se na utilização de sensores.

# 1.2. Objetivos Específicos
•	Desenvolver um controlador para trajetória reta;
•	Desenvolver um controlador para curvas;
•	Realizar testes nos controladores;
•	Demonstrar a eficiência dos controladores em diferentes trajetórias.

# 2. METODOLOGIA

Nesse tópico do trabalho tem como finalidade apresentar a metodologia utilizada para a implementação dos controladores de trajetória, bem como a estrutura do robô móvel utilizado e o desenvolvimento do algoritmo. Então para que seja possível o desenvolvimento deste projeto é necessário possuir alguns conhecimentos sobre eletrônica, sistema de controle e de robótica móvel juntamente com seus componentes, como microcontroladores, sensores, motores e programação.

Primeiramente, será feita a descrição do conceito do projeto e a construção de um protótipo físico do robô, definindo assim os materiais e como foram projetados. Na segunda parte apresenta-se o hardware eletrônico, o método de sensoriamento, o sistema eletrônico de controle e a atuação dos motores microcontrolados e por final mostrará o algoritmo implementado dos controladores PID em função dos sensores utilizados.

# 2.1. Conceito do Projeto

Este trabalho tem como fundamento apresentar uma aplicação de um robô móvel, com pequenos recursos sensoriais e cuja função principal é de cumprir um percurso pré-definido na programação, utilizando-se das informações obtidas dos sensores como referência de locomoção com base na implementação dos controladores PID. O robô móvel utilizado possui um sensor de localização na parte central que o ajuda a manter na posição definida do percurso, e um sensor eletromecânico para determinar a distância percorrida.

As informações dos sensores são recebidas pelo microcontrolador embarcado no robô e são processadas pelo algoritmo do controlador que está inserido na programação do microcontrolador. Após o processamento dos dados, é enviado para o drive dos motores os valores do PWM (Pulse Width Modulation) e o sentido de giro dos motores para que possam manter o robô na posição e orientação inicial.

# 2.2. Construção do Robô Móvel

O robô móvel, conforme ilustrado na Figura 1, tem sua estrutura mecânica feita em acrílico e pode ser dividida em duas partes, de potência e de controle. A primeira parte é composta pelo par de motores DC acoplados as respectivas rodas. Na segunda parte aloca-se uma placa de desenvolvimento Arduino, os sensores e os módulos e a alimentação externa do sistema.

Figura 1.
![carro3](https://user-images.githubusercontent.com/32027941/41351676-7f51d5fe-6eed-11e8-9f68-15abb8b502d8.PNG)

O Arduino UNO, conforme ilustrado na Figura 2, é uma placa microcontrolada com o ATmega328. Esta placa possui 14 pinos digitais de entrada e saída (dos quais 6 são pinos de saídas PWM), 6 pinos de entrada analógicas, 1 porta de comunicação serial UART, um cristal oscilador de 16MHz, uma conexão USB, um conector de alimentação, um cabeçalho ICSP, e um botão de reset.

Figura 2.
![arduinouno2](https://user-images.githubusercontent.com/32027941/41351692-91366b7c-6eed-11e8-8311-edf70632a4ff.PNG)

Os motores são de tração diferencial e são alimentados juntamente com o sistema eletrônico por uma fonte regulável DC que fornece 12V/2A. O acionamento tem como função controlar a velocidade e a tração diferencial dos atuadores. Os motores são controlados separadamente por sinais PWM, que controlam a velocidade de rotação e para qual sentido girar o motor, ambos gerados pelo microcontrolador com o auxilio de uma ponte-H modelo L298N para controle de dois motores de até 2A, como ilustra a Figura 3.

Figura 3.
![l298n](https://user-images.githubusercontent.com/32027941/41351719-a4b3e36e-6eed-11e8-8474-80dc5f15e227.PNG)

A ponte-H L298N é responsável por fazer a mudança de sentido da rotação dos motores, e ela tem seu acionamento com sinais digitais vindas do microcontrolador onde fecham determinadas chaves fazendo o motor girar no sentido desejável.

# 2.3. MPU6050

A localização de um objeto pode ser obtida por meio de GPS, estimada matematicamente ou obtida por um módulo. As informações adquiridas pelo GPS são através de uma rede de satélites, ou seja, a precisão de localização por meio deste método se torna mais viável para ambientes externos. Entretanto, o trabalho aqui apresentado necessita de precisão de localização para ambientes externos e internos. Isso quer dizer que a localização do robô será matematicamente obtida por meio de um módulo.

O MPU6050, apresentado na Figura 4, é um módulo que possui um acelerômetro e um giroscópio em uma mesma placa eletrônica. Respectivamente, estes sensores servem para medir aceleração ou medir vibrações e para medir rotações ou inclinações. Este módulo fornece a leitura de seis eixos, ou seja, três valores do acelerômetro e três do giroscópio que são correspondentes aos eixos X, Y e Z. O MPU6050 utiliza o protocolo i2C, que será comentado no próximo tópico, para comunicar-se com um microcontrolador.

Figura 4.
![mpu6050](https://user-images.githubusercontent.com/32027941/41351745-b59a4326-6eed-11e8-9b70-8f42949850f0.PNG)

Conforme visto na Figura 4, os pinos VCC e GND tem a função de energizar o circuito da placa, já os pinos SCL e SDA são utilizados para a comunicação do protocolo i2C. Os pinos XDA e XCL são habilitados quando é necessário o uso de um outro módulo, como por exemplo o sensor magnético, para que possam criar um sistema de orientação completo. Os pinos AD0 e INT são, respectivamente, os pinos de endereçamento e interrupção. O pino de endereçamento como o próprio nome diz, tem a função de determinar em qual endereço o módulo se encontra, quando este estiver em uma rede de comunicação. O pino de interrupção é utilizado do mesmo modo como o pino anterior, porém este gera uma interrupção toda vez que tiver aquisição dos dados do módulo. Neste trabalho foram utilizados os pinos VCC, GND, SCL e SDA, pois se trata de uma comunicação simples que não houve a necessidade de auxilio de outro módulo e da capacidade de interrupção do módulo.

# 2.3.1. Protocolo de comunicação i2C

De acordo com THOMSEN (2014), o funcionamento deste protocolo é baseado na relação mestre/escravo, ou seja, um dos dispositivos será o mestre e o outro será o escravo. A função de mestre consiste em manter a coordenação de toda comunicação, já que ele possui a capacidade de enviar e requisitar informações aos escravos existentes. 

Os pinos restantes do MPU6050 poderiam ser usados se tivéssemos uma rede de comunicação entre dispositivos, pois com eles podemos determinar quem é o mestre e o escravo consequentemente sabendo onde se encontra cada módulo por meio do endereçamento. A rede de comunicação i2C é exemplificada na Figura 5.

Figura 5.
![redei2c](https://user-images.githubusercontent.com/32027941/41351774-cf152726-6eed-11e8-95e6-c385898475f0.PNG)

Tomando como base a estrutura da rede de comunicação i2C da Figura 5, o MPU6050 é o nosso dispositivo escravo e a placa Arduino UNO é o mestre da rede. Isso quer dizer que o Arduino UNO requisita o retorno dos ângulos obtidos pelo módulo. As configurações necessárias para a comunicação entre o Arduino UNO e o MPU6050, foram retiradas de um código que está disponível no site de desenvolvimento GITHUB com o nome AutonomousSystems.

# 2.4. Encoders

São sensores eletromecânicos capazes de auxiliar na determinação do deslocamento, da posição e do sentido de giro de um objeto, através da geração de uma onda quadrada de pulsos elétricos de acordo com o tipo de codificação utilizado. No presente projeto este dispositivo foi utilizado para fornecer a distância percorrida pelo robô na trajetória. O desempenho do encoder pode diminui por causa da interferência de alguns fatores do ambiente externo, como o atrito e rampas. 

O encoder utilizado foi retirado do scroll de um mouse, ilustrado na Figura 6, pelo fato dele ser um codificador rotativo e de fácil encaixe no eixo do motor utilizado.

Figura 6.
![encodermouse](https://user-images.githubusercontent.com/32027941/41351843-01fcc4a0-6eee-11e8-9f64-b3caf84b868c.PNG)
![encoder](https://user-images.githubusercontent.com/32027941/41352025-98400706-6eee-11e8-85e5-d4087334da8f.PNG)

Quando o motor é acionado ele ativa o encoder que gera uma onda quadrada consequentemente gerando interrupções no Arduino. Porém, antes das informações enviadas pelo sensor chegarem no Arduino UNO, as mesmas são filtradas por um circuito eletrônico pelo fato da onda quadrada das interrupções apresentar um sinal menor do que o microcontrolador consegue processar. Portanto, o circuito eletrônico mostrado na Figura 7 serve para intensificar o sinal da onda quadrada vinda do encoder e que pode variar de acordo com o tipo de sensor utilizado.

Figura 7.
![circuitoencoder](https://user-images.githubusercontent.com/32027941/41351868-1418d246-6eee-11e8-853b-118301ea3b0b.PNG)

O algoritmo de implementação de leitura das interrupções no Arduino foi retirado de um blog desenvolvido por MERRETT (2016), onde se faz o uso de variáveis para indicar o incremento e decremento da contagem de pulsos gerado pelo encoder.

# 2.5.	A Trajetória

O algoritmo tem como base uma trajetória pré-definida que é inserida no microcontrolador e que recebe o retorno das informações dos sensores para que possa cumprir corretamente o percurso programado. Na Figura 8 pode ser observado o fluxograma lógico considerando que a trajetória já tenha sido definida em sua programação e que essa seja um percurso reto em superfície plana. Após ser inicializado, o sistema do robô faz a definição do ângulo referencial retornado do sensor de localização e em seguida verifica a sua distância atual em relação a distância referenciada para então ativar os motores de forma a andar reto, caso o mesmo se encontre no ângulo referencial, caso não esteja dentro dos parâmetros, o microcontrolador embarcado no robô irá gerar sinais PWM a partir dos cálculos dos controladores PID com o objetivo de corrigir a sua trajetória. Se a contagem de pulsos do encoder, ou seja, a distância atual ficar igualada com a distância referenciada o próprio controlador fará os motores pararem.

Figura 8.
![fluxogramareto](https://user-images.githubusercontent.com/32027941/41351885-2725b426-6eee-11e8-924e-b2eedb1355b8.PNG)

Como nem toda trajetória será sempre uma reta, logo foi implementado um controlador para que o robô pudesse fazer curvas de 90° para esquerda ou para direita dependendo da definição do percurso. Quando for necessário fazer uma curva o controlador receberá como referência o ângulo que determina para qual dos lados o robô deve girar, então caso o mesmo queira fazer uma curva para a direita o ângulo indicado será de 90º e quando o ângulo for de -90º o robô fará uma curva para a esquerda. Após o controlador saber para qual sentido terá que girar o robô, ele aciona os motores até que o módulo MPU6050 retorne o ângulo de 90º ou -90 e fazendo com que o próprio controlador faça a parada. Então na Figura 9, pode-se observar o fluxograma lógico do controlador de curvas.

Figura 9.
![fluxogramacurva](https://user-images.githubusercontent.com/32027941/41351903-349aea36-6eee-11e8-9515-9c29f74ea28a.PNG)

# 2.5.1. Cálculo de conversão da distância

Esse desenvolvimento matemático trata-se de uma simples conversão da distância real definida, ou seja, aquela em que se deseja que o robô percorra, na quantidade de pulsos que o encoder precisa gerar para alcançar o final do percurso. Para melhor entendimento, o desenvolvimento foi dividido em:

Passo 1: Calcular o comprimento da circunferência do pneu utilizado pelo robô:

                                           C=Ꝋ* π                                    (1)

Na formula acima, C é o comprimento da circunferência, Ꝋ é o diâmetro do pneu e π é o número pi que tem o valor aproximado de 3,1416. Neste trabalho utilizou-se um pneu com diâmetro de 68 mm, logo o comprimento da circunferência será de 213,629 mm.  

Passo 2: Calcular a resolução do encoder. Para esse procedimento, se faz necessário a utilização do Arduino UNO juntamente com o encoder, pois percorrerá a distância do comprimento do pneu e acompanhará quantos pulsos o encoder gera até atingir a tal distância dada. Após isso poderá aplicar na formula a seguir:

                                            D=C/N                                                  (2)

Onde D é a distância percorrida em milimetro (mm) pelo robô a cada um pulso do encoder, C é o mesmo comprimento calculado no passo anterior e N é a quantidade de pulsos que o encoder gerou até chegar ao final da distância definida. No presente trabalho utilizou o comprimento da roda para a contagem dos pulsos que teve como resultado 12 pulsos gerados pelo encoder. Então a resolução do encoder aqui utilizado é de 17,8 mm por pulso.

Passo 3: Calcular a conversão da distância real em quantidade de pulsos necessária para que seja possível atingir a distância desejada. Para isso acontecer se pede que forneça a distância real em milimetro (mm) devido todos os passos anteriores utilizarem essa unidade métrica, aplicando assim a seguinte formula:
 
                                           P=  d/D                                         (3)

Em que d é a distância em milimetro (mm) fornecida pelo usuário e P é a quantidade de pulsos que equivale a distância real. Portanto para o nosso trabalho qualquer conversão de distância real será divida pelo valor de 17,8 mm por pulso para que assim se tenha a quantidade de pulsos necessária para atingir a distância real.

# 2.5.2. O Controlador de Retas

O objetivo deste controlador é de fornecer parâmetros de controle para o acionamento dos motores e assim possam reduzir gradativamente a zero os erros de posição e orientação do robô. A função que aciona os motores transforma os parâmetros recebido pelo controlador em sinais de PWM que serão enviados para cada motor. O sinal de controle é regido pelo o uso das seguintes equações:

                        x=constante-output                                       (4)
                        y=constante+output                                       (5)

Onde x e y são as velocidades que os motores da direita e esquerda irão receber, respectivamente, constante é a saída do controlador do encoder e output é a saída do controlador do módulo MPU6050. 
 
# 2.5.2.1. Algoritmo de Controle Reto

Nesta seção será descrito os procedimentos do algoritmo de controle no qual tem como objetivo fazer com que o robô alcance a distância fornecida e não permitir com que o robô oscile em sua orientação durante a execução do percurso definido. Este algoritmo é implementado no microcontrolador embarcado do robô. Na Figura 10 pode ser observado a programação do controlador de retas.

Primeiramente antes de começar o controle é necessário fornecer qual a distância definida e esperar o módulo por 400 milissegundos (ms) para que o mesmo informe um ângulo adequado para o referencial. Na linha 1, o ângulo referencial é definido para que o robô tenha uma orientação e na linha seguinte calcula-se o erro entre a distância definida e o contador de pulsos. A partir desses dois parâmetros o controlador é inicializado e consequentemente movimenta o robô.

Na linha 3 é atualizada a variável do tempo de ciclo passado. A linha 4 é onde ocorre a medida do ciclo de tempo do controlador. Em seguida na linha 5 ocorre a diferença entre os tempos cujo resultado será utilizado nos cálculos dos controladores PID que tiveram como base as equações de sistemas de controle. Nas linhas 6, 7 e 8 ocorre os cálculos dos parâmetros do controlador PID do encoder a partir do erro encontrado na linha 2. Nas linhas 9 a 14 é feito um filtro limitante para que o valor do parâmetro integral não seja muito grande nem muito pequeno. Na linha 15 ocorre a soma dos parâmetros do controlador PID do encoder e para que esse valor não extrapole para mais ou para menos usa-se novamente um filtro limitante a partir da linha 16 até a linha 21. Na linha 22 atualiza-se a variável do erro passado do encoder.

A linha 23 o erro anterior do MPU6050 é atualizado e na linha 24 é calculado o erro entre o ângulo referencial e o ângulo lido constantemente pelo módulo. Nas linhas 25, 26 e 27 é feito o cálculo dos parâmetros do controlador PID do MPU6050. Na linha 28 até a 33 é utilizado mais uma vez um filtro limitante para o valor do parâmetro integral. A partir da linha 34 até a linha 40 é feito os mesmos procedimentos que foram realizados nas linhas 15 a 21.

Nas linhas 41 e 42 ocorre a junção entre as saídas dos dois controladores PID existentes por meio das equações da lei de controle. Após a realização dos cálculos os resultados passam por um último filtro limitante nas linhas 43 a 50 para que não ultrapassem os valores permitidos pelo PWM do microcontrolador. Na linha 51, os resultados das equações já filtrados são usados como parâmetros na função de movimento.
```
1 - setPointR = deg[0];
2 - erroReto = distancia - encoderPos;
3 - last_time = now;
4 - now = millis();
5 - dt = now - last_time;
6 - pdist = kpdist * errodist;
7 - ddist = (kddist*(errodist - errodistAnt)) / dt;
8 - idist += kidist * errodist * dt;
9 - if(idist > 100) { 
10-   idist = 100;
11- }
12- if(idist < -100) {
13-   idist = -100;
14- }
15- int constante = (int)(pdist  + idist + ddist);
16- if(constante > 255) {
17-   constante = 255;
18- }
19- if(constante < -255) {
20-   constante = -255;
21- }
22- errodistAnt = errodist;
23- errorAnt = error;
24- error = setPointR - anguloAtual;
25- p = kp*error;
26- d = (kd*(error - errorAnt)) / dt;
27- i += ki * error * dt;
28- if(i > 100) { 
29-   i = 100;
30- }
31- if(i<-100) {
32-   i = -100;
33- }
34- float output = (p) + (d) + (i);
35- if(output > 255) {
36-   output = 255;
37- }
38- if(output < -255) {
39-   output = -255;
40- }
41- y = constante + output;
42- x = constante - output;
43- if(x < 0 || y < 0) {
44-    if(x < 0) x = 0;
45-    if(y < 0) y = 0;
46- }
47- if(x > 255 || y > 255) {
48-    if(x > 255) x = 255;
49-    if(y > 255) y = 255;
50- }
51- Mov(x,y);
```
# 2.5.3. O Controlador de Curvas

O objetivo deste controlador é garantir com que o robô mude de direção para a esquerda ou direita a partir da indicação do ângulo. Existem duas funções que acionam os motores, pois cada uma tem um sentido especifico a cumprir. As equações utilizadas no sinal de controle são as seguintes:

                                     y=pidc                                         (6)
                                     x= -pidc                                       (7)

Onde x e y ainda continuam sendo as velocidades que os motores irão receber, porém serão definidas pela saída do controlador PID, pidc. Neste controlador usa-se apenas os dados do módulo de localização. Logo, torna o algoritmo dele simples de ser implementado. Na Figura 11 é ilustrado a programação do controlador de curvas.

Primeiramente é necessário o fornecimento do ângulo da curva que pode ser 90º ou -90º que respectivamente sinalizam curva para direita ou esquerda. Na linha 1, é assumido o valor do ângulo indicado como referencial. Em seguida na linha 2 calcula-se o erro pela diferença entre o referencial e o ângulo atual lido pelo módulo. Na linha 3 é feita uma condição de entrada para o tratamento do erro, que só aceitável se o módulo do erro for maior que cinco (valor de erro escolhido). Na linha 4, após a condição do erro ser verdadeira, se tem o início do cálculo dos parâmetros do controlador PID que se encerra na linha 6.

Como trata-se de um controlador proporcional integral derivativo é preciso ter um filtro limitante para o valor do parâmetro integral, portanto nas linhas 7 a 12 é feito este filtro. Na linha 13 é realizado a atualização do erro anterior. A linha 14 recebe a soma dos parâmetros do controlador PID que posteriormente serão os valores das velocidades dos motores indicados nas linhas 15 e 16. Nas linhas 17 a 20 é utilizado um último filtro para que as velocidades não ultrapassem o valor máximo determinado pelo microcontrolador.

Nas linhas 21 e 22 foram implementadas condições em cada uma das linhas, sendo que uma condição autoriza o robô a girar para a direita e a outra para a esquerda, assumindo as velocidades calculadas como parâmetros de movimento. Nas linhas 23, 24 e 25 são realizados os procedimentos iniciais do controlador, que são fazer uma nova leitura do MPU6050 assumindo assim um ângulo atual e por final calculando o erro. Esses procedimentos se repetem até que o erro seja menor do que cinco fazendo com que a condição da linha 3 seja falsa.

```
1 - setPointC = c;
2 - float erroCurva = setPointC - anguloAtual;
3 - while(abs(erroCurva) > 5) {
4 -    float pc = kpc*erroCurva;
5 -    float dc = (kdc * (erroCurva - erroCurvaAnt))/dt;
6 -    ic += kic*erroCurva*dt;
7 -    if(ic > 50) {
8 -       ic = 50;
9 -    }
10-    if(ic < -50) {
11-       ic = -50;
12-    }
13-    erroCurvaAnt = erroCurva;
14-    float pidc = pc + ic + dc;
15-    y = pidc;
16-    x = -pidc;
17-    if(x < -255) x = -255;
18-    if(y < -255) y = -255;
19-    if(x > 255) x = 255;
20-    if(y > 255) y = 255;
21-    if(x<0) Right (-x,y);
22-    if(y<0) Left (x,-y);
23-    MPU6050_loop();
24-    anguloAtual = deg[0];
25-    erroCurva = setPointC - anguloAtual;
26- }
```
# 2.5.4. Aquisição das Constantes dos Controladores PID

As constantes, ou ganhos, dos controladores PID estão indiretamente relacionados entre si, ou seja, quando um dos valores é modificado esse novo valor influenciará no desempenho dos controladores PID. Então para evitar esse problema, faz-se necessário calibrar os ganhos um por um de cada controlador para consequentemente aumentar o desempenho do controlador. A metodologia de calibração dos ganhos usada é um sequenciamento de passos como é observado a seguir:

I.	Assumir valores para os ganhos proporcionais dos três controladores PID;

II.	Fazer um teste de desempenho e observar se os valores assumidos demostram ação de controle perante o erro. Caso a ação de controle não seja suficiente então se faz necessário a atualização dos valores proporcionais.

III.	Após a calibração do ganho proporcional, insere dois ganhos para a estabilidade caso ocorra oscilação de amortecimento do erro. Os dois ganhos, integral e derivativo, serão calibrados um de cada vez na tentativa e erro, e deixando o valor do ganho proporcional fixado.

# 3. RESULTADOS E DISCUSSÕES

A estrutura de montagem prática do robô contribuiu para a fácil alocação dos componentes durante o desenvolvimento. Os componentes utilizados foram organizados de forma capaz de criar uma simetria para balancear o peso suportado pelo robô. Pode-se perceber que os motores apresentaram folgas devido ao uso constante dos mesmos no período de desenvolvimento e testes dos controladores. O mesmo ocorreu com o encoder, pois ele está diretamente acoplado no eixo do motor. Então após um certo tempo o encoder começou a gerar uma quantidade de pulsos menor do que era prevista para a distância referenciada. 

A implementação do controlador de trajetória reta teve um bom desempenho em percursos em que não exista distúrbios externos, porém quando isso ocorre nota-se que seu rendimento diminui fazendo com que o erro aumente e não alcance o final do percurso. Então foi desenvolvido uma melhoria que utiliza o cosseno do ângulo retornado do MPU6050, portanto contribuindo para que o robô chegasse no final da trajetória com um erro menor. Os dois controladores PID, que compõem o controlador de retas, demostraram ter um desempenho satisfatório atuando em conjunto com as informações obtidas pelos sensores.

O controlador de curvas foi desenvolvido e mostrou ser eficaz no cumprimento do objetivo proposto, porém apresentou um pequeno erro na finalização da curva. Isso quer dizer que o controlador de curvas faz o robô girar para o lado determinado, mas o mesmo não consegue chega completamente no ângulo de 90º.

Os dois controladores de trajetória foram submetidos a três diferentes percursos com e sem distúrbios durante o caminho. Percebeu-se com a análise dos resultados experimentais que o erro ocorre de proporção maior quando há existência de distúrbio. Entretanto o erro apresentado na finalização dos percursos com distúrbio se deve pelo motivo de os controladores já apresentarem um pequeno erro ocasionando assim um somatório dos mesmos. 

Os erros encontrados são por dois motivos, o primeiro foi a limitação da estrutura do robô móvel quanto aos motores e ao encoder. Em segundo foi realizar uma calibração dos parâmetros dos controladores PID ser por tentativa e erro, e não por uma modelagem matemática do sistema. Então se a estrutura do robô móvel fosse mais precisa e a calibração dos parâmetros for por modelagem matemática esses erros seriam menores ao ponto de serem desprezados.

# 4. REFERÊNCIAS

ALMALIKY, Ali. Development of Wireless Monitoring System for Photovoltaic Panels. 2014. Disponível em: <http://researcharchive.wintec.ac.nz/3362/1/Development of Wireless Monitoring System for photovoltaic panels.pdf>. Acesso em: 25 abr. 2018.

CARDOSO, Daniel. Driver motor com Ponte H L298N. 2017. Disponível em: <https://portal.vidadesilicio.com.br/driver-motor-com-ponte-h-l298n/>. Acesso em: 15 maio 2018.

MERRETT, Simon. Improved arduino rotary encoder reading. 2016. Disponível em: <http://www.instructables.com/id/Improved-Arduino-Rotary-Encoder-Reading/>. Acesso em: 01 abr. 2018.

RODRIGUES, Valdinei. I2C - Protocolo de Comunicação. 2017. Disponível em: <http://www.arduinobr.com/arduino/i2c-protocolo-de-comunicacao/>. Acesso em: 15 maio 2018.

ROMERO, Roseli Aparecida Francelin et (orgs.). Robótica Móvel. LTC, 07/2014.

SANTOS, Winderson dos, GORGULHO JR., José Chaves. Robótica Industrial - Fundamentos, Tecnologias, Programação e Simulação. Érica, 06/2015.

THOMSEN, Adilson. Tutorial: Acelerômetro MPU6050 com Arduino. 2014. Disponível em: <https://www.filipeflop.com/blog/tutorial-acelerometro-mpu6050-arduino/>. Acesso em: 26 abr. 2018.
