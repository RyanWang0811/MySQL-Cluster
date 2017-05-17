# MySQL-Cluster

##1.Download MySQL-Cluster

http://dev.mysql.com/get/Downloads/MySQL-Cluster-7.4/mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64.tar.gz

##2.Use vargrant with the Vagrantfile

vagrant up 'machinename' 

the up order is Manage.Nodes > Data.Nodes.1 > SQL.Nodes.1 


##3.Login Manage.Nodes to check all nodes are working 
vagrant ssh Manage.Nodes
$ndb_mgm
>show

##4.Login SQL.Nodes.1 & 2 to create database user
a.login SQL.Nodes.1 & 2
vagrant ssh SQL.Nodes.1
1. set root password
  sudo /usr/local/mysql/bin/mysqladmin -u root password "password"
2. Login MySQL console
  sudo /usr/local/mysql/bin/mysql -u root -p
3.create a user that it can be login from any ip.
  create user 'user'@'%' identified by 'user' ;
4.Give the authority to user
grant all on *.* to 'user'@'%';


  