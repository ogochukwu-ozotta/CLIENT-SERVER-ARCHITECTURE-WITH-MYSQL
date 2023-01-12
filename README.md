# CLIENT-SERVER-ARCHITECTURE-WITH-MYSQL

# PROJECT 5 - CLIENT/SERVER ARCHITECTURE USING A MYSQL RELATIONAL DATABASE MANAGEMENT SYSTEM (DBMS).

## CLIENT-SERVER ARCHITECTURE WITH MYSQL

### Understanding Client-Server Architecture

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".

A simple diagram of Web Client-Server architecture is presented below:

![image](https://user-images.githubusercontent.com/88560609/197212912-19f8f73b-a6be-4c0f-a287-ce169acf9615.png)


In the example above, a machine that is trying to access a Web site using Web browser or simply ‘curl’ command is a client and it sends HTTP requests to a Web server 
(Apache, Nginx, IIS or any other) over the Internet.

If we extend this concept further and add a Database Server to our architecture, we can get this picture:

![image](https://user-images.githubusercontent.com/88560609/197213104-66b49974-8564-4402-a6fa-0fb62ea4812a.png)


In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, 
but it is a common practice to place Web Server and DB Server close to each other in local network).

The setup on the diagram above is a typical generic Web Stack architecture that you have already implemented in previous projects (LAMP, LEMP, MEAN, MERN), 
this architecture can be implemented with many other technologies – various Web and DB servers, from small Single-page applications SPA to large and complex portals.

Real example of LAMP website
In Project 1 you implemented a LAMP STACK website, let us take an example of commercially deployed LAMP website – www.propitixhomes.com.

This LAMP website server(s) can be located anywhere in the world and you can reach it also from any part of the globe over global network – Internet.

Assuming that you go on your browser, and typed in there www.propitixhomes.com. It means that your browser is considered the "Client". Essentially, 
it is sending request to the remote server, and in turn, would be expecting some kind of response from the remote server.

Lets take a very quick example and see Client-Server communicatation in action.

Open up your Ubuntu or Windows terminal or GitBash and run curl command:


```
curl -Iv www.propitixhomes.com
```

<img width="506" alt="image" src="https://user-images.githubusercontent.com/88560609/197215237-982e3cbc-5c4a-42ac-9eea-8d47b7667712.png">


Note: If your Ubuntu does not have ‘curl’, you can install it by running sudo apt install curl

In this example, your terminal will be the client, while www.propitixhomes.com will be the server.

See the response from the remote server in below output. You can also see that the requests from the URL are being served by a computer
with an IP address 160.153.133.153 on port 80. More on IP addresses and ports when we get to Networking related projects

<img width="529" alt="image" src="https://user-images.githubusercontent.com/88560609/197215756-2bcad393-3ae6-464c-ba55-3b18636d8af8.png">


Another simple way to get a server’s IP address is to use a simple diagnostic tool like ‘ping’, it will also show round-trip time – time for packets to go to 
and back from the server, this tool uses ICMP protocol.

## IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

TASK – Implement a Client Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), follow the below instructions

1. Create and configure two Linux-based ubuntu virtual servers (EC2 instances in AWS). security group allows port 22

```
Server A name - `mysql-db-server`
Server B name - `mysql-client`
```

<img width="821" alt="image" src="https://user-images.githubusercontent.com/88560609/197217638-5045e69a-c071-4b08-963d-b0cb5454178e.png">

2. On mysql-db-server Linux Server install MySQL Server software.

```
sudo apt install mysql-server -y
```
Enable the service
```
sudo systemctl enable mysql
```

Interesting fact: MySQL is an open-source relational database management system. Its name is a combination of "My", the name of co-founder Michael Widenius’s daughter, 
and "SQL", the abbreviation for Structured Query Language.


3. On mysql-client Linux Server install MySQL Client software.
```
sudo apt install mysql-client -y
```

4.By default, both of your EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. 
Use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, 
so you will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, 
do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of your ‘mysql client’.

<img width="803" alt="image" src="https://user-images.githubusercontent.com/88560609/197241386-bedb0c94-0a54-444b-af37-75ef3b6d2c0d.png">

How to get local IP Address of mysql client 

<img width="449" alt="image" src="https://user-images.githubusercontent.com/88560609/197226636-301da922-2817-4a6d-95da-ee3030a0858a.png">



```
sudo mysql
```

<img width="443" alt="image" src="https://user-images.githubusercontent.com/88560609/197243083-81964a7e-f2e5-499f-a1d2-e058480efdcf.png">

Set up mysql server 

```
CREATE USER ogo@'%' IDENTIFIED BY 'password';
```
```
CREATE DATABASE test_db;
```
```
GRANT ALL ON test_db.* TO ogo@'%' WITH GRANT OPTION;
```
```
FLUSH PRIVILEGES;
```
```
exit
```

<img width="448" alt="image" src="https://user-images.githubusercontent.com/88560609/197246362-7bb0a8e1-2842-4376-b6a5-c4d529dfe8a2.png">

5. You might need to configure MySQL server to allow connections from remote hosts.

```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:

<img width="446" alt="image" src="https://user-images.githubusercontent.com/88560609/197255259-1259d3fb-c031-436d-89de-025fd6aa9c50.png">

Restart the service 

```
sudo systemctl restart mysql
```

6. From mysql client Linux Server connect remotely to mysql server Database Engine without using SSH. You must use the mysql utility to perform this action. `-u` for user, `-h` for host, `-p` for password prompt
```
sudo mysql -u ogo -h <private-IP-Address of mysql db server> -p
```

<img width="452" alt="image" src="https://user-images.githubusercontent.com/88560609/197256096-9855e194-1ad6-4eb1-b558-b39039b278dc.png">


7. Check that you have successfully connected to a remote MySQL server and can perform SQL queries:


```
Show databases;
```

If you see an output similar to the below image, then you have successfully completed this project – you have deloyed a fully functional MySQL Client-Server set up.

<img width="451" alt="image" src="https://user-images.githubusercontent.com/88560609/197256401-5f204268-c89e-4ef5-acb3-cf9b600fcae5.png">



