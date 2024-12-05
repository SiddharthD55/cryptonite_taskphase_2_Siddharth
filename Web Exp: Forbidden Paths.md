After launching the instance, we get a website that has the URL, `http://saturn.picoctf.net:52891/`.

And the website is basically this.

![Screenshot (1259)](https://github.com/user-attachments/assets/6403c5b8-5320-43d3-8df8-5c23044c9e81)


Here we need to type in the path to the flag file.

But the twist here, is that, the search only allows us to type in relative paths, and not absolute paths.

Now we know that the website files live in `/usr/share/nginx/html/` and the flag is at `/flag.txt`.

We have the knowledge that, with respect to file paths, a preceeding `./` means current directory, and a preceeding `../` means enclosing directory.

Now the absolute path needes is `/usr/share/nginx/html/flag.txt`.

We can also use the relative path `../../../../flag.txt` to access the file.

I type in this path in the search, and I find the flag.

![Screenshot (1260)](https://github.com/user-attachments/assets/39b902c0-0b7f-4191-9f4c-c6c2bd1e5aaa)


**picoCTF{7h3_p47h_70_5ucc355_e5fe3d4d}**
