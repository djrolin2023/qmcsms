# 乾明学生管理系统 (QMCSMS) - 完整使用手册

## 📋 目录导航
- [系统概述](#-系统概述)
- [系统部署](#-系统部署)
  - [Windows环境部署](#windows-系统部署)
  - [Linux环境部署](#linux-系统部署)
    - [Debian/Ubuntu部署](#debianubuntu-部署)
    - [RHEL/CentOS部署](#rhelcentos-部署)
    - [openEuler部署](#openeuler-部署)
    - [HarmonyOS部署](#harmonyos-部署)
    - [KylinOS部署](#kylinos-部署)
- [硬件配置推荐](#-硬件配置推荐)
- [浏览器兼容性](#-浏览器兼容性)
- [系统维护](#-系统维护)
- [技术支持](#-技术支持)
- [版本信息](#-版本信息)

## 📋 系统概述

**乾明学生管理系统** (Qianming Campus Student Management System, 简称QMCSMS) 是一个基于Flask框架开发的现代化学生信息管理平台，为学校、教育机构提供全面的学生管理解决方案。

### 🎯 核心功能模块

- **学生信息管理** - 学生档案、班级分配、基本信息维护
- **成绩管理系统** - 多科目成绩录入、统计分析、权限控制
- **考勤管理** - 日常考勤记录、请假审批、统计报表
- **奖惩管理** - 奖励与惩罚记录、行为分析
- **通知系统** - 系统公告、个人消息、邮件通知
- **权限管理** - 多角色权限控制、数据权限隔离
- **帮助中心** - 完整的在线帮助文档系统

### ✨ 核心功能特性

#### 1. 智能ID编码系统
- **动态ID生成**：基于用户角色和状态自动生成显示ID
- **角色变更支持**：班主任变科目老师、内宿生变外宿生时ID自动更新
- **编码规则**：
  - 管理员：1开头
  - 教师：班主任21、科目老师22、代课老师23
  - 查看员：3开头
  - 学生：内宿生41、外宿生42

#### 2. 多方式登录认证
- **5种登录方式**：账号名、ID、手机号、身份证号、邮箱
- **智能识别**：自动识别输入类型，提供实时提示
- **统一认证**：使用`LoginService`统一处理所有登录方式

#### 3. 科目类别管理系统
- 支持12种标准科目分类（语文、数学、英语、物理、化学、生物、政治、历史、地理、体育、音乐、美术）
- 科目代码规范化管理，支持科目类别筛选

#### 4. 教师权限细化体系
- **班主任**：分管班级所有学生信息和成绩的全面管理权限
- **科目老师**：仅限任教科目成绩的管理权限，可查看学生基本信息
- **代课老师**：与科目老师相同权限，支持临时代课管理

#### 5. 基于科目的成绩权限控制
- 双重权限验证（班级权限 + 科目权限）
- 动态数据过滤，确保数据安全
- 灵活的权限分配机制

#### 6. 完整的通知服务
- **多平台通知**：企业微信、爱语飞飞、Server酱、邮箱、钉钉、飞书
- **自动提醒**：请假、成绩变动、考勤异常、奖惩记录、系统告警
- **自定义配置**：用户可配置通知方式和内容

---

## 🚀 系统部署指南

### Windows 系统部署

#### 环境要求
- **操作系统**: Windows 10/11 或 Windows Server 2016+
- **Python版本**: Python 3.8+
- **内存要求**: 4GB RAM (推荐8GB)
- **存储空间**: 10GB 可用磁盘空间
- **网络要求**: 局域网或互联网访问

#### 详细部署步骤

1. **安装Python环境**
   ```bash
   # 1. 访问Python官网下载Python 3.8+安装包
   # 2. 运行安装程序，确保勾选以下选项：
   #    - [x] Add Python to PATH
   #    - [x] Install pip
   #    - [x] Create shortcuts
   # 3. 验证安装：打开CMD，输入 python --version
   ```

2. **获取项目文件**
   ```bash
   # 方式1：从Git仓库下载
   git clone https://github.com/djrolin2023/qmcsms/QMCSMS.git
   
   # 方式2：下载压缩包并解压到指定目录
   # 建议目录：D:\QMCSMS 或 C:\Program Files\QMCSMS
   ```

3. **安装依赖包**
   ```bash
   # 进入项目目录
   cd D:\QMCSMS
   
   # 创建虚拟环境（推荐）
   python -m venv venv
   venv\Scripts\activate
   
   # 安装依赖包
   pip install -r requirements.txt
   
   # 或者使用国内镜像加速安装
   pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/
   ```

4. **初始化数据库**
   ```bash
   # 初始化数据库表结构
   python init_db.py
   
   # 如果需要进行数据迁移
   python migrate_user_roles.py
   
   # 如果需要完全重建数据库（解决表结构问题）
   python clean_recreate_db.py
   ```

5. **启动系统**
   ```bash
   # 方式1：开发模式启动（推荐测试环境）
   python app.py
   
   # 方式2：使用批处理文件启动（推荐生产环境）
   Start.bat
   
   # 方式3：使用Build.bat打包为可执行文件
   Build.bat
   ```

6. **访问系统**
   - 打开浏览器访问：`http://localhost:8081`
   - 默认管理员账号：`admin` / `admin`
   - 默认操作员账号：`operator` / `operator`
   - 默认查看员账号：`viewer` / `viewer`

#### Windows 服务部署（生产环境）

```bash
# 1. 创建Windows服务（需要管理员权限）
sc create "QMCSMS Service" binPath="D:\QMCSMS\Start.bat" start=auto

# 2. 启动服务
sc start "QMCSMS Service"

# 3. 设置服务描述
sc description "QMCSMS Service" "乾明学生管理系统 - 学生信息管理平台"

# 4. 查看服务状态
sc query "QMCSMS Service"

# 5. 停止服务
sc stop "QMCSMS Service"

# 6. 删除服务
sc delete "QMCSMS Service"
```

#### 防火墙配置
```bash
# 允许8081端口通过防火墙（需要管理员权限）
netsh advfirewall firewall add rule name="QMCSMS" dir=in action=allow protocol=TCP localport=8081

# 查看防火墙规则
netsh advfirewall firewall show rule name="QMCSMS"

# 删除防火墙规则（如需重新配置）
netsh advfirewall firewall delete rule name="QMCSMS"
```

#### Windows性能优化配置

1. **应用池配置**（如果使用IIS）
```xml
<!-- 在web.config中添加 -->
<system.webServer>
  <aspNetCore processPath=".\venv\Scripts\python.exe" 
              arguments="app.py" 
              stdoutLogEnabled="true" 
              stdoutLogFile=".\logs\stdout" 
              hostingModel="OutOfProcess">
    <environmentVariables>
      <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Production" />
    </environmentVariables>
  </aspNetCore>
</system.webServer>
```

2. **注册表优化**（可选）
```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters]
"TcpTimedWaitDelay"=dword:0000001e
"MaxUserPort"=dword:0000fffe

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HTTP\Parameters]
"MaxConnections"=dword:00000100
```

3. **Windows事件日志配置**
```powershell
# 创建自定义事件日志源
New-EventLog -LogName "Application" -Source "QMCSMS"

# 设置日志轮转策略
wevtutil sl Application /ms:10485760
```

---

### Linux 系统部署

#### 环境要求
- **操作系统**: Ubuntu 18.04+ / Debian 10+ / CentOS 7+ / RHEL 8+ / openEuler 20.03+ / HarmonyOS 2.0+ / KylinOS V10+
- **Python版本**: Python 3.8+
- **内存要求**: 2GB RAM (推荐4GB)
- **存储空间**: 5GB 可用磁盘空间
- **网络要求**: 局域网或互联网访问

---

#### Debian/Ubuntu 部署

1. **系统准备与依赖安装**
   ```bash
   # 更新系统包管理器
   sudo apt update
   sudo apt upgrade -y
   
   # 安装基础依赖
   sudo apt install -y python3 python3-pip python3-venv python3-dev nginx git curl wget
   
   # 安装数据库客户端（可选，用于外部数据库连接）
   sudo apt install -y postgresql-client mysql-client
   
   # 验证Python安装
   python3 --version
   pip3 --version
   ```

2. **创建专用用户和目录**
   ```bash
   # 创建系统专用用户
   sudo useradd -r -s /bin/false -d /opt/qmcsms qmcsms
   
   # 创建应用目录
   sudo mkdir -p /opt/qmcsms
   sudo chown qmcsms:qmcsms /opt/qmcsms
   
   # 设置目录权限
   sudo chmod 755 /opt/qmcsms
   ```

3. **部署应用文件**
   ```bash
   # 切换到应用目录
   cd /opt/qmcsms
   
   # 下载项目文件（示例：使用git）
   sudo -u qmcsms git clone https://github.com/djrolin2023/QMCSMS.git .
   
   # 或者手动复制文件到目录
   # sudo cp -r /path/to/qmcsms/* /opt/qmcsms/
   
   # 设置文件所有权
   sudo chown -R qmcsms:qmcsms /opt/qmcsms
   ```

4. **配置Python虚拟环境**
   ```bash
   # 创建虚拟环境
   sudo -u qmcsms python3 -m venv venv
   
   # 激活虚拟环境并安装依赖
   sudo -u qmcsms /opt/qmcsms/venv/bin/pip install --upgrade pip
   sudo -u qmcsms /opt/qmcsms/venv/bin/pip install -r requirements.txt
   
   # 使用国内镜像加速安装（可选）
   # sudo -u qmcsms /opt/qmcsms/venv/bin/pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/
   ```

5. **初始化数据库**
   ```bash
   # 初始化数据库结构
   sudo -u qmcsms /opt/qmcsms/venv/bin/python init_db.py
   
   # 如果需要进行权限迁移
   sudo -u qmcsms /opt/qmcsms/venv/bin/python migrate_user_roles.py
   ```

6. **配置系统服务**
   ```bash
   # 创建systemd服务文件
   sudo nano /etc/systemd/system/qmcsms.service
   ```

   服务文件内容：
   ```ini
   [Unit]
   Description=QMCSMS Student Management System
   After=network.target
   Wants=network.target
   
   [Service]
   Type=simple
   User=qmcsms
   Group=qmcsms
   WorkingDirectory=/opt/qmcsms
   Environment=PATH=/opt/qmcsms/venv/bin
   Environment=PYTHONPATH=/opt/qmcsms
   ExecStart=/opt/qmcsms/venv/bin/python app.py
   Restart=always
   RestartSec=10
   StandardOutput=journal
   StandardError=journal
   
   [Install]
   WantedBy=multi-user.target
   ```

7. **启动并启用服务**
   ```bash
   # 重新加载systemd配置
   sudo systemctl daemon-reload
   
   # 启用服务（开机自启）
   sudo systemctl enable qmcsms
   
   # 启动服务
   sudo systemctl start qmcsms
   
   # 检查服务状态
   sudo systemctl status qmcsms
   
   # 查看服务日志
   sudo journalctl -u qmcsms -f
   ```

8. **配置Nginx反向代理（生产环境推荐）**
   ```bash
   # 创建Nginx配置文件
   sudo nano /etc/nginx/sites-available/qmcsms
   ```

   配置文件内容：
   ```nginx
   server {
       listen 80;
       server_name your-domain.com;  # 替换为实际域名或IP
       
       # 静态文件服务
       location /static {
           alias /opt/qmcsms/static;
           expires 30d;
           add_header Cache-Control "public, immutable";
       }
       
       # 反向代理到应用
       location / {
           proxy_pass http://127.0.0.1:8081;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
           proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
           proxy_set_header X-Forwarded-Proto $scheme;
       }
   }
   ```

   ```bash
   # 启用站点配置
   sudo ln -s /etc/nginx/sites-available/qmcsms /etc/nginx/sites-enabled/
   
   # 测试Nginx配置
   sudo nginx -t
   
   # 重启Nginx
   sudo systemctl restart nginx
   ```

---

#### RHEL/CentOS 部署

1. **系统准备**
   ```bash
   # CentOS/RHEL 7+
   sudo yum update -y
   sudo yum install -y python3 python3-pip nginx git
   
   # RHEL 8+/CentOS 8+ 使用dnf
   sudo dnf update -y
   sudo dnf install -y python3 python3-pip nginx git
   
   # 启用EPEL仓库（CentOS）
   sudo yum install -y epel-release
   ```

2. **SELinux配置（如启用）**
   ```bash
   # 允许Nginx连接应用端口
   sudo setsebool -P httpd_can_network_connect 1
   
   # 或者临时禁用SELinux（不推荐生产环境）
   # sudo setenforce 0
   ```

3. **防火墙配置**
   ```bash
   # 开放HTTP和HTTPS端口
   sudo firewall-cmd --permanent --add-service=http
   sudo firewall-cmd --permanent --add-service=https
   sudo firewall-cmd --reload
   ```

4. **后续步骤与Debian/Ubuntu相同**（从创建用户开始）

---

#### openEuler 部署

1. **系统准备**
   ```bash
   # openEuler 20.03+
   sudo dnf update -y
   sudo dnf install -y python3 python3-pip nginx git
   
   # 配置软件源（如果需要）
   sudo dnf config-manager --add-repo=https://repo.openeuler.org/openEuler-20.03-LTS/everything/x86_64/
   ```

2. **安全增强配置**
   ```bash
   # 配置SELinux策略
   sudo semanage port -a -t http_port_t -p tcp 8081
   
   # 或者使用openEuler的安全模块
   sudo security-tool --configure-application qmcsms
   ```

3. **后续步骤与RHEL/CentOS相同**

---

#### HarmonyOS 部署

**注意**: HarmonyOS主要用于移动设备，服务器部署建议使用openEuler或标准Linux发行版。如需要在HarmonyOS环境部署，建议采用以下方案：

1. **容器化部署（推荐）**
   ```dockerfile
   # 使用标准Python镜像，确保兼容性
   FROM python:3.8-slim
   
   WORKDIR /app
   COPY . .
   RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/
   
   EXPOSE 8081
   CMD ["python", "app.py"]
   ```

2. **兼容性配置**
   ```bash
   # 在HarmonyOS环境下，确保使用兼容的Python版本
   # 建议使用Python 3.8+的稳定版本
   
   # 检查系统架构
   uname -m
   
   # 如果是ARM架构，可能需要特定依赖
   if [ "$(uname -m)" = "aarch64" ]; then
       # ARM64架构特定配置
       pip install --no-binary :all: cryptography
   fi
   ```

---

#### KylinOS 部署

1. **系统准备**
   ```bash
   # KylinOS V10+
   sudo apt update
   sudo apt install -y python3 python3-pip nginx git
   
   # 麒麟系统特有配置
   sudo kylin-secure-config --enable-service qmcsms
   ```

2. **国产化适配**
   ```bash
   # 适配国产CPU架构（如飞腾、龙芯）
   # 可能需要从源码编译Python
   
   # 飞腾架构
   export ARCH=arm64
   
   # 龙芯架构
   export ARCH=loongarch64
   ```

3. **安全加固**
   ```bash
   # 启用麒麟安全模块
   sudo kylin-security --enable-app-protection qmcsms
   
   # 配置应用沙箱
   sudo apparmor_parser -r /etc/apparmor.d/qmcsms
   ```

#### Linux性能优化配置

1. **系统内核参数优化**
```bash
# 编辑sysctl配置
sudo nano /etc/sysctl.conf

# 添加以下参数
net.core.somaxconn = 65535
net.core.netdev_max_backlog = 65536
net.ipv4.tcp_max_syn_backlog = 65536
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 30

# 应用配置
sudo sysctl -p
```

2. **文件描述符限制优化**
```bash
# 编辑limits.conf
sudo nano /etc/security/limits.conf

# 添加以下配置
qmcsms soft nofile 65536
qmcsms hard nofile 65536
* soft nofile 65536
* hard nofile 65536

# 重新登录或重启生效
```

3. **Nginx性能优化**
```nginx
# 在Nginx配置中添加
worker_processes auto;
worker_rlimit_nofile 65535;

events {
    worker_connections 65535;
    use epoll;
    multi_accept on;
}

# 应用配置优化
http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    keepalive_requests 1000;
}
```

4. **应用层优化配置**
```python
# 在app.py中添加性能配置
import os
from werkzeug.middleware.profiler import ProfilerMiddleware

# 生产环境配置
if os.environ.get('FLASK_ENV') == 'production':
    # 启用性能分析（可选）
    app.wsgi_app = ProfilerMiddleware(app.wsgi_app, restrictions=[30])
    
    # 优化线程配置
    app.config['MAX_CONTENT_LENGTH'] = 16 * 1024 * 1024  # 16MB文件上传限制
```

---

#### 通用Linux部署验证

1. **服务状态检查**
   ```bash
   # 检查服务运行状态
   sudo systemctl status qmcsms
   
   # 检查端口监听
   sudo netstat -tlnp | grep 8081
   
   # 检查进程
   ps aux | grep qmcsms
   ```

2. **应用功能测试**
   ```bash
   # 测试HTTP访问
   curl -I http://localhost:8081
   
   # 测试API接口
   curl http://localhost:8081/api/health
   ```

3. **日志监控**
   ```bash
   # 实时查看应用日志
   sudo journalctl -u qmcsms -f
   
   # 查看Nginx访问日志
   sudo tail -f /var/log/nginx/access.log
   ```

4. **性能监控**
   ```bash
   # 监控系统资源使用
   top -p $(pgrep -f "python app.py")
   
   # 监控网络连接
   ss -tlnp | grep 8081
   ```

#### 故障排除

1. **常见问题解决**
   ```bash
   # 服务启动失败
   sudo systemctl status qmcsms
   sudo journalctl -u qmcsms --no-pager
   
   # 端口占用问题
   sudo lsof -i :8081
   sudo netstat -tlnp | grep 8081
   
   # 权限问题
   sudo chown -R qmcsms:qmcsms /opt/qmcsms
   sudo chmod -R 755 /opt/qmcsms
   ```

2. **数据库连接问题**
   ```bash
   # 检查数据库文件权限
   ls -la /opt/qmcsms/instance/
   
   # 重新初始化数据库
   sudo -u qmcsms /opt/qmcsms/venv/bin/python init_db.py
   ```

3. **依赖包问题**
   ```bash
   # 重新安装依赖
   sudo -u qmcsms /opt/qmcsms/venv/bin/pip install --force-reinstall -r requirements.txt
   
   # 清理缓存
   sudo -u qmcsms /opt/qmcsms/venv/bin/pip cache purge
   ```

#### 高级监控和日志管理

1. **日志轮转配置**
```bash
# 创建日志轮转配置
sudo nano /etc/logrotate.d/qmcsms

# 配置内容
/opt/qmcsms/logs/*.log {
    daily
    missingok
    rotate 30
    compress
    delaycompress
    notifempty
    copytruncate
}

# 手动测试日志轮转
sudo logrotate -vf /etc/logrotate.d/qmcsms
```

2. **系统监控脚本**
```bash
#!/bin/bash
# qmcsms-monitor.sh - 系统健康检查脚本

# 检查服务状态
if ! systemctl is-active --quiet qmcsms; then
    echo "QMCSMS服务未运行，尝试重启..."
    sudo systemctl restart qmcsms
    sleep 5
    
    if systemctl is-active --quiet qmcsms; then
        echo "服务重启成功"
    else
        echo "服务重启失败，请检查日志"
        # 发送告警通知
        echo "QMCSMS服务异常" | mail -s "系统告警" admin@example.com
    fi
fi

# 检查端口监听
if ! netstat -tln | grep -q 8081; then
    echo "端口8081未监听，服务可能异常"
fi

# 检查磁盘空间
DISK_USAGE=$(df /opt/qmcsms | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 80 ]; then
    echo "磁盘空间使用率超过80%，请及时清理"
fi

echo "系统健康检查完成"
```

3. **自动化备份脚本**
```bash
#!/bin/bash
# qmcsms-backup.sh - 数据库备份脚本

BACKUP_DIR="/opt/qmcsms/backups"
DATE=$(date +%Y%m%d_%H%M%S)

# 创建备份目录
mkdir -p $BACKUP_DIR

# 备份数据库
cp /opt/qmcsms/qmcsms.db $BACKUP_DIR/qmcsms_$DATE.db

# 备份配置文件
tar -czf $BACKUP_DIR/config_$DATE.tar.gz /opt/qmcsms/*.py /opt/qmcsms/templates/ /opt/qmcsms/static/

# 保留最近7天的备份
find $BACKUP_DIR -name "*.db" -mtime +7 -delete
find $BACKUP_DIR -name "*.tar.gz" -mtime +7 -delete

echo "备份完成: $BACKUP_DIR"
```

4. **性能监控配置**
```bash
# 安装系统监控工具
sudo apt install -y htop iotop iftop nethogs

# 实时监控命令
htop                    # 进程监控
iotop                  # I/O监控
iftop                  # 网络流量监控
nethogs               # 进程网络流量监控
```

---

## 🖥️ 硬件配置推荐

### 最低配置（小型机构）
- **CPU**: 双核处理器 2.0GHz+
- **内存**: 4GB RAM
- **存储**: 50GB SSD
- **网络**: 100Mbps 带宽
- **并发用户**: 支持20个并发用户

### 推荐配置（中型机构）
- **CPU**: 四核处理器 3.0GHz+
- **内存**: 8GB RAM
- **存储**: 100GB SSD
- **网络**: 1Gbps 带宽
- **并发用户**: 支持100个并发用户

### 高性能配置（大型机构）
- **CPU**: 八核处理器 3.5GHz+
- **内存**: 16GB RAM
- **存储**: 200GB SSD + 数据库服务器
- **网络**: 1Gbps 专线
- **并发用户**: 支持500个并发用户

---

## 🌐 浏览器兼容性

### 推荐浏览器
- **Chrome 90+** ⭐⭐⭐⭐⭐ (最佳体验)
- **Firefox 88+** ⭐⭐⭐⭐
- **Edge 90+** ⭐⭐⭐⭐
- **Safari 14+** ⭐⭐⭐

### 技术要求
- 支持HTML5、CSS3、ES6+
- 启用JavaScript
- 支持WebSocket（实时通知功能）
- 建议屏幕分辨率：1920x1080+

### 移动端支持
- 响应式设计，支持平板设备
- 手机端基础功能可用
- 建议使用平板或桌面设备获得最佳体验

---

## 🔧 系统维护

### 日常维护
- **数据库备份**: 每日自动备份数据库文件
- **日志监控**: 实时监控系统日志，设置告警阈值
- **安全更新**: 定期更新系统安全补丁和依赖包
- **临时文件清理**: 每周清理临时文件和缓存
- **性能监控**: 监控系统资源使用情况，及时扩容

### 系统测试工具
系统提供以下测试工具用于验证系统完整性：

```bash
# 检查模板语法
python check_templates.py

# 测试应用程序完整性
python test_app.py

# 清理并重建数据库
python clean_recreate_db.py

# 检查Python依赖
pip list
```

### 安全配置

1. **SSL/TLS配置**（生产环境必须）
```nginx
# Nginx SSL配置示例
server {
    listen 443 ssl http2;
    server_name your-domain.com;
    
    ssl_certificate /etc/ssl/certs/qmcsms.crt;
    ssl_certificate_key /etc/ssl/private/qmcsms.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512;
    
    # HSTS安全头
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
}
```

2. **应用安全配置**
```python
# 在app.py中添加安全配置
app.config.update(
    SESSION_COOKIE_SECURE=True,      # 仅HTTPS传输
    SESSION_COOKIE_HTTPONLY=True,    # 防止XSS
    SESSION_COOKIE_SAMESITE='Lax',   # CSRF防护
    PERMANENT_SESSION_LIFETIME=3600  # 会话超时1小时
)
```

3. **防火墙和安全组配置**
```bash
# 仅开放必要端口
sudo ufw allow 22    # SSH
sudo ufw allow 80     # HTTP (重定向到HTTPS)
sudo ufw allow 443    # HTTPS
sudo ufw enable

# 或者使用iptables
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -j DROP
```

### 性能优化建议
- **日志轮转**: 配置日志文件自动轮转，避免磁盘占满
- **数据库优化**: 定期优化数据库索引和清理碎片
- **CDN加速**: 使用CDN加速静态资源加载
- **Redis缓存**: 配置Redis缓存提升系统性能（可选）
- **负载均衡**: 高并发场景下配置负载均衡

### 更新管理

1. **依赖包更新**
```bash
# 检查可用更新
/opt/qmcsms/venv/bin/pip list --outdated

# 安全更新（仅更新有安全漏洞的包）
/opt/qmcsms/venv/bin/pip install --upgrade package-name

# 全量更新（谨慎使用）
/opt/qmcsms/venv/bin/pip install --upgrade -r requirements.txt
```

2. **系统版本更新流程**
```bash
# 1. 备份当前版本
cp -r /opt/qmcsms /opt/qmcsms_backup_$(date +%Y%m%d)

# 2. 停止服务
sudo systemctl stop qmcsms

# 3. 更新代码（如使用Git）
cd /opt/qmcsms
sudo -u qmcsms git pull

# 4. 更新依赖
sudo -u qmcsms /opt/qmcsms/venv/bin/pip install -r requirements.txt

# 5. 数据库迁移（如有）
sudo -u qmcsms /opt/qmcsms/venv/bin/python migrate_user_roles.py

# 6. 启动服务
sudo systemctl start qmcsms

# 7. 验证更新
curl -I https://your-domain.com
```

3. **回滚流程**
```bash
# 如果更新失败，快速回滚
sudo systemctl stop qmcsms
rm -rf /opt/qmcsms
cp -r /opt/qmcsms_backup_$(date +%Y%m%d) /opt/qmcsms
sudo systemctl start qmcsms
```

---

## 📞 技术支持

### 问题反馈
- 系统使用问题：查看系统内置帮助文档
- 技术问题：联系系统管理员
- 功能建议：通过系统反馈功能提交

### 紧急联系方式
- 系统管理员：系统设置中查看
- 技术支持邮箱：139606886@qq.com

---

## 📄 版本信息

- **当前版本**: v3.1.0
- **发布日期**: 2025年3月
- **更新内容**: 
  - 新增智能ID编码系统，支持角色变更动态更新
  - 支持5种登录方式（账号名、ID、手机号、身份证号、邮箱）
  - 完善教师权限细化体系
  - 增强通知服务，支持多平台通知
  - 优化系统性能和用户体验
  - **新增成绩评分标准系统**：支持传统的1-100分、A+-E评分、优良中差评分三种标准
  - **新增学生评价功能**：基于成绩、奖惩、考勤等数据自动生成评价，支持手动修改
  - **新增奖状打印功能**：支持自定义奖状模板，批量生成和打印奖状
  - **新增帮助文档系统**：完善的使用手册、开发手册、API文档
- **兼容性**: 支持Python 3.8+

---

*本系统为教育机构专用，未经授权不得用于商业用途.*
