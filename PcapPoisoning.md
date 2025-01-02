Here, I'll take you through my experience analyzing the `trace.pcap` file, leading to the discovery of a flag for the challenge. I’ll share the step-by-step process, the tools I used, and the insights I gained along the way.

## Tools I Used

1. **strings**: This command-line tool extracts printable strings from binary files.
2. **grep**: A command-line tool used for searching text using patterns.
3. **Wireshark**: A powerful network protocol analyzer for capturing and analyzing network traffic.

## The Process

### 1. Navigating to the Correct Directory

First, I navigated to the `/home` directory to access the `trace.pcap` file:

```bash
root@MyLenovoDude:~# cd /home
```

### 2. Extracting Strings from the PCAP File

Next, I used the `strings` command to pull out human-readable strings from the binary PCAP file, and then piped the output to `grep` to search for `picoCTF`:

```bash
root@MyLenovoDude:/home# strings trace.pcap | grep picoCTF
```

This command helped me search for the occurrence of `picoCTF` in the extracted strings.

### 3. Finding the Flag

The command revealed the flag.

### 4. Validating with Wireshark

To ensure the flag was valid and to understand its context, I opened the `trace.pcap` file in Wireshark:

```bash
root@MyLenovoDude:/home# wireshark trace.pcap
```

I carefully inspected the packets in Wireshark and found the flag embedded in packet 507.

## What I Learned

### Key Takeaways

1. **Efficiency of Command-Line Tools**: Using `strings` and `grep` proved to be a quick way to filter relevant information from a binary file.
2. **Comprehensive Analysis with Wireshark**: While command-line tools provided a fast initial discovery, Wireshark was indispensable for a thorough analysis and validation of the findings.

### Why I Took These Steps

1. **Directory Navigation**: Ensuring I had access to the correct file.
2. **Using `strings` and `grep`**: These tools are excellent for quickly extracting strings and searching within large data files.
3. **Wireshark Analysis**: Provided a detailed, packet-by-packet analysis to understand the data's context and confirm the flag.

## Conclusion

By using a combination of `strings`, `grep`, and Wireshark, I was able to successfully uncover the picoCTF flag in the `trace.pcap` file. This process highlighted the strengths of command-line utilities for initial analysis and the necessity of protocol analyzers for in-depth investigation.

![Screenshot (9)](https://github.com/user-attachments/assets/a243dd25-c141-4646-99cc-52ddb4d764e5)


![Screenshot (10)](https://github.com/user-attachments/assets/5d4a32f2-7885-43da-b8d2-470d3ac06fc5)


**picoCTF{P64P_4N4L7S1S_SU55355FUL_5b6a6061}**
