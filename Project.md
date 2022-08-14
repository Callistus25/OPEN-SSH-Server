Open SSH Server
- Setup and Ubuntu linux server on your local system.

![alt](./Images/Ubuntu%20setup.JPG)

- Clone the Ubuntu server and rename to Client so you can have double OS setup with one as Server while the other as Client.
![alt](./Images/Client%20Server.JPG)

- On both OS, go to settings, Network, configure adapter 1 to NAT, and adpater 2 to bridge adapter. On both server and client, set adapter 2 brigde adapter to the LAN ethernet.

![alt](./Images/client%20bridge.JPG)

![alt](./Images/server%20bridge.JPG)

- Launch both servers and open both terminal. Notice that both servers bears the same identity because the are running same configuration. For identification purposes, need to rename the client server to "client"
run the below command:
```
sudo vi /etc/hostname
```
![alt](./Images/cally%20client.JPG)

Reboot the OS afterwards with command 

```
sudo reboot now
```
- Next is generate the keypair as SSH uses assymetric cypher for encryption. chekc the location where SSH stores the keypair using the below command.
```
ls -latr /home/<systemname>/.ssh
```
![alt](./Images/Ls%20-latr.JPG)

- Generate the keypair using the ssh keypair generating command below, and select the storage location
```
ssh-keygen
```
after generating the keypair, run the command to see the keypairs.
```
ls -latr /home/<systemname>/.ssh
```
![alt](./Images/SSH%20keypair.JPG)

In the keypair list, the id_rsa is the private key while the id_rsa.pub is the public key which can be shared.

- Next is to ensure that the open SSH is running on the "server" OS by installing it using the command below:
```
sudo apt install Openssh-server
```
![alt](./Images/sudo%20install%20Open%20Ssh%20server.JPG)

- We will need to get the IP address of the server using command below. Note: both servers will have different IP addresses. Incase both server on "enops8" network IP appeard same, you can allocate a manual IP to separate both routes. Besides, you dont need to be connected to the internet to achieve this.

```
Ifconfig
```
Client IP address ![alt](./Images/client%20IP%20address.JPG)

Server IP address ![alt](./Images/Server%20IP%20address.JPG)

- To ensure public access is granted to the client by the server, need to find a way to transfer the public key "id.pub" generated at the client to the server so that the server can recognise the client during access request. Use the command below:
```
ssh-copy-id -i /home/<client username>/.ssh/id_ras.pub <serverusername>@<serverIPaddress>
```
In my case is 
```
ssh-copy-id -i /home/cally/.ssh/id_rsa.pub cally@10.199.21.2
```
or
```
ssh-copy-id -i ~/.ssh/id_rsa.pub cally@10.199.212.2
```

often time, we can use the tilda sign ~ to represent "home" in linux.

![alt](./Images/SSH%20copy%20success.JPG)

From the output, we can see that it returned an ssh command we can use to login securely to the remote server. ``ssh cally@10.199.212.2``

- We need to check to ensure that the key was transfer succesfully to the server destination. On the server, run the ``ls -latr`` command to display the sent rsa.pub key.
![alt](./Images/Public%20key%20recognsed%20at%20the%20server.JPG)


- Post confirmation, run the recommended ssh command to login to remote server.
```
ssh cally@10.199.212.2
```
![alt](./Images/Successful%20login%20to%20server.JPG)

- To exit the server, use the "exit" command to return to client command interface.

Note: From the client end, you can run the recommended ssh command with v or vv or vvv to output more details of the processes before authetication and successful configuration. ``ssh cally@10.199.212.2 -v or ssh cally@10.199.212.2 -vv or ssh cally@10.199.212.2 -vvv``
From the server end, yYou can monitor the process and logs of all activites and data received by the serve. You can use command ``sudo tail -f /var/log/auth.log``. The ``tail -f`` command gives a real time output of all the log activites. 