# Raspberry Pi OS

## Create a Bootable SD Card
I generally do this from a Mac, so that what I'm going to describe. It's simple, and I think it works pretty much the same regardless of what OS GUI you're in.

## First Boot, Log-in, and Password Update
Log in as user **pi**, giving **raspberry** as the password. Change that password immediately.

```shell
passwd
```

## Create a jclough Account
Install Zshell and Git first. Be sure to enable sudo for jclough.

```shell
sudo bash
apt-get --yes install git zsh
adduser -aG sudo --shell /bin/zsh jclough
exit
```

You'll be prompted for full name, some other stuff (which you can leave blank), and finally the password.

Set up jclough so that sudo works works without a password.

```shell
sudo bash
cd /etc/sudoers.d
echo "jclough ALL=(ALL) NOPASSWD: ALL" >010_jclough-nopasswd
chmod 440 010_jclough-nopasswd
exit
```

Configure some general system settings by running `sudo raspi-config`:
1. System Options / Wireless LAN - Enter the SSID and password of your WiFi access point here.
    
1. System Options / Hostname - Set the name of your new Raspberry Pi machine.
    
1. Interface Options / SSH - Enable incomming SSH connections
    
1. Performance Options / GPU Memory - Set GPU memory to the minimum if you're not going to be using this machine as a GUI workstation. Otherwise, consider GPU memory to the maximum.
    
1. Advanced Options / Expand Filesystem - Make the whole SD card (not just the first 8GB) available to the Raspberry Pi operating system.

1. Finish - It will prompt you to reboot, so let that happen.

## Setting up the jclough Account
Log in to the Raspberry Pi as jclough. Add an SSH public key (found under ~/.ssh in either id_rsa.pub or authorized_keys on already-configured hosts). You can also just copy and paste the one from the example below. That's my actual public SSH key. (Obviously, if you're not me, use your own or I'll be able to log in to your new system without a password.)

```shell
cd
mkdir .ssh
cd .ssh
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhVkY/qrO1wy3xjIMsqJzPFADT1XcbB4JIx8jKWCKQvQqTSrozdUeNAkY1upE49lzilc2pM3+WKcAI+Q7zSg1bj1CgFvS9+4pjfRdS/xMJsTXxsnZEDWqHyENxTrtRh3cfH3aZzwO/N6n8MF6ilNYAkq4H1NiWTzV4saI2Zq3/XDKIhqx7W6GJlIPUYvtFU8mJPJia6mz1XwDLZjHi8vIT5rbDbp+G8lHVsoT6G/KUaoX+GHhDJo3rCDjee0WpWEWLQnZivJ4L4lwSW+Ty23DxNlGYVNXCtKNx9F8e5wMHNZZ0hnBixukrXFDa9RiUwyx4Al2LNM/g74JCjFMXg/ID jeff@cloughcottage.com" >authorized_keys
chmod 640 authorized_keys
cd
```

## Install Some Handy Utility Scripts

As jclough ...

```shell
cd ~
mkdir src
cd src
git clone git@github.com:jeffclough/handy.git
cd handy
git submodule update --init --recursive
./install
```

That install command will create a ~/my directory and bin, include, and lib below that. ~/my/bin will contain lots of handy utility scripts, and ~/my/lib/python will contain lots of helpful Python modules. Our shell profile scripts for jclough rely on some of these, so we'll set those up next.

## Install My Profile Scripts

As jclough ...

```shell
cd ~/my
git clone git@github.com:jeffclough/profile.git
cd profile
./install
```

