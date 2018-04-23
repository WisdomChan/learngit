# CTP国内期货行情收集程序部署
-------------------
## 1.需要的离线安装包
* boost_1_62_0.7z
* protobuf-2.5.0.tar.gz
* zeromq-4.2.3.tar.gz

## 2.安装必要文件
### （1）安装boost
#### 　　　依次执行以下命令：
          7za x boost_1_62_0.7z
          cd boost_1_62_0
          ./bootstrap.sh --with-libraries=all --with-toolset=gcc
          ./b2 toolset=gcc
          ./b2 install
          ldconfig –v

### （2）安装google protobuf
#### 　　　依次执行以下命令：
          tar –xvf protobuf-2.5.0.tar.gz
          cd protobuf-2.5.0
          ./configure
          make
          make check
          make install
          ldconfig

### （3）安装zeromq
#### 　　　依次执行以下命令：
          tar –xvf zeromq-4.2.3.tar.gz
          cd zeromq-4.2.3
          ./autogen.sh
          ./configure
          make
          make check
          make install
          ldconfig

### （4）编译CTP国内期货行情收集程序
          cd ctpfuture
          mkdir allinst
          make

### （5）修改getinstruments.sh
#### 　　　修改ctp_future_recv_sender路径
        /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ctp_future_recv_sender t
        
### （6）修改getinstruments.sh
#### 　　　修改ctp_future_recv_sender路径
        /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ctp_future_recv_sender

### （7）修改crontab
#### 　　　向crontab添加以下语句（注意修改路径）
          46 08 * * 1-5 cd /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ && ./getinstruments.sh
          48 08 * * 1-5 cd /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ && ./recvquotes.sh
          30 15 * * * kill -9 `pgrep recvquotes`
          30 15 * * * kill -9 `pgrep ctp_future_recv`
          46 20 * * 1-5 cd /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ && ./getinstruments.sh
          48 20 * * 1-5 cd /home/zhibin/livetrading/backoffice/recvquotes/ctpfuture/ && ./recvquotes.sh
          30 3 * * * kill -9 `pgrep recvquotes`
          30 3 * * * kill -9 `pgrep ctp_future_recv`

## 3.行情收集保存路径
### （1）现在保存路径
          /mnt/hdd1/F/tickdata/recvquotes/
### （2）修改保存路径
          修改commonlibs/cpp/config/recvquotes.ini中的PATH来达到修改保存路径的目的
