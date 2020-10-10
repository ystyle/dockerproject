FROM ubuntu:20.04 AS build-env

# Set your hardware
ENV HARDWARE=wifiiot
# Prevent interactive
ENV DEBIAN_FRONTEND=noninteractive

# Setting up the build environment
RUN sed -i 's/archive.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list && \
    apt-get clean -y && \
    apt-get -y update && \
    apt-get remove python* -y && \
    apt-get install git curl build-essential libdbus-glib-1-dev libgirepository1.0-dev -y && \
    apt-get install zip libncurses5-dev pkg-config -y && \
    apt-get install python3-pip -y && \
    apt-get install scons dosfstools mtools -y && \
    rm -rf /var/lib/apt/lists/*

# Setup python
# Make sure python install on the right python version path
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1 && \
    pip3 install --upgrade pip -i https://mirrors.aliyun.com/pypi/simple && \
    pip3 install ninja kconfiglib pycryptodome ecdsa -i https://mirrors.aliyun.com/pypi/simple && \
    pip3 install six --upgrade --ignore-installed six -i https://pypi.tuna.tsinghua.edu.cn/simple && \
    rm -rf /var/cache/apt/archives

#Fix Dash
RUN rm -rf /bin/sh && \
    ln -s /bin/bash /bin/sh

#Setup gn
ENV PATH /tools/gn:$PATH
RUN mkdir /tools && \
    cd /tools && \
    curl -LO http://tools.harmonyos.com/mirrors/gn/1523/linux/gn.1523.tar && \
    tar xvf /tools/gn.1523.tar && \
    rm -rf /tools/gn.1523.tar

#Setup LLVM
#ADD ./llvm-linux-9.0.0-34042.tar /tools
ENV PATH /tools/llvm/bin:$PATH
RUN cd /tools && \
    curl -LO http://tools.harmonyos.com/mirrors/clang/9.0.0-34042/linux/llvm-linux-9.0.0-34042.tar && \
    tar xvf /tools/llvm-linux-9.0.0-34042.tar && \
    rm -rf /tools/llvm-linux-9.0.0-34042.tar

#Setup hc-gen
ENV PATH /tools/hc-gen:$PATH
RUN cd /tools && \
    curl -LO http://tools.harmonyos.com/mirrors/hc-gen/0.64/linux/hc-gen-0.64-linux.tar && \
    tar xvf /tools/hc-gen-0.64-linux.tar && \
    rm -rf /tools/hc-gen-0.64-linux.tar

#Setup gcc_riscv32
ENV PATH /tools/gcc_riscv32/bin:$PATH
RUN cd /tools && \
    curl -LO http://tools.harmonyos.com/mirrors/gcc_riscv32/7.3.0/linux/gcc_riscv32-linux-7.3.0.tar.gz && \
    tar xvf /tools/gcc_riscv32-linux-7.3.0.tar.gz && \
    rm -rf /tools/gcc_riscv32-linux-7.3.0.tar.gz

#Create work dir
RUN mkdir /OpenHarmony
WORKDIR /OpenHarmony

# Gitee Repo tool and download
# Make sure requests install at the right location
RUN curl https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/bin/repo && \
    chmod a+x /usr/bin/repo && \
    pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple requests

#Download source, update to your info
RUN git config --global user.email "lxy5266@live.com" && \
    git config --global user.name "ystyle" && \
    git config --global color.ui false && \
    git config --global credential.helper store && \
    repo init -u https://gitee.com/openharmony/manifest.git -b master --repo-branch=stable --no-repo-verify && \
    repo sync -c

# compile
ENV LANGUAGE en
ENV LANG en_US.utf-8
RUN export|grep LANG
CMD ["/bin/bash", "-c", "python build.py ${HARDWARE} -b debug"]
