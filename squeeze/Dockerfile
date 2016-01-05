FROM    buildpack-deps:squeeze

RUN     curl https://cmake.org/files/v3.4/cmake-3.4.1-Linux-x86_64.tar.gz | tar xvz && \
        mv cmake-3.4.1-Linux-x86_64/bin/cmake /usr/bin/cmake && \
        chmod +x /usr/bin/cmake && \
        rm -rf cmake-3.4.1-Linux-x86_64
