Initially, when I tried to open the file, I got this.

![Screenshot (1252)](https://github.com/user-attachments/assets/a2b5187f-296b-48b1-aa0b-24c6a809f214)


I began by analyzing the bytes of my file, which was named `tunn3l_v1s10n`. To inspect the contents and make modifications, I uploaded it to HexEd.it.

![Screenshot (1251)](https://github.com/user-attachments/assets/bdf73c14-1c1e-4168-9a16-112e7df1264d)


In the file, I noticed the magic bytes `42 4D`, which are typical of BMP files.

The size of the image was listed as `0x2c268e` bytes, and the pixel array started at offset `0xd0ba`.

This was a bit unusual, since the DIB header should only be 40 bytes long, but here it indicated a size of `0xd0ba`, which seemed incorrect.

To fix this, I replaced the `BA D0` with `28 00` and saved the file with a `.bmp` extension.

I got an image that gave us a **not correct flag*.

![Screenshot (1253)](https://github.com/user-attachments/assets/005f3107-f794-4ed4-b2fb-7977c2156202)


According to the BMP specification, the pixel array should be located 54 bytes after the header, so I adjusted the offset to `54 bytes`.

While the image was now visible, it still wasn’t perfect. I had a feeling the image was supposed to be bigger, and I recalled that the part of the image labeled "notaflag" should be in it, but I couldn’t see much of it. The image had been cut off, and something was off about the dimensions.

When I tried to change the width, I got a distorted image.

![Screenshot (1254)](https://github.com/user-attachments/assets/9e392775-a453-48e3-af10-aa6e2c0d3bee)


But when I tried to change the height, I just got some of the image that was hidden before.

![Screenshot (1255)](https://github.com/user-attachments/assets/4541b38a-3d59-465e-9174-8c80b9b546da)


Given that the total size of the file was `0x2c268e` bytes, and I knew the header size was 54 bytes, I could calculate the number of pixels. The width was already given as 1134 pixels, and each pixel took up 3 bytes (one for each color channel). So, I could compute the height of the image by subtracting the header size from the total file size, then dividing by the number of bytes used for each pixel.

Here’s the math I did to calculate the height:

```
(0x2c268e - 54) / (3 * 1134) = 850
```

The height of the image was 850 pixels. I then updated the header by replacing the previous value with `850`, which is `0x352` in hexadecimal.

![Screenshot (1258)](https://github.com/user-attachments/assets/46a12446-cef8-46b3-9b3b-3ac4ba8dbb7f)


Once I made the necessary adjustments and saved the image again, I could finally see the complete flag.

![Screenshot (1257)](https://github.com/user-attachments/assets/1eae3c39-185e-47bb-bb8d-8aff24376060)


**Flag: picoCTF{qu1t3_a_v13w_2020}**
