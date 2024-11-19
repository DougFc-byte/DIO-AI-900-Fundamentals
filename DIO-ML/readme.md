---
    title: Desafio 01: Trabalhando com Machine Learning na Prática no AZURE ML
---

# Explorando a automatização de ML no Azure

## Primeiro, criei um Azure Machine Learning workspace e uma Subscription 

1. Selecionei **+ Create a resource**, procurando por *Machine Learning*, e criando um novo recurso de **Azure Machine Learning** com os parâmetros indicados no DESAFIO da DIO e na documentação do Microsoft Learn. Depois, entrei no workspace criado do Azure Machine Learning studio.

## Em seguida, usei automated machine learning para treinar um modelo usando os passos a seguir:

1. Criei um novo ML job automatizado:

    **Parâmetros Básicos**:

    - **Job name**: mesmo nome sugerido.
    - **New experiment name**: `mslearn-bike-rental`
    - **Descrição**: Automated machine learning for bike rental prediction.
    - **Tags**: *nenhuma*

   **Tipo da Task e Dados**:

    - **Select task type**: Regression
    - **Select dataset**: Criei um novo new dataset com os seguintes parâmetros:
        - **Data type**:
            - **Name**: `bike-rentals`
            - **Description**: `Historic bike rental data`
            - **Type**: Table (mltable)
        - **Data source**:
            - Select **From local files**
        - **Destination storage type**:
            - **Datastore type**: Azure Blob Storage
            - **Name**: workspaceblobstore
        - **MLtable selection**:
            - **Upload folder**: *Baixei e unzipei a pasta que contém os dois arquivos para dar upload do link seguinte* `https://aka.ms/bike-rentals`

     Selecionei **Create** e depois o conjunto de dados **bike-rentals**.

    **Parâmetros de Tarefa**:

    - **Task type**: Regression
    - **Dataset**: bike-rentals
    - **Target column**: rentals (integer)
    - **Additional configuration settings**:
        - **Primary metric**: NormalizedRootMeanSquaredError
        - **Explain best model**: *Unselected*
        - **Enable ensemble stacking**: *Unselected*
        - **Use all supported models**: <u>Un</u>selected. *You'll restrict the job to try only a few specific algorithms.*
        - **Allowed models**: *Select only **RandomForest** and **LightGBM** — normally you'd want to try as many as possible, but each model added increases the time it takes to run the job.*
    - **Limits**: *Expand this section*
        - **Max trials**: `3`
        - **Max concurrent trials**: `3`
        - **Max nodes**: `3`
        - **Metric score threshold**: `0.085` (*so that if a model achieves a normalized root mean squared error metric score of 0.085 or less, the job ends.*)
        - **Experiment timeout**: `15`
        - **Iteration timeout**: `15`
        - **Enable early termination**: *Selected*
    - **Validation and test**:
        - **Validation type**: Train-validation split
        - **Percentage of validation data**: 10
        - **Test dataset**: None

    **Compute**:

    - **Select compute type**: Serverless
    - **Virtual machine type**: CPU
    - **Virtual machine tier**: Dedicated
    - **Virtual machine size**: Standard_DS3_V2\*
    - **Number of instances**: 1

    \* Aqui não foi esse o selecionado pois a versão trial não permite esse tamanho de máquina. Escolhi o maior que pudia. .*

1. Segui todos esses passos que foram tirados da documentação da microsoft.

## Revisando o melhor modelo

When the automated machine learning job has completed, you can review the best model it trained.

1.Escolhi o melhor modelo e vi as métricas.

## Deploy e testar o modelo

Nessa parte, tentei fazer exatamente o que diz a documentação da microsoft:
1. On the **Model** tab for the best model trained by your automated machine learning job, select **Deploy** and use the **Real-time endpoint** option to deploy the model with the following settings:
    - **Virtual machine**: Standard_DS3_v2
    - **Instance count**: 3
    - **Endpoint**: New
    - **Endpoint name**: *Leave the default or make sure it's globally unique*
    - **Deployment name**: *Leave default*
    - **Inferencing data collection**: *Disabled*
    - **Package Model**: *Disabled*

   2. No entanto, o deplou deu erro. Vasculhando a internet, desobri que tinha que registrar resource provider na minha subscription: Microsoft.CDN e Microsoft.PolicyInsights. Depois disso o deploy funcionou.

## Testando o deploy

1. Selecionei em Azure Machine Learning studio, no menu da esquerda, e seleiconei **Endpoints** e real-time endpoint que dei deploy anteriormente.

1. Abri a página de **Test**.

1. Substituti o template JSON com os seguintes dados:

    ```json
      {
     "input_data": {
       "columns": [
         "day",
         "mnth",
         "year",
         "season",
         "holiday",
         "weekday",
         "workingday",
         "weathersit",
         "temp",
         "atemp",
         "hum",
         "windspeed"
       ],
       "index": [0],
       "data": [[1,1,2022,2,0,1,1,2,0.3,0.3,0.3,0.3]]
     }
    }

    ```

1. Cliquei o botão de **Test** e recebi os resultados:

[
  352.661485332827
]

2. Os resultados e o json teste estão na página do repositório.

