Vagrant.configure("2") do |config|
  
  # 此處的 shell provision 會套用到所有的 VM
	config.vm.provision "file", source: "./mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64.tar.gz", destination: "mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64.tar.gz"
  config.vm.provision "shell", inline: <<-SHELL
	sudo tar -zxvf mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64.tar.gz
	sudo mv mysql-cluster-gpl-7.4.11-linux-glibc2.5-x86_64 mysql
	sudo mv mysql /usr/local
#1. 新增mysql群組
  sudo groupadd mysql
#2. 新增mysql使用者
  sudo useradd mysql -s /sbin/nologin -g mysql
#3. 設定資料夾權限
  sudo chown -R root:root /usr/local/mysql
	sudo chown -R mysql:mysql /usr/local/mysql/data
	SHELL
  
 

  # Manage Nodes設定
  config.vm.define "Manage.Nodes" do |m1|
  m1.vm.box = "ubuntu/trusty64"
	m1.vm.hostname = "Manage.Nodes"
	m1.vm.network "private_network", ip: "192.168.30.5"
  m1.vm.provision "file", source: "./mysql-cluster-config/config.ini", destination: "config.ini"
  m1.vm.provision "shell", inline: <<-SHELL
  #1.新增mysql-cluster資料夾
  sudo mkdir /usr/local/mysql/mysql-cluster
  #2.移動config.ini
  sudo mv config.ini /usr/local/mysql/mysql-cluster
  #3.啟動且初始化
	sudo /usr/local/mysql/bin/ndb_mgmd -f /usr/local/mysql/mysql-cluster/config.ini --initial
  #4.將工具複製到 /usr/local/bin 方便使用
  sudo cp /usr/local/mysql/bin/ndb_mgm* /usr/local/bin/
  SHELL
   m1.vm.provider "virtualbox" do |v|
      v.memory = 2048
       end
  end

  # Data Nodes 1 設定
  config.vm.define "Data.Nodes.1" do |d1|
    d1.vm.box = "ubuntu/trusty64"
	d1.vm.hostname = "Data.Nodes.1"
	d1.vm.network "private_network", ip: "192.168.30.10"
  d1.vm.provision "file", source: "./mysql-cluster-config/my.cnf", destination: "my.cnf"
  d1.vm.provision "shell", inline: <<-SHELL
  #1.移動 my.cnf
  sudo mv my.cnf /etc
  #2.啟動且初始化
	sudo /usr/local/mysql/bin/ndbd --initial
  SHELL
  d1.vm.provider "virtualbox" do |v|
      v.memory = 2048
       end
  end 
   # Data Nodes 2 設定
  config.vm.define "Data.Nodes.2" do |d2|
    d2.vm.box = "ubuntu/trusty64"
	d2.vm.hostname = "Data.Nodes.2"
	d2.vm.network "private_network", ip: "192.168.30.15"
  d2.vm.provision "file", source: "./mysql-cluster-config/my.cnf", destination: "my.cnf"
  d2.vm.provision "shell", inline: <<-SHELL
  #1.移動 my.cnf
  sudo mv my.cnf /etc
  #2.啟動且初始化
	sudo /usr/local/mysql/bin/ndbd --initial
  SHELL
  d2.vm.provider "virtualbox" do |v|
      v.memory = 2048
       end
  end 
   # SQL Nodes 1 設定
  config.vm.define "SQL.Nodes.1" do |s1|
    s1.vm.box = "ubuntu/trusty64"
	s1.vm.hostname = "SQL.Nodes.1"
	s1.vm.network "private_network", ip: "192.168.30.20"
  s1.vm.provision "file", source: "./mysql-cluster-config/my.cnf", destination: "my.cnf"
  s1.vm.provision "shell", inline: <<-SHELL
  #1.移動 my.cnf
  sudo mv my.cnf /etc
  #2. 安裝libaio1套件
  sudo apt-get install libaio1
  #3. 初始化資料庫
  sudo /usr/local/mysql/scripts/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
  #4. 啟用SQL節點服務
  sudo /usr/local/mysql/support-files/mysql.server start
  SHELL
  	 s1.vm.provider "virtualbox" do |v|
      v.memory = 1024
       end
  end 
   # SQL Nodes 2 設定
  config.vm.define "SQL.Nodes.2" do |s2|
    s2.vm.box = "ubuntu/trusty64"
	s2.vm.hostname = "SQL.Nodes.2"
	s2.vm.network "private_network", ip: "192.168.30.25"
  s2.vm.provision "file", source: "./mysql-cluster-config/my.cnf", destination: "my.cnf"
  s2.vm.provision "shell", inline: <<-SHELL
  #1.移動 my.cnf
  sudo mv my.cnf /etc
  #2. 安裝libaio1套件
  sudo apt-get install libaio1
  #3. 初始化資料庫
  sudo /usr/local/mysql/scripts/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
  #4. 啟用SQL節點服務
  sudo /usr/local/mysql/support-files/mysql.server start
  SHELL
  	 s2.vm.provider "virtualbox" do |v|
      v.memory = 1024
       end
  end 
  
end