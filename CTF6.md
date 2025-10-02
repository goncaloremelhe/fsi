# CTF6

By running checksec we can already guess that we might need to find the address of a function since there's no PIE and, therefore, we can use the same function addresses we find while debugging localy an when we interact with the server.

![checksec](/Images/CTF6/terminal_1.png)

## Questions
1. Is there any file that's open and read by the program?

Yes, "rules.txt".

2. Is there any way to control the file that is opened?

We cannot control the file that is opened when the program expectedly call the readtxt function. However, there is a way to control what function the fun variable points to the the argument it takes the second time it is called. If we do this, we can make the program open not just "rules.txt" but also another file.

3. Is there any format string? If so what's its vulnerability and what can you do?

Yes. Specifically in line 33:

![main](/Images/CTF6/main.png)

```printf(buffer)```

The user-controlled buffer variable is passed to printf, which allows us to insert a payload that makes part of the stack visible and to write to memory.

## The exploit

First, we try finding where in the stack is the string we wrote. By using the "aaaa" string, which is easily spottable (0x61616161) and providing format specifiers (%x) (which the printf does not link any variable to and therefore pops the first elements in the stack), we can find how many bytes we have to pop out of the stack to be able to write to memory.

![find_start](/Images/CTF6/find_start.png)

Here we can see the 0x61616161 value right at the start.

![found_start](/Images/CTF6/found_start.png)

So, now that we know where to write, we'll go into what to write.

If we look back at the main.c file, we can see that the printf we put the payload into is called before the function fun is pointing to is called. Since the echo function is useless to us, we'll try calling readtxt instead. However, we need to first find its address.

![found_readtxt_address](/Images/CTF6/p-readtxt.png)


Note also that *fun is called with our input as an argument. Good! Now we can just change the fun variable to point to readtxt and, at the start of the buffer, include a "flag" string, making the readtxt function read the "flag.txt" file!

However, the address of fun is randomized :(((. Good thing someone forgot to clean up the debug messages! Now we can extract the address from the server's message.

![flag_and_code](/Images/CTF6/flag_and_code.png)

Note: The fmtstr_payload function is building a payload with an offset of 3 4 byte words since those the first 2 are occupied the b"./flag\x01\x01" array and that takes into account that 8 bytes have already been written (because of that same array) so that it can correctly write readtxt's address as fun's value.
