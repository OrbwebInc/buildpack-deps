FROM    buildpack-deps:squeeze

RUN     curl https://cmake.org/files/v3.4/cmake-3.4.1-Linux-x86_64.tar.gz | tar xvz && \
        chmod +x cmake-3.4.1-Linux-x86_64/bin/* && \
        cp cmake-3.4.1-Linux-x86_64/bin/* /usr/bin/. && \
        rm -rf cmake-3.4.1-Linux-x86_64
