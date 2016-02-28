
# DC2 Setup
1. Assemble the DC2
2. Find the DC2 on your network
3. Configure the DC2
4. Add the DC2 as a docker machine

## 1. DC2 Assembly

1.1 Connect the MinnowBoard Turbot to the faceplate. This will let you work with your MinnowBoard Turbot and minimize risk of shorting a component.

1.1.1 Get a #1 or #2 philips head screwdriver and screw the 2 XXX screws each in a few turns. You want the screws to go in from the left side if you are looking at the front of the faceplate. We are doing this to loosen the threads as the faceplate threads are 3D printed.

1.1.2 Take out the screws, set the MinnowBoard Turbot on the faceplate, and screw the screws back in. Tighten enough that the MinnowBoard Turbot does not wobble around on the faceplate. If you have a SilverJaw Lure, you can first mount it to the MinnowBoard Turbot placing the spacers between the two boards, then screw the screws into the faceplate, and then screw the screws into the standoff at the other end of the boards.

1.2 Conenct your DC2 to the world and configure it before installing any addons or installing in container. Let's make sure it works when you can easily see all the lights. If you have more than one DC2, let's do one at a time.

1.2.1 Plug the Ventura Ultra USB drive into the MinnowBoard Turbot's USB 3.0 connector. This is the one that is blue and closest to the board.

1.2.2 If you have one, plug in an HDMI cable so you can see the MinnowBoard Turbot boot up.

1.2.3 Plug in an ethernet cable connected to the same network as the computer you are going to use to setup your DC2

1.2.4 Plug in your power supply to the wall, and plug in the 5V power. Both blue lights on the MinnowBoard Turbot should light up. The green light on the ethernet adapter should flash with activity. The green light on the Ventura Ultra USB drive should go on and start to flash.

1.3 Do steps 2 - 4 below, then come back to finish assembly. Disconnect power, ethernet and HDMI before continuing. 

1.4 If you have an OLED you need to connect it before installing the MinnowBoard Turbot into the container.

1.4.1 Take the supplied allen wrench and mount the OLED with the 4 XXX screws.

1.4.2 Connect the cable from the USB 2.0 connector to the OLED. Make sure you coil the cable as indicated and connect per the diagram.

1.5 Slide the assembled MinnoBoard Turbot into the container. It should click when fully installed.

1.6 Reconnect ethernet and power and enjoy your DC2

## 2. Find the DC2 on your network 

2.1. Make sure you have Bonjour / mDNS

When you connect your DC2 to your network, it will be using DHCP to get an IP address. To work with the DC2 from other machines, we will need to find out where it is. The DC2 will default to having the '''dc2.local''' hostname which will be broadcast with mDNS aka Bonjour. If you have a Mac or Windows 10 machine, you are good to go. If you have an earlier version of Windows, you will need to install [Bonjour Services](https://support.apple.com/kb/DL999?viewlocale=en_US&locale=en_US), If you are on a linux box, you can install [Avahi](https://wiki.archlinux.org/index.php/Avahi).

2.2 If you have more than one DC2, then set them up one at a time so you can change the host names to differentiate between them.

2.3. Check you can connect to your DC2

Open up http://DC2.local:8765 with your browser -- this should return the configuration information from your DC2. 
It can take XX minutes for the DC2 to boot, and XX for the dc2-node script to run and Bonjour to have completed its broadcast. You should see something like this:

```
{
"latestVersion": "1.0.4",
"version": "1.0.4",
"hostname": "dc2",
"ip": "a.b.c.d",
"MAC": "ff:ff:ff:ff:ff:ff",
"callHomeResponse": 200
}
```

where `a.b.c.d` is the IP address of your DC2 and `ff:ff:ff:ff:ff:ff` is the MAC address of your DC2 ethernet port.

## 3. Configure the DC2

3.1 Check that you can SSH into your DC2. `jack` is the preconfifured username

`$ ssh jack@dc2.local`

enter `hardtware` when prompted for the password

3.2 Change your password

`$ passwd`

Enter `hardtware` for the old password, and then enter your new password

3.3 If you don't have an ssh key, create one *link to directions*

3.4 Copy your SSH public key to your DC2.

First `exit` your SSH session with your DC2. 

If your SSH public key is at `~/.ssh/id_rsa.pub` then from your machine:

`ssh jack@dc2.local "cat >> .ssh/authorized_keys" < ~/.ssh/id_rsa.pub`

enter your new password when prompted

You should now be able to `ssh jack@dc2.local` and not be prompted for a password.

3.5 If you are not in the `America/Los Angeles` timezone you can change it with `sudo dpkg-reconfigure tzdata`

3.6 If you want to change the locale, follow directions at https://help.ubuntu.com/community/Locale

3.7 If you have more than one DC2, change your hostname by replacing `dc2` to a new hostname unique on your network (eg. dc2a, dc2b, dc2c) in the 

`/etc/hostname` and `/etc/hosts` files

with the editor of your choice. Then run `sudo service hostname restart`
*need to also restart the avahi service -- NOT SURE HOW TO DO THIS*

NOTE: Everwhere you see `dc2` below, change that to the new hostname you have given your DC2.

## 4. Setup DC2 as a Docker Machine

4.1 Make sure you have the [Docker Toolbox](https://www.docker.com/products/docker-toolbox) intalled on your computer.

4.2 Find the IP address of your DC2

`ping dc2.local`

Substitute `a.b.c.d` in the following commands with your DC2's IP address

4.3 Check that you can use SSH against your IP address

`ssh dc2@a.b.c.d`

4.4 Setup the DC2 as a generic machine

`docker-machine create --driver generic --generic-ssh-user=dc2 --generic-ip-address=a.b.c.d dc2`

This command takes a while as it will be updating the DC2. It will create add your DC2 as a machine to run containers in.

4.5 Setup the DC2 as your docker machine

`docker-machine env dc2` will show you how to setup the envirnment to run commands against your DC2

4.5 Run the hello-world container

`docker run hello-world`

4.6 Success! Disconnect power, ethernet and HTMI (if connected) and finish assembly in 1.4

