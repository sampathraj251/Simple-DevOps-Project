# Ansible Installation

Ansible is an open-source automation platform. It is very, very simple to set up and yet powerful. Ansible can help you with configuration management, application deployment, task automation.

### Pre-requisites

1. An AWS EC2 instance (on Control node)

### Installation steps:
#### on Amazon EC2 instance

1. Install python and python-pip
   ```sh
   yum install python
   yum install python-pip
   ```
1. Install ansible using pip check for version
    ```sh
    pip install ansible
   ansible --version
   ```
  *check ansbile config file path should be /etc/ansible/ansbile.cfg  .In latest version it is not available.if it is showing none withe above command then create it.
    ```sh
    mkdir /etc/ansible
    ```
1. Create a user called ansadmin (on Control node and Managed host)  
   ```sh
   useradd ansadmin
   passwd ansadmin
   ```
1. Below command grant sudo access to ansadmin user. But we strongly recommended using "visudo" command if you are aware vi or nano editor.  (on Control node and Managed host)
   ```sh
     visudo
     ```
     goto last line by pressing shift+g add   ansadmin ALL=(ALL)       NOPASSWD: ALL   then save it by pressing shift + : then wq
 
   ```sh
   echo "ansadmin ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers
   ```

   ```
Note:By default user authentication is not enabled in AWS EC2 instances so we should edit password authentication as yes in the sshd_config file available in /etc/ssh/sshd_config. 
Note2:reload sshd after modification..by using command `service reload sshd` and check ok..otherwise use `systemctl enable sshd` `systemctl start sshd` `systemctl status sshd`
```sh
vi /etc/ssh/sshd_config
```
1. Log in as a ansadmin user on master and generate ssh key (on Control node)
   ```sh 
   sudo su - ansadmin
   ssh-keygen
   ```
1. Copy keys onto all ansible managed hosts (on Control node)
   ```sh 
   ssh-copy-id ansadmin@<target-server>
   ```
1. Ansible server used to create images and store on docker registry. Hence install docker, start docker services and add ansadmin to the docker group. 
   ```sh
   yum install docker
   
   # start docker services 
   service docker start
   service docker start 
   
   # add user to docker group 
   usermod -aG docker ansadmin   


1. Create a directory /etc/ansible and create an inventory file called "hosts" add control node and managed hosts IP addresses to it. 
 
### Validation test

   
1. Run ansible command as ansadmin user it should be successful (Master)
   ```sh 
   ansible all -m ping
   ```
