Project to create a Ansible playbook based of the one at: https://github.com/tomgelbling/Securing-your-Raspberry-Pi-with-Ansible

The aim is to create a Pi Hole Raspberry Pi install with the relevant ufw rules via a playbook



# Securing your Raspberry Pi with Ansible

Ansible playbook to secure your Raspberry Pi.
Based on [Securing your Raspberry Pi](https://www.raspberrypi.org/documentation/configuration/security.md)  by the Raspberry Pi Foundation.

## What will be achieved by this Ansible playbook?

The playbook will perform configuration modifications in the following areas.

### Raspbian users
* Change the password of the pi user
* Create an alternative superuser
* Make sudo require a password

### Software package updates
* Establish Cronjob to update the openssh-server package on a daily basis

### SSH
* Set users that are allowed to use SSH
* Set users that are not allowed to use SSH
* Establish key-based authentication and disable all other authenticaton methods

### Firewall
* Install & enable ufw and fail2ban
* Set default and ssh firewall rules


## Prerequisites

The following software packages have to be installed on your local machine and the Raspberry Pi.

### On your local machine
* Python 2.6 or later
  * [Python BeginnersGuide](https://wiki.python.org/moin/BeginnersGuide/Download)


* Ansible
  * [What version to pick?](http://docs.ansible.com/ansible/latest/intro_installation.html#what-version-to-pick)
  * [Installing the Control Machine](http://docs.ansible.com/ansible/latest/intro_installation.html#installing-the-control-machine)


* For macOS and Linux only
  * [Install sshpass from source](https://gist.github.com/arunoda/7790979#installing-from-the-source)

### On your Raspberry Pi
* Raspbian
  * [Latest Raspbian images](https://www.raspberrypi.org/downloads/raspbian/)
  * [Installing Raspbian](https://www.raspberrypi.org/documentation/installation/installing-images/)


* Python 2.6 or later
  * [Python BeginnersGuide](https://wiki.python.org/moin/BeginnersGuide/Download)

* Enable ssh
  * from the console using sudo raspi-config
  * after the image has been created, mount the boot partition and create a file named "ssh" in it
```
# If the boot partition has been mounted at /media/jane/boot
touch /media/jane/boot/ssh
```

## Deployment

This chapter describes how to
* get a copy of the project.
* edit the config files.
* run the Ansible playbook to secure your Raspberry Pi.

### How to get a copy of the project

```
git clone https://github.com/timrwwatson/Ansible-Pi-Hole-Set-Up.git
cd Ansible-Pi-Hole-Set-Up/
```

### How to edit the config files

On your Ansible Host machine you may need to add/edit the `ansible.cfg` file

For me this was located in the `~/.ansible.cfg` and I added/changed:
```
[defaults]
host_key_checking = False
interpreter_python=auto_silent
```
The first allows ssh connections without the host being in the known host file and the latter silences warnings about the location of the python dir.

Add your Raspberry Pi IP address to the pi host group
```
echo "192.168.1.xyz" >> hosts
```

Add your public key to the authorized_keys files
```
cat ~/.ssh/id_rsa.pub >> roles/security/files/authorized_keys
```

Edit the variables file to set e.g. the custom password for the pi user, the name of the alternative user etc.
```
vim roles/security/vars/main.yaml
```

### How to clean up from previous runs

Remove the remote ssh key

```
ssh-keygen -f "/home/wayne/.ssh/known_hosts" -R "192.168.1.41"
```

### How to run the Ansible playbook to secure your Raspberry Pi

```
ansible-playbook -i hosts playbook.yaml
```

---

### Built With

* [Ansible 2.9.6](https://releases.ansible.com/ansible/)
* [Ubuntu 20.04 Ansible VM](https://ubuntu.com/download/desktop)
* [Raspberry Pi 3 Model B+](https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/)

* [Desktop Raspian OS VM 2021-01-11](https://www.raspberrypi.org/downloads/raspbian/)
`Note however that using the desktop/windows vm images I found that they weren't able to install Docker correctly`

---

## Authors
The base of this project comes from the work done below:

[Tom Gelbling](https://www.linkedin.com/in/tomgelbling/) - *Project initiator*

[timrwwatson](https://github.com/timrwwatson/Ansible-Pi-Hole-Set-Up) - *I forked his project*

See also the list of [contributors](https://github.com/tomgelbling/Securing-your-Raspberry-Pi-with-Ansible/graphs/contributors) who participated in this project.

## License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments

* **Raspberry Pi Foundation** - *Initial work* - [Securing your Raspberry Pi](https://www.raspberrypi.org/documentation/configuration/security.md)
* **PurpleBooth** - *A template to make good README.md* -  [README.md Template](https://gist.github.com/PurpleBooth/109311bb0361f32d87a2)
