---
layout:     post
title:      Processamento Digital de Imagens Primeira Unidade
date:       2019-04-09 19:44:18
summary:    Exercícios Processamento Ditial de Imagens.
categories: digital image processing
thumbnail: PDI
tags:
 - digital image processing
---
## Resumo
Esse post consiste na aplicação dos conceitos estudados na primeira Unidade da disciplina de Processamento Digital de Imagens DCA0445 do curso de Engenharia de Computação da Universisdade Federal do Rio Grande do Norte. Os Exercícios apresentados foram desenvolvidos por [Giovanna Severo][1] e por [João Victor Costa][2] .

## Manipulando pixels em uma imagem
Exercício 1:
O primeiro exercício baseia-se em calcular o negativo de uma determinada região de uma imagem.
Primeiramente é feita a declaração da matriz de valores da imagem, declarou-se no código duas variáveis que serão solicitadas ao usuário, representando as coordenadas x e y de dois pontos delimitando uma região retangular da imagem e foi feita a leitura da imagem que sofrerá o cálculo.

```C
Mat image;
Vec3b val;
int x1, x2, y1, y2; //coordenadas passadas pelo usuário

image= imread("biel.png",CV_LOAD_IMAGE_GRAYSCALE); 
if(!image.data) //teste para saber se a imagem foi realmente carregada
cout << "Nao abriu o arquivo!" << endl;

namedWindow("janela",WINDOW_AUTOSIZE);

cout << "Digite as coordenadas X1 e Y1, seguidos de enter\n";
cin >> x1 >> y1;
cout << "Digite as coordenadas X2 e Y2, seguidos de enter\n";
cin >> x2 >> y2;
```
O cálculo do negativo é realizado a partir de dois laços for  que irão varrer a matriz de valores da imagem dentro do intervalo determinado(pontos informados pelo usuário) fazendo a subtração do valor 255 em cada pixel.

```C
for(int i=x1;i<x2;i++){
 for(int j=y1;j<y2;j++){
  image.at<uchar>(i,j)=255-image.at<uchar>(i,j); //substitui o valor do pixel pela subtração entre 255 e o valor dele
 }
}
```
Na Figura abaixo é possível observar o resultado da aplicação:
<center><img src="https://i.imgur.com/cUYH5pY.png" style="height:200px;"/></center>
<center><img src="https://i.imgur.com/WOVujQi.png" style="height:200px;"/></center>


[1]: http://giovanna96.github.io
[2]: http://joaovictor1996.github.io