I was presented with a cryptography challenge that required me to decrypt an encoded message hidden in a file named `ciphertext(4)`. The filename itself was a bit tricky because of the parentheses, which caused some initial confusion when trying to access the file. However, I managed to work around it by using escape characters (`\`) in the terminal to read the file content.

Inside the file was an encrypted string that I needed to decode. I realized that the solution likely involved a Python script that was provided as part of the challenge. The script was available for download, so I fetched it and started examining it. The script seemed to perform some sort of character mapping between two sets of characters (`lookup1` and `lookup2`), which is a common technique in cryptography.

After copying the script to a new file, I tried running it with Python 3. However, I encountered an error that mentioned a variable (`t`) wasn’t defined. This pointed to an issue in the script, where a part of the logic needed tweaking. I dove into the code and made the necessary corrections, ensuring that the variable `t` was properly initialized before being used.

With the fix in place, I attempted to run the script again, but I realized it was written for Python 2. At this point, my system didn’t have Python 2 installed, so I quickly installed it. Once that was done, I re-ran the script, and this time, it worked. The output wasn’t immediately readable, but it was a sequence of characters that seemed to be part of a larger message.

After some inspection, I realized that the string produced by the script needed to be interpreted in the right context, and it eventually revealed the flag. The decoding process was a mix of trial and error, figuring out how to make the script work, and understanding the logic behind the character mappings. It was a rewarding experience because it involved both technical problem-solving and a bit of persistence.

In the end, the challenge was all about carefully analyzing the provided tools, adjusting them where necessary, and thinking through the encryption method to unlock the hidden flag.

Source Code:

![Screenshot (1286)](https://github.com/user-attachments/assets/858cc08d-cd1e-4c17-8ec0-5db68d90937a)


Modified Code:

![Screenshot (1287)](https://github.com/user-attachments/assets/73da1fe6-90c8-46fe-a76e-a9df7a8f26de)

Ciphertext:

![Screenshot (1283)](https://github.com/user-attachments/assets/706fe8e8-307f-464c-b2bd-b4b85d0aba2f)


Ubuntu terminal:

![Screenshot (1284)](https://github.com/user-attachments/assets/41531ec4-3f0f-446a-b6ba-cfb2e73d57e0)


![Screenshot (1285)](https://github.com/user-attachments/assets/c9b9afc3-2623-4162-9811-b873ffdd27fc)

(Some part of it was just installing python2, as it wasn't installed on this yet. The main code that is important is there.)

**picoCTF{adlibs}**
