# Conclusão

## Principais Achados

- O dataset apresenta **forte desbalanceamento da classe alvo**: apenas 4,87% das instâncias correspondem a casos de AVC (`stroke = 1`). Qualquer avaliação de modelo na APS2 deverá considerar isso — acurácia isolada não é uma métrica confiável.
- **Idade é o fator isoladamente mais associado ao AVC** entre as variáveis analisadas: a correlação de Pearson com `stroke` (0,25) é a maior do conjunto, e a mediana de idade dos casos positivos (~70 anos) é bem superior à dos casos negativos (~43 anos), padrão confirmado tanto na análise bivariada quanto na multivariada (idade × gênero × AVC).
- **Hipertensão, doença cardíaca e glicemia elevada** também aumentam a proporção observada de AVC (respectivamente ~3,3x, ~4x e uma associação positiva mais fraca), mas nenhuma variável isolada separa claramente as classes — os boxplots cruzados e o PCA mostram sobreposição considerável entre pacientes com e sem AVC.
- A variável `bmi` possui 201 valores ausentes (~3,9%) e, junto com `avg_glucose_level`, concentra outliers superiores que, por critério clínico, **não devem ser removidos**: representam justamente os pacientes de maior risco (diabéticos/pré-diabéticos, obesidade severa).
- O **PCA sobre as variáveis numéricas** (`age`, `avg_glucose_level`, `bmi`) mostrou que as duas primeiras componentes explicam ~78% da variância, mas a projeção 2D/3D não separa linearmente as classes — reforçando que o risco de AVC resulta da combinação de múltiplos fatores (idade avançada + comorbidades metabólicas), não de uma única variável.

## Estratégias Definidas para a Modelagem (APS2)

Com base nesses achados, o pré-processamento adotado (ver [Pipeline de Pré-processamento](eda/pipeline.md)) foi:

| Etapa | Estratégia | Justificativa resumida |
|---|---|---|
| Imputação (`bmi`) | `SimpleImputer` por mediana | Distribuição assimétrica e com outliers; mediana é robusta |
| Encoding (categóricas nominais) | `OneHotEncoder` | Evita ordinalidade artificial entre categorias sem ordem natural |
| Escalonamento (numéricas) | `StandardScaler` | Variáveis em escalas muito diferentes; `RobustScaler` documentado como alternativa dado os outliers legítimos |
| Outliers | Não remover | Outliers em `avg_glucose_level`/`bmi` representam risco clínico real, não erro de medição |
| Separação treino/teste | `train_test_split` estratificado, `random_state=42` | Preserva a proporção da classe minoritária (~4,9%) em treino e teste |

O `ColumnTransformer`/`Pipeline` foi ajustado exclusivamente no conjunto de treino (verificação explícita de ausência de *data leakage*) e exportado (`artifacts/stroke_preprocessing_pipeline.joblib`) para reutilização direta na APS2.

## Próximos Passos (APS2)

1. Carregar o pipeline de pré-processamento já ajustado e anexar um estimador de classificação.
2. Avaliar diferentes modelos (ex.: regressão logística com `class_weight="balanced"`, árvores/ensembles) com validação cruzada estratificada.
3. Priorizar métricas adequadas ao desbalanceamento de classes — recall, F1 e AUC-PR — em vez de acurácia simples.
4. Considerar técnicas de balanceamento (ex.: SMOTE, ajuste de `class_weight`) dado que a classe positiva representa menos de 5% das instâncias.
5. Testar `RobustScaler` como alternativa ao `StandardScaler`, comparando o impacto nos modelos sensíveis a outliers.
