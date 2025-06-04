## 一、（虚拟机）共享文件夹无法找到

```bash
sudo vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other
```

然后到/mnt/hgfs/ubuntu共享文件夹下可以找到从win共享文件

## 二、查看电脑存储

```bash
df -h # 查看所有挂载点使用情况 (Size, Used, Avail, Use%, Mounted on)
lsblk # 查看设备、分区及其挂载点 (NAME, SIZE, MOUNTPOINT)
du -sh /path/to/directory # 查看特定目录大小
sudo fdisk -l # 查看硬盘分区详细信息
```

## 三、ubuntu文件操作

```bash
# 文件操作 (Linux)

# 1. 创建
touch filename             # 创建空文件
echo "内容" > filename    # 创建并写入
cat > filename             # 创建并写入 (Ctrl+D 保存)
mkdir foldername           # 创建文件夹
nano filename			   # 创建文件或编辑文件

# 2. 删除
rm filename                # 删除文件
rmdir foldername           # 删除空文件夹
rm -r foldername           # 删除非空文件夹
rm -rf foldername          # 强制删除 (谨慎使用)

# 3. 移动/重命名
mv source destination      # 移动文件/文件夹
mv oldname newname         # 重命名文件/文件夹

# 4. 查找
find /path -name "filename" # 按名称查找文件/目录
find /path -type d -name "foldername" # 查找目录
grep "内容" filename       # 在文件内容中查找
locate filename            # 全局查找 (需安装 mlocate)

# 5. 复制
cp source destination      # 复制文件
cp -r source_folder destination # 复制文件夹

# 6. 权限
chmod +x filename          # 赋予执行权限
```

## 四、deb包安装方法

```bash
# .deb 包操作 (Ubuntu)

# 安装 .deb 文件
sudo dpkg -i /path/to/package.deb      # 安装 (可能需要手动解决依赖)
sudo apt install ./package.deb         # 安装 (推荐, Ubuntu 16.04+, 自动解决依赖)
# 如果 dpkg -i 出错依赖问题:
# sudo apt --fix-broken install        # 或 sudo apt-get install -f

# 查看 .deb 文件内容 (不安装)
dpkg -c /path/to/package.deb           # 列出包内文件列表

# 提取 .deb 文件内容 (不安装)
dpkg -x /path/to/package.deb .         # 提取文件到当前目录
# 或 手动解压 (ar + tar):
# ar x package.deb
# tar -xf data.tar.xz

# 查看已安装包的信息
dpkg -L package-name                   # 列出已安装包的所有文件
dpkg -S /path/to/file                  # 查找某个文件属于哪个已安装包
```

## 五、imwheel启动以后，鼠标侧键和中键工作异常

我在 Ubuntu 上遇到了类似的问题，并找到了一种可能对其他人有帮助的解决方法。安装`imwheel`修复鼠标滚动问题后，我注意到滚动缩放和前进/后退鼠标按钮无法按预期工作。

基于以下解决方案[@soji256](https://github.com/soji256)和[@emmanuelvlad](https://github.com/emmanuelvlad)，我添加了几行代码来处理鼠标上的前进和后退按钮。具体操作如下：

1. 打开一个终端。
2. 键入以在文本编辑器中`nano ~/.imwheelrc`打开文件。`.imwheelrc`
3. 将文件内容替换为以下配置(`xprop | grep WM_CLASS`这个命令可以点击查看当前软件的ID)：

```bash
"(firefox_firefox)|(google-chrome)|(discord)|(brave)|(code)|(spotify)|(notion.*)|(mailspring)|(slack)|(inkscape)"
None,      Thumb1,   Alt_L|Left
None,      Thumb2,   Alt_L|Right
Control_L, Up,       Control_L|Button4
Control_L, Down,     Control_L|Button5
Control_R, Up,       Control_R|Button4
Control_R, Down,     Control_R|Button5
Shift_L,   Up,       Shift_L|Button4
Shift_L,   Down,     Shift_L|Button5
```

1. 按`Ctrl+X`关闭编辑器，然后按`Y`保存更改。
2. `imwheel`使用命令重新启动`killall imwheel && imwheel`。

此配置将`Thumb1`（`Thumb2`通常是鼠标上的前进和后退按钮）映射到大多数浏览器和文件管理器中的后退和前进操作。

为了确保`imwheel`在启动时启动，您可以将其添加`imwheel`到`.bashrc`文件中。操作方法如下：

1. 在终端中，输入`echo "imwheel" >> ~/.bashrc`或`echo "imwheel" >> ~/.zshrc`如果您正在使用`zsh`。

我希望这能帮助遇到同样问题的人！

参考：

+ [编辑配置文件](https://wiki.archlinux.org/title/IMWheel#Edit_your_configuration_file)
+ [如何解决按 Alt+Tab 后出现滚动异常行为？](https://askubuntu.com/a/1143466)
+ [如何在登录时自动启动应用程序？](https://askubuntu.com/a/48327)
+ [设置](https://blog.csdn.net/qq_32767041/article/details/84034280?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522216d2e79b01ba6ced5accd70eee2089a%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=216d2e79b01ba6ced5accd70eee2089a&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-84034280-null-null.142^v102^pc_search_result_base9&utm_term=imwheel%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E5%8A%A8&spm=1018.2226.3001.4187)`wheel`[开机自启动](https://blog.csdn.net/qq_32767041/article/details/84034280?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522216d2e79b01ba6ced5accd70eee2089a%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=216d2e79b01ba6ced5accd70eee2089a&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-1-84034280-null-null.142^v102^pc_search_result_base9&utm_term=imwheel%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E5%8A%A8&spm=1018.2226.3001.4187)



## 六、无法运行AppImage文件

```bash
chmod +x YourAppImageFileName.AppImage	# 给予运行权限
```

可能是缺少FUSE库，AppImage 文件需要使用 FUSE (Filesystem in Userspace) 技术来挂载和运行自身包含的文件系统。在一些 Ubuntu 版本（特别是较新的版本，如 Ubuntu 22.04 及更高版本）中，可能默认没有安装 AppImage 所需的特定版本的 FUSE 库 (libfuse2)。

```bash
sudo apt update
sudo apt install libfuse2
```

## 七、Linux终端设置代理

```bash
export http_proxy="http://[username:password@]proxy_address:port/"
export https_proxy="http://[username:password@]proxy_address:port/"
export ftp_proxy="http://[username:password@]proxy_address:port/"
export no_proxy="localhost,127.0.0.1,localaddress,.localdomain.com"

# 某些程序可能使用大写变量名，所以最好也设置一份大写的
export HTTP_PROXY="http://[username:password@]proxy_address:port/"
export HTTPS_PROXY="http://[username:password@]proxy_address:port/"
export FTP_PROXY="http://[username:password@]proxy_address:port/"
export NO_PROXY="localhost,127.0.0.1,localaddress,.localdomain.com"

# 如果是SOCKS5代理
export ALL_PROXY="socks5://[username:password@]proxy_address:port/"
```

```bash
export http_proxy="http://127.0.0.1:7890/"
export https_proxy="http://127.0.0.1:7890/"
export HTTP_PROXY="http://127.0.0.1:7890/"
export HTTPS_PROXY="http://127.0.0.1:7890/"
export no_proxy="localhost,127.0.0.1,*.local" # 排除本地地址不走代理
```

## 八、Conda相关操作

```bash
# Conda 基本信息与更新
conda --version             # 查看 conda 版本
conda update conda          # 更新 conda 自身

# 环境管理
conda create -n <环境名> python=<python版本>  # 创建新环境
# 示例: conda create -n myenv python=3.9

conda activate <环境名>      # 激活环境
# 示例: conda activate myenv

conda deactivate             # 停用当前环境

conda env list               # 列出所有环境
# 或: conda info --envs

conda env remove -n <环境名> # 删除指定环境
# 示例: conda env remove -n myenv

# 包管理
conda list                   # 列出当前环境已安装的包

conda install <包名>         # 在当前环境安装包
# 示例: conda install numpy

conda install -n <环境名> <包名> # 在指定环境安装包
# 示例: conda install -n myenv numpy

conda search <包名>          # 搜索包
# 示例: conda search tensorflow

conda update --all           # 更新当前环境所有包

conda update <包名>          # 更新指定包
# 示例: conda update numpy

conda uninstall <包名>       # 删除当前环境的包
# 或: conda remove <包名>
# 示例: conda uninstall numpy

# 环境导入导出
conda env export > environment.yml      # 导出当前环境到yml文件
conda env create -f environment.yml     # 从yml文件创建环境

# 配置与清理
conda config --add channels <镜像源URL> # 添加镜像源
# 示例: conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda config --show-sources          # 查看配置的镜像源
# 或: conda config --get channels

conda clean -a                       # 清理 conda 缓存 (删除所有包、tarballs、cache索引)
# 更详细的清理:
# conda clean -p # 删除未使用过的包
# conda clean -t # 删除tarballs
# conda clean -y --all # 自动确认，删除所有

# 帮助
conda --help                 # 获取 conda 整体帮助
# 或: conda -h

conda <命令> --help          # 获取特定命令的帮助
# 或: conda <命令> -h
# 示例: conda create --help
```

## 九、创建桌面快捷方式

```bash
sudo vim /usr/share/applications/cursor.desktop
```

```bash
[Desktop Entry]
Encoding=UTF-8
Name=Cursor
Comment=Cursor
Exec=/home/hpc/Cursor-0.50.5-x86_64.AppImage
Icon=/home/hpc/Pictures/cursor.png
Terminal=false
StartupNotify=true
Type=Application
Categories=Application;Development;
```

## 十、Graphviz使用方式

Graphviz 提供了多个命令行工具，用于根据 DOT 文件生成不同类型的图布局。最常用的是 dot。

```bash
dot -Tpng input.dot -o output.png
-T<format>: 指定输出格式，如 png, svg, pdf, jpg, gif, eps 等。
-o <output_file>: 指定输出文件名。

布局引擎：
dot: 适用于有向图的层次化布局，也是最常用的工具。它会尝试将边从上到下或从左到右排列，并尽量减少边的交叉。
neato: 适用于无向图，使用“弹簧模型”布局，节点之间像被弹簧连接，力求最小化能量。
fdp: 也是力导向布局，类似于 neato，但更适合处理大型无向图。
sfdp: fdp 的多尺度版本，适用于超大型无向图。
twopi: 径向布局，节点根据与给定根节点的距离放置在同心圆上。
circo: 圆形布局，适用于某些具有多循环结构的图，例如电信网络。
osage: 块布局，根据模块或子图的层次结构进行布局。
patchwork: 专门用于将图绘制成方形拼凑网格，适用于布局非常大的图。
```

## 十一、解压tar.**压缩文件

+ 解压`.tar`文件（没有额外压缩）

```bash
tar -xvf 文件名.tar
-x: 表示解压 (extract)
-v: 表示显示详细过程 (verbose)，可以看到哪些文件被解压出来了。此选项可选。
-f: 表示后面紧跟着的是要处理的文件名 (file)。
```

+ 解压 `.tar.gz` 或 `.tgz` 文件 (使用 gzip 压缩):

```bash
tar -zxvf 文件名.tar.gz
-z: 表示使用 gzip 进行解压缩。
```

+ 解压 `.tar.bz2` 或 `.tbz2` 文件 (使用 bzip2 压缩):

```bash
tar -jxvf 文件名.tar.bz2
-j: 表示使用 bzip2 进行解压缩。
```

+ 解压 `.tar.xz` 或 `.txz` 文件 (使用 xz 压缩):

```bash
tar -Jxvf 文件名.tar.xz
-J: (大写的J) 表示使用 xz 进行解压缩。
```

