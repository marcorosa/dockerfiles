#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.

FROM centos:centos6

RUN echo root:changeme | chpasswd

## Install some basic utilities that aren't in the default image
RUN yum clean all -y && yum update -y
RUN yum -y install vim wget sudo rpm-build
RUN yum -y install which telnet tar openssh-server openssh-clients
RUN yum -y install ntp git python-setuptools python-devel httpd
# phantomjs dependency
RUN yum -y install fontconfig freetype libfreetype.so.6 libfontconfig.so.1 libstdc++.so.6
RUN rpm -e --nodeps --justdb glibc-common
RUN yum -y install glibc-common
# Install Python 2.7
RUN yum -y install centos-release-scl
RUN yum -y install python27
RUN scl enable python27 bash

ENV HOME /root

#Install JAVA
RUN wget --no-check-certificate --no-cookies --header "Cookie:oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/jdk/8u191-b12/2787e4a523244c269598db4e85c51e0c/jdk-8u191-linux-x64.rpm
RUN rpm -ivh jdk-8u191-linux-x64.rpm
ENV JAVA_HOME /usr/java/default/

#Install Maven
RUN mkdir -p /opt/maven
WORKDIR /opt/maven
RUN wget http://apache.crihan.fr/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
RUN tar -xvzf /opt/maven/apache-maven-3.6.0-bin.tar.gz
RUN rm -rf /opt/maven/apache-maven-3.6.0-bin.tar.gz

ENV M2_HOME /opt/maven/apache-maven-3.6.0
ENV MAVEN_OPTS -Xmx2048m -XX:MaxPermSize=256m
ENV PATH $PATH:$JAVA_HOME/bin:$M2_HOME/bin

# SSH key
RUN ssh-keygen -f /root/.ssh/id_rsa -t rsa -N ''
RUN cat /root/.ssh/id_rsa.pub > /root/.ssh/authorized_keys
RUN chmod 600 /root/.ssh/authorized_keys
RUN sed -ri 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

#To allow bower install behind proxy. See https://github.com/bower/bower/issues/731
# RUN git config --global url."https://".insteadOf git://

# Install python, nodejs and npm
RUN yum -y install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum -y install nodejs npm --enablerepo=epel
#@2.1.11
RUN npm install -g npm
#@1.7.20
RUN npm install -g brunch
RUN npm set registry https://registry.npmjs.org/
ENV PHANTOMJS_CDNURL http://cnpmjs.org/downloads

# Once run some mvn commands to cache .m2/repository
WORKDIR /
RUN git clone https://github.com/apache/ambari.git
WORKDIR /ambari
RUN npm set registry https://registry.npmjs.org/


ADD scripts/build.sh /build.sh
