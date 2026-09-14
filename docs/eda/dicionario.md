# Visão Geral e Dicionário de Dados

## Inspeção Inicial do Dataset

O dataset escolhido para este projeto foi o **Stroke Prediction Dataset**, utilizado para estudar fatores associados à ocorrência de AVC.

O conjunto possui **5.110 instâncias** (linhas) e **12 atributos** (colunas), incluindo a variável alvo `stroke`.

```python
import pandas as pd

df = pd.read_csv("healthcare-dataset-stroke-data.csv")

print(f"Dimensões: {df.shape}")
df.head()
```

A inspeção inicial revelou a presença de variáveis numéricas, categóricas e binárias. A coluna `id` possui 5.110 valores distintos e funciona apenas como identificador de cada indivíduo.

## Dicionário de Dados

| Variável | Tipo | Descrição |
|---|---|---|
| `id` | Identificador | Identificador único do indivíduo |
| `gender` | Categórica | Gênero do indivíduo |
| `age` | Numérica | Idade do indivíduo |
| `hypertension` | Binária | Presença de hipertensão (0 = não, 1 = sim) |
| `heart_disease` | Binária | Presença de doença cardíaca (0 = não, 1 = sim) |
| `ever_married` | Categórica | Indica se o indivíduo já foi casado |
| `work_type` | Categórica | Tipo de ocupação |
| `Residence_type` | Categórica | Tipo de residência (urbana ou rural) |
| `avg_glucose_level` | Numérica | Nível médio de glicose no sangue |
| `bmi` | Numérica | Índice de massa corporal |
| `smoking_status` | Categórica | Histórico de tabagismo |
| `stroke` | Binária (Target) | Ocorrência de AVC (0 = não, 1 = sim) |

## Valores Ausentes

Foi realizada uma verificação de valores ausentes no dataset:

```python
df.isnull().sum()
```

A análise identificou **201 valores ausentes na variável `bmi`**, correspondendo a aproximadamente **3,93% das observações**. As demais variáveis não apresentam valores ausentes.

O tratamento desses valores será definido na etapa de pré-processamento, evitando a remoção ou imputação antes da análise das características da variável.

## Inconsistências e Categorias Especiais

As variáveis categóricas foram inspecionadas por meio de suas frequências:

```python
for coluna in [
    "gender",
    "ever_married",
    "work_type",
    "Residence_type",
    "smoking_status"
]:
    print(f"\n{coluna}:")
    print(df[coluna].value_counts())
```

Foram identificados dois pontos que merecem atenção:

- A variável `gender` apresenta a categoria `Other` em apenas **1 das 5.110 observações**, caracterizando uma categoria extremamente rara.
- A variável `smoking_status` contém **1.544 registros classificados como `Unknown`**, indicando que a informação sobre o histórico de tabagismo não está disponível para uma parcela relevante do dataset.

A categoria `Unknown` não é representada pelo Pandas como um valor ausente (`NaN`), mas deve ser considerada durante o pré-processamento por representar informação desconhecida sobre o histórico de tabagismo.

A categoria `Other` em `gender` também não será considerada automaticamente uma inconsistência, pois sua baixa frequência não é suficiente para concluir que o registro seja incorreto.

Neste momento, nenhuma observação será removida ou modificada. As estratégias de tratamento serão definidas posteriormente com base nos resultados da análise exploratória.

## Desbalanceamento da Variável Alvo

A distribuição da variável alvo `stroke` foi analisada em valores absolutos e percentuais:

```python
df["stroke"].value_counts()
df["stroke"].value_counts(normalize=True) * 100
```

Foram encontrados:

| Classe | Quantidade | Percentual |
|---|---:|---:|
| Sem AVC (`0`) | 4.861 | 95,13% |
| Com AVC (`1`) | 249 | 4,87% |

Os resultados evidenciam um **forte desbalanceamento entre as classes**, já que apenas 4,87% das observações correspondem a indivíduos que tiveram AVC.

Esse desbalanceamento deverá ser considerado na etapa de modelagem, pois métricas como a acurácia, quando utilizadas isoladamente, podem fornecer uma avaliação inadequada do desempenho de um classificador.