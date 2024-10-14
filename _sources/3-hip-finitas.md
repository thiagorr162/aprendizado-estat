# Classes de Hipóteses Finitas

Uma forma simples de impor restrições sobre uma classe de hipóteses é estabelecer um limite superior para o tamanho da classe, ou seja, o número de preditores $h$ em $\mathcal{H}$. Nesta seção, mostramos que, se $\mathcal{H}$ é uma classe finita, então o ERM sobre $\mathcal{H}$ não apresentará overfitting, desde que a amostra de treinamento seja suficientemente grande (esse tamanho dependerá do tamanho de $\mathcal{H}$).

Limitar o modelo a regras de predição dentro de uma classe finita de hipóteses pode ser considerado uma restrição razoável. 

Agora, vamos analisar o desempenho da regra ERM assumindo que $\mathcal{H}$ é uma classe finita. Para uma amostra de treinamento $S$, rotulada de acordo com uma função $f : \mathcal{X} \to Y$, seja $h_S$ o resultado da aplicação do ERM sobre $S$:

$$
h_S \in \arg \min_{h \in \mathcal{H}} L_S(h).
$$

Nesta análise, assumimos a seguinte simplificação (que será relaxada no próximo capítulo):

**Definição (Hipótese de Realizabilidade):** Existe $h^* \in \mathcal{H}$ tal que $L_{\mathcal{D},f}(h^*) = 0$. Essa suposição implica que, com probabilidade 1, sobre amostras aleatórias $S$ (onde as instâncias de $S$ são amostradas de acordo com $\mathcal{D}$ e rotuladas por $f$), temos $L_S(h^*) = 0$.

A suposição de realizabilidade implica que, para qualquer hipótese ERM, temos que $L_S(h_S) = 0$ (Por que?).   No entanto, estamos interessados no risco verdadeiro de $h_S$, ou seja, $L_{\mathcal{D},f}(h_S)$, em vez de seu risco empírico.

## Hipótese de dados i.i.d.

Qualquer garantia sobre o erro em relação à distribuição subjacente $\mathcal{D}$, para um algoritmo que tem acesso apenas a uma amostra $S$, deve depender da relação entre $\mathcal{D}$ e $S$. A suposição comum em aprendizado de máquina estatístico é que a amostra de treinamento $S$ é gerada amostrando pontos da distribuição $\mathcal{D}$ de forma independente. Formalmente, a suposição i.i.d. (independente e identicamente distribuída) é que os exemplos no conjunto de treinamento são independentes e idênticos de acordo com a distribuição $\mathcal{D}$. Denotamos essa suposição por $S \sim \mathcal{D}^m$, onde $m$ é o tamanho de $S$, e $\mathcal{D}^m$ representa a probabilidade sobre $m$-tuplas induzida pela aplicação de $\mathcal{D}$ a cada elemento de forma independente.

## Parâmetros de Confiança e Precisão

Como o conjunto de treinamento é gerado por um processo aleatório, existe incerteza na escolha do preditor $h_S$ e, consequentemente, no risco $L_{\mathcal{D},f}(h_S)$. A probabilidade de obter uma amostra não representativa é denotada por $\delta$, e $(1 - \delta)$ é o parâmetro de confiança da predição. Introduzimos também um parâmetro de precisão, $\varepsilon$, que define a qualidade da predição. O evento $L_{\mathcal{D},f}(h_S) > \varepsilon$ é considerado uma falha do modelo.

# Nosso objetivo

Queremos mostrar que, se $\mathcal{H}$ é finito, então:

$$
\mathbb{P}_S(L_{\mathcal{D},f}(h_S) > \varepsilon) <\delta ,
$$

lembrando  que $L_S(h_S) = 0$.
