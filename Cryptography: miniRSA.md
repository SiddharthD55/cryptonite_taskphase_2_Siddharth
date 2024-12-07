First of all, the full form of RSA is Rivest-Shamir-Adleman.

The full name RSA comes from the initials of the three inventors who developed the algorithm: Ron Rivest, Adi Shamir, and Leonard Adleman. In 1977, they introduced the RSA algorithm, which became the foundation of modern public key cryptography.

In the miniRSA challenge (mini because we are using a much smaller exponent), I was given an encrypted message, and my task was to decrypt it to retrieve the flag, which follows the format `picoCTF{...}`. The provided information included the ciphertext `c`, the modulus `N`, and the public exponent `e`, which was set to `3`.

![Screenshot (1272)](https://github.com/user-attachments/assets/2889c25e-ec21-4934-b01e-b29eee91528f)


Given these, I realized that this was an RSA encryption problem, where the ciphertext is derived from the plaintext using the formula:

\[
c = m^e \mod N
\]

Where:
- `c` is the ciphertext,
- `m` is the plaintext message,
- `e` is the public exponent,
- `N` is the modulus.

To decrypt the message, the goal was to find `m` (the plaintext) by reversing this encryption process. Specifically, I needed to compute the modular root of `c` with respect to `e`, which would give me the original plaintext message `m`.

The key thing here was that the public exponent `e` was small (3), which made the problem easier to tackle. With small exponents, RSA decryption can be simplified by taking the `e`-th root of the ciphertext modulo `N`. This means that instead of performing the full RSA decryption process, I could directly compute the cube root of `c` to find the original message.

To do this efficiently, I used Python and the `gmpy2` library, which is designed for fast arithmetic with large numbers. Here's the code I used:

![Screenshot (1270)](https://github.com/user-attachments/assets/dace7828-0a2f-4c4c-9596-02d642c2283a)


In this code, I first converted the ciphertext, modulus, and exponent into GMP integers, which can handle the large values. The key part is the function `gmpy2.iroot(gs, ge)`, which calculates the integer `e`-th root of `gs` (the ciphertext). This gives me the potential plaintext `m`. If the root was exact (i.e., the computed value raised to the power of `e` modulo `N` equals `c`), I then decoded the result by converting it from hexadecimal back to a byte string.

After running the code with the provided values for `c`, `N`, and `e`, I received the output that gave the flag.

![Screenshot (1271)](https://github.com/user-attachments/assets/b1b03d8a-3949-421a-bea4-162f13b78aba)


The key to solving this was realizing that the small value of `e` made it possible to compute the cube root of the ciphertext, which is a shortcut for RSA decryption in this case. By using Python and `gmpy2`, I was able to efficiently decrypt the ciphertext and reveal the flag.

**picoCTF{n33d_a_lArg3r_e_ccaa7776}**
