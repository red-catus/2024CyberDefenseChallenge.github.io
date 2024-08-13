# D8. YARA Analysis

You and the rest of The Lucky Lion's IR team are deep in your investigation, digging into hosts with signs of unusual activity. While pulling artifacts from host A (WDIGCVY2S), you identified the tool download utilized by the threat actor. You capture the file and submit it to Strelka, a real-time, container-based file scanning system used for threat hunting, threat detection, and incident response. The strelka.json results identified the file as AnyDesk and you determined the file was downloaded and utilized by the threat actor.
### Objectives
	• Create a YARA rule that will detect the target file. The target file has similar meta information identified in strelka.json. There are a total of 100 files, only one is the target file.
### Flag Format Typical CTF flag th4t_l00kz_l1k3_th1s
### Tools Required
	• yara
	• curl
### File
strelka.json is uploaded in the D8. Yara Analysis directory
### Additional Resources
	• YARA Documentation
	• YARA
	• Strelka
	• CyberChef
 ```
curl -H "Content-Type: text/plain" https://target-flask.chals.io/api/v1/yara-scan -X POST -d 'rule test {condition: true}'
```
From <https://target.ctfd.io/challenges#D8.%20YARA%20Analysis-12> 

# Solution

YARA is a tool used in malware investigations  or detection. With Yara, you can create a rule to describe your characteristics of the file or malware that is the target. Great sources to learn about this are listed at the end. 

![1st](https://github.com/user-attachments/assets/807c6a2e-49e4-4507-99c6-95456e17c71d)


This challenge was a doozy and I spent almost a whole day trying to figure this out. I spent a few hours using strings to test search for keywords, such as "AnyDesk" or "x-dosexec" and managed to bring down the target to 50/100. 

I had much better luck looking at the strelka file after taking a break. While skimming through the strelka file, I identified key descriptions: "mz_file" and the header beginning with "MZ." Google searches indicated that these are characteristics of pe file formats, which are used in Windows executables. Scrolling down the file,  I observed that there was "pe" header information listed here. This is what I focused to learn more about. 

![2nd](https://github.com/user-attachments/assets/e3ee5bad-5ff6-4bda-92c1-e2a4e3dde73d)


To test out the rule, I added the strings option  for "MZ" and imported the PE module to add the pe.checksum condition in my rule. Checksums are used to verify data integrity and look for errors or corruption during transmission, so I expected this to be unique. As seen in the result below, the rules were true for my target and I narrowed the target down to 8/100. 

![3rd](https://github.com/user-attachments/assets/0436ed81-5979-4a71-b901-77ee8fe389fe)


I decided to read up more about PE headers and malware detection. The main question here is what other characteristic would be flagged as abnormal?  This is when I was introduced to bloated overlays. Overlays are data added to the end of the pe file and are NOT part of the metadata. Threat actors use this as a way to avoid detection. (*See References below to learn more about Overlays). I tested to see if the overlay exists by using == 0. Result was false, so the target does have an overlay. 

![4th](https://github.com/user-attachments/assets/a3735b6c-5f93-4d81-9cf2-9eb7e0264524)


Considering that the overlay is not part of the pe metadata, I could not figure out the size and decided to just use > 0. If the overlay exists, it should be greater than 0. The results gave me the flag! 

![5th](https://github.com/user-attachments/assets/663da848-ad9d-4b88-86dd-538af3773bea)


I was not able to get this answer right away, but I was very ecstatic to be the **first** to complete this challenge! I tried multiple characteristics under pe, but most gave me either 2/100 or 3/100. In creating Yara rules or any kind of threat detection rules, it is important to learn about the type of file format you are working with and to understand the reported signatures threat actors use. In this case, quality matters. 

This challenge not only gave me firsthand experience in writing Yara rules, but also taught me the importance of fine tuning those rules to achieve better results. 



### NOTE:

After solving this, there was something bugging me about this target. During one of my deep dives into solving this problem, I went back to challenge D6. Since then I have been trying to tie this target to that downloaded exe in D6. I've used HEX dumps from the packet bytes in the PCAP file and they were true... But is there something else that might tie these 2 challenges together? 

Was my target the file from D6?! While trying different combinations, I found that the filesize written in the strelka file 5328201 was producing FALSE results. It's understandable considering the strelka file was that of a file SIMILAR to the target, not THE target. However, when I use filesize < 5328201, I get a TRUE result. 
 

![7th](https://github.com/user-attachments/assets/bc02c704-bebd-49b5-9140-a59c1648fe16)

I looked at the PCAP file from challenge D6 once again and found that the content length was VERY close to what's been reported in strelka. It was **5328200**! 

![4th](https://github.com/user-attachments/assets/409248dc-cb64-4e70-a2a0-8acf42336532)

![9th](https://github.com/user-attachments/assets/5155224a-f9c8-496f-b8e3-622e119e9619)

Using this, I ran the rule and was also able to obtain the flag. The strings were HEX dumps from the packet bytes from the PCAP analysis of the downloaded executable. 

![Screenshot_2024-08-12_17-22-20](https://github.com/user-attachments/assets/1c175c8c-e510-4f03-a229-04e913c63a09)

I don't think the principal security engineer who wrote this intended it solved this way, but I thought it was neat to tie these 2 challenges together. 



### References:

#### Learn more about Yara:

https://github.com/VirusTotal/yara/tree/master

https://yara.readthedocs.io/en/stable/modules/pe.html

#### Learn more about pe overlays:

https://squiblydoo.blog/2023/06/05/understanding-pe-bloat-with-malcat/ 

https://www.youtube.com/watch?v=pUjBJ4m7tUI

#### Learn more about checksums:

https://practicalsecurityanalytics.com/pe-checksum/
