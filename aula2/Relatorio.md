# Proposta resolução da ficha 12 Fev

## 1 - Números aleatórios/pseudoaleatórios

### Pergunta 1.1 
Quando usamos o `/dev/random` verificamos que o computador bloqueia se não existir entropia suficiente na _entropy pool_. Assim, para tentar gerar entropia foram feitas pesquisas e downloads na web, movimentos do rato e digitaram-se caracteres no teclado. Verificou-se que a geração de uma sequência aleatória de 1024 bytes foi bastante demorada. A título experimental, gerou-se uma sequência aleatória de 40 bytes e verificou-se que o tempo necessário para a geração desta era bastante inferior. Este facto leva-nos a supor que o tempo necessário para gerar as sequências é dependente da entropia existente na _entropy pool_.

Quando utilizamos o `/dev/urandom` obtiveram-se imediatamente as sequências pseudo-aleatórias. Isto deve-se ao facto do `/dev/urandom` reutilizar a _entropy pool_ para produzir mais bits aleatórios e, por essa razão, não bloqueia se não existir entropia suficiente.
No entanto estes valores obtidos quando não há entropia suficiente são _teoricamente_ vulneráveis, apesar de não existirem _exploits_ conhecidos.

Assim, apesar do `/dev/urandom` ser criptograficamente seguro, apenas deve ser utilizado quando o desempenho obtido com o uso do `/dev/random/` não satisfizer os
requisitos da aplicação.

### Pergunta 1.2

Depois de instalar o package `haveged`, a obtenção das sequências aleatórias (mesmo com 1024 bytes) foi imediata.

Segundo a página do projeto, o `haveged` utiliza a informação do estado interno dos componentes de hardware como fonte de entropia. Desta forma, permite que a recolha de entropia seja feita de forma mais rápida dado que existem mais fontes.

Assim, podemos confirmar a nossa suposição de que o tempo de geração de sequências aleatórias será tanto menor quanto maior for o número de fontes de entropia disponiveis.

### Pergunta 1.3

A função responsável por gerar a sequência é a `generateSecret` do módulo `shamirsecret` que apenas recebe como argumento o tamanho pretendido para a sequência:  

```python
def main(length):
    sys.stdout.write("%s\n" % shamirsecret.generateSecret(length))
```

Ao analisar o código desta função verifica-se que apenas os bytes que correpondem a caracteres imprimíveis, i.e. que constam em `string.ascii_letters + string.digits`, são adicionados à sequência gerada.

```Python
def generateSecret(secretLength):
"""
This function generates a random string with secretLength characters (ascii_letters and digits).
Args:
    secretLength (int): number of characters of the string
Returns:
    Random string with secretLength characters (ascii_letters and digits)
"""
l = 0
secret = ""
while (l < secretLength):
    s = utils.generateRandomData(secretLength - l)
    for c in s:
        if (c in (string.ascii_letters + string.digits) and l < secretLength): # printable character
            l += 1
            secret += c
return secret
```



## 2 - Partilha/Divisão de segredo (Secret Sharing/Splitting)

### Experiência 2.1
Para dividir o segredo _EngenhariaSegurança_ por duas pessoas, invocou-se o comnado
```bash
php genSharedSecret.php EngenhariaSegurança 2
```
que produziu o seguinte _ouput_:
```bash
Codigo 0: 0011000000100001111000000101101010010000111111101011010110011110011101001110101011010000111111101110110011110100000111010001101101111100100000011000110011011100 
Codigo 2: 0111010101001111100001110011111111111110100101101101010011101100000111011000101110000011100110111000101110000001011011110111101000010010010000100010101110111101
```
Pode-se verificar que é possível obter o segredo a partir das duas sequências geradas. Para tal basta usar o comando 
```bash
php reconstroiSecret.php 0011000000100001111000000101101010010000111111101011010110011110011101001110101011010000111111101110110011110100000111010001101101111100100000011000110011011100 0111010101001111100001110011111111111110100101101101010011101100000111011000101110000011100110111000101110000001011011110111101000010010010000100010101110111101
```
Este comando produz o resultado original. Ao passarmos as duas sequências obtidas e uma terceira (neste caso, replicamos uma das sequências) obtem-se um resultado errado. O mesmo se verifica se só passarmos uma das sequências.

### Experiência 2.2
No segundo caso, utilizou-se o seguinte comando para dividir o mesmo exemplo da experiência anterior por 3 pessoas, sendo apenas necessário 2 para reaver o segredo:
```bash
echo EngenhariaSeguranca | ./shares.pl 2 3 
```
que por sua vez, produziu o seguinte _output_:
```bash
2:1:85641a056309a8b813d9fda692e70e9bbc1e2d:
2:2:c55acea658abeffebe50a6e7bd58abd509dafa:
2:3:045081464d4c354368c84f27e8ca470e5795c6:
```
Para reaver o segredo, fez-se
```bash
perl reconstruct.pl
```
passando-se de seguida as partes do segredo. Verificou-se que se se passarem menos de duas partes, o programa produz uma mensagem de erro, avisando de que o número de partes é insuficiente. Caso se passem mais do que duas, as partes que estão a mais são ignoradas e produz-se o resultado correto. O mesmo acontece quando se passam duas partes. 

### Pergunta 2.1

**A** - Uma vez que um dos argumentos do programa é a chave privada, foi gerado um novo par de chaves através do comando ```openssl genrsa -aes128 -out mykey.pem 1024```, com a _password_ "1234". De seguida, executou-se o programa usando o comando ```python createSharedSecret-app.py 7 3 m1 mykey.pem```. O programa pediu a introdução da _password_ da chave privada ("1234") e de seguida o segredo, sendo que introduzimos: "Agora temos um segredo muito confidencial".

**B** - Ambos os programas ```recoverSecretFromComponents-app.py``` e ```recoverSecretFromAllComponents-app.py``` pedem como argumento o certificado correspondente à chave gerada no passo anterior. Assim, o primeiro passo foi a geração do certificado através do comando ```openssl req -key mykey.pem -new -x509 -days 365 -out mykey.crt```.

- ```python recoverSecretFromComponents-app.py 3 m1 mykey.crt```

  Ao correr este comando de seguida foram introduzidos 3 componentes e recupera-se o segredo "Agora temos um segredo muito confidencial". Também se experimentou correr o programa com um número inferior ao _quorum_, com apenas 2 componentes, e verificou-se que dava erro. 

- ```python recoverSecretFromAllComponents-app.py 7 m1 mykey.crt```

  Depois de se introduzirem os 7 componentes verificou-se que o segredo "Agora temos um segredo muito confidencial" era recuperado. 

O primeiro é usado em situações que se pretenda que o segredo possa ser recuperado com pelo menos _quorum_ utilizadores, não sendo necessário todos os componentes nos quais o segredo foi dividido. O segundo deverá ser usado apenas quando se pretenda que sejam necessárias todas as _n_ partes nas quais o segredo foi dividido para que seja possível recuperá-lo.




#4 - Algoritmos e tamanhos de chaves
### Grupo 4 - Hungria, para as três ECs que emitem certificados "QCert for ESig"

Na Hungria as três ECs que emitem certificados _QCert for ESig_ são:
* Microsec Micro Software Engineering & Consulting Private Company Limited by Shares
    O último certificado concedido por esta EC foi o certificado _e-Szigno Qualified Pseudonymous CA 2017_ e utiliza um algoritmo de geração de chaves públicas usando curvas elipticas. Este gera uma chave de 256 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves é adequado por pelo menos alguns anos. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 512 bits.

* NETLOCK Informatics and Network Privacy services Limited Company
    O último certificado concedido por esta EC foi o certificado _e-Szigno Qualified Pseudonymous CA 2017_ e utiliza um algoritmo de geração de chaves públicas com o RSA. Este gera uma chave de 2048 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves já não é adquado se pensarmos no curto prazo, pelo que uma chave de 3072 bits seria mais apropriada. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 15360 bits.

* NISZ National Infocommunications Services Company Limited by Shares
 O último certificado concedido por esta EC foi o certificado _Állampolgári Tanúsítványkiadó - Qualified Citizen CA_ e utiliza um algoritmo de geração de chaves públicas com o RSA. Este gera uma chave de 4096 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves é apenas adequado se considerar-mos o curto prazo. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 15360 bits.
