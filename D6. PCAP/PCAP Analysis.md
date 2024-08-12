# D6. PCAP Analysis

You have been given a PCAP file of Bob's browsing traffic concerning a known Remote Access tool. You'll need to examine the PCAP to determine which tool was downloaded on the host and where it came from.
Password for Zip: infected

### General Questions:
	• Did the user have any suspicious downloads?
	• What is the purpose of the executable?
	• What was the domain the executable was downloaded from?
	• What is the source port?
## Objectives
	• Determine if there was a suspicious download, what it was, and where it came from. Identify the full request URI that the executable was downloaded from.
### Tools Required
	• Wireshark or a similar tool.
### Additional Resources
	• Wireshark crash course:  https://www.youtube.com/watch?v=vUdOxcRJgME

 ### File
 pcap.zip found in the D6. PCAP directory

From <https://target.ctfd.io/challenges#D6.%20PCAP%20Analysis-28> 

# Solution

PCAPs are files used to store information about packet captures (www.endace.com/learn/what-is-a-pcap-file). There are many open source tools that can be used to read or write PCAP files and the 2 I'm most familiar with are tcpdump and Wireshark.  In this challenge, Wireshark is the best tool to use given the amount of information it provides and newbie friendly UI. Wireshark is a very popular network protocol analyzer (learn more and downloads available at: www.wireshark.org). 


After unzipping the file, we are left with a file: anydesk_hostonly.pcapng. To open the file using Wireshark, navigate to file> open > select the pcap file. The screenshot below shows what the file looks like after opening. Each packet capture is color coded based on the type of protocol. In the screenshot below, TCP ACK requests are in light gray, TCP FIN/FIN,ACK are in dark gray, and HTTP are in light green. 

![1st](https://github.com/user-attachments/assets/86e01053-1809-4f1f-bcc7-fbef005c7955)


There are a total of 196 packets in the file, so trying my luck by looking at each 1 by 1 would take up too much time. Since we are looking for suspicious downloads, we can narrow down our search by using the "find packet" option under the "Edit" tab. Another way to do this would be to press CTRL + F. I changed the first option of "Packet List" to "Packet Details to allow for the search to look the packet details. Since I will be searching for an executable or "exe," I also changed the "Display Filter" option to "String". Finally, I searched for "executable" and did not have any results. This made sense considering that executables are usually written in the format ".exe," so I searched for this key term and found matches!

 ![2nd](https://github.com/user-attachments/assets/4462cfb0-9afa-44f8-ac60-00b30080c92b)


The match show a request to download the AnyDesk executable. By looking deeper into this packet detail, I observed a Full request URI. 

![3rd](https://github.com/user-attachments/assets/a1df83c7-e7cd-40ef-afb3-02f02400cca7)


**The answer was: http://anydesk.com:8000/AnyDesk.exe**


A different approach because Wireshark is so cool: 

There are many ways to filter results and find information in Wireshark. Another way to filter out the noise is to apply a display filter, which is found next to the blue bookmark icon. You can use the filter:  http contains ".exe" and the output will be http packets which will contain the key word ".exe." This removes the other TCP/HTTP packets that are irrelevant to the challenge. 
 

![4th](https://github.com/user-attachments/assets/a497202b-42a3-4ed4-9923-a89664a85b46)



### Note

If you aren't interested in looking at the details of each packet and just want to skim through the PCAP file, you can use tcpdump. A simple command to use is: tcpdump -r yourfile.pcap



### To learn more about Wireshark:

www.wireshark.org
