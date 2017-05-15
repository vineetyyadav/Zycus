#The base image to use in the build
FROM centos:centos6
	MAINTAINER The Vineet Yadav <vineet.yadav@hcl.com>
	
	###Installing MongoDB server using epel release Repo
	#yum is the primary tool for getting, installing, deleting, querying, and managing Red Hat Enterprise Linux RPM software packages from official Red Hat software repositories, as well as other third-party repositories
	RUN yum -y update; yum clean all
	RUN yum -y install epel-release; yum clean all
	RUN yum -y install mongodb-server; yum clean all
	RUN mkdir -p /data/db
	
	#Opens a port for linked containers
	EXPOSE 27017
	#Expose a port or a range of ports
	#Overwrite the default ENTRYPOINT of the image
	ENTRYPOINT ["/usr/bin/mongod"]
	
	###Installing python
	
	# Install yum dependencies
	#yum grouplist command to list available package groups.
	#OpenSSL is a toolkit for supporting cryptography. The openssl-devel package contains include files needed to develop applications which support various cryptographic algorithms and protocols.
	#sqlite-devel:This package contains the header files and development documentation for sqlite. If you like to develop programs using sqlite, you will need to install sqlite-devel
	#The sudo command offers another approach to giving users administrative access. When trusted users precede an administrative command with sudo, they are prompted for their own password
	#Tar files place several files or the contents of a directory or directories in one file.
	#wget is a free utility for non-interactive download of files from the web. It supports HTTP, HTTPS, and FTP protocols, as well as retrieval through HTTP proxies.
	#The purpose of the make utility is to determine automatically which pieces of a large program need to be re-compiled, and issue the commands necessary to recompile them.
	RUN yum -y update && \
	    yum groupinstall -y development && \
	    yum install -y \
	    bzip2-devel \
	    git \
	    hostname \
	    openssl \
	    openssl-devel \
	    sqlite-devel \
	    sudo \
	    tar \
	    wget \
	    zlib-dev
	
	# Install python2.7
	RUN cd /tmp && \
	    wget https://www.python.org/ftp/python/2.7/Python-2.7.tgz && \
	    tar xvfz Python-2.7.tgz && \
	    cd Python-2.7 && \
	    ./configure --prefix=/usr/local && \
	    make && \
	    make altinstall
	
	# Install setuptools + pip
	RUN cd /tmp && \
	    wget --no-check-certificate https://pypi.python.org/packages/source/s/setuptools/setuptools-1.4.2.tar.gz && \
	    tar -xvf setuptools-1.4.2.tar.gz && \
	    cd setuptools-1.4.2 && \
	    python2.7 setup.py install && \
	    curl https://raw.githubusercontent.com/pypa/pip/master/contrib/get-pip.py | python2.7 - && \
	    pip install virtualenv
	
	###To install Apache Tomcat 7
	
	#Install WGET
	RUN yum install -y wget
	#Install tar
	RUN yum install -y tar
	
	#Install Openjdk7 
	
	RUN yum update -y && \
	yum install -y wget && \
	yum install -y java-1.7.0-openjdk java-1.7.0-openjdk-devel && \
	yum clean all
	
	# Download Apache Tomcat 7
	RUN cd /tmp;wget http://redrockdigimark.com/apachemirror/tomcat/tomcat-7/v7.0.73/bin/apache-tomcat-7.0.73.tar.gz
	
	# untar and move to proper location
	#extract all,decompress using gunzip 
	#mv renames file SOURCE to DEST, or moves the SOURCE file (or files) to DIRECTORY.
	 #chmod command to change permissions.
	 #for imnstallation of java so we configure JAVA 
	RUN cd /tmp;gunzip apache-tomcat-7.0.73.tar.gz
	RUN cd /tmp;tar xvf apache-tomcat-7.0.73.tar
	RUN cd /tmp;mv apache-tomcat-7.0.73 /opt/tomcat7
	RUN chmod -R 755 /opt/tomcat7
	
	#Sets an environment variable in the new container
	ENV JAVA_HOME /opt/jdk1.7.0_79
	
	#Opens a port for linked containers
	EXPOSE 8080
	
	#The command that runs when the container starts
	CMD ["catalina.sh", "run"]