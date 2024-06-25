# >> Sudhanshu Chatterjee | Jun 17th '24

## TASK 1 - INTRODUCTION
#msf #metasploit #msfconsole

>Metasploit is the most widely used exploitation framework.

Yes you got that right! *Most widely used*, it's a powerful tool, supporting all phases of a pentest engagement. Recon to post-exploit, you name it, this got it!
Mainly 2 versions, info on that is on task! For more info on about *what's*, *how's*, *when's* and anything else, refer to note -> [[Metasploit - A must tool for a Pentest Engineer!]]

Like said before, because of being multi-talented, it's also used for vulnerability research and exploit development.
To list main components of Metasploit Framework:
- `msfconsole` : The cli interface
- `Modules` : supporting modules such as scanners, exploits, payloads, etc.
- `Tools` : Read on room. Too long to write here!

## TASK 2 - MAIN COMPONENTS OF METASPLOIT

Primary interaction with the tool is CLI based on *Metasploit Console*, which can be used with following command:
```bash
msfconsole
```
Once started, that's where we work with different `modules`.
>Modules are small components within the Metasploit framework that are built to perform a specific task, such as exploiting a vulnerability, scanning a target, or performing a brute-force attack.

Take a thorough read on all the mentioned modules and what they do. With special attention to the ones with sub-sections.
> If possible, read/research on differences between single/inline and staged payload. It can come in handy in future.

Rest is pretty much `textbook`, as far as the answers are concerned. 

## TASK 3 - MSFCONSOLE

Once you launch `msfconsole`, the cli changes, blah-blah-blah, yeah sure! Let's ping someone here... Let's ping `8.8.8.8`
```bash
ping -c 1 8.8.8.8
```
- `-c` is a switch, used with a number to specify no. of pings sent, single ping, in our case

But did you ever wonder, who's `8.8.8.8`? **Google it!**
Once you're done having fun, `clear` the screen. And trust me when I say this, ***use your own local machine as much as possible instead of the attack-box on website***.

Apart from that, you can mess around all you want, run all kinds commands like `history`, `help`, etc. Look around, learn what you can from yourself, but also do things mentioned in the task.

The demonstration task provides, on let say, using the `ms17_010` `eternal_blue` exploit, for illustration purposes. Rest is pretty nicely shown and explained over there!

And then there's ranking system for exploits. Read carefully about them, and try to remember what these mean, as they're gonna be useful in long run if you'll use Metasploit a lot!
For last answer in this task, I suggest running `msfconsole`, and try finding out on your own!

## TASK 4 - WORKING WITH MODULES

To list, there are few options, which need to be set manually in order to run your exploit such as:
- `RHOSTS` - Target IP
- `RPORT` - Target IP Port
- `LHOSTS` - Listener IP
- `LPORT` - Listener IP Port
- `PAYLOAD` - The payload used with the exploit
- `SESSION` - Each established connection with the target. Can be ID'd, mostly used in case of post-exploitation.

Unless you're an advanced attacker, most of the time, the `Listener` settings would be left to default, unless they are meant to be changed in order work with something specific. To put the info in these `options`, `set` command is used.
In order to list these option, you need to use the command:
```bash
show options
```
If you want to `unset` or reset all options to *global settings*, you can use the `setg` command.
The thing with **global settings** is that, you can set it whatever you're working on, and *use it on multiple modules*, *rather than* putting up the options *manually* each time.  

You can also view information of options *individually* as well, with the help of switch `-i`, like let say you wanna see the `session` options, it'll look like this (remember we are in `msf` console right now!) -
```bash

session -i
```

In the end, once you are done with all the setup aspect, you run the *command of your dreams*, i.e.
```bash

exploit
```

## TASK 5 - SUMMARY
As far as this room is concerned, that'll be it for Metasploit Framework here but, there's a whole lot more out there! Go check it out, and if you wish to run this exploit with like, a demo practical or something like that, try the room ['BLUE'](https://tryhackme.com/r/room/blue).
It'll prove to be a great exercise **AND** an awesome learning experience.