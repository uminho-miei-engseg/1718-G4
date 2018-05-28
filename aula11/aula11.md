# Relatório Aula 11

## Experiência 1.1 
O tamanho de cada tipo de dados pode ser visto no _output_ do programa dado:
```
int válido   entre -2147483648 e 2147483647
byte válido  entre -128 e 127
short válido entre -32768 e 32767
long válido  entre -9223372036854775808 e 9223372036854775807
```

## Experiência 1.2

Se for necessário inteiros maiores pode-se utilizar a classe `BigInteger`, que permite representar inteiros com precisão arbitrária. 

## Experiência 1.3

- O valor é truncado para o tamanho da variável de destino, mantendo os bytes menos significativos. 
- Verifica se o valor lido se encontra dentro do intervalo de valores suportado pelo tipo onde vai ser armazenado lançando uma exceção caso tal não se verifique. 

## Pergunta 1.1

Uma vez que os valores de `x` e `y` são `size_t` ou seja, correspondem a um inteiro _unsigned_, pelo que se se chamar a função com valores de `x` ou `y` cujo produto exceda o limite superior deste tipo, pode dar-se o caso do produto `x*y` computado ser inferior ao valor real e, por isso, alocam-se menos bytes do que o pretendido. Nestes casos, é provável que se comecem a escrever em locais inválidos da memória.

```c
int main(){
	vulneravel(NULL,2147483650,2147483650,4);
}
```

Ao executar o código dá _segmentation fault_ porque não e possível alocar a quantidade de memória pretendida. 

## Pergunta 1.2

1. A vulnerabilidade deve-se ao facto de não existir verificação do limite inferior da variável `tamanho`. 

2. Se se passar como argumento `tamanho` o valor `0` à função vulnerável, como na variável `size_t tamanho_real` vai ser armazenado o valor `tamanho-1`, ou seja `-1`, mas como a variável não suporta valores negativos vai ser armazenado um valor muito grande, com o qual não é possível fazer o `malloc`. 

   ``` c
   int main(){
   	char * buf[1024] = "Olá Mundo\n";
   	vulneravel(buff,0);
   }
   ```

   ​

3. Dá _segmentation fault_ porque não e possível alocar a quantidade de memória pretendida. 

## Pergunta 1.3
1. A vulnerabilidade resulta da conversão de valores de variáveis entre tipos _signed_ (`int`) e _unsigned_ (`size_t`).
Uma vez que a variável `tamanho` e `tamanho_real` têm tipos diferentes, `size_t` e `int` respetivamente, caso se passe no argumento `tamanho` um valor que exceda o limite superior de um `int`, na instrução `tamanho_real = tamanho - 1`  a variável `tamanho_real` passa a armazenar uma valor negativo, devido ao cast para um tipo _signed_. No entanto, quando se passa o `tamanho_real` como argumento da função `malloc` ele é convertido para `size_t` e passa a representar um valor positivo, superior ao do limite superior do tipo `int`.

2. Esta vulnerabilidade pode ser despelotada com a seguinte função `main`:
```c
int main() {
  char buf[MAX_SIZE];
  vulneravel(buf,-1);
}
```

3. Durante a execução, obtém-se um _segmentation fault_ resultante de se tentar alocar um bloco de memória muito grande (relembra-se que nas condições indicadas anteriormente, o valor do argumento da função malloc excederá o limite superior dos inteiros com sinal).
