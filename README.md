# ansible-k8s
Ansible playbook to install a kubernetes cluster on ubuntu 

## Preparation
Install basic dependencies for ansible
```bash
apt update && apt upgrade -y && apt autoremove -y && reboot
apt update
apt install software-properties-common -y
apt-add-repository --yes --update ppa:ansible/ansible
apt install python-argcomplete
vim /etc/ansible/hosts
```
Add the following configuration to your `/etc/asible/hosts` file.
```bash
[master]
apu03.home

[k8s]
apu[03:06].home

[nodes]
apu[04:06].home

[k8s:vars]
ansible_python_interpreter=/usr/bin/python3
```
Add an `ansible` user
```bash
useradd -m ansible
```
With group `ansible` and `sudo` allowance
```bash
usermod -a -G sudo ansible
```
Switch to the user
```bash
su - ansible
```
Start `bash`
```bash
bash
```
Create an ssh key
```bash
ssh-keygen
```
Give the user `ansible` the bash shell as default
```bash
usermod --shell /bin/bash ansible
```
Allow to sudo without password from the ansible user
```bash
echo "ansible ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible
sudo chmod 0440 /etc/sudoers.d/ansible
```
Copy the public ssh key of the ansible user
```bash
vim .ssh/id_rsa.pub
```
Add it on all nodes as authorized key
```bash
mkdir .ssh
vim .ssh/authorized_keys
```
