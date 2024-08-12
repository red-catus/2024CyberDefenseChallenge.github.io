# D2. Look for Insider Threats

In addition to securing your perimeter, it would probably be a good idea to double-check that you don't have any insiders working against The Lucky Lion, especially knowing Tacky Termite has occasionally used insiders to help them gain access to their victims' environments in the past.
One standard way to look for insider threats is to try to find sensitive data in places it shouldn't be. As a member of the Data Loss Prevention team, you could craft a Regular Expression (RegEx) to find TINs, or Tax Identification Numbers stored in unusual locations in The Lucky Lion's environment. The Lucky Lion is required to store TINs (only Social Security Numbers and Individual Taxpayer Identification Numbers) for gamblers who win more than $5000 (the regulations don't say how they have to store them, though!), though they should never appear anywhere other than the database that's intended to store them.
Normally, this task wouldn't be too hard, and there are lots of examples out there for TINs already. Unfortunately, the decision was made at one point to "encrypt" the TINs in a misguided attempt to increase security. Your job is now much more fun™!
The "encryption" method, which they've taken to calling Visionàry Algorithm Protecting IDs, involves modifying each digit using its corresponding value in the passphrase: LUCKYLION
```
def vapid(tin, key="LUCKYLION") -> bytes:
    if isinstance(key, str):
        key = key.encode("ascii")
    if isinstance(tin, bytes):
        tin = tin.decode("ascii")
    key_len = len(key)
    ciphertext = []
    for idx, character in enumerate(tin):
        ciphertext.append(int(character) + key[idx % key_len])
    return bytes(ciphertext)
```
    
### For example:
	• 000000000 becomes LUCKYLION
	• 111111111 becomes MVDLZMJPO
## Objective
Your mission is to write a RegEx that can find these obfuscated TINs so it can be deployed into various DLP sensors. This will ensure we'll be alerted if someone or something is exfiltrating sensitive customer data.
Fortunately, your coworker wrote a script (snort.py that you can use to test your RegEx against a representative dataset. Download the script and run it with --help to get started:
python snort.py --help

### Flag Format
found unauthorized user handling TINs: <flag> Example: If snort.py outputs: found unauthorized user handling TINs: ins1d3r_thr34t, then the flag would be ins1d3r_thr34t
Otherwise, you'll see errors like:
	• valid regex, but ill-fitting
	• malformed regex: <error message>
### Tools Required
	• snort.py, SHA256 verification hash: ff3aec78659b82907bc9f34886b785850dc3988b79b33f167de196e14e7a2d87
	• Python 3

### Files
snory.py is found in the directory: D2. RegEx, not Rejects!

From <https://target.ctfd.io/challenges#D2.%20Look%20for%20Insider%20Threats-29> 


# Solution

The first time I dealt with RegEx in a bioinformatics course, I thought it was a nightmare and incredibly complex. However, the use of RegEx does save time when doing searches or processing texts. 

Looking at the encryption method, we can do a quick glance and pick out important details. I'm not Python expert, but this was my understanding of the first 5 lines: 

	• Vapid is function being defined with key value being LUCKYLION and the function returns as bytes object.
	• If key is a string, ASCII encoding is used to convert it to bytes
	• If tin is a bytes object, ASCII decoding should be used to convert it to strings

The method reveals that ASCII plays an important role in the encryption. The example assigns LUCKYLION the digits 000000000. When moving up 1 letter, we get MVDLZMJPO which is assigned the digits 111111111. Through this example, we can assume that the each digit in the TIN will be between 0-9, so each digit will have a 10 possible combinations. To form the correct RegEx, we will have to  figure out the correct ranges to write using the ASCII table. In simpler terms, we would have to figure out the possible characters corresponding to 2-9. 

To begin, I used the key value: LUCKYLION. Starting from the target letter, I counted from 1 to 10 and used the characters at 1 as the beginning of the range and 10th character as the end. In the example below: Starting with L, I counted to 10 and landed on U. In RegEx, it will be written as [L-U]. I continued this count with the rest of the letters and ended up with: [L-U][U-^][C-L][K-T][Y-b][L-U][I-R][O-X][N-W]

![1st](https://github.com/user-attachments/assets/5af12b2b-e002-4792-a269-d2af0e1c5b70)


In order to form the RegEx, I utilized an online RegEx testing tool at the webpage regex101.com. This tool offers the options to check your RegEx AND details explanations for what each component of your RegEx will search matches for. As you can see below, the 'test strings' I used both matched their results (see 'match information') which confirms that the RegEx should work.

![2nd](https://github.com/user-attachments/assets/bfe8cc60-c59c-41db-b59b-e16d58e8deb6)


I ran the python snort.py -h option to double check the format. After following instructions, I ran the script to check my RegEx: 
 ```
python snort.py '[L-U][U-^][C-L][K-T][Y-b][L-U][I-R][O-X][N-W]'
```

![3rd](https://github.com/user-attachments/assets/bb379d93-6823-44f4-b58e-734bd5cc05ed)



It was a success. **The answer was: RegexRanger**


### Easier way to do this: 

Take the decimal value assigned to the character, add 9, and use the corresponding character for that value as the end of your range. 

L= 76 + 9=85 85

85=85 U

So for L, it would be [L-U]


## Lesson Learned:

For these challenges, I realized that I can spend a couple of hours trying to solve a problem… After I solve it, I figure out EASIER, time saving ways to get my answer. My brain does like to run marathons using flipflops sometimes. 

RegEx isn't so bad… As long as you use the regex testing tools and it doesn’t spit out "ILL-FITTING" results. 



## To learn more:

### RegEx:

https://www.regexbuddy.com/regex.html

### ASCII Table:

https://www.asciitable.com/


