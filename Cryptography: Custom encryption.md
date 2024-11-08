To solve the challenge, I made several key modifications to the original code to reverse the encryption process and ultimately retrieve the flag.

First, I added a `dynamic_xor_decrypt` function, which was a modified version of the `dynamic_xor_encrypt` function.

The key change here was that instead of reversing the XOR operation only once, I modified the function to apply the decryption **three times**.

This was necessary to undo the layered XOR encryption that had been applied during the encryption process.

Next, I introduced the `decrypt` function, which specifically reverses the encryption done by the `encrypt` function.

While the `encrypt` function multiplies the ASCII value of each character by the key multiplied by `311`, the `decrypt` function divides the encrypted values by the same factor (`key * 311`).

This ensures the original characters are restored from their encrypted form.

I then created the `test2` function to simulate the specific decryption process, using pre-calculated values for `a`, `b`, `p`, and `g`.

This function hardcoded the values for `a` and `b`, and used them to compute the shared key through modular exponentiation with the `generator` function.

It then used the `decrypt` function to reverse the encryption and applied the `dynamic_xor_decrypt` function to recover the flag from the decrypted data.

The hardcoded values were crucial because they were derived from the test function originally used for encryption, and they were necessary to perform the decryption correctly.

The key generation was aligned with the modular exponentiation logic from the original encryption.

The values for `p` and `g` (which are prime numbers used for modular arithmetic) were checked using the `is_prime` function to ensure valid key generation.

To simplify the process, I modified the main function to directly call the `test2()` function.

This removed the need for command-line input, making the decryption process automatic when running the program.

Additionally, I hardcoded the cipher into the `test2` function.

This cipher, which was provided as part of the challenge, was essential for the decryption process.

I needed to decrypt the cipher using the `decrypt` function and then apply the `dynamic_xor_decrypt` function to reveal the flag.

Finally, the program no longer required user input via command-line arguments, which made the decryption process run automatically.

With these changes, I was able to reverse the encryption process: first generating the shared key through modular exponentiation, then decrypting the cipher using that key, and finally applying the XOR decryption three times to retrieve the flag.

These modifications ensured the flag could be correctly decrypted from the provided encrypted content.

**picoCTF{custom_d2cr0pt6d_dc499538}** 

![Screenshot (1216)](https://github.com/user-attachments/assets/0b65ef80-f61d-4267-91ac-48da36b77644)


![Screenshot (1217)](https://github.com/user-attachments/assets/a56f4c2f-efcb-4894-933f-e1791acc289b)


![Screenshot (1218)](https://github.com/user-attachments/assets/8e861eca-8ced-4559-a022-5e20c0e12400)


![Screenshot (1219)](https://github.com/user-attachments/assets/b02d627f-89f2-4518-9035-524d44b3df6e)


![Screenshot (1220)](https://github.com/user-attachments/assets/a3dfb19f-f7e8-40a9-a659-8d0b7be22d11)


![Screenshot (1221)](https://github.com/user-attachments/assets/fa50be13-5565-4883-8d59-fd7f9fed7c7e)


![Screenshot (1222)](https://github.com/user-attachments/assets/9aeee04f-bb89-4fe1-ac3b-51180d3a823a)


![Screenshot (1223)](https://github.com/user-attachments/assets/cb12b0ff-406d-49c3-bf59-7d6d728dc35d)


![Screenshot (1224)](https://github.com/user-attachments/assets/264603fd-0fcd-4a08-b36a-fce77ddfbbf4)


![Screenshot (1225)](https://github.com/user-attachments/assets/b71dcf51-3b48-4186-973a-95c959177d2a)


![Screenshot (1226)](https://github.com/user-attachments/assets/5273cb26-0986-4add-860d-d5a855e43346)


![Screenshot (1227)](https://github.com/user-attachments/assets/d70d3453-63e6-4534-936f-c41e20e0c904)


![Screenshot (1228)](https://github.com/user-attachments/assets/e259489c-aa11-4a20-ac0d-d60ea706a443)
