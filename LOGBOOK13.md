# Logbook 13

## Tarefa 1.1A

Inicialmente, após a construção dos contentores do Docker, utilizou-se o comando ifconfig de forma a obter a interface alvo, neste caso: `br-04ca65e83730`.

![Interface](/Images/Logbook13/task1ainterface.png)

Após a descoberta da interface, pudemos utilizar o seguinte script python de forma captarmos pacotes ICMP:

![Codigo Task 1.1A ](/Images/Logbook13/task1acode.png.png)

De forma a iniciarmos o código, tivemos de utilizar os comandos:
- `chmod a+x sniff.py` para tornarmos o código executável
- `./sniff.py` com os privilégios do root

