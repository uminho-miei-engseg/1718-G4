# Aula TP - 21/Mai/2018

## Pergunta 1.1 - _String SQL Injection_

Após se ter experimentado com alguns nomes, de seguida experimentou-se utilizar uma tautologia, introduzindo a string `' OR '' = '`, obtendo-se com sucesso a tabela de todos os cartões de crédito. 

## Pergunta 1.2 - _Numeric SQL Injection_

Para conseguir explorar a vulnerabilidade basta abrir o HTML da página e alterar o campo value correspondente à Columbia de forma a que este contenha uma tautologia. Mais concretamente, mudou-se o campo value para a string `101 OR  1=1`, e desta forma obtiveram-se todas as entradas da tabela.

## Pergunta 1.3 - _Database Backdoors_

Na primeira fase verificou-se que o utilizador 101 tem salario 55000. A vulnerabilidade foi explorada com sucesso introduzindo-se como input `101; UPDATE employee SET salary = 56000 WHERE userid = 101`.



## Pergunta 2.1 - _Reflected XSS_

Verificou-se que todos os campos de quantidade as strings são sanitizadas de forma a eliminar caracteres especiais. Esta medida não é utilizada nos 2 ultimos campos de input:  _credit card number_ e _digit access code_. No _credit card Number_ não se obtiveram resultados no ataque, no entanto no campo do _digit access code_ o ataque foi bem sucedido quando se inseriu como input `script> alert("SSA!!!")</script>`.

## Experiência 2.2 - _Stored XSS_

Verificou-se que introduzindo o script `<script language="javascript" type="text/javascript">alert(document.cookie);</script>` no campo _Message_, se se tiver introduzido um titulo da mensagem, então quando alguém clicar na mensagem inserida, o script é executado.  

A vulnerabilidade pode ser explorada se um atacante publicar uma mensagem na plataforma, cujo campo de mensagem é um script que envia os cookies para si.



## Pergunta 3.1 - _Forgot Password_

A conta com mais interesse em comprometer será a do administrador, usualmente _admin_. A password foi obtida com sucesso experimentando diversas cores na resposta à cor preferida, sendo que _green_ foi a resposta certa.  



  