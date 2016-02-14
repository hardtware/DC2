
# DC2 Setup
1. Assemble the DC2
2. Find the DC2 on your network
3. Configure the DC2

## 1. DC2 Assembly

X. Connect your DC2 to power and your LAN. You should see lights show up. If you have more than one DC2, setup one at a time.

## 2. Find the DC2 on your network 

2.1. Make sure you have Bonjour / mDNS

When you connect your DC2 to your network, it will be using DHCP to get an IP address. To work with the DC2 from other machines, we will need to find out where it is. The DC2 will default to having the '''dc2.local''' hostname which will be broadcast with mDNS aka Bonjour. If you have a Mac or Windows 10 machine, you are good to go. If you have an earlier version of Windows, you will need to install [Bonjour Services](https://support.apple.com/kb/DL999?viewlocale=en_US&locale=en_US), If you are on a linux box, you can install [Avahi](https://wiki.archlinux.org/index.php/Avahi).

2.2 If you have more than one DC2, then set them up one at a time so you can change the host names to differentiate between them.

2.3. Check you can connect to your DC2

Open up http://DC2.local:8765 with your browser -- this should return the configuration information from your DC2. 

## 3. Configure the DC2

3.1 Check that you can SSH into your DC2

`$ ssh dc2@dc2.local`

enter `ubuntu` when prompted for the password

3.2 Change your password

`$ passwd`

Enter `ubuntu` for the old password, and then enter your new password

3.3 If you have more than one DC2, change your hostname by replacing `dc2` to a new name unique on your network in the 

`/etc/hostname` and `/etc/hosts` files

with the editor of your choice. Then run `sudo service hostname restart`

NOTE: Everwhere you see `dc2` below, change that to the new hostname you have given your DC2.

3.4 If you are not in the `America/Los Angeles` timezone you can change it with `sudo dpkg-reconfigure tzdata`

3.5 If you want to change the locale, follow directions at https://help.ubuntu.com/community/Locale

3.6 If you don't have an ssh key, create one *link to directions*

3.7 Copy your SSH public key to your DC2. If your SSH public key is at `~/.ssh/id_rsa.pub`

`ssh dc2@dc2.local "cat >> .ssh/authorized_keys" < ~/.ssh/id_rsa.pub`

4. Setup Docker

4.1 Make sure you have the [Docker Toolbox](https://www.docker.com/products/docker-toolbox) intalled on your computer.

4.2 Find the IP address of your DC2

`ping dc2.local`

Substitute `a.b.c.d` in the following commands with your DC2's IP address

4.3 Check that you can use SSH against your IP address

`ssh dc2@a.b.c.d`

4.4 Setup the DC2 as a generic machine

`$ docker-machine create --driver generic --generic-ssh-user=dc2 --generic-ip-address=a.b.c.d dc2`

This will create add your DC2 as a machine to run containers in.

4.5 Run the hello-world container

`$ docker xxxx`








