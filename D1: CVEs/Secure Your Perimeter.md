# D1. Secure Your Perimeter
## Scenario

After receiving a tip from a peer organization's Cyber Threat Intelligence team that prolific threat actor group Tacky Termite recently posted that they're gearing up to cyber-heist a casino, you've been tasked with making sure The Lucky Lion Casino is secured against any such cyber attacks.
As if it were that simple! You know it's impossible for any company to be fully protected against attacks, but you can certainly make it more difficult for any would-be attacker.
Let's start by eliminating any easy entry points by scanning The Lucky Lion's network for vulnerabilities. After running your vulnerability scanning tool Centauros, you now have a very long list of potential security issues impacting hosts in your environment.
It would definitely be best to remediate ALL of these issues, but who knows how much time you have before an attacker also discovers the vulnerabilities? You will need to prioritize the most critical issues first - take a look at the list and identify the CVE of the vulnerability that would be most prudent to remediate quickly.

### Objectives

Identify the most critical vulnerability that should be remediated first
 
**Flag Format CVE-year-number**

## Files
Found under D1: CVEs directory



# Solution

The vulnerability report can be viewed using a text editor and  includes reports ranging from low severity to critical. Common Vulnerabilities and Exposures or CVEs are used to classify and assign IDs to publicly disclosed vulnerabilities. CVEs are written in the format: CVE-YEAR-ArbitraryDigits, i.e. CVE-2024-10000. There are many great resources to learn more about CVEs, but the one that I used for this writeup was: cve.org. 

![0](https://github.com/user-attachments/assets/0c20dca2-6368-43dd-9ec8-944c754c94aa)



The objective is to find the MOST critical vulnerability. The text editor and search option can be used, but using the command-line would make it easier to extract the relevant data. The grep command can be used to find the keyword "Critical" in the report. Grep is a Linux command-line utility used to find text in a file. It only prints matches (unless direct it not to), so it helps minimize the noise when working with reports like this. Through this command, I was able to find that there are at least 5 vulnerabilities labeled critical in severity. 


![1st](https://github.com/user-attachments/assets/ffe3dadd-d915-4159-beef-7eb5c4ba8421)



Going back to the text editor, I observed that each vulnerability is written in detail consisting of 6 lines: vulnerability name, synopsis, description, solution, cve, and severity name. Using this information, I added a parameter in my grep command to  extract the information I needed. (You may use the grep --help command to show options or google helpful Linux cheat sheets!) 

By adding the **-B 5** to my grep command, the command prints both the matching line and the 5 lines before that match. Results are shown below.

![2nd](https://github.com/user-attachments/assets/dc88c3a4-59b5-4ac9-9d51-af8ef3a28e44)


Looking at the results, I observed that the critical vulnerabilities included unsupported or vulnerable older versions of Chrome, Python, Windows, and Edge. These should be remediated, but the most alarming vulnerability listed is CVE-2024-29994 or the Windows 10 Version 21H2 Security Update. 

![3rd](https://github.com/user-attachments/assets/98e9a4b9-2fbe-4863-be1e-35a49bf5c111)


Before submitting the answer, I wanted to get more information about the vulnerability. This can be done using different webpages to lookup the CVE. I used cve.org to look up the CVE-2024-29994. The search results detailed that this vulnerability may lead to the escalation of privilege and full access to SYSTEM privileges. To put it in simpler terms, this is similar to leaving your laptop or phone unlocked and unattended in a public space.  


![4th](https://github.com/user-attachments/assets/0a2f9766-574b-4f67-90af-4dad74d0b2b7)


**The answer was CVE-2024-29994.**







### Learn more about CVEs:

https://www.cve.org/About/Overview


Learn more about grep + different ways to use it:

https://www.redhat.com/sysadmin/find-text-files-using-grep






