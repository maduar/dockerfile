# VERSION 0.0.1
FROM centos:7
# 签名啦
MAINTAINER madaur "maduar@163.com"

ENV JENKINS_WORRKSTATION /opt/workstation/jenkins
ENV MAVEN_VERSION 3.5.3
ENV CONFIG_FILE /etc/profile

RUN yum update
# init service comand
RUN yum -y install initscripts
RUN yum install -y wget

RUN mkdir -p /opt/java
RUN mkdir -p $JENKINS_WORRKSTATION

WORKDIR /opt/java

# download jdk
RUN cd /opt/java && wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz \
	&& tar -zxvf  /opt/java/jdk-8u171-linux-x64.tar.gz

# jkd config
RUN sed -i '$a export JAVA_HOME=/opt/java/jdk1.8.0_171 \n\
export PATH=$JAVA_HOME/bin:$PATH \n\
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar \n\
export JENKINS_HOME=/opt/workstation/jenkins ' $CONFIG_FILE
RUN source $CONFIG_FILE


# download maven
RUN cd /opt/java && wget http://mirrors.hust.edu.cn/apache/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
	&& tar -zxvf /opt/java/apache-maven-$MAVEN_VERSION-bin.tar.gz

# download git
RUN yum install -y git
RUN wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
RUN rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
RUN yum install -y jenkins

# change jenkins config
RUN perl -p -i -e "s/JENKINS_USER=\"jenkins\"/JENKINS_USER=\"root\"/g" /etc/sysconfig/jenkins
RUN perl -p -i -e "s/\/usr\/lib\/jvm\/java\-1\.7\.0\/bin\/java/\/opt\/java\/jdk1\.8\.0\_171\/bin\/java/g" /etc/init.d/jenkins
RUN perl -p -i -e "s/\-\-daemon/\-\-daemon \-\-prefix=\/jenkins/g" /etc/init.d/jenkins

EXPOSE 8080

# init 
RUN touch /opt/java/init.sh && chmod 777 /opt/java/init.sh
RUN echo "#! /bin/bash" >> /opt/java/init.sh
RUN echo "source $CONFIG_FILE" >> /opt/java/init.sh
RUN echo "service jenkins start " >> /opt/java/init.sh

# shell -> container running
CMD ["sh", "-c", "sh /opt/java/init.sh && /bin/bash"]
