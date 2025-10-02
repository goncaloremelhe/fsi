
Task 1

Como é pedido nesta task, corremos os seguintes comandos para imprimir as variáveis de ambiente

![Task 1](/Images/Logbook4/task_1.png)

Task 2

Depois de seguir os passos 1 e 2 e correr o comando diff com os ficheiros antes e depois das
alterações pedidas é possível concluir que não há diferença entre os ficheiros.
A conclusão que podemos tirar é que o processo child herda as variáveis de ambiente do processo parent

![Task 2](/Images/Logbook4/task_2.png)

Task 3

Ao correr o programa sem mudar o código nada é imprimido, mas seguindo o passo 2
(mudando a variável NULL para environ) podemos concluir que mudamos o contexto do programa
já que agora quando corremos myenv.c a variável de ambiente é imprimida

Task 4

Ao compilar e correr o programa podemos verificar que as environment variables do
calling process são imprimidas

Task 5

Depois de compilar o código dado no primeiro passo desta tarefa e em seguida mudar
a propriedade do programa para root com os comandos dados no segundo passo, depois
alteramos as variáveis de ambiente PATH,LD_LIBRARY_PATH e HELLOWORLD.

![Task 5.1](/Images/Logbook4/task_5.png)


Depois de correr o programa a que demos propriedade de root podemos verificar que
as variáveis PATH e ANY_NAME se foram imprimidas e podemos visualizar os seus valores

![Task 5.2](/Images/Logbook4/task_5_2.png)

![Task 5.3](/Images/Logbook4/task_5_3.png)

Porém a variável LD_LIBRARY_PATH não foi imprimida, isto significa que ela não foi
herdada pelo processo child

Task 6

Como é pedido nesta task, criamos um ficheiro chamado task6.c com o código fornecido, compilamos,
mudamos para propriedade root e fizemo-lo um programa SET_UID. Também mudamos a variável PATH para
que apontasse para home/seed

![Task 6.1](/Images/Logbook4/task_6_1.png)


Depois, no diretório home/seed criamos outro ficheiro (ls.c) com o seguinte código, que vai ser o
código malicioso que queremos correr, e compilámo-lo nesse mesmo diretorio

![Task 6.2](/Images/Logbook4/task_6_2.png)
![Task 6.3](/Images/Logbook4/task_6_3.png)

Depois, voltando ao diretório em que se encontra task6.c, podemos observar no screenshot que corremos o ficheiro task6
(obtido por compilação do task6.c) duas vezes: a primeira foi quando ainda não tínhamos compilado ls.c no diretório
home/seed, e segunda depois da sua compilação. É possível verificar que por haver um executável no diretório para o qual
a variável PATH aponta (home/seed) foi executado o código malicioso em vez de /bin/ls

![Task 6.4](/Images/Logbook4/task_6_4.png)

Task 8

Verificando que temos a possibilidade de introduzir texto, que no caso do programa fornecido é previsto ser argumentos para o comando cat, experimentámos introduzir o comando rm na linha seguinte.

![Task 8.1](/Images/Logbook4/task8_1.png)

Como podemos ver, somos capazes de injetar comandos da shell porque a chamada system() chama os seus argumentos na shell e, assim, alterar ficheiros.

No entanto, quando é utilizada a chamada execve(), já não conseguimos fazer o mesmo porque esta não recorre à shell para executar comandos.

![Task 8.2](/Images/Logbook4/task8_2.png)





