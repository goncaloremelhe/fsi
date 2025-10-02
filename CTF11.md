# CTF11

## Tarefa 1

Ao investigar o `cipherspec.py` percebeu-se o problema do mesmo: apenas são usados os últimos 3 bytes de forma aleatória aquando da geração. Os outros 13 são inicializados a 0, tornando esta encriptação suscetível a ataques de força bruta.

```
KEYLEN = 16

def gen(): 
	offset = 3 # Hotfix to make Crypto blazing fast!!
	key = bytearray(b'\x00'*(KEYLEN-offset)) 
	key.extend(os.urandom(offset))
	return bytes(key)
```

Uma das formas de encriptar poderia ser da seguinte forma:

```
key = gen()
nonce = unhexlify("a3bac1c9692695ec4392af380d2c3019")
message = b"flag{teste}"
ciphertext = enc(key, message, nonce)
```

E para decifrar:

```
message = unhexlify("f3c4fbf2eb40ed02bed375359d92fa09200245c57b48")
solution = dec(key, message, nonce)
```

Como o a funcao `gen()` só utiliza 3 bytes aleatórios, um ataque de força bruta torna-se muito fácil de criar. Gera-se todas as chaves possíveis com 3 bytes e testa-se se o resultado da mensagem está no formato `flag{...}`:

```
def brute_force(nonce, ciphertext):
    prefix = b'\x00' * 13
    for suffix in product(range(256), repeat=3):
        key = prefix + bytes(suffix)
        try:
            plaintext = dec(key, ciphertext, nonce)
            if plaintext.startswith(b"flag{") and plaintext.endswith(b"}"):
                return plaintext.decode(), key.hex()
        except Exception:
            pass
    return None
```

Utilizando esta função, foi possível descobrir a flag procurada, bem como a chave associada:

![Flag e key](/Images/CTF11/flag.png)


## Tarefa 2

Alterando o código da função feita (utilizando o módulo time), percebemos que o são vistas 1 595 168 chaves num total de 115 segundos, isto é, 13 871 chaves por segundo.
Além disso, 10 anos tem aproximadamente 315 360 000 segundos.
Assim, em 10 anos são vistas 4 374 366 786 782 chaves.
Finalmente, será preciso um offset mínimo de 6 bytes (2^(8*offset) > 4.3 * 10^12).

## Tarefa 3

Apesar da complexidade do sistema aumentar utilizando um byte aleatório, esta 'otimização' torna-se ineficiente, uma vez que o atacante tem de testar 256 mais vezes todas as chaves possíveis, mas o próprio utlizador também terá de os testar já que o nonce não é enviado, isto é, adiciona um custo computacional tanto ao atacante como ao utilizador. Assim, esta medida torna-se minimamente eficaz para aumentar a segurança do sistema, mas muito pouco eficiente.
