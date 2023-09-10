# Cryptography

## Easy
1. Downloading the file and running cat showed a pretty obvious Caesar cipher.
2. Second challenge output was in decimal code, plugging it into Cyberchef yielded the flag.
3. Third challenge was a bit trickier. They gave us a message and a key, I can't remember exactly what I did but I think it was a PGP encryption and it redirected us to a website that I found completely useless. I think I ended up installing gnupg on kali and running that on the file: gpg -d.

## Medium
1. This challenge had us download a file, what looked like a txt file. Catting it gave a huge output with mostly capital 'A's. Very confusing. Anyway, I plugged it into CyberChef and attempted to decode it from base64, because that's what it looked like to me, and then first word in the result was ELF! CyberChef also declared it as an executable, so I was able to download the output - which automatically downloaded as a .dat file. Executing that in the terminal yielded the flag!

## Hard
1. Haven't solved this one. I got two files - one a txt file with a "key", the second a python script with what looks like Chinese letters? Some parts are in english - binasciii.hexlify, for example. The hints are not helpful yet - Deobfuscate, then "reverse the function or smash it with prior knowledge". I think the prior knowledge part refers to the key provided? Not sure.
2. Haven't solved this one either. Instructions are to decrypt the code to get the flag. The downloaded file gave me binary output, whigh yielded "xhRmogZz9Nc2VuRUlhZG5lbkMtc3gyODA=OT" on Cyberchef. The hints seem very unhelpful:

Hint 1
Examine the data closely, it gives clues. Then use your tools

Hint 2
Cyberchef will help, but it won't do all the work  

Sigh.
