TFTP (Trivial File Transfer Protocol) is a simple, lightweight protocol used for transferring files over a network.

Unlike FTP, it doesn't require authentication or advanced features like directory listing, making it faster but less secure.

TFTP is commonly used for booting systems or transferring configuration files.

After downloading the `tftp.pcapng` file from the challenge, I opened it using Wireshark, which is a network protocol analyzer that allows you to capture and analyze packets in detail.

When I opened the file, Wireshark automatically detected the packet capture and displayed the network traffic.

![Screenshot (1183)](https://github.com/user-attachments/assets/95fed90d-ddf2-400f-a018-c049fd40d431)


Here I could kind of make out that there were three files: `instructions.txt`, `plan`, and `program.deb`.

I then used the "Export Objects" feature in Wireshark to extract files from the capture.

Basically, I navigated to **File** → **Export Objects** → **TFTP**, where I found these three files listed. I also found three corresponding pictures — `picture1.bmp`, `picture2.bmp`, and `picture3.bmp`.

![Screenshot (1184)](https://github.com/user-attachments/assets/51d6e099-8925-40fc-9dcf-4008dc0fc685)


![Screenshot (1185)](https://github.com/user-attachments/assets/dff16cd8-d657-4e5f-9617-e662e484794d)


With these files identified, I speculated that the flag might be hidden in the `program.deb` file.

I then switched to the Ubuntu terminal, where I navigated to the folder where I had stored all the extracted files.

There was a new problem I faced where I was unable to access any files.

After trying to figure it out and with the help of my mentor, I discovered that the files were actually stored in the path `\\wsl.localhost\Ubuntu\home`.

I then created a `Cryptonite` folder in this directory and stored all the files and pictures in this folder.

![Screenshot (1187)](https://github.com/user-attachments/assets/a1369ffe-d0a5-4698-b004-b2eb4a384bb3)


![Screenshot (1188)](https://github.com/user-attachments/assets/a217e454-05a7-4b5f-ad51-2a1ad4596335)


![Screenshot (1189)](https://github.com/user-attachments/assets/7ad564f9-24a6-4e00-b7d4-3f817f7b277a)


Once everything was in place, I switched to the Ubuntu terminal to continue with the process.

```
cd /home/
ls
cd Cryptonite
```

Once inside the folder, I extracted the contents of the `program.deb` file by using the `ar` command to unpack the `.deb` package:

```
ar x program.deb
```

This command extracted three files: `data.tar.xz`, `control.tar.gz`, and `debian-binary`. To extract the contents of the `data.tar.xz` file, I used the `tar` command:

```
tar -xf data.tar.xz
```

This gave me the folder structure, and when I listed the files with `ls`, I saw several files including `instructions.txt`, `plan`, and pictures. To further explore, I also extracted the `control.tar.gz` file using:

```
tar -xvzf control.tar.gz
tar -xvf data.tar.xz
```

and saw this.

```
./
./md5sums
./control
./
./usr/
./usr/share/
./usr/share/doc/
./usr/share/doc/steghide/
./usr/share/doc/steghide/ABOUT-NLS.gz
./usr/share/doc/steghide/LEAME.gz
./usr/share/doc/steghide/README.gz
./usr/share/doc/steghide/changelog.Debian.gz
./usr/share/doc/steghide/changelog.Debian.amd64.gz
./usr/share/doc/steghide/changelog.gz
./usr/share/doc/steghide/copyright
./usr/share/doc/steghide/TODO
./usr/share/doc/steghide/HISTORY
./usr/share/doc/steghide/CREDITS
./usr/share/doc/steghide/BUGS
./usr/share/man/
./usr/share/man/man1/
./usr/share/man/man1/steghide.1.gz
./usr/share/locale/
./usr/share/locale/ro/
./usr/share/locale/ro/LC_MESSAGES/
./usr/share/locale/ro/LC_MESSAGES/steghide.mo
./usr/share/locale/fr/
./usr/share/locale/fr/LC_MESSAGES/
./usr/share/locale/fr/LC_MESSAGES/steghide.mo
./usr/share/locale/de/
./usr/share/locale/de/LC_MESSAGES/
./usr/share/locale/de/LC_MESSAGES/steghide.mo
./usr/share/locale/es/
./usr/share/locale/es/LC_MESSAGES/
./usr/share/locale/es/LC_MESSAGES/steghide.mo
./usr/bin/
./usr/bin/steghide
```

I could figure out from here that there were files related to **steghide**.

Which meant that there could be some passcode or passphrase that I would need.

At this point, I opened the `plan` file with:

```
cat plan
```

and this is what I got.

```
VHFRQGURCEBTENZNAQUVQVGJVGU-QHRQVYVTRAPR.PURPXBHGGURCUBGBF
```

I thought that this would be encoded in Caesar Cipher.

I tried that, and found the decrypted message `IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS`.

Upon adding spaces in required places, I got `I USED THE PROGRAM AND HID I TWITH - DUEDILIGENCE. CHECK OUT THE PHOTOS`.

Meaning there was a passcode required, which was `DUEDILIGENCE`.

I then turned to the tool **steghide** to attempt to extract hidden data from the images.

First, I needed to install `steghide`, so I updated the package list and installed it using the following commands:

```
sudo apt update
sudo apt install steghide
```

Once `steghide` was installed, I tried to extract hidden data from `picture1.bmp`, but I wasn’t successful.

I then repeated the extraction process for `picture2.bmp`, but still, no data was found. Finally, I tried `picture3.bmp` with:

```
steghide extract -sf picture3.bmp
```

This time, I was successful. The program prompted me for the passphrase (basically passcode), which I entered. The hidden data was extracted into a file named `flag.txt`. After opening this file with:

```
cat flag.txt
```

I found the flag.

![Screenshot (1190)](https://github.com/user-attachments/assets/7a8b1d72-2bc6-49a8-a5f2-a9524cbcb9be)


![Screenshot (1191)](https://github.com/user-attachments/assets/445ee3d7-2d36-46af-a21b-4b2242404a74)


![Screenshot (1192)](https://github.com/user-attachments/assets/47e8f3c3-5c8b-4924-8906-f674e81693b1)


![Screenshot (1193)](https://github.com/user-attachments/assets/ee6d7b38-3ef6-4045-804b-66c319d05fd4)


![Screenshot (1194)](https://github.com/user-attachments/assets/2b288065-adc1-4872-ac60-a466a74d8f72)


**picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}**
