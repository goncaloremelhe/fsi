# CTF 12

Após a análise do código fornecido, tivemos a necessidade de encontrar os pares `p` e `q`. Para isso, utilizamos uma estratégia 'brute-force' de forma a descobrir quais primos satisfaziam as seguintes restrições:
- `p` e `q` são primos;
- `p * q` tem de ser igual a `n`.

Além disso, como sugerido, foi implementado o algoritmo de Miller-Rabin de forma a encontrarmos valores mais rapidamente.

```
def is_prime(n, k=5):
    if n <= 1:
        return False
    if n <= 3:
        return True
    if n % 2 == 0:
        return False

    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False
    return True

def infer_primes(n):
    approx_p = 2**527
    approx_q = 2**528
    l =[]
    for i in range(-1000,1000):
        p1 = approx_p+i
        if (is_prime(p1)):
            for j in range(-1000,1000):
                q1 = approx_q+j
                if (is_prime(q1)):
                    if (p1 * q1 == n):
                        l.append((p1,q1))
    return l
```

Descobrindo os valores de `p` e `q` torna-se possível desencriptar o criptograma. Primeiramente, seria necessário desenvolver uma implementação da função extendedEuclidean(). Nesta função era preciso implementar o algoritmo Euclidiano estendido para calcular o inverso multiplicativo ao `e` no RSA (d):

```
def gcd(a, b):
    if b == 0:
        return a,1,0
    res, x1, y1 = gcd(b, a % b)
    x = y1
    y = x1 - (a // b) * y1
    return res,x,y

def extendedEuclidean(e, phi):
    res,x,y = gcd(e,phi)
    return x % phi
```

De forma a confirmarmos a veracidade dos nossos valores, podemos calcular a expressao `(d*e) mod phi`, que, como esperado, obteve-se o resultado `1`.

Assim, basta só utilizar as mesmas funções usadas na encriptação, uma vez que utilizar a função `enc(x, e, n)` com uma mensagem já encriptada (com os mesmos valores de `p`, `q`), mas desta vez com o `d` (ao contrário do uso do `e` na encriptação), a mensagem volta à original. 

É importante referir que:
- para encriptar uma mensagem pode-se utilizar a expressão `c = m^e mod n` (`c` é o criptograma, `m` a mensagem)
- para desencriptar uma mensagem pode-se utilizar a expressão `m = c^d mod n` (`c` é o criptograma, `m` a mensagem, `d` o inverso multiplicativo de `e`)

Por fim, reutilizou-se a função `enc(cipher_bytes, d, n)` que nos permitiu obter a flag:

```
def enc(x, e, n):
    int_x = int.from_bytes(x, "little")
    y = pow(int_x,e,n)
    return y.to_bytes(256, 'little')

def decrypt_message(e, n, cipher_hex):
    l = infer_primes(n)
    if len(l) > 1:
        print("Alert! More than one tuple")
    (p,q) = l[0]
    phi = (p - 1) * (q - 1)
    cipher_bytes = unhexlify(cipher_hex)
    d = extendedEuclidean(e, phi)
    message = enc(cipher_bytes, d, n)
    print(message.decode())
```

Utilizando o código, conseguimos descobrir a flag:

```
flag{narrhdgvjywfmcsy}
```