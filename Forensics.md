# Forensics

## Easy
1. For the first challenge you download a zip folder containing an image. Running "file" on the image yielded the flag in a comment.
2. The second one was quite challenging, I thought. The zip file was encrypted. So I had to download John the Ripper and Hashcat to help me get the job done. Not to mention 7zip because it was a PK compression.
   Trying to recreate it is quite hard, I'm almost certain I used both John an Hashcat to do this the first time, and I've forgotten the exact syntax for the hashcat now. At least it worked once and I got the flag!
3. This one was more challenging that it should have been, really. The clue was right there - it is the name of the VB function that tests for internet connectivity. I think it might have been encrypted? Or just a lot of unzipping to do or hunting around for directories and I finally found one with VB in it, and running strings on it I think is what led me to the exact phrase- something along the lines of IsConnectionAvailable or something.
4. This one was a breeze. The zip folder contained a pdf with blacked out lines. Doing a search for "flag" showed that the flag was blacked out, but I copy pasted it into notepad and there was the flag!
5. This fifth one was fairly straightforward. The zip file contained a myterious file with no extension. Running xxd on it yielded the flag!

## Hard
1. Only one hard challenge, but it is VERY hard. I haven't cracked it yet. It is a gif file (the crash override one to be precise) and the hints are very unhelpful:
```
Hint 1
Close inspection of the gif is key, sometimes we make it harder so you have to do more

Hint 2
XOR hides things
```
Uploading it to Aperisolve indicated that there is a PGP Secret Key, but I haven't figured out how to extract it yet.

## Extra Hard
1. This really was extra hard! The file was a pcapng, so I used wireshark. Looking through the different packets I started following the streams. When I followed a UDP stream the first hing I saw was a .jpg, so I extracted the stream. I was lucky enough to pull together a python code with tons of googling to extract the files from it. 

```python
EOF = False
file_count = 0
f = open("res01.txt","rb")
while not EOF:
    out_filename = "file" + str(file_count)
    out_file = open(out_filename, "wb")
    client_target_file = f.read(50)
    if len(client_target_file) == 0:
        break
    server_file_size = f.read(4)
    client_echo_file_size = f.read(8)
    read_byte_count = 0
    file_data = ""
    while True:
        chunk_header = f.read(16)
        file_data = f.read(2048)
        out_file.write(file_data)
        read_byte_count += 2048
        client_chunk_echo = f.read(8)
        if client_echo_file_size == client_chunk_echo:
            file_count += 1
            break
    out_file.close()
f.close    
```

Running this script dumped out a few files, one of which was a zip folder that had an image with the flag on it.
