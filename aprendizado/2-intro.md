# Introdução



## Modelo Formal – A Estrutura de Aprendizado Estatístico

O processo de aprendizado estatístico pode ser descrito formalmente da seguinte forma:

### Entrada do Modelo
- **Conjunto de domínio** $\mathcal{X}$: O modelo recebe exemplos $(x, y)$ onde $x \in \mathcal{X}$ e $y \in \{0, 1\}$ no caso binário. O objetivo é descobrir uma regra de predição que relacione as entradas às saídas.

- **Conjunto de rótulos** $\mathcal{Y}$: O conjunto $\mathcal{Y}$ representa o espaço dos possíveis valores de saída. Em um problema de classificação binária, $\mathcal{Y}$ é usualmente $\{0, 1\}$ ou $\{-1, +1\}$.

- **Dados de treinamento**: O modelo tem acesso a um conjunto de exemplos de treinamento $S = \{(x_1, y_1), (x_2, y_2), \dots, (x_m, y_m)\}$ onde $x_i \in \mathcal{X}$ e $y_i \in \mathcal{Y}$ são os pares de entradas e saídas. Aqui, $S$ é uma sequência (não necessariamente um conjunto) de pares de exemplos, sendo que o mesmo exemplo pode aparecer mais de uma vez. 

  Formalmente, $S$ é um subconjunto do espaço $\mathcal{X} \times \mathcal{Y}$, e contém $m$ exemplos, ou seja, $S \in (\mathcal{X} \times \mathcal{Y})^m$.

### Saída do Modelo
- **Regra de predição**: O modelo aprende uma função $h : \mathcal{X} \to \mathcal{Y}$, que mapeia os exemplos $x \in \mathcal{X}$ para rótulos $y \in \mathcal{Y}$, tentando minimizar o erro de predição. Essa função também é chamada de hipótese ou classificador. O objetivo é que $h(x)$ seja uma boa aproximação de $y$ para novos exemplos $(x, y)$ não observados durante o treinamento.

### Objetivo
O objetivo é minimizar o **erro de generalização** do modelo, que é a probabilidade de que $h(x) \neq y$ para exemplos $(x, y)$ de uma distribuição desconhecida de dados, $D$. O erro de generalização é dado por:

$$
L_{D}(h) = \mathbb{P}_{x \sim D} \left[ h(x) \neq y \right]
$$

### Medida de Sucesso

A medida de sucesso de um modelo no aprendizado estatístico é avaliada através do **erro de classificação** e da **função de perda**. 

#### Erro de Classificação
- O **erro de classificação** mede a frequência com que o modelo $h$ faz predições incorretas, ou seja, a probabilidade de $h(x) \neq y$. Para uma distribuição de dados $D$ , o erro de classificação de $h$ é dado por:

  $$
  L_{D}(h) = \mathbb{P}_{x \sim D}[h(x) \neq y]
  $$

Este é o **erro esperado** ou **erro de generalização**. O objetivo do modelo é minimizar esse erro, encontrando uma hipótese que tenha um bom desempenho em dados nunca antes vistos.

#### Função de Perda
- A **função de perda** $l(h(x), y)$ mensura o quanto a predição $h(x)$ do modelo se desvia do valor verdadeiro $y$. A função de perda 0-1, por exemplo, é comumente usada em problemas de classificação binária:

  $$
  l(h(x), y) =
  \begin{cases}
  1 & \text{se } h(x) \neq y \\
  0 & \text{se } h(x) = y
  \end{cases}
  $$

Essa função conta o número de erros de predição. Cada predição incorreta resulta em uma penalidade de 1, e cada predição correta resulta em uma penalidade de 0. A soma dessas penalidades sobre um conjunto de dados nos dá o **erro total**.

#### Erro Empírico
- O **erro empírico** ou **erro de treinamento** é a média das perdas calculadas no conjunto de treinamento $S = \{(x_1, y_1), \dots, (x_m, y_m)\}$:

  $$
  L_S(h) = \frac{1}{m} \sum_{i=1}^{m} \mathbf{1}(h(x_i) \neq y_i)
  $$

Aqui, $\mathbf{1}$ é a função indicadora, que vale 1 quando $h(x_i) \neq y_i$ e 0 quando $h(x_i) = y_i$. O erro empírico é utilizado para estimar o desempenho do modelo durante o treinamento.

#### Minimização de Risco Empírico (ERM)
- O modelo que minimiza o erro empírico é chamado de **Empirical Risk Minimizer (ERM)**. O objetivo é encontrar uma hipótese $h_S$ que minimize o erro empírico em $S$. A função de risco empírico é formalmente definida como:

  $$
  h_S = \underset{h \in \mathcal{H}}{\arg\min} \ L_S(h)
  $$

Esse é o processo de minimização de risco empírico, no qual buscamos a hipótese que melhor se ajusta aos dados de treinamento.

#### Overfitting
- Um problema comum ao minimizar o erro empírico é o **overfitting**. Isso ocorre quando o modelo se ajusta muito bem aos dados de treinamento, capturando não apenas o padrão subjacente, mas também o ruído presente nos dados. Um modelo que sofre de overfitting tem um erro empírico muito baixo, mas pode ter um erro de generalização alto, já que não generaliza bem para novos dados.

  Em resumo, um modelo que faz overfitting apresenta um bom desempenho nos dados de treinamento, mas seu desempenho cai significativamente ao ser testado em dados fora da amostra.

## Minimização de Risco Empírico com Viés Indutivo

A minimização de risco empírico (ERM) com viés indutivo implica a incorporação de suposições no processo de escolha de uma hipótese a partir da classe de hipóteses $\mathcal{H}$. Essas suposições, conhecidas como **viés indutivo**, orientam o processo de aprendizado para priorizar hipóteses que possuam certas características desejáveis, como simplicidade ou capacidade de generalização.

### Definição
Na ERM tradicional, o objetivo é encontrar uma hipótese $h_S$ que minimize o erro empírico sobre os dados de treinamento $S = \{(x_1, y_1), \dots, (x_m, y_m)\}$:

$$
h_S = \underset{h \in \mathcal{H}}{\arg\min} \ L_S(h)
$$

Onde o erro empírico $L_S(h)$ é dado por:

$$
L_S(h) = \frac{1}{m} \sum_{i=1}^{m} \mathbf{1}(h(x_i) \neq y_i)
$$

No entanto, sem um viés indutivo, a ERM pode selecionar hipóteses complexas que se ajustam muito bem aos dados de treinamento, mas falham em generalizar para novos dados, levando ao **overfitting**.

### Incorporando o Viés Indutivo
Quando incorporamos o **viés indutivo**, o espaço de hipóteses $\mathcal{H}$ é restringido para priorizar hipóteses com certas propriedades. Essas propriedades podem ser impostas de várias formas, como preferir hipóteses mais simples (Navalha de Occam) ou por meio da adição de regularizações que penalizam a complexidade excessiva do modelo.

O problema de minimização de risco empírico com viés indutivo se torna:

$$
h_S = \underset{h \in \mathcal{H}}{\arg\min} \ L_S(h)
$$

## Classes de Hipóteses Finitas

Uma forma simples de impor restrições sobre uma classe de hipóteses é estabelecer um limite superior para o tamanho da classe, ou seja, o número de preditores $h$ em $\mathcal{H}$. Nesta seção, mostramos que, se $\mathcal{H}$ é uma classe finita, então o ERM sobre $\mathcal{H}$ não apresentará overfitting, desde que a amostra de treinamento seja suficientemente grande (esse tamanho dependerá do tamanho de $\mathcal{H}$).

Limitar o modelo a regras de predição dentro de uma classe finita de hipóteses pode ser considerado uma restrição razoável. 

Agora, vamos analisar o desempenho da regra ERM assumindo que $\mathcal{H}$ é uma classe finita. Para uma amostra de treinamento $S$, rotulada de acordo com uma função $f : \mathcal{X} \to Y$, seja $h_S$ o resultado da aplicação do ERM sobre $S$:

$$
h_S \in \arg \min_{h \in \mathcal{H}} L_S(h).
$$

Nesta análise, assumimos a seguinte simplificação (que será relaxada no próximo capítulo):

**Definição (Hipótese de Realizabilidade):** Existe $h^* \in \mathcal{H}$ tal que $L_{\mathcal{D},f}(h^*) = 0$. Essa suposição implica que, com probabilidade 1, sobre amostras aleatórias $S$ (onde as instâncias de $S$ são amostradas de acordo com $\mathcal{D}$ e rotuladas por $f$), temos $L_S(h^*) = 0$.

A suposição de realizabilidade implica que, para qualquer hipótese ERM, temos que $L_S(h_S) = 0$ (Por que?).   No entanto, estamos interessados no risco verdadeiro de $h_S$, ou seja, $L_{\mathcal{D},f}(h_S)$, em vez de seu risco empírico.

### Hipótese de dados i.i.d.

Qualquer garantia sobre o erro em relação à distribuição subjacente $\mathcal{D}$, para um algoritmo que tem acesso apenas a uma amostra $S$, deve depender da relação entre $\mathcal{D}$ e $S$. A suposição comum em aprendizado de máquina estatístico é que a amostra de treinamento $S$ é gerada amostrando pontos da distribuição $\mathcal{D}$ de forma independente. Formalmente, a suposição i.i.d. (independente e identicamente distribuída) é que os exemplos no conjunto de treinamento são independentes e idênticos de acordo com a distribuição $\mathcal{D}$. Denotamos essa suposição por $S \sim \mathcal{D}^m$, onde $m$ é o tamanho de $S$, e $\mathcal{D}^m$ representa a probabilidade sobre $m$-tuplas induzida pela aplicação de $\mathcal{D}$ a cada elemento de forma independente.

### Parâmetros de Confiança e Precisão

Como o conjunto de treinamento é gerado por um processo aleatório, existe incerteza na escolha do preditor $h_S$ e, consequentemente, no risco $L_{\mathcal{D},f}(h_S)$. A probabilidade de obter uma amostra não representativa é denotada por $\delta$, e $(1 - \delta)$ é o parâmetro de confiança da predição. Introduzimos também um parâmetro de precisão, $\varepsilon$, que define a qualidade da predição. O evento $L_{\mathcal{D},f}(h_S) > \varepsilon$ é considerado uma falha do modelo.

### Nosso objetivo

Queremos mostrar que, se $\mathcal{H}$ é finito, então:

$$
\mathbb{P}_S(L_{\mathcal{D},f}(h_S) > \varepsilon) <\delta ,
$$

lembrando  que $L_S(h_S) = 0$.
