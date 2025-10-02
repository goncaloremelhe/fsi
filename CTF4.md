# CTF4

Após a entrada no website fornecido, rapidamente se percebeu onde poderia estar a vulnerabilidade: no primeiro "form". Isto porque caso o utilizador submeta o formulário sem nenhuma entrada, é realizado o comando de "ls -al". No caso do input ser ";", o comando "ls -al" continua a ser realizado, mas o sistema também realiza um comando de "printenv" por defeito. Além disso, a partir da página inicial percebmos que o sistema usa o Linux como sistema operativo, neste caso, o Ubuntu.

![Página base gerada](/Images/CTF4/printenvCTF4.png)

Fig. 1: Página redirecionada após ";" como input

Desta forma, percebemos que a vulnerabilidade relacionava-se com a injeção de comandos no Ubuntu. Com esta informação, chegamos ao CVE-2014-6271. Esta vulnerabilidade é nomeada como "Shellshock" e descrita como uma vulnerabilidade na linha de comandos Bash que permite o abuso de variáveis de ambiente. No caso do website fornecido, o Shellshock vem da injeção de comandos por meio de scripts de CGI (Common Gateway Interface) que realiza os pedidos através o Bash.

Portanto, depois da descoberta do CVE, pudemos, então, abusar da vulnerabilidade descoberta. Pensámos que a chave poderia estar em algum documento por exemplo "flag.txt". Assim, no formulário inicial realizámos o input "; find / -name flag;" ficando como resultado final os comandos "ls -al; find / -name flag;". Deste modo, descobrimos que dentro do diretório /var existe outro diretório com o nome de "flag". 

![Procura da flag](/Images/CTF4/findCTF4.png)

Fig. 2: Pesquisa da localização da flag

Acrescentando, desta vez realizamos o comando "/var/flag" (ficando "ls -al /var/flag") pelo que descobrimos que existe um ficheiro de texto com o nome "flag.txt".

![Localização da flag](/Images/CTF4/textCTF4.png)

Fig. 3: Localização exata da flag num ficheiro de texto

Por fim, realizou-se o input "sudo cat /var/flag/flag.txt", no qual permitiu-nos obter, assim, a flag.

![Flag](/Images/CTF4/catCTF4.png)

Fig. 4: Conteúdo do ficheiro flag.txt

Apesar do grande impacto que este CVE pode ter, o problema poderia ser resolvido através da sanitização do "input" no formulário para caracteres maliciosos, tais como ";" ou "|".