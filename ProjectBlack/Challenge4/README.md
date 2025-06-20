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

Let's open the challenge4.pcap!
![image](https://github.com/user-attachments/assets/5d625cf3-659a-4762-9b33-8f3a35137180)

Now, I'll check some HTTP traffic.
![image](https://github.com/user-attachments/assets/a0796906-1ea0-419d-b3cb-893eb57787ed)

Let's click on any of these and follow the TCP Stream!
![image](https://github.com/user-attachments/assets/19792969-36bd-42aa-a89e-3538ff3b3628)

BOOM! We have some lead! Let's check the [https://projectblack.io/supersecret]!

![image](https://github.com/user-attachments/assets/79461436-7422-45df-9c58-caf7f4a06e21)
We see that the website is 404. Upon checking the "Page Source", there is nothing useful.

Let's find a different traffic and follow the "TCP Stream" again

![image](https://github.com/user-attachments/assets/2cd3e9e9-fffa-48de-9491-78a2788be7ae)
We found a new website which is the [neopets.com]. But upon checking the website, its a legitimate website and not part of the CTF.

Let's go back to Wireshark and examine more HTTP Traffic!

![image](https://github.com/user-attachments/assets/44740c5e-ebb6-406a-a4a7-8fd56ec9ef0f)
This is interesting! There is a lot of 'POST" Traffic, let's focus at this HTTP Traffic. 

Let's export all these so we can analyze is much better!
![image](https://github.com/user-attachments/assets/0026aa30-b6d7-42b4-86f8-a701bb4b864d)

![image](https://github.com/user-attachments/assets/f54cb1be-00d8-4a50-9171-792fa207427d)
Now, let's analyze these files!

![image](https://github.com/user-attachments/assets/3c70fbdf-176e-49d9-bde3-370b876e53b0)
This is interesting... Let's look more of these files.

![image](https://github.com/user-attachments/assets/0c4ff81a-2b64-4224-9cd1-1efa42cbf2b1)

![image](https://github.com/user-attachments/assets/548c831b-cb22-478a-8910-91f0b9a9750f)

![image](https://github.com/user-attachments/assets/c5b2352b-2444-4738-a48c-f085bdb1ef64)

Upon looking at these files, the "supersecret(6) onwards are all numbers. And the "supersecret(2)" and "supersecret(4) contains a Letter and Symbols. If you guys are familiar with HTML URL Encoding, the "%3D" can represents "=" sign. So base on these two files, we can assume that we have the N and E value and we know the rest are just numbers.

This look like a RSA encrpytion! But knowing the RSA, we need more value to decrypt a RSA encryption. Let's search it up!
![image](https://github.com/user-attachments/assets/209467ac-597d-4780-857b-f5628d7c32f3)

Let's use the best tool (ChatGPT) to find the missing values.
![image](https://github.com/user-attachments/assets/4af7c7ce-19eb-4110-bbe5-2d64eb9ebd85)

Personally, I'm a beginner in Python so let's ask the best tool to make us a script that decrypt cipher text in "supersecret" files excluding "supersecret (2) & (4)" using the RSA decrypt formula.

Using the script given us by the best tool. Let's try running it!

AND THERE! WE GOT THE FLAG!
![image](https://github.com/user-attachments/assets/7f3d7d1d-82bb-46b1-9a1f-5858842f533a)
PROJECTBLACK{Flag:YOU_KnoW_CryPTO_Too?}
