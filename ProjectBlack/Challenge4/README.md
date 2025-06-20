# ProjectBlack CTF Challenge 4 - Write-up

## About the Challenge

We’re provided with a [challenge4.txt](https://projectblack.io/ctf/challenge4.txt) file from the Project Black website. Upon inspection, it's clearly **encoded**, and our goal is to decode and analyze its contents.

![image](https://github.com/user-attachments/assets/a3f95a9b-b4a4-4845-8f25-d93fb4410a95)

---

## Step 1: Download and Decode

We start by downloading the `.txt` file.
![image](https://github.com/user-attachments/assets/7337f41b-32b8-4090-afe5-9dade117ce4e)

Then let's use a simple Base64 decode:
![image](https://github.com/user-attachments/assets/98046159-4005-4c51-b4b1-f60eb8d095ab)

The result is still not human-readable, but there are some hints of meaningful content.

---

## Step 2: Analyze the Decoded Data

![image](https://github.com/user-attachments/assets/f5add0da-2619-4fe0-929a-049a6efbea60)

![image](https://github.com/user-attachments/assets/43599323-f1b5-440f-b4ac-53b0ce4adc35)

![image](https://github.com/user-attachments/assets/2e926369-176e-4594-992b-e2915d1be17f)

Upon a closer look at the decoded file, we start to notice **HTTP/Web request patterns**.

This suggests that the original file might be network traffic in disguise.

---

## Step 3: Converting to PCAP for Wireshark

To inspect it further, we convert the decoded Base64 into a **.pcap** file using [CyberChef](https://gchq.github.io/CyberChef/#recipe=From_Base64%28'A-Za-z0-9%2B/%3D',true,false%29Detect_File_Type%28true,true,true,true,true,true,true/disabled%29).
![image](https://github.com/user-attachments/assets/810e0c14-b775-44f4-b44a-51f5133dcb26)

---

## Step 4: Opening in Wireshark

Now that we have a `challenge4.pcap`, we open it using **Wireshark** to investigate the traffic.
![image](https://github.com/user-attachments/assets/5d625cf3-659a-4762-9b33-8f3a35137180)

We filter for **HTTP traffic** and start following TCP streams:
![image](https://github.com/user-attachments/assets/a0796906-1ea0-419d-b3cb-893eb57787ed))

![image](https://github.com/user-attachments/assets/19792969-36bd-42aa-a89e-3538ff3b3628))

One of the streams hints at a suspicious URL: [`/supersecret`](https://projectblack.io/supersecret)

The page is a **404**, and the source is empty — dead end.
![image](https://github.com/user-attachments/assets/79461436-7422-45df-9c58-caf7f4a06e21)

---

## Step 5: Deeper Traffic Inspection

Looking further, we notice `neopets.com`, but that’s a real website and unrelated.
![image](https://github.com/user-attachments/assets/2cd3e9e9-fffa-48de-9491-78a2788be7ae)

So we go back to **analyzing more HTTP traffic**, which looks promising because there's a lot of **POST** traffic.
![image](https://github.com/user-attachments/assets/44740c5e-ebb6-406a-a4a7-8fd56ec9ef0f)

We export all HTTP objects for closer inspection:
!\[image]\([https://github.com/user-attachments/assets/0026aa30-b6d7-42b4-86f8-a701bb4b864d](https://github.com/user-attachments/assets/0026aa30-b6d7-42b4-86f8-a701bb4b864d))

!\[image]\([https://github.com/user-attachments/assets/f54cb1be-00d8-4a50-9171-792fa207427d](https://github.com/user-attachments/assets/f54cb1be-00d8-4a50-9171-792fa207427d))

Now we begin analyzing individual files.

---

## Step 6: Spotting the RSA Clue

Several files (`supersecret(6)` onwards) contain numeric data. However, `supersecret(2)` and `supersecret(4)` contain symbols like `%3D`, which translates to `=` in **URL encoding**. This is a classic sign of **Base64-encoded binary values**, likely part of RSA keys.
![image](https://github.com/user-attachments/assets/0c4ff81a-2b64-4224-9cd1-1efa42cbf2b1)

![image](https://github.com/user-attachments/assets/548c831b-cb22-478a-8910-91f0b9a9750f)

![image](https://github.com/user-attachments/assets/c5b2352b-2444-4738-a48c-f085bdb1ef64)

* `supersecret(2)` = Possibly **N** (modulus)
* `supersecret(4)` = Possibly **E** (public exponent)
* Others = Likely **ciphertexts**

We suspect that we’re dealing with **RSA encryption**.

---

## Step 7: RSA Decryption with Python & ChatGPT

With only N, E, and ciphertexts, we’re stuck unless we factor N into p and q — which is very difficult unless N is small. We can try using **ChatGPT** to help build a Python script that brute-forces small RSA keys or decrypts using known values.

Using the Python script provided, we input N, E, and the ciphertext files.

---

## Step 8: The Flag!

Running the decryption script...
![image](https://github.com/user-attachments/assets/7f3d7d1d-82bb-46b1-9a1f-5858842f533a)

 **Success! We get the flag:**

```text
PROJECTBLACK{Flag:YOU_KnoW_CryPTO_Too?}
```

---

## Conclusion

This challenge was a great blend of:

* Base64 decoding
* PCAP/network analysis using Wireshark
* URL encoding
* RSA cryptography fundamentals
* Python scripting for decryption

---

## Tools Used

* Kali Linux
* [CyberChef](https://gchq.github.io/CyberChef/)
* Wireshark
* Python 
* ChatGPT 

---

## References

* [RSA Algorithm Basics](https://en.wikipedia.org/wiki/RSA_%28cryptosystem%29)
