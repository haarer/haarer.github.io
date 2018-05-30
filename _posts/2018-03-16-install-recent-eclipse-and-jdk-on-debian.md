---
layout: post
title: "Install Eclipse and JDK on Debian"
categories: eclipse,debian,java
---

I had to set up a new development machine with Debian 9. 
Debian hasn't the newest packages available - because they prefer stability.

This is how i installed a current Eclipse and JDK (oxygen at time of writing and JDK8).

**Prepare for JDK Installation**

Install the software-properties-common package in order to use the apt-get-repository command. This allows adding a repository to apt

    sudo apt-get install software-properties-common

Add the repository 

    sudo add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"

Update package list

    sudo apt-get update

**Install Oracle JDK 8**

Oracle JDK 8 is the latest stable version of Java. Install it.

    sudo apt-get install oracle-java8-installer

Verify Java version:

    javac -version


**Select used Java Version**
You can configure which version is the default for use in the command line:

    sudo update-alternatives --config java

Similarly this can be configured for the java compiler

    sudo update-alternatives --config javac


**Set JAVA_HOME**

Many programs use the JAVA_HOME environment variable.


Add a line to your .profile

	JAVA_HOME="/usr/lib/jvm/java-8-oracle"

**Install Eclipse**

Download [Eclipse][https://www.eclipse.org/downloads/eclipse-packages]

Unpack eclipse to /opt

	cd /opt
	sudo tar xzf eclipse-cpp-oxygen-2-linux-gtk-x86_64.tar.gz

Create a symbolic link 

	sudo ln -s /opt/eclipse/eclipse /usr/bin/eclipse

Now you should be able to run eclipse 

