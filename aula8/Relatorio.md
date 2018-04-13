## Pergunta 1

Tendo em conta a estimativa apresentada na aula e de acordo com o site https://informationisbeautiful.net/visualizations/million-lines-of-code/:
- Linux 3.1 - 15 milhões LOC -> 75 - 750 mil bugs
- Facebook - 61 milhões LOC -> 300 mil a 3 milhões bugs
- Car software - 100 milhões LOC -> 500 mil a 5 milhões bugs
- Google - 2000 milhões LOC -> 10 milhões a 100 milhões bugs

## Pergunta 2

* Vulnerabilidades de Projeto:
**CWE - 266: Incorrect Privilege Assignment**, permissões são incorretamente atribuídas a um utilizador criando uma esfera de controlo não pretendida para o mesmo. Deste modo o utilizador pode aceder a funcionalidades restritas ou a informação sensível.
A correção passa por, na fase de planeamento, ser rigoroso no processo de atribuição de permissões, explicitando como a gestão das zonas de confiança deve ser feita no software.
**CWE - 188: Reliance on Data/Memory Layout**, durante o planeamento é possível que se baseie o desenho do software com base em assunções inválidas a cerca da forma como a memória está organizada ou disposta. Propício a acontecer quando se pretende desenvolver software multi-plataforma, nas quais o modelo de organização de memória pode ser diferente.
A correção passa por explicitar as assunções sobre o modelo de memória e verificar a compatibilidade das assunções com as plataformas alvo.

* Vulnerabilidade de Codificação:
**CWE-120: Buffer Copy wihtout Checking Size of Input ('Classic Buffer Overflow')** - o programa copia de um _input buffer_ para um _output buffer_ sem verificar que o tamanho do primeiro é inferior ou igual ao tamanho do segundo.
Para corrigir e mitigar esta vulnerabilidade, sempre que se faz uma operação que armazene dados num buffer, verificar que o tamanho do mesmo é suficiente para os dados que se pretendem armazenar.
**CWE-20: Improper Input Validation** - o software não valida ou valida incorretamente os _inputs_, o que pode levar a alterações no fluxo de controlo ou dados do programa.
Como correção podemos, no caso de caracteres especiais usar uma função que verifica se o input não possui caracteres sensíveis, com base numa "blacklist" de caraceteres. Também poderá ser criada uma "whitelist" de inputs aceitáveis, conforme a especificação.7

* Vulnerabilidade Operacional
**CWE-555: J2EE Misconfiguration: Plaintext Password in Configuration File** - a aplicação J2EE guarda a password num ficheiro de configuração.
Como correção, utilizar bibliotecas standard para cifrar as passwords antes de as guardar em ficheiros de configuração.
**CWE-5: J2EE Misconfiguration: Data Transmission without Encryption** - poderá existir o comprometimento da informação enviada. Um atacante pode conseguir ler ou modificar o conteúdo dos dados se forem enviados como plaintext.
Como correção, a configuração da aplicação deverá assegurar que o SSL está em utilização sempre que se transmite dados.

## Pergunta 1.3

Uma vulnerabilidade dia-zero é uma em que as partes interessadas em resolver a vulnerabilidade não estão cientes 
da existência da mesma. Estas vulnerabilidades podem ser particularmente nefastas quando comparadas com as
que não são dia-zero dado que qualquer ataque baseado nestas vulnerabilidades demorará, à partida, mais tempo a 
resolver. Isto deve-se ao facto de que as entidades que pretendem corrigir a vulnerabilidade terem que determinar a
sua origem e só depois estarão em condições de a corrigirem. Enquanto tal não acontecer, os atacantes poderão 
tirar partido desta vulnerabilidade.

