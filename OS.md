# OS note

## linux

* 绝对路径和相对路径
  * 绝对路径永远是相对于根文件夹的。标志第一个字符时"/"
  * 相对路径永远是相对于我们所处的文件夹。第一个字符没有"/","."表示当前路径,".."表示上一级目录
* 文件操作命令
  * ls
  * pwd 查看当前路径
  * mkdir 创建目录
  * cd 切换目录
  * cp 复制文件 cp [源路径][目的路径]
  * mv 移动文件 mv [源路径][目的路径]
  * rm 删除文件或目录 -r 递归删除 -f 强制删除
  * rmdir 删除目录
  * cat 查看文件内容
  * more 分页查看 car [文件]|more f/空格显示下一页，回车显示下一行，Q/q 退出
* 权限操作
  * su 切换到root，返回当前用户用exit
  * sudo 在指令前使用sudo，使得本条指令以最高权限运行
  * chmod 使用chmod 命令更改文件权限
  * chown 使用chown命令更改文件所有者
  * chgrp 使用chgrp命令更改文件的所属组
* 网络管理命令
  * Ifconfig 查看当前网络配置
  * Netstat 查看当前网络状态
  * Ping 检查网络是否联通
* 查询命令帮助手册 man命令
  * man 命令
* yum命令
* vi编辑器
  * 光标上下左右移动对应kjhl，部分vi支持上下左右键
  * home移动到行首，end移动到行尾
  * x删除光标所在字符，dd删除光标所在行
  * yy复制当前行，nyy复制自当前行起，包括当前行在内的n行，如3yy
  * p将内容粘贴到光标处
  * u撤掉上一次操作
  * 只有在插入模式下才能修改文本，i或者insert从命令模式进入插入模式
  * 底行模式则是在输入模式下按esc进入。:q退出vi，:w保存修改内容，:wq退出并保存。:q!强制退出。
* gcc编译器
  * 检查版本 gcc -v
  * gcc编译单文件 gcc hello.c -o hello
  * gcc编译多文件 gcc a.c b.c -o test

## 内中断

* 中断信息。中断的意思是指，CPU不再接着（刚执行完的指令）向下执行，而是转去处理这个特殊信息

### 内中断的产生

1. 除法错误，比如执行div指令产生的除法溢出 中断类型码:0
2. 单步执行 中断类型码:1
3. 执行into指令 中断类型码:4
4. 执行int指令 int n

### 中断向量表

* 中断向量，中断处理程序的入口地址。
* 存储N号，偏移地址的内存单元地址为：4N
* 中断处理程序入口的段地址的内存单元的地址为：4N+2

### 中断过程

1. （从中断信息中）取得中断类型码
2. 标志寄存器的值入栈（因为需要改变标志寄存器的值）
3. 设置标志寄存器的第8为TF和第9位IF的值为0
4. cs的内容入栈
5. IP的内容入栈
6. 从内存地址为中断类型码*4和中断类型码*4+2的两个字单元中读取中断IP和CS

### 中断处理程序和iret指令

1. 保存用到的寄存器
2. 处理中断
3. 恢复用到的寄存器
4. 用iret指令返回

* iret指令

1. pop IP
2. pop CS
3. popf

### 单步中断

* 基本上，cpu在执行完一条指令之后，如果检测到标志寄存器的TF位为1，则产生单步中断，引发中断过程。单步中断的中断类型码为1，则它所引发的中断过程如下。

1. 取得中断类型码1
2. 标志寄存器入栈，TF,IF设置为0
3. CS,IP入栈
4. IP=(1*4),CS=(1*4+2)

### 响应中断的特殊情况

* 在执行完向ss寄存器传送数据的指令后，即便是发生中断，CPU也不会响应。如果引发中孤单，要在栈中压入标志寄存器等，而ss改变，sp并未改变，ss:sp指向的不是正确的栈顶。

## int指令

### int 指令

* int n,n为中断类型码，它的功能是引发中断过程

1. 取中断类型码n
2. 标志寄存器入栈
3. CS,IP入栈
4. IP=4*n,CS=4*n+2

* int 10h 设置光标位置功能
  * mov ah,2;置光标
  * mov bh,0;第0页
  * mov dh,5;dh中放行号
  * mov dl,12;dl中放列号
  * int 10h
* int 21h 在光标位置显示字符串功能
  * ds:dx 指向字符串;要显示的字符串需用"$"作为结束符
  * mov ah，9;功能号9，表示在光标位置显示字符串
  * int 21h

* 中断安装
  * 设置ds:si指向源地址
  * 设置es:di指向目标地址
  * 设置cx为传输长度
  * cld
  * rep movsb
  * mov word ptr es:[7ch*4],IP位置
  * mov word ptr es:[7ch*4+2],CS位置

## 第14章 端口

### 端口的读写

* 访问内存 mov ax,ds:[8]

* 访问端口 in al,60h ;从60h号端口读入一个字节

* 注意，在in和out指令中，只能使用ax或al存放从端口中读入的数据或要发送到端口中的数据。访问8位端口时用al，访问16位端口时用ax

* 14.1
  mov al,2
  out 70h,al
  in al,71h

  mov al,2
  out 70h,al
  mov al,0
  out 71h,al

### shl和shr指令

* shl是逻辑左移指令，它的功能位：
  1. 将一个寄存器或内存单元中的数据向左移位
  2. 将最后移出的一位写入CF中
  3. 最低位用0补充
* 如果移动的位数大于1时，必须将移动位数放在cl中。
  mov cl,3
  shl al,cl

* shr是逻辑右移指令，它和shl所进行的操作刚好相反
  1. 将一个寄存器或内存单元中的数据向右移位
  2. 将最后移出的一位写入CF中
  3. 最高位用0补充

## 第15章 外中断

* 可屏蔽中断
  * 当CPU检测到可屏蔽中断信息时，如果IF=1，则CPU在执行王当前指令后响应中断，引发中断过程，如果IF=0，则不响应中断。

### PC机键盘的处理过程

1. 键盘输入
2. 引发9号中断
3. 执行int 9中断例程
  读出60h中的扫描码
  如果时字符键，将对应ASCII码送入内存的bios键盘缓冲区
  对键盘系统进行相关的控制

## 栈

* ss:sp指向栈顶，两byte
* push ax, pop ax
* push sp-2, pop sp+2

* ds:bx/si/di ss:bp

## 寻址

* jmp 无条件跳转
* call
* ret