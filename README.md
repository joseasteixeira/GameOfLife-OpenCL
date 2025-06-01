# Game of Life com OpenCL
O Game of Life, proposto por John Conway, é um autômato celular estudado em simulações de sistemas dinâmicos complexos. Sua simplicidade de regras e capacidade
de gerar padrões complexos tornam-no ideal para explorar conceitos em computação paralela. Diante do crescimento da demanda por desempenho computacional, técnicas
que exploram a capacidade massivamente paralela de GPUs vêm sendo cada vez mais aplicadas. Nesse contexto, o presente trabalho propõe uma implementação do Game of Life utilizando a plataforma OpenCL, que permite a execução paralela do código em dispositivos heterogêneos, como GPUs.


## Arquivos do projeto
O projeto está organizado nos seguintes arquivos arquivos: 
- config.txt, este arquivo é responsável por passar parâmetros de entrada para a execução do software, nele são especificados quatro paramentos, separados por um espaço simples, os parâmetros são respectivamente, a altura da matriz, a largura da matriz, a quantidade de interações que serão realizadas, e o intervalo de interações para a capitara da imagem.
- Main.c, este é o arquivo no qual está a função principal, esta função é a primeira a ser chamada ao executar o código, ela realiza a leitura do arquivo index.txt e pega as os dados contidos nele, e utiliza para fazer a comunicação com as funções que estão em outros arquivos fazendo o jogo funcionar. Na função main é calculado o tempo real de execução e salvo em um arquivo de texto. Além disso é responsável pela inicialização do OpenCL.
- Image.c, este arquivo possui o código responsável por gerar as imagens .pbm, e salvá-las.
- grid.c, este arquivo possui o código que gera matriz na qual ocorre todas as interações do jogo.
- logic.c, contem a função gen_next_gpu, responsável por calcular a próxima geração do Jogo da Vida usando a GPU via OpenCL. Ela prepara os dados, envia-os para a GPU, executa o kernel e lê o resultado de volta.
- kernel_gol.cl, neste está a o Kernel responsável por receber duas grades (grid_atual e grid_prox) e as dimensões da grade (width e height); cada work-item calcula o estado da célula em (x, y) na próxima geração; as regras do Jogo da Vida são aplicadas com bordas periódicas.
- gol.h, este arquivo é responsável por fazer a conexão entre os demais arquivos.
## Como executar
Para executar o código é preciso um compilador para a linguagem C e o drive do openCL compativel com a GPU utilizada. Para compilar o código utilizando os parametros do arquivo de texto deve-se usar no terminal o comando: "gcc main.c logic.c grid.c image.c -o gol -lOpenCL" e para executar usa-se o comando: "./gol config.txt" 
Caso queira compilar e executar o codigo por meio da Makefile basta utilizar o comando "make run" no terminal.
### Realização dos testes
Para realizar os testes de execução a GPU utilizada possui as seguintes características:  
- Name: Intel(R) UHD Graphics 620 
- Version: OpenCL 3.0 
- Max. Compute Units: 24 

As dimensões utilizadas para as matrizes foi 2000X2000. E a quantidade de interações foi variada nos seguintes valores: 50, 100, 500, 1000, 5000 e 10000. A grade fou processada sem ser dividida em blocos de colunas para que a granularidade fosse baixa.
Os tempos de execução estão disponiveis no arquivo tempos_gpu.txt.
Para a comparação com a versão serial utilisou-se o código disponivel no repositório: https://github.com/joseasteixeira/GameOfLife/tree/main/C%C3%B3digo%20Serial.
As imagens geradas nos experimentos estão disponíveis na pasta imagens.
