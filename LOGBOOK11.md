# Logbook11

## Task 1

For this task, our objective was to become a root certificate authority, CAs are responsible for creating and signing certificates.

Since we needed an openssl.cnf file, we copied the one already existing in the directory /usr/lib/ssl, we then uncommented the unique_subject line, so we could create certificates for the same subject. We also created the necessary directories and the index.txt and serial file.



In order to generate a self-signed certificate we ran the following command:

```
openssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
-keyout ca.key -out ca.crt \
-subj "/CN=www.modelCA.com/O=Model CA LTD./C=US" \
-passout pass:deesopenssl req -x509 -newkey rsa:4096 -sha256 -days 3650 \
-keyout ca.key -out ca.crt
```

We then ran both of these commands in order to print out the decoded content of de x509 certificate and the RSA key:

```
openssl x509 -in ca.crt -text -noout
openssl rsa -in ca.key -text -noout

```

![Task 1](/Images/Logbook11/task1.png)


### What part of the certificate indicates this is a CA’s certificate?

When opening up the ca.crt file, on the basic constraints part, the certificate authority field is set to yes.

### What part of the certificate indicates this is a self-signed certificate?

In the ca.crt file the subject and issuer are the same

### In the RSA algorithm, we have a public exponent e, a private exponent d, a modulus n, and two secret numbers p and q, such that n = pq. Please identify the values for these elements in your certificate and key files

#### ca.crt

Exponent: 65537

Modulus: 

![ca.crt modulus](/Images/Logbook11/task1_cacrt_modulus.png)


The private exponent, primes and coefficient are not displayed for the ca.crt

#### ca.key

Public Exponent:  65537

Exponent 1:

![ca.key exp1](/Images/Logbook11/task1_cakey_exp1.png)

Exponent 2:

![ca.key exp2](/Images/Logbook11/task1_cakey_exp2.png)

Prime 1:

![ca.key prime1](/Images/Logbook11/task1_cakey_prime1.png)

Prime 2:

![ca.key prime2](/Images/Logbook11/task1_cakey_prime2.png)

Coefficient: 

![ca.key coeff](/Images/Logbook11/task1_cakey_coeff.png)


## Task 2

In this task, our objective is creating a certificate signing request (CSR) for our web server in order to get a public key CA, we named our website www.joananoites.com and ran the following command to generate a CSR:

```
openssl req -newkey rsa:2048 -sha256 \
-keyout server.key -out server.csr \
-subj "/CN=www.joananoites.com/O=Joananoites Inc./C=US" \
-passout pass:dees
-addext "subjectAltName = DNS:www.joananoites.com, \
DNS:www.joananoitesA.com, \
DNS:www.joananoitesB.com"
```

## Task 3

Now we needed to get the CA's signature for our CSR, in order to do that we ran the following command:

```
openssl ca -config myCA_openssl.cnf -policy policy_anything \
-md sha256 -days 3650 \
-in server.csr -out server.crt -batch \
-cert ca.crt -keyfile ca.key

```

We can also verify that the alternate named for the website are also present after obtaining the signature.

![Task 3](/Images/Logbook11/task3.png)

## Task 4

Now, in order to deploy our apache HTTPS website, we need to set up the apache_ssl.conf file, we did it as shown.

![Task 4.1](/Images/Logbook11/task4_1.png)

Since the apache's ssl mode was already enabled in the VM, now we only needed to start the apache service. In order to do this, we ran:

```
dockps
docksh<id-of-host-in10.9.0.80>
```

We then started the apache service and deployed our website, however, a warning was displayed, after adding the modelCA's certificate to firefox's trusted certificates we successfully entered our website.

![cert](/Images/Logbook11/task4_cert.png)


![Task 4.2](/Images/Logbook11/task4_2.png)

## Task 5

Now, we will use the same apache server to impersonate example.com, our malicious website, we then, similarly to task 4 added a VirtualHost entry to apache's SSL configuration file: the ServerName is www.example.com, but the rest of the file is the same.

![Task 5.1](/Images/Logbook11/task5_2.png)

Then, to the hosts file in directory /etc, we added the following entry.

![Task 5.2](/Images/Logbook11/task5_1.png)

Now, when we restarted the docker build and run apache again, we then got a warning for insecure connection (which we forgot to screenshot), however, after pressing ignore and continue, we can see there's still a warning beside the link.

![Task 5.3](/Images/Logbook11/task5_3.png)

This shows that the PKI infrastructure can defeat man-in-the-middle attacks.

## Task 6

In this task we assumed our CA certificate was leaked by an attacker, this way they can generate a certificate for the malicious website.

So we did the same thing as before, we generate a CSR and then sign it with our CA, the difference is that now we use the domain example.com instead of www.joananoites.com.

![Task 6.1](/Images/Logbook11/task6_1.png)

![Task 6.2](/Images/Logbook11/task6_really1.png)

We also change the apache_ssl file as so.

![Task 6.3](/Images/Logbook11/task6_3.png)

And the dockerfile.

![Task 6.4](/Images/Logbook11/task6_4.png)

Now, when we start the website using apache again, we can see that firefox didn't detect abnormal activity, this is because our certificate was signed by a trusted CA. With this we can conclude that if a CA certificate is compromised, an attacker can successfully launch a man in the middle attack.

![Task 6.5](/Images/Logbook11/task6_5.png)







