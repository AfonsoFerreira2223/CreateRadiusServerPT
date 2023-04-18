### First the topology:


![image](https://user-images.githubusercontent.com/114146560/232753282-9bbf14a7-f89d-45a9-be43-761ee9cf02b2.png)

> Name the routers R1 and R2
> Router R2 will be used to ssh to R1. The username and passwords used will be authenticated using the databse on the Radius Server

## Create the network topology

> All three devices will be on the same network.

> R1 is 192.168.1.1 /24

> R2 is 192.168.1.2 /24

> The server is 192.168.1.3 /24 (To configure the Ip on radius server go to config-Interface-Ipv4)

## In the Radius server:

![image](https://user-images.githubusercontent.com/114146560/232759760-813956af-5283-417a-8072-43cddf57bfe7.png)

> Make sure to turn on the server
> Put R1 as the client name and the Ip of the interface facing the server as Client IP
> Set the secret. This will work as a password and should match on teh device when configured there

>Click add and save

> Now in the lower window create your user. They will be used to log into the router (similar to the command username user password password on the local router)
> Put the username and the password, click add and then save

## Now in router 1

> Make sure to ping the 192.168.1.2 and .3 to verify connectivity

> First tell the router to use aaa auth
> then define a backup user and password. This will only be used in case the Radius server is down and you need access to the router
> Now define the group. First comes radius and then local. This means the local user you defined before will only be used if Radius auth is unreachable
> Replace the server Ip with whichever Ip you have put on your Radius server interface
> The key should be the same as the one in the Radius server setup window

### Commands:

aaa new-model
username backup privilege 15 password password
aaa authentication login default group radius local
aaa authentication enable default group radius local
radius server host
add ipv4 192.168.1.3
key password
exit
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh v 2
line vty 0 15
transport input ssh
no shut
login authentication default


