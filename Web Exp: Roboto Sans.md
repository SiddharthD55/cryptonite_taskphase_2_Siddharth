Well in this challenge I'm supposed to find the flag in the website. Simple.

And I got the website URL http://saturn.picoctf.net:53982/, and clicked on that, and accessed a 'Yoga' website.

And then I viewed the source code.

![Screenshot (45)](https://github.com/user-attachments/assets/df039ab2-2ca6-4fa0-b76d-2ad1a488500c)


![Screenshot (46)](https://github.com/user-attachments/assets/1643e7f8-99f2-4001-9312-bd9691e10536)

Well the second picture has a green coulored funny message, and that message kinda gave me a hint; that the flag is stored in some file or link in the website page.

I tried some options, but well, I failed.

![Screenshot (44)](https://github.com/user-attachments/assets/c867d7f6-27d4-4d40-82f3-398bf8b6c60e)


![Screenshot (47)](https://github.com/user-attachments/assets/96d533de-d274-4dbf-ba6f-c537ed58997e)


When I first saw the challenge name, I thought it might refer to the text font Roboto or the /robots.txt file. I decided to check out the /robots.txt file first.

**Step 1: Exploring /robots.txt**

Navigating to /robots.txt, I found the following content:

```
User-agent *
Disallow: /cgi-bin/
Think you have seen your flag or want to keep looking.

ZmxhZzEudHh0;anMvbXlmaW
anMvbXlmaWxlLnR4dA==
svssshjweuiwl;oiho.bsvdaslejg
Disallow: /wp-admin/
```

![Screenshot (48)](https://github.com/user-attachments/assets/1b868efb-caee-412f-aaa4-3eafa1ac0729)


The `Disallow` directives were typical, directing web crawlers away from specific directories. However, the text above the `Disallow` lines immediately grabbed my attention. The double equal sign (`==`) at the end of some lines suggested that this might be base64-encoded text—a common method for encoding data.

**Step 2: Decoding Base64**

I decided to decode the suspicious text using an online base64 decoder. Initially, the text appeared malformed. Noticing that the base64 text was spread over three lines, I considered that it might be three separate base64-encoded strings.

**Step 3: Decoding Each Line Separately**

I decoded each line individually:

- **First line:** `ZmxhZzEudHh0`
  - This line decoded to `flag1.txt`, which looked like a file name but didn’t lead to any immediate results.

![Screenshot (49)](https://github.com/user-attachments/assets/e702d709-b280-4ddd-8008-d23cf92c7c74)


![Screenshot (51)](https://github.com/user-attachments/assets/f3bd4f61-c977-48bd-ab4d-6c09f8c8a801)


- **Second line:** `anMvbXlmaWxlLnR4dA==`
  - This line decoded to a valid path: `js/myfile.txt`.

![Screenshot (50)](https://github.com/user-attachments/assets/97fd74ad-7824-49d2-85db-8cb7585c6939)


- **Third line:** `svssshjweuiwl;oiho.bsvdaslejg`
  - I didn't need to decode this line. Why...

**Step 4: Navigating to the Sub-URL**

I followed  the decoded path `js/myfile.txt`, I navigated to the corresponding URL on the target website. As expected, this led me directly to the flag. This confirmed that the second line was indeed the key to finding the flag.

![Screenshot (52)](https://github.com/user-attachments/assets/6a86c52e-7e81-4cc2-8d5b-3396e4ee619d)


**Step 5: Reflection and Analysis**

After successfully finding the flag, I took some time to reflect on the challenge and what I had learned:

- **Attention to Detail:** The challenge emphasized the importance of examining every detail carefully. The double equal signs (`==`) were a crucial hint pointing to base64 encoding.
  
- **Understanding Encoding Methods:** Knowing about encoding methods like base64 is essential in cybersecurity and web challenges. Recognizing these patterns can speed up the decoding process significantly.
  
- **Breaking Down Problems:** When dealing with complex or malformed encoded text, breaking it into smaller, manageable parts is often effective. Decoding each line separately allowed me to identify the correct path.
  
- **Systematic Approach:** A systematic approach helps narrow down potential solutions and avoid unnecessary complexity. By methodically decoding each part, I was able to pinpoint the correct path.

**Final Thoughts:**

This challenge was a fascinating exercise in problem-solving and critical thinking. It reinforced the importance of observation, methodical thinking, and familiarity with common web technologies and encoding techniques. By following these principles, I successfully decoded the text and found the flag.

**picoCTF{Who_D03sN7_L1k5_90B0T5_718c9043}**
