In this challenge, I had the task of exploiting JSON Web Tokens (JWT) to gain administrative access. Here's a detailed account of my journey, what I learned, and the entire process I followed.

![Screenshot (53)](https://github.com/user-attachments/assets/2f390029-6a3b-4026-8b67-884771b1b8f5)


### Getting Started

The challenge began with a visit to a specific web page where I had to authenticate myself. The hint suggested that I look out for a cookie. And the word 'John' on the website suggested that somewhere I may need to use John the Ripper.

And...admin could be a hint too. 

![Screenshot (61)](https://github.com/user-attachments/assets/e56499ea-e917-4368-ae60-ff1687a6085b)


(Ok well you see a 'Cookie Editor' thing on the side. You'll find out why later in the writeup. Read on!)

Well, I entered a username John.

![Screenshot (54)](https://github.com/user-attachments/assets/edeb86ac-1e7c-43c3-a9b8-da21e5ceac80)


Well there is no flag diaplayed.

![Screenshot (55)](https://github.com/user-attachments/assets/fad32634-dc5f-450f-9d0e-2d3016354eee)


Using Chrome developer tools (F12) and a browser extension called Cookie Editor, I found a cookie value that seemed to be a JWT.

![Screenshot (56)](https://github.com/user-attachments/assets/0b369ad6-6ff9-450f-ba35-05937e86e1f2)


### Understanding JWT

JSON Web Token (JWT) is an open standard that defines a way to securely transmit information between parties as a JSON object. It's digitally signed, so it can be verified and trusted.

![Screenshot (57)](https://github.com/user-attachments/assets/9a07a2d9-dc6b-4823-b984-b6593207e917)


A JWT is divided into three parts, each separated by a dot:

1. **Header**
   This part tells us the type of token and the algorithm used for signing it. It's a base64URL encoded string. When I decoded it, I found:
   ```
   {"typ":"JWT","alg":"HS256"}
   ```

2. **Payload**
   The payload contains the claims, which include user information and other data. For this challenge, the payload showed that I was logged in as "akshay". Decoding it revealed:
   ```
   {"user":"akshay"}
   ```

3. **Signature**
   This part validates the token and ensures it hasn't been tampered with. Changing the token or the user field to "admin" without the correct signature would result in errors.

### Exploiting JWT

The token used the HS256 algorithm, a symmetric key algorithm. If a weak secret key was used to sign the token, it could be brute-forced to discover the key. I decided to use JohnTheRipper, a well-known password-cracking tool, for this task.

### Brute-Forcing the Secret Key

Here's what I did:

1. **Saved the JWT Token to a File**
   I saved the JWT token to a file named `jwt.txt`:
   ```
   eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyIjoiSm9obiJ9.K1Omo0Gk5saKwJTkkgT7PUZohD7USknEE0lmT2AYAiM
   ```

2. **Ran JohnTheRipper**
   I used JohnTheRipper to crack the secret key. Here are the commands I ran:
   ```
   root@MyLenovoDude:/home# john jwt.txt
   Using default input encoding: UTF-8
   Loaded 1 password hash (HMAC-SHA256 [password is key, SHA256 256/256 AVX2 8x])
   Will run 16 OpenMP threads
   Proceeding with wordlist:/snap/john-the-ripper/current/run/password.lst
   ilovepico (?)
   1g 0:00:02:04 DONE
   ```

   The output showed that the secret key was `ilovepico`.
   
![Screenshot (58)](https://github.com/user-attachments/assets/27d0ed90-2766-43ff-8cda-83b02ec8adbf)


### Generating a New Token

![Screenshot (59)](https://github.com/user-attachments/assets/966cd233-f53a-4569-b172-938854dff8a6)


![Screenshot (60)](https://github.com/user-attachments/assets/ddaa27bd-6eb4-425e-abd4-02f8701f05d9)


With the secret key, I signed a new token with administrative privileges. I used a site that allows generating and verifying JWT tokens, entered the secret key, and created a token with the `user` field set to `admin`.

![Screenshot (64)](https://github.com/user-attachments/assets/a9cb46a6-1f95-43f9-8328-b0c113dc5248)


### Gaining Admin Access

To gain administrative access, I used Cookie Editor to replace the existing JWT with the new one. Here's how:

1. **Used Cookie Editor**
   I opened Cookie Editor, found the JWT cookie, and replaced its value with the new token.

![Screenshot (62)](https://github.com/user-attachments/assets/bc65e2e9-5ba4-4ab8-a959-29e884a964c6)


2. **Saved Changes and Refreshed the Page**
   After saving the changes, I clicked 'Logout', and the website recognized the token, granting me admin access. And I found the flag.

![Screenshot (63)](https://github.com/user-attachments/assets/1cee7819-dada-47b7-8de9-6b1da8e2e24c)


### Learnings

This challenge highlighted the importance of using strong, complex secret keys for signing tokens. Weak keys can be easily brute-forced, compromising security. Here are some key takeaways:

- **Use Strong Secret Keys:** Always use strong, complex keys for signing JWT tokens to prevent brute-force attacks.
- **Regularly Rotate Keys:** Regularly update and rotate secret keys to enhance security.
- **Monitor for Unauthorized Access:** Implement monitoring and alerting mechanisms to detect unauthorized access attempts.
- **Understand JWT Structure:** A solid understanding of JWT structure and algorithms is crucial for both exploiting and securing JWT tokens.

### Conclusion

This challenge was a fascinating experience that deepened my understanding of JWT security and cryptographic principles. It underscored the importance of strong key management and provided valuable insights into both offensive and defensive security practices.

**picoCTF{jawt_was_just_what_you_thought_f859ab2f}**
