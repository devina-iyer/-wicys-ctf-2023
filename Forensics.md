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
1. This really was extra hard! The file was a pcapng, so I used wireshark. Looking through the different packets I started following the streams. When I followed a UDP stream the first hing I saw was "get 1.jpg", so I extracted the stream. I remembered I had read a writeup of this CTF challenge, and another player had shared their python script, and this was the hint they provided:

```
client -> server: 50 bytes with the command and filename: e.gfile001.zip padded with 0s# server -> client: answer with 4 bytes informing the amount of chunks itwill take to transfer
client -> server: send the received count of chunks (but in 8 bytes)
server -> client: send a packet with 2064 bytes to client.  - This packet is formed by a header (16 bytes with the actual chucksent) and the remaining bytes are data.
client -> server: send to server the actual chunk number received (foundin the previous header received) but send as 8 bytes
server -> client: send the next chunk of data (header with actual chunknumber + data)

LOOP until the server sends to the cliente a chunk of data which headercontains the chunk number that the server sent on first packet.STARTS THE WHOLE FILE TRANSFER FROM #1 TO ANOTHER FILE.
```

```python

# Python script to parse custom udp file transfer# built for sans bootupctf -> FX01
# Author: ludarkstar@gmail.com
# Usage:
# 1. Export the while UDP stream as raw (not as ascii, the default visualization from wireshark).
# 2. Save the stream in the same directory as this script with name res01.txt
#------------------------------------------------------------------

# Mark for end of file reads.
EOF = False

# Counter for the number of files inside the stream
file_count = 0

# Opening the exported file for binary read "rb"
f = open("res01.txt","rb")

# Iterative while we do not reach End of File
whilenotEOF:
   # Open a new output file and give an increased name for it
   out_filename = "file" + str(file_count)
   out_file = open(out_filename,"wb")

   # Read 50 Bytes
   # Contains the get command from client to server (start ofconversation)
   client_target_file    = f.read(50)

   # If no more data to read, just exit the script.
   iflen(client_target_file) == 0:
         break

   # Read 4 Bytes
   # Contains the server response to cliente, with the amount ofchunks this transfer will take
   server_file_size      = f.read(4)

   # Read 8 Bytes
   # Contains the client response to server, acknowledging the amountof chunks
   client_echo_file_size = f.read(8)

   # Holds the amount of pure data bytes we read so far
   read_byte_count = 0

   # Will hold the current read data
   file_data = ""

   # Loop while we are extracting this file until the last chunk ofdata
   while True:
         # Read 16 bytes
         # Contains the first 16 bytes from the server response. Itis the actual chunk counter
         chunk_header = f.read(16)

         # Read 2048 Bytes
         # Contains the remaining packet, the data we want toassemble the file
         file_data = f.read(2048)

         # As soon we read from source, write to output file.
         out_file.write(file_data)

         # Let's just count the amount of data we have read so far
         read_byte_count += 2048

         # Read 8 Bytes# Contains the response from client to server,
acknowledging the actual chunk number received (took from header)
         client_chunk_echo = f.read(8)

         # Check if the actual chunk number is the same that theserver sent to client in the beginning of conversation. If it's the same chunk number assume wereached the last chunk, or End of This File.
         if client_echo_file_size == client_chunk_echo:                                      file_count += 1
                  break
   # After read while file from stream, let's close it so we can start a new file export.
   out_file.close()

# we finished... Close the source (stream) file
f.close()
```

Running this script dumped out a few files, one of which was a zip folder that had an image with the flag on it.
