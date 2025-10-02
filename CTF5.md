# CTF5

On the main.c file provided to us we detected a buffer overflow vulnerability. It happens due to the fact that the buffer has a size of 35 Chars, but there's a scanf which reads 45 Chars into the buffer.

## Questions
1. Is there any file that's open and read by the program?

Yes, rules.txt:

```
fun=&readtxt;
(*fun)("rules");
```

2. Is there any way to control the file that is opened?

Yes, we can control the file that will be opened changing the argument "rules" to "flag". This substitution can be made through a Buffer Overflow attack changing the pointer of the 'fun' function to call the readtxt with the string "flag".

3. Is there any buffer overflow? If so what's its vulnerability and what can you do?

Yes. Specifically in the command:

```
char buffer[32];
scanf("%45s", &buffer);
```

Due to the fact that the user's input is copied to the buffer, which has 32 bytes, we can write beyond its limits and modify the 'fun' pointer to call readtxt with "flag" as an argument.


## The exploit

First, we started by finding the address of the readtxt function, which was at 0x80497a5, using the gdb.

![find_readtxt_address](/Images/CTF5/readtxtaddr.png)

Then, it was time to elaborate our payload. In order to run the readtxt with the "flag" argument, we started our payload writing the flag (b"flag\0" with the '\0' string terminator). After that, we filled the rest of the buffer with "A" and finally we wrote the address of the readtxt function. This works because the 'fun' is overwritten with readtxt address and since the buffers starts with "flag\0", the command system("cat flag.txt") is called.

![exploit_code](/Images/CTF5/exploit-template.png)

Finally, using the exploit to access the remote server, we found the hidden flag:

![flag](/Images/CTF5/flag.png)

