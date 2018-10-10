FROM    orbweb/buildpack-deps:arm32v7-jessie-curl

RUN     apt-get update && mkdir -p /opt/src
RUN     apt-get install -y binutils

RUN     apt-get install -y libacl1-dev

RUN     apt-get install -y --no-install-recommends ca-certificates \
        curl wget bzr git mercurial openssh-client subversion autoconf \
        automake bzip2 file g++ gcc imagemagick libbz2-dev libc6-dev \
        libcurl4-openssl-dev libevent-dev libffi-dev libglib2.0-dev libjpeg-dev \
        liblzma-dev libmagickcore-dev libmagickwand-dev libmysqlclient-dev \
        libncurses-dev libpq-dev libreadline-dev libsqlite3-dev libssl-dev libtool \
        libxml2-dev libxslt-dev libyaml-dev make tar patch xz-utils zlibc zlib1g-dev

# install opencv
RUN     apt-get install -y build-essential cmake
RUN     mkdir -p /opt/src/opencv && cd /opt/src/opencv && \
        curl -fSL https://github.com/Itseez/opencv/archive/2.4.13.6.tar.gz | tar xvz --strip-components=1
RUN     mkdir -p /opt/src/opencv/release && cd /opt/src/opencv/release && \
        cmake -Wno-dev -DENABLE_PRECOMPILED_HEADERS=OFF -DCMAKE_BUILD_TYPE=RELEASE -DBUILD_SHARED_LIBS=NO -DCMAKE_INSTALL_PREFIX=/usr/local -DBUILD_opencv_python=ON .. && \
        make && make install

# install python
ENV     LANG=C.UTF-8
ENV     PYTHON_VERSION=2.7.11
ENV     PYTHON_PIP_VERSION=8.1
RUN     apt-get purge -y python.*
RUN     set -x && \
        mkdir -p /usr/src/python && \
        wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tar.xz -O python.tar.xz && \
        tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz && \
        rm python.tar.xz* && \
        cd /usr/src/python && \
        ./configure --enable-shared --enable-unicode=ucs4 && \
        make -j$(nproc) && \
        make install && \
        ldconfig

# install pip & dep
RUN     curl -fSL 'https://bootstrap.pypa.io/get-pip.py' | python2 && pip install --no-cache-dir --upgrade pip==$PYTHON_PIP_VERSION
RUN     pip install numpy==1.10.2

# UPX(the Ultimate Packer for eXecutables)
RUN     apt-get install -y upx

# ACL feature
RUN     apt-get install -y acl