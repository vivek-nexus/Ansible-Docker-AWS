# What is this?

This is **beginner's dairy** of getting started with deploying a **Docker Container, serving a simple web page, with Ansible.** Know more about [What is Docker](https://docker-curriculum.com/#what-is-docker-) and [What is Ansible](https://www.ansible.com/overview/how-ansible-works)?


# How does it work?

This project consists of three components:

 1. A static web page that is to be displayed
 2. A Docker container serving this webpage, using Ngnix on the server
 3. Ansible script that deploys the Docker container on to a server machine and gets it up and running

Ansible script will be run from a local machine/another server which will connect, install required packages on the actual server, download the docker container image, deploy and launch the container.

# Pre-Requisites
## On the local Machine/Control Server

 - Ansible installed. If not, use below command to install:
`sudo apt update && sudo apt install ansible`

## On the Actual Server
Although any remote server can be used, we will be using AWS EC2 Ubuntu Linux 18.04 Server for this project. You can get access to AWS EC2 with Amazon Free Tier plan.

The EC2 instance needs to be launched with the following configuration/settings:

 1. Ubuntu 18.04 Server Image for the VM
 2. Default Free Tier CPU & Storage Specs
 3. Security Groups with incoming traffic open to SSH on port 22 and HTTP on port 8888, as shown below:
 
 | Type | Protocol | Port Range | Source | 
 | -- | -- | -- | -- |
 | Custom TCP Rule | TCP | 8888 | 0.0.0.0/0 |
 | SSH | TCP | 22 | 0.0.0.0/0 |
 
 4. SSH access to EC2 instance with a private-public key set up. Add your private key file to SSH with `ssh-add /path-to-private-key/AWSkey.pem`

# Usage

 - Clone this repository onto a folder on local machine/control server
 - Grab your EC2 instance IP. Navigate your terminal to the cloned directory, edit the hosts file with your favourite text editor and replace the server IP with your EC2 instance IP. 
 `$ nano hosts`
 - Run the Ansible Playbook **MainScript.yml**: `$ ansible-playbook MainScript.yml -i hosts`. Each step might take a few minutes depending on the bandwidth and server resources.
 - Finally you would get a message displaying PLAY RECAP and successful run of the Playbook.
 - Access the site with your EC2 instance IP on port 8888, from any web browser, like this: `12.13.14.15:8888`
 - There we get the site, running on a single docker container with the following message!
 `Hi from Docker Container! This page is being served from a docker container running Nginx, hosted on AWS EC2 Instance.`

# Trouble Shooting

## Host refused connection

 - Check if your server is accessible over SSH with `ssh ubuntu@PublicDNS`. If not check inbound traffic rules in the EC2 security groups or issues with the private key file permissions.

## Ansible failed to connect

 - Check if the target server IP is correctly mentioned in the hosts file
 - Check  SSH access as above
 - Run `$ ansible-playbook AnsiblePing.yml -i hosts` to check if the connection attempt returns OK message. If not, SSH access needs fixing.
 - Default user of the target server. If the user to be used is other than `ubuntu`, change the `remote_user` value in `MainScript.yml` file.

## Cloned directory already exists

 - Remove cloned directory on the EC2 Server home folder, by logging in via SSH

# Resources & References

 - https://docker-curriculum.com/
 -  https://www.codementor.io/@mamytianarakotomalala/how-to-deploy-docker-container-with-ansible-on-debian-8-mavm48kw0

 - https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-ansible-on-ubuntu-18-04
 - https://www.digitalocean.com/community/tutorials/how-to-use-ansible-to-install-and-set-up-docker-on-ubuntu-18-04#how-to-use-this-playbook
 - https://unix.stackexchange.com/questions/363048/unable-to-locate-package-docker-ce-on-a-64bit-ubuntu
 
