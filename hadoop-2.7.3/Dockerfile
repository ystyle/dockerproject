FROM ibmjava:8-sdk-alpine

MAINTAINER ystyle <lxy5266@live.com> (@ystyle)

RUN apk --update add --no-cache ca-certificates curl bash \
    && curl -L http://mirrors.shu.edu.cn/apache/hadoop/common/hadoop-2.7.5/hadoop-2.7.5.tar.gz > /tmp/hadoop.tar.gz \
    && mkdir -p /opt \
    && tar -zxf /tmp/hadoop.tar.gz -C /opt \
    && apk del curl \
    && mv /opt/hadoop-2.7.5 /opt/hadoop \
    && rm -rf /tmp/* /var/cache/apk/* \
    && mkdir -p /data/temp /data/dfs/name /data/dfs/data \    
    && sed -i '19a<property>' /opt/hadoop/etc/hadoop/core-site.xml \
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
    && sed -i '30a</property>' /opt/hadoop/etc/hadoop/hdfs-site.xml
    
# HDFS
EXPOSE 50010 50020 50070 50075 50090

CMD /opt/hadoop/sbin/start-dfs.sh
