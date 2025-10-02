# CTF10

Inicialmente, vimos que o ficheiro continha apenas caracteres especiais que deveriam representar letras, assim, percebemos que este CTF se tratava de uma cifra de substituição. Daí, utilizamos o código python do SeedLab "Secret Key Encryption Lab" (freq.py) de forma a sabermos a frequência dos caracteres e outros n-gramas:

![Frequencia](/Images/CTF10/freq.png)

A partir dos resultados obtidos, presumimos que os caracteres mais frequentes ':', ';' e '@' representariam as letras mais frequentes da língua portuguesa - 'a', 'e' ou 'o' - apesar de não sabermos qual representaria qual.

![Letras Frequentes](/Images/CTF10/mostfrequent.png)

Para isso, reparamos nas sequencias de 4 caracteres e vimos o '*;,;' que poderia ser tanto 'para' como 'como', no entanto, como o caracter ',' não é tão frequente, tentamos com a palavra 'como'. Além disso, como ':' é significamente mais frequente do que '@', tentamos ':' como 'a' e '@' como 'e'.

![Primeira Troca](/Images/CTF10/first.png)

Como podemos reparar na imagem anterior a secção 'ecomo=eaccao' reparamos que o '=' só poderia ser 'r' (formando 'e como reaccao').

![Segunda Troca](/Images/CTF10/second.png)

Desta vez, a secção 'comore/]meracao' podia-se assimilar como 'como renumeracao', bem como a expressão '|orma' só poderia ser 'forma'.

![Terceira Troca](/Images/CTF10/third.png)

Agora, com a expressão '$rocura' percebemos que o caracter '$' representa o 'p' e a expressão 'f(nance(ra' precisava de uma vogal, no caso o 'i', que era última vogal em falta.

![Quarta troca](/Images/CTF10/fourth.png)

Assim, com todas as vogais descobertas e grande parte das consoantes também já descobertas testamos os restantes caracteres, obtendo a seguinte soluçao:

```
reditos no oriente face a suspensao da ajuda financeira do ocidente como reaccao a agressao sovietica nas republicas do baltico existe tambem a tendencia para qualificar o credito do kuwait como renumeracao da posicao sovietica na coligacao anti iraquiana o credito sul coreano e tambem considerado como um bonus concedido a urss devido ao levantamento do impedimento da adesao da coreia as nacoes unidas quem tem medo de andar de elevador nao procura ajuda psiquica embora esse medo seja algo irracional ele nao colide de forma violenta como bem estar ou com a autonomia da pessoa mas ha fobias que
{ahdwvjmfeebvfmug}
```

Finalmente, percebemos que a flag seria

```
flag{ahdwvjmfeebvfmug}
```