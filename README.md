# NagiosXI-Setup
The goal of this task was to add our nagios monitoring tool to our  We implemented nagious monitoring tool on our LAMP stack.

Task 1:
-Ssh into the NagiosXi: ssh -i "Python.pem" ubuntu@pubip

-Upgrade and update the Nagios ec2 instance: sudo apt update && sudo apt upgrade

Sss-keygen to create a public key and private key: ssh-keygen

Cd ~/.
Copy the contents of your public key 
And paste into authized keys

ssh -i id_rsa root@18.216.77.164


Task 2:

1)Change the security group of your lamp ec2 to allow all traffic

![alt text](C:\Users\Bishajit\Desktop\nagios-assignment\task2-1.png)



2)Then go back to the terminal that you ssh into the LAMP instance from your Nagios terminal in task 1.And then use this command to add a old ubuntu repo to the LAMP server
 
sudo echo "deb http://security.ubuntu.com/ubuntu bionic-security main" | sudo tee -a /etc/apt/sources.list.d/bionic.list


3)update your LAMP terminal now 
sudo apt update

4) Lastly for TASK 2 Make sure the lbssl exists.

Task 3:
1)In another terminal ssh back into Nagios instance

2) cd into ncpa_autoregister
cd  /usr/local/nagiosxi/scripts/automation/ansible/ncpa_autoregister

3)We will need to change directory to hosts.example top change the hosts
cd hosts.example

4)Once inside host.example change the host to LAMP private ip

5) Change the file name to hosts
sudo mv hosts.example hosts
6) Decrypt the secrets.yml file using this command 
sudo ansible-vault decrypt secrets.yml --ask-vault-pass
password:nagiosxi





7)Copy paste the public ip of the Nagios instance to the browser to access Nagios

8)Once inside Nagios click on user account that is right next to logout. (For me it was Nogiosadmin)


9) Now copy the api key

10)Go into secrets.yml, delete the default message and paste your key 
 sudo nano secrets.yml 

Task 4:
1)Go into ncpa file and change the ip to the nagios private ip
Sudo nano ncpa_install_and_register.yml

2) Inside ncpa file change the xi_ip to the private ip
Xi_ip:privateip

3)Now to instal ncpa use this command(also change your private key) : 
sudo ansible-playbook -i hosts --private-key=/home/ubuntu/.ssh/nagios
ncpa_install_and_register.yml -u root --ask-vault-pass

password:nagiosxi

4)Check nagios ui to see if the LAMP instance was added. (For me it was 1 now since I added its 2)


Task 5
1)Download stress-ng to the Lamp server 
sudo apt install stress-ng

2)Test one at a time. First I did the cpu, to do this use this command:
stress-ng --cpu 0 -t 1m
Then go to service status-->click cpu---> force an imediate check



3)Then I tested ram or memery
stress-ng --vm 2 --vm-bytes 80%
Then go to service status-->click ram---> force an imediate check


Crtl +c in terminal
4) Lastly test the disk
stress-ng --hdd 1 --hdd-bytes 100%




