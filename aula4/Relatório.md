# Pergunta 1
## P1.1

Não é possível garantir que estamos localizados nos Estados Unidos (EUA), porque este comando não permite especificar os _ORs_ utilizados no protocolo *TOR*. No entanto, o protocolo permite que tal aconteça se se implementar uma aplicação que escolha, de entre os _ORs_ registados no _Directory Server_,  um servidor que esteja nos Estados Unidos como último _OR_.

## P1.2
3 saltos que o OP conhece
3 saltos do servidor até ao RP

Protocolo começa por perguntar ao Directory Server quem tem conhecimento do ASDASDASD.onion, este reposte com algum IP que o conhece. Alice partilha com uma cookie onde está o seu RP e a sua parte da chave ao qual o servidor vai responder através dos relays points com o a sua parte da chave DH, para o RP. Depois a comunicação começa pelo canal dos seis ORs, onde cada uma das partes tem apenas conhecimento de metade do circuito.