## OSINT 

### Intelligence Lifecycle
1. planning and direction
2. collection
3. processing and exploitation
4. analysis and production
5. dissemination and integration

### Handy Tools 
* Greenshot
* Obsidian

---

### Sock Puppets
online Identity that is not who you are, aka fake account, fake person, synthetic account.

**Creating an account**  
1. https://web.archive.org/web/20210125191016/https://jakecreps.com/2018/11/02/sock-puppets/
2. https://www.reddit.com/r/OSINT/comments/dp70jr/my_process_for_setting_up_anonymous_sockpuppet/
3. https://www.secjuice.com/the-art-of-the-sock-osint-humint/
4. https://www.fakenamegenerator.com/

**Image creation**  
1. https://www.thispersondoesnotexist.com/

**Layered CCs**  
1. https://privacy.com/join/LADFC 

---

### Search Engine Operators
1. google  
    * google advanced search https://www.google.com/advanced_search
    * google advanced search guide http://www.googleguide.com/print/adv_op_ref.pdf

2. Bing
    * https://www.bing.com/
    * Bing advanced search guide https://www.bruceclay.com/blog/bing-google-advanced-search-operators/
  
Others:  
1. Yandex - Russian Based
2. DuckDuckGo - More privacy based
3. Baidu - Asian based

**Examples of advanced google searching:**  

site:Tesla.com password filetype:pdf [parse through different file types i.e xlsx, docx, doc,]  
"tesla.com" filetype:xlsx pwd  
site:tesla.com filetype:pdf pass  
site:tesla.com -www [removes www and looks for subdomains]  
"bobby" intext:password site:newgrounds.com  
"bobby" intitle:password site:newgrounds.com  
"bobby" inurl:password site:newgrounds.com  

---

### Image OSINT
**Reverse Image Searching:**  
Google Image Search - https://images.google.com  
Yandex - https://yandex.com  
TinEye - https://tineye.com  
Pimeyes - https://pimeyes.com

**Viewing EXIF Data**  
Essentially meta data that may reside in the background of an image  
Jimpl - https://jimpl.com/  

**Physical Location OSINT**  
Gaining access to a building  
1. google maps
2. drones

**Identifying Geograpgical Locations**
1. https://somerandomstuff1.wordpress.com/2019/02/08/geoguessr-the-top-tips-tricks-and-techniques/
2. https://www.geoguessr.com

---

### Email OSINT
**Discovering Email Addresses**
1. Hunter.io - https://hunter.io/
2. Phonebook.cz - https://phonebook.cz/
3. VoilaNorbert - https://www.voilanorbert.com/
4. Email Hippo - https://tools.verifyemailaddress.io/
5. Email Checker - https://email-checker.net/validate
6. Clearbit Connect - https://chrome.google.com/webstore/detail/clearbit-connect-supercha/pmnhcgfcafcnkbengdcanjablaabjplo?hl=en

'Forgot password' could help validate an email along with Email hippo and Email checker.

---

### Password OSINT
**Hunting Breacked PW 1**
* Dehashed.com
* hashes.org - could help identify valid hashes
* https://www.tunnelsup.com/hash-analyzer/ - could help identify valid hashes

**Hunting Breacked PW 2**
* WeLeakInfo - https://weleakinfo.to/v2/
* LeakCheck - https://leakcheck.io/
* SnusBase - https://snusbase.com/
* Scylla.sh - https://scylla.sh/
* HaveIBeenPwned - https://haveibeenpwned.com/

---

### Username OSINT
**Hunting Usernames and Accounts**
* NameChk - https://namechk.com/
* WhatsMyName - https://whatsmyname.app/
* NameCheckup - https://namecheckup.com/

---

### Username OSINT
[study later]

---

### Social Media OSINT
[study later]

---

### Website OSINT
* BuiltWith - https://builtwith.com/
* Domain Dossier - https://centralops.net/co/
* DNSlytics - https://dnslytics.com/reverse-ip
* SpyOnWeb - https://spyonweb.com/
* Virus Total - https://www.virustotal.com/
* Visual Ping - https://visualping.io/
* Back Link Watch - http://backlinkwatch.com/index.php
* View DNS - https://viewdns.info/
* urlscan.io - https://urlscan.io/
* DNSdumpster - https://dnsdumpster.com/
* Web Check - https://web-check.as93.net/
* crt.sh - https://crt.sh/ - Example to search for subdomains %.tesla.com

* Shodan - https://shodan.io
* Wayback Machine - https://web.archive.org/
* google = check through cached data 
  
---

### Business OSINT  
* Open Corporates - https://opencorporates.com/  
* AI HIT - https://www.aihitdata.com/

---

### Wireless OSINT  
[study later]  

---

### Building an OSINT Lab  
[study later]  

---

### Working with OSINT Tools  

**Image Location - exiftool**  
`exiftool image.jpg`   
`exiftool image.jpg > image.txt`  

**Hunting Emails and Breached Data - DeHashed API Tool**
TCM H.A personal .py api script to pull dehashed creds.

**Username and Account**  
`sherlock juon`

**Phone number**  
[study later]

**Social Media**  
InstagramOSINT script https://github.com/sc1341/InstagramOSINT - Suspected API key has changed  
navigate to /opt/instagramOSINT  
`./main.py --username 21savage`  
`python3 --username 21savage`  

**OSINT Frameworks**  
[DEFINITELY STUDY THIS LATER]  

**Other Tools**  
Hunchly - https://hunch.ly [very good tool espicially for investigative work]  

---

### Automation Foundations

**Website OSINT Tools**
1. whois
2. subfinder

**Automated tools**  
Simple single scanner with live check  
https://github.com/Ju-On/Tools/blob/main/subdomainhunterV1.sh

Aggresive multi scanner with live check and screenshot [can cause rate limiting, so comment out specific scanners]  
https://github.com/Ju-On/Tools/blob/main/subdomainhunterV2.sh

---

**Report Writing**

Front page
Table of content
Summary
Technical Evidence
   * OSINT
   * Link
   * Notes

