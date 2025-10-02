**Lab set-up**

We started by turning off the address randomization using the following command:

$ sudo sysctl -w kernel.randomize_va_space=0

After compiling task.c, we set up our lab environment by running dcbuild and dcup in another terminal

![Lab set-up 1](/Images/Logbook6/task1_1.png)
![Lab set-up 2](/Images/Logbook6/task_1_2.png)

**Question 1**

**Task 1**

With the objective of crashing the server when it invokes myprintf(), we ran the given python program (build_string.py) and ran the command cat badfile on the server, sucessfully crashing the program, this is made apparent by the program not printing the 'Returned properly (^_^) (^_^)' message

![Task 1.1](/Images/Logbook6/task_1_4.png)
![Task 1.2](/Images/Logbook6/task_1_3.png)

**Task 2A**

We selected the input 0xaabbccdd so that we could easily identify it and we determined that to exploit the vulnerability on myprintf() function we need 64 %x format specifiers so that the first 4 bytes of our input are printed

![Task 2A.1](/Images/Logbook6/task_2a_2.png)
![Task 2A.2](/Images/Logbook6/task_2a_3.png)

**Task 2B**

Since our objective is printing the secret message, first, we need to find its address which is located on the server printout (0x080b4008), next, we need to place it in the format string so that it is stored in the stack, in order to do that, we changed the value of the variable number in the file build_string.py to 0x080b4008. Then we echoed the %x format specifiers 63 times and were able to print the message "A secret message"

![Task 2B.1](/Images/Logbook6/task_2b_1.png)
![Task 2B.2](/Images/Logbook6/task_2b_2.png)

**Task 3A**

On this task we were instructed to change the value of the target variable, we started by locating its address on the server printout (0x080e5068), then, we placed it in the start of the format string similarly to the previous task, we also used the value discovered previously (64) to know where the address of the target variable was going to be stored, then instead of using the %x or other format specifiers which print out the value found in the stack, we used %n which reads that value and uses it as an address in which to store information (the number of characters in the string before %n)

![Task 3A.1](/Images/Logbook6/task_3a_1.png)
![Task 3A.2](/Images/Logbook6/task_3a_2.png)

**Task 3B**

On this task the goal is to change the target variable's value to 0x0005000, in order to accomplish that we need to be precise about the number of bytes that are writen before the %n specifier in the string. In this case, the number is 20480 in decimal, which is a problem since we can only send 1500 chars. Therefore, we used the width argument of one of the %x specifiers to achieve that length of written chars with fewer bytes sent.

![Task 3B.1](/Images/Logbook6/task_3b_1.png)
![Task 3B.2](/Images/Logbook6/task_3b_2.png)


**Question 2**

If the format string isn't alocated in the stack, we can't make the format specifiers reference our input so we can only reliably make the program crash and read stack values since it's much more likely that the byte in the stack that is read by overloading printf with format specifiers is an invalid pointer.


