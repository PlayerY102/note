# linux note

## find

* find ./ -maxdepth 1 -name *.后缀
* 当前目录下的后缀名的文件

## |xargs rm -rf

* 删除传输过来的所有文件

## chmod

* chmod 用户+权限 filename
* 用户 u:所有者 g:组员
* 权限 r=读取属性 w=写入属性 x=执行属性
* 给shell脚本执行权限 chmod +x filename