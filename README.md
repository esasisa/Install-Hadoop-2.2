Install-Hadoop-2.2
==================

Steps to install Hadoop 2.2 on Ubuntu 12.04

1- Create hadoop user :- This is not a mandatory step. It is just to seperate the Hadoop instalation from other installed applications and users which are running on this machine.

    sudo addgroup hadoop
    sudo adduser —ingroup hadoop hduser
