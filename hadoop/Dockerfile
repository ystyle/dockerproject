FROM ubuntu:14.10
MAINTAINER lxy5266@live.com <ystyle>

ENV HADOOP_HOME /usr/lib/hadoop
ENV HADOOP_CONFIG_HOME $HADOOP_HOME/etc/hadoop

RUN apt-get update
RUN apt-get install software-properties-common python-software-properties
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get install oracle-java7-installer
RUN apt-get install ssh
RUN systemctl enable sshd
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
RUN cat id_dsa.pub >> authorized_keys
RUN curl http://mirrors.sonic.net/apache/hadoop/common/hadoop-2.7.2/hadoop-2.7.2.tar.gz -o /usr/lib
RUN mv /usr/lib/hadoop-2.7.2 $HADOOP_HOME
RUN sed -i 's/export JAVA_HOME=\${JAVA_HOME}/export JAVA_HOME=\/usr\/lib\/jvm\/java-7-oracle/' $HADOOP_CONFIG_HOME/hadoop-env.sh

COPY core-site.xml $HADOOP_CONFIG_HOME/core-site.xml
COPY hdfs-site.xml $HADOOP_CONFIG_HOME/hdfs-site.xml
COPY mapred-site.xml $HADOOP_CONFIG_HOME/mapred-site.xml
COPY slaves $HADOOP_CONFIG_HOME/slaves

VOLUME ["/data"]

RUN mkdir /data/tmp /data/namenode /data/datanode

ENV JAVA_HOME /usr/lib/jvm/java-7-oracle
ENV PATH $PATH:$HADOOP_HOME/bin
ENV PATH $PATH:$HADOOP_HOME/sbin

WORKDIR /usr/lib/hadoop
