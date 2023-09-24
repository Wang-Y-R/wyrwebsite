基础的windows powershell命令
# 命令组成

执行什么命令 + 如何执行

- ls E:/
>ls 代表执行list,E:/表示在E盘中执行

# Terminal
## 操作
### 分栏
- alt + shift + "-" 水平分栏
- alt + shift + "=" 竖直分栏
- 输入exit 关闭终端
### 切换分栏
- alt + 箭头键
### 打开操作面板
+ ctrl + shift + P
### 查找
- ctrl + shift + F 查找
>在浏览器中也可以使用
>>可以选择区不区分大小写

# Powershell
## 路径和CD
- 工作路径
- *绝对路径*(不管字符串在哪里都表示同一个路径)
> 
- *相对路径*(在不同情境下表示不同路径)
>如`.\XXX` 表示当前工作路径下的XXX文件夹 `.`指工作路径

- 改变路径方式: CD + 路径
>`.`表示当前路径
>`..`表示上一路径
>约定:`~`表示特定路径:用户根目录

- pushd记忆路径 和 popd返回路径
>pushd可以多次记忆,popd按次序依次返回

- pwd 可以查看当前路径
## 文件和文件夹的增删改查
1. 增
- mkdir + 文件夹路径 创建文件夹(==m==a==k==e ==dir==ectory) 
> -p 用于创建多级目录`虚空生成`
- ni + 文件路径 创建新文件(new item)

2. 删
- rm + 名字 删除文件夹(remove)
>不可恢复!!!

3. 查
- ls + 文件夹路径 显示路径下内容(list)
>若不输入路径则是显示工作路径
- cat + 文件名 显示文件内容(concatenate)

4. 移
- mv(move)
> mv + 文件名 + 新文件名 重命名
> mv + 文件路径 + 新路径 移动

## 获取帮助
- man + 命令 获取帮助(manual)

## 查询版本
- $PSVersionTable.PSversion

# 小操作

### 光标
- ctrl + L
### 搜索以前执行过的命令
- ctrl + R (back work search)
>按ESC(escape)退出查找

