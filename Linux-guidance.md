# Linux 常用命令

## 文件和目录管理

```
pwd  //显示用户当前所处的工作目录

ls     //列出目录及文件名
ls -lh  // 更详细的显示

cd /  //切换到根目录
cd .   // 切换到当前目录
cd ..  // 切换到上一级目录
cd -   // 切换到上次所在目录

mkdir  dir  // 创建一个新的目录
touch file  //创建新文件 

mv file1 file2  // 移动或重命名文件
cp [-rf] file1 file2  //复制文件
rm [-rf] file1 file2  //删除文件
   -r  递归，用于复制或移动目录
   -f  强制
   
//文件查找
find . -name *.txt  //将目前目录及其子目录下所有后缀名为txt的文件列出来
find . -type f   //将目前目录及其下子目录中所有一般文件列出

tar -cvf demo.tar *.jpg    //将当前目录的所有jpg打包为demo.tar
tar -xvf demo.tar -C ./tmp   //解压到指定文件夹

zip demo.zip *.jpg //将当前目录的所有jpg压缩为demo.zip
unzip -d ./tmp demo.zip  //解压到指定文件夹
```



## 文件查看

```
cat [-bn] file  //由第一行开始显示文件内容
tac [-bn] file  // 从最后一行开始显示
    -b 列出行号，仅针对非空白行做行号显示
    -n 列印出行号，连同空白行也会有行号
    
head [-n] file // 取出文件前面n行
tail [-n] file // 取出文件后面面n行
```

[vim使用简介](http://c.biancheng.net/linux_tutorial/40/)

## 权限管理

```
chmod  //更改文件9个属性
chow  //更改文件属主，也可以同时更改文件属组
```

[参考链接](http://c.biancheng.net/view/761.html)

## 系统管理

```
df h    //磁盘使用情况统计
du -h   //显示目录或文件的大小
free -m //以MB为单位显示内存使用情况

top   //实时显示进程的动态

ps [options]  //显示进程信息
   -aux 显示所有包含其他使用者的行程
   -ef 显示所有命令，连带命令行

# kill -9 pid  //彻底杀死进程
```

## 高级技巧

### 重定向输入输出

<img src=".\image\linux重定向.png"  />



### grep命令

**grep [-option] pattern file**

- -c	仅列出文件中包含模式的行数。
- -i	忽略模式中的字母大小写。
- -l	列出带有匹配行的文件名。
- -n	在每一行的最前面列出行号。
- -v	列出没有匹配模式的行。

```
grep test *.txt //在当前目录中，查找后缀名为txt的文件中包含 test 字符串的文件，并打印出该字符串的行
grep -c [^cd] *.txt   //分别统计各txt文件以cd开头的行数
```



### awk命令

**awk [选项] '脚本命令' 文件名**

- -F  指定以 fs 作为输入行的分隔符，awk 命令默认分隔符为空格或制表符。- 
- -f  从脚本文件中读取 awk 脚本指令，以取代直接在命令行中输入指令。
- -v  在执行处理过程之前，设置一个变量 var，并给其设备初始值为 val。

*脚本命令=匹配规则{执行命令},在 awk 程序执行时，如果没有指定执行命令，则默认会把匹配的行输出；如果不指定匹配规则，则默认匹配文本中所有的行*

```
//遇到一个空行便输出一个Blank line
awk '/^$/ {print "Blank line"}' test.txt 

//每行按空格或TAB分割，输出文本中的1、4项
awk '{print $1,$4}' log.txt

hecho "My name is Rich" | awk '{$4="Christine"; print $0}'
>>My name is Christine
```



### xargs命令

xargs 是给命令传递参数的一个过滤器，也是组合多个命令的一个工具可以将管道或标准输入（stdin）数据转换成命令行参数，也能够从文件的输出中读取数据, 一般是和管道一起使用。

**somecommand |xargs -item  command**

- -n   表示命令在执行的时候一次用的argument的个数，默认是用所有的

- -d   默认的xargs分隔符是回车，argument的分隔符是空格，这里修改的是xargs的分隔符

```
echo "nameXnameXnameXname" | xargs -dX -n2
>>name name
  name name
  
//假如你有一个文件包含了很多你希望下载的 URL，你能够使用 xargs下载所有链接
cat url-list.txt | xargs wget -c

// 杀死关于python的所有进程
ps -ef|grep python |awk '{print $2}' |xargs kill -9

//从根目录开始查找所有扩展名为 .log 的文件，并找出包含ERROR 的行：
find / -type f -name *.log | xargs grep ERROR
```



### nohup命令

**nohup Command [ Arg … ] [　& ]**

nohup 英文全称 no hang up（不挂起），用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行

```
//后台执行root目录下的 runoob.sh 脚本
nohup /root/test.sh &  

//后台执行 root 目录下的 runoob.sh 脚本，并重定向输入到 runoob.log 文件
nohup /root/test.sh > runoob.log 2>&1 & 
```

