# Linux常用命令？

## 基本文件和目录操作

- **ls**：列出目录内容

  ```sh
  ls
  ls -l # 详细信息
  ls -a # 显示隐藏文件
  ```

- **cd**：更改目录

  ```sh
  cd /path/to/directory
  cd .. # 返回上一级目录
  cd ~ # 返回用户主目录
  ```

- **pwd：**显示当前工作目录

  ```sh
  pwd
  ```

- **mkdir：**创建目录

  ```sh
  mkdir new_directory
  mkdir -p /path/to/new_directory # 递归创建目录
  ```

- **rmdir：**删除空目录

  ```sh
  rmdir directory_name
  ```

- **rm：**删除文件或目录

  ```sh
  rm file_name
  rm -r directory_name # 递归删除目录及其内容
  rm -f file_name # 强制删除
  ```

- **cp：**复制文件或目录

  ```sh
  cp source_file destination_file
  cp -r source_directory destination_directory # 递归复制目录
  ```

- **mv：**移动或重命名文件或目录

  ```sh
  mv old_name new_name
  mv file_name /path/to/destination
  ```

- **touch：**创建空文件或更新文件的时间戳

  ```sh
  touch file_name
  ```

## 文件内容查看和编辑

- **cat：**显示文件内容

  ```sh
  cat file_name
  ```

- **more：**分页显示文件内容

  ```sh
  more file_name
  ```

- **less：**分页显示文件内容，支持向前分页

  ```sh
  less file_name
  ```

- **head：**显示文件的前几行

  ```sh
  head file_name
  head -n 10 file_name # 显示前十行
  ```

- **tail：**显示文件的后几行

  ```sh
  tail file_name
  tail -n 10 file_name  # 显示后 10 行
  tail -f file_name     # 实时显示文件新增内容
  ```

- **nano、vim、gedit：**文本编辑器

  ```sh
  nano file_name
  vim file_name
  gedit file_name  # 图形界面文本编辑器
  ```

## 文件权限和所有权

- **chmod：**改变文件权限

  ```sh
  chmod 755 file_name  # 设置文件权限
  chmod +x file_name   # 增加执行权限
  ```

- **chown：**改变文件所有者

  ```sh
  chown user:group file_name
  chown -R user:group directory_name  # 递归改变目录所有者
  ```

## 系统管理

- **ps：**显示当前进程

  ```sh
  ps
  ps aux  # 显示所有进程
  ```

- **top：**实时显示系统资源使用情况

  ```sh
  top
  ```

- **htop：**更友好的实时系统资源监视工具

  ```sh
  htop
  ```

- **df：**显示文件系统磁盘空间使用情况

  ```sh
  df -h  # 以人类可读的格式显示
  ```

- **du：**显示目录或文件的磁盘使用情况

  ```sh
  du -h directory_name  # 以人类可读的格式显示
  du -sh directory_name # 显示总计
  ```

- **free：**显示内存使用情况

  ```sh
  free -h  # 以人类可读的格式显示
  ```

- **uname：**显示系统信息

  ```sh
  uname -a  # 显示所有信息
  ```

- **uptime：**显示系统运行时间

  ```sh
  uptime
  ```

- **shutdown：**关闭系统

  ```sh
  shutdown -h now  # 立即关机
  shutdown -r now  # 立即重启
  ```

## 网络相关

- **ifconfig：**显示或配置网络接口（现代系统使用 `ip` 命令代替）

  ```sh
  ifconfig
  ```

- **ip：**显示或配置网络接口

  ```sh
  ip addr show
  ip link set eth0 up  # 启动网络接口
  ip link set eth0 down  # 关闭网络接口
  ```

- **ping：**测试网络连通性

  ```sh
  ping www.example.com
  ```

- **netstat：**显示网络连接、路由表等信息

  ```sh
  netstat -tuln  # 显示监听的端口
  ```

- **curl：**命令行工具，用于发送 HTTP 请求

  ```sh
  curl http://www.example.com
  ```

- **wget：**下载文件

  ```sh
  wget http://www.example.com/file.zip
  ```

## 压缩和解压

- **tar：**打包和解压文件

  ```sh
  tar -czvf archive.tar.gz directory_name  # 打包并压缩
  tar -xzvf archive.tar.gz  # 解压
  ```

- **zip和unzip：**压缩和解压文件

  ```sh
  zip -r archive.zip directory_name  # 压缩
  unzip archive.zip  # 解压
  ```

## 搜索

- **find：**在文件系统中查找文件

  ```sh
  find /path/to/search -name "file_name"
  ```

- **grep：**在文件中搜索文本

  ```sh
  grep "search_term" file_name
  grep -r "search_term" /path/to/search  # 递归搜索
  ```

## 用户和组管理

- **useradd：**添加用户

  ```sh
  sudo useradd user_name
  sudo passwd user_name  # 设置用户密码
  ```

- **usermod：**修改用户

  ```sh
  sudo usermod -aG group_name user_name  # 将用户添加到组
  ```

- **userdel：**删除用户

  ```sh
  sudo userdel user_name
  ```

- **groupadd：**添加组

  ```sh
  sudo groupadd group_name
  ```

- **groupdel：**删除组

  ```sh
  sudo groupdel group_name
  ```

## 其他命令

- **alias：**创建命令别名
- **history：**显示命令历史

