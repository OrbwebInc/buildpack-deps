FROM        buildpack-deps:squeeze
MAINTAINER  Orbweb Inc. <devs@orbweb.com>
RUN         apt-get update && mkdir -p /opt/src
RUN         apt-get install -y libc6-dev-i386

RUN         apt-get remove -y cmake && \
            mkdir -p /etc/cmake && cd /etc/cmake && curl -fSL https://cmake.org/files/v3.4/cmake-3.4.1-Linux-x86_64.tar.gz | tar xvz --strip-components=1 && cp bin/* /usr/bin/. && cp -R share/cmake-3.4 /usr/share/. && rm -rf /etc/cmake

RUN         apt-get remove -y yasm && \
            mkdir -p /etc/yasm && cd /etc/yasm && curl -fSL http://www.tortall.net/projects/yasm/releases/yasm-1.3.0.tar.gz | tar xvz --strip-components=1 && ./configure && make -j 4 && make install && rm -rf /etc/yasm

RUN         apt-get remove -y gettext && \
            mkdir -p /etc/gettext && cd /etc/gettext && curl -fSL https://ftp.gnu.org/pub/gnu/gettext/gettext-0.19.tar.gz | tar xvz --strip-components=1 && ./configure && make && make install && rm -rf /etc/gettext

RUN         apt-get purge -y python.*
ENV         LANG C.UTF-8

RUN         gpg --keyserver ha.pool.sks-keyservers.net --recv-keys C01E1CAD5EA2C4F0B8E3571504C367C218ADD4FF
ENV         PYTHON_VERSION 2.7.11
ENV         PYTHON_PIP_VERSION 7.1.2

RUN         set -x && \
            mkdir -p /usr/src/python && curl -fSL "https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz" -o python.tar.xz && curl -fSL "https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tar.xz.asc" -o python.tar.xz.asc && gpg --verify python.tar.xz.asc && tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz && rm python.tar.xz* && cd /usr/src/python && ./configure --enable-shared --enable-unicode=ucs4 && make -j$(nproc) && make install && ldconfig && curl -fSL 'https://bootstrap.pypa.io/get-pip.py' | python2 && pip install --no-cache-dir --upgrade pip==$PYTHON_PIP_VERSION && find /usr/local \( -type d -a -name test -o -name tests \) -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) -exec rm -rf '{}' + && rm -rf /usr/src/python

RUN         pip install numpy==1.10.2

RUN         mkdir -p /opt/src/opencv && \
            cd /opt/src/opencv && curl -fSL https://github.com/Itseez/opencv/archive/2.4.12.3.tar.gz | tar xvz --strip-components=1 && cmake -Wno-dev -D CMAKE_BUILD_TYPE=RELEASE -D BUILD_SHARED_LIBS=NO -D BUILD_opencv_python=ON . && make && make install

RUN         apt-get install -y upx