 

**🤟致敬读者**

*   🟩感谢阅读🟦笑口常开🟪生日快乐⬛早点睡觉

**📘博主相关**

*   🟧[博主信息](https://hanshan.blog.csdn.net/article/details/145651885)🟨[博客首页](https://hanshan.blog.csdn.net)🟫[专栏推荐](https://blog.csdn.net/mo_sss/article/details/145784137)🟥[活动信息](https://blog.csdn.net/mo_sss/article/details/145784228)

* * *

#### 文章目录

*   [MySQL安装与配置详细讲解](#MySQL_19)
*   *   [一、安装前的准备](#_23)
    *   [二、安装过程详解（以常见场景为例）](#_43)
    *   *   [场景 1：在 Windows 上安装 (使用官方 MSI 安装程序)](#_1_Windows___MSI__45)
        *   [场景 2：在 Ubuntu/Debian 上安装 (使用 APT 包管理器)](#_2_UbuntuDebian___APT__77)
        *   [场景 3：在 macOS 上安装 (使用官方 DMG 安装程序)](#_3_macOS___DMG__105)
        *   [场景 4：在 CentOS/Red Hat/Fedora 上安装 (使用 YUM/DNF 包管理器)](#_4_CentOSRed_HatFedora___YUMDNF__141)
    *   [三、基础配置](#_176)
    *   *   [常用配置项 (可以在配置文件中修改)](#__184)
        *   [修改配置步骤](#_205)
    *   [四、验证安装与连接](#_231)
    *   [五、重要安全注意事项](#_270)
    *   [六、卸载 MySQL](#_MySQL_280)
    *   [总结](#_291)

* * *

**📃文章前言**

*   🔷文章均为学习工作中整理的笔记。
*   🔶如有错误请指正，共同学习进步。

* * *

MySQL安装与配置详细讲解
--------------

**核心目标：** 在你的[计算机](https://so.csdn.net/so/search?q=%E8%AE%A1%E7%AE%97%E6%9C%BA&spm=1001.2101.3001.7020)上成功安装 MySQL 服务器软件，并进行基本配置，使其能够运行、接受连接并确保安全。

### 一、安装前的准备

1.  **确定操作系统：** MySQL 支持 Windows, macOS, Linux (各种发行版如 Ubuntu, CentOS, Fedora 等)。你需要根据你的操作系统选择对应的安装包和方法。
2.  **选择安装方式：**
    *   **官方安装包/安装程序：** 最常见和推荐的方式。MySQL 官方提供了针对不同操作系统的安装包（如 Windows 的 `.msi` 安装程序， macOS 的 `.dmg`， Linux 的 `.rpm` 或 `.deb` 包）。
    *   **压缩包/二进制包：** 下载包含预编译二进制文件的压缩包，解压后手动配置环境变量和初始化。更灵活，但步骤稍复杂。
    *   **包管理器安装 (Linux/macOS)：** 使用系统自带的包管理器安装（如 `apt-get` for Ubuntu/Debian, `yum`/`dnf` for CentOS/RedHat/Fedora, `brew` for macOS）。这是 Linux 上最便捷的方式。
    *   **Docker 容器：** 使用 Docker 拉取 MySQL 镜像并运行容器。适合需要快速部署、隔离环境或开发测试。
3.  **选择 MySQL 版本：**
    *   **MySQL Community Server：** 免费、开源的版本，功能强大，适合绝大多数个人用户、开发者和中小企业。这是最常用的版本。
    *   **MySQL Enterprise Edition：** 商业版，提供额外的企业级功能、工具和支持服务。
    *   **MySQL Cluster：** 面向高可用性、高吞吐量的分布式数据库。
    *   **建议初学者：** 选择最新的 **MySQL Community Server** 稳定版（GA 版）。
4.  **下载安装包：**
    *   访问 MySQL 官方网站： `https://dev.mysql.com/downloads/mysql/`
    *   选择对应的操作系统和版本。
    *   对于 Windows 和 macOS，通常下载体积较大的安装程序（如 `mysql-installer-community-<version>.<os>.msi` 或 `.dmg`）。
    *   对于 Linux，通常建议使用包管理器安装。如果需要下载特定 `.rpm` 或 `.deb` 包，也在此页面查找。
5.  **检查系统要求：** 确保你的计算机硬件（CPU、内存、磁盘空间）满足所选 MySQL 版本的最低要求（通常现代个人电脑都远超要求）。

### 二、安装过程详解（以常见场景为例）

#### 场景 1：在 Windows 上安装 (使用官方 MSI 安装程序)

1.  **运行安装程序：** 双击下载好的 `.msi` 文件。
2.  **选择安装类型：**
    *   **Developer Default：** 安装 MySQL Server 和开发相关工具（如 MySQL Workbench, Shell, Router 等）。适合开发者。
    *   **Server only：** 仅安装 MySQL 服务器核心组件。最简洁。
    *   **Client only：** 仅安装客户端工具（如 `mysql` 命令行客户端）。
    *   **Full：** 安装所有组件。
    *   **Custom：** 自定义选择要安装的组件。
    *   **建议初学者：** 选择 `Developer Default` 或 `Server only`。
3.  **检查依赖：** 安装程序会自动检查所需依赖（如 .NET Framework, Visual C++ Redistributable）。如果缺失，会提示下载安装。按提示操作。
4.  **下载产品 (可选)：** 如果选择 `Developer Default` 或 `Full` 且未预先下载所有组件，安装程序会联网下载所需文件。
5.  **安装产品：** 等待安装程序复制文件并进行安装。
6.  **配置产品：**
    *   安装完成后，通常会立即启动 **MySQL Server Configuration** 向导。
    *   **选择配置类型：**
        *   `Development Computer`： 开发环境，占用较少资源。
        *   `Server Computer`： 服务器环境，占用更多资源优化性能。
        *   `Dedicated Computer`： 专用数据库服务器，分配所有可用资源。
        *   **建议：** 选择 `Development Computer`。
    *   **设置身份验证方法 (Authentication Method)：**
        *   `Use Strong Password Encryption for Authentication (RECOMMENDED)`： 使用强密码加密（`caching_sha2_password`），安全性高，是 MySQL 8.0 的默认方式。**推荐选择此项。**
        *   `Use Legacy Authentication Method (Retain MySQL 5.x Compatibility)`： 使用旧式密码认证（`mysql_native_password`），兼容旧客户端。除非有特定兼容需求，否则不建议。
    *   **设置 root 用户密码：**
        *   为 MySQL 超级管理员账户 `root` 设置一个**强密码**（包含大小写字母、数字、特殊符号）。**务必牢记此密码！**
        *   可以添加其他具有管理员权限的用户（可选）。
    *   **配置 Windows 服务：**
        *   **Windows Service Name：** 设置 MySQL 服务在 Windows 服务管理器中的名称（默认 `MySQL80`）。
        *   **Start the MySQL Server at System Startup：** 是否让 MySQL 随系统启动自动运行（建议勾选）。
    *   **应用配置：** 点击 `Execute` 应用配置设置。配置工具会初始化数据目录、创建系统表、设置 root 密码、启动 MySQL 服务等。
7.  **完成安装：** 所有步骤完成后，点击 `Finish`。MySQL 服务器应已安装并运行在你的 Windows 上。

#### 场景 2：在 Ubuntu/Debian 上安装 (使用 APT 包管理器)

1.  **更新包索引：** 打开终端，执行：
    
    ```bash
    sudo apt update
    ```
    
2.  **安装 MySQL Server 包：**
    
    ```bash
    sudo apt install mysql-server
    ```
    
    *   安装过程中，可能会提示你设置 `root` 用户的密码。**请设置强密码并牢记。**
3.  **运行安全脚本 (强烈推荐)：** MySQL 安装后包含一个安全脚本，用于提高默认安装的安全性：
    
    ```bash
    sudo mysql_secure_installation
    ```
    
    *   按提示操作：
        *   是否设置 `VALIDATE PASSWORD` 组件？(可选，用于设置密码强度策略)
        *   为 `root` 用户设置密码（如果安装时未设置或需要修改）。
        *   是否移除匿名用户？(**强烈建议输入 `Y`**)
        *   是否禁止 `root` 用户远程登录？(**强烈建议输入 `Y`**，生产环境 root 应仅限本地登录)
        *   是否移除测试数据库 `test`？(**强烈建议输入 `Y`**)
        *   是否立即重新加载权限表？(**输入 `Y`**)
4.  **检查服务状态：**
    
    ```bash
    sudo systemctl status mysql.service
    ```
    
    *   应看到状态为 `active (running)`。

#### 场景 3：在 macOS 上安装 (使用官方 DMG 安装程序)

1.  **运行安装程序：** 双击下载好的 `.dmg` 文件，然后双击里面的 `.pkg` 安装包。
2.  **安装向导：** 跟随图形化安装向导步骤。通常不需要修改默认设置。
3.  **成功安装：** 安装完成后，MySQL 服务器通常会自动启动。
4.  **配置 PATH (可选但推荐)：** 为了能在终端方便地使用 `mysql`, `mysqladmin` 等命令，需要将 MySQL 的 `bin` 目录添加到系统的 `PATH` 环境变量中。
    *   **查找安装路径：** 默认通常在 `/usr/local/mysql/bin`。
    *   **修改 Shell 配置文件：** 根据你使用的 shell (如 `zsh` 或 `bash`)，编辑 `~/.zshrc` 或 `~/.bash_profile` 文件：
        
        ```bash
        echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.zshrc  # 如果是 zsh
        # 或
        echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile  # 如果是 bash
        ```
        
    *   **使配置生效：**
        
        ```bash
        source ~/.zshrc   # 或 source ~/.bash_profile
        ```
        
5.  **设置 root 密码：** 安装后 `root` 用户可能没有密码或有一个随机初始密码（检查安装日志或通知）。**务必设置强密码！**
    *   登录 MySQL (可能需要先停止服务或使用 `sudo`)：
        
        ```bash
        sudo mysql -u root
        ```
        
    *   在 `mysql>` 提示符下：
        
        ```sql
        ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YourStrongPasswordHere'; -- 如果安装时选择了 legacy auth
        -- 或 (MySQL 8.0+ 推荐)
        ALTER USER 'root'@'localhost' IDENTIFIED BY 'YourStrongPasswordHere';
        FLUSH PRIVILEGES;
        EXIT;
        ```
        
    *   运行安全脚本 (可选但推荐)：
        
        ```bash
        sudo mysql_secure_installation
        ```
        
        (参考 Linux 部分的安全脚本步骤)

#### 场景 4：在 CentOS/Red Hat/Fedora 上安装 (使用 YUM/DNF 包管理器)

1.  **添加 MySQL Yum Repository (可选)：** 为了获得最新版本，通常需要添加官方仓库。
    *   访问 `https://dev.mysql.com/downloads/repo/yum/` 下载对应你系统的 `.rpm` 包。
    *   安装仓库包：
        
        ```bash
        sudo rpm -Uvh mysql80-community-release-el7-<version>.noarch.rpm  # 替换为实际文件名
        ```
        
2.  **安装 MySQL Server：**
    
    ```bash
    sudo yum install mysql-community-server   # 使用 yum
    # 或 (Fedora/较新 CentOS/RHEL)
    sudo dnf install mysql-community-server
    ```
    
3.  **启动服务：**
    
    ```bash
    sudo systemctl start mysqld
    ```
    
4.  **设置开机启动：**
    
    ```bash
    sudo systemctl enable mysqld
    ```
    
5.  **查找临时 root 密码：** MySQL 5.7+ 首次启动会生成一个临时密码在日志文件中：
    
    ```bash
    sudo grep 'temporary password' /var/log/mysqld.log
    ```
    
    **记录下这个临时密码。**
6.  **运行安全脚本：**
    
    ```bash
    sudo mysql_secure_installation
    ```
    
    *   输入上一步得到的临时密码。
    *   提示是否修改 root 密码？**输入 `Y`**，然后设置你的**强密码**。
    *   后续步骤（移除匿名用户、禁止 root 远程登录、移除 test 数据库、刷新权限）**强烈建议全部输入 `Y`**。

### 三、基础配置

MySQL 的主要配置文件是 `my.cnf` (Linux/macOS) 或 `my.ini` (Windows)。它的位置因操作系统和安装方式而异。常见位置：

*   **Linux:** `/etc/my.cnf`, `/etc/mysql/my.cnf`, `/usr/etc/my.cnf`, `~/.my.cnf`
*   **macOS:** `/usr/local/mysql/etc/my.cnf`, `/etc/my.cnf`, `~/.my.cnf`
*   **Windows:** `C:\ProgramData\MySQL\MySQL Server 8.0\my.ini` (通常是隐藏目录), 安装目录下的 `my.ini`

#### 常用配置项 (可以在配置文件中修改)

1.  **\[mysqld\] 部分 (服务器核心配置):**
    *   `port = 3306`: MySQL 服务器监听的端口号（默认 3306）。
    *   `datadir = /var/lib/mysql`: 数据文件存储目录（非常重要！Linux 默认位置）。
    *   `socket = /tmp/mysql.sock` (Linux/macOS) 或 `socket = MySQL` (Windows)： 本地连接使用的套接字文件。
    *   `character-set-server = utf8mb4`: 设置服务器默认字符集。强烈推荐使用 `utf8mb4` 以支持完整的 Unicode（包括 emoji）。
    *   `collation-server = utf8mb4_unicode_ci`: 设置服务器默认排序规则。`utf8mb4_unicode_ci` 是通用性较好的选择。
    *   `default-storage-engine = InnoDB`: 设置默认存储引擎。`InnoDB` 是支持事务、外键的现代引擎，推荐使用。
    *   `max_connections = 151`: 允许的最大并发客户端连接数。根据服务器资源调整。
    *   `bind-address = 127.0.0.1`: 服务器绑定的 IP 地址。`127.0.0.1` 表示只允许本机连接。如需远程连接，可改为 `0.0.0.0` (**注意：改为 0.0.0.0 后务必配置用户权限和防火墙！安全风险高**)。
    *   `innodb_buffer_pool_size = 128M`: InnoDB 存储引擎用于缓存数据和索引的内存池大小。这是最重要的性能调优参数之一，通常设置为系统物理内存的 50%-80%。
    *   `log_error = /var/log/mysql/error.log` (Linux) 或 `log-error = mysql.err` (Windows)： 错误日志文件路径。排查问题必备。
    *   `slow_query_log = 1`: 是否启用慢查询日志（1 启用，0 禁用）。
    *   `slow_query_log_file = /var/log/mysql/mysql-slow.log` (Linux)： 慢查询日志文件路径。
    *   `long_query_time = 2`: 定义执行时间超过多少秒的查询为慢查询（记录到慢日志）。
2.  **\[client\] 部分：** 影响客户端程序的默认设置（如 `mysql` 命令行工具）。
    *   `port = 3306`
    *   `socket = /tmp/mysql.sock` (Linux/macOS)
    *   `default-character-set = utf8mb4`

#### 修改配置步骤

1.  **找到正确的配置文件：** 使用命令 `mysql --help | grep "Default options" -A 1` (Linux/macOS) 或查看 MySQL 服务属性中的启动参数 (Windows) 可以确定 MySQL 实际加载的配置文件顺序。
2.  **备份配置文件：** 修改前务必备份！`cp /etc/my.cnf /etc/my.cnf.bak`
3.  **编辑配置文件：** 使用文本编辑器（如 `vi`, `nano`, `Notepad++`）打开配置文件。
4.  **修改配置项：** 在相应的 `[section]` 下添加或修改键值对。注意格式通常是 `key = value`。
5.  **保存文件。**
6.  **重启 MySQL 服务：** 修改配置后，**必须重启 MySQL 服务** 才能使新配置生效。
    *   **Linux (Systemd):**
        
        ```bash
        sudo systemctl restart mysql   # 或 sudo systemctl restart mysqld (CentOS/RHEL)
        ```
        
    *   **Windows:**
        *   打开“服务”管理器 (services.msc)，找到 MySQL 服务（如 `MySQL80`），右键选择“重启”。
        *   或使用命令提示符 (管理员)：
            
            ```cmd
            net stop MySQL80
            net start MySQL80
            ```
            
    *   **macOS:**
        *   系统偏好设置 -> MySQL -> Stop/Start。
        *   或使用终端：
            
            ```bash
            sudo /usr/local/mysql/support-files/mysql.server restart
            ```
            

### 四、验证安装与连接

1.  **检查服务状态：**
    
    *   Linux/Systemd: `systemctl status mysql`
    *   Windows: 服务管理器查看状态。
    *   应显示为正在运行 (`active (running)` 或 `Started`)。
2.  **使用命令行客户端连接：**
    
    ```bash
    mysql -u root -p
    ```
    
    *   `-u root`: 指定用户名为 `root`。
    *   `-p`: 提示输入密码。
    *   输入你设置的 `root` 密码。
    *   如果成功，你将看到 `mysql>` 提示符。
3.  **执行简单 SQL 命令验证：**
    
    ```sql
    mysql> SHOW DATABASES;
    ```
    
    *   应显示包含 `information_schema`, `mysql`, `performance_schema`, `sys` 等系统数据库的列表。
    
    ```sql
    mysql> SELECT VERSION();
    ```
    
    *   应显示你安装的 MySQL 版本号。
    
    ```sql
    mysql> STATUS;
    ```
    
    *   显示连接状态信息，包括服务器版本、连接 ID、当前数据库、字符集等。
4.  **退出客户端：**
    
    ```sql
    mysql> EXIT;
    ```
    
    或
    
    ```sql
    mysql> QUIT;
    ```
    

### 五、重要安全注意事项

1.  **强密码：** 为 `root` 用户和其他所有用户设置**复杂且唯一的强密码**。避免使用默认密码或简单密码。
2.  **移除匿名用户：** 使用 `mysql_secure_installation` 脚本移除匿名用户。
3.  **限制 root 远程访问：** 生产环境中，**禁止 `root` 用户从远程主机登录** (`mysql_secure_installation` 会处理)。应创建具有必要权限的特定用户用于远程管理。
4.  **移除测试数据库：** 使用 `mysql_secure_installation` 移除默认的 `test` 数据库。
5.  **防火墙配置：** 如果服务器需要接受远程连接，必须在操作系统的防火墙（如 `iptables`, `firewalld`, Windows 防火墙）上**仅开放 MySQL 服务端口（默认 3306）给特定的、可信的 IP 地址或地址段**。禁止无限制开放 3306 端口到公网。
6.  **定期更新：** 及时应用 MySQL 的安全更新和补丁。
7.  **最小权限原则：** 为应用程序创建专用的数据库用户，并仅授予该用户操作其所需数据库的最小必要权限（`SELECT`, `INSERT`, `UPDATE`, `DELETE` 等）。避免使用 `root` 用户运行应用。

### 六、卸载 MySQL

**谨慎操作！卸载会删除所有数据和配置！务必先备份重要数据！**

1.  **停止 MySQL 服务。**
2.  **使用系统卸载程序 (Windows/macOS)：** 通过“添加/删除程序”或安装包自带的卸载程序。
3.  **使用包管理器卸载 (Linux)：**
    *   Ubuntu/Debian: `sudo apt purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*` 然后 `sudo apt autoremove` 最后 `sudo rm -rf /etc/mysql /var/lib/mysql`
    *   CentOS/RHEL: `sudo yum remove mysql-community-server mysql-community-client mysql-community-common` 然后 `sudo rm -rf /var/lib/mysql`
4.  **手动删除残留文件：** 删除数据目录 (`datadir`)、配置文件 (`my.cnf`/`my.ini`)、日志文件等。位置取决于安装方式和配置。

### 总结

安装和配置 MySQL 是使用它的第一步。核心步骤包括：选择合适的版本和安装方式、运行安装程序/包管理器命令、进行初始安全配置（特别是设置 root 密码和运行 `mysql_secure_installation`）、理解并可能需要修改核心配置文件 (`my.cnf`/`my.ini`)、重启服务使配置生效、最后验证连接和基本功能。**始终将安全性放在首位（强密码、移除匿名用户、限制 root 远程访问、防火墙）**。

完成这些后，你就可以开始创建数据库、表，并使用 SQL 语言管理和查询数据了！接下来通常会学习：

1.  **基础 SQL:** `CREATE DATABASE`, `CREATE TABLE`, `INSERT`, `SELECT`, `UPDATE`, `DELETE`, `DROP` 等。
2.  **用户与权限管理:** `CREATE USER`, `GRANT`, `REVOKE`。
3.  **备份与恢复:** `mysqldump` 工具的使用。
4.  **使用图形化工具:** 如 MySQL Workbench 进行可视化操作。

* * *

**📜文末寄语**

*   🟠关注我，获取更多内容。
*   🟡技术动态、实战教程、问题解决方案等内容持续更新中。
*   🟢[《全栈知识库》](https://bbs.csdn.net/forums/hanshan?typeId=7167914)技术交流和分享社区，集结全栈各领域开发者，期待你的加入。
*   🔵​加入开发者的[《专属社群》](https://bbs.csdn.net/topics/619601404)，分享交流，技术之路不再孤独，一起变强。
*   🟣点击下方名片获取更多内容🍭🍭🍭👇

* * *

 

![](https://i-blog.csdnimg.cn/direct/5f5883489725408cb959d3bce39db916.png) 岫珩 日常分享|文章同步|技术交流|干货获取

![](https://g.csdnimg.cn/extension-box/1.1.9/image/weixin.png)微信公众号

![](https://g.csdnimg.cn/extension-box/1.1.9/image/ic_move.png)

本文转自 <https://blog.csdn.net/mo_sss/article/details/148446053?spm=1001.2100.3001.7377&utm_medium=distribute.pc_feed_blog.none-task-blog-hot-19-148446053-null-null.nonecase&depth_1-utm_source=distribute.pc_feed_blog.none-task-blog-hot-19-148446053-null-null.nonecase>，如有侵权，请联系删除。