# IanseoBox



IanseoBox is a Ubuntu ARM-64 Architecture Linux Image built by DHUNY Riyad for Raspberry Pi with a Ianseo inside.IanseoBox uses  the Wi-Fi of the Raspberry Pi to create an Access Point. 

When deployed, IanseoBox makes it easy for archers to use with [Ianseo Scorekeeper](https://play.google.com/store/apps/details?id=net.ianseo.scorekeeperlite).


## The IanseoBox Documentation

Visit the [Ianseo web site](http://ianseo.net) for more information about ianseo features or any question about the usage of a Ianseo.

If you just want to use a IanseoBox, a [prepared disk image][download] of the latest released version is [available for downloading][download] and using out of the box on your Raspberry Pi 3A, 3B, 3B+ or 4B. Follow the instructions on the [ianseoBox web site][website].

### Asking Support Questions

Please don't use the GitHub issue tracker to ask questions.

## Building the IanseoBox disk image from scratch

> If you just want to use a IanseoBox, __you don't need__ to build the IanseoBox disk image yourself. Just [download the IanseoBox image][download] and follow the instructions on the [IanseoBox web site][website]. 

To build a IanseoBox from scratch with this script, you need a Raspberry Pi 3B, 3B+ or 4B.

1. Clone [Ubuntu ARM-64](https://ubuntu.com/download/raspberry-pi) on your microSD card.
1. Insert the microSD card into your Raspberry Pi.
1. Connect your Raspberry Pi to your Ethernet network and boot it.
1. Login with username `ubuntu` password `ubuntu`. Set new password to `Ianseobox4$`.
1. Create a `ssh.txt` file on the `boot` partition, e.g. using 
        
		sudo touch /boot/ssh.txt
		
1. [Install Ansible](https://docs.ansible.com/intro_installation.html) on your computer.
1. [Install `sshpass`](https://gist.github.com/arunoda/7790979) to enable passing SSH password to the Raspberry Pi. On macOS, use e.g. `brew tap esolitos/ipa; brew install sshpass`.
1. [Clone this repository][git] to your local drive. A quick way is:
	
	sudo apt install sshpass ansible unzip -y
	
1. A server restart may be required for apt update & upgrade to remove locks on tmp files. Perform 
	
	sudo apt update && sudo apt upgrade -y
	
1. Create a `keys` directory in the repository folder and copy your public key into it, under the name `id_rsa.pub`.
1. A quick way  for above is: 

	ssh-keygen -t rsa

	with blanks as defaults. 
	
	sudo cp /home/ubuntu/.ssh/id_rsa.pub /home/ubuntu/IanseoBox/keys/
	
1. Get the IP address of your Raspberry Pi and change it in the `hosts.yml` file. Do not change anything else, unless you know what you're doing. You're on your own.
1. Create a config.yml with `cp default.config.yml config.yml`.
1. Run   `ansible-playbook ubuntu.yml`   from the repository folder to prepare the OS.
1. Wait 1 -2 mins, playbook will change username from ubuntu to moodlebox, script will exit with failure.
1. Playbook will stop after username change, with an error similar to `Unable to create local directories(/home/ubuntu/.ansible/cp’`...
1. Reboot server with sudo init 6 and login with username ianseobox password Ianseobox4$
1. Run `ansible-playbook ianseobox.yml` from the repository folder.
1. Wait 15–50 minutes, depending on your Raspberry Pi model, SD card speed and Internet bandwidth. You're done.
1. If the script fails at adminer, run the ianseobox.yml script again.
1. On first boot run passwd and change the default password. Delete the Install folder from the root directory.
1. Set your default Country Code by typing sudo nano /etc/hostapd/hostapd.conf.

### Overriding defaults

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can change the MoodleBox main credentials and the timezone with something like:

    moodlebox_username: 'myusername'
    moodlebox_password: 'secret'
    moodlebox_timezone: 'Indian/Mauritius'

Any variable can be overridden in `config.yml`; see the file `default.config.yml` for a list of available variables.

## Availability


The Ianseobox code is available at [https://github.com/dhuny/ianseobox][git].

### Release notes

See Original Ianseobox [Release notes](https://github.com/dhuny/ianseobox/blob/master/CHANGELOG.md).


## Credits

Credits to: Nicolas Martignoni, Daniel Méthot and Christian Westphal  for their original work on Moodlebox.  
Credits to  Ardingo Scarzella, Andrea Gabard, Christian Deligant, Matteo Pisani and Marco Carpignano for this IANSEO software.


- To Nicolas Martignoni, the maintainer of Moodlebox. IanseoBox concept originates from moodlebox and Martignoni's work.
- To Daniel Méthot, for the [idea of a MoodleBox](https://moodle.org/mod/forum/discuss.php?d=278493)
- To Christian Westphal, for the [first POC](https://moodle.org/mod/forum/discuss.php?d=331170) of a MoodleBox
- To the [Raspberry Pi Foundation](https://www.raspberrypi.org/), for a splendid small computer
- To Ardingo Scarzella, Andrea Gabard, Christian Deligant, Matteo Pisani and Marco Carpignano, for giving us IANSEO.

## License

The original Ianseobox copyright info is as follow: 

Copyright © 2020 onwards, DHUNY Riyad, riyad@dhuny.org.

All contributions to this repository are licensed under AGPLv3 or any later version.

IanseoBox doesn't require a CLA (Contributor License Agreement). The copyright belongs to all the individual contributors. Therefore we recommend that every contributor adds following line to the header of a file, if they
changed it substantially:

```
@copyright Copyright © <year>, <your name> (<your email address>)
```

  [website]: https://ianseobox.dhuny.org
  [download]: https://ianseobox.dhuny.org/ianseobox.zip
  [git]: https://github.com/dhuny/ianseobox
