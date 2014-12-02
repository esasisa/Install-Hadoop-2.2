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
	
	2- Configure hadoop settings :- 
		cd <hadoop installation dir>/etc/hadoop/
		
		In this case <hadoop installation dir> value will be "/home/hduser/hadoop"
	
		2.1 - Configure yarn-site.xml :- 
			
			Open yarn-site.xml 
			
				vi yarn-site.xml
			
			Add following lines within <configuration> tag
			
				<property>
				    <name>yarn.nodemanager.aux-services</name>
				    <value>mapreduce_shuffle</value>
				</property>
				<property>
				    <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
				    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
				</property>
		
		2.2 - Configure core-site.xml :- 
			
			Open core-site.xml 
			
				vi core-site.xml
			
			Add following lines within <configuration> tag
			
				<property>
			    	    <name>fs.default.name</name>
			    	    <value>hdfs://localhost:9000</value>
				</property>
				
		2.3 - Configure mapred-site.xml :- 
			
			Open mapred-site.xml 
			
				vi mapred-site.xml
			
			Add following lines within <configuration> tag
			
				<property>
				    <name>mapreduce.framework.name</name>
				    <value>yarn</value>
				</property>

		2.4 - Configure hdfs-site.xml :- 
			
			Create name node and data node directories
			
			mkdir -p $HADOOP_HOME/yarn_data/hdfs/namenode
			mkdir -p $HADOOP_HOME/yarn_data/hdfs/datanode
			
			Open hdfs-site.xml 
			
				vi hdfs-site.xml
			
			Add following lines within <configuration> tag
			
				<property>
				    <name>dfs.replication</name>
				    <value>1</value>
				</property>
				<property>
				    <name>dfs.namenode.name.dir</name>
				    <value>file:<hadoop Home>/yarn_data/hdfs/namenode</value>
				</property>
				<property>
				    <name>dfs.datanode.data.dir</name>
				    <value>file:<hadoop Home>/yarn_data/hdfs/datanode</value>
				</property>
				
			Note:- In this case <Hadoop Home> will be "/home/hduser/hadoop" 
			
			
	3- Formatting HDFS filesystem :-
	
		hadoop namenode -format       (hadoop command is depricated in 2.2 version) or
		hdfs namenode -format
	
Start and Stop Hadoop

	1- Start Name node:- 
	
	
	
