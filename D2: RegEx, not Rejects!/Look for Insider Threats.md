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

From <https://target.ctfd.io/challenges#D2.%20Look%20for%20Insider%20Threats-29> 
