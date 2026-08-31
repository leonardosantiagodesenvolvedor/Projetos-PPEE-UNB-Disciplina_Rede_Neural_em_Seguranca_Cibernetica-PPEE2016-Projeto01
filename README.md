# Identificação de Ativos IoT por Tráfego de Rede utilizando Redes Neurais Artificiais

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-orange.svg)](https://colab.research.google.com/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-Keras-orange.svg)](https://www.tensorflow.org/)
[![Scikit--learn](https://img.shields.io/badge/Scikit--learn-ML-F7931E.svg)](https://scikit-learn.org/)

Projeto desenvolvido no âmbito da disciplina **Redes Neurais em Segurança Cibernética**, do **Programa de Pós-Graduação Profissional em Engenharia Elétrica (PPEE) da Universidade de Brasília (UnB)**.

O projeto apresenta uma aplicação prática de **Redes Neurais Artificiais para identificação de dispositivos IoT a partir de características de tráfego de rede**, relacionando técnicas de Aprendizado de Máquina ao **Controle 1 – Inventário e Controle de Ativos Corporativos**.

---

## 1. Contexto

O **Controle 1 – Inventário e Controle de Ativos Corporativos** destaca a importância de conhecer os ativos presentes na infraestrutura de uma organização.

No contexto de segurança cibernética, o inventário não deve ser entendido apenas como uma relação estática de equipamentos. É necessário manter uma visão atualizada dos ativos, identificar novos dispositivos e tratar aqueles que não deveriam estar presentes na infraestrutura.

Nesse contexto, a identificação automática de dispositivos a partir de seu comportamento na rede pode funcionar como um mecanismo complementar aos processos tradicionais de descoberta e inventário.

Este projeto explora essa possibilidade utilizando uma **Rede Neural Artificial (RNA)** para classificar dispositivos IoT a partir de características extraídas de seu tráfego de rede.

A proposta conceitual é:

```text
Tráfego de rede
       ↓
Extração de características
       ↓
Rede Neural Artificial
       ↓
Classificação do dispositivo
       ↓
Apoio à identificação e ao inventário de ativos
```

> **Importante:** o modelo desenvolvido realiza classificação de dispositivos. Ele não determina, isoladamente, se um ativo é autorizado ou não. A decisão de autorização deve ser realizada mediante comparação com o inventário e as políticas de segurança da organização.

---

## 2. Objetivo

### Objetivo geral

Desenvolver uma aplicação de Redes Neurais Artificiais capaz de identificar categorias de dispositivos IoT a partir de características de tráfego de rede.

### Objetivos específicos

* utilizar uma base de dados de tráfego de dispositivos IoT;
* realizar análise exploratória dos dados;
* preparar as características para utilização por uma rede neural;
* desenvolver um classificador multiclasse;
* avaliar o desempenho utilizando diferentes métricas;
* analisar a matriz de confusão;
* avaliar o desempenho individual por categoria de dispositivo;
* analisar os erros de classificação;
* analisar a confiança das previsões;
* demonstrar como a classificação pode apoiar processos de inventário de ativos corporativos.

---

## 3. Relação com Segurança Cibernética

O projeto está relacionado principalmente ao **Controle 1 – Inventário e Controle de Ativos Corporativos** apresentado no material da disciplina.

O material destaca que uma organização deve estabelecer e manter um inventário detalhado e atualizado de seus ativos, além de identificar ativos não autorizados e utilizar mecanismos de descoberta ativa e passiva.

A abordagem desenvolvida neste projeto explora especificamente o uso do **tráfego de rede como fonte de informação para descoberta e identificação de ativos**, utilizando aprendizado de máquina como mecanismo auxiliar.

Dessa forma, a aplicação pode ser representada conceitualmente como:

```text
                   INFRAESTRUTURA CORPORATIVA
                              │
                              ▼
                       Tráfego de rede
                              │
                              ▼
                   Características de tráfego
                              │
                              ▼
                    Rede Neural Artificial
                              │
                              ▼
                  Categoria do dispositivo IoT
                              │
                              ▼
                       Inventário de ativos
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
             Conhecido                  Não identificado
                 │                         │
                 ▼                         ▼
          Monitoramento                 Investigação
```

A identificação automática não substitui o processo de inventário, mas pode contribuir para aumentar a **visibilidade sobre os dispositivos presentes na rede**.

---

## 4. Dataset

Foi utilizado o dataset **IoT Device Identification**, disponibilizado no Kaggle.

O conjunto contém registros associados a diferentes categorias de dispositivos IoT, representadas pela variável alvo:

```text
device_category
```

Entre as categorias presentes no conjunto de dados estão:

* `TV`
* `baby_monitor`
* `lights`
* `motion_sensor`
* `security_camera`
* `smoke_detector`
* `socket`
* `thermostat`
* `watch`
* `water_sensor`

O dataset foi dividido em arquivos de treinamento e teste:

```text
iot_device_train.csv
iot_device_test.csv
```

### Características utilizadas

Na Versão 02, após o processo de preparação dos dados, foram utilizadas **160 características** para treinamento da rede neural.

O notebook realiza:

* verificação de valores ausentes;
* análise dos tipos de dados;
* análise de cardinalidade;
* identificação e remoção de características constantes;
* separação entre características e variável alvo;
* normalização dos dados.

O dataset é baixado diretamente no notebook a partir do Kaggle, não sendo armazenado neste repositório.

---

## 5. Metodologia

A metodologia da Versão 02 foi organizada nas seguintes etapas:

### 5.1 Carregamento dos dados

Os arquivos de treinamento e teste são carregados separadamente.

O conjunto oficial de teste é preservado para avaliação final.

### 5.2 Análise dos dados

São verificados:

* dimensões dos conjuntos;
* distribuição das classes;
* valores ausentes;
* tipos das variáveis;
* cardinalidade das características;
* características constantes.

### 5.3 Divisão treinamento/validação

O conjunto de treinamento é dividido utilizando:

* **80%** para treinamento;
* **20%** para validação;
* divisão estratificada;
* `random_state = 42`.

O conjunto oficial de teste não participa dessa etapa.

### 5.4 Pré-processamento

É utilizado um pipeline contendo:

```text
Imputação pela mediana
        ↓
StandardScaler
```

O pré-processamento é ajustado exclusivamente sobre o conjunto de treinamento e posteriormente aplicado à validação e ao teste.

Essa estratégia evita o uso de informações do conjunto de avaliação durante o treinamento do pré-processador.

---

## 6. Baseline

Antes da utilização da rede neural, foi estabelecido um modelo baseline utilizando uma estratégia de classificação pela classe mais frequente.

O objetivo é estabelecer um ponto de referência para avaliar se a rede neural apresenta capacidade efetiva de classificação.

Resultado na validação:

| Modelo   | Accuracy | Macro-F1 | Balanced Accuracy |
| -------- | -------: | -------: | ----------------: |
| Baseline |   10,00% |    1,82% |            10,00% |

Como existem dez classes no conjunto de treinamento, o resultado do baseline evidencia que uma estratégia sem capacidade de discriminação apresenta desempenho muito inferior ao obtido pela rede neural.

---

## 7. Arquitetura da Rede Neural

Foi desenvolvida uma rede neural artificial do tipo **Multilayer Perceptron (MLP)** para classificação multiclasse.

A arquitetura utilizada na Versão 02 é:

```text
Entrada
  │
  ▼
Dense – 128 neurônios
ReLU
  │
  ▼
Dropout – 30%
  │
  ▼
Dense – 64 neurônios
ReLU
  │
  ▼
Dropout – 20%
  │
  ▼
Dense – 32 neurônios
ReLU
  │
  ▼
Dense – 10 neurônios
Softmax
  │
  ▼
Categoria do dispositivo
```

### Configuração

**Otimizador:**

```text
Adam
```

**Learning rate:**

```text
0.001
```

**Função de perda:**

```text
Sparse Categorical Crossentropy
```

**Função de ativação das camadas ocultas:**

```text
ReLU
```

**Função de ativação da saída:**

```text
Softmax
```

**Batch size:**

```text
32
```

**Número máximo de épocas:**

```text
150
```

Também foi utilizado **Early Stopping**, monitorando a perda de validação e restaurando os melhores pesos encontrados durante o treinamento.

---

## 8. Métricas utilizadas

O desempenho do modelo foi avaliado utilizando:

* Accuracy;
* Balanced Accuracy;
* Precision;
* Recall;
* F1-score;
* Macro-F1;
* Weighted-F1;
* matriz de confusão.

Além das métricas globais, foi realizada uma análise individual das categorias de dispositivos.

Também foi analisada a confiança produzida pelo Softmax para investigar o comportamento das previsões.

---

# 9. Resultados – Versão 02

A Versão 02 apresentou os seguintes resultados:

| Experimento          |   Accuracy |   Macro-F1 | Balanced Accuracy |
| -------------------- | ---------: | ---------: | ----------------: |
| Baseline – validação | **10,00%** |  **1,82%** |        **10,00%** |
| RNA – validação      | **82,00%** | **79,66%** |        **82,00%** |
| RNA – teste oficial  | **19,11%** | **15,68%** |        **19,11%** |

### Validação interna

A rede neural alcançou:

**Accuracy = 82,00%**

e

**Macro-F1 = 79,66%**

na validação interna.

Esse resultado representa uma melhora significativa em relação ao baseline.

### Teste oficial

Quando aplicada ao conjunto oficial de teste, a rede neural apresentou:

**Accuracy = 19,11%**

**Macro-F1 = 15,68%**

**Balanced Accuracy = 19,11%**

A diferença entre a validação interna e o teste oficial constitui um dos principais resultados observados nesta primeira versão do experimento.

---

## 10. Desempenho por classe

No teste oficial, foram observadas diferenças significativas entre as categorias.

Os melhores resultados de F1-score foram observados para:

| Dispositivo       |   F1-score |
| ----------------- | ---------: |
| `smoke_detector`  | **59,07%** |
| `socket`          | **48,51%** |
| `motion_sensor`   | **16,44%** |
| `security_camera` | **16,04%** |

Algumas classes apresentaram desempenho muito baixo ou nulo no teste oficial, incluindo:

* `baby_monitor`;
* `lights`;
* `thermostat`.

A categoria `water_sensor` não possui amostras no conjunto oficial de teste e, portanto, não pode ter seu desempenho avaliado nesse conjunto.

---

## 11. Análise dos erros

A análise da matriz de confusão mostrou que os erros não ocorreram de maneira aleatória.

Entre as principais confusões observadas estão:

| Classe real     | Classe predita    | Quantidade |
| --------------- | ----------------- | ---------: |
| `watch`         | `baby_monitor`    |         89 |
| `thermostat`    | `TV`              |         87 |
| `lights`        | `socket`          |         78 |
| `TV`            | `baby_monitor`    |         76 |
| `baby_monitor`  | `security_camera` |         74 |
| `socket`        | `water_sensor`    |         43 |
| `motion_sensor` | `smoke_detector`  |         41 |

Esses resultados indicam que determinadas categorias apresentam padrões de características semelhantes ou que existem diferenças entre os conjuntos de treinamento e teste que dificultam a generalização do modelo.

---

## 12. Análise da confiança

A análise das probabilidades produzidas pela camada Softmax mostrou:

```text
Confiança média das previsões: 87,96%
```

Entretanto:

```text
Confiança média nos acertos: 78,78%
Confiança média nos erros:   90,13%
```

Esse resultado é particularmente relevante.

O modelo apresentou, em média, **maior confiança nas previsões incorretas do que nas corretas**.

Isso demonstra que a saída Softmax não deve ser interpretada automaticamente como uma probabilidade calibrada de acerto.

Em uma aplicação de segurança cibernética, essa característica deve ser considerada antes da utilização operacional do modelo para tomada de decisão automática.

---

## 13. Aplicação ao inventário de ativos

A aplicação desenvolvida pode ser utilizada conceitualmente como uma camada auxiliar de identificação de ativos.

Por exemplo:

```text
Dispositivo observado
        ↓
Características do tráfego
        ↓
Rede Neural
        ↓
"security_camera"
        ↓
Consulta ao inventário
        ↓
Ativo conhecido?
        ├── Sim → continuar monitoramento
        │
        └── Não → investigar
```

Essa abordagem pode contribuir para processos de descoberta passiva de ativos e aumento da visibilidade da infraestrutura.

Entretanto, uma classificação realizada pela rede neural **não deve ser confundida com uma decisão de autorização**.

A classificação responde:

> Qual categoria de dispositivo apresenta maior compatibilidade com o padrão de tráfego observado?

O processo de inventário responde:

> Esse ativo é conhecido, autorizado e devidamente registrado pela organização?

As duas informações devem ser utilizadas conjuntamente.

---

## 14. Limitações

A Versão 02 possui algumas limitações importantes.

### 14.1 Diferença entre validação e teste

O desempenho caiu de **82,00% na validação interna para 19,11% no teste oficial**.

Essa diferença indica que a capacidade de generalização do modelo ainda precisa ser investigada.

### 14.2 Distribuição do teste

A categoria `water_sensor` está presente no treinamento, mas não possui amostras no teste oficial.

Consequentemente, não é possível avaliar essa classe no teste oficial.

### 14.3 Confiança das previsões

A análise mostrou confiança elevada mesmo em previsões incorretas.

Uma aplicação operacional exigiria estudos adicionais de calibração da confiança.

### 14.4 Tamanho do dataset

O experimento utiliza um conjunto relativamente pequeno para um problema de classificação multiclasse com grande quantidade de características.

### 14.5 Ambiente experimental

Os resultados foram obtidos a partir de um dataset disponibilizado para fins de pesquisa e não representam necessariamente o comportamento de dispositivos em uma rede corporativa real.

---

## 15. Próximos passos

Como continuidade do projeto, pretende-se investigar:

1. diferenças de distribuição entre treinamento e teste;
2. seleção e redução de características;
3. análise de importância das características;
4. PCA e técnicas de visualização da separabilidade das classes;
5. comparação com Random Forest;
6. comparação com XGBoost;
7. otimização dos hiperparâmetros da rede neural;
8. validação cruzada;
9. calibração das probabilidades;
10. investigação de dispositivos desconhecidos;
11. avaliação em outros datasets;
12. possível integração com mecanismos de inventário de ativos.

---

## 16. Estrutura do projeto

```text
identificacao-ativos-iot-redes-neurais/
│
├── README.md
│
├── notebooks/
│   └── Projeto01_Ativos_Corporativos_V02.ipynb
│
├── results/
│   └── README.md
│
├── .gitignore
│
└── LICENSE
```

O dataset não é armazenado diretamente no repositório. O notebook realiza o download da base durante a execução.

---

## 17. Execução

### Google Colab

O projeto foi desenvolvido para execução no **Google Colab**.

Após abrir o notebook:

1. executar as células em sequência;
2. permitir o download do dataset;
3. aguardar o treinamento da rede neural;
4. analisar as métricas;
5. analisar as matrizes de confusão;
6. analisar os erros e a confiança das previsões.

### Dependências principais

```text
Python
NumPy
Pandas
Scikit-learn
TensorFlow / Keras
Matplotlib
Seaborn
Joblib
```

---

## 18. Notebook

O notebook principal deste projeto é:

```text
Projeto01_Ativos_Corporativos_V02.ipynb
```

Ele contém todo o processo experimental, desde o carregamento do dataset até a avaliação final do modelo.

---

## 19. Considerações finais

A aplicação desenvolvida demonstra uma possibilidade de utilização de **Redes Neurais Artificiais na identificação de dispositivos IoT a partir do tráfego de rede**, relacionando aprendizado de máquina a uma necessidade prática de segurança cibernética: aumentar a visibilidade sobre os ativos presentes em uma infraestrutura.

A Versão 02 apresentou desempenho de **82,00% na validação interna**, porém o desempenho caiu para **19,11% no teste oficial**. Esse resultado evidencia que a avaliação de generalização é fundamental e constitui uma oportunidade para evolução do projeto.

O principal propósito desta primeira versão é demonstrar a construção do pipeline completo:

```text
Dataset
   ↓
Análise dos dados
   ↓
Pré-processamento
   ↓
Treinamento
   ↓
Rede Neural
   ↓
Avaliação
   ↓
Identificação de dispositivos IoT
   ↓
Aplicação ao inventário de ativos
```

Os resultados obtidos serão utilizados como ponto de partida para futuras melhorias e investigações.

---

## 20. Referências

### Material da disciplina

SERRANO, André. **Segurança Cibernética – Controles 1 a 6 do CIS Controls**. Departamento de Engenharia de Produção, Faculdade de Tecnologia, Universidade de Brasília (UnB). Material didático da disciplina Redes Neurais em Segurança Cibernética.

### Referencial de segurança

CENTER FOR INTERNET SECURITY. **CIS Controls v8**. CIS, 2021.

CENTER FOR INTERNET SECURITY. **Guide to Enterprise Assets and Software**. CIS, 2022.

### Dataset

**IoT Device Identification**. Kaggle. Dataset utilizado para classificação de dispositivos IoT a partir de características de tráfego de rede.

---

## Autor

**Leonardo Santiago Melgaço Silva**

Programa de Pós-Graduação Profissional em Engenharia Elétrica – PPEE
Universidade de Brasília – UnB

**Disciplina:** Redes Neurais em Segurança Cibernética

---

## Status do projeto

**Versão 02 – Experimento inicial concluído**

O projeto encontra-se em desenvolvimento acadêmico. Os resultados apresentados correspondem ao experimento realizado na Versão 02 do notebook.
