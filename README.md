Install-Hadoop-2.2
==================

Steps to install Hadoop 2.2 on Ubuntu 12.04

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
	
	For verification of ssh configuration, run following command - 
		
		ssh localhost or ssh hduser@localhost
