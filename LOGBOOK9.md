# LOGBOOK9

## Task 1

In the first task, we were asked to decrypt the file ciphertext.txt with through the substitution cipher. Firstly, we ran the program freq.py in order to see the frequency of sequence of letters and frequency of each letter.

![1LetterGram](/Images/Logbook9/1lettergram.png)

![3LetterGram](/Images/Logbook9/3lettergram.png)

With this information, we started by changing the "ytn" to "THE", because the word "THE" is the most frequent 3-letter word in English. Also, "n" (encrypted) was the most frequent character. In english, "e" is also the most frequent letter, so our assumption was correct.

![First modification](/Images/Logbook9/task1first.png)

Then, using this process of the most frequent words/letters and character matching, we were able to find the solution:

![Solution](/Images/Logbook9/task1sol.png)

Substitution solution:
FROM: vgapnbrtmosicuxejhqyzflkdw
TO:   abcdefghijklmnopqrstuvwxyz

## Task 2

In this task, we were asked to test three different ciphers - aes-128-ecb, aes-128-cbc and aes-128-ctr - and explain their flags and differences during encryptation and decryptation.

For encryptation, besides the -in, -out, -e and -K flags which are common to all the ciphers, CBC and CTR also needed and -iv flag of 16 bytes. CBC takes the IV as an initialization vector and then all the blocks depend on the previous block (the first block uses the IV). CTR takes IV as the value of the nonce.

For decryptation, CBE only needs the -K flag (besides the -in, -out and -d), which makes this encryptation really weak. CBC and CTR in addition need the -iv flag. The difference between CBC and CTR is that if the -iv flag is wrong, in CBC it only affects the first block, whereas in CTR it affects all blocks.

Correct IV on the left, wrong IV on the right for CBC:

![CBC](/Images/Logbook9/task2cbc.png)

Correct IV on the left, wrong IV on the right for CTR:

![CTR](/Images/Logbook9/task2ctr.png)

## Task 5

For this task, we were asked to change the byte 200 (0xC8) for all the encryptations and see how many bytes are corrupted upon decryptation. After encrypting in each mode and changing the byte 200, we saw this modifications on each file:

- ECB: the whole block of the byte 200 is modified - 16 bytes in total;
- CBC: the whole block of the byte 200 is modified and in the next block the 9th byte is modified (byte 200 was also the 9th byte of the block) - 17 bytes in total;
- CTR: only byte 200 is modified - 1 byte in total;

![Modification](/Images/Logbook9/task5.png)