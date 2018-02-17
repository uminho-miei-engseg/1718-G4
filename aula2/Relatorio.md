# Proposta resolução da ficha 12 Fev

## 1 - Números aleatórios/pseudoaleatórios

### 1.1 
Quando usamos o `/dev/random` verificamos que o computador bloqueia se não existir entropia suficiente na _entropy pool_. Assim, para tentar gerar entropia foram feitas pesquisas e downloads na web, movimentos do rato e digitaram-se caracteres no teclado. Verificou-se que a geração de uma sequência aleatória de 1024 bytes foi bastante demorada. A título experimental, gerou-se uma sequência aleatória de 40 bytes e verificou-se que o tempo necessário para a geração desta era bastante inferior. Este facto leva-nos a supor que o tempo necessário para gerar as sequências é dependente da entropia existente na _entropy pool_.

Uma vez que o `/dev/urandom` é independente da fonte de entropia a obtenção das sequências aleatórias foi imeadiata, como seria expectável. Após uma breve pesquisa, ficamos a saber que no entanto estes valores obtidos quando não há entropia suficiente são _teoricamente_ vulneráveis, apesar de não existir literatura que o prove.

Assim, apesar de `/dev/urandom` ser criptograficamente seguro ele apenas deve ser utilizado quando a rapidez for valorizada no desenvolvimento da aplicação que irá utilizar o gerador. Quando a aplicaçaõ precisar de segurança elevada a outra opção será mais indicada.

### 1.2

Depois de instalar o package `haveged`, a obtenção das sequências aleatórias (mesmo com 1024 bytes) foi imediata.

O `haveged` utiliza a informação do estado interno dos componentes de hardware como fonte de entropia, permite que a recolha de entropia seja feita de forma mais rápida dado que existe uma fonte maior.

Assim, podemos confirmar a nossa suposição de que o tempo de geração de sequências aleatórias será menor quanta maior for a entropia disponivel.

## 4 - Algoritmos e tamanhos de chaves
### Grupo 4 - Hungria, para as três ECs que emitem certificados "QCert for ESig"

Na Hungria as três ECs que emitem certificados _QCert for ESig_ são:
* Microsec Micro Software Engineering & Consulting Private Company Limited by Shares
    O último certificado concedido por esta EC foi o certificado _e-Szigno Qualified Pseudonymous CA 2017_ e utiliza um algoritmo de geração de chaves públicas usando curvas elipticas. Este gera uma chave de 256 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves é adequado por pelo menos alguns anos. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 512 bits.

* NETLOCK Informatics and Network Privacy services Limited Company
    O último certificado concedido por esta EC foi o certificado _e-Szigno Qualified Pseudonymous CA 2017_ e utiliza um algoritmo de geração de chaves públicas com o RSA. Este gera uma chave de 2048 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves já não é adquado se pensarmos no curto prazo, pelo que uma chave de 3072 bits seria mais apropriada. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 15360 bits.

* NISZ National Infocommunications Services Company Limited by Shares
 O último certificado concedido por esta EC foi o certificado _Állampolgári Tanúsítványkiadó - Qualified Citizen CA_ e utiliza um algoritmo de geração de chaves públicas com o RSA. Este gera uma chave de 4096 bits de comprimento. Este tipo de algoritmo com este comprimento de chaves é apenas adequado se considerar-mos o curto prazo. Para uma solução mais duradoura, o comprimento da chave podia ser aumentado para os 15360 bits.