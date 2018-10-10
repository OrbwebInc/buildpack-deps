# 使用注意
- docker image 區分
    - buildpack-deps :
        - 基本鏡像檔
        - 請使用官方有安裝 curl 的鏡像
            - intel : https://hub.docker.com/_/buildpack-deps/
            - arm : https://hub.docker.com/r/arm32v7/buildpack-deps/
        - 從官方 docker pull 下來後的鏡像(buildpack-deps), 請推送到 `orbweb/buildpack-deps:{tag}`
            - tag 分類請依照 `{architectures}-{versionNickName}`, ex : intel-wheezy-curl
    - orbweb-agent-buildpack-deps : 
        - orbweb-agent 用到的鏡像檔, 請參考各個 dockerfile
        - 此版本為已安裝好會使用到的基本相依工具, ex : python 2.7.10, acl, cmake, c++...
        - 建立好 orbweb-agent 的 base img 後, 請推送到 `orbweb/orbweb-agent-buildpack-deps:{tag}`
            - tag 分類請依照 `{architectures}-{thecus-version}`, ex : intel-thecus-n7770
- 避免未來無法從官方抓取鏡像檔, 請確實執行推送到 docker hub orbweb 的動作. Orbweb 備份位置 : 
    - https://hub.docker.com/r/orbweb/buildpack-deps/
    - https://hub.docker.com/r/orbweb/orbweb-agent-buildpack-deps/

# how to use
1. choose folder
    - intel : `docker build -t orbweb/orbweb-agent-buildpack-deps:intel-thecus-n7770 .`
    - arm : `docker build -t orbweb/orbweb-agent-buildpack-deps:arm-thecus-n2350 .`
2. `docker push orbweb/orbweb-agent-buildpack-deps....`
3. 在 orbweb-agent 的 linux branch 中, 使用建立好的 image

# about thecus
- intel 為 linux6(squeeze), GLIBC 只支援到 2.11 以下
    - 目前 jessie / stretch 的版本, 因為 libz.so.1 中的 GLIBC_2.14 版本過高, 無法放入 thecus-intel 中
    - 若未來 GLIBC 過高問題導致 pyinstaller 編譯後出現 GLIBC 問題, 請使用 `https://github.com/wheybags/glibc_version_header`
- arm, GLIBC 支援到 2.18 以下

# architectures

## intel (Debian)
- use `strings /lib/x86_64-linux-gnu/libc.so.6 | grep GLIBC`

- `wheezy(7)` : 2018 目前使用在 thecus-intel 上可以執行
>
```
GLIBC_2.2.5
GLIBC_2.2.6
GLIBC_2.3
GLIBC_2.3.2
GLIBC_2.3.3
GLIBC_2.3.4
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_PRIVATE
GNU C Library (Debian EGLIBC 2.13-38+deb7u12) stable release version 2.13, by Roland McGrath et al.
```

- sqeeze(6) : 已無法使用

## arm (Debian)
- use `strings /lib/arm-linux-gnueabihf/libc.so.6 | grep GLIBC`

- stretch(9)
>
```
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_2.14
GLIBC_2.15
GLIBC_2.16
GLIBC_2.17
GLIBC_2.18
GLIBC_2.22
GLIBC_2.23
GLIBC_2.24
GNU C Library (Debian GLIBC 2.24-11+deb9u3) stable release version 2.24, by Roland McGrath et al.
```

- jessie(8) : 2018 使用版本
>
```
GLIBC_2.4
GLIBC_2.5
GLIBC_2.6
GLIBC_2.7
GLIBC_2.8
GLIBC_2.9
GLIBC_2.10
GLIBC_2.11
GLIBC_2.12
GLIBC_2.13
GLIBC_2.14
GLIBC_2.15
GLIBC_2.16
GLIBC_2.17
GLIBC_2.18
GLIBC_PRIVATE
GNU C Library (Debian GLIBC 2.19-18+deb8u10) stable release version 2.19, by Roland McGrath et al.
```



