# Predição de Conversão de Usuários

## Descrição

Este repositório contém modelos desenvolvidos para prever com maior precisão os testes que se converterão em pagamentos. O objetivo é fornecer insights úteis para o negócio, otimizando os recursos e aumentando a eficiência do processo de teste.

## Métrica de Avaliação

Devido ao desbalanceamento entre os valores de *target* disponível no dataset, uma simples métrica de *accuracy* poderia nos trazer resultados irreais.

Com isso, a métrica escolhida para avaliação foi uma *accuracy* separada para cada *target*, possibilitando a visualização de uma melhor *performance* da rede.

`Acurácia = (Número de previsões corretas) / (Número total de previsões)`


## Modelo Desenvolvido

Foram utilizados dois modelos para predição dos resultados: Light GBM e Tensorflow.
Além disso, alguns modelos foram treinados de mais de uma maneira, a fim de exploração e obter melhores resultados.

Os seguintes modelos foram desenvolvidos:

#### 0. Exploração dos dados (`explore.ipynb`)
Antes do desenvolvimento dos modelos, esse *notebook* é dedicado a exploração dos dados. Ajudando a ter um maior entendimento e identificar possíveis problemas, como desbalanceamanto dos dados ou valores *missing*.


#### 1. Fine tuning Light GBM (`light_gbm model.ipynb`)
O primeiro modelo treinado foi feito um fine tuning manual do Light GBM.
Utilizando as features disponíveis no dataset, foi feito o ajuste manual dos hiperparâmetros em busca de um melhor resultado.

Acurácia encontrada:
| Target 0 	| Target 1 	|
|----------	|----------	|
| 0.801    	| 0.802    	|



#### 2. Busca por parâmetros com Light GBM (`light_gbm multiple train model.ipynb`)
Esse modelo também se baseia no Light GBM, porém é feito uma busca pelos melhores hiperparâmetros que conseguirão aumentar a acurácia do modelo.

Para isso, é definido uma lista de hiperparâmetros e diversos valores para eles. Após isso, será gerado uma combinação de todos os valores possíveis e um treino para cada uma dessas combinações, o que pode levar um tempo para a execução completa.

Para comparação dos modelos gerados, foi utilizado a métrica `recall`. Depois foi escolhido o melhor modelo e feito as comparações conforme descrito em Métrica de Avaliação.

Acurácia encontrada:
| Target 0 	| Target 1 	|
|----------	|----------	|
| 0.803    	| 0.803    	|



#### 3. Feature Selection com Light GBM (`feature selection.ipynb`)
A ideia desse modelo é não utilizar todas as *features* disponíveis no *dataset*, mas sim escolher as que possuem maior valor de predição.

Para isso, foi utilizado o módulo de *Feature Selection* do *sklearn*.
Após o processamento, descobrimos que as *features* com maior relevância são:
 - creation_weekday
 - creation_hour
 - total_product_categories
 - total_events_on_Web
 - source_pulido_index
 
Com isso, um novo modelo foi treinado utilizando apenas essas *features*.

Acurácia encontrada:
| Target 0 	| Target 1 	|
|----------	|----------	|
| 0.801    	| 0.802    	|



#### 4. Modelo com Tensorflow (`tensorflow model.ipynb`)
Tentando mudar a abordagem para um modelo mais pesado em busca de se encontra uma maior acurácia, foi também criado um modelo utilizando o *Tensorflow*.

Assim como os demais, foi criado um simples modelo de predição binária. O modelo convergiu rapidamente, logo, não foram necessárias muitas épocas de treino.

Acurácia encontrada:
| Target 0 	| Target 1 	|
|----------	|----------	|
| 0.828    	| 0.738    	|



#### 5. Blend de modelos (`Blending models.ipynb`)
Com alguns modelos desenvolvidos, esse modelo tenta juntar a capacidade de predição do modelos, realizando uma média de seus valores, em busca de um maior poder preditivo.



Acurácia encontrada:
| Target 0 	| Target 1 	|
|----------	|----------	|
| 0.799    	| 0.804    	|


## Insights do modelo

1. **Dados desbalanceados**: O número de registros do *dataset* com *target* `0` não é proporcial aos registros de *target* `1`, tornando a base de dados desbalanceada. Com isso, é necessário ter atenção, pois pode impactar diretamente nas métrias e resultados, dando ideias falsas sobre o modelo. É recomendado o uso de técnias para esse cenário.
2. **Segmentação de targets**: Não existe uma fácil separação de comportamento entre os usuários que irão se converter em pagamentos e que não irão se converter, a distribuição individual das *features* são próximas para ambos os *targets*. Dessa forma, é difícil criar uma fácil visualização entre esses dois usuários, sendo necessário o desenvolvimento de modelos para se identificar os usuários que serão convertidos em pagamentos.
3. **Ambiente Web**: De todos os ambientes (Web, iOS e Android), o Web se mostrou o mais importante para definir se um usuário irá se converter ou não. Seria isso devido ao fato do Web estar associado a uma tela maior e uso de mouse e teclado ou os outros ambientes possuem alguma problema de usabilidade? Devemos focar em melhorar o ambiente Web, que é mais efetivo, ou melhorar as outras plataformas e tornar elas tão efetivo quanto? São perguntas que podem ser feitas em busca de mais resultados.
4. **Modelos bem próximos**: Apesar das diversas experimentações, os modelos tiveram resultados bem próximos, tendo o modelo Light GBM treinado com busca de parâmetros tendo a melhor acurácia em ambos os *targets*.
5. **Feature Engineering**: Um passo futuro para melhorar o modelo pode ser a criação de `features`, extraindo características dos valores atuais antes de inserir no modelo. O que também pode ser potencializado com informações de outras bases de dados.

## Como Utilizar

1. **Requisitos**: Instale as dependências listadas no arquivo `requirements.txt` através do comando `pip install -r requirements.txt`.
1.1 **Light GBM**: Para instalar o Light GBM, recomendo a leitura de seu [Installation Guide](https://lightgbm.readthedocs.io/en/latest/Installation-Guide.html).
1.2 **Tensorflow**: Acredito que não há problemas para instalar sua versão CPU com o Python.
2. **Preparação dos Dados**: Copie o arquivo `test_data.csv` para a raiz do projeto. Ele não está incluso no repositório.
3. **Treinamento do Modelo**: Todos os modelos já foram previamente treinados e estão disponíveis na pasta `/models`. Caso queira treinar algum modelo novamente, basta apenas executar o seu notebook por completo. Pode ser necessário alterar algum *threshold* para se obter melhores resultados.
