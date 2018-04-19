# 功夫交易系统部署
-------------------
## 1.需要的离线安装包
> boost_1_62_0.7z
> cmake-3.11.0-rc4.tar.gz
> log4cplus-2.0.0-rc2.zip
> rfoo
## 2.安装必要文件
### （1）安装cmake
#### 　　　(a)依次执行以下命令：
          tar xvf cmake-3.11.0-rc4.tar.gz
          cd cmake-3.11.0-rc4
          ./configure
          make
          make install
#### 　　　(b)修改PATH环境变量
           vim ~/.bashrc
           在最后一行添上：
           export PATH=/usr/local/bin:$PATH
       export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
### （2）安装boost
#### 　　　依次执行以下命令：
          7za x boost_1_62_0.7z
          cd boost_1_62_0
          ./bootstrap.sh --with-libraries=all --with-toolset=gcc
          ./b2 toolset=gcc
          ./b2 install
          ldconfig –v
### （3）安装log4cplus
#### 　　　依次执行以下命令：
          unzip log4cplus-2.0.0-rc2.zip
          cd log4cplus-2.0.0-rc2
          ./configure
          make
          make install
### （4）安装rfoo
#### 　　　依次执行以下命令：
          cd rfoo
          python setup.py install
### （5）安装pid
#### 　　　执行以下命令：
          pip install pid
### （6）安装supervisor
#### 　　　执行以下命令：
          pip install supervisor
## 3.编译与安装功夫交易系统
### （1）获取代码
          git clone https://github.com/zrdfsinvest/kungfu.git
### （2）编译
#### 　　　(a)依次执行以下命令：
          cd kungfu
          mkdir build
          cd build
          cmake ..
#### 　　　(b)修改cmake_install.cmake
          vim cmake_install.cmake
          将第41行内容
          file(INSTALL DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/boost" TYPE DIRECTORY FILES "/opt/kungfu/toolchain/boost-1.62.0/lib/")
      修改为：
      file(INSTALL DESTINATION "${CMAKE_INSTALL_PREFIX}/lib/boost" TYPE DIRECTORY FILES "/usr/local/lib/")
#### 　　　(c)执行以下命令：
          make
#### 　　　(d)修改CPackConfig.cmake
          vim CPackConfig.cmake
          将67行内容
          set(CPACK_RPM_PACKAGE_REQUIRES "rfoo >= 1.3.1, pid >= 2.1.1, log4cplus2 == 2.0.0_RC1, supervisor >= 3.1.0")
          修改为：
          #set(CPACK_RPM_PACKAGE_REQUIRES "rfoo >= 1.3.1, pid >= 2.1.1, log4cplus2 == 2.0.0_RC1, supervisor >= 3.1.0")
### （3）生成安装包并安装
#### 　　　依次执行以下命令：
          make package
          rpm -ivh kungfu-0.0.5-Linux.rpm
## 4.测试是否成功安装功夫交易系统
#### 　　　依次执行以下命令：
          sudo systemctl start kungfu
          kungfuctl