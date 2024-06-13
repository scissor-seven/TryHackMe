#linux #priv_esc #enumeration #os 
# >> Sudhanshu Chatterjee | Jun 9th' 24
## TASK 1 - INTRO
`Privilege Escalation` can be considered as **one of the most critical concepts** to understand, as it mainly involves understanding what an attacker can do to *escalate their privilege inside the target system, and do more damage or steal the data, which, otherwise, wouldn't have been possible*!

>*Doing this skill will add another essential skill in our arsenal for any purpose!*

## TASK 2 - WHAT IS PRIV-ESC?
>*At it's core, Privilege Escalation usually involves going from a lower permission account to a higher permission one. More technically, it's the exploitation of a vulnerability, design flaw, or configuration oversight in an operating system or application to gain unauthorized access to resources that are usually restricted from the users.*

`QUESTION` - **WHY IS IT IMPORTANT?**
It's rare that you get admin-rights as soon as you get access to your target. Therefore, priv-esc becomes crucial at that point. Some great examples are given in the [task](https://tryhackme.com/r/room/linprivesc) which tells us what are benefits of this.

FOR MORE INFO, REFER: [[Privilege Escalation]].

## TASK 3 - ENUMERATION
The first step towards escalation after gaining system access!. Unlike CTF's, pentest engagements don't end once target is compromised! Enumeration is important even post-compromise.
>You may have accessed the system by exploiting a critical vulnerability that resulted in root-level access or just found a way to send commands using a low privileged account.

I think it's better if you read about it on the task itself, because the [room](https://tryhackme.com/r/room/linprivesc) it better. It has info on all kinds of commands and stuff. If you want, you can refer to [[Linux Commands]] as well, to know more about them and more such commands. **I do that a lot!** 
#### --> PRATICAL
[[Practical - Linux Priv-Esc#TASK 3 - Enumeration]].

## TASK 4 - AUTOMATED ENUMERATION TOOLS
**Guess what you lazy bums(including myself), there are automation tools out there for you to do this!**
But who am I kidding, these tools save a whole lot of time in this enumeration process. *Like... crazy!* Still, just for the sake of saving time, **don't depend on them absolutely, since they might miss some priv-esc vectors.**

There are a few popular ones, even I have used 1-2 from these:
- **LinPeas**: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- **LinEnum:** [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)[](https://github.com/rebootuser/LinEnum)
- **LES (Linux Exploit Suggester):** [https://github.com/mzet-/linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- **Linux Smart Enumeration:** [https://github.com/diego-treitos/linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
- **Linux Priv Checker:** [https://github.com/linted/linuxprivchecker](https://github.com/linted/linuxprivchecker)

Try them on your local Linux, you can know better about them doing this!
## TASK 5 - PRIV-ESC : KERNEL EXPLOITS

The Kernel exploit methodology is simple;
1. Identify the kernel version
2. Search and find an exploit code for the kernel version of the target system
3. Run the exploit

**Research sources:**  
1. Based on your findings, you can use Google to search for an existing exploit code.
2. Sources such as [https://www.linuxkernelcves.com/cves](https://www.linuxkernelcves.com/cves) can also be useful.
3. Another alternative would be to use a script like LES (Linux Exploit Suggester) but remember that these tools can generate false positives (report a kernel vulnerability that does not affect the target system) or false negatives (not report any kernel vulnerabilities although the kernel is vulnerable).
4. My personal favourite is [ExploitDB](https://www.exploit-db.com/). One stop shop, although there's nothing to buy here! 😅

**Hints/Notes:**
1. Being too specific about the kernel version when searching for exploits on Google, Exploit-db, or searchsploit
2. Be sure you understand how the exploit code works BEFORE you launch it. Some exploit codes can make changes on the operating system that would make them unsecured in further use or make irreversible changes to the system, creating problems later. Of course, these may not be great concerns within a lab or CTF environment, but these are absolute no-no's during a real penetration testing engagement.
3. Some exploits may require further interaction once they are run. Read all comments and instructions provided with the exploit code.
4. You can transfer the exploit code from your machine to the target system using the `SimpleHTTPServer` Python module and `wget` respectively.

#### --> PRACTICAL
[[Practical - Linux Priv-Esc#TASK 5 - KERNEL EXPLOIT]].

## TASK 6 - PRIV-ESC : SUDO

*The sudo command, by default, allows you to run a program with root privileges. Under some conditions, system administrators may need to give regular users some flexibility on their privileges.*
[GTFOBins](https://gtfobins.github.io/) is a valuable source that provides information on how any program, on which you may have sudo rights, can be used. Great site, definitely give it a read!
**Basically** what this is trying to say is, users, with permission to use the `sudo` command can also be used as a vector to escalate privilege. As `sudo` command also allows user to run some programs or commands with root privileges!
A good example of how it's done, in very simple terms, is given in the task!

#### --> PRACTICAL
[[Practical - Linux Priv-Esc#TASK 6 - EXPLOIT WITH SUDO]].
## TASK 7 - PRIV-ESC : SUID
*By now, you know that files can have **read, write, and execute permissions**. These are **given to users within their privilege levels**. This changes with SUID (Set-user Identification) and SGID (Set-group Identification). **These allow files to be executed with the permission level of the file owner or the group owner, respectively.***

I think the above para is quite a hint, enough to get an idea of what this task means. In any case, if you still don't understand, do read more about on internet, it's never harmful to do some research now, is it?
All in all, this task was quite interesting to do, so do not skip this one! (Not like you have any other choice! 😏)
#### -->> PRACTICAL
[[Practical - Linux Priv-Esc#TASK 7 - SUID/SGID EXPLOIT]]
# TASK 8 - PRIV-ESC : CAPABILITIES
*Capabilities help manage privileges at a more granular level. For example, if the SOC analyst needs to use a tool that needs to initiate socket connections, a regular user would not be able to do that. If the system administrator does not want to give this user higher privileges, they can change the capabilities of the binary. As a result, the binary would get through its task without needing a higher privilege user.*

For more info on `Capabilities`, refer to [[Privilege Escalation#-> 'CAPABILITIES' In Linux]].
# TASK 9 - PRIV-ESC : CRON JOBS
#cron 
