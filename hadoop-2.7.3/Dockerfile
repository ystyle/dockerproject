FROM ibmjava:8-sdk-alpine

MAINTAINER ystyle <lxy5266@live.com> (@ystyle)

RUN apk --update add --no-cache ca-certificates curl \
    && HADOOP_VERSION="2.7.3" \
    && curl -Ls http://mirrors.hust.edu.cn/apache/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz > /tmp/hadoop-${HADOOP_VERSION}.tar.gz \
    && mkdir -p /opt/hadoop 
RUN tar -zxvf /tmp/hadoop-${HADOOP_VERSION}.tar.gz -C /opt/hadoop \
    && apk del curl \
    && rm -rf /tmp/* /var/cache/apk/* \
    && mkdir -p /data/temp /data/dfs/name /data/dfs/data 
    
RUN    sed -i '19a<property>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '20a  <name>hadoop.tmp.dir</name>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '21a  <value>file:/data/temp</value>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '22a</property>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '23a<property>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '24a  <name>fs.default.name</name>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '25a  <value>hdfs://0.0.0.0:9000</value>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '26a</property>' /opt/hadoop/etc/hadoop/core-site.xml \
    && sed -i '19a<property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '20a  <name>dfs.replication</name>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '21a  <value>1</value>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '22a</property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '23a<property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '24a  <name>dfs.namenode.name.dir</name>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '25a  <value>file:/data/dfs/name</value>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '26a</property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '27a<property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '28a  <name>dfs.datanode.data.dir</name>    ' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '29a  <value>file:/data/dfs/data</value>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
    && sed -i '30a<property>' /opt/hadoop/etc/hadoop/hdfs-site.xml \
RUN /opt/hadoop/bin/hdfs namenode -format

CMD /opt/hadoop/sbin/start-dfs.sh
