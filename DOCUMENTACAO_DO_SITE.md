# Harmonia Áurea — análise e funcionamento do site

## 1. Visão geral

**Harmonia Áurea** é um site educativo e interativo que aproxima matemática, música e arquitetura. Sua proposta é permitir que o visitante **ouça uma interpretação artística da proporção áurea** e compare esse resultado com uma sequência de notas gerada aleatoriamente.

O projeto foi pensado para acompanhar um trabalho sobre o segmento áureo e a Sala São Paulo. Em vez de apresentar apenas fórmulas, ele transforma conceitos matemáticos em três experiências:

- **visual**, por meio de espirais, proporções, ritmo gráfico e uma estética inspirada em uma sala de concertos;
- **sonora**, por meio de notas sintetizadas no navegador;
- **interativa**, permitindo tocar, pausar, comparar sequências e alterar andamento e volume.

O site é formado apenas por HTML, CSS e JavaScript.

## 2. Organização da página

### 2.1. Abertura

A primeira seção apresenta a pergunta central: **“Existe uma proporção capaz de soar bonita?”**. Ao lado do texto, um `canvas` desenha uma espiral construída com o número de ouro, enquanto o valor aproximado de φ, **1,618**, fica em destaque.

Essa abertura não afirma que existe uma fórmula universal para a beleza. Ela convida o visitante a realizar uma comparação e formar sua própria percepção.

### 2.2. Laboratório sonoro

Esta é a parte principal do site. O visitante pode escolher entre dois modos:

- **Sequência áurea:** usa uma lista baseada em números de Fibonacci e calcula as frequências com potências de φ;
- **Sequência aleatória:** sorteia 16 valores sem usar a regra áurea e os converte em frequências distribuídas entre 180 Hz e 880 Hz.

O painel mostra a nota atual, sua frequência, o número do passo, a forma de onda utilizada e uma visualização animada. Também existem controles de reprodução, andamento e volume.

### 2.3. Explicação matemática

A seção “Por trás do som” resume o processo em três etapas:

1. parte da sequência de Fibonacci;
2. converte valores numéricos em frequências;
3. representa graficamente a intensidade das frequências.

O próprio texto do site ressalta um cuidado importante: o experimento **não prova que φ produz uma escala musical perfeita**. Trata-se de uma maneira artística de organizar sons usando uma regra matemática.

### 2.4. Relação com a Sala São Paulo

A página estabelece uma conexão conceitual com a Sala São Paulo. Elementos como arcos, colunas, vãos, ornamentos, repetição e simetria são apresentados como formas de **ritmo visual**, enquanto a música cria um **ritmo sonoro**.

Essa parte funciona principalmente como contextualização para a visita e para o tema do trabalho. O site não apresenta medições arquitetônicas que demonstrem que a Sala São Paulo foi projetada segundo φ; portanto, a relação mostrada é visual e educativa, não uma comprovação histórica ou arquitetônica.

### 2.5. Encerramento

Ao final, o visitante é convidado a responder qual sequência pareceu mais harmoniosa e pode voltar ao experimento. Isso reforça o caráter investigativo da página: a conclusão depende da escuta e da comparação.

## 3. O que é o segmento áureo

Considere um segmento dividido em uma parte maior, de comprimento **a**, e uma parte menor, de comprimento **b**. A divisão é áurea quando a razão entre o segmento inteiro e a parte maior é igual à razão entre a parte maior e a parte menor:

```text
(a + b) / a = a / b = φ
```

Essa razão é representada pela letra grega φ (phi) e vale:

```text
φ = (1 + √5) / 2 ≈ 1,6180339887
```

Se a parte menor medir 1 unidade, a maior medirá aproximadamente 1,618, e o segmento inteiro medirá aproximadamente 2,618.

Uma propriedade importante é:

```text
φ² = φ + 1
```

Essa característica explica a repetição da mesma proporção em construções geométricas, como o retângulo áureo: ao retirar um quadrado de um retângulo áureo, o retângulo restante conserva a mesma proporção.

## 4. Relação entre o segmento áureo e Fibonacci

A sequência de Fibonacci começa, neste projeto, assim:

```text
1, 1, 2, 3, 5, 8, 13, 21, 34, 55...
```

Cada termo é a soma dos dois anteriores. Quando dividimos um termo pelo termo anterior, os resultados se aproximam de φ:

| Divisão | Resultado aproximado |
|---|---:|
| 2 ÷ 1 | 2,000 |
| 3 ÷ 2 | 1,500 |
| 5 ÷ 3 | 1,667 |
| 8 ÷ 5 | 1,600 |
| 13 ÷ 8 | 1,625 |
| 21 ÷ 13 | 1,615 |
| 34 ÷ 21 | 1,619 |

Quanto maiores os termos consecutivos, mais a razão se aproxima de **1,618**. Essa convergência é a ligação matemática direta entre Fibonacci e o segmento áureo.

No site, os números de Fibonacci não são reproduzidos como frequências diretamente. Eles são usados como **passos** que alimentam outra fórmula baseada em φ.

## 5. Como o som é produzido

### 5.1. Inicialização do áudio

O áudio é criado pela **Web Audio API**. Ela só é inicializada depois que o usuário pressiona o botão de reprodução, comportamento necessário porque navegadores modernos normalmente bloqueiam áudio automático.

O circuito de áudio é:

```text
oscilador triangular → filtro passa-baixas → envelope de volume
→ volume principal → analisador → alto-falantes/fones
```

- O **oscilador** gera a nota.
- O **filtro passa-baixas**, configurado em 1900 Hz, suaviza frequências mais altas.
- O **envelope** faz a nota crescer rapidamente e desaparecer em cerca de 0,42 segundo.
- O **analisador** mede a energia das frequências para alimentar a animação.

### 5.2. Sequência áurea

O código usa 16 passos:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 34, 21, 13, 8, 5
```

Os valores sobem segundo Fibonacci e depois retornam, criando uma frase musical simétrica. Para cada passo `n`, a frequência inicial é calculada por:

```text
f = 220 × φ^((n mod 36) / 6)
```

Depois, o programa transpõe o resultado por oitavas:

- se a frequência passar de 880 Hz, divide por 2 até voltar à faixa escolhida;
- se ficar abaixo de 180 Hz, multiplica por 2.

O número **220 Hz** corresponde à nota Lá 3 no padrão de afinação mais comum. O uso de expoentes fracionários faz cada passo multiplicar a frequência por uma fração constante de φ. Os números de Fibonacci determinam quais desses passos são selecionados.

### 5.3. Sequência aleatória

Ao entrar no modo aleatório, o programa sorteia 16 números inteiros entre 0 e 35. Cada número é transformado linearmente em uma frequência:

```text
f = 180 + (n / 36) × 700
```

Isso distribui as notas aproximadamente entre 180 Hz e 861 Hz. Uma nova sequência é sorteada sempre que o usuário seleciona novamente esse modo.

### 5.4. Ritmo

O intervalo entre notas é calculado por:

```text
intervalo = 60 / BPM / 2
```

Em 92 BPM, uma nova nota é programada aproximadamente a cada **0,326 segundo**. O agendador verifica o relógio do sistema de áudio a cada 25 milissegundos e agenda antecipadamente as próximas notas, reduzindo falhas de sincronização.

O controle de andamento aceita valores de 60 a 140 BPM. O volume visual varia de 0% a 100%, mas o ganho máximo real é limitado a 35% para evitar um som excessivamente forte.

## 6. Como as animações funcionam

O projeto utiliza dois elementos HTML `canvas`.

### 6.1. Espiral da abertura

A espiral da primeira tela usa uma equação polar exponencial:

```text
r = 5 × φ^(θ / (π/2))
```

Assim, a cada quarto de volta, o raio é multiplicado por φ. Isso produz uma **espiral áurea aproximada**. Ela não é exatamente a conhecida ilustração formada por arcos de quarto de círculo dentro de quadrados de Fibonacci, embora represente a mesma ideia de crescimento proporcional.

### 6.2. Visualizador do experimento

No modo áureo, o analisador de áudio fornece 128 valores de intensidade. Esses valores alteram a transparência e a espessura do traço ao longo de outra espiral baseada em φ. O resultado é uma linha dourada que reage ao som.

No modo aleatório, a espiral é substituída por 70 pontos distribuídos com um incremento angular de aproximadamente **2,399 radianos**, valor próximo do chamado ângulo áureo. A energia do áudio altera a posição, o tamanho e a opacidade dos pontos.

O desenho é atualizado continuamente com `requestAnimationFrame`, acompanhando a taxa de atualização da tela.

## 7. Interface e identidade visual

O design usa uma paleta de preto, creme e dourado. O dourado (`#c99a38`) representa visualmente o tema áureo, enquanto a combinação de fontes serifadas, sem serifa e monoespaçadas separa títulos, textos e dados técnicos.

A composição alterna grandes blocos claros e escuros, linhas finas, arcos e divisões em colunas. Essas escolhas criam repetição e ritmo visual, coerentes com os temas de música, arquitetura e proporção.

O layout é responsivo:

- acima de 900 px, as principais seções usam duas ou três colunas;
- abaixo de 900 px, conteúdo, sintetizador e explicações passam para uma coluna;
- abaixo de 560 px, espaçamentos e títulos são reduzidos, o menu é simplificado e alguns elementos secundários são ocultados.

## 8. Estrutura técnica dos arquivos

| Arquivo | Função |
|---|---|
| `index.html` | Define o conteúdo, as seções, os botões e os elementos de acessibilidade. |
| `styles.css` | Controla cores, tipografia, composição, estados dos botões e responsividade. |
| `app.js` | Calcula φ e as frequências, sintetiza o áudio, controla a interação e desenha as animações. |
| `README.md` | Apresenta instruções resumidas para executar o projeto. |

O site não coleta dados do visitante. A única dependência externa visível são as fontes carregadas do Google Fonts. Se não houver internet, a página continua funcionando, mas usará fontes alternativas do sistema.

## 9. Fluxo de uso

1. O visitante abre a página e conhece a pergunta do experimento.
2. Pressiona “Iniciar experimento” para navegar até o laboratório.
3. Escolhe a sequência áurea ou a aleatória.
4. Pressiona o botão de reprodução, inicializando o sistema de áudio.
5. Escuta as notas e observa a animação reagir às frequências.
6. Pode trocar de modo durante a reprodução; a próxima nota já usa o novo modo.
7. Ajusta andamento e volume conforme desejar.
8. Lê a explicação matemática e a relação proposta com a Sala São Paulo.
9. Compara subjetivamente os dois resultados e decide qual lhe parece mais harmonioso.

## 10. Limites e observações sobre a implementação atual

Alguns detalhes devem ser considerados ao explicar ou evoluir o projeto:

- **A experiência é artística, não uma demonstração de que φ gera beleza ou consonância.** A percepção de harmonia depende também de cultura, afinação, timbre, ritmo e experiência individual.
- A fórmula apresentada na página, `fₙ = 220 × φ^(n/6)`, é uma versão simplificada. No código existe também `n mod 36`, além do ajuste por oitavas para manter o som em uma faixa limitada.
- Os passos de Fibonacci são usados como índices de uma transformação. Portanto, o resultado não é uma escala tradicional baseada apenas em Fibonacci nem uma afinação musical convencional de 12 semitons iguais.
- O mostrador visual indica duração total de **24 segundos**, mas a reprodução não para nesse ponto. A sequência de 16 notas se repete indefinidamente e a barra de progresso reinicia a cada 24 segundos.
- Ao pausar e tocar novamente, o contador visual de tempo volta a zero, mas o passo musical continua de onde parou.
- O botão “Áudio pronto” informa o estado, mas não controla o áudio; o controle verdadeiro é o botão circular de reprodução.
- A troca de modo afeta a nota seguinte agendada. Como o programador prepara notas até 0,1 segundo antes, em situações específicas uma nota já agendada ainda pode pertencer ao modo anterior.
- A associação da Sala São Paulo com φ é apresentada como analogia entre proporção, repetição, arquitetura e música. Para afirmar uma aplicação arquitetônica exata do segmento áureo, seriam necessárias plantas, medidas e fontes históricas.

## 11. Síntese da relação com o segmento áureo

A presença do segmento áureo no site acontece em diferentes níveis:

| Nível | Aplicação |
|---|---|
| Matemático | Definição de `φ = (1 + √5) / 2`. |
| Sequencial | Uso de termos de Fibonacci, cujas razões consecutivas convergem para φ. |
| Sonoro | Cálculo de frequências por potências fracionárias de φ. |
| Visual | Crescimento do raio das espirais segundo φ e uso de um ângulo próximo do ângulo áureo. |
| Estético | Cor dourada, repetição, equilíbrio e ritmo na composição da página. |
| Conceitual | Comparação entre ordem matemática, ritmo musical e organização arquitetônica. |

Em resumo, o site não tenta provar que a proporção áurea é uma “fórmula da beleza”. Ele usa o segmento áureo como **regra de organização** e transforma essa regra em uma experiência que pode ser vista, ouvida, comparada e discutida.
