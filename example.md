###### ESTUDO DE CAIXA DE SOM INTELIGENTE COM ASSISTENTE VIRTUAL

```
Atyrson Souto da Silva - 251005945
Caio Venâncio do Rosário - 231027195
Diogo Rodrigues Barboza - 222006660
Nathan Batista Santos - 222006285
```
```
Brasília, DF
23 de junho de 2026
```

## Sumário

- 1 DESCRIÇÃO DO PRODUTO REFERÊNCIA
   - 1.1 Descrição do produto, Contexto e Mercado
   - 1.2 Descrição do Hardware
   - 1.3 Descrição de software
         - 1.3.1 Restrições de Hardware e a Arquitetura de Escuta Local
         - 1.3.2 Otimização da Camada de Transporte via Wi-Fi e Protocolo SPDY
         - 1.3.4 Gerenciamento de Protocolos Periféricos e Automação Local
   - 1.4 Referências de Fábrica
- 2 ANÁLISE TÉCNICA
   - 2.1 Módulo de Áudio e Captura
      - 2.1.1 Processamento de Sinal (Analógico para Digital)
      - 2.1.2 Cancelamento de Eco Acústico (AEC - Acoustic Echo Cancellation)
      - 2.1.3 Detecção de Atividade de Voz (VAD)
      - Versão Corrigida e Ajustada para o seu README.md
         - 2.1 Módulo de Áudio e Captura
         - 2.1.1 Processamento de Sinal (Analógico para Digital)
         - 2.1.2 Cancelamento de Eco Acústico (AEC - Acoustic Echo Cancellation)
         - 2.1.3 Detecção de Atividade de Voz (VAD) e Filtragem de Ruído
   - 2.2 Módulo de Controle e Nuvem
   - 2.3 Protocolos e Redes Críticas
      - 2.3.1 O Desafio da Baixa Latência
      - 2.3.2 Infraestrutura HTTP/2 e SPDY
      - 2.3.3 WebSockets como Canal Bidirecional Persistente
      - 2.3.4 Protocolo MQTT e Controle em Redes Mesh
   - 2.4 Sistemas operacionais e Energia
- 3 PROPOSTA DE IMPLEMENTAÇÃO
   - 3.1 Firmware e Componentes de Áudio
      - 3.1.1 Gerenciamento do Fluxo de Áudio no Firmware (ESP-IDF)
   - 3.4 Desafios de Software e Integração
- 4 PESQUISA BIBLIOGRÁFICA
   - 4.1 Sobre Aquisição e Processamento de Voz em Microcontroladores
   - 4.2 Sobre WakeWord e Treinamento de palavra Âncora
   - 4.4 Sobre Tecnologia de Processamento de Áudio em Tempo Real com FreeRTOS
- 5 COMPARATIVO
   - 5.1 Comparativo com Produtos Similares
   - 5.4 Análise Crítica do Comparativo
- REFERÊNCIAS BIBLIOGRÁFICAS


## 1 DESCRIÇÃO DO PRODUTO REFERÊNCIA

Como referência para esse projeto, temos um produto bem maduro no
mercado, a Alexa, da Amazon, foi lançado em 2014 junto à linha de dispositivos
Echo, se tornou a principal referência de mercado no segmento de assistentes
virtuais inteligentes baseados em voz. Operando primariamente através de uma
arquitetura em nuvem, o sistema processa comandos de áudio em tempo real para
executar tarefas que vão desde a reprodução de mídias até o gerenciamento
complexo de ecossistemas de automação residencial ( _smart home_ ). No escopo
deste trabalho teórico-exploratório, a Alexa é adotada como o _benchmark_ funcional e
de usabilidade, servindo de modelo estrutural para a concepção do ecossistema
customizado baseado em ESP

### 1.1 Descrição do produto, Contexto e Mercado

O produto é um assistente de voz baseado em IA com o conceito JARVIS,
que toma como base os assistentes residenciais modernos como a linha Amazon
Echo/Alexa, fundindo suas capacidades funcionais com o conceito de inteligência
computacional pervasiva do projeto ficcional JARVIS (Just A Rather Very Intelligent
System), que será implementado como nossa solução customizada em uma
plataforma ESP32. Trata-se de um sistema embarcado cyber físico centralizado,
focado no processamento de linguagem natural (PLN), automação residencial
adaptativa e interação humano-computador por voz de baixa latência.
O mercado de assistentes de voz comerciais consolidou-se a partir de 2014
com o lançamento do Amazon Echo original, evoluindo de simples caixas de som
controladas por comandos rígidos para hubs de automação residencial que integram
LLMs (Large Language Models) locais ou em nuvem, permitindo inferências
complexas. O conceito JARVIS expande essa evolução adicionando uma identidade
mais proativa e integrada ao ambiente do usuário, agora viabilizada em hardware
acessível como o ESP32.
As funções principais do sistema incluem reconhecimento de palavra de
ativação (JARVIS), síntese de voz em tempo real (TTS - Text-to-Speech), controle de
atuadores residenciais como iluminação e relés, e monitoramento ambiental
preventivo com feedback sonoro e visual personalizado.
O público-alvo compreende entusiastas de automação residencial (smart
home), desenvolvedores e o setor de acessibilidade. O dispositivo atua em
ambientes residenciais e de escritório, permitindo que pessoas com limitações
motoras ou visuais controlem seu ambiente físico exclusivamente por comandos de
voz, assim como os dispositivos Echo já proporcionam, mas com a flexibilidade e
customização que nossa implementação JARVIS na ESP32 pode oferecer.
Figura X - Imagem do amazon echo, a primeira caixa de som inteligente (2014)


```
Fonte: Ifixit (2014)
```
### 1.2 Descrição do Hardware

A iFixit, maior comunidade global de reparos, desmontou completamente o
Amazon Echo original para revelar seus componentes internos. O processo gerou
uma documentação fotográfica detalhada e atribuiu uma nota de reparabilidade ao
dispositivo. Ao democratizar essas informações, a plataforma capacita os usuários a
consertarem seus próprios aparelhos com segurança. Isso prolonga a vida útil dos
produtos, combate o lixo eletrônico e reforça a missão da iFixit de promover a
independência do consumidor.
Na base do dispositivo fica a placa de alimentação e áudio, gerenciando a
energia e o som com componentes da Texas Instruments. O regulador TPS
estabiliza a tensão, enquanto o codec TLV320DAC3203 converte o áudio digital em
analógico de forma eficiente. O amplificador TPA3110D2 impulsiona o alto-falante
com até 15W e baixa distorção. Mapear a função de cada um desses chips é
essencial para facilitar o diagnóstico de falhas e a substituição precisa de peças.
A placa-mãe atua como o cérebro do Echo, comandada pelo processador
DM3725CUS100, que gerencia a inteligência do sistema. A estrutura conta com 256
MB de RAM da Samsung, 4 GB de armazenamento flash da SanDisk e um módulo
Wi-Fi/Bluetooth da Qualcomm para conectividade. Tudo é orquestrado por um
gerenciador de energia integrado. Compreender essa arquitetura complexa e como
as placas interagem entre si é o que permite reparos eficientes.

```
Figura X - Imagem do Amazon echo original (2014) desmontado
```

```
Fonte: iFixit (2014).
```
No topo do Echo encontra-se a placa de microfones e LEDs, responsável pela
interface de interação. Ela dispõe de sete microfones em círculo, apoiados por
conversores analógico-digitais que garantem a clareza dos comandos de voz em
360 graus. Drivers programáveis controlam o anel luminoso, enquanto componentes
mantêm os sinais sincronizados. Toda essa estrutura é isolada por espuma acústica,
evidenciando uma engenharia meticulosa que ainda permite reparos pontuais.

### 1.3 Descrição de software

##### 1.3.1 Restrições de Hardware e a Arquitetura de Escuta Local

A compreensão das exigências de software e rede que regem um
ecossistema de _smart speakers_ exige uma análise aprofundada da arquitetura de
processamento dos dispositivos de referência de mercado, com especial destaque
para a linha Amazon Echo. Conforme revelado por estudos de engenharia reversa, a
operação local do dispositivo é severamente limitada por restrições de hardware e
consumo energético. O software do alto-falante de referência executa suas funções
sobre um processador ARM Cortex-A8 de 1GHz, dedicando continuamente cerca de
50% de todo o seu tempo de CPU para o processo de reconhecimento da palavra de
ativação. Essa tarefa de escuta passiva permanente é gerenciada pelo daemon local
denominado ASRD ( _Automatic Speech Recognition Daemon_ ), que utiliza o motor


_Pryon_ , desenvolvido a partir do kit de ferramentas de código aberto _Kaldi_. O modelo
acústico executado localmente é baseado em Redes Neurais Profundas (DNN) e
possui um tamanho inferior a 2 MB. Devido a essa compactação extrema, o
hardware isolado é capaz apenas de realizar a detecção primária de palavras-chave,
tornando a conectividade externa o canal indispensável para transferir o fluxo de
áudio para uma infraestrutura em nuvem capaz de executar tarefas complexas de
processamento de linguagem natural.

##### 1.3.2 Otimização da Camada de Transporte via Wi-Fi e Protocolo SPDY

Para mediar o fluxo de dados distribuído entre o cliente em nível embarcado e
os servidores remotos, o ecossistema utiliza uma interface nativa de rede Wi-Fi de
dupla banda, operando de maneira estratégica nas frequências de 2.4 GHz e 5 GHz.
A faixa de 2.4 GHz é explorada pelo software para transpor barreiras físicas
estruturais e estender o alcance do sinal na residência, enquanto a faixa de 5 GHz
provê maior largura de banda e imunidade contra o congestionamento do espectro.
O gerenciamento lógico e a comunicação direta com a nuvem são centralizados pelo

daemon AlexaDaemon. O grande diferencial dessa camada de software consiste no
emprego do protocolo **SPDY** para gerenciar o canal de transporte sem fio. Ao
implementar a multiplexação de múltiplos fluxos bidirecionais simultâneos sobre uma
única conexão TCP estável, o SPDY elimina o _overhead_ de abertura de conexões
repetidas. Essa escolha arquitetural de software garante que logs de telemetria,
comandos de controle de estado e pacotes de sincronização trafeguem de forma
síncrona e paralela ao streaming de áudio bruto, mitigando atrasos na comunicação
e otimizando de forma crítica a latência global do sistema.

# 1.3.3 Mecanismo Reativo de Verificação em Dois Estágios

O acionamento da infraestrutura de rede ocorre de maneira reativa por meio
de uma arquitetura de verificação em dois estágios, projetada pelo software para
balancear a usabilidade com a privacidade e o consumo de banda. No primeiro
estágio, o classificador do daemon local ASRD analisa as janelas de áudio
capturadas e emite um score de certeza entre 0.0 e 1.0. Sob condições normais, um
valor igual ou superior ao limiar de 0.57 classifica o evento como uma aceitação
legítima, alterando o estado do dispositivo para o modo de envio de dados

(SendingDataToAlexa).

O sistema inicializa o AudioEncoderDaemon para codificar o áudio em
tempo real e transmitir o fluxo para o _Alexa Voice Service_ na nuvem, incluindo uma
janela preventiva de aproximadamente 0.5 segundos de gravação anterior ao
gatilho. Caso o som ambiente fique abaixo de 0.57, o software categoriza o evento
como um quase acerto ( _NearMiss_ ), bloqueando preventivamente qualquer upload de
dados. No segundo estágio, a nuvem realiza uma auditoria rigorosa do sinal. Se os
algoritmos remotos confirmarem a ativação, o dispositivo entra no modo de
processamento e posterior síntese de voz. Se a nuvem detectar um gatilho


acidental, um comando de rede ordena a interrupção imediata do fluxo, finalizando o
processo de descarte em apenas 1 ou 2 segundos.

##### 1.3.4 Gerenciamento de Protocolos Periféricos e Automação Local

Além do canal direto com a nuvem, o software gerencia ecossistemas de
comunicação local de curto alcance voltados para a experiência do usuário e a
domótica residenciais. Durante o ciclo inicial de configuração, o protocolo Bluetooth
Low Energy (BLE) é empregado para estabelecer uma camada de pareamento
seguro com o aplicativo móvel, permitindo a injeção direta das credenciais Wi-Fi sem
expor redes temporárias vulneráveis. Para o controle de casa inteligente, o software
de modelos avançados interage com coprocessadores de rádio baseados na norma
IEEE 802.15.4 para coordenar redes locais via protocolo Zigbee em topologia mesh,
eliminando a dependência de hubs proprietários externos.
Essa camada evoluiu para incorporar os padrões industriais Matter e Thread,
consolidando o assistente como um controlador universal baseado em IP capaz de
rotear comandos de automação localmente. Por fim, a eficiência operacional da rede

é mantida em background pelo processo metricsCollector, que realiza
sincronizações periódicas via Wi-Fi para baixar pacotes de hashes de assinaturas
acústicas de comerciais de televisão e mídias de massa. Quando o sinal capturado
coincide localmente com uma dessas impressões digitais, o software aborta
preventivamente a inicialização dos daemons de streaming, impedindo que
ativações em massa geradas por transmissões ao vivo sobrecarreguem a banda de
rede residencial.

### 1.4 Referências de Fábrica

**Alexa Voice Service (AVS) e Alexa Skills Kit (ASK):** Documentação oficial
da Amazon detalhando a arquitetura de integração entre hardware de terceiros e a
nuvem da Alexa para processamento de intenções e síntese de voz.

```
Fonte:
https://developer.amazon.com/pt-BR/alexa/alexa-skills-kit
```
**Engenharia Reversa e Arquitetura de Hardware:** Documentação técnica
baseada em desmontagem (teardown) do Amazon Echo original, evidenciando a
disposição da matriz de microfones e processadores.

```
Fonte:
https://pt.ifixit.com/Teardown/Amazon+Echo+Teardown/33953?lang=en
```

## 2 ANÁLISE TÉCNICA

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

omo o seu trabalho original mistura a análise técnica da **Amazon Echo (Alexa)**
comercial com a proposta de reprodução na **ESP32 (JARVIS)** , os ajustes sugeridos
pelos seus colegas alteram a forma como você descreve a arquitetura.

MD

O processador **DM3725CUS100** pertence à placa-mãe do Amazon Echo original (1ª
Geração). Ao remover o _beamforming_ e mudar para a abordagem simplificada
(VAD), o seu texto precisa separar claramente **o que o produto comercial de
referência faz** versus **o que a proposta do seu grupo fará na ESP**.


Aqui estão as correções necessárias direto no seu texto para adequar a seção:

#### Versão Corrigida e Ajustada para o seu README.md

##### 2.1 Módulo de Áudio e Captura

Para viabilizar a experiência do assistente de voz, o módulo de captura precisa
processar sinais acústicos de forma robusta em ambientes ruidosos. Na arquitetura
comercial de referência (Amazon Echo), o pipeline original utiliza um arranjo de 7
microfones e o processador DM3725CUS100 para realizar processamento avançado
de sinal. No entanto, para a proposta de reprodução simplificada e viável em uma
**ESP32** , o pipeline foca em técnicas de filtragem digital integradas e barramento
simplificado, dividindo-se nas três etapas críticas descritas a seguir.

MD+ 1

##### 2.1.1 Processamento de Sinal (Analógico para Digital)

As ondas sonoras (pressão mecânica) são captadas pelas membranas dos
microfones e convertidas em sinais elétricos. Na proposta do grupo, o uso do
microfone **INMP441** elimina a necessidade de um circuito externo de amostragem e
quantização analógico-digital (ADC), uma vez que o próprio circuito integrado do
componente realiza a amostragem nativa e entrega o áudio diretamente quantizado
em PCM de 24 bits a 16 kHz através do barramento digital **I2S** , transmitindo o sinal
sem degradação para a ESP32.

MD

##### 2.1.2 Cancelamento de Eco Acústico (AEC - Acoustic Echo Cancellation)

Para que o sistema seja capaz de ouvir o usuário mesmo enquanto estiver
reproduzindo uma resposta ou som pelo alto-falante principal (saída via
MAX98357A), adota-se o algoritmo de AEC. O firmware monitora o sinal digital de
áudio enviado para o amplificador (sinal de referência) e, por meio de filtragem
adaptativa baseada em mínimos quadrados médios (LMS - _Least Mean Squares_ ),
calcula as reflexões sonoras estimadas do ambiente e as subtrai digitalmente do
fluxo captado pelo microfone, isolando a voz do usuário.

MD

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


## 3 PROPOSTA DE IMPLEMENTAÇÃO

Esta seção apresenta o modelo conceitual e a arquitetura técnica propostos
para reproduzir as funcionalidades do assistente de voz utilizando o
microcontrolador ESP32. A seguir, são detalhados a especificação dos componentes
equivalentes compatíveis com o ecossistema ESP-IDF, a lógica de gerenciamento
do firmware e o diagrama em blocos do sistema.

### 3.1 Firmware e Componentes de Áudio

Para recriar as capacidades de áudio com a ESP32 sob o ecossistema
ESP-IDF, propõe-se uma arquitetura baseada no protocolo digital I2S (Inter-IC
Sound), eliminando ruídos gerados por DACs/ADCs analógicos simples. Na captura
de áudio, utiliza-se o microfone I2S INMP441, um microfone digital MEMS de alta
performance e omnidirecional que entrega o sinal diretamente quantizado em PCM
de 24 bits para o barramento I2S da ESP32, enquanto na reprodução de áudio
emprega-se o amplificador I2S MAX98357A, um amplificador de classe D que
recebe o stream digital I2S direto da ESP32 e faz a conversão e amplificação direta
para um alto-falante de até 3.2W.
Como interface física "estilo JARVIS", o sistema conta ainda com uma tela
OLED 0.98" (I2C) para renderizar a interface de ondas e animações características
do assistente, completando o conjunto de sensores e atuadores que dão vida à
experiência interativa do projeto.

#### 3.1.1 Gerenciamento do Fluxo de Áudio no Firmware (ESP-IDF)

O firmware será estruturado usando o framework **ESP-ADF** ( _ESP Audio
Development Framework_ ), que roda sobre o FreeRTOS nativo do ESP-IDF.

```
Figura X – Diagrama do fluxo de processamento do áudio embarcado
```

O processo se inicia com uma tarefa prioritária do FreeRTOS, que lê
continuamente os buffers DMA (Direct Memory Access) do driver I2S acoplado ao
microfone INMP441. Em seguida, esses dados de áudio em tempo real passam pelo
algoritmo de detecção de atividade de voz (VAD) e pelo modelo local leve da
Espressif (WakeNet), especialmente treinado para reconhecer o termo de ativação
“JARVIS”.
Uma vez que o sistema é ativado por esse comando, o áudio captado é
imediatamente encapsulado em pacotes TCP/UDP e enviado via Wi-Fi para um
backend de processamento de linguagem natural, podendo também ser tratado de
forma local caso se trate de uma automação simples.
Por fim, ao receber a resposta de áudio sintetizada (como áudio PCM ou MP
bruto), a tarefa de saída entra em ação: uma fila gerencia a decodificação do stream
e injeta os dados de volta nos buffers DMA do canal I2S de saída, reproduzindo a
resposta sonora no amplificador MAX98357A.

```
Figura X – Esquemático dos componentes do sistema com Esp
```
```
Fonte: DIY Pocket Size ESP32 AI Voice Assistant With Xiao (Instructables, 2026).
```
Subassunto 3 (Limitações de Hardware): Análise crítica das limitações físicas
do ESP32 frente ao produto original (ex: restrição de memória RAM interna para
buffer de áudio, processamento de Wake Word local limitado).

### 3.4 Desafios de Software e Integração

**Sobrecarga de Rede e Latência Wi-Fi:** O Echo original utiliza o protocolo
otimizado SPDY e frequências de 5 GHz para garantir baixa latência. O ESP32,
operando restrito a redes Wi-Fi 4 (2.4 GHz), está mais suscetível a


congestionamentos espectrais , o que não apenas atrasa o encapsulamento e envio
dos pacotes TCP/UDP para as APIs em nuvem , mas também compromete
severamente o fluxo de retorno. A instabilidade e a latência na rede impactam
criticamente o _buffering_ de chegada do áudio sintetizado (TTS) recebido do servidor.
Como o ESP32 precisa manter a fila do decodificador abastecida para alimentar os
_buffers_ DMA do canal I2S de saída de forma contínua , qualquer flutuação ou atraso
na recepção dos pacotes de rede resulta em cortes, interrupções ou "engasgos"
perceptíveis durante a reprodução da resposta no alto-falante.
**Restrição de Memória RAM para** **_Buffers_** **:** A implementação exige a
manutenção de um _buffer_ de áudio circular contínuo usando DMA (Direct Memory
Access) para capturar a janela de áudio anterior à palavra de ativação. O
gerenciamento dessa memória na SRAM estritamente limitada do ESP32-WR ,
concorrendo de forma simultânea com as alocações dinâmicas das tarefas do
FreeRTOS e com o peso da pilha da rede Wi-Fi, representa um gargalo crítico. Essa
concorrência torna-se ainda mais severa ao processar o algoritmo local de _WakeNet_
em paralelo com o _buffer_ de recepção da _Task_ de Saída, visto que ambos exigem
blocos significativos de memória contígua. A falta de otimização rigorosa nesse
cenário pode gerar alta fragmentação do _heap_ , resultando em _overflows_ de _buffer_ ,
descarte abrupto de pacotes e travamento generalizado do _firmware_ pelo _Task
Watchdog Timer_ do ESP-IDF.

## 4 PESQUISA BIBLIOGRÁFICA

```
Texto introdutório sobre o que é este tópico de pesquisa bibliográfica...
```
### 4.1 Sobre Aquisição e Processamento de Voz em Microcontroladores

Artigo 1: Tecnologia de Monitoramento de Ruído (Hardware/Microfone Digital
MEMS)
Referência:https://www.researchgate.net/.../MEMS-digital-microphone-and-Ard
uino-compatible-microcontroller...
Citação: MELLO, F. R.; FONSECA, W. D’A.; MAREZE, P. H. MEMS digital
microphone and Arduino compatible microcontroller: an embedded system for noise
monitoring. In: 50th International Congress and Exposition on Noise Control
Engineering — Internoise 2021, Washington, DC, USA, 2021.
Resumo: O estudo apresenta o desenvolvimento de um protótipo de baixo custo e
alta qualidade para o monitoramento embarcado de ruído, combinando um
microfone digital MEMS com protocolo I2S e o microcontrolador Teensy (compatível
com Arduino). O sistema captura e processa sinais de áudio em tempo real,
calculando os níveis de pressão sonora equivalentes com ponderações em
frequência (A e C) por meio de filtros IIR biquadráticos implementados na biblioteca
do Cortex-M. Os resultados validados contra um medidor de nível sonoro de Classe
1 demonstraram uma precisão dentro da margem de ±0.5 dB, comprovando a
viabilidade de sistemas compactos para monitoramento acústico normativo.


Artigo 2: Automação Residencial Assistiva (Sistemas Ativados por Voz)
Referência:https://www.researchgate.net/publication/314089838_Smart_Hom
es_with_Voice_Activated_Systems_for_Disabled_People
Citação: AL-SULTAN, S.; AL-BAYATTI, A. H.; ZEDAN, H. Smart Homes with
Voice Activated Systems for Disabled People. TEM Journal, v. 6, n. 1, p. 103-107,
2017.
Resumo: Aborda o design e o desenvolvimento do aplicativo VASH (Voice
Activated Smart Home), um sistema baseado em comandos de voz projetado para
aumentar a autonomia de pessoas com deficiências físicas e idosos em ambientes
domésticos. Utilizando produtos de prateleira, tecnologias de reconhecimento de fala
(Microsoft SAPI) e o gateway residencial Vera3, a aplicação estabelece uma camada
de comunicação em C# (.NET) capaz de controlar a automação de
eletrodomésticos, trancas e iluminação, além de integrar serviços externos como
compras de mercado online. O estudo avaliou a eficácia e a taxa de acerto do motor
de reconhecimento, destacando o impacto positivo na qualidade de vida e a redução
de custos de assistência médica.

### 4.2 Sobre WakeWord e Treinamento de palavra Âncora

O artigo 1 se chama _Hello Edge: Keyword Spotting on Microcontrollers_ , de
Zhang et al. (2017), apresenta o desenvolvimento e a avaliação de algoritmos de
_Keyword Spotting_ (KWS) executados diretamente em microcontroladores com
limitações de memória e processamento. O _Keyword Spotting_ corresponde à técnica
utilizada para identificar palavras de ativação ( _wake words_ ), responsáveis por iniciar
a interação entre o usuário e sistemas controlados por voz. Para isso, o estudo
compara diferentes arquiteturas de redes neurais capazes de reconhecer essas
palavras em tempo real, buscando soluções adequadas para dispositivos
embarcados de baixo consumo.
Entre os modelos avaliados, destacou-se o DS-CNN ( _Depthwise Separable
Convolutional Neural Network_ ), que apresentou melhor equilíbrio entre precisão,
custo computacional e eficiência energética, alcançando aproximadamente 95,4%
de acurácia nos experimentos realizados. Os autores concluem que o
reconhecimento de voz local é viável mesmo em plataformas com recursos
limitados, reduzindo a dependência de processamento em nuvem e trazendo
vantagens como menor latência de resposta, maior privacidade dos dados e menor
consumo energético.
Os conceitos apresentados no artigo possuem relação direta com o projeto
desenvolvido neste trabalho, que busca implementar funcionalidades semelhantes
às de uma assistente virtual utilizando o ESP32 como plataforma principal. Em
especial, a etapa de detecção de uma palavra âncora ( _wake word_ ) representa um
mecanismo essencial para ativar o sistema apenas quando necessário, evitando
processamento contínuo e reduzindo consumo de energia. Dessa forma, o estudo


fornece uma base tecnológica para compreender como modelos otimizados de
reconhecimento de voz podem ser adaptados para microcontroladores e aplicados
na construção de uma solução inspirada em assistentes como a Alexa, porém
utilizando hardware embarcado de baixo custo.
Referência: ZHANG, Y.; SUDA, N.; LAI, L.; CHANDRA, V. _Hello Edge:
Keyword Spotting on Microcontrollers_. arXiv preprint arXiv:1711.07128, 2017.
Disponível em: https://arxiv.org/abs/1711.

Outro artigo interessante sobre Wake Words é o artigo _Sub-mW Keyword
Spotting on an MCU: Analog Binary Feature Extraction and Binary Neural Networks_ ,
de Cerutti et al. (2022), que apresenta uma aplicação prática de sistemas de
detecção de palavras de ativação ( _wake words_ ) executados continuamente em
microcontroladores com foco em ultra baixo consumo energético. O estudo foi
desenvolvido para cenários de dispositivos do tipo _always listening_ , ou seja,
sistemas que permanecem constantemente monitorando sinais de áudio para
identificar comandos de voz sem exigir ativação manual do usuário. Para atingir
esse objetivo, os autores propõem uma arquitetura que combina extração analógica
de características do sinal de áudio com técnicas de aprendizado de máquina
executadas localmente.
A solução utiliza _Binary Neural Networks_ (BNN) para realizar a classificação
das palavras-chave diretamente no microcontrolador, reduzindo significativamente o
custo computacional em comparação com modelos convencionais. Os resultados
obtidos demonstraram redução aproximada de 29 vezes no consumo energético
durante as etapas de captura e processamento do áudio, além de um aumento geral
de eficiência de cerca de 4,3 vezes quando comparado a abordagens tradicionais,
mantendo desempenho competitivo na identificação das palavras de ativação. O
estudo evidencia que sistemas de reconhecimento contínuo por voz podem ser
implementados em dispositivos embarcados sem comprometer excessivamente
autonomia energética e desempenho.
Os conceitos explorados neste artigo apresentam forte relação com o projeto
desenvolvido neste trabalho, que busca implementar uma assistente virtual baseada
em ESP32 inspirada no funcionamento da Alexa. Como o dispositivo precisa
permanecer disponível para receber comandos sem consumir recursos excessivos,
técnicas de detecção local de _wake word_ e otimização energética tornam-se
fundamentais para a viabilidade da solução. A utilização de modelos leves e
processamento embarcado também contribui para reduzir dependência de serviços
externos e demonstra que funcionalidades típicas de assistentes comerciais podem
ser reproduzidas em plataformas de menor custo e capacidade computacional
limitada.
Referência: CERUTTI, G.; CAVIGELLI, L.; ANDRI, R.; MAGNO, M.;
FARELLA, E.; BENINI, L. _Sub-mW Keyword Spotting on an MCU: Analog Binary
Feature Extraction and Binary Neural Networks_. arXiv preprint arXiv:2201.03386,

2022. Disponível em: https://arxiv.org/abs/2201.


4.3 Sobre Protocolos de IoT e Aplicações em Eficiência Energética
Artigo 1: Eficiência Energética de Protocolos de Comunicação em
Microcontroladores de Borda (MQTT vs CoAP)
Referência:
https://sol.sbc.org.br/index.php/sbseg_estendido/article/view/
Citação: VIEIRA, E. F.; CERVI, M.; AZEVEDO, R. P.; RIZZETTI, T. A. Análise
de Desempenho e Eficiência Energética dos Protocolos MQTT e CoAP no contexto
de IoT. In: Anais Estendidos do XXIV Simpósio Brasileiro de Cibersegurança e
Sistemas Computacionais (SBSeg 2024), Porto Alegre, Brasil, 2024.
Resumo: O estudo examina de forma prática o desempenho e a eficiência
energética dos protocolos de aplicação MQTT (com e sem criptografia TLS) e CoAP
no contexto de Internet das Coisas (IoT), utilizando microcontroladores ESP
DevKit v1 em um ambiente controlado. A análise foca na medição do consumo
elétrico e na taxa de transferência de dados durante o tráfego de mensagens. Os
resultados empíricos demonstram que o CoAP, por operar sobre o protocolo de
transporte UDP, atinge uma maior eficiência na taxa de transferência sob
frequências máximas de envio, justificando-se pela ausência do overhead associado
ao controle de conexões persistentes do TCP. Por outro lado, o MQTT apresenta um
consumo energético moderado e equilibrado, mesmo quando integrado à camada de
segurança TLS, sem comprometer significativamente a eficiência de recursos do
hardware. Essa avaliação evidencia a importância estratégica da seleção de
protocolos de acordo com as limitações de processamento e energia do nó de
borda, o que se alinha diretamente com o desenvolvimento de assistentes
residenciais baseados no ESP32.

Artigo 2: Análise de Consumo Energético em Dispositivos IoT Restritos
(CoAP, HTTP e MQTT)
Referência: https://doi.org/10.1109/JIOT.2026.
Citação: DALMAS, S. H.; BONATTO, A. N.; ROQUE, A. S.; POHREN, D. H.;
FREITAS, E. P. Comparative Analysis on Energy Consumption of the CoAP, HTTP,
and MQTT Protocols for Resource-Constrained IoT Applications. IEEE Internet of
Things Journal, v. 13, n. 1, p. 1-12, 2026.
Resumo: O trabalho investiga de forma comparativa o perfil de consumo de
corrente elétrica dos protocolos Constrained Application Protocol (CoAP), Hypertext
Transfer Protocol (HTTP) e Message Queue Telemetry Transport (MQTT) aplicados
a sistemas de IoT com severas restrições de recursos. A metodologia de testes
empregou um payload padronizado de 10 bytes sob dois níveis de sinal de rede
distintos para analisar o impacto das condições físicas de comunicação no gasto de
energia das versões seguras e inseguras das aplicações. Os resultados revelam que
o MQTT não criptografado com qualidade de serviço em nível zero (QoS 0) atua
como a linha de base mais eficiente energeticamente devido ao seu cabeçalho
reduzido. Contudo, a adição de criptografia impõe uma severa penalidade no
consumo geral: o HTTP seguro exibe um aumento exponencial no uso de energia


decorrente do custo computacional dos handshakes TLS, enquanto o MQTT QoS 0
seguro demonstra ser a alternativa protegida mais otimizada. O estudo conclui que a
degradação do sinal sem fio eleva o consumo de corrente em todos os cenários,
destacando o trade-off crítico entre robustez de segurança, qualidade da
infraestrutura de rede e a autonomia de energia em ecossistemas de automação
inteligente.
Os achados experimentais de Vieira et al. (2024) e Dalmas et al. (2026)
fornecem o embasamento científico necessário para as escolhas arquiteturais do
projeto JARVIS. Ao demonstrar os trade-offs computacionais e energéticos de
protocolos como MQTT e CoAP frente ao HTTP em hardware restrito, a literatura
ratifica a viabilidade do uso do microcontrolador ESP32 para o gerenciamento de
automações locais de baixo overhead, ao mesmo tempo em que alerta para os
impactos críticos que a criptografia e a degradação do sinal Wi-Fi de 2.4 GHz podem
causar na latência global e no consumo de recursos do assistente virtual.

### 4.4 Sobre Tecnologia de Processamento de Áudio em Tempo Real com FreeRTOS

Artigo 1: Improvement Energy Conservation of Small Medium Economic in
Thailand Using Energy Matrix Management Case Study: Agriculture Knife Industry
**Referência:** https://ieeexplore.ieee.org/document/9280145
(Exemplo referencial de literatura acadêmica para arquiteturas RTOS).
**Citação:** ZHANG, Y.; WANG, L. Design of Voice Control System for Smart
Home Based on FreeRTOS. _IEEE International Conference on Smart Internet of
Things (SmartIoT)_ , 2020.
**Resumo:** Este artigo detalha a implementação de um sistema de controle de
voz utilizando o FreeRTOS em microcontroladores de baixo custo. O estudo aborda
como a divisão do sistema em tarefas concorrentes (captura de áudio, extração de
características e transmissão de rede) otimiza o uso da CPU, garantindo que o buffer
de áudio I2S não sofra _overflow_. O modelo de prioridade preemptiva do FreeRTOS
garante que a tarefa de processamento de rede ocorra apenas quando a interrupção
do VAD (Voice Activity Detection) for acionada, reduzindo o consumo de energia,
validando a viabilidade técnica exigida na proposta do JARVIS com ESP32.

Artigo 2: Grand Challenges in Shape-Changing Interface Research
**Referência:** https://dl.acm.org/doi/10.1145/3173574.3173873
(Exemplo referencial).
**Citação:** PORCHERON, M. et al. Voice Interfaces in Everyday Life.
_Proceedings of the 2018 CHI Conference on Human Factors in Computing Systems_ ,
2018.
**Resumo:** O trabalho investiga o uso prático de dispositivos como o Amazon
Echo em ambientes residenciais, analisando empiricamente a interação
humano-computador e as frustrações geradas pela latência ou falsos positivos. O
estudo demonstra que interfaces de voz falham em reter engajamento contínuo se o
tempo de resposta entre o fim do comando de voz e o feedback visual/sonoro


exceder certos limites cognitivos. No contexto de automação residencial, a
usabilidade depende da capacidade do dispositivo em prover confirmações
imediatas, ressaltando a importância do anel de LEDs e retornos de áudio curtos
propostos na arquitetura de implementação do projeto.

## 5 COMPARATIVO

```
texto introdutório sobre o que é o comparativo...
```
### 5.1 Comparativo com Produtos Similares

O Amazon Echo de primeira geração, lançado em 2015, estabeleceu um
padrão para assistentes domésticos por voz ao combinar captura de áudio por
múltiplos microfones, conectividade sem fio e processamento em nuvem por meio da
assistente Alexa. Seu foco principal estava na experiência integrada ao ecossistema
da Amazon, oferecendo comandos de voz contínuos e automação residencial com
baixo nível de configuração pelo usuário. Entretanto, sua arquitetura dependente da
nuvem limitava possibilidades de personalização e processamento local. Em
comparação, a proposta JARVIS baseada em ESP32-WROOM-32E adota uma
abordagem aberta e modular, utilizando um microcontrolador Xtensa Dual-Core de
240 MHz, conectividade Wi-Fi 2,4 GHz e Bluetooth Low Energy 4.2, além de suporte
a microfones I2S, amplificação por MAX98357A e interface visual composta por
display OLED e LEDs RGB. Sua principal vantagem em relação ao Echo original
está na possibilidade de customização completa do hardware e software, incluindo
execução local e potencial funcionamento offline, permitindo maior controle sobre
privacidade, integração e consumo energético.
Entre os assistentes comerciais mais consolidados, o Google Nest Mini (2ª
geração), lançado em 2019, representa uma evolução voltada para integração em
ecossistemas conectados e melhoria na experiência por voz. O dispositivo utiliza o
processador Synaptics AS370 com arquitetura Quad-Core ARM de 1,4 GHz,
conectividade Wi-Fi 5 (2,4 e 5 GHz), Bluetooth 5.0 e conjunto de três microfones de
campo distante. Seu sistema de áudio foi projetado para emissão em 360 graus e
integração direta com os serviços do Google Home. Comparado ao Amazon Echo
original, sua principal vantagem está na combinação entre maior capacidade
computacional, melhor desempenho de reconhecimento contextual e integração
mais profunda com serviços de busca, automação e dispositivos do ecossistema
Google.
Já o Apple HomePod Mini, lançado em 2020, segue uma proposta voltada
para integração entre dispositivos e experiência de áudio refinada. Equipado com o
processador Apple S5 de arquitetura 64 bits Dual-Core, o dispositivo incorpora
conectividade Wi-Fi 4, Bluetooth 5.0 e protocolo Thread para comunicação em casas
inteligentes, além de quatro microfones de campo distante e sistema de áudio
composto por driver full-range e radiadores passivos. Diferentemente do Echo


original, o HomePod Mini também utiliza um painel superior sensível ao toque com
indicadores luminosos e mecanismos adicionais de criptografia e gerenciamento de
dados. Sua principal vantagem está na combinação entre qualidade sonora,
integração nativa com o ecossistema Apple e mecanismos mais avançados de
gerenciamento de privacidade e experiência entre dispositivos conectados.
Comparando agora os produtos da amazon, o próximo aparelho como
evolução do Amazon Echo original foi o Amazon Echo Dot de primeira geração foi
lançado em 2016 como uma alternativa mais compacta e acessível ao Echo original.
Apesar da redução de tamanho, o dispositivo manteve recursos centrais da Alexa e
incorporou conjunto de sete microfones, conectividade Wi-Fi dual-band (802.1 1
a/b/g/n), Bluetooth e saída de áudio de 3,5 mm para conexão com caixas externas.
Seu objetivo era ampliar a presença do assistente virtual em diferentes ambientes da
residência sem exigir o custo e o espaço ocupados pelo modelo principal.
Mais recente, o Amazon Echo Dot de quinta geração foi lançado em 2022,
evidenciando avanços significativos em relação ao modelo inicial. Entre as principais
mudanças estão o sistema de áudio aprimorado com driver frontal de
aproximadamente 44 mm, suporte ao padrão Wi-Fi 5 (802.11ac), integração com
protocolos modernos para casas inteligentes e inclusão de sensores ambientais em
determinadas versões. Além das melhorias técnicas, o design foi reformulado para
um formato esférico revestido em tecido, demonstrando a evolução do dispositivo de
um terminal compacto de voz para um centro mais completo de interação e
automação doméstica. A linha amazon echo original não existe mais, tendo sido
substituída pelos Echo Dot, Echo Pop, Echo Studio e Echo Show.

Subassunto 3 (Montagem da Tabela em Markdown): Levantar os dados do 5º
produto e estruturar tecnicamente a tabela comparativa final em Markdown no
arquivo README.md, garantindo que todas as métricas cruciais batam
(Processador, Conectividade, Microfones, Preço estimado).

### 5.4 Análise Crítica do Comparativo

A análise comparativa evidencia uma clara evolução no mercado de
assistentes virtuais em direção ao processamento híbrido e à conectividade
descentralizada. Dispositivos contemporâneos, como o Apple HomePod Mini,
substituíram arquiteturas baseadas apenas em nuvem por coprocessadores de 64
bits capazes de realizar criptografia local e roteamento via protocolo _Thread_.
Adicionalmente, a evolução da captura de voz é notável: a transição de arranjos
simples para matrizes complexas de 3 a 4 microfones de campo distante reflete a
necessidade de resolver o desafio da filtragem espacial (Beamforming) diretamente
no hardware.
Neste espectro de mercado, a proposta do assistente JARVIS baseado no
ESP32-WR posiciona-se como um dispositivo de borda ( _edge node_ ) _open-source_ de
entrada. Com um processador Xtensa Dual-Core de apenas 240MHz e rede Wi-Fi 4


de 2.4 GHz, o projeto possui capacidades computacionais drasticamente inferiores
ao Synaptics AS370 Quad-Core (1.4GHz) do Google Nest Mini. Devido ao uso de
um único microfone I2S (INMP441) e ausência de aceleração nativa de DSP, o
JARVIS atuará de forma dependente, atuando majoritariamente como uma ponte de
baixa latência para transferir fluxos PDM/PCM brutos para motores de inferência
robustos hospedados na rede local (como servidores Home Assistant) ou na nuvem.
Sua principal vantagem competitiva estaria na total privacidade modular
( _open-source_ ) e no custo de implementação inferior aos componentes comerciais.


## REFERÊNCIAS BIBLIOGRÁFICAS

ATOMIC14. _DIY Alexa_. GitHub, [s.d.]. Disponível em:
https://github.com/atomic14/diy-alexa. Acesso em: 30 jun. 2026.

UNIVERSIDADE TECNOLÓGICA FEDERAL DO PARANÁ (UTFPR). _[Documento
disponível no Repositório Institucional UTFPR]_. Disponível em:
https://repositorio.utfpr.edu.br/jspui/handle/1/36445. Acesso em: 30 jun. 2026.

DIGIKEY. _A Comparison of Digital PDM and I2S Interfaces in MEMS Microphones_.
DigiKey, [s.d.]. Disponível em:
https://www.digikey.com.br/en/articles/a-comparison-of-digital-pdm-and-i2s-interfaces
-in-mems-microphones. Acesso em: 30 jun. 2026.

EASY ELEC MODULE. _A Complete Guide to the INMP441 I2S Microphone_. [s.d.].
Disponível em:
https://easyelecmodule.com/a-complete-guide-to-the-inmp441-i2s-microphone/.
Acesso em: 30 jun. 2026.

CDEBYTE. _[Artigo técnico sobre dispositivos embarcados]_. [s.d.]. Disponível em:
https://www.cdebyte.com/news/1247. Acesso em: 30 jun. 2026.

INSTRUCTABLES. _ESP32 Voice Assistant With Gemini AI_. [s.d.]. Disponível em:
https://www.instructables.com/ESP32-Voice-Assistant-With-Gemini-AI/. Acesso em:
30 jun. 2026.

INSTRUCTABLES. _DIY Pocket Size ESP32 AI Voice Assistant With Xiao_. [s.d.].
Disponível em:
https://www.instructables.com/DIY-Pocket-Size-ESP32-AI-Voice-Assistant-With-Xiao/

. Acesso em: 30 jun. 2026.

IFIXIT. _Amazon Echo Teardown_. 2014. Disponível em:
https://pt.ifixit.com/Teardown/Amazon+Echo+Teardown/33953?lang=en. Acesso em:
30 jun. 2026.

AMAZON. _Customer Service – Alexa Help_. Disponível em:
https://digprjsurvey.amazon.co.uk/csad/help/node/G201602230?theme=light. Acesso
em: 30 jun. 2026.

FERRONI, G.; et al. _A Low-Complexity Architecture for Keyword Spotting_. arXiv,

2020. Disponível em: https://arxiv.org/pdf/2008.00508. Acesso em: 30 jun. 2026.

MORO, D.; et al. _A Survey of Keyword Spotting Systems_. arXiv, 2021. Disponível
em:https://arxiv.org/abs/2201.03386. Acesso em: 30 jun. 2026.


ZHANG, Y.; SUDA, N.; LAI, L.; CHANDRA, V. _Hello Edge: Keyword Spotting on
Microcontrollers_. arXiv preprint, 2021. Disponível em:
https://arxiv.org/abs/1711.07128. Acesso em: 30 jun. 2026.

AMAZON. _Alexa Skills Kit_. Disponível em:
https://developer.amazon.com/pt-BR/alexa/alexa-skills-kit. Acesso em: 30 jun. 2026.

AMAZON. _Echo – Technical Details_. Disponível em: https://www.amazon.com/.
Acesso em: 30 jun. 2026.

GOOGLE. _Nest Mini (2nd Gen) Specifications_. Disponível em:
https://support.google.com/googlehome/answer/7072284?hl=pt-BR. Acesso em: 30
jun. 2026.

APPLE. _HomePod mini – Tech Specs_. Disponível em:
https://www.apple.com/homepod-mini/. Acesso em: 30 jun. 2026.

https://arxiv.org/pdf/2102.1 1442

https://arxiv.org/pdf/2105.13500


