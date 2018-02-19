# Aula Prática 19/Fev/2018

## 1. Assinaturas cegas (Blind signatures) baseadas no ECDLP.
### Pergunta 1.1 

Nesta pergunta pretende-se que se altere o código de forma a que o _input_ e o _output_ sejam simplificados. Assim:

#### Assinante:
   
  * `initSigner-app.py`
    Neste ficheiro temos o que é necessário para inicializar o processo de assinatura cega. É pretendido que este ficheiro retorne o **R'** utilizado no processo, que corresponde ao ponto da curva aleatóriamente escolhido para cada request. Também é pedido que através da execução de `python initSigner-app.py -init` os componentes _initComponents_ e _pRDashComponents_ sejam calculados e guardados num ficheiro **settings**.
De notar que o utilizador terá que correr primeiro o comando: `python initSigner-app.py -init` para que o estado interno seja criado com o _initComponents_ e _pRDashComponents_ (R'). Ao correr `python initSigner-app.py` este irá ler do ficheiro **settings** o componente R' e irá imprimi-lo no ecrã. Assim o código alterado no ficheiro base foi:
```python
settings_file = "./settings"
initComponents = ""
pRDashComponents = ""

...

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

...
```

* `blindSignature-app.py`
    Este ficheiro é o que contém o código necessário para realizar uma "blind signature". É pretendido que sejam adicionadas as opções `-key <chave_privada>` e `-bmsg BLIND_MESSAGE`, que permitirão o uso de um ficheiro com a chave privada do assinante e ler o ficheiro que contem a blind_message, respetivamente. Deverá retornar ao utilizador a _BlindSignature_ (s).
O código alterado foi o seguinte:

- Requerente
-- `