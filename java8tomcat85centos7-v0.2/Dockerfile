SION 0.0.1
FROM centos:7
# 签名啦
MAINTAINER madaur "maduar@163.com"

RUN yum update
RUN yum install -y wget

RUN mkdir -p /opt/java
WORKDIR /opt/java

# download jdk
RUN yum install -y java-1.8.0-openjdk*

# download tomcat
RUN cd /opt/java && wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-8/v8.5.31/bin/apache-tomcat-8.5.31.tar.gz \
    && tar -zxvf /opt/java/apache-tomcat-8.5.31.tar.gz

EXPOSE 8080

CMD ["sh", "-c", "sh /opt/java/apache-tomcat-8.5.31/bin/startup.sh && /bin/bash"]

