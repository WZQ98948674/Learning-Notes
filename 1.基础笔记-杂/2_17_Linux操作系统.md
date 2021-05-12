### Linux操作系统

1. 常用目录 : 
   1.  /  : 根目录
   2.  ~ : 
      - root用户 : /root目录
      - 普通用户 : /home目录
   3. usr : 安装软件的目录
   
2. 常用命令 : 
   1.  查看文件列表 : 
      1. ls  :  查看文件及文件夹名称
      2. ls -a : 查看所有文件及文件夹名称(包含隐藏文件)
      3. ls -l : 查看文件及文件夹详情  , 等同于 ll
      
   2. 切换目录 : 
      1. cd a/  : 切换a目录
      2. cd ..   : 退回上一级
      3. cd -    : 返回之前的目录
      4. pwd   : 查看当前所在目录
      
   3. 文件夹操作 : 
      1. mkdir  aaa : 创建目录
      2. redir  aaa   : 删除目录
      3. mkdirs  -p  aaa/bbb  :  创建多级目录
      
   4. 文件查看 : 
      1. cat  a.log : 查看文件的所有内容
      2. more  a.log : 只查看一个屏幕的文件内容     q (ctrl+c):退出    回车:下一行   空格:下一页
      3. less  a.log   :  和more类似    新增通过键盘上下键查看日志
      4. tail  -200  a.log : 查看最后200行
      5. tail -f  a.log : 动态查看日志文件
      
   5. 文件操作 : 
      
      1.  touch  a.txt     :  创建一个空文件
      2.  cp  a.log     aaa/a.txt    :  把当前目录下的a.log拷贝到aaa目录下的a.txt中
      3.  mv  a.txt  ../bb/b.txt    :  把当前目录下的a.txt剪切到bb目录下的b.txt
      4.  rm  a.txt   :删除文件
      5.  rm -r  bb  : 删除文件夹
      6.  rm -rf  bb : 删除文件/文件夹 , 不询问
      
   6.  打包/压缩/解压缩

       - linux系统下的压缩文件格式 :  a.tar.gz
         - -c : 创建一个新的tar文件
         - -v : 显示运行过程
         - -f : 指定文件名
         - -z : 压缩
         - -t : 查看压缩文件内容
         - -x : 解压

       1.  tar  -cvf  xxx.tar  ./*            : 打包
       2.  tar -zcvf  xxx.tar.gz   ./*     : 打包并压缩
       3.  tar  -xvf  xx.tar                    : 解压
       4.  tar  -zxvf  xxx.tar.gz  -C  /usr/local   :  解压到local文件夹下

   7.  查找文件 : 

       1.  find /  -name  abc*.log    : 查找文件 : /   --查找的路径   -name -- 按名称查找

   8.  查找文件内容 : 

       1.  grep  abc    /usr/local/a.txt   --color -A1  -B1 
           - abc : 要查找的内容
           - /usr/local/a.txt : 要查找的文件
           - --color : 高亮显示 
           - -A1   :  显示后一行
           - -B1   :  显示前一行

3. vi / vim 编辑器 : 查看并修改文件内容

   - 命令行模式 ,  插入模式  , 底行模式
     1. vim  a.txt  : 进入文件 , 默认是命令行模式
     2.  i    :  命令行模式下 , 按 i 进入插入模式
     3.  :    :  命令行模式下 , 按 :  进入底行模式
        1. /abc : 将文件内的abc高亮显示
        2. wq : 保存并退出
        3. q!   : 不保存 , 强制退出

4. 重定向输出

   1.   '>'   :  cat  aa.txt  > bb.txt      :  将aa.txt的内容覆盖输出到bb.txt
   2.  '>>'  :  cat aa.txt  >>  bb.txt    : 将aa.txt的内容追加输出到bb.txt

5. 进程管理

   1. ps -ef : 查看所有进程
   2. ps -ef | grep  java  : 查看带有java的进程
   3. kill  -9  2868  :  强制杀死进程id为2868的进程 , 不带-9 为不强制

6. 管道  : 

   1.  '|' : 管道前的输出作为管道的输入

7. 文件权限 : 

   - 十位  :      -   ---    ---    ---

   1.  第一位代表文件类型
      1. '-'  :  文件
      2. d : 文件夹
      3. l  : 连接
   2. 第2-4位代表当前用户对该文件的权限 :
      1. r  : read         =4
      2. w：write       =2
      3. x  : execute   =1
   3. 第5-7位代表当前用户所在的组对该文件的权限:
   4. 第8-10位代表其他组对该文件的权限 :
   - 修改文件权限 : 
     - chmod  755  a.txt
   
8. 网络

   1. 查看IP地址 : ipconfig
   2. 修改静态IP配置: vim /etc/sysconfig/network-scripts/ipcfg-eth0
      1. 修改ONBOOT=yes
      2. 修改BOOTPROTO=static
      3. 新增IPADDR,NETMASK,NETWORK,BROADCAST
   3. 域名映射 
      1. /etc/hosts  相当于windows下的hosts文件
   4. 网络服务管理 
      1. service  network  restart
   5. 防火墙
      1. service iptables status
