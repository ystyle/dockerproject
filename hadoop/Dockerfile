FROM ubuntu:16.04
MAINTAINER lxy5266@live.com <ystyle>

ENV HADOOP_HOME /usr/lib/hadoop
ENV HADOOP_CONFIG_HOME $HADOOP_HOME/etc/hadoop

RUN echo "" /etc/apt/sources.list && tee /etc/apt/sources.list <<-'EOF'\
deb http://mirrors.hust.edu.cn/ubuntu/ xenial main restricted universe multiverse\
deb http://mirrors.hust.edu.cn/ubuntu/ xenial-security main restricted universe multiverse\
deb http://mirrors.hust.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse\
deb http://mirrors.hust.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse\
deb http://mirrors.hust.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse\
deb-src http://mirrors.hust.edu.cn/ubuntu/ xenial main restricted universe multiverse\
deb-src http://mirrors.hust.edu.cn/ubuntu/ xenial-security main restricted universe multiverse\
deb-src http://mirrors.hust.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse\
deb-src http://mirrors.hust.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse\
deb-src http://mirrors.hust.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse\
EOF

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
