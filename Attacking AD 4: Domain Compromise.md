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
An extremely critical and highly sensitive database used to store AD data within Active Director / Domain Controllers:
* user information
* gorup information
* security dscriptors
* password hashes

Using a **known domain admin account** we could use secretsdump and its switch of **-just-dc-ntlm** to dump out the DCs NTDS file. A successful Domain dump will contain a range of information from: SAM, credentials, domains, krbtgt, hashes, local account users, pc logins, other accounts that may of been created from the ipv6 relay attack.  

impacket-secretsdump / secretsdump 'impacket-secretsdump MARVEL.local/pparker:'Password1'@192.168. continue here
