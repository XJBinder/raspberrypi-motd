## Raspberry Pi MOTD (Custom Fork)
Customized dynamic message of the day (motd) for Raspberry Pi (Original Repo: https://github.com/ar51an/raspberrypi-motd)
<div align="center">

![motd](https://img.shields.io/badge/-motd-D8BFD8?logo=themodelsresource&logoColor=3a3a3d)
&nbsp;&nbsp;[![release](https://img.shields.io/badge/release-v1.6.1--custom-90EE90?logo=rstudio&logoColor=8FBC8F)](https://github.com/XJBinder/raspberrypi-motd/releases/tag/v1.6.1-custom)
&nbsp;&nbsp;![downloads](https://img.shields.io/github/downloads/xjbinder/raspberrypi-motd/total?color=orange&label=downloads&logo=github)
&nbsp;&nbsp;![lang](https://img.shields.io/badge/lang-Bash-5F9EA0?logo=gnubash&logoColor=4EAA25)
&nbsp;&nbsp;![license](https://img.shields.io/badge/license-MIT-CED8E1)
</div>

### Preview
<div align="center">
<img width="508" height="272" alt="Image" src="https://github.com/user-attachments/assets/cdfab2ae-1287-4073-aeb5-779d1e5b507f" />
</div>

---
<div align="justify">

### Intro
Many folks use Raspberry Pi as headless. If you use SSH more often to connect Raspberry Pi and interested in changing the MOTD (message of the day), keep on reading. **The goal is to create simple, swift and useful motd**.
<br/>

The goal of this fork is to simplify the MOTD display and add a touch of visual flair. I removed the update count out of personal preference and replaced it with a stylized ASCII Raspberry Pi logo. If your still interested in displaying the update count I would definitely recommend taking a look at ar51an's repo, he did a great job.
<br/>

The process used for the motd is the same as RaspiOS or Ubuntu uses to show the motd dynamically. There is no additional package or third party tool used. It is written in bash and executes using the same mechanism as the default motd. There are multiple commands for retrieving the same information. This dynamic motd takes approximately 1 sec after authentication. This is the fastest you can get with all the information it is displaying.
<br/>

#### Specs:
> |HW                      |OS                      |
> |:-----------------------|:-----------------------|
> |`Raspberry Pi 5 Model B`|`raspios-bookworm-arm64`|
#
### Steps
#### ❯ Remove Default MOTD

* Delete `/etc/motd`. This file contains the static text about Debian GNU/Linux liability. Alternatively you can keep a backup of this file at some place.
  > `sudo rm /etc/motd`  

* Delete `/etc/update-motd.d/10-uname`. This file prints the OS version dynamically to the default message. New motd will print a trimmed down version of OS.  
  > `sudo rm /etc/update-motd.d/10-uname`  

* Modify `/etc/ssh/sshd_config`. This file prints last login timestamp and location. New motd will print last login timestamp and location.  
  > `sudo nano /etc/ssh/sshd_config`  
  > Add: `PrintLastLog no`  
  > Save & Exit  
  > Restart sshd: `sudo systemctl restart sshd`  

#
> **_NOTE:_**  
> Default motd is completely removed. Reconnect ssh session and if you followed the steps correctly you will not see any motd.  
#

#### ❯ Implement New MOTD
* Copy `10-welcome` and `15-system` scripts from the latest release under `update-motd.d` dir to `/etc/update-motd.d` dir. 
Make sure scripts are under the ownership of root and are executable.
  > **Change ownership [If needed]:**  
  > `sudo chown root:root /etc/update-motd.d/<script-name>`  

  > **Make scripts executable [If needed]:**  
  > `sudo chmod +x /etc/update-motd.d/<script-name>`  

  > <img width="412" height="85" alt="Image" src="https://github.com/user-attachments/assets/4ff9d073-dbe9-49ec-9e8e-50a3fc8d317e" />

#
> **_NOTE:_**  
> Let's test new motd to verify that everything is working properly. **Either** reconnect ssh session **or** run the new motd from the same shell. If steps were followed correctly you will see new motd, something similar to the preview.  
> > **Run motd from the same shell:**  
> > `sudo run-parts /etc/update-motd.d`
#

### Scripts Info
#### ❯ /etc/update-motd.d/10-welcome
> Displays the raspberry model, welcome user message, current timestamp and kernel version.

#### ❯ /etc/update-motd.d/15-system
> Shows various details of the system. It includes temperature, memory, running processes and others. Few labels are trimmed down, like `Procs` for `Processes`, `Temp` for `Temperature`, `Last` for `Last Login`. You can use full labels according to your preference and arrange them accordingly.
