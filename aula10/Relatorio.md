# Relatorio Aula 10

## Experiência 1.1

Relativamente ao segmento de _text_, as observações foram as esperadas uma vez que as instruções são as mesmas em todos os programas. 

Relativamente ao segmento de dados, verifica-se que o `size4` possui mais 4 bytes em relação ao `size1` e o `size5` mais 4 bytes em relação ao `size4`. Este facto era expectável uma vez que no `size4` existe uma variável `static int` inicializada, que corresponde a mais 4 bytes na secção de _data_. No `size5` a variável global `int`é também inicializada, correspondendo a um acréscimo  de 4 bytes no segmento _data_.

Em relação ao segmento _bss_ as diferenças entre `size1`, `size2` e `size3` não eram expectáveis, uma vez que se esperava que no `size2` o _bss_ tivesse alocados mais 4 bytes relativamente ao `size1` e menos 4 bytes que o _bss_ do `size3`. Este facto pode dever-se a otimizações do compilador.



## Experiência 1.3

No caso do programa em `Java` o programa termina com uma exceção porque se tenta aceder a um índice inválido do array. O mesmo se verifica no programa em `Python`. No caso do programa em `c++` isto não acontece, porque não existe verificação dos limites do array. Quando o programa escreve na posição 10 do array (que já está fora do limite), na verdade está a escrever na variável `i`, que é mudada para o valor 7, e por isso o programa entra em _loop_. 



## Pergunta 1.1

Ambos os programas (`Java` e `Python`) terminam com um erro se se introduzir um tamanho do array superior ao tamanho com que o mesmo foi inicializado (10). No programa em`c++` verificam-se 2 comportamentos possíveis, dependendo dos inputs:

- O programa termina em _segmentation fault_, se quando escrevemos além do limite do array introduzirmos um valor (que vai ser escrito em `i`), que também ele corresponde a uma valor fora do limite do array.
- O programa entra em loop,  se quando escrevemos além do limite do array introduzirmos um valor (que vai ser escrito em `i`), que se encontre dentro do limite do array. 

Em alguns casos, dependendo dos inputs, é possível escrever além do limite do _buffer_. 



## Pergunta 1.2

 Tanto em `Java` como em `Python`, caso o número de valores a guardar no array seja igual ou inferior ao tamanho do array, o comportamento do programa corre sem problemas. Caso se pretenda guardar mais elementos no array do que o tamanho do mesmo, o programa termina com uma exceção.

No caso do programa em `C++`, caso se insira no número de elementos do array uma valor inferior ao tamanho do array, o programa corre normalmente. Caso se insira uma valor entre [10;20[ o programa entra em _loop_, isto porque como ele escreve em `vals[i] = count-i`, quando o `i` ultrapassa o tamanho do array (10), então ele escreverá na posição da variável `i` o valor `count-i`, ou seja escreverá o número `count-11`, ou seja, o `i` continua dentro dos limites do array.  No caso particular de se inserir no número de elementos o valor 20, quando o `i` chega a 11 ele escreve no valor `i` o resultado de uma subtração entre um valor signed e unsigned, fazendo com que na próxima iteração o valor i seja superior ao count. Por essa razão, caso se pedisse o resultado que estivesse num indice inferior ou igual a 11, o resultado estaria certo mas caso fosse superior, a resposta conteria valores errados dado que o ciclo terminou antes destes valores serem calculados. Caso o valor seja superior a 20, o programa escreve os todos os indices do array e termina em segmentation fault uma vez que se escreveu no endereço de retorno.

Ao contrário do que seria de esperar, o programa não termina sempre em Segmentation Fault, apesar de em todas as execuções em que se passa um valor superior a 10, se escrever para além dos limites do array.



## Pergunta 1.3

- No ficheiro `RootExploit.c` a vulnerabilidade de _buffer overflow_ existe porque a função `gets` não valida o tamanho do input. Por essa razão, é possível escrever na variável `pass` caso se insira um input com tamanho superior a 4. Como para obter as confirmação das permissões de root basta que a variável `pass` tenha uma valor diferente de 0, basta inserir uma string com 5 caracteres. 
- No ficheiro `0-simple.c` a vulnerabilidade existe e é explorada pelas mesmas razões do exemplo anterior, no entanto, o tamanho do input necessário para explorar a vulnerabilidade tem que ser superior em mais do que uma unidade ao tamanho do buffer. Este facto pode estar relacionado com o alinhamento de memória forçado pelo compilador. 

## Experiência 1.5
Na função segura, o output da função `printf` é formatado pela string `"%s"`, pelo que qualquer sequência de formatação no input ("%s", "%d", "%p",...) vai ser lida de forma literal, i.e. o output não vai ser expandido para substituir as ocorrências destes padrões.
Na função vulnerável, como a string passada à função `printf` é diretamente aquela que é lida, caso esta contenha alguma sequência de formatação, esta vai ser substituída por um valor indeterminado, dado que não são passa nenhum argumento adicional a esta função. Abaixo apresenta-se uma imagem que demonstra o que foi dito:

![](./exp1_5.png)



# Pergunta 1.4

Como se pode verificar pelo código, embora a leitura das strings esteja protegida por invocações da função `fgets`, o número de bytes dados no output é apenas limitado pelo número dado como resposta à primeira pergunta do programa. Desta forma, é possível obter o conteúdo de outras posições da memória, mesmo aquelas que estão fora dos limites do array. A imagem abaixo demonstra como é possível ler 120 bytes, apesar do array ter apenas 100 bytes.

![Pergunta 1.4](ex_1_4.png)



# Pergunta 1.5

Depois de compilar o programa, testou-se por tentativa e erro, quantos bytes tem de ter o input para começar a alterar o valor da variável. Constatou-se então que apenas ao 78º byte é que se começa a alterar o valor da variável `control`. Dado que se pretende obter o valor _0x61626364_ (ou *abcd* em ASCII) na variável, temos de inserir uma string qualquer de 78 bytes como input, concatenada com a string _dcba_, uma vez que os inteiros são guardados em memória no formato _little-endian_ (byte menos significativo primeiro).  Abaixo demonstra-se como fazer o que foi descrito:

![](ex1_5.png)



# Pergunta 1.6

Inicialmente, compilou-se o programa com a flag de debug (`-g`) para podermos analisar os endereços das variáveis do programa.  Seguindo passos semelhantes aos da _experiência 7_, verificou-se que a função `win` está no endereço _0x555555554740_. Verificou-se também que se começa a alterar a variável `fp` a partir do 74º byte de input. À semelhança do que se fez antes, converteu-se o endereço da função para ASCII e adicionou-se o resultado invertido (devido à disposição em _little-endian_) a uma string de 73 bytes de forma a mudar o valor do `fp`. O valor ASCII correspondente ao endereço da função `win` é _UUUUG@_ (_@GUUUU_ em _little endian_). Basta então concatenar esta string a outra com 73 bytes para obter o resultado pretendido, tal como se demonstra abaixo:

![](ex1_6.png)

# Pergunta 1.7

Este exercício é em tudo semelhante aos dois anteriores, excepto que desta vez se pretende modificar o valor de retorno da função `main`. 

Compilou-se então o programa com a flag de debug para se obterem os seguintes valores:

- endereço de _buffer_: 0x7fffffffffe0c0
- endereço do _rip_: 0x7fffffffffe108
- endereço de _win_: 0x5555555546f0

Subtraindo o endereço do _buffer_ ao do _rip_, verifica-se que é necessário escrever 72 bytes para começar a alterar o conteúdo do _rip_. Tal como se fez nos exercícios anteriores, escrevemos o endereço da função `win` em ASCII e `little-endian`, de forma a obtermos o sufixo a adicionar a uma string qualquer de 71 bytes.