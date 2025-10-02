
# Trabalho realizado nas Semanas #2 e #3


## Identificação

-Vulnerabilidade CSRF, juntamente com falta de verificação de credenciais.

-Não é verificada a existência de uma token CSRF no pedido para alterar a palavra passe da conta atual.

-Pode afetar qualquer browser comum, desde que o utilizador esteja autenticado em casdoor.org (versões até 1.331.0).

## Catalogação

-omriman067 a 09/02/2023 (https://github.com/casdoor/casdoor/issues/1531)

-CVSS base score: 6.5

-Esta vulnerabilidade foi publicada juntamente com um exploit.

-Nota: Apesar da facilidade com que a vulnerabilidade podia ser resolvida, não o foi até 8 meses a seguir a ser publicada. Além disso, a reportagem foi etiquetada como uma pergunta em vez de como um bug.

## Exploit

-Como explicado no link na catalogação, temos que fazer com que um utilizador autenticado corra um programa html que envia um pedido de troca de password.

-Este pedido é automaticamente aceite por não haver qualquer cross site request forgery token nem verificação da password atual.

## Ataques

-Não foi encontrado nenhum ataque com sucesso.

-Devido a este website ser um Identity Access Manager e as suas contas poderem estar associadas ao Google Workspace, Steam, Produtos Meta, Gitlab, entre outros, um hipotético ataque seria bastante grave, podendo comprometer informações importantes de qualquer utilizador.


