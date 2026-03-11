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

### 🎯 核心功能

- **学生信息管理** - 学生档案、班级分配、基本信息维护
- **成绩管理系统** - 多科目成绩录入、统计分析、权限控制
- **考勤管理** - 日常考勤记录、请假审批、统计报表
- **奖惩管理** - 奖励与惩罚记录、行为分析
- **通知系统** - 系统公告、个人消息、邮件通知
- **权限管理** - 多角色权限控制、数据权限隔离

### ✨ 新增功能特性

#### 1. 科目类别管理系统
- 支持12种标准科目分类（语文、数学、英语、物理、化学、生物、政治、历史、地理、体育、音乐、美术）
- 科目代码规范化管理，支持科目类别筛选

#### 2. 教师权限细化体系
- **班主任**：分管班级所有学生信息和成绩的全面管理权限
- **科目老师**：仅限任教科目成绩的管理权限，可查看学生基本信息
- **代课老师**：与科目老师相同权限，支持临时代课管理

#### 3. 基于科目的成绩权限控制
- 双重权限验证（班级权限 + 科目权限）
- 动态数据过滤，确保数据安全
- 灵活的权限分配机制

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
   git clone https://github.com/your-repo/QMCSMS.git
   
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
   sudo -u qmcsms git clone https://github.com/your-repo/QMCSMS.git .
   
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

**注意**: HarmonyOS部署需要特殊的环境配置

1. **环境准备**
   ```bash
   # HarmonyOS 2.0+ 通常基于AOSP环境
   # 需要确保Python环境可用
   
   # 安装必要的包管理器
   curl -sSL https://install.python-poetry.org | python3 -
   
   # 或者使用系统包管理器
   hpm install python3
   ```

2. **HarmonyOS特定配置**
   ```bash
   # 配置HarmonyOS权限
   hdc shell app install -p /opt/qmcsms
   
   # 设置网络权限
   hdc shell aa grant com.qmcsms android.permission.INTERNET
   ```

3. **容器化部署（推荐）**
   ```dockerfile
   # 使用Docker容器部署
   FROM harmonyos/python:3.8
   
   WORKDIR /app
   COPY . .
   RUN pip install -r requirements.txt
   
   EXPOSE 8081
   CMD ["python", "app.py"]
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
- 定期备份数据库
- 监控系统日志
- 更新安全补丁
- 清理临时文件

### 性能优化建议
- 定期清理日志文件
- 优化数据库索引
- 使用CDN加速静态资源
- 配置Redis缓存（可选）

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

- **当前版本**: v2.0.0
- **发布日期**: 2025年3月
- **更新内容**: 新增科目管理系统、教师权限细化、成绩权限控制
- **兼容性**: 支持Python 3.8+

---

*本系统为教育机构专用，未经授权不得用于商业用途。*
