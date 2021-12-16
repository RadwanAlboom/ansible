# Ansible Task

## Install Ansible

```
sudo yum install epel-release -y
sudo yum install ansible -y
```

## Setup VMs Cluster (Controller, Server, Client Machines)

-   Edit host name and hosts lists on each machine.

```
sudo vi /etc/hostname
sudo /etc/hosts
```

- hostname on controller machine after editing hostname file
```
ansiblecontroller
```
- hosts list on controller after editing hosts file
```
127.0.0.1   localhost ansiblecontroller
::1         localhost ansiblecontroller
192.168.43.62  servermachine
192.168.43.78  clientmachine
192.168.43.167  controllermachine
```

-   Generate ssh key on controller machine

```
ssh-keygen -t rsa
cd .ssh/
```

-   Copy ssh public key on target machines (server machine, client machine)

```
ssh-copy-id -i ~/.ssh/id_rsa.pub osboxes@servermachine
ssh-copy-id -i ~/.ssh/id_rsa.pub osboxes@clientmachine
```

-   Then answer for completion of the process by yes.
-   After that, he will ask for the password of the target machines.
-   After completing the process, you can now ssh to the target machines without providing any password.

```
ssh servermachine
ssh clientmachine
```

-   Install and configure apache server on local machine.

```
sudo yum install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

-   Configure firewall on local machine.

```
sudo systemctl status firewalld
sudo systemctl start  firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

-   Disable SElinux.

```
vi /etc/selinux/config
SELINUX=disabled
```

-   Restart the machine.

```
sudo shutdown -r now
```

## Run Ansible Playbook

-   First clone repository for the above ansible code.

```
git clone https://github.com/RadwanAlboom/ansible.git
```

-   Run asible palybook.

```
ansible-playbook playbook.yml -i inventory.txt
```

## Access Zabbix Server

-   To open zabbix server on browser type ifconfig in the terminal to get ip address of the host machine.

```
ifconfig
```

-   After obtaining ip address, type on your browser:

```
http://x.x.x.x/zabbix
```

-   After that the zabbix fronten GUI will appear.
-   Then provide your database info to establish connection with it.
-   After successfully installing zabbix frontend, you can enter to the zabbix dashbard by entering Admin as user name, zabbix as password.

## Monotring client machine

-   First create a new host (machine to be monitored) on zabbix frontend.

![Create Host](https://videos-storing.s3.ap-south-1.amazonaws.com/linux/CreateNewHost.PNG)

-   Choose template.

![Template](https://videos-storing.s3.ap-south-1.amazonaws.com/linux/template.PNG)

-   After setup new host, go to monitoring tab, then choose graphs.
-   Choose the machine you want to monitor as host
-   Then choose the graph that you want to see.

## CPU Jumps Graph

![CPU](https://videos-storing.s3.ap-south-1.amazonaws.com/linux/cpuZabbix.PNG)

## Memory Usage Graph

![Memory](https://videos-storing.s3.ap-south-1.amazonaws.com/linux/memZabbix.PNG)
