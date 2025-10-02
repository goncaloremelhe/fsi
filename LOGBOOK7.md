**Task 1**

On task 1, we were asked to embed the following java script code into the 'Short Description' field in our profile:

 `<script>alert(’XSS’);</script>`

We did just that, which resulted in the following pop up appearing whenever Alice's profile was opened:

![Task 1](/Images/Logbook7/task1.png)

**Task 2**

On task 2, similarly, we added the following script into our profile:

`<script>alert(document.cookie);</script>`

This made it so that when anyone opened Alice's profile, their cookies would be displayed. This is Boby's view when visiting Alice's profile:

![Task 2](/Images/Logbook7/task2.png)

**Task 3**

On task 3, the objective is to 'listen in' on the user when their cookies are displayed, this is done by attaching an HTTP request to the attacker, with the cookies attached to it. This was accomplished by embedding the following link yet again to Alice's profile, this link sends the cookies to the IP address 10.9.0.1 and port 5555 of our machine.

![Task 3.1](/Images/Logbook7/task3_1.png)

As seen in the following image, we were able to listen in on Boby when their cookies are displayed.

![Task 3.2](/Images/Logbook7/task3_2.png)

**Task 4**

On task 4, we wrote an XSS worm that made it so whenever someone vsisited Boby's profile, they automatically became their friend. To write this script, the first step was to find out what was sent to the server whenever a friend request was dispatched.
By using Firefox's HTTP inspection tool, we were able to find out that the friend request action had the following format:

`http://www.seed-server.com/action/friends/add?friend=USER_ID&__elgg_tokenSECURITY_TOKEN&__elgg_tsSECURITY_TS`

Next, we embedded the script shown in the image in Boby's "About Me" profile field:
![Task 4.1](/Images/Logbook7/task4_2.png)

Then, we logged into charlie's account, looked up Boby and opened their profile:
![Task 4.2](/Images/Logbook7/task4_3.png)

Then, we were able to verify that the XSS worm worked as expected, as Boby was automatically added as Charlie's friend.

![Task 4.3](/Images/Logbook7/task4_4.png)


***Question 1:Explain the purpose of Lines 1 and 2, why are they are needed?***
These 2 lines fetch 2 tokens used to prevent unauthorized actions on Elgg. By including them, our JavaScript makes the server think the request is legitimate. Without them, the server would see the request as suspicious and deny it.

**Question 2:If the Elgg application only provide the Editor mode for the "About Me" field, i.e.,
you cannot switch to the Text mode, can you still launch a successful attack?**
No, because the editor mode adds extra HTML which breaks our java script.



