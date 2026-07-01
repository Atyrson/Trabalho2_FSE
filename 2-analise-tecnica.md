## 2 ANÁLISE TÉCNICA

Esta parte apresenta a Análise Técnica do projeto, estruturada para detalhar
a engenharia interna de um assistente de voz sob duas perspectivas: a arquitetura
comercial de referência do Amazon Echo. Ao longo desta seção, serão examinados
os quatro pilares fundamentais do sistema: o Módulo de Áudio e Captura, que
abrange desde a digitalização do som via barramento I2S até algoritmos de
cancelamento de eco (AEC) e detecção de voz (VAD); o Módulo de Controle e
Nuvem, que explica o fluxo híbrido de processamento de comandos através do
Alexa Voice Service (AVS) e o uso de skills; os Protocolos e Redes Críticas,
detalhando as estratégias de comunicação de baixa latência como conexões
multiplexadas, WebSockets e redes IoT (MQTT e Matter); e, por fim, a gestão de
Sistemas Operacionais e Energia, que aborda o isolamento de processos e a
eficiência no consumo energético durante o monitoramento passivo e ativo do
dispositivo.

### 2.1 Módulo de Áudio e Captura

Para viabilizar a experiência do assistente de voz, o módulo de captura
precisa processar sinais acústicos de forma robusta em ambientes ruidosos. Na
arquitetura comercial de referência (Amazon Echo), o pipeline original utiliza um
arranjo de 7 microfones e o processador DM3725CUS100 para realizar
processamento avançado de sinal. No entanto, para a proposta de reprodução
simplificada e viável em uma ESP32, o pipeline foca em técnicas de filtragem digital
integradas e barramento simplificado, dividindo-se nas três etapas críticas descritas
a seguir.

#### 2.1.1 Processamento de Sinal (Analógico para Digital)

As ondas sonoras (pressão mecânica) são captadas pelas membranas dos
microfones e convertidas em sinais elétricos. Na proposta do grupo, o uso do
microfone INMP441 elimina a necessidade de um circuito externo de amostragem e
quantização analógico-digital (ADC), uma vez que o próprio circuito integrado do
componente realiza a amostragem nativa e entrega o áudio diretamente quantizado
em PCM de 24 bits a 16 kHz através do barramento digital I2S, transmitindo o sinal
sem degradação para a ESP32.

#### 2.1.2 Cancelamento de Eco Acústico (AEC - Acoustic Echo Cancellation)

Para que o sistema seja capaz de ouvir o usuário mesmo enquanto estiver
reproduzindo uma resposta ou som pelo alto-falante principal (saída via
MAX98357A), adota-se o algoritmo de AEC. O firmware monitora o sinal digital de
áudio enviado para o amplificador (sinal de referência) e, por meio de filtragem
adaptativa baseada em mínimos quadrados médios (LMS - _Least Mean Squares_ ),
calcula as reflexões sonoras estimadas do ambiente e as subtrai digitalmente do
fluxo captado pelo microfone, isolando a voz do usuário.

#### 2.1.3 Detecção de Atividade de Voz (VAD)

##### 2.1 Módulo de Áudio e Captura

Para viabilizar a experiência do assistente de voz, o módulo de captura precisa
processar sinais acústicos de forma robusta em ambientes ruidosos. Na arquitetura
comercial de referência (Amazon Echo), o pipeline original utiliza um arranjo de 7
microfones e o processador DM3725CUS100 para realizar processamento avançado
de sinal. No entanto, para a proposta de reprodução simplificada e viável em uma
**ESP32** , o pipeline foca em técnicas de filtragem digital integradas e barramento
simplificado, dividindo-se nas três etapas críticas descritas a seguir.

##### 2.1.1 Processamento de Sinal (Analógico para Digital)

As ondas sonoras (pressão mecânica) são captadas pelas membranas dos
microfones e convertidas em sinais elétricos. Na proposta do grupo, o uso do
microfone **INMP441** elimina a necessidade de um circuito externo de amostragem e
quantização analógico-digital (ADC), uma vez que o próprio circuito integrado do
componente realiza a amostragem nativa e entrega o áudio diretamente quantizado
em PCM de 24 bits a 16 kHz através do barramento digital **I2S** , transmitindo o sinal
sem degradação para a ESP32.

##### 2.1.2 Cancelamento de Eco Acústico (AEC - Acoustic Echo Cancellation)

Para que o sistema seja capaz de ouvir o usuário mesmo enquanto estiver
reproduzindo uma resposta ou som pelo alto-falante principal (saída via
MAX98357A), adota-se o algoritmo de AEC. O firmware monitora o sinal digital de
áudio enviado para o amplificador (sinal de referência) e, por meio de filtragem
adaptativa baseada em mínimos quadrados médios (LMS - _Least Mean Squares_ ),
calcula as reflexões sonoras estimadas do ambiente e as subtrai digitalmente do
fluxo captado pelo microfone, isolando a voz do usuário.

##### 2.1.3 Detecção de Atividade de Voz (VAD) e Filtragem de Ruído

Em substituição a arranjos complexos de múltiplos microfones em hardware
( _beamforming_ ) que exigiriam poder computacional alto e processadores dedicados,
a proposta adota um único microfone omnidirecional operando sob um algoritmo de
**Detecção de Atividade de Voz (VAD)** em software, utilizando como base a
biblioteca integrada do framework **ESP-Skainet**. O firmware analisa continuamente a
energia do sinal e a Taxa de Cruzamento por Zero ( _Zero-Crossing Rate_ ) restrita às
frequências típicas da fala humana (entre 300 Hz e 3400 Hz). Isso permite que a
ESP32 ignore ruídos estáticos de fundo (como coolers ou vento) e ative a pilha de
rede e interpretação apenas quando uma assinatura vocal válida for detectada,
otimizando drasticamente o uso de CPU e memória RAM

### 2.2 Módulo de Controle e Nuvem

Como o Amazon Echo e o ecossistema Alexa utilizam tecnologias
proprietárias e de código fechado, informações detalhadas sobre sua implementação
interna não são disponibilizadas integralmente pela Amazon. Por esse motivo, a
análise apresentada neste trabalho foi construída a partir da combinação de
múltiplas fontes públicas complementares. Foram utilizadas documentações oficiais
e FAQs disponibilizadas pela Amazon para compreender funcionalidades e
comportamento esperado do sistema, artigos científicos sobre segurança e
privacidade para analisar arquitetura e fluxo de dados observáveis, estudos de
engenharia reversa para investigar componentes internos e comunicação entre
dispositivo e nuvem, além da documentação pública do Alexa Skills Kit (ASK) e do
Alexa Voice Service (AVS), utilizada para compreender como comandos são
processados e encaminhados para serviços externos. Dessa forma, as conclusões
apresentadas buscam representar o funcionamento do sistema com base em
evidências públicas e observáveis, sem assumir acesso ao código-fonte ou à
implementação interna da plataforma.
A arquitetura do Amazon Echo (1ª geração) segue um modelo híbrido entre
processamento embarcado e serviços em nuvem. O dispositivo realiza localmente
funções relacionadas à captura e codificação de áudio, controle de estados do
dispositivo e reconhecimento inicial da palavra de ativação por meio do daemon
ASRD, que utiliza o mecanismo Pryon para identificar a wake word configurada. Já
recursos de reconhecimento de fala, compreensão de linguagem natural e síntese
de voz são disponibilizados pelo Alexa Voice Service (AVS), serviço executado na
infraestrutura em nuvem da Amazon.
A comunicação entre o Echo e o AVS ocorre por meio de uma conexão
persistente mantida entre o dispositivo e um endpoint dedicado da Amazon
utilizando o protocolo SPDY. Durante o estabelecimento da conexão, o dispositivo
realiza autenticação com credenciais obtidas no processo de pareamento e envia
informações necessárias para identificação e atualização do estado de seus
subsistemas. Após autenticado, o Echo mantém um canal contínuo para envio de
eventos, transmissão de áudio e recebimento de respostas, caracterizando uma
arquitetura cliente-servidor baseada em processamento distribuído.


Fonte: artigo https://arxiv.org/pdf/2102.
O mecanismo de _wake word_ funciona como a etapa inicial de interação entre
o usuário e o Amazon Echo. Em estado normal de operação, o dispositivo
permanece escutando continuamente o ambiente até identificar a palavra de
ativação configurada, como “Alexa”. Essa palavra atua como um gatilho para iniciar
o processamento do comando de voz, evitando que todo o áudio ambiente seja
enviado continuamente para os servidores. Quando a ativação ocorre, o Echo passa
a registrar o comando do usuário e inicia o envio do áudio ao serviço em nuvem
responsável pelo processamento da solicitação.
Após a ativação, o fluxo de processamento é transferido para o Alexa Voice
Service (AVS), que executa funções de reconhecimento automático de fala (ASR),
compreensão de linguagem natural (NLU) e geração da resposta por síntese de voz
(TTS). O resultado desse processamento pode ser tratado diretamente pelos
serviços internos da Alexa ou encaminhado para uma _skill_ , que funciona como uma
aplicação orientada por voz. Nesse caso, o comando interpretado é convertido em
uma requisição estruturada enviada para serviços na nuvem, como funções AWS
Lambda, responsáveis por processar a intenção do usuário e retornar uma resposta
que será transformada em áudio e reproduzida pelo dispositivo.
Após a detecção local da palavra de ativação, o Echo inicia a transmissão do
fluxo de áudio para o Alexa Voice Service. Antes do processamento do comando, os
servidores executam uma etapa adicional de verificação da ativação recebida.
Confirmada a ativação, o AVS (Automatic Speech Recognition) realiza
reconhecimento automático de fala, convertendo o áudio enviado pelo dispositivo em
uma representação textual do comando do usuário.
Com o texto obtido, o Alexa Voice Service aplica mecanismos de
compreensão de linguagem natural, ou Natural Languange Understanding (NLU)
para interpretar a solicitação recebida e determinar uma resposta apropriada. Em
seguida, o serviço utiliza síntese de voz TTS (Text-to-Speech) para gerar a resposta
em formato de áudio, que é enviada ao dispositivo para reprodução. Após concluir a
reprodução, o Echo informa o término da execução ao serviço em nuvem,
encerrando o ciclo de interação entre dispositivo embarcado e infraestrutura remota.


As _skills_ do Amazon Alexa funcionam como aplicações externas que ampliam
as funcionalidades disponíveis no Amazon Echo. O Echo atua principalmente como
dispositivo de entrada e saída: ele captura o comando de voz do usuário por meio
dos microfones, identifica a ativação pelo assistente e envia o áudio para a
infraestrutura em nuvem da Alexa. As skills permitem que essa infraestrutura
execute tarefas adicionais além das funções nativas do sistema, como consultar
informações, controlar serviços externos ou interagir com aplicações específicas.
Dessa forma, o Echo não executa diretamente a lógica dessas aplicações, mas atua
como intermediário entre o usuário e os serviços remotos.
O processamento da skill ocorre fora do dispositivo, na infraestrutura de
nuvem. Após receber o áudio enviado pelo Echo, o Alexa Voice Service (AVS)
realiza reconhecimento de fala e interpretação do comando com base no modelo de
interação definido pela skill. Uma vez identificada a solicitação do usuário, o AVS
encaminha uma requisição para a lógica da aplicação, executada em um serviço de
back-end hospedado pela Alexa, AWS ou servidores externos. Esse serviço
processa a solicitação e retorna uma resposta para o AVS, que converte o resultado
em áudio e o envia de volta ao Echo para reprodução ao usuário.

```
fonte: https://developer.amazon.com/pt-BR/alexa/alexa-skills-kit
```

### 2.3 Protocolos e Redes Críticas

#### 2.3.1 O Desafio da Baixa Latência

A interação por voz exige respostas em milissegundos para manter a
naturalidade do diálogo. Para viabilizar essa comunicação proativa, o software
embarcado gerencia simultaneamente dois fluxos de dados distintos: o streaming de
áudio bruto (que demanda alta largura de banda e entrega sequencial) e os
comandos de controle e eventos (pacotes estruturados leves, mas que exigem
entrega imediata e alta confiabilidade). A coordenação desses fluxos em redes
residenciais requer protocolos otimizados para tempo real que minimizem o
_overhead_ de cabeçalhos.

#### 2.3.2 Infraestrutura HTTP/2 e SPDY


Conforme evidenciado nos estudos de engenharia reversa de Schönherr et al.
(2020), o ecossistema Amazon Echo mitiga a latência estabelecendo uma **conexão
TCP/TLS persistente** entre o hardware e o _Alexa Voice Service_ (AVS). Centralizada
pelo AlexaDaemon, a arquitetura utiliza streams multiplexados para manter canais
lógicos independentes sobre um único canal físico:

```
● Upchannel (Subida): Transmite o fluxo de voz codificado em tempo real logo
após a validação da palavra de ativação.
● Downchannel (Descida): Permanece aberto indefinidamente para que a
nuvem envie diretivas assíncronas, como áudio sintetizado e atualizações de
estado.
```
Essa abordagem elimina o tempo de estabelecimento de novas conexões e otimiza
a banda através da compactação de cabeçalhos.

#### 2.3.3 WebSockets como Canal Bidirecional Persistente

O protocolo **WebSockets** atua como uma solução de ultra-baixa latência,
convertendo uma requisição HTTP inicial em um canal _full-duplex_ permanente
diretamente sobre a camada TCP. Essa arquitetura elimina o custo de técnicas como
o _polling_ contínuo, permitindo que o nó de borda e o servidor troquem _frames_ de
dados leves de forma inteiramente assíncrona. Em assistentes virtuais, o modelo é
ideal para o tráfego de metadados, telemetria e sincronização de interfaces visuais
periféricas em tempo real.

#### 2.3.4 Protocolo MQTT e Controle em Redes Mesh

Para o controle de periféricos residenciais de baixa largura de banda
(lâmpadas, sensores e relés), utiliza-se o **MQTT** , um protocolo baseado no
paradigma de publicação/assinatura ( _publish-subscribe_ ) mediado por um _broker_. O
MQTT minimiza o consumo de energia e banda, trafegando pacotes de poucos
bytes.
No ecossistema de IoT, essa comunicação é integrada a coprocessadores
que rodam redes mesh ( **Zigbee** ou **Thread** ) sob a camada unificada do padrão
**Matter**. Essa organização de software permite que o assistente virtual funcione
como um nó coordenador local, garantindo a execução das automações e a
resiliência do sistema mesmo em cenários de interrupção da internet principal.

### 2.4 Sistemas operacionais e Energia

O Amazon Echo de primeira geração opera com um sistema operacional
baseado em kernel Linux (Fire OS), gerenciando os recursos do processador ARM
Cortex-A8 de 1GHz. Para conciliar a escuta contínua com a eficiência energética, o
sistema adota uma estratégia de isolamento de processos.
Aproximadamente 50% do tempo de CPU é dedicado exclusivamente ao

_daemon_ local ASRD, que roda o motor de reconhecimento _Pryon_ para detectar a
palavra de ativação. Enquanto o dispositivo não atinge o limiar de aceitação (score
superior a 0.57), os rádios de transmissão Wi-Fi e os processos de codificação de
áudio ( _AudioEncoderDaemon_ ) permanecem inativos ou em estado de baixo
consumo. Apenas quando a transição para o estado SendingDataToAlexa ocorre, o
sistema acorda os módulos de rede para transmitir o _buffer_ de áudio. Essa
arquitetura garante que o consumo de energia e a banda de rede não sejam
desperdiçados durante o monitoramento passivo.