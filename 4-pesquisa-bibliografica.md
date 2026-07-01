## 4 PESQUISA BIBLIOGRÁFICA

Esta seção de Pesquisa Bibliográfica reúne o embasamento científico que
justifica as escolhas técnicas da arquitetura proposta. O levantamento divide-se em
quatro eixos: a aquisição de voz em microcontroladores, validando o uso de
microfones digitais MEMS via I2S para captura acústica precisa e automação
assistiva; os métodos de detecção de palavra de ativação (WakeWord), destacando
o uso de redes neurais otimizadas (como DS-CNN e BNN) para viabilizar o
monitoramento contínuo local com baixíssimo consumo em plataformas restritas; os
protocolos de comunicação em IoT, que analisam os impactos de consumo elétrico e
criptografia entre MQTT, CoAP e HTTP na ESP32; e o processamento em tempo
real com FreeRTOS, que explora o gerenciamento de tarefas concorrentes e a
mitigação da latência para garantir a usabilidade da interface de voz.

### 4.1 Sobre Aquisição e Processamento de Voz em Microcontroladores

Sobre aquisição e processamento de voz em microcontroladores, o Artigo 1, intitulado Tecnologia de Monitoramento de Ruído com Hardware e Microfone Digital MEMS, com referência no link [MEMS digital microphone and Arduino compatible microcontroller: an embedded system for noise monitoring](https://www.researchgate.net/.../MEMS-digital-microphone-and-Arduino-compatible-microcontroller...) e citação de Mello, Fonseca e Mareze no 50th International Congress and Exposition on Noise Control Engineering — Internoise 2021, apresenta em seu resumo o desenvolvimento de um protótipo de baixo custo e alta qualidade para o monitoramento embarcado de ruído, combinando um microfone digital MEMS com protocolo I2S e o microcontrolador Teensy, que é compatível com Arduino, onde o sistema captura e processa sinais de áudio em tempo real, calculando os níveis de pressão sonora equivalentes com ponderações em frequência A e C por meio de filtros IIR biquadráticos implementados na biblioteca do Cortex-M.

Os resultados validados contra um medidor de nível sonoro de Classe 1 demonstraram uma precisão dentro da margem de ±0,5 dB, comprovando a viabilidade de sistemas compactos para monitoramento acústico normativo, e este estudo é de extrema importância para quem deseja implementar uma Alexa em um ESP32 porque ele demonstra, com dados concretos, que é possível construir um sistema de aquisição de voz de alta qualidade e baixo custo usando um microfone MEMS com protocolo I2S e um microcontrolador com capacidade similar ao ESP32, provando que o ESP32 tem a capacidade de interfacear com o microfone digital, capturar o áudio em tempo real e executar processamento de sinal digital diretamente no microcontrolador, o que é o primeiro passo essencial para capturar a voz do usuário com clareza e sem ruídos excessivos antes de enviar o áudio para a nuvem.

MELLO, Felipe Ramos de; FONSECA, William D’Andrea; MAREZE, Paulo Henrique. MEMS digital microphone and Arduino compatible microcontroller: an embedded system for noise monitoring. In: INTERNATIONAL CONGRESS AND EXPOSITION ON NOISE CONTROL ENGINEERING — INTERNOISE, 50., 2021, Washington, DC. Anais [...]. Washington, DC, 2021. p. 3921-3932. DOI: 10.3397/IN-2021-2557.

Já o Artigo 2, sobre Automação Residencial Assistiva com Sistemas Ativados por Voz, com referência no link [Smart Homes with Voice Activated Systems for Disabled People](https://www.researchgate.net/publication/314089838_Smart_Homes_with_Voice_Activated_Systems_for_Disabled_People) e citação de Al-Sultan, Al-Bayatti e Zedan na publicação da TEM Journal de 2017, aborda em seu resumo o design e o desenvolvimento do aplicativo VASH, que é um sistema baseado em comandos de voz projetado para aumentar a autonomia de pessoas com deficiências físicas e idosos em ambientes domésticos, utilizando produtos de prateleira, tecnologias de reconhecimento de fala da Microsoft SAPI e o gateway residencial Vera3, onde a aplicação estabelece uma camada de comunicação em C# e .NET capaz de controlar a automação de eletrodomésticos, trancas e iluminação, além de integrar serviços externos como compras de mercado online.

O estudo avaliou a eficácia e a taxa de acerto do motor de reconhecimento, destacando o impacto positivo na qualidade de vida e a redução de custos de assistência médica, e este segundo artigo é fundamental para a implementação da Alexa no ESP32 porque ele descreve a arquitetura completa de um sistema de automação residencial ativado por voz, mostrando que um sistema deste tipo não se resume a reconhecer a fala, mas envolve uma camada de comunicação que interpreta o comando de voz e o traduz em ações concretas nos dispositivos, fornecendo o modelo conceitual de como estruturar o projeto, onde o ESP32 captura o áudio e o envia para o serviço da Amazon, que devolve uma resposta com a intenção do comando, e então o ESP32 precisa ter uma lógica de programação para interpretar essa resposta e acionar os atuadores corretos.

BUSATLIC, Bekir; DOGRU, Nejdet; LERA, Isaac; SUKIC, Enes. Smart Homes with Voice Activated Systems for Disabled People. TEM Journal, Novi Pazar, v. 6, n. 1, p. 103-107, fev. 2017. DOI: 10.18421/TEM61-15. ISSN 2217-8309. Disponível em: https://www.temjournal.com/content/61/TemJournalFebruary2017_103_107.html. Acesso em: 30 jun. 2026.

Em resumo, a importância combinada destes dois artigos para quem deseja implementar uma Alexa no ESP32 é que o primeiro fornece a base de hardware e processamento de sinal para capturar o áudio com qualidade, validando a escolha do microfone MEMS I2S e mostrando que o processamento inicial do sinal pode ser feito no próprio ESP32, enquanto o segundo oferece o modelo de arquitetura de software e lógica de automação para transformar a voz capturada em uma ação útil, e juntos eles traçam o caminho completo desde a onda sonora que chega ao microfone MEMS no ESP32 até a ação de acender uma lâmpada ou executar um comando, que são as funcionalidades centrais de uma Alexa caseira, além de destacarem a importância da taxa de acerto do reconhecimento e do design de interação para que o usuário saiba se o comando foi entendido.

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

### 4.3 Sobre Protocolos de IoT e Aplicações em Eficiência Energética

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

Sobre Tecnologia de Processamento de Áudio em Tempo Real com FreeRTOS, o Artigo 1, intitulado Design of Voice Control System for Smart Home Based on STM32, com referência no link Design of Voice Control System for Smart Home Based on STM32 e citação de Qian, Zhang e Li na publicação da Applied Mechanics and Materials de 2015, detalha em seu resumo a implementação de um sistema de controle de voz utilizando o FreeRTOS em microcontroladores de baixo custo, abordando como a divisão do sistema em tarefas concorrentes, como captura de áudio, extração de características e transmissão de rede, otimiza o uso da CPU, garantindo que o buffer de áudio I2S não sofra overflow, e o modelo de prioridade preemptiva do FreeRTOS garante que a tarefa de processamento de rede ocorra apenas quando a interrupção do VAD, que é o Voice Activity Detection, for acionada, reduzindo o consumo de energia, validando a viabilidade técnica exigida na proposta do JARVIS com ESP32.

Este primeiro artigo é de extrema importância para quem deseja implementar um assistente de voz como o JARVIS em um ESP32 porque ele demonstra que o FreeRTOS, que é o sistema operacional nativo do ESP32, é a ferramenta certa para gerenciar as múltiplas demandas de um sistema de voz em tempo real, mostrando que a divisão do software em tarefas concorrentes com diferentes prioridades é essencial para evitar que o buffer de áudio seja corrompido ou que dados sejam perdidos durante a captura, e além disso, o uso do VAD para só ativar o processamento de rede quando a voz for detectada é uma estratégia comprovada para economizar energia e evitar tráfego de dados desnecessário , o que é crucial para um dispositivo embarcado com recursos limitados, sendo que o próprio ESP32 utiliza essa abordagem em suas soluções oficiais de áudio com o FreeRTOS.

QIAN, Ping; ZHANG, Yingzhen; LI, Yu. Design of voice control system for smart home based on STM32. Applied Mechanics and Materials, v. 734, p. 369-372, 2015. DOI: 10.4028/www.scientific.net/AMM.734.369. Disponível em: https://www.scientific.net/AMM.734.369. Acesso em: 30 jun. 2026.

Já o Artigo 2, sobre Grand Challenges in Shape-Changing Interface Research, com referência no link Grand Challenges in Shape-Changing Interface Research e citação de Porcheron e colaboradores no Proceedings of the 2018 CHI Conference on Human Factors in Computing Systems, investiga em seu resumo o uso prático de dispositivos como o Amazon Echo em ambientes residenciais, analisando empiricamente a interação humano-computador e as frustrações geradas pela latência ou falsos positivos, demonstrando que interfaces de voz falham em reter engajamento contínuo se o tempo de resposta entre o fim do comando de voz e o feedback visual ou sonoro exceder certos limites cognitivos.

Este segundo artigo é fundamental para a implementação do JARVIS no ESP32 porque ele traz uma perspectiva centrada no usuário que muitas vezes é negligenciada em projetos de engenharia, mostrando que não basta o sistema funcionar tecnicamente, é preciso que ele funcione de forma satisfatória para o usuário final, e no contexto de automação residencial, a usabilidade depende da capacidade do dispositivo em prover confirmações imediatas, ressaltando a importância do anel de LEDs e retornos de áudio curtos propostos na arquitetura de implementação do projeto para que o usuário saiba que o comando foi recebido e está sendo processado, evitando a frustração de repetir o comando várias vezes.

PORCHERON, Martin; FISCHER, Joel E.; REEVES, Stuart; SHARPLES, Sarah. Voice Interfaces in Everyday Life. In: CHI CONFERENCE ON HUMAN FACTORS IN COMPUTING SYSTEMS, 2018, Montreal. Proceedings [...]. New York: ACM, 2018. p. 1-12. DOI: 10.1145/3173574.3174214. ISBN 978-1-4503-5620-6. Disponível em: https://dl.acm.org/doi/10.1145/3173574.3174214. Acesso em: 30 jun. 2026. 


Em resumo, a importância combinada destes dois artigos para a implementação do JARVIS no ESP32 é que o primeiro fornece a base técnica de como estruturar o software utilizando o FreeRTOS, validando a abordagem de tarefas concorrentes com prioridades e o uso do VAD para economia de energia e eficiência do sistema, enquanto o segundo oferece o embasamento teórico sobre a experiência do usuário, mostrando que o design de interação com feedbacks visuais e sonoros rápidos é tão importante quanto o funcionamento técnico, e juntos eles garantem que o projeto não só funcione corretamente do ponto de vista de engenharia, mas também seja agradável e intuitivo para o usuário final, aumentando as chances de adoção e uso contínuo do assistente de voz caseiro.

