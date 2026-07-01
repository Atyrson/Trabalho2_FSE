# Trabalho 2 - ESTUDO DE CAIXA DE SOM INTELIGENTE COM ASSISTENTE VIRTUAL

Atyrson Souto da Silva - 251005945
Caio Venâncio do Rosário - 231027195
Diogo Rodrigues Barboza - 222006660
Nathan Batista Santos - 222006285

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

## 2 ANÁLISE TÉCNICA

Esta parte apresenta a Análise Técnica do projeto, estruturada para detalhar a engenharia interna de um assistente de voz sob duas perspectivas: a arquitetura comercial de referência do Amazon Echo. Ao longo desta seção, serão examinados os quatro pilares fundamentais do sistema: o Módulo de Áudio e Captura, que abrange desde a digitalização do som via barramento I2S até algoritmos de cancelamento de eco (AEC) e detecção de voz (VAD); o Módulo de Controle e Nuvem, que explica o fluxo híbrido de processamento de comandos através do Alexa Voice Service (AVS) e o uso de skills; os Protocolos e Redes Críticas, detalhando as estratégias de comunicação de baixa latência como conexões multiplexadas, WebSockets e redes IoT (MQTT e Matter); e, por fim, a gestão de Sistemas Operacionais e Energia, que aborda o isolamento de processos e a eficiência no consumo energético durante o monitoramento passivo e ativo do dispositivo.

## 3 PROPOSTA DE IMPLEMENTAÇÃO

Esta seção apresenta o modelo conceitual e a arquitetura técnica propostos
para reproduzir as funcionalidades do assistente de voz utilizando o
microcontrolador ESP32. A seguir, são detalhados a especificação dos componentes
equivalentes compatíveis com o ecossistema ESP-IDF, a lógica de gerenciamento
do firmware e o diagrama em blocos do sistema.

## 4 PESQUISA BIBLIOGRÁFICA

## 5 COMPARATIVO

### Referências Bibliográficas