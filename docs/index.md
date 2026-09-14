# Predição de AVC — Análise Exploratória de Dados

## Projeto de Machine Learning

Este projeto realiza uma **Análise Exploratória de Dados (EDA)** do **Stroke Prediction Dataset**, com o objetivo de compreender as características do conjunto de dados e investigar possíveis relações entre fatores demográficos e clínicos e a ocorrência de acidente vascular cerebral (AVC).

A análise corresponde à primeira etapa do projeto de Machine Learning e servirá como base para as etapas posteriores de pré-processamento e construção de modelos de classificação.

## Integrantes

- **Nome do integrante 1** Erick Barbosa
- **Nome do integrante 2** Rafael Lemos

## Informações do Projeto

**Disciplina:** Machine Learning
**Instituição:** Insper  
**Data de entrega:** 14/09/2026  
**Dataset:** Stroke Prediction Dataset  
**Variável alvo:** `stroke`

## Estrutura da Análise

A análise exploratória foi organizada nas seguintes etapas:

1. **Visão Geral e Dicionário de Dados** — inspeção inicial, descrição das variáveis, valores ausentes, inconsistências e desbalanceamento da variável alvo.
2. **Análise Univariada** — estatísticas descritivas e análise individual das principais variáveis numéricas e categóricas.
3. **Análise Bivariada** — correlações entre variáveis numéricas e relações entre variáveis categóricas e a variável alvo.
4. **Análise Multivariada e Outliers** — cruzamento de variáveis numéricas, categóricas e o target, e diagnóstico formal de outliers (IQR/Tukey).
5. **Redução de Dimensionalidade (PCA)** — padronização, variância explicada e visualização da separabilidade das classes.
6. **Pipeline de Pré-processamento** — justificativas de imputação, encoding e escalonamento, e construção do `ColumnTransformer`/`Pipeline` do scikit-learn, sem *data leakage*.
7. **Conclusão** — síntese dos principais achados e das estratégias propostas para a etapa de modelagem (APS2).

## Divisão do Trabalho

- **Membro 1 — Erick Barbosa:** Visão Geral e Dicionário de Dados, Análise Univariada e Análise Bivariada.
- **Membro 2 — Rafael Lemos:** Análise Multivariada e Diagnóstico de Outliers, PCA, Pipeline de Pré-processamento e Conclusão.

## Notebook Reprodutível

Além deste site, o notebook completo (`.ipynb`) com todo o código executado está disponível no repositório do projeto: [`APS1_Stroke_Parte2_Membro2.ipynb`](https://github.com/erick-ins25/projeto-ml/blob/main/APS1_Stroke_Parte2_Membro2.ipynb).

## Dataset

O conjunto de dados utilizado é o **Stroke Prediction Dataset**, disponibilizado publicamente no Kaggle.

O dataset contém **5.110 observações e 12 atributos**, reunindo informações demográficas e clínicas dos indivíduos.

## Referências

1. FEDESORIANO. *Stroke Prediction Dataset*. Kaggle, 2021. Disponível em: <https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset>
2. PEDREGOSA, F. et al. *Scikit-learn: Machine Learning in Python*. JMLR 12, pp. 2825-2830, 2011. Disponível em: <https://scikit-learn.org/stable/>
3. JOLLIFFE, I. T.; CADIMA, J. *Principal component analysis: a review and recent developments*. Phil. Trans. R. Soc. A, 2016.
4. TUKEY, J. W. *Exploratory Data Analysis*. Addison-Wesley, 1977. (Método do intervalo interquartil — IQR — para detecção de outliers.)