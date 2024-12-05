The challenge I encountered was a format string vulnerability in a burger-serving program. Let's break down how I approached it and the reasoning behind my choices to exploit this vulnerability and ultimately retrieve the flag.

The code consists of a burger-serving application with two sets of customers: Patrick and SpongeBob. Haha. Each customer has a list of burgers, and I need to choose one to serve. The twist is that one of the burger lists contains **format specifiers**—a common vulnerability that, when exploited, can leak memory and allow me to retrieve the flag.

### First Customer: Patrick

When the program asks for my burger recommendation for Patrick, the menu includes three options:

- **Breakf@st_Burger**
- **Gr%114d_Cheese**
- **Bac0n_D3luxe**

None of these choices directly contain format specifiers like `%s` or `%d`.

However, one of the options, **Gr%114d_Cheese**, contains a `%` symbol followed by a number (114), which is unusual but does not directly cause a format string vulnerability.

Despite this, I chose **Gr%114d_Cheese** because it seemed like a good opportunity to experiment with input. It was correct.

I then proceeded with my selection and printed it. If the string length exceeded a certain threshold, the program would call the function `serve_bob`. In this case, the length of my choice did not trigger this condition, and I was prompted to serve the next customer, SpongeBob.

### Second Customer: SpongeBob

Now, the second customer, SpongeBob, had a more interesting burger menu with **actual** format specifiers that could trigger vulnerabilities:

- **Pe%to_Portobello**
- **$outhwest_Burger**
- **Cla%sic_Che%s%steak**

The choice **Cla%sic_Che%s%steak** contains two `%s` format specifiers. These `%s` specifiers, when passed to `printf`, instruct the program to expect string arguments.

If I don't provide valid strings, `printf` will instead attempt to read data from memory, potentially revealing sensitive information like the flag.

The format string vulnerability in **Cla%sic_Che%s%steak** is the key to triggering the flag leakage.

When I selected **Cla%sic_Che%s%steak**, the program attempted to print the string, but the `%s` specifiers didn't have corresponding arguments. As a result, the program began reading from the stack and printed memory content, including the flag stored in the variable `flag`.

The output I received looked like this:

```
ClaCla%sic_Che%s%steakic_Che(null)
```

In this case, the memory content was revealed as part of the program's output, and the flag was printed.

The format string vulnerability is exploited through the `%s` format specifiers in the burger name **Cla%sic_Che%s%steak**.

By using `%s` without corresponding arguments, I forced the `printf` function to read values from the stack.

Since the flag was stored on the stack, it was inadvertently printed when the program accessed the memory that contained it.

This type of vulnerability is actually very common in C programs that use functions like `printf` without proper input sanitization, allowing attackers to manipulate memory and read sensitive data.

This challenge demonstrated how format string vulnerabilities can be leveraged to leak information from a program's memory, which is a fundamental concept in reverse engineering and exploitation.

![Screenshot (1261)](https://github.com/user-attachments/assets/db3de99f-1953-4e2b-ae57-e4dcf056c1f6)


![Screenshot (1262)](https://github.com/user-attachments/assets/019d1216-752a-423c-bded-28c1e38ef193)


![Screenshot (1263)](https://github.com/user-attachments/assets/76d6e758-3827-4626-868a-842d228b2b6a)


![Screenshot (1264)](https://github.com/user-attachments/assets/4282cbac-3a38-40d0-bc09-7cbe556867f4)


![Screenshot (1265)](https://github.com/user-attachments/assets/67d95dcb-0816-475c-98a2-42018642a7a8)


**picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_c8362f05}**
