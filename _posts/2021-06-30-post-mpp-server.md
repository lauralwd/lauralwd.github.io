---
title: "Post: Our humble lab server, what's inside and how to use it."
excerpt_separator: "<!--more-->"
categories:
  - Blog
tags:
  - blog
  - technology
  - Linux
  - Bash
toc: true
---

==== post in progress ====

# A short history
In this post, I aim to inform anyone interested of the server I made for the _Azolla_ lab and the molecular plant physiology group.
When we got started with NGS data and bioinformatics, I worked on a default university workstation with Ubuntu Linux installed.
However, I quickly realised that such a default workstation was not sufficient to do our analyses with sufficient speed.
To meet my computing needs, and those of my colleagues who would soon follow doing analyses on their own NGS data, we set out in two paths.
First, we got access to a local computing cluster.
The learning curve to use such a cluster effectivelly is quite steep. 
Whilst this was sufficient to get my analyses running at the time, it was by no means a friendly learning environment for students and other 'Linux beginners' in our lab.
Therefore we set out to create our own local hubmle Linux server, sufficiently fast to run big analyses locally, and sufficiently accessible for beginners to learn on.

# Hardware
Being novices ourselves, we secured some budget and started compiling parts.
Only fora for computer building prooved a valueable resource to double check our planned purchases.
In the end, we bought a stand-alone case with a 6-core multithreading xeon processor.
The machine has a 1TB ssd for the opperating system and user home folders.
To store bigger files securely and quickly, we have a 12TB raid10 array configured with btrfs.
In practice the btrfs-10 raid array reads nearly as quickly as the SSD, hence the latter is only usefull for rapid writing tasks.
Secondly, a btrfs array allows instant copy-on-write snapshotting of a subvolume or directory structure.
This is nice for a local "back-up" without actually taking the double space. 
Additionally, we put in a small GPU so we could use a GUI, but not for GPU intensive calculations per say.

# Software
Being a fan of the Ubuntu opperating system, and since this computer will be used by many beginners, we opted for a flavour of Ubuntu called 'BioLinux'.
This distribution has many tools and programmes installed already.
Since the introduction of conda, I don't think this I would strongly recommend this anymore, but I see now argument against using BioLinux over regular Ubuntu either.

## conda
I rememeber taking a week to install all software for my first metagenomics experiments as a MSc student. 
Worse even, I took more than a week to install all software for plant genome annotation via makerp, and gave up because I couldn't get it to run.
Conda (or specifically miniconda3) takes a lot of that pain away!
I highly recommend anyone interested in bioinformatics to get acquainted with miniconda right away. 
Software management has never been so easy.
Sure it has it problems, but doing this by hand, solving dependencies and non-matching software versions... that's way worse.

In a typical setup, each user installs miniconda3 in their own 'home directory'. 
Also in a typical setup, the disk space that this software takes is quite big.
Using the same server with 5-10 people, I figured having 5-10 miniconda installs running is a waste of disk space.
That's why I chose the following configuration. 
I, as administrator and only advanced user have my own miniconda3 installation in my home directory.
In here, I can experiment and play with conda environments.
For all other users, I have a second miniconda3 installation in `/opt/miniconda/miniconda3`.
This miniconda install is owned by an unprevileged user `mpp` and all other users (students, PhDs, etc) are added to the 'mpp' group.
A new user, doesn't have conda installed in their user environment by default, hence a new user should run this command first: `/opt/miniconda/miniconda3/bin/conda init` and then log out, and log in.
The entire directory should be writable by this group, and if any problems arise a quick run of `sudo chmod g+w -R /opt/miniconda/` solves this.
This way, any beginners only have to know how to get a conda environment running, and use the tools.
When they are more familiar, they can make their own conda environments if they'd like to. 

## jupyterhub
Besides conda, the most valuable software tool used by our lab is a JupyterHub server.
Jupyter notebooks are a great way by which we can keep track of both our code, as well as our intents and interpretations.
Too often have the latter two been lost by students who struggle with writing the code. 
Using jupyter notebooks, I try to make it as easy as possible for students to create a labjournal with code, and do propper science.

To make it even easier, I have jupyterhub installed. 
Our server serves a https jupyter webpage on which users can login with their own username and password.
This way, a student who is connected to our local network can reach all our bioinformatics tools via the webbrowser without ever having to install any software on their own computer.

Although propperly firewalled, and secured, I consider this a relativelly vulnerable service. 
Hence, every user has to be given access explicitly via a config on the server owned by the root user. 
See the 'adding new users' section for the details.

## R server
Additionally, I also have an R-studio-server running. 
This works pretty much the same as the jupyterhub server, a webpage is served at the servers address and a specific port nr.
Users can login with the same username and password as for any service on this server, and run R over a webpage.

# Maintenance

# How to use

## Adding new users
Adding new users can be done in the termina, or via ubuntu settings. 
You will need to be the admin user with the appropriate password.
Let's say we're adding a new user called 'laura' then the command would look like so:
In the terminal type
```
 sudo useradd -d /home/laura -m -s /bin/zsh -p lauras_standard_password laura
```
Naturally replace the password by something secure.
Also, observe the space just before the sudo command. 
This is not a typo, but prevents this command from being recorded in the terminal history!

Because of how I organised stuff, we'll need to add our new user to the 'mpp' group.
This way, the new user can access files made by the shared mpp user.
```
sudo usermod -G mpp -a laura
```

To prevent weird issues, I usually go to the server after this step and manually log in on the physical interface.
This seems to be required before being able to use jupyterhub.

### Adding new users to JupyterHub
Adding users to jupyter hub is a bit more cumbersome.
Jupyterhub uses the users own environment to run, so we need to make sure the IPykernel is installed and configured with an additional bash kernel.
If that sounds like garbage to you, just follow the instructions:
Login as the new user, either on the physical computer or via your own terminal
```
su laura
```
Now we'll install the ipykernel and the bash-kernel with the following three commands.
```
pip3 install ipykernel
pip3 install bash_kernel
python3  -m  bash_kernel.install
```

Second, we need to explicitly allow the new user to login via Jupyterhub via the main config file.
To edit this config, do:

```sudo nano /opt/jupyterhub/jupyterhub_config.py```

Then find the line `c.Authenticator.whitelist = {'user1','mpp'}`. 
Add any user to this list, separated by a comma and with the user name between single quotes.
Also, this is as good a time as any to remove any users from this list who should not have access anymore.
Again, we're propperly firewalled but any user/password combo is a weakpoint.

Finally, the Jupyterhub service should be restarted
```
sudo systemctl reload-or-restart jupyterhub.service
```

### Allowing a user to login via ssh
The final mode of loging in, and perhaps the most "dangerous" is via ssh.
This especially comes in handy when connecting to the server from home.
We have to be the admin user, and edit the `sshd_config` file
```
sudo nano /etc/ssh/sshd_config
```

Find the line `AllowUsers sjors henriette peter` etcetera.
Now add the new user to the end of this line, separated by a space from the other names.
This is also a perfect moment to check if anyone should be removed from this list.
ssh access is very powerfull and should be limited to need-to-have access. 

Finally, the service should be restarted
``` 
sudo systemctl reload-or-restart ssh.service
```

More on how to reach the server via ssh in the 'access' section

## Shared resource

check computer load and adjust accordingly

`htop`

`uptime`

Computer load should be no higher than 12, the number of processors available.

## access

### physical
### local
### ssh


## Shared resource

### making user

local access
jupyterhub access
ssh access


### archiving user

### back-up











