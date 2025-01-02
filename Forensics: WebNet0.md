In this, I'll share my experience of decrypting TLS packets using Wireshark and an RSA key. I had to uncover a hidden flag in a capture.pcap file. The process was kind of educational, and I learned a lot along the way. Here's a detailed account of what I did.

![Screenshot (22)](https://github.com/user-attachments/assets/08ef6ba1-bcf3-4350-993d-c772c625f5d3)


## Tools Used

- **Wireshark**: A powerful network protocol analyzer that I used to inspect network traffic.

## Process

### 1. Opening the Capture File

To start, I opened the `capture.pcap` file provided for the challenge in Wireshark. Once opened, Wireshark displayed numerous packets, and I noticed that many of them were labeled as "Encrypted handshake message" within the TLS protocol. This indicated that these packets were encrypted and required a key to decrypt them.

![Screenshot (25)](https://github.com/user-attachments/assets/bd27f102-2389-4ec7-a806-afcb84d63828)


### 2. Understanding the Task

The task was clear: I needed to decrypt the TLS packets using the provided RSA key. This key would allow Wireshark to decipher the encrypted messages and reveal their contents.

### 3. Configuring Wireshark to Use the RSA Key

To decrypt the TLS packets, I had to configure Wireshark to use the RSA key. Here's how I did it:

![Screenshot (26)](https://github.com/user-attachments/assets/fbb361e5-d2c9-48e5-8a4b-4694542b24e7)


![Screenshot (30)](https://github.com/user-attachments/assets/2aa0b4ac-b674-41db-85d9-aae0ddfaef28)


![Screenshot (23)](https://github.com/user-attachments/assets/04c3811e-ecf1-46d9-9b90-f1c3674a84c1)


#### 3.1 Navigating to Preferences

1. **Open Preferences**: I went to `Edit` > `Preferences` to open Wireshark's preferences window.
   
#### 3.2 Adding the RSA Key

2. **RSA Keys Configuration**: Within the preferences window, I navigated to `Protocols` > `RSA Keys`.
   - Here, I clicked `New` and added the RSA key file provided for the challenge.
   - I entered the necessary details, including the IP address, port number, and the path to the RSA key file.

#### 3.3 Configuring the TLS Protocol

3. **TLS Protocol Settings**: Still in the preferences window, I went to `Protocols` > `TLS`.
   - I added the RSA key file here as well to enable decryption of TLS packets.

### 4. Reloading the Capture File

After configuring the RSA key, I reloaded the capture file in Wireshark. This step was crucial for Wireshark to reprocess the packets and apply the decryption settings using the newly added RSA key.

![Screenshot (24)](https://github.com/user-attachments/assets/65255682-d9d9-481e-b758-0c04c7f1cdf7)


### 5. Analyzing Decrypted Packets

With the TLS packets now decrypted, I could see additional details that were previously hidden. One of the decrypted packets was particularly interesting:

- **HTTP Packet**: It was an HTTP GET request for `starter-template.css`.
- 
![Screenshot (27)](https://github.com/user-attachments/assets/6c6e4e2c-8fd8-44c8-9983-55096b2b3c7d)

### 6. Following the TLS Stream

To get more details from the decrypted packet, I followed these steps:

1. **Right-click on the Packet**: I right-clicked on the HTTP GET request packet.
   
2. **Follow TLS Stream**: From the context menu, I selected `Follow` > `TLS Stream`.

This option allowed me to view the full conversation of the decrypted TLS stream. Within this stream, I discovered the hidden flag.

![Screenshot (28)](https://github.com/user-attachments/assets/7da90cb0-5822-492c-b5d2-f430a6867690)


## Learnings and Insights

Throughout this challenge, I gained several valuable insights:

1. **RSA Key Usage in Wireshark**: I learned how to configure Wireshark to use RSA keys to decrypt TLS packets effectively.
2. **TLS Decryption**: Understanding the importance of RSA keys in decrypting TLS communications was enlightening.
3. **Wireshark Configuration**: Navigating through Wireshark’s preferences to configure various protocol settings was a hands-on experience.
4. **Packet Analysis**: Following decrypted TLS streams and analyzing the conversation details provided a deeper understanding of network traffic.

## Conclusion

This challenge was a good exercise in network traffic analysis and decryption. By following the detailed steps outlined above, I was able to decrypt the TLS packets using the provided RSA key and successfully uncover the hidden flag.

**picoCTF{nongshim.shrimp.crackers}**
