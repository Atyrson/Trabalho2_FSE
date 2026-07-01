## 5 COMPARATIVO

Esta seção de Comparativo apresenta um estudo mercadológico e
tecnológico contrapondo o projeto JARVIS às soluções comerciais consolidadas. O
capítulo detalha a evolução dos assistentes virtuais, analisando desde pioneiros
como o Amazon Echo e Echo Dot (1ª e 5ª gerações) até ecossistemas focados em
conectividade e privacidade, como o Google Nest Mini e o Apple HomePod Mini. Em
seguida, as especificações técnicas de hardware — como processador,
conectividade, microfones e custo — são consolidadas em uma tabela comparativa
estruturada. Por fim, uma análise crítica avalia os limites computacionais da ESP32
frente aos chips comerciais, destacando o diferencial do JARVIS como um nó de
borda de código aberto, econômico e centrado na privacidade local.

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