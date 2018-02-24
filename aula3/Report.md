# Aula Prática 19/Fev/2018

## 1. Assinaturas cegas (Blind signatures) baseadas no ECDLP.
### Pergunta 1.1 

Nesta pergunta pretende-se que se altere o código de forma a que o _input_ e o _output_ sejam simplificados. Assim:

#### Assinante:

  * `initSigner-app.py`
    Neste ficheiro temos o que é necessário para inicializar o processo de assinatura cega. É pretendido que este ficheiro retorne o **R'** utilizado no processo, que corresponde ao ponto da curva aleatóriamente escolhido para cada request. Também é pedido que através da execução de `python initSigner-app.py -init` os componentes _initComponents_ e _pRDashComponents_ sejam calculados e guardados num ficheiro **signer.settings**.
De notar que o utilizador terá que correr primeiro o comando: `python initSigner-app.py -init` para que o estado interno seja criado com o _initComponents_ e _pRDashComponents_ (R'). Ao correr `python initSigner-app.py` este irá ler do ficheiro **signer.settings** o componente R' e irá imprimi-lo no ecrã. Assim o código alterado no ficheiro base foi:
```python
settings_file = "./signer.settings"
initComponents = ""
pRDashComponents = ""

(...)

def init():
    initComponents, pRDashComponents = eccblind.initSigner()
    f = open(settings_file, "w")
    f.write(initComponents + "\n" + pRDashComponents) # Guarda as componentes no ficheiro settings

def load_settings():
    global initComponents
    global pRDashComponents
    f = open(settings_file, "r")
    initComponents = f.readline()
    pRDashComponents = f.readline()
    return initComponentes, prDashComponents
    
def parseArgs():
    if len(sys.argv) == 1: # Programa inicializado sem a opção -init
        load_settings()
        print("pRDashComponents: %s" % pRDashComponents)
    elif sys.argv[1] == "-init": # Quando inicializado com o -init
        init()

(...)
```

* `blindSignature-app.py`
    Este ficheiro é o que contém o código necessário para realizar uma "blind signature". É pretendido que sejam adicionadas as opções `-key <chave_privada>` e `-bmsg BLIND_MESSAGE`, que permitirão o uso de um ficheiro com a chave privada do assinante e ler o ficheiro que contem a blind_message, respetivamente. Deverá retornar ao utilizador a _BlindSignature_ (s). Apesar de ser pedida uma passphrase, ela não foi utilizada, pelo que deverá ser ignorada.
O código alterado foi o seguinte:
```python
def load_settings():
    global initComponents
    global pRDashComponents
    f = open(settings_file, "r")
    initComponents = f.readline()
    pRDashComponents = f.readline()
    return initComponentes, prDashComponents
    
def printUsage():
    print("Usage: python blindSignature-app.py -key <private-key.pem> -bmsg <Blind message>")

def parseArgs():
    if len(sys.argv) == 5 and sys.argv[1] == "-key" and sys.argv[3] == "-bmsg":
        eccPrivateKeyPath = sys.argv[2] # Caminho para a chave privada
        msg = sys.argv[4] # Blind Message
        main(eccPrivateKeyPath, msg)
    else:
        printUsage()
(...)
def main(eccPrivateKeyPath, blindM):
    initCoponents, pRDashComponents = load_settings() # Carrega do ficheiro os componentes
    pemKey = utils.readFile(eccPrivateKeyPath)
    print("Input")
    passphrase = raw_input("Passphrase: ")
    errorCode, blindSignature = eccblind.generateBlindSignature(pemKey, passphrase, blindM, initComponents)
    showResults(errorCode, blindSignature) # Retorna a Blind Signature.
```

#### Requerente
* `ofusca-app.py` É o programa que vai gerar a _Blind Message_ (m'), imprimindo-a no ecrã, e vai guardar as _BlindComponents_ e _pRComponentes_ num ficheiro do Requerente (**req.settings**). Este programa deverá aceitar como opções `-msg <MENSAGEM_ORIGINAL>` e `-RDash <pRDashComponents>` que são a mensagem a ofuscar e o **R'** que é o ponto aleatório da curva elítica selecionado na inicialização, respetivamente. Assim as alterações essenciais ao código foram:
```python
(...)
settings_file = "./req.settings"

def printUsage():
    print("Usage: python ofusca-app.py -msg <mensagem a assinar> -RDash <pRDashComponents>")

def parseArgs():
    if len(sys.argv) == 5 and sys.arv[1] == "-msg" and sys.argv[3] == "-RDash":
        main(sys.argv[2], sys.argv[4])
    else:
        printUsage()

def showResults(errorCode, result):
    if (errorCode is None):
        blindComponents, pRComponents, blindM = result
        print("Blind message: %s" % blindM) # Imprime a blind message no ecrã
        f = open(settings_file, "w") 
        f.write(blindComponents + "\n" + pRComponents) # Guarda num ficheiro as components
        f.close()
    elif (errorCode == 1):
        print("Error: pRDash components are invalid")

def main(data, pRDashComponents):
    print("Input")
    errorCode, result = eccblind.blindData(pRDashComponents, data)
    showResults(errorCode, result)

```

* `desofusca-app.py` É o programa que vai gerar a signature (s') através da _blind signature_ e do **R'**. Para isto deverá utilizar opções como `-s <BlindSignature>` e `-RDash <pRDashComponents>` que são a _BlindSignature_ retornada por `blindSignature-app.py` e o **R'** que é o ponto aleatório da curva elítica selecionado na inicialização, respetivamente. Desta forma foram feitas as seguintes alterações ao código:
```python
(...)
settings_file = "./req.settings"

def printUsage():
    print("Usage: python desofusca-app.py -s <Blind Signature> -RDash <pRDashComponents>")

def parseArgs():
    if len(sys.argv) == 5 and sys.argv[1] == "-s" and sys.argv[3] == "-RDash":
        main(sys.argv[2], sys.argv[4])
    else:
        printUsage()

def showResults(errorCode, signature):
    if (errorCode is None):
        print("Signature: %s" % signature) # Imprime a assinatura
    elif (errorCode == 1):
        print("Error: pRDash components are invalid")
    elif (errorCode == 2):
        print("Error: blind components are invalid")
    elif (errorCode == 3):
        print("Error: invalid blind signature format")

def load_settings():
    f = open(settings_file, "r")
    blindComponents = f.readline()
    pRComponents = f.readline()
    return blindComponents, pRComponents

def main(blindSignature, pRDashComponents):
    print("Input")
    blindComponents, pRComponents = load_settings() # Carrega do ficheiro do Requerente as components
    errorCode, signature = eccblind.unblindSignature(blindSignature, pRDashComponents, blindComponents)
    showResults(errorCode, signature)
```

#### Verificador
* `verify-app.py` É o programa que vai validar se a assinatura obtida com `desofusca-app.py` é válida ou não. Para o efeito serão necessários comandos como: `-cert <certificado do assinante>`, `-msg <mensagem original a assinar>`, `-sDash <Signature>` e `-f <ficheiro do requerente>` que usam o certificado com a chave pública do **Assinante**, a mensagem do Requerente sem ser assinada, a assinatura devolvida pelo `desofusca-app.py` e o ficheiro das settings do requerente (**req.settings**), respetivamente. Desta forma foram feitas as seguintes alterações ao código:
```python
(...)
def printUsage():
    print("Usage: python verify-app.py -cert <certificado do assinante> -msg <mensagem original a assinar> -sDash <Signature> -f <ficheiro do requerente>")

def parseArgs():
    if len(sys.argv) == 9 and sys.argv[1] == "-cert" and sys.argv[3] == "-msg" and sys.argv[5] == "-sDash" and sys.argv[7] == "-f":
        main(sys.argv[2], sys.argv[4], sys.argv[6], sys.argv[8])
    else:
        printUsage()

def load_settings(settings_file):
    f = open(settings_file, "r")
    blindComponents = f.readline()
    pRComponents = f.readline()
    return blindComponents, pRComponents

def showResults(errorCode, validSignature):
    if (errorCode is None):
        if (validSignature):
            print("Valid signature")
        else:
            print("Invalid signature")
    elif (errorCode == 1):
        print("Error: it was not possible to retrieve the public key")
    elif (errorCode == 2):
        print("Error: pR components are invalid")
    elif (errorCode == 3):
        print("Error: blind components are invalid")
    elif (errorCode == 4):
        print("Error: invalid signature format")

def main(eccPublicKeyPath, data, signature, req_file):
    pemPublicKey = utils.readFile(eccPublicKeyPath) # Leitura do certificado
    blindComponents, pRComponents = load_settings(req_file) # Leitura das componentes
    errorCode, validSignature = eccblind.verifySignature(pemPublicKey, signature, blindComponents, pRComponents, data)
    showResults(errorCode, validSignature)
```


## 2. Protocolo SSL/TLS

### Experiência 2.1

Verificamos que o certificado não se encontra revogado, e que embora tenha uma boa escolha de cifras, permite o uso de alguma cifras que o _SSL Labs_ considera fracas. Além disso, só suporta browsers com suporte para SNI.

### Pergunta 2.1
#### i. 
Podem ser encontrados na pasta anexos deste repositório.
#### ii.
##### Ministério da Justiça: justiça.gov.pt
#### iii.
O facto de se encontrar a palavra _DROWN_ na descrição do protocolo, significa que um teste à vulnerabilidade para o _DROWN Attack_ foi feito pelo SSL Server Test. Esta é uma vulnerabilidade que pode ser explorada se o website permitir a utilização do protocolo SSL v2 e TLS simultâneamente, e pode ser explorada de duas formas:

- Um servidor que tenha o SSL v2 ativo, pode ser utilizado para atacar outros servidores que tenham reutilizado a mesma chave RSA, mesmo os servidores que não suportam SSL v2.
- Um servidor que tenha o SSL v2 ativo e que esteja a correr uma versão vulneravél do OpenSSL também pode ser usado para atacar todos os hostnames que apareçam no certificado.

Através do _DROWN Attack_, será possível quebrar a criptografia por de trás da informação e desta forma ler ou roubar informação sensível como, passwords, números de cartões de crédio ou dados financeiros.

Por forma a proteger um servidor contra este tipo de ataque é necessário que os gestores dos servidores tenham garantias que as chaves utilizadas pelos mesmos não entram em contacto com nenhum servidor que utilize o SSL v2. Uma das formas para desativar o uso deste protocolo será através do upgrade do OpenSSL (1.0.2 -> 1.0.2g e 1.0.1 -> 1.0.1s).




 
