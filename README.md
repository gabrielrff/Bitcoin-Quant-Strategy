# Estratégia Quantitativa de Trading com XGBoost e Análise de Sentimento (BTC/USD)

## Visão Geral do Projeto

Este projeto implementa uma robusta estratégia quantitativa de trading focada no par **BTC/USD**, utilizando o modelo de Machine Learning **XGBoost**.

O diferencial desta estratégia reside na inclusão de *features* de **Análise de Sentimento** (extraídas de Twitter/X, Reddit e Fear & Greed Index) em conjunto com indicadores de Análise Técnica (como Médias Móveis, Volatilidade e Variação de Preço).

O objetivo é provar que o sentimento do mercado é um **fator chave para a geração de Alpha** (retorno acima do mercado) e para a **gestão de risco** no trading de criptomoedas, especialmente em mercados desafiadores.

##  Resultados de Destaque

O backtest *out-of-sample* (20%) foi realizado em um período de **Bear Market** (ano de 2022). A performance da estratégia principal superou todos os *benchmarks*, conforme a tabela consolidada de métricas financeiras:

| Estratégia | Capital Final | Retorno Total (%) | Sharpe Ratio (Anualizado) | Max Drawdown (%) |
| :--- | :--- | :--- | :--- | :--- |
| **XGBoost (Completo)** | **126.462,73** | **26,46** | **2,64** | **-14,03** |
| Buy & Hold | 86.627,06 | -13,37 | -1,21 | -27,11 |
| Momentum Simples | 101.742,86 | 1,74 | 0,43 | -27,44 |
| XGBoost (Apenas Técnico) | 73.312,92 | -26,69 | -2,92 | -30,28 |

### Análise de Performance e Risco

* **Alpha Gerado:** A estratégia XGBoost completo gerou um retorno positivo de **+26,46%** em um ano em que o ativo subjacente (BTC) perdeu **-13,37%**.
* **Gestão de Risco (Sharpe Ratio):** O Sharpe Ratio de **2,64** é considerado excepcional, indicando que os retornos foram muito superiores ao risco assumido, ao contrário dos *benchmarks*.
* **Proteção de Capital (Max Drawdown):** O modelo reduziu a perda máxima (Drawdown) para **-14,03%**, menos da metade do Drawdown de ambos os *benchmarks* (aproximadamente $-27\%$).

### 🧠 Valor da Análise de Sentimento (Alpha Isolado)

A comparação direta demonstra que o sentimento foi o fator decisivo para a geração de Alpha e para a gestão de risco.

| Métrica | XGBoost (Completo) | XGBoost (Apenas Técnico) | VALOR DO SENTIMENTO |
| :--- | :--- | :--- | :--- |
| Retorno Total (%) | 26,46% | -26,69% | **Geração de +53,15 pontos de Alpha.** |
| Risco Ajustado (Sharpe) | 2,64 | -2,92 | **Transformação de risco extremo em eficiência.** |
| Max Drawdown (%) | -14,03% | -30,28% | **Redução do risco de perda em mais de 50%.** |

## ⚙️ Arquitetura e Engenharia de Features

1.  **Dados e Frequência:** O projeto utiliza dados de preço (OHLCV) em frequência de 1 minuto, reamostrados para a frequência **horária**.
2.  **Target (`Y_Class`):** A variável alvo é definida como $-1, 0,$ ou $1$ com base no retorno percentual futuro após **3 horas**. Um **`THRESHOLD_PCT` de $0.001\%$** é utilizado para classificar o movimento como significativo (longo, neutro ou curto).
3.  **Features de Sentimento:**
    * **Fontes:** Dados de sentimento do Reddit, Twitter e Fear & Greed Index.
    * **Tratamento:** Criação de *features* de *lag* (atraso) e Médias Móveis (MA) para capturar a dinâmica do sentimento ao longo do tempo.
4.  **Treinamento:**
    * O *Data Leakage* é prevenido através de uma **divisão temporal** (80% treino, 20% teste).
    * A **Padronização (`StandardScaler`)** é aplicada apenas nos dados de treino.
    * A técnica de **`Sample Weights`** foi aplicada durante o treinamento do XGBoost para lidar com o desbalanceamento natural da variável *target*.

## 🤝 Como Reproduzir o Projeto

Para executar o pipeline e as análises, você precisará do Python e das bibliotecas listadas abaixo.

### Pré-requisitos

* Python 3.x
* Bibliotecas: `pandas`, `numpy`, `matplotlib`, `seaborn`, `xgboost`, `scikit-learn`, `requests`

### Estrutura de Arquivos

- Certifique-se de que os arquivos de dados (não inclusos neste README) estejam presentes na estrutura de pastas esperada pelo notebook, conforme as chamadas de `pd.read_csv` na Célula 2 e Célula 4:
- **OBS:** Os dados de sentimento e preço do bitcoin podem ser baixado através desse link [Dados sentimento e preço](https://drive.google.com/drive/folders/1chP4z_y3jJ0Jwb5B4yvoP-sOw-Osj9IU?usp=sharing).
