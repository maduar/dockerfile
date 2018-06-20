# VERSION 0.0.1
FROM centos:7
# 签名啦
MAINTAINER madaur "maduar@163.com"

RUN yum update
RUN yum install -y wget

RUN mkdir -p /opt/java
WORKDIR /opt/java

# download jdk
RUN cd /opt/java && wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz \
	&& tar -zxvf  /opt/java/jdk-8u171-linux-x64.tar.gz

# jkd config
RUN sed -i '$a export JAVA_HOME=/opt/java/jdk1.8.0_171 \n\
export PATH=$JAVA_HOME/bin:$PATH \n\
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar ' ~/.bash_profile

RUN source ~/.bash_profile

# download tomcat
RUN cd /opt/java && wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz \
	&& tar -zxvf /opt/java/apache-tomcat-8.5.31.tar.gz

	EXPOSE 8080

# init 
RUN touch /opt/java/init.sh && chmod 777 /opt/java/init.sh
RUN echo "#! /bin/bash" >> /opt/java/init.sh
RUN echo "source ~/.bash_profile" >> /opt/java/init.sh
RUN echo "sh /opt/java/apache-tomcat-8.5.31/bin/startup.sh" >> /opt/java/init.sh
#RUN echo "# tail -f /dev/null" >> /opt/java/init.sh

# shell -> container running
CMD ["sh", "-c", "sh /opt/java/init.sh && /bin/bash"]

