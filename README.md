# MySQL-Cluster

## 1.Download MySQL-Cluster

http://dev.mysql.com/get/Downloads/MySQL-Cluster-7.4/mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64.tar.gz

## 2.Use vargrant with the Vagrantfile

`vagrant up 'machinename'`

the up order is Manage.Nodes > Data.Nodes.1 > SQL.Nodes.1 


## 3.Login Manage.Nodes to check all nodes are working 
`vagrant ssh Manage.Nodes` <br>
`$ndb_mgm` <br>
`>show` <br>

You will get the info of Nodes like this

    Cluster Configuration 
    ---------------------
    [ndbd(NDB)]     2 node(s)
    id=2    @192.168.30.10  (mysql-5.6.29 ndb-7.4.11, Nodegroup: 0)
    id=3    @192.168.30.15  (mysql-5.6.29 ndb-7.4.11, Nodegroup: 0, *)

    [ndb_mgmd(MGM)] 1 node(s)
    id=1    @192.168.30.5  (mysql-5.6.29 ndb-7.4.11)

    [mysqld(API)]   2 node(s)
    id=4    @192.168.30.20  (mysql-5.6.29 ndb-7.4.11)
    id=5    @192.168.30.25  (mysql-5.6.29 ndb-7.4.11)



## 4.Login SQL.Nodes.1 & 2 to create database user

###  a.login SQL.Nodes.1 & 2
`vagrant ssh SQL.Nodes.1`
### b. set root password
`sudo /usr/local/mysql/bin/mysqladmin -u root password "password"`
### c. Login MySQL console
`sudo /usr/local/mysql/bin/mysql -u root -p`
### d.create a user that it can be login from any ip.
`create user 'user'@'%' identified by 'user' ;`
### e.Give the authority to user
`grant all on *.* to 'user'@'%';`


---------------------------------------

@@尚未完成服務自動啟動 若重新開機需手動啟動各項服務

啟動Manage.Nodes <br>
`sudo /usr/local/mysql/bin/ndb_mgmd -f /usr/local/mysql/mysql-cluster/config.ini --initial` <br>
啟動Data.Nodes <br>
`sudo /usr/local/mysql/bin/ndbd --initial` <br>
啟動SQL.Nodes <br> 
`sudo /usr/local/mysql/support-files/mysql.server start` <br>
  
