FROM    buildpack-deps:xenial-curl

RUN     apt-get update && mkdir -p /opt/src
RUN     apt-get install -y libc6-dev-i386


RUN     curl https://cmake.org/files/v3.4/cmake-3.4.1-Linux-x86_64.tar.gz | tar xvz && \
        chmod +x cmake-3.4.1-Linux-x86_64/bin/* && \
        mv cmake-3.4.1-Linux-x86_64 /usr/local/share/
ENV     PATH=$PATH:/usr/local/share/cmake-3.4.1-Linux-x86_64/bin


RUN     apt-get purge -y python.*
ENV     LANG=C.UTF-8
ENV     PYTHON_VERSION=2.7.11
ENV     PYTHON_PIP_VERSION=8.1
RUN     apt-get install -y xz-utils tar make zlibc zlib1g-dev openssl libssl-dev
RUN     set -x && \
        mkdir -p /usr/src/python && \
        wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tar.xz -O python.tar.xz && \
        tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz && \
        rm python.tar.xz* && \
        cd /usr/src/python && \
        ./configure --enable-shared --enable-unicode=ucs4 && \
        make -j$(nproc) && \
        make install && \
        ldconfig && \
        curl -fSL 'https://bootstrap.pypa.io/get-pip.py' | python2 && \
        pip install --no-cache-dir --upgrade pip==$PYTHON_PIP_VERSION && \
        find /usr/local \( -type d -a -name test -o -name tests \) -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) -exec rm -rf '{}' + && \
        cd / && \
        rm -rf /usr/src/python
RUN     pip install numpy==1.10.2


RUN     apt-get install build-essential -y
RUN     mkdir -p /opt/src/opencv && \
        cd /opt/src/opencv && \
        curl -fSL https://github.com/Itseez/opencv/archive/2.4.12.3.tar.gz | tar xvz --strip-components=1 && \
        cmake -Wno-dev -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_SHARED_LIBS=NO -D BUILD_opencv_python=ON . && \
        make && make install


RUN     apt-get install -y upx