# About the Challenge
If you head to the [CTF](https://projectblack.io/ctf/challenge4.txt) website, we're just given a .txt file that is encoded.
![image](https://github.com/user-attachments/assets/a3f95a9b-b4a4-4845-8f25-d93fb4410a95)

# Decoding
Now let's try decoding this with base64. First let's download this .txt file from the website
![image](https://github.com/user-attachments/assets/7337f41b-32b8-4090-afe5-9dade117ce4e)

Now, we can decode it with base64.
![image](https://github.com/user-attachments/assets/98046159-4005-4c51-b4b1-f60eb8d095ab)

Now even after decoding, the text is still undreadable. But, let's look further for some text that converted into plain text.

# Analyzing the Decoded .txt file

Upon analyzing, I noticed that there are some "Web Request" text!
![image](https://github.com/user-attachments/assets/f5add0da-2619-4fe0-929a-049a6efbea60)

![image](https://github.com/user-attachments/assets/43599323-f1b5-440f-b4ac-53b0ce4adc35)


![image](https://github.com/user-attachments/assets/2e926369-176e-4594-992b-e2915d1be17f)

For this findings, let's further investigate this in Wireshark. First, we need to convert this .txt file into .pcap file.

I'm using [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64('A-Za-z0-9%2B/%3D',true,false)Detect_File_Type(true,true,true,true,true,true,true/disabled)) for this one.

![image](https://github.com/user-attachments/assets/810e0c14-b775-44f4-b44a-51f5133dcb26)

# Using Wireshark
