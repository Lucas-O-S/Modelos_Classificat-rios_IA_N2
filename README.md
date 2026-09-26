# Modelos_Classificatorios_IA_N2

Trabalho da Avaliação N2 (1º Bimestre) da disciplina de Inteligência Artificial, do Prof. Marcones Cleber Brito da Silva.

**Exercício id=40922**

## Sobre o projeto

O dataset reúne informações de 7.043 clientes de uma empresa de telecomunicações, a telco, com o objetivo de prever o cancelamento de contrato (*churn*) e apoiar estratégias de retenção. O notebook percorre todo o pipeline de um problema de classificação binária: carga e limpeza dos dados, análise exploratória, preparação para treino, treinamento de múltiplos classificadores e avaliação comparativa de desempenho.

## Dataset

- **Fonte:** [Telco Customer Churn](https://raw.githubusercontent.com/Lucas-O-S/Modelos_Classificat-rios_IA_N2/main/Telco%20Customer%20Churn.csv)
- **Dimensões:** 7.043 linhas × 21 colunas
- **Qualidade:** sem valores ausentes nativos e sem linhas duplicadas. Uma inconsistência foi identificada e corrigida durante a limpeza: a coluna `TotalCharges` estava armazenada como texto, e 11 registros (todos com `tenure = 0`, ou seja, clientes recém-contratados sem fatura fechada) ficaram `NaN` ao converter para número — foram preenchidos com 0.
- **Target:** `Churn` (`Yes`/`No`), com desbalanceamento moderado: 73,46% não cancelaram vs. 26,54% cancelaram.

## Como executar

1. Abrir o notebook `Telco_Customer_Churn.ipynb` no Google Colab.
2. Executar as células em ordem, de cima para baixo (**Ambiente de execução → Executar tudo**). O dataset é carregado direto do GitHub, sem necessidade de upload manual.
3. Todas as bibliotecas usadas (`pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`) já vêm pré-instaladas no Colab.

## Pipeline

1. **Carregamento dos dados** direto do repositório do GitHub.
2. **Explicação das features e do target**, agrupadas em identificador, atributos categóricos (perfil do cliente e serviços contratados), atributos numéricos e o target `Churn`.
3. **Análise exploratória (EDA):** checagem de dados ausentes/duplicados, correção do `TotalCharges`, distribuição do target, histogramas e boxplots por Churn, correlação entre `tenure` e `TotalCharges` (r = 0,826), ranking de atributos categóricos por amplitude da taxa de churn e investigação dirigida por hipóteses sobre o motivo do maior churn em clientes de fibra óptica.
4. **Separação treino/teste** estratificada (80/20), preservando a proporção do target em ambos os conjuntos (5.634 / 1.409 linhas, 26,54% de churn nos dois).
5. **Engenharia de features:** criação de `fibra_sem_suporte`, justificada pela investigação de hipóteses (item 3).
6. **Pré-processamento:** `ColumnTransformer` com `StandardScaler` nas variáveis numéricas e `OneHotEncoder(handle_unknown='ignore')` nas categóricas.
7. **Treinamento de 6 classificadores:** Regressão Logística, Árvore de Decisão (profundidade 6), Naive Bayes, KNN (k=15), Perceptron e MLP (rede neural).
8. **Avaliação:** acurácia, precisão, recall, F1-score e matriz de confusão para cada modelo.
9. **Conclusão** com interpretação dos resultados e dos achados de negócio.

## Principais resultados

| Modelo | Acurácia | Precisão | Recall | F1 |
|---|---|---|---|---|
| Regressão Logística | 0,8048 | 0,6542 | 0,5615 | **0,6043** |
| Árvore de Decisão | 0,7991 | 0,6391 | 0,5588 | 0,5963 |
| MLP (rede neural) | 0,7935 | 0,6361 | 0,5187 | 0,5714 |
| KNN | 0,7842 | 0,6006 | 0,5588 | 0,5789 |
| Naive Bayes | 0,6906 | 0,4543 | 0,8235 | 0,5856 |
| Perceptron | 0,7268 | 0,4747 | 0,2754 | 0,3486 |

A **Regressão Logística** teve o melhor equilíbrio entre as métricas (maior F1-score), sendo o modelo recomendado para este problema.

Achados de negócio mais relevantes da EDA:
- `Contract` é o atributo que mais separa quem cancela de quem não cancela (amplitude de 0,399 na taxa de churn entre categorias).
- Dentro do grupo de clientes com internet fibra óptica, ter suporte técnico contratado reduz o churn quase pela metade (49,4% sem suporte vs. 22,6% com suporte) — indicando que parte do problema é qualidade de serviço não resolvida, não só o custo da fibra.

## Autores

- Adriana Kaori Kakazu — 082220004
- Beatriz dos Santos Buglio — 082220028
- Lucas Oliveira Silva — 082220019
- Vitoria Kaori Kuriyama — 082220005
