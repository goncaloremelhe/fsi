# CTF8

Inicialmente, tentamos procurar plugins/softwares utilizados pelo website, pelo que descobrimos:
- WordPress v6.7.1
- Notificationx v2.8.1

A partir desta informação, procuramos algum CVE que se relacionasse com as características do website vulnerável. Assim, descobrimos o CVE-2024-1698, que relata a vulnerabilidade de 'SQL injection' a partir do plugin Notificationx até à versão 2.8.2. O problema surge do facto das 'queries' de SQL não serem propriamente sanitizadas. O endpoint vulnerável torna-se assim o seguinte: `/wp-json/notificationx/v1/analytics`. Além disso, este CVE é catalogado como CWE-89 (sanitização imprópria).

Utilizando o código python deste repositório no github - https://github.com/kamranhasan/CVE-2024-1698-Exploit - pudemos descobrir a hash referente à password do utilizador 'admin':

![Admin](/Images/CTF8/admin.png)

Analisando a hash obtida, percebemos que o servidor guarda as passwords com hash de PHPass, na qual utiliza o prefixo $P (hash gerada por PHPass) e $B representa o contador de iterações. Além disso, é seguido de um 'salt' de 8 caracteres - 'uRuB0Mi3'.

Apesar do uso de hash ser mais seguro do que a password literal e mesmo com o uso de 'salt' (já que as hash são irreversíveis), elas não são infalíveis. Dicionários de passwords comuns (através de 'data leaks') tornam este processor mais falível. A partir do uso do dicionário 'rockyou.txt' e o uso do 'hashcat', foi possível encontrar a password referente ao admin (hash:password):

```
hashcat -m 400 -a 0 '$P$BuRuB0Mi3926H8h.hcA3pSrUPyq0o10' rockyou.txt
```

O uso das flags '-m 400' especifica o algoritmo utilizado no PHPass e '-a 0' ativa o modo de ataque baseado no uso em dicionário.

![Password](/Images/CTF8/password.png)
