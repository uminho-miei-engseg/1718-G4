# Pergunta 1
## P1.1

Não é possível garantir que estamos localizados nos Estados Unidos (EUA), porque este comando não permite especificar os _ORs_ utilizados no protocolo *TOR*. No entanto, o protocolo permite que tal aconteça se se implementar uma aplicação que escolha, de entre os _ORs_ registados no _Directory Server_,  um servidor que esteja nos Estados Unidos como último _OR_.

## P1.2
- Circuito quando se acede ao site http://zqktlwi4fecvo6ri.onion/wiki/index.php/Main_Page	![Circuito](pergunta1.2/wiki.png) 

  ​

- Circuito quando se acede ao site https://www.facebookcorewwwi.onion/![Circuito](pergunta1.2/facebook.png) 



Para aceder ao serviço anónimo, o utilizador do serviço começa por aceder ao Directory Server para extrair informação sobre os _Introduction Points_ (IP)  e a chave pública do serviço anónimo XYZ.onion. De seguida, cria um circuito TOR até um _Rendez-Vouz_ point (RP) - para conexão com o serviço anónimo - fornecendo-lhe um _rendez-vouz cookie_ (um segredo aleatório único para posterior reconhecimento do XYZ.onion). O utilizador do serviço abre uma stream até um dos IP's do serviço anónimo a quem envia uma mensagem, cifrada com a chave pública do serviço anónimo, com a informação sobre o RP, o _rendez-vouz cookie_ e a sua parte da chave de sessão Diffie-Hellman. O serviço anónimo, para responder ao utilizador, constrói um circuito TOR até ao RP do utilizador, enviando uma mensagem com o _rendez-vouz cookie_, a sua parte da chave Diffie-Hellman e o Hash da chave partilhada. Assim, existe entre o utilizador e o serviço anónimo um circuito de 6 OR's onde cada uma das partes tem apenas conhecimento de metade do circuito. Desta forma, o utilizador do serviço anónimo tem conhecimento dos três OR's até ao RP, mas a partir daí os saltos são _rellay_ porque o utilizador não tem conhecimento dos mesmos, e como o circuito TOR do RP até ao serviço anónimo é de três OR's existem três saltos rellay. 