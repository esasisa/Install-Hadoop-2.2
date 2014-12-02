Install-Hadoop-2.2
==================

Steps by Step Guide to install Hadoop 2.2 on Ubuntu 12.04

Pre requisit for Hadoop installation

	1- Create hadoop user :- This is not a mandatory step. It is just to seperate the Hadoop instalation from other
	installed applications and users which are running on this machine.

    	sudo addgroup hadoop
    	sudo adduser —ingroup hadoop hduser

	2- Install Python :- 
	
		sudo apt-get install python-software-properties
	
	Note:- If you are running behind proxy, set the proxy before installation.
	
		export http_proxy="<your proxy>"
		export https_proxy="<your proxy>"
		export ftp_proxy="<your proxy>"
	
	3- Install Java :- 
	
		3.1 Add repository :- (If you are rnning behind proxy use E option, other wise remove this option)
		
			sudo -E add-apt-repository ppa:webupd8team/java
		
		3.2 Update repository :- 
	
			sudo apt-get update
			
		3.3 Install Oracle Java 7
		
			sudo apt-get install oracle-java7-installer
			
	4- Install SSH Server :-
	
		sudo apt-get install openssh-server openssh-client
		
	5- Configure SSH :- 
	
		su - hduser
		ssh-keygen -t rsa -P ""
		cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
	
		Verify SSH configuration:-  
		
		ssh localhost or ssh hduser@localhost
	
	6- Disable IPV6 :- 
	
		Open the file sysctl.conf
	
		vi /etc/sysctl.conf
	
		Add the following lines below of the file
	
		net.ipv6.conf.all.disable_ipv6 = 1
		net.ipv6.conf.default.disable_ipv6 = 1
		net.ipv6.conf.lo.disable_ipv6 = 1
		
		Verify IPV6 configuration:-  
	
		cat /proc/sys/net/ipv6/conf/all/disable_ipv6
	
Install Hadoop

	1 - Download Haddop Tar :- Download this file in hduser home.
	
		cd /home/hduser
		wget http://apache.mirrors.pair.com/hadoop/common/stable2/hadoop-2.2..tar.gz
		
	2- Extract tar file :-
		
		tar –xvzf hadoop-2.2.0.tar.gz
		
	3- Move files into hadoop :- 
	
		mv hadoop-2.2.0 hadoop
		
	4- Change the files permission :- 
		
		sudo chown -R hduser:hadoop Hadoop
		
Configure Hadoop	

	1- Update .bashrc file
		open .bashrc of the hduser in vi editor
		
			vi $HOME/.bashrc
		
		Add following lines at below of the file
			
			# Set Hadoop-related environment variables
			export HADOOP_PREFIX=<hadoop installation directory>
			export HADOOP_HOME=<hadoop installation directory>
			export HADOOP_MAPRED_HOME=${HADOOP_HOME}
			export HADOOP_COMMON_HOME=${HADOOP_HOME}
			export HADOOP_HDFS_HOME=${HADOOP_HOME}
			export YARN_HOME=${HADOOP_HOME}
			export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
			# Native Path
			export HADOOP_COMMON_LIB_NATIVE_DIR=${HADOOP_PREFIX}/lib/native
			export HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib"
			#Java path
			export JAVA_HOME='<Java Home>'
			# Add Hadoop bin/ directory to PATH
			export PATH=$PATH:$HADOOP_HOME/bin:$JAVA_PATH/bin:$HADOOP_HOME/sbin
			
		Note :- Provide the value of HADOOP_PREFIX, HADOOP_HOME and JAVA_HOME.
			In this case HADOOP_PREFIX and HADOOP_HOME value will be "/home/hduser/hadoop"
			and the value of JAVA_HOME will be "/usr/lib/jvm/java-7-oracle" 
	
	
	
	
	
	
	
	
	
