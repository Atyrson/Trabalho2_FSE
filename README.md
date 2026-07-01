# Trabalho 2 - Estudo de Caixa de Som Inteligente com Assistente Virtual

Atyrson Souto da Silva - 251005945

Caio Venâncio do Rosário - 231027195

Diogo Rodrigues Barboza - 222006660

Nathan Batista Santos - 222006285

## 1 Descrição do Produto Referência

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

[Texto completo da descrição do produto](1-descricao-produto.md)

## 2 Análise Técnica

Esta parte apresenta a Análise Técnica do projeto, estruturada para detalhar a engenharia interna de um assistente de voz sob duas perspectivas: a arquitetura comercial de referência do Amazon Echo. Ao longo desta seção, serão examinados os quatro pilares fundamentais do sistema: o Módulo de Áudio e Captura, que abrange desde a digitalização do som via barramento I2S até algoritmos de cancelamento de eco (AEC) e detecção de voz (VAD); o Módulo de Controle e Nuvem, que explica o fluxo híbrido de processamento de comandos através do Alexa Voice Service (AVS) e o uso de skills; os Protocolos e Redes Críticas, detalhando as estratégias de comunicação de baixa latência como conexões multiplexadas, WebSockets e redes IoT (MQTT e Matter); e, por fim, a gestão de Sistemas Operacionais e Energia, que aborda o isolamento de processos e a eficiência no consumo energético durante o monitoramento passivo e ativo do dispositivo.

[Texto completo da análise técnica](2-analise-tecnica.md)

## 3 Proposta de Implementação

Esta seção apresenta o modelo conceitual e a arquitetura técnica propostos
para reproduzir as funcionalidades do assistente de voz utilizando o
microcontrolador ESP32. A seguir, são detalhados a especificação dos componentes
equivalentes compatíveis com o ecossistema ESP-IDF, a lógica de gerenciamento
do firmware e o diagrama em blocos do sistema.

[Texto completo da proposta de implementação](3-proposta-implementacao.md)

## 4 Pesquisa bibliográfica

Esta seção de Pesquisa Bibliográfica reúne o embasamento científico que justifica as escolhas técnicas da arquitetura proposta. O levantamento divide-se em quatro eixos: a aquisição de voz em microcontroladores, validando o uso de microfones digitais MEMS via I2S para captura acústica precisa e automação assistiva; os métodos de detecção de palavra de ativação (WakeWord), destacando o uso de redes neurais otimizadas (como DS-CNN e BNN) para viabilizar o monitoramento contínuo local com baixíssimo consumo em plataformas restritas; os protocolos de comunicação em IoT, que analisam os impactos de consumo elétrico e criptografia entre MQTT, CoAP e HTTP na ESP32; e o processamento em tempo real com FreeRTOS, que explora o gerenciamento de tarefas concorrentes e a mitigação da latência para garantir a usabilidade da interface de voz.

[Texto completo da pesquisa bibliográfica](4-pesquisa-bibliografica.md)

## 5 Comparativo

Esta seção de Comparativo apresenta um estudo mercadológico e tecnológico contrapondo o projeto JARVIS às soluções comerciais consolidadas. O capítulo detalha a evolução dos assistentes virtuais, analisando desde pioneiros como o Amazon Echo e Echo Dot (1ª e 5ª gerações) até ecossistemas focados em conectividade e privacidade, como o Google Nest Mini e o Apple HomePod Mini. Em seguida, as especificações técnicas de hardware — como processador, conectividade, microfones e custo — são consolidadas em uma tabela comparativa estruturada. Por fim, uma análise crítica avalia os limites computacionais da ESP32 frente aos chips comerciais, destacando o diferencial do JARVIS como um nó de borda de código aberto, econômico e centrado na privacidade local.

[Texto completo do comparativo](5-comparativo.md)

---

### Referências Bibliográficas

ATOMIC14. DIY Alexa. GitHub, [s.d.]. Disponível em: https://github.com/atomic14/diy-alexa. Acesso em: 30 jun. 2026.
UNIVERSIDADE TECNOLÓGICA FEDERAL DO PARANÁ (UTFPR). [Documento disponível no Repositório Institucional UTFPR]. Disponível em: https://repositorio.utfpr.edu.br/jspui/handle/1/36445. Acesso em: 30 jun. 2026.

DIGIKEY. A Comparison of Digital PDM and I2S Interfaces in MEMS Microphones. DigiKey, [s.d.]. Disponível em: https://www.digikey.com.br/en/articles/a-comparison-of-digital-pdm-and-i2s-interfaces-in-mems-microphones. Acesso em: 30 jun. 2026.

EASY ELEC MODULE. A Complete Guide to the INMP441 I2S Microphone. [s.d.]. Disponível em: https://easyelecmodule.com/a-complete-guide-to-the-inmp441-i2s-microphone/. Acesso em: 30 jun. 2026.

CDEBYTE. [Artigo técnico sobre dispositivos embarcados]. [s.d.]. Disponível em: https://www.cdebyte.com/news/1247. Acesso em: 30 jun. 2026.

INSTRUCTABLES. ESP32 Voice Assistant With Gemini AI. [s.d.]. Disponível em: https://www.instructables.com/ESP32-Voice-Assistant-With-Gemini-AI/. Acesso em: 30 jun. 2026.

INSTRUCTABLES. DIY Pocket Size ESP32 AI Voice Assistant With Xiao. [s.d.]. Disponível em: https://www.instructables.com/DIY-Pocket-Size-ESP32-AI-Voice-Assistant-With-Xiao/. Acesso em: 30 jun. 2026.

IFIXIT. Amazon Echo Teardown. 2014. Disponível em: https://pt.ifixit.com/Teardown/Amazon+Echo+Teardown/33953?lang=en. Acesso em: 30 jun. 2026.

AMAZON. Customer Service – Alexa Help. Disponível em: https://digprjsurvey.amazon.co.uk/csad/help/node/G201602230?theme=light. Acesso em: 30 jun. 2026.

FERRONI, G.; et al. A Low-Complexity Architecture for Keyword Spotting. arXiv, 2020. Disponível em: https://arxiv.org/pdf/2008.00508. Acesso em: 30 jun. 2026.

MORO, D.; et al. A Survey of Keyword Spotting Systems. arXiv, 2021. Disponível em:https://arxiv.org/abs/2201.03386. Acesso em: 30 jun. 2026.

ZHANG, Y.; SUDA, N.; LAI, L.; CHANDRA, V. Hello Edge: Keyword Spotting on Microcontrollers. arXiv preprint, 2021. Disponível em: https://arxiv.org/abs/1711.07128. Acesso em: 30 jun. 2026.

AMAZON. Alexa Skills Kit. Disponível em: https://developer.amazon.com/pt-BR/alexa/alexa-skills-kit. Acesso em: 30 jun. 2026.
AMAZON. Echo – Technical Details. Disponível em: https://www.amazon.com/. Acesso em: 30 jun. 2026.

GOOGLE. Nest Mini (2nd Gen) Specifications. Disponível em: https://support.google.com/googlehome/answer/7072284?hl=pt-BR. Acesso em: 30 jun. 2026.

APPLE. HomePod mini – Tech Specs. Disponível em: https://www.apple.com/homepod-mini/. Acesso em: 30 jun. 2026.
LI, Yanyan; KIM, Sara; SY, Eric. A survey on Amazon Alexa attack surfaces. [S.l.]: arXiv, 2021. Disponível em: https://arxiv.org/pdf/2102.11442. Acesso em: 30 jun. 2026.

JANAK, Jan et al. An analysis of Amazon Echo's network behavior. [S.l.]: arXiv, 2021. Disponível em: https://arxiv.org/pdf/2105.13500. Acesso em: 30 jun. 2026.
