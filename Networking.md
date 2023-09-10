# Networking

## Easy
1. Gosh this one held me up longer than I liked. Connecting to the port gave me a what looked like a very LARGE hexadecimal output. It wasn't until 2 days into the challenge that I realised the output kept changing. Anyway, no deciphering tool helped me here. I finally decided to pipe the connection through xxd -r -p, which reversed the code and showed me the flag!
2. This one was fun. When I connected to the network, it asked me for a password, but also happened to display the authentication code for the previous successful connection! Looked like an md5 hash, so plugging it into an online decoder gave me the password. Entering the password revealed the flag!
   

## Medium
1. This one was a little strange. The instructions wanted me to connect to a network server that would require a password. The instructions also said that the password poslicy required at least one capital letter and four digits, and that the policy is also to force a password change every year. That pointed to the four digit number possibly being the year, and the password therefore likely being Password2023. Well, it didn't work. So I tried Password2022. That didn't work either, so as a last ditch attempt I tried Password2021, and it worked! I guess they haven't updated this challenge in a couple of years?
2. I used the same technique as the first easy challenge to find the flag for this one too!
3. This challenge took longer than it needed to as well. Again, the output from the server was slightly different each time, and very long, made up of numbers separated by dashes. I saved the output to a file, ran a python script to replace the dashes with spaces:

``` python
str='32-12-98-32-....'
print(str.replace('-',' '))
```

I recognized that the numbers had to be octal because there were no 9s, so I plugged it into Cyberchef. The flag was not obvious at all at first, but looking closely I saw that the word Flag was written as galF, and so I was able to copy it, straighten it out and plug it in!

## Hard
1. Challenge wanted us to connect to a network server. The response from the server was "Welcome to the server. Patched with size limits. Please provide your numeric user ID:" so of course I realised I had to do the exact opposite and entered a very long string of As. That yielded the flag! Easy for a hard challenge!
2. This second one was hard. The server I connected to asked for a "challenge". I typed in rubbish output and kept yielding a numeric output of about 10-11 digits. I did it over and over again and the responses kept changing. I eventually realised that some of the expected challenges at least were the current epoch time, without a decimal point. this clearly called for a python script:

```python
import socket
import time

server = 'example.net'
port = 0000

while True:
   s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
   connect = s.connect((server, port))
   s.recv(1024)
   t = int(time.time())
   t = str(t)
   print(t)
   s.send(t + "\n")
   a = s.recv(1024)
   print(a)
   if "Flag" in a:
      break
```
Now, an important thing to note is that I had to run this in python2, with py -2 script.py. The reason being that python3 did not like that I was trying to submit a string value to the server and insisted on it being bytes.
