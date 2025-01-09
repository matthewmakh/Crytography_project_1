# Crytography_project_1

In this project I downloaded 2 books in plaintext and encoded them using AES-128-CTR. To do this I used the same key and nonce. You are normally not supposed to do this for security reasons but for this project it was essential, especially for the extra credit part. This function takes the file in chunks so that it can easily handle the files efficiently. It takes in 4 parameters, a key (a 16 byte key), A Nonce (usually used only once. This is an initialization value), the incoming filepath, the filename of the output file, and the chunk size (the number of bytes read from the file at a time). 

Next we initialize the AES cipher and provide it with the key and the nonce to work with.
What is done here is that the CTR mode uses the key and nonce to create a keystream. That is then XOR’ed with the plaintext to produce the ciphertext. 

The file is read in binary read and outputted in binary write. This is important to make sure that the encryption works properly. 

We then read in a piece of data the size of the prespecified chunk. This is important to do so that we dont load the entire file into memory. This is even more important the larger a file gets. 

We then encrypt each chunk and write it to the new output file. 

We keep doing this until the end of the file. We know we have reached the end when the last chunk is empty.

Below is a code snippet showing how I used the encoding:

def encrypt_file(key, nonce, in_filename, out_filename, chunk_size=64 * 1024):
   cipher = AES.new(key, AES.MODE_CTR, nonce=nonce)


   with open(in_filename, 'rb') as f_in, open(out_filename, 'wb') as f_out:
       while True:
           chunk = f_in.read(chunk_size)
           if not chunk:
               break
           encrypted_chunk = cipher.encrypt(chunk)
           f_out.write(encrypted_chunk)

The next part of the project required us to XOR the ciphertexts, 

We again take in the 2 files and create an output file. We take in both files in binary read mode and the output file is binary write. 

We once again process the files in chunks for the same reasons as before. 

Next we XOR bytes from chunk 1 and chunks 2. This is done by zip(chunk1, chunk2) which pairs the bytes from both chunks to then XOR. a^b is the actual XOR. and Bytes(...) is used to convert the resultin XOR values back into bytes.

below is the code for how i did that: 

def xor_files(file1, file2, output_file, chunk_size=64 * 1024):


   with open(file1, 'rb') as f1, open(file2, 'rb') as f2, open(output_file, 'wb') as fout:
       while True:
           chunk1 = f1.read(chunk_size)
           chunk2 = f2.read(chunk_size)


           # Stop when we reach the end of either file
           if not chunk1 or not chunk2:
               break


           # XOR the bytes of the two chunks
           xor_chunk = bytes(a ^ b for a, b in zip(chunk1, chunk2))


           # Write the XOR result to the output file
           fout.write(xor_chunk)


The last part required us to check and make sure that the XOR of the plaintexts matched the XOR of the ciphertexts. 

Below is the code snippet: 

xor_files('book1.enc', 'book2.enc', "xord_enc_files.bin")
xor_files('/Users/matthewmakh/Desktop/Book 1 crypto project 1.txt', '/Users/matthewmakh/Desktop/Book 2 crypto project 1.txt', "xord_files.bin")


with open('xord_enc_files.bin', 'rb') as f_plain, open('xord_files.bin', 'rb') as f_cipher:
   plaintext_xor = f_plain.read()
   ciphertext_xor = f_cipher.read()


   if plaintext_xor == ciphertext_xor:
       print("XOR of plaintexts matches XOR of ciphertexts! Verification successful.")
   else:
       print("Verification failed: XOR results are different.")

Here is my chat gpt convo: https://chatgpt.com/share/677f4cfa-9888-800e-b654-df27e0f0a7d1
