# Deteção e disposição de informação de electrodomésticos no âmbito de “Ambient Assisting Living”
David Capela e Luís Aguiar

## I.	Introdução
Um Assistente Pessoal Eletrónico é um sistema que suporta comandos específicos dos utilizadores de maneira a encontrar informação, marcar eventos , ou gerenciar fluxos de trabalho. Nos últimos anos os APE evoluíram bastante no mercado orientado ao consumidor. Este processo é baseado em vários fatores , entre os quais , estão principalmente destacados a evolução das interfaces consumidor-máquina ( ex: comandos vocais) e a expansão dos dispositivos IoT ( Internet Of Things ). 
A ampla difusão dos dispositivos IoT requer ter um dispositivo que se comporte como um controlador e permita controlar a informação recebida do(s) nó(s) conectado(s). Apesar da grade evolução da capacidade de processamento dos “smartphones” , a sua maior condicionante , a bateria,  ainda os torna incapacitados para atender constantemente a pedidos.

## II.	Contexto e pesquisa do problema
O objetivo do nosso experimento é desenhar e implementar um modelo que permite ao consumidor final detetar e informar-se sobre os electrodomésticos por via da câmara do smartphone. Estas informações serão disponibilizadas no mesmo dispositivo depois de uma comunicação com um servidor que contém um sistema anteriormente treinado para tal.

## III.	Sumário
A visão computacional aliada aos modelos de Deep Learning traz cada vez mais projetos inovadores e que acrescentam valor ao estilo de vida de todos nós. Este facto faz com que seja pertinente escolhermos este projeto para explorar e aprender algo de novo.
O nosso caminho passará por reconhecer a framework Keras e desenvolver pequenos protótipos com um “Dataset” mais diminuto, o que levará , mais tarde , a uma maior e mais madura escala.


## Dependências
**python** 

**requests** package para pedidos HTTP

```
pip install requests
```

**OpenCV**
```
pip install opencv
```

**Keras**
```
pip install scipy matplotlib pillow
pip install imutils h5py requests progressbar2
pip install scikit-learn scikit-image
pip install tensorflow
pip install keras
```

Instalação mais detalhada [aqui](https://www.pyimagesearch.com/2017/09/25/configuring-ubuntu-for-deep-learning-with-python/).
## Estrutura do projeto

```
smallvgg
├── bin_image_search.py
├── classify.py
├── dataset
│   ├── cooker
│   ├── fridge
│   └── microwave
├── electro.model
├── examples
│   ├── cooker.jpg
│   ├── fridge.jpg
│   └── microwave.jpg
├── lbelectro.pickle
├── pyimagesearch
│   ├── __init__.py
│   ├── __pycache__
│   └── smallervggnet.py
└── train.py
```
### **bin_image_search.py** :

 Script em python que faz uso da [API de imagens da Bing](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) para obter imagens diferentes.
 
 **Parâmetros** 
 ```
-q ou --query
 ```
 Procura imagens pelo argumento passsado
  ```
-o ou --output
 ```
 Caminho desejado das imagens. No nosso caso será ```dataset```

 **Exemplo**
 ```
 python bin_image_search.py --query "microwave" --output dataset/microwave
 ```
___

 ### **<span>train.py</span>** :

 Script que permite treinar o modelo propriamente dito. O output são dois ficheiros: ```.model``` e ```.pickle```

 **.model** ficheiro compilado com o modelo treinado

 **.pickle** ficheiro Label Binarizer que permite resumidamente escrever texto em frames.

**Parâmetros** 
 ```
-d ou --dataset
 ```
Caminho para os datasets
  ```
-m ou --model
 ```
Caminho para o modelo a ser criado
```
-l ou --labelbin
```
Caminho para o Label Binarizer
```
-p ou --plot
```
Caminho para ficheiro de estatísticas. Não muito relevante

 **Exemplo**
 ```
python train.py --dataset dataset --model electro.model --labelbin lbelectro.pickle
 ```
 
 ___

 ### **<span>classify.py</span>** :

 Script que permite classificar uma imagem dado um modelo compilado e um label binarizer.

**Parâmetros** 
 
 ```
-m ou --model
 ```
Caminho para o modelo a ser criado
```
-l ou --labelbin
```
Caminho para o Label Binarizer
```
-i ou --image
```
Caminho para ficheiro de teste - imagem que não pertença ao dataset

 **Exemplo**
 ```
python classify.py --model electro.model --labelbin lbelectro.pickle --image examples/microwave.jpg
 ```

  ___

 ### **dataset** :

* Diretório onde serão guardadas as nossas imagens referentes à classe que pretendemos treinar.
* A estrutura é importante neste diretório pois o ficheiro label binarizer ```.pickle``` vai ser feito com base nos sub-diretórios dentro do dataset , ou seja ```dataset/microwave``` , ```dataset/fridge``` e ```dataset/cooker``` são vistos como classes do tipo ```microwave``` , ```cooker``` e ```fridge```. Caso esta estrutura não seja respeitada , o ficheiro ```train.py``` ou dará um erro , ou o ```.pickle``` sairá errado.

___

 ### **examples** :

 Diretório com imagens de exemplo que não foram usadas para treinar o modelo.

___

 ### **pyimagesearch** :

 Módulo de python com o ficheiro ```smallervggnet.py``` que tem as camadas necessárias para treinar o nosso modelo. 

## Pipeline

A nossa pipeline para criar um modelo treinado e testá-lo.


* Popular o nosso diretório de dataset com o ficheiro ```bin_image_search.py```
* Treinar a nossa rede com base no dataset criado anteriormente. Utilizamos o ```train.py``` para compilar o modelo.
* Depois de compilado o modelo e criado o label binarizer , temos que testar com um exemplo na pasta ```exemples``` 
* Tiramos conclusões sobre a prababilidade de certeza do exemplo e tentamos testar com imagens menos percetíveis de maneira a meter à prova o modelo em ambientes menos favoráveis à decisão da classe.


## Próximos passos

* Amadurecer o nosso modelo com mais imagens e mais imagens de teste.
* Implementar o modelo treinado num smartphone na framework Flutter.


## Referências

[pyimagesearch](https://www.pyimagesearch.com/)
