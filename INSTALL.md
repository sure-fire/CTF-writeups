## Download and Install Kali

Download Kali from the Offensive Security site, as either a [virtual machine](https://www.offensive-security.com/kali-linux-vmware-virtualbox-image-download/) (recommended) or an [ISO image](https://www.kali.org/downloads/).

For most cases, I suggest [VMware Player](http://www.vmware.com/products/player/playerpro-evaluation.html), which unfortunately requires registering a throwaway account with VMware.  Alternatively, [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is a pretty solid open-source alternative.

## After Installation

After install, you should consider:
 - Change your Kali root password, if you haven't already:
   - Go to Applications (in the top-left)
   - Highlight **Favorites** (at the top), and carefully move the mouse cursor right and down to choose **Terminal**.
   - Now, run the `sudo passwd` command.
   - Enter your new password twice to make sure you didn't have any typos.  If you mess up, just re-run the `sudo passwd` command.
   - Type `exit` to close the Terminal.
 - Screen savers are irritating, especially in a VM:
   - Open the settings GUI (use the dropdown in the top-right, then the wrench button).
   - Open the **Power** settings, and set **Blank Screen** to **Never**.
 - Screen locks aren't off yet:
   - *If you don't have Settings open:* Open the settings GUI (use the dropdown in the top-right, then the wrench button).
   - *If you already have Settings open:* Use the left-arrow in the top-left of the Settings screen to go back to the main selection.
   - Click on **Privacy**.
   - Set **Screen Lock** to **Off**.
 - VMs think they should be using WiFi and Bluetooth.  I disagree:
   - *If you don't have Settings open:* Open the settings GUI (use the dropdown in the top-right, then the wrench button).
   - *If you already have Settings open:* Use the left-arrow in the top-left of the Settings screen to go back to the main selection.
   - Click on **Bluetooth**.
   - Move the slider knob to the left (at the top of the panel).  This enables Airplane Mode and disables Bluetooth

## Installing Tools

Here's a pastable of what I've got installed on my Kali machine.  (Note: The following instructions assume you are running as root.)

```
# GENERAL UBUNTU UPDATES
apt-get update
apt-get upgrade
apt-get dist-upgrade
apt-get autoremove

# UPDATE METASPLOIT FRAMEWORK -- this'll take a few minutes
msfupdate

# SUPPORT x86 BINARIES ON AN x64 PLATFORM
apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386

# (OPTIONAL) APT-FILE
# Used when some stupid CTF challenge has a dependency a library you need to find
#   root@kali:~/Desktop# apt-file search libssl.so.1.0.2
#   libssl1.0.2: /usr/lib/x86_64-linux-gnu/libssl.so.1.0.2
apt-get install apt-file
apt-get update

# (OPTIONAL) THEFUCK
# A handy Bash extension to fix a ludicrous number of brain-dead 2AM mistakes:
#   root@kali:~/Desktop/IOLI-crackme/bin-linux# apt-get search tcpreplay
#   E: Invalid operation search
#   root@kali:~/Desktop/IOLI-crackme/bin-linux# fuck
#   apt-cache search tcpreplay [enter/↑/↓/ctrl+c]
#   tcpreplay - Tool to replay saved tcpdump files at arbitrary speeds
apt update
apt install python3-dev python3-pip
-H pip3 install thefuck
echo 'eval "$(thefuck --alias)' >> ~/.bashrc

# A PLACE TO STORE DOWNLOADED GIT REPOS
mkdir ~/git

# DOWNLOAD AND INSTALL RADARE2 FROM GIT
cd ~/git
git clone https://github.com/radare/radare2.git
cd radare2
sys/install.sh

```
