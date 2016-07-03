# Copyright (c) 2012-2016 Codenvy, S.A.
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
# Contributors:
# Codenvy, S.A. - initial API and implementation

FROM alpine:3.3

# Here we use several hacks collected from https://github.com/gliderlabs/docker-alpine/issues/11:
# 1. install GLibc (which is not the cleanest solution at all)
# 2. hotfix /etc/nsswitch.conf, which is apperently required by glibc and is not used in Alpine Linux

ENV JAVA_VERSION=8 \
    JAVA_UPDATE=72 \
    JAVA_BUILD=15 \
    JAVA_HOME="/usr/lib/jvm/default-jvm" \
    HADOOP_HOME="/usr/lib/hadoop" \
    HADOOP_CONFIG_HOME="$HADOOP_HOME/etc/hadoop"

ENV PATH $JAVA_HOME/bin:$M2_HOME/bin:$PATH

RUN apk add --no-cache --virtual=build-dependencies wget ca-certificates && \
    export ALPINE_GLIBC_BASE_URL="https://circle-artifacts.com/gh/andyshinn/alpine-pkg-glibc/6/artifacts/0/home/ubuntu/alpine-pkg-glibc/packages/x86_64" && \
    export ALPINE_GLIBC_PACKAGE="glibc-2.21-r2.apk" && \
    export ALPINE_GLIBC_BIN_PACKAGE="glibc-bin-2.21-r2.apk" && \
    wget "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE" "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_BIN_PACKAGE" && \
    apk add --no-cache --allow-untrusted "$ALPINE_GLIBC_PACKAGE" "$ALPINE_GLIBC_BIN_PACKAGE" && \
    /usr/glibc/usr/bin/ldconfig "/lib" "/usr/glibc/usr/lib" && \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf && \
    rm "$ALPINE_GLIBC_PACKAGE" "$ALPINE_GLIBC_BIN_PACKAGE" && \
    apk add --update --no-cache openssh curl sudo unzip bash ca-certificates && \
    cd "/tmp" && \
    wget -q --header "Cookie: oraclelicense=accept-securebackup-cookie;" \
        "http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}u${JAVA_UPDATE}-b${JAVA_BUILD}/jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    tar -xzf "jdk-${JAVA_VERSION}u${JAVA_UPDATE}-linux-x64.tar.gz" && \
    mkdir -p "/usr/lib/jvm" && \
    mv "/tmp/jdk1.${JAVA_VERSION}.0_${JAVA_UPDATE}" "/usr/lib/jvm/java-${JAVA_VERSION}-oracle" && \
    ln -s "java-${JAVA_VERSION}-oracle" "$JAVA_HOME" && \
    ln -s "$JAVA_HOME/bin/"* "/usr/bin/" && \
    rm -rf "$JAVA_HOME/"*src.zip && \
    wget -q "http://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-2.7.2/hadoop-2.7.2.tar.gz" && \
    tar -xzf "hadoop-2.7.2.tar.gz" && \
    mv "/tmp/hadoop-2.7.2" "/usr/lib" && \
    sed -i 's/export JAVA_HOME=\${JAVA_HOME}/export JAVA_HOME=${JAVA_HOME}' $HADOOP_CONFIG_HOME/hadoop-env.sh && \
    rm "/tmp/"* && \
    deluser svn && \
    adduser -S user -h /home/user -s /bin/bash -G root -u 1000 && \
    echo "%root ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    PASS=$(openssl rand -base64 32) && \
    echo -e "${PASS}\n${PASS}" | passwd user && \
    chown -R user /home/user/


COPY core-site.xml ${HADOOP_CONFIG_HOME}/core-site.xml
COPY hdfs-site.xml ${$HADOOP_CONFIG_HOME}/hdfs-site.xml
COPY mapred-site.xml ${$HADOOP_CONFIG_HOME}/mapred-site.xml
COPY slaves ${$HADOOP_CONFIG_HOME}/slaves

EXPOSE 22

USER user

VOLUME ["/data"]

RUN mkdir /data/tmp /data/namenode /data/datanode

WORKDIR /usr/lib/hadoop-2.7.2

CMD sudo /usr/bin/ssh-keygen -A && \
    sudo /usr/sbin/sshd -D && \
    tail -f /dev/null
