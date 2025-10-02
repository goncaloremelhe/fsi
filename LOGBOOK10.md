# LOGBOOK10

## Task 1

In the first task, we were asked to compute valid request to the server in www.seedlab-hashlen.com with the following format:

```
http://www.seedlab-hashlen.com/?myname=<name>&uid=<need-to-fill>&lstcmd=1&mac=<need-to-calculate>
```
To complete this request we picked the uid 1005 (which has "xciujk" as its key) and the name JoanaNoites and we calculated the following mac.

![Task 1.1](/Images/Logbook10/task1_creating_mac1.png)

We then used this to make a request to the server, which successfully listed all the files in the server.

![Task 1.2](/Images/Logbook10/task1_valid_mac1.png)

## Task 2

In order to execute the length extension attack, we need to calculate the padding of the message sent in the previous task:

```xciujk:myname=JoanaNoites&uid=10005&lstcmd=1```

It is 43 bytes long, and because the block size of the hashing function used (SHA-256) is 64 bytes long, its padding should measure 21 bytes, with the last byte(s) being the length of the message, which is 43*8 in hexadecimal notation: 0x158. With this information we calculated the padding as such:

![Task 2](/Images/Logbook10/task2_padding_calc.png)

## Task 3

Given that we calculated the padding for the valid request, we will now generate a new MAC based on the one used for the valid request on task 1 with the appended "&download=secret.txt" message (because we also have access to the mac generating process).

![Task 3.1](/Images/Logbook10/task3function.png)

Now the calculated padding comes into play as, for the new MAC (line 15) to be valid, the padding needs to be part of the message (line 14):

![Task 3.2](/Images/Logbook10/task3_link_with_padding_and_new_mac.png)

![Task 3.3](/Images/Logbook10/task3_valid_mac3.png)
