FROM codenvy/ubuntu_jdk8

USER root

ENV HADOOP_HOME=/usr/lib/hadoop-2.7.2
ENV HADOOP_CONFIG_HOME=$HADOOP_HOME/etc/hadoop
ENV PATH=$PATH:$HADOOP_HOME/bin
ENV PATH=$PATH:$HADOOP_HOME/sbin

RUN cd "/usr/lib" && \
    wget -q "http://mirrors.sonic.net/apache/hadoop/common/hadoop-2.7.2/hadoop-2.7.2.tar.gz" && \
    tar -xzf "hadoop-2.7.2.tar.gz" && \
    rm "hadoop-2.7.2.tar.gz" && \
    ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa && \
    cat ~/.ssh/id_dsa >> ~/.ssh/authorized_keys && \
    sed -i 's/export JAVA_HOME=\${JAVA_HOME}/export JAVA_HOME=\/usr\/lib\/jvm\/java-8-oracle/' $HADOOP_CONFIG_HOME/hadoop-env.sh

COPY core-site.xml $HADOOP_CONFIG_HOME/core-site.xml
COPY hdfs-site.xml $HADOOP_CONFIG_HOME/hdfs-site.xml
COPY mapred-site.xml $HADOOP_CONFIG_HOME/mapred-site.xml
COPY slaves $HADOOP_CONFIG_HOME/slaves

VOLUME ["/data"]
RUN mkdir /data/tmp /data/namenode /data/datanode

WORKDIR "${HADOOP_HOME}"
