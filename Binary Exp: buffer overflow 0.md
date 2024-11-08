When I started this buffer overflow challenge, I knew the goal was to overflow a specific buffer to reveal a hidden flag.

The challenge provided a C program with source code, so I began by examining it to identify the vulnerability.

I noticed that the flag was stored in a global array called `flag`, which was read from a file named `flag.txt`.

There was also a function called `sigsegv_handler`, set up to print the flag if a segmentation fault occurred.

This function seemed like a potential way to access the flag.

In the `vuln` function, I found a small buffer `buf2` of only 16 bytes, used with `strcpy` to copy user input without any length checks.

This made `buf2` vulnerable to overflow if I entered a string longer than 16 bytes.

Based on this, I planned to provide an overly long input to cause an overflow and trigger the `sigsegv_handler`.

To start the challenge, I navigated to the home directory and used `nc` (Netcat), as mentioned in the challenge description (nc saturn.picoctf.net 54111), to connect to the challenge server with the provided address and port.

I typed in these commands.

   ```
   cd /home/
   ```

   ```
   nc saturn.picoctf.net 54111
   ```

   When prompted with `Input:`, I entered a string much longer than 16 characters to overflow `buf2`. Here’s the exact input I used:
   ```
   EEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEODJISKLNFIOERDHNUEWDJKSNFUVIEFKJADFUILEWJKADSFNUIADJKSBUIAERJKDSBFNUCWIOADJSBKVIUEAKDSJBVIUAKDSJVB
   ```

After entering this input, the program crashed, triggering the `sigsegv_handler` function, which printed the flag on the screen:

This was a satisfying example of how a buffer overflow exploit works.

By exploring the code and experimenting with commands and inputs, I managed to overflow `buf2` and get the flag printed, successfully solving the challenge.

**picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}**

![Screenshot (1203)](https://github.com/user-attachments/assets/7c25bf35-1de6-450c-ada4-bdd41d4bf78b)


![Screenshot (1204)](https://github.com/user-attachments/assets/71d80c71-34cd-4a4e-a561-624ea022954d)


![Screenshot (1205)](https://github.com/user-attachments/assets/c976b9fd-409f-4bf1-99a1-d258195c942d)


![Screenshot (1206)](https://github.com/user-attachments/assets/bf662aa2-96f5-43fc-80d4-920aa6fdf900)
