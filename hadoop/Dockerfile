FROM codenvy/jdk8_maven3_tomcat8

ENV HADOOP_HOME /usr/lib/hadoop
ENV HADOOP_CONFIG_HOME $HADOOP_HOME/etc/hadoop
ENV PATH $PATH:$HADOOP_HOME/bin
ENV PATH $PATH:$HADOOP_HOME/sbin

ENV PATH $JAVA_HOME/bin:$M2_HOME/bin:$PATH

RUN cd "/tmp"
RUN wget "http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-2.7.2/hadoop-2.7.2.tar.gz"
RUN tar -xzf "hadoop-2.7.2.tar.gz"
RUN mv "/tmp/hadoop-2.7.2" "/usr/lib"
RUN rm "/tmp/"*
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_dsa
RUN cat id_dsa.pub >> authorized_keys
RUN sed -i 's/export JAVA_HOME=\${JAVA_HOME}/export JAVA_HOME=\/usr\/lib\/jvm\/java-8-oracle/' $HADOOP_CONFIG_HOME/hadoop-env.sh

COPY core-site.xml $HADOOP_CONFIG_HOME/core-site.xml
COPY hdfs-site.xml $HADOOP_CONFIG_HOME/hdfs-site.xml
COPY mapred-site.xml $HADOOP_CONFIG_HOME/mapred-site.xml
COPY slaves $HADOOP_CONFIG_HOME/slaves

VOLUME ["/data"]
RUN mkdir /data/tmp /data/namenode /data/datanode

WORKDIR /usr/lib/hadoop
