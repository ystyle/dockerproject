FROM centos:5.11
MAINTAINER ystyle "lxy5266@live.com"
RUN yum -y install automake libtool libreadline-dev zlib1g-dev uuid-dev libgoogle-perftools-dev gcc-c++ wget && \
    wget http://centos.ustc.edu.cn/epel/5/x86_64/epel-release-5-4.noarch.rpm && \
    wget http://rpms.famillecollet.com/enterprise/5/remi/x86_64/remi-release-5.9-1.el5.remi.noarch.rpm && \
    rpm -ivh *.rpm && \
    yum -y --enablerepo=remi install mysql mysql-server mysql.x86_64 mysql-devel.x86_64 && \
    yum -y install make automake zlib-devel readline-devel ncurses-devel.x86_64 ncurses.x86_64 subversion && \
    svn checkout -r 18 http://code.taobao.org/svn/tb-common-utils/trunk/ ~/tb-common-utils && \
    export TBLIB_ROOT=/usr/local/tb/lib && \
    cd ~/tb-common-utils/ && \
    chmod +x *.sh && \
    ./build.sh && \
    svn checkout http://code.taobao.org/svn/tfs/tags/release-2.2.10 ~/tfs-2.2.10 && \
    export TBLIB_ROOT=/usr/local/tb/lib && \
    cd ~/tfs-2.2.10/ && chmod +x *.sh && \
    ./build.sh init && \
    ./configure --prefix=/usr/local/tfs --without-tcmalloc && \
    make && make install
