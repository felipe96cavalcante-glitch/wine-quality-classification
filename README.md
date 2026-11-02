# Classificação da Qualidade de Vinhos com Machine Learning

Projeto desenvolvido para o Tech Challenge da Pós Tech FIAP - Data Analytics.

## Objetivo

Analisar as características físico-químicas de amostras de vinho e identificar padrões associados aos diferentes níveis de qualidade.

A variável `quality` foi utilizada inicialmente para compreender as diferenças entre os vinhos e posteriormente transformada em uma classificação binária para o desenvolvimento dos modelos de Machine Learning.

- **Alta Qualidade:** `quality >= 7`
- **Baixa/Média Qualidade:** `quality < 7`

## Pergunta de negócio

> Quais características físico-químicas estão mais associadas aos vinhos de alta qualidade e é possível identificar padrões que diferenciem vinhos classificados como nota 6 daqueles classificados como nota 7 ou superior?

## Base de dados

O projeto utiliza o dataset Wine Quality.

A base contém:

- 1.143 amostras de vinho;
- 11 características físico-químicas;
- 1 variável de qualidade (`quality`);
- 1 identificador (`Id`).

### Variáveis analisadas

- fixed acidity
- volatile acidity
- citric acid
- residual sugar
- chlorides
- free sulfur dioxide
- total sulfur dioxide
- density
- pH
- sulphates
- alcohol

## Análise exploratória

A análise exploratória foi realizada com o objetivo de:

- compreender a distribuição das notas;
- identificar a concentração de amostras por nível de qualidade;
- comparar as características físico-químicas entre diferentes notas;
- identificar possíveis padrões associados à alta qualidade;
- avaliar a distribuição e a variabilidade das características;
- investigar especialmente as diferenças entre vinhos nota 6 e nota 7;
- analisar possíveis valores extremos por meio do critério de 1,5 × IQR.

A análise mostrou que as amostras estão fortemente concentradas nas notas 5 e 6.

As classes utilizadas no Machine Learning ficaram distribuídas da seguinte forma:

- **Baixa/Média Qualidade:** 984 amostras (86,1%);
- **Alta Qualidade:** 159 amostras (13,9%).

Essa diferença entre as classes foi considerada durante a modelagem.

## Principais achados da análise exploratória

A comparação entre as notas 6 e 7 mostrou diferenças relevantes em algumas características físico-químicas.

Entre os principais destaques estão:

- **Ácido cítrico:** média aproximadamente 46,6% maior na nota 7;
- **Acidez volátil:** média aproximadamente 22,0% menor na nota 7;
- **Cloretos:** média aproximadamente 11,8% menor na nota 7;
- **Sulfatos:** média aproximadamente 9,9% maior na nota 7;
- **Álcool:** média aproximadamente 7,7% maior na nota 7.

A análise de correlação também indicou associações relevantes entre `quality` e características como álcool, sulfatos, ácido cítrico e acidez volátil.

Essas relações representam associações observadas nos dados e não devem ser interpretadas como relações de causalidade.

## Machine Learning

A variável `quality` foi transformada em uma classificação binária:

| Classe | Definição |
|---|---|
| 0 | Baixa/Média Qualidade (`quality < 7`) |
| 1 | Alta Qualidade (`quality >= 7`) |

Os dados foram divididos em conjuntos de treinamento e teste utilizando estratificação para preservar a proporção das classes.

As características foram padronizadas utilizando `StandardScaler`, com o ajuste realizado exclusivamente sobre os dados de treinamento.

## Modelos avaliados

Foram avaliados três modelos de classificação:

- KNN;
- SVM;
- SVM com balanceamento das classes.

O balanceamento do SVM foi utilizado devido à diferença significativa entre a quantidade de amostras das duas classes.

## Avaliação dos modelos

Inicialmente, os modelos foram avaliados no conjunto de teste:

| Modelo | Acurácia | Precisão - Alta Qualidade | Recall - Alta Qualidade | F1-score - Alta Qualidade | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| KNN | 86,5% | 51,9% | 43,8% | 47,5% | 83,4% |
| SVM | 90,0% | 76,5% | 40,6% | 53,1% | 85,0% |
| SVM Balanceado | 81,7% | 41,1% | 71,9% | 52,3% | 87,0% |

Como a classe Alta Qualidade representa apenas 13,9% das amostras, a acurácia isoladamente não foi utilizada como principal critério de seleção.

## Validação cruzada

Para verificar a consistência dos resultados, foi utilizada validação cruzada estratificada com 5 folds sobre o conjunto de treinamento.

| Modelo | Acurácia média | Precisão média | Recall médio | F1 médio | ROC-AUC médio |
|---|---:|---:|---:|---:|---:|
| KNN | 87,20% | 56,39% | 40,25% | 46,67% | 83,31% |
| SVM | 88,19% | 65,95% | 32,34% | 43,31% | 85,37% |
| SVM Balanceado | 80,75% | 40,36% | **78,83%** | **53,29%** | **88,85%** |

A validação cruzada confirmou o melhor desempenho do SVM Balanceado para a identificação da classe Alta Qualidade, apresentando o maior recall, F1-score e ROC-AUC entre os modelos avaliados.

## Modelo selecionado

O **SVM com balanceamento das classes** foi selecionado como modelo final.

Na validação cruzada, o modelo apresentou:

- **Recall médio de 78,83%**;
- **F1-score médio de 53,29%**;
- **ROC-AUC médio de 88,85%**.

No conjunto de teste, o modelo identificou corretamente:

- **23 dos 32 vinhos de Alta Qualidade**;
- **Recall de 71,9%**;
- **ROC-AUC de 87,0%**.

A escolha prioriza a capacidade de identificar a classe minoritária de Alta Qualidade, mesmo com redução da acurácia geral e aumento de falsos positivos.

## Variáveis mais relevantes

A análise de importância por permutação aplicada ao modelo final indicou maior relevância das seguintes variáveis:

1. alcohol
2. sulphates
3. total sulfur dioxide
4. density
5. chlorides
6. volatile acidity

A variável `alcohol` apresentou a maior importância média na análise realizada.

A importância por permutação indica o impacto da variável no desempenho do modelo quando seus valores são embaralhados. Ela não deve ser interpretada como causalidade.

## Tratamento dos possíveis outliers

A identificação de possíveis outliers foi realizada utilizando o critério de 1,5 × IQR.

Foram identificados valores extremos em diferentes características físico-químicas. Entretanto, a identificação estatística de um outlier não significa necessariamente que o registro seja incorreto.

Como não foram identificadas evidências suficientes de erros ou inconsistências nos dados, os possíveis outliers foram mantidos para preservar a variabilidade original do conjunto de dados.

Dessa forma, não foi realizada a remoção automática dos outliers nesta etapa.

## Implicações para produção

Os resultados indicam que as características físico-químicas analisadas podem apoiar a identificação de amostras com maior probabilidade de pertencer à classe Alta Qualidade.

O SVM com balanceamento das classes apresentou maior capacidade de identificar essa classe, com recall médio de 78,83% na validação cruzada e 71,9% no conjunto de teste.

Em um cenário de produção, o modelo poderia ser utilizado como ferramenta de apoio à triagem, sinalizando amostras para análises ou avaliações adicionais.

O modelo não deve substituir avaliações técnicas ou sensoriais. Os resultados representam padrões observados no conjunto de dados e não permitem afirmar que uma característica físico-química isolada cause aumento da qualidade.

Além disso, a utilização prática deve considerar o custo associado aos falsos positivos e às análises adicionais das amostras sinalizadas.

## Estrutura do projeto

```text
wine-quality-classification/
│
├── data/
│   └── WineQT.csv
│
├── notebooks/
│   └── 01_analise_wine_quality.ipynb
│
├── src/
│   ├── preprocessing.py
│   └── models.py
│
├── results/
│
├── requirements.txt
├── README.md
└── .gitignore
```

## Tecnologias utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Jupyter Notebook

## Como executar

Clone o repositório e instale as dependências:

```bash
pip install -r requirements.txt
```

Em seguida, abra o notebook:

```text
notebooks/01_analise_wine_quality.ipynb
```

Execute as células em ordem para reproduzir a análise exploratória, o pré-processamento, o treinamento, a validação, a avaliação e a interpretação dos modelos.