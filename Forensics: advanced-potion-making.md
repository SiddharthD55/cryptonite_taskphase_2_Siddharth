This writeup has the steps I took to identify and recover the corrupted PNG file. During the process, I encountered various challenges and learned valuable insights on how to handle corrupted image files using hex editors and online image editors.

### Step-by-Step Process

#### Step 1: Initial File Check
Initially, I attempted to determine the file type using the `file` command:
```
file advanced-potion-making
```
Unfortunately, this command did not provide any useful information about the file type, I just got `advanced-potion-making: data`, suggesting it could be corrupted; at least that's what I thought.

#### Step 2: Inspecting the File with a Hex Editor
To investigate further, I opened the image file on hexed.it, a user-friendly hex editor. Upon inspection, it was evident that the file was indeed corrupted.

![Screenshot (14)](https://github.com/user-attachments/assets/c1ac1985-83d4-46d6-ac53-44c3f8d661af)


#### Step 3: Identifying the File Type
To identify the file type, I searched for common image file headers using a reference. After comparing the file's header with those listed on Wikipedia, I deduced that the file should be a PNG. The hexadecimal header for a PNG file is:
```
89 50 4E 47 0D 0A 1A 0A
```

![Screenshot (13)](https://github.com/user-attachments/assets/6f14f01b-3e4f-485d-8508-6bb064a1272a)


![Screenshot (15)](https://github.com/user-attachments/assets/fbaaadb9-a669-4159-b570-c599328c49a5)


#### Step 4: Correcting the Header
Using the hex editor, I corrected the file header to match the expected PNG header.

![Screenshot (21)](https://github.com/user-attachments/assets/e5a97a0f-2840-4dea-b047-478b7d274375)


After making these changes, I attempted to open the image file again. The file opened, but it displayed a red-colored image with no discernible content.

![Screenshot (16)](https://github.com/user-attachments/assets/c84b0544-b282-4444-aa75-544a36c42557)


#### Step 5: Enhancing the Image
To reveal the actual content, I used an online hex image editor with color change and black-and-white filter options. By adjusting the color settings and applying the black-and-white filter, I could finally read the hidden flag within the image.

![Screenshot (19)](https://github.com/user-attachments/assets/907435ed-ead3-4531-92a4-13c110d99b2d)


![Screenshot (20)](https://github.com/user-attachments/assets/3f91016a-e9cb-4120-9182-3f280c40dc8e)


### Learnings and Insights
1. **Hex Editors are Powerful**: Hex editors like hexed.it can provide detailed insights into corrupted files and allow for precise modifications to recover data.
2. **Understanding File Headers**: Knowledge of common file headers is crucial for identifying and correcting corrupted files.
3. **Image Enhancement Techniques**: Online image editors with advanced features can be extremely useful for revealing hidden content within images.

### Conclusion
Recovering a corrupted PNG image file involved several steps, including identifying the file type, correcting the file header, and enhancing the image using online tools. This process highlighted the importance of hex editors and image enhancement techniques in data recovery. Through this experience, I gained valuable knowledge that can be applied to similar challenges in the future.

**picoCTF{w1z4rdry}**
