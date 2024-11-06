I navigated to the `Cryptonite` directory on my system and identified the file `debugger0_a` as an ELF 64-bit executable.

I then started debugging it using GDB (GNU Debugger), a debugger for analyzing and manipulating programs to find bugs or vulnerabilities.

GDB is a powerful tool that allows me to inspect the assembly code, set breakpoints, and manipulate program execution.

After starting GDB with the `debugger0_a` file, I used the `info functions` command to list the available functions in the binary.

This revealed several non-debugging symbols, including `main` at address `0x0000000000001129`.

I proceeded to disassemble the `main` function using the `disassemble main` command, which displayed its corresponding assembler code.

The code showed the function setting up the stack, moving values to memory, and returning a specific value (in this case, `0x86342`).

This value was the key to obtaining the flag. I then converted the hexadecimal value `0x86342` into its decimal equivalent using Python with the command `print(int(0x86342))`, which resulted in `549698`.

By inputting this value into the flag prompt, I successfully obtained the flag: `picoCTF{549698}`.

In summary, I used GDB to disassemble and analyze the program's flow, specifically the `main` function, where I identified the hexadecimal value `0x86342` and converted it into its decimal form, yielding the correct flag.

![Screenshot (1196)](https://github.com/user-attachments/assets/54e0d3bb-7f82-4ad0-ad81-beb01a18f0cb)


![Screenshot (1197)](https://github.com/user-attachments/assets/8deed10b-faab-4ba0-8f7a-e88235a8db30)


![Screenshot (1198)](https://github.com/user-attachments/assets/086163ac-4621-4a51-b311-d4b5babe7e1f)

