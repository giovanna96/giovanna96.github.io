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

# Resumo
Este post consiste na aplicação dos conceitos estudados na primeira Unidade da disciplina de Processamento Digital de Imagens DCA0445 do curso de Engenharia de Computação da Universisdade Federal do Rio Grande do Norte. Os Exercícios apresentados foram desenvolvidos por [Giovanna Severo][1] e por [João Victor Costa][2], todos os códigos podem ser encontrados [AQUI][3] .

- # Manipulando pixels em uma imagem
**Exercício 1:**
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
[Código Completo][4]

Na Figura abaixo é possível observar o resultado da aplicação:
<center>Imagem Original<img src="https://i.imgur.com/cUYH5pY.png" style="height:200px;"/></center>
<center>Negativo<img src="https://i.imgur.com/WOVujQi.png" style="height:200px;"/></center>

**Exercício 2:**
No segundo exercício é solicitado que a imagem seja dividida em quatro quadrantes e que seja realizada a troca dessas regiões.
Para resolução desse exercício identificou-se qual o tamanho(altura e largura) da matriz da imagem utilizada e criou-se
uma matriz auxiliar cópia, após isso foram utilizados dois laços for para preencher cada quadrante da matriz cópia pelos valores dos quadrantes opostos, requisitando os valores na matriz da imagem original. 

```C
  largura=image.size().width; //recebe o valor da largura
  altura=image.size().height; //recebe o valor da altura
  cout<<largura<<"\n"<<altura<<"\n";

  image.copyTo(copia); //copia a imagem original para uma 

  for(int i=0;i<largura/2;i++){ //transfere para a cópia os valores de cada quadrante da imagem original
    for(int j=0;j<altura/2;j++){
      copia.at<uchar>((largura/2)+i,(altura/2)+j)=image.at<uchar>(i,j);
      copia.at<uchar>(i,j)=image.at<uchar>((largura/2)+i,(altura/2)+j);
      copia.at<uchar>(i,(altura/2)+j)=image.at<uchar>((largura/2)+i,j);
      copia.at<uchar>((largura/2)+i,j)=image.at<uchar>(i,(altura/2)+j);
    }
  }
```
[Código Completo][5]

<center>Regiões Trocadas<img src="https://i.imgur.com/LLL1sp7.png" style="height:200px;"/></center>

- # Preenchendo Regiões
**Exercício:**
O exercício baseia-se na contagem de bolhas em uma imagem, nesse problema foi abordada uma estratégia de labeling(Rotulamento) em que uma imagem binária será percorrida na procura de pixels de valores diferentes do fundo, uma vez que um pixel de valor diferente é encontrado aplica-se o algoritmo floodfill para preencher o aglomerado de pixels de cada objeto.
Inicialmente foi necessário eliminar os elementos das laterais, já que não é possível identificar se é uma bolha não sendo possível identificar os buracos, para isso utilizou-se dois laços for que percorrem as laterais da imagem, um para a vertical e outro para a horizontal e foi adicionada uma condição if para caso o pixel seja diferente de zero(valor do fundo) seja atribuído a ele esse valor.

```C
for(int i=0; i<height; i++){ //percorre as bordas verticais para eliminar as bolhas presentes nela
      if(image.at<uchar>(i,0) == 255){
		p.x=0;
		p.y=i;
		floodFill(image,p,0); //pinta a bolha com a cor do fundo
	  }
      if(image.at<uchar>(i,255) == 255){
		p.x=255;
		p.y=i;
		floodFill(image,p,0); //pinta a bolha com a cor do fundo
	  }
}

for(int y=0; y<width; y++){ //percorre as bordas horizontais para eliminar as bolhas presentes nela
      if(image.at<uchar>(0,y) == 255){
		p.x=y;
		p.y=0;
		floodFill(image,p,0); //pinta a bolha com a cor do fundo
	  }
      if(image.at<uchar>(255,y) == 255){
		p.x=y;
		p.y=255;
		floodFill(image,p,0); //pinta a bolha com a cor do fundo
	  }
}
```
Após isso é necessário percorrer a imagem por completo em busca dos pixels de valor 255(valor do contorno das bolhas) caso seja encontrado soma-se um ao valor de objetos  e faz-se o rotulamento atribuindo o valor do incremento a cada pixel encontrado. A partir de dois laços for a matriz foi percorrida pixel a pixel e a condição if verificou o valor dos pixels somando 1 a variável de controle da quantidade de objetos a cada valor 255 encontrado e com o algoritmo floodfill atribuiu-se o valor do incremento ao aglomerado.	

```C
for(int i=0; i<height; i++){ //percorre toda a imagem procurando pelas bolhas
    for(int j=0; j<width; j++){
      if(image.at<uchar>(i,j) == 255){
		nobjects++; //incrementa a quantidade de bolhas
		p.x=j;
		p.y=i;
		floodFill(image,p,nobjects); //pinta a bolha com o valor do incremento
	  }
	}
}
```
Por último foi feita a contagem do número de bolhas que contêm falhas(buracos em seu contorno) para isso mudou-se o valor do fundo e percorreu a imagem com laços for e foram adicionadas duas condições uma para verificar a presença de buracos e fazendo uma marcação para evitar contar a bolha encontrada mais de uma vez e outra condição para verificar se a bolha já foi contada.

```C
p.x = 0;
p.y = 0;
floodFill(image,p,100); //pinta o fundo com o valor 100

for(int i=0; i<height; i++){
    for(int j=0; j<width; j++){
      if(image.at<uchar>(i,j) == 0 && image.at<uchar>(i-1,j) != 255){ //percorre a imagem procurando por pixels que possuem o valor igual a 0 e o pixel anterior diferente de 255
		com_buracos++;
		p.x=j;
		p.y=i;
		floodFill(image,p,100); //pinta o buraco com a cor do fundo
		p.x=j;
		p.y=i-1;
		floodFill(image,p,255); //pinta a bolha de branco
        }
	if(image.at<uchar>(i,j) == 0 && image.at<uchar>(i-1,j) == 255){
		p.x=j;
		p.y=i;
		floodFill(image,p,100); //pinta o buraco com a cor do fundo
        }
      }
}
```
Para o caso em que o número de bolhas ultrapasse 255 nãos será possível fazer a contagem, uma vez que terão mais bolhas do que a quantidade de rótulos.
[Código Completo][6]

<center>Imagem Bolhas<img src="https://i.imgur.com/Oi4d9lT.png" style="height:200px;"/></center>


<center>Imagem Após Contagem<img src="https://i.imgur.com/qmgYIYq.png" style="height:200px;"/></center>

- # Manipulação de histogramas:
**Exercício 1:** 
Nesse exercício é proposto que seja desenvolvido um programa para fazer a equalização do histograma de uma imagem, considerando que as imagens geradas são em tons de cinza. 
Para criação do histograma equalizado, fez-se a captura da imagem e utilizou-se a função *equalizeHist* para fazer a equalização, depois utilizando a função *calcHist* foi calculado o histograma da imagem equalizada e com o a função *normalize* foi possível normalizar os valores calculados, após isso é atribuído a imagem do histograma o valor 0 e feito o desenho do gráfico de barras com a função *line*.

```C
  while(1){
    cap >> image;
	cvtColor(image,image,CV_BGR2GRAY);

	equalizeHist(image, equalizado);//equalizando a imagem

	calcHist(&image,1,0,Mat(),histC,1,&nbins,&histrange,uniform,acummulate);//criando o histograma da imagem origianl
	normalize(histC, histC, 0, histImg.rows, NORM_MINMAX, -1, Mat());
  	histImg.setTo(Scalar(0));

	calcHist(&equalizado,1,0,Mat(),equa,1,&nbins,&histrange,uniform,acummulate);//criando o histograma da imagem equalizada
	normalize(equa, equa, 0, histImg.rows, NORM_MINMAX, -1, Mat());
  	histEqu.setTo(Scalar(0));

	for(int i=0; i<nbins; i++){
      line(histImg,
           Point(i, histh),
           Point(i, histh-cvRound(histC.at<float>(i))),
           Scalar(255), 1, 8, 0);
      line(histEqu,
           Point(i, histh),
           Point(i, histh-cvRound(equa.at<float>(i))),
           Scalar(255), 1, 8, 0);
    }

	histImg.copyTo(image(Rect(0, 0,nbins, histh)));//copiando o histograma para a imagem original
	imshow("Original", image);

histEqu.copyTo(equalizado(Rect(0, 0,nbins, histh)));//copiando o histograma para a imagem equalizada
```
[Código Completo][7]

<center>Imagem em escala de cinza<img src="https://i.imgur.com/9lmDQB6.png" style="height:200px;"/></center>

<center>Imagem Equalizada<img src="https://i.imgur.com/0oVuOyV.png" style="height:200px;"/></center>

- # Exercício 2:
O problema consiste na formulação de um detector de movimento fazendo o cálculo do histograma continuamente e comparando o valor com o anterior, quando um movimento é detectado deve-se lançar um alarme.
Para identificar se há movimento fez-se o cálculo e a normalização do histograma com as funções *calcHist* e *normalize* de duas imagens atribuídas a uma escala de cinza, e foi feita a comparação dos dois histogramas com a função *compareHist*, após isso é feito um teste em uma estrutura de seleção if que confere se a comparação ultrapassou o limiar pré determinado, caso esse valor seja maior que a tolerância o movimento é detectado e a mensagem de movimento é imprimida, caso a comparação seja menor que a tolerância seu valor é imprimido na tela e o movimento não é detectado.

```C
 while(1){
	cap >> image1;//captura a primeira imagem
	cvtColor(image1,cinza1,CV_BGR2GRAY);

	calcHist(&cinza1,1,0,Mat(),hist1,1,&nbins,&histrange,uniform,acummulate);//calcula o histograma da primeira imagem
	normalize(hist1, hist1, 0, histImage.rows, NORM_MINMAX, -1, Mat());
  	histImage.setTo(Scalar(0));

	cap >> image2;//captura a segunda imagem
	cvtColor(image2,cinza2,CV_BGR2GRAY);
	
	calcHist(&cinza2,1,0,Mat(),hist2,1,&nbins,&histrange,uniform,acummulate);//calcula o histograma da segunda imagem
	normalize(hist1, hist1, 0, histImage.rows, NORM_MINMAX, -1, Mat());

	comparacao=compareHist(hist2,hist1,CV_COMP_BHATTACHARYYA);//compara os dois histogramas 

	if(comparacao*100>tolerancia){//compara para saber se existe movimento
		cout << "Movimento!!" << endl;
	}
	else{
		cout << comparacao << endl;
}
```
[Código Completo][8]

- # Filtragem no domínio espacial I
**Exercício:**
Para esse exercício foi solicitado a criação de uma funcionalidade extra no código dado previamente, de forma que seja permitido calcular o laplaciano do gaussiano da imagem.
Foi acrescentada a opção *p* no menu do código representando a opção de aplicação do filtro laplaciano e para o cálculo foi adicionada uma máscara que será convoluída na matriz da imagem em que se deseja aplicar o filtro.

```C

void menu(){
  cout << "\npressione a tecla para ativar o filtro: \n"
	"a - calcular modulo\n"
    "m - media\n"
    "g - gauss\n"
    "v - vertical\n"
	"h - horizontal\n"
    "l - laplaciano\n"
    "p - laplgauss\n"
	"esc - sair\n";
}
```
```C
float laplgauss[]={0,0,1,0,0,   //máscara usada para fazer o laplaciano do gaussiano
					 0,1,2,1,0,
					 1,2,-16,2,1,
					 0,1,2,1,0,
0,0,1,0,0};
```
```C
case 'p':
	  menu();
      mask = Mat(5, 5, CV_32F, laplgauss);//utilizando a máscara
      printmask(mask);
      break;
    default:
break;
```
[Código Completo][9]

<center>Imagem com filtro laplaciano<img src="https://i.imgur.com/ZBMuC3T.png" style="height:200px;"/></center>





[1]: http://giovanna96.github.io
[2]: http://joaovictor1996.github.io
[3]: https://github.com/joaovictor1996/joaovictor1996.github.io/tree/master/PDI
[4]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/regions/regions.cpp
[5]: https://github.com/joaovictor1996/joaovictor1996.github.io/tree/master/PDI/trocaregioes
[6]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/labeling/labeling.cpp
[7]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/histograma/equalize.cpp
[8]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/histograma/motiondetector.cpp
[9]: https://github.com/joaovictor1996/joaovictor1996.github.io/blob/master/PDI/laplgauss/laplgauss.cpp