# 从源码安装Go

这俩月来，迷上了Go语言，也看了不少运行时的源码，为了更好的了解Go的底层原理，决定从源码编译一把，这样就能够在源码关键的地方自己输出一些信息，辅助理解底层原理。整个过程还是非常简单的，相比于openJDK，不知道简单了多少。。。

### 编译流程(Mac为例)
1. 下载go1.4版本([点我下载](https://storage.googleapis.com/golang/go1.4-bootstrap-20161024.tar.gz))
2. 将下载的压缩包解压，并放到预想的目录下，假定为 

        /home/go1.4

3. 编译安装go1.4

        cd /home/go1.4/src
        ./make.bash
        
4. 通过git下载go源码

        git clone https://go.googlesource.com/go
        cd go
        git checkout go1.8
        
5. 通过go1.4编译1.8

        cd src
        GOROOT_BOOTSTRAP=/home/go1.4 ./make.bash
        
6. 大功告成，之后就可以自己添加一些代码，然后重复5的编译步骤就可以了，可以用alias偷懒




