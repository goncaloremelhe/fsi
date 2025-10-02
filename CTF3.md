# CTF3

Após uma rápida análise do website, foi possível encontrar as versões e os plugins utilizados:

![Versões dos plugins](/Images/CTF3/versionsCTF3.png)

Fig. 1: Versões dos plugins do website

Além disso, foram encontrados dois utilizadores distintos, sendo um deles um administrador.

![Utilizadores](/Images/CTF3/usersCTF3.png)

Fig. 2: Utilizadores do website

Desta forma, foi-nos possível encontrar um CVE com características semelhantes - CVE-2023-2732. Analisando as características do CVE, descobriu-se que o plugin "MStore API" é vulnerável a autenticações não autorizadas para versões até 3.9.2 devido à falta de verificação do utilizador que está a fazer o pedido de API REST durante a "add listing".

Assim, procurou-se um exploit na internet ao qual chegamos a este repositório no GitHub: https://github.com/RandomRobbieBF/CVE-2023-2732
Este repositório continha um programa em Python que, através de pedidos de API REST, fazia com que fossem descobertos os utilizadores da plataforma e como burlar a sua autenticação. No caso do Administrador, era preciso aceder a este link: http://143.47.40.175:5001/wp-json/wp/v2/add-listing?id=1

![Repositório do GitHub](/Images/CTF3/githubCTF3.png)

Fig. 3: Exemplo de abuso da vulnerabilidade

Por fim, já com a autenticação como administrador, só era preciso procurar no perfil do admin pela publicação privada.

![Mensagem do administrador](/Images/CTF3/adminMessageCTF3.png)

Fig. 4: Mensagem do administrador com a flag

Após a conclusão do CTF3, foi-nos possível perceber como funcionam as autenticações não autorizadas e os riscos delas. Acrescentado, percebemos que este tipo de exploits poderiam ser facilmente resolvidos caso os plugin tivessem sido atualizados (como mencionado por outro utilizador), por exemplo.
