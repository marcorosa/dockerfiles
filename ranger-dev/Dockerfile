FROM centos:centos6

MAINTAINER Jesus Alvarez @Chuyqa

RUN echo root:changeme | chpasswd

# Build packages
RUN yum clean all -y && yum update -y
# Install SCL
RUN yum -y install centos-release-scl
# Install Python 2.7
RUN yum -y install python27
# Use Python 2.7 instead of 2.6
RUN scl enable python27 bash
# Other dependencies
RUN yum -y install vim wget rpm-build which tar openssh-server openssh-clients git python-setuptools python-devel glibc-common
RUN yum -y install gcc  # for credValidator
RUN yum -y install bc  # for admin service

ENV HOME /root

# Install JAVA
RUN wget --no-check-certificate --no-cookies --header "Cookie:oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.rpm
RUN rpm -Uvh jdk-8u191-linux-x64.rpm
ENV JAVA_HOME /usr/java/default/

# Install Maven 3.3.0+
RUN mkdir -p /opt/maven
WORKDIR /opt/maven
RUN wget http://apache.crihan.fr/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
RUN tar -xvzf /opt/maven/apache-maven-3.6.0-bin.tar.gz
RUN rm -rf /opt/maven/apache-maven-3.6.0-bin.tar.gz

ENV M2_HOME /opt/maven/apache-maven-3.6.0
ENV MAVEN_OPTS -Xmx2048m
ENV PATH $PATH:$JAVA_HOME/bin:$M2_HOME/bin


# SSH key
RUN ssh-keygen -f /root/.ssh/id_rsa -t rsa -N ''
RUN cat /root/.ssh/id_rsa.pub > /root/.ssh/authorized_keys
RUN chmod 600 /root/.ssh/authorized_keys
RUN sed -ri 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

# Clone Ranger
WORKDIR /
RUN git clone https://github.com/apache/incubator-ranger

WORKDIR /incubator-ranger/
RUN mvn dependency:resolve

# Build (step 1.2 of Ranger guide)
RUN mvn clean compile package install assembly:assembly
