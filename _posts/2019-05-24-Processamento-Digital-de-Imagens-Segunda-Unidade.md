---
layout:     post
title:      Processamento Digital de Imagens Segunda Unidade
summary:    Exercícios Processamento Ditial de Imagens.
categories: digital image processing
thumbnail: PDI
tags:
 - digital image processing
---

# Resumo
Este post consiste na aplicação dos conceitos estudados na segunda unidade da disciplina de Processamento Digital de Imagens DCA0445 do curso de Engenharia de Computação da Universisdade Federal do Rio Grande do Norte. Os Exercícios apresentados foram desenvolvidos por [Giovanna Severo][1] e por [João Victor Costa][2], todos os códigos podem ser encontrados [AQUI][3] .

- # Filtragem no domínio da frequência
**Exercício 1:**
O exercício de baseia na aplicação de um filtro homomórfico para melhorar imagens com iluminação irregular.
Para a realização do exercício é necessário fazer a transformada de fourier dos pontos da matriz da imagem e realizar a troca dos quandrantes, para que assim seja possível aplicar o filtro atenuando as frequências baixas e mantendo as frequências altas, o filtro foi determinado pela seguinte expressão:
<center>Filtro H(u,v)<img src="https://i.imgur.com/8ogq5zD.png" style="height:200px;"/></center>
```C
 for(int i=0; i<dft_M; i++){
    for(int j=0; j<dft_N; j++){
				d2 = (i-dft_M/2)*(i-dft_M/2)+(j-dft_N/2)*(j-dft_N/2);//calculo da distancia
				tmp.at<float>(i,j) = (gama_h-gama_l)*(1.0-(float)exp(-(c*d2/(d*d))))+gama_l;//função que gera o filtro
    }
  }
```
Após determinar o filtro é feita a convolução do filtro com a matriz da imagem (H(u,v).Z(x,y)) e aplicado a troca dos quadrante novamente, depois calcula-se a transformada inversa de fourier.
[Código Completo][4]

- # Detecção de bordas com o algoritmo de Canny
**Exercício 2:**
No segundo exercício é solicitado a utilização do algoritmo de Canny para gerar uma imagem em pontilhismo usando os conceitos de borda para melhorar a qualidade da imagem.
Para resolução a estratégia utilizada foi de gerar duas imagens uma representando a imagem original em pontos e outra aplicando o algoritmo de canny e obtendo as bordas, com isso percorreu-se a imagem contendo as bordas verificando os pixels com valor maior que 0 ao encontrar era criado um circulo na imagem dos pontos, melhorando assim a imagem pontilhada.

```C
 for (int j = 0; j< height; j++){
    for(int k = 0; k < width; k++){
      if(border.at<uchar>(j,k)>0){//fazendo um teste para saber se o pixel é uma borda
              gray = image.at<uchar>(j,k);
              circle(points, Point(k,j),RAIO,CV_RGB(gray,gray,gray),-1,CV_AA);//criando um circulo na posição da borda
              }
    }
  }
```
<center>Imagem Original<img src="https://i.imgur.com/8Mx54Ns.png" style="height:200px;"/></center>
<center>Imagem Pontos<img src="https://i.imgur.com/EfMThaz.jpg" style="height:200px;"/></center>
<center>Imagem Bordas<img src="https://i.imgur.com/WM6reEt.png" style="height:200px;"/></center>
<center>Imagem Pontos após tratamento<img src="https://i.imgur.com/ITzSvER.jpg" style="height:200px;"/></center>
[Código Completo][5]



- # Quantização vetorial com k-means
**Exercício 3:**
Nesse exercício é abordado o conceito de quantização vetorial e é solicitado que sejam realizadas diversas rodadas a fim de avaliar as diferenças nas imagens geradas.
Para resolução do exercício foi utilizado o algoritmo k-mean gerando os centros de forma aleatória. O algoritmo como dito anteriormente gera os centros de forma aleatória, além disso ele faz o cálculo de distância e cria grupos fazendo as classificações, a medida que que o algoritmo era executado foi possível perceber que as imagens poderiam ser bem diferentes, chegamos a conclusão que isso ocorre dado o fato do nRodadas ser igual a 1 e os centros serem gerados de forma aleatória, assim o algoritmo não consegue recalcular e ajustar os grupos, o que ocorreria caso o nRodadas fosse maior, ou seja os centros acabam variando muito uma vez que que só são calculados uma vez e podem ser gerados de formas diferentes dado pela aleatoriedade de sua criação.  

<center>Imagem Original<img src="https://i.imgur.com/EgBuoFI.jpg" style="height:200px;"/></center>
<center>Gif mostrando imagens geradas<img src="https://i.imgur.com/TcaBWYm.gif" style="height:200px;"/></center>


[Código Completo][6]





[1]: http://giovanna96.github.io
[2]: http://joaovictor1996.github.io
[3]: https://github.com/joaovictor1996/joaovictor1996.github.io/tree/master/PDI
[4]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/homomomorfico/homomomorfico.cpp
[5]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/pontos/pontos.cpp
[6]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/kmeans/kmeans.cpp
