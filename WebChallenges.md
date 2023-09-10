# Web Challenges

There were quite a few web challenges:

## Easy
1. For this first challenge I was directed to login to a webpage, and looking through the source code I found two scripts- one JS and the other that looked like variables with hexadecimal code?
   Looking through these variables I found one thatreturned the flag as a function. Typing the variable into the console gave me the flag!
2. The second one also took me to a website, this time no login page but simply a gif. Looking at the source code revealed that the gif was stored in the "img" directory. Running dirbuster on the url revealed a "security" directory under which was the flag in a txt file.
3. For the third challenge, looking through the source code did not help. Remembering what I'd learned pretty early on in my dive into the world of cybersecurity, I tried navigating to a "robots" file and it worked! The robots.txt page contained the directory extension which led me to the flag.
4. The fourth challenge was reallly simple, more than the previous three actually, I thought. The source code had the flag in the html head!
5. For the fifth challenge, the source code of the webpage revealed a JS titled "secret.js", which obviously received my attention. Looking at the short javascript code revealed a function dontrunme() which would reveal the flag, which it did.
6. The sixth and final "easy" challenge had me stumped for a minute! The webpage said that I had to be an admin to access the flag. Running the webpage through a proxy allowed me to change the useragent variable from "User" to "admin" which revealed the flag! The tricky bit here was I had to pay attention to the case sensitivity!

## Hard
1. This hard challenge had me stumped for a few hours actually. It was another page to login with a password. The source code showed right the username right away, but the password was a little trickier, as it was equal to a variable "pw". That being a variable, however, pointed me to the console and sure enough, inputing the variable yielded the password, and logging in yielded the flag!

## Extra Hard
1. This challenge took me to a webpage asking just for a password. The webpage showed the password salt and hash, and that the password was encrypted with md5. Since a hash can't be reversed, I had to somehow use the given hash function to get A password (not necessarily THE password), since all the password had to do was provide a hash value of the same length and the same beginning, which was 0e. A python script seemed like the solution:
   Now, I'm new to python, so I had to check and see if python had a hasing library I could use, and sure enough they did.

```python
import hashlib

salt = "enteryoursalthere"

i = 100000000

while 1:
    password = salt + str(i)
    password = password.encode('utf8')

    new_hash = hashlib.md5(password).hexdigest()
    if new_hash[0:2] == "0e" and new_hash[2:32].isdigit():
        print(password, str(i), new_hash)
        exit(0)
    i += 1
```

2. I haven't solved this challenge yet. The webpage doesn't have a login, simply three buttons that reveal different text when you click on them. The text sources are .txt files. The source code has a line that's hashed out that says the flag is in 12345678910111213141516.txt. Running the web server through I proxy, I tried to change the file name that would display as output upon clicking the button to the text file with the flag, but the error message tells me to behave as the file name is too long?
