# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.


### How to install ansible on AWS ec2 instances
to install ansible on Amazon Linux or to setup ansible lab in aws we need two or three ec2 instances. one is ansible master ec2 instance remaining ec2 instances are clients. in the master ec2 instance only we will install ansible.

1. Launch three or two  ubuntu 16.04 instances. Give Name one ubuntu ec2 instances as ansible-master and give remaining ec2 instances names as client1, clinet2 in both ansible master and clients  
2. set security groups open ssh port no  22 from anywhere

###Install python in ansible master and clients instances

So we have to install python in all master and client machines.

To install python execute below commands as root user

```
sudo -i
apt-get install python-minimal
apt-get install python3
```

check python version with
```
python --version

```

### Installing ansible in ansible master instance
Run below commands as root user

```
sudo -i

apt-get update

apt-get install software-properties-common

apt-add-repository ppa:ansible/ansible

apt-get update

apt-get install ansible
```

check ansible with
```
ansible --version

```
Output looks like:
```
ansible 2.6.3
config file = /etc/ansible/ansible.cfg
configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
ansible python module location = /usr/lib/python2.7/dist-packages/ansible
executable location = /usr/bin/ansible
python version = 2.7.12 (default, Dec 4 2017, 14:50:18) [GCC 5.4.0 20160609]

```

###Establish ssh connection between ansible master and clients
To establish a connection between master and clients we have to generate the id_rsa.pub key in master and paste this key in authorized_keys file of client machines. This file exists in the .ssh directory. So if the .ssh directory has not existed in client ec2 instances, We have to create the .ssh directory and inside that, we have to create the authorized_keys file.

###Generating id_rsa.pub  public key in ansible master instance
in master, ec2 instance execute below commands

```
sudo –i

ssh-keygen -t rsa

```

It will create the id_rsa.pub key in the .ssh directory

```
cd .ssh

ls

id_rsa  id_rsa.pub known_hosts

cat id_rsa.pub

```
Copy this id_rsa.pub key


###In All Client Ec2 Instances

```
Sudo -i

cd .ssh

ls
```

Here you can see the authorized_keys file.

open the file in terminal using nano editor or anyeditor

like:
``` nano authorized_keys
```
and paste the id_rsa.pub key in this file Paste id_rsa.pub  key of the master here
now we have shared ssh keys between master and clients


###Adding clients to ansible master
To add clients to ansible master machine, we need to add all IP’s of clients in master machine /etc/ansible/hosts file

now go to ansible master machine

###Ansible AWS Inventory

```
cd /etc/ansible

nano hosts

```

and add like bellow

```
[web]
173.72.39.105
```
here 173.72.39.105 is private IP of the client1 machine
here you can mention all client machines private IP’s

now check the nodes using the command bellow

``` 
ansible -m ping all

```

it will give you a success message with green color.

the first time it will ask are you sure you want to continue connecting yes/no

write yes and click on enter

you can see the output in green color.

now we have successfully configured ansible practice lab in aws.

now you can run your playbooks and roles in clients.
