# sedUse
linux sed 使用总结



#### sed 优点 ：一行一行处理（可以处理较大的文件）



特点：

1自动打印（处理一行，打印一行）

2.如果后面不接文件参数，意味着需要标准输入



#### 格式：

~~~shell
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
~~~



~~~shell
 -n, --quiet, --silent   (关闭自动打印)
                 suppress automatic printing of pattern space
  -e script, --expression=script   (多脚本查询 （或者的关系）)
                 add the script to the commands to be executed
  -f script-file, --file=script-file
                 add the contents of script-file to the commands to be executed
  --follow-symlinks
                 follow symlinks when processing in place
  -i[SUFFIX], --in-place[=SUFFIX]  （修改文件 ，事先备份）
                 edit files in place (makes backup if SUFFIX supplied)
  -c, --copy
                 use copy instead of rename when shuffling files in -i mode
  -b, --binary
                 does nothing; for compatibility with WIN32/CYGWIN/MSDOS/EMX (
                 open files in binary mode (CR+LFs are not treated specially))
  -l N, --line-length=N
                 specify the desired line-wrap length for the `l' command
  --posix
                 disable all GNU extensions.
  -E, -r, --regexp-extended  (使用扩展的正则表达式)
                 use extended regular expressions in the script
                 (for portability use POSIX -E).
  -s, --separate
                 consider files as separate rather than as a single,
                 continuous long stream.
      --sandbox
                 operate in sandbox mode (disable e/r/w commands).
  -u, --unbuffered
                 load minimal amounts of data from the input files and flush
                 the output buffers more often
  -z, --null-data
                 separate lines by NUL characters
  --help
                 display this help and exit
  --version
                 output version information and exit
~~~



**scrip格式：**

地址+ 指令



**指令：**

~~~
P ： 自动打印
d :  删除特定的行
a : 在特定行的后面追加
i : 在特定行的前面插入
c : 替换特定行
！ : 取反
~~~



指定要查找的行

~~~
[root@VM-20-16-opencloudos ~]# sed -n '3p'  /etc/fstab
# Created by anaconda
~~~



双斜线 ： 指定要查找的内容

~~~
[root@VM-20-16-opencloudos ~]# ifconfig eth0 |sed -n '/netmask/p'
        inet 10.2.20.16  netmask 255.255.252.0  broadcast 10.2.23.255

~~~



**查找的格式  sed  '//,//p'** 

查找 16:20 到 16:30 之间的日志

~~~
[root@VM-20-16-opencloudos ~]# sed -n '/Aug  9 16:20:/,/Aug  9 16:30:/p' /var/log/messages
Aug  9 16:20:01 VM-20-16-opencloudos systemd[1]: Starting system activity accounting tool...
Aug  9 16:20:01 VM-20-16-opencloudos systemd[1]: sysstat-collect.service: Succeeded.
Aug  9 16:20:01 VM-20-16-opencloudos systemd[1]: Started system activity accounting tool.
Aug  9 16:20:01 VM-20-16-opencloudos systemd[1]: Started Session 18518 of user root.
Aug  9 16:20:01 VM-20-16-opencloudos systemd[1]: session-18518.scope: Succeeded.
Aug  9 16:25:01 VM-20-16-opencloudos systemd[1]: Started Session 18519 of user root.
Aug  9 16:25:01 VM-20-16-opencloudos systemd[1]: session-18519.scope: Succeeded.
Aug  9 16:30:01 VM-20-16-opencloudos systemd[1]: Starting system activity accounting tool...

~~~



步进

从第一行开始，每次打印2行

~~~
[root@VM-20-16-opencloudos ~]# seq 10 | sed -n '1~2p'
1
3
5
7
9
~~~



不显示以#开头的行

~~~
[root@VM-20-16-opencloudos ~]# sed '/^#/d' testfstab 
UUID=c07baf5f-3867-45e4-b4e5-1d6e374a8caa                /                       ext4    noatime,acl,user_xattr  1 1

~~~



删除#开头的行

~~~shell
[root@VM-20-16-opencloudos ~]# sed -i.bak '/^#/d' testfstab
~~~



 **搜索替换**

sed  's/parttern/string/范围'

~~~
[root@VM-20-16-opencloudos ~]# echo abc1233ccc  | sed 's/abc/efg/'
efg1233ccc
~~~



在abc 123之中追加文本

~~~
[root@VM-20-16-opencloudos ~]# echo abc123xyz  | sed -E 's/(abc)(123)/\1666\2/'
abc666123xyz
~~~



在文本最后追加字符 （& 表示前面搜索出的内容）

~~~
[root@VM-20-16-opencloudos ~]# echo abc123xyz  | sed -E 's/.*/&666/'
abc123xyz666
~~~



替换文本中的id 为 当前用户的uid

~~~
sed -Eni "s/^number ([0-9]+)/number `id -u`/p" test.txt
~~~































