# QMCSMS 开发文档

## 📋 目录

- [项目概述](#项目概述)
- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [核心功能模块](#核心功能模块)
- [数据库设计](#数据库设计)
- [API接口](#api接口)
- [开发规范](#开发规范)
- [部署指南](#部署指南)
- [常见问题](#常见问题)

## 项目概述

乾明学生管理系统(QMCSMS)是一个基于Flask框架的现代化学生信息管理平台，专为教育机构设计，提供全面的学生管理解决方案。

### 核心功能
- 学生信息管理
- 成绩管理（支持多种评分标准）
- 考勤管理
- 奖惩管理
- 学生评价（自动+手动）
- 奖状打印
- 通知系统
- 权限管理

## 技术栈

### 后端技术
- **Python**: 3.8+
- **Flask**: 2.0+
- **Flask-SQLAlchemy**: 数据库ORM
- **Flask-Login**: 用户认证
- **Flask-WTF**: 表单处理
- **Werkzeug**: WSGI工具集

### 前端技术
- **HTML5**: 语义化标记
- **CSS3**: 样式设计
- **JavaScript**: 交互逻辑
- **Bootstrap**: 响应式框架
- **Font Awesome**: 图标库

### 数据库
- **SQLite**: 开发环境
- **MySQL/PostgreSQL**: 生产环境（可选）

### 其他工具
- **SQLAlchemy**: ORM框架
- **Alembic**: 数据库迁移
- **Jinja2**: 模板引擎
- **WTForms**: 表单验证

## 项目结构

```
QMCSMS/
├── app.py                      # 应用入口
├── models.py                   # 数据模型
├── forms.py                    # 表单定义
├── routes/                     # 路由模块
│   ├── routes.py              # 总路由注册
│   ├── sys_core.py            # 核心管理路由
│   ├── sys_aux.py             # 辅助管理路由
│   ├── sys_pv.py              # 权限验证路由
│   ├── help_manual.py         # 使用手册路由
│   ├── help_dev.py            # 开发手册路由
│   └── help_center.py         # 帮助中心路由
├── templates/                  # 模板文件
│   ├── base.html              # 基础模板
│   ├── dashboard.html         # 仪表板
│   ├── student/               # 学生管理模板
│   ├── score/                 # 成绩管理模板
│   ├── reward/                # 奖励管理模板
│   ├── punishment/            # 惩罚管理模板
│   ├── attendance/            # 考勤管理模板
│   └── Help/                  # 帮助文档模板
├── static/                     # 静态文件
│   ├── css/                   # 样式文件
│   ├── js/                    # JavaScript文件
│   └── iconfont/              # 图标字体
├── utils/                      # 工具模块
│   ├── grading_utils.py       # 评分系统工具
│   └── evaluation_utils.py    # 评价生成工具
├── services/                   # 服务模块
│   └── login_service.py       # 登录服务
├── instance/                   # 实例配置
├── requirements.txt            # 依赖列表
├── README.md                   # 项目说明
└── DEVELOPMENT_GUIDE.md        # 开发文档
```

## 核心功能模块

### 1. 成绩评分系统

#### 功能特点
- 支持三种评分标准：
  - 传统的1-100分制
  - A+-E等级制（8个等级）
  - 优良中差等级制（5个等级）
- 系统设置中可灵活切换
- 自动转换和显示

#### 主要文件
- `utils/grading_utils.py` - 评分转换工具
- `routes/sys_core.py` - 成绩管理路由
- `templates/score/` - 成绩管理模板

#### 使用示例
```python
from utils.grading_utils import convert_to_grading_system

# 转换分数
grade = convert_to_grading_system(92.5, 'a_plus_e')  # 返回 "A"
grade = convert_to_grading_system(92.5, 'excellent_poor')  # 返回 "优秀"
grade = convert_to_grading_system(92.5, 'numeric')  # 返回 "92.5"
```

### 2. 学生评价系统

#### 功能特点
- 基于多维度数据自动生成评价
- 支持手动修改和确认
- 批量生成和打印

#### 评价维度
1. **学业表现**：基于成绩数据
2. **班级贡献**：基于班级职务
3. **行为表现**：基于奖惩和考勤

#### 主要文件
- `utils/evaluation_utils.py` - 评价生成工具
- `models.py` - 评价数据模型
- `templates/score/evaluation.html` - 评价页面模板

#### 使用示例
```python
from utils.evaluation_utils import generate_auto_evaluation

# 生成学生评价
evaluation = generate_auto_evaluation(student, "2024-2025第一学期")
print(evaluation['academic_performance'])  # 学业表现
print(evaluation['class_contribution'])    # 班级贡献
print(evaluation['behavior_performance'])  # 行为表现
```

### 3. 奖状打印系统

#### 功能特点
- 自定义奖状模板
- 批量生成和打印
- 支持多种奖状类型

#### 主要文件
- `models.py` - 奖状模板和记录模型
- `templates/score/print_certificates.html` - 打印模板

### 4. 权限管理系统

#### 功能特点
- 多角色支持（管理员、教师、查看员）
- 基于科目和班级的细粒度权限控制
- 动态权限验证

#### 权限类型
- 班主任：分管班级所有权限
- 科目老师：任教科目权限
- 代课老师：与科目老师相同
- 管理员：全部权限

## 数据库设计

### 核心模型

#### User（用户模型）
```python
- id: 主键
- username: 用户名
- password: 密码（加密）
- role: 角色（admin/teacher/supervisor）
- permissions: 权限列表（JSON）
- teaching_type: 教师类型
- teaching_subjects: 任教科目（JSON）
```

#### Student（学生模型）
```python
- id: 主键
- student_no: 学号
- name: 姓名
- class_id: 班级ID（外键）
- gender: 性别
- birth_date: 出生日期
- status: 状态（在读/休学/毕业/退学）
```

#### Score（成绩模型）
```python
- id: 主键
- student_id: 学生ID（外键）
- subject: 科目
- score: 分数
- semester: 学期
- exam_date: 考试日期
```

#### StudentEvaluation（学生评价模型）
```python
- id: 主键
- student_id: 学生ID（外键）
- semester: 学期
- academic_performance: 学业表现
- class_contribution: 班级贡献
- behavior_performance: 行为表现
- auto_evaluation: 自动生成的评价
- manual_evaluation: 手动修改的评价
```

#### SystemSetting（系统设置模型）
```python
- id: 主键
- user_id: 用户ID（外键）
- score_grading_system: 评分系统（numeric/a_plus_e/excellent_poor）
- certificate_template: 奖状模板
- evaluation_template: 评价模板
```

## API接口

### 成绩评分API

#### 获取当前评分系统
```http
GET /api/grading-system

Response:
{
    "system": "a_plus_e",
    "description": { ... }
}
```

#### 转换分数
```http
POST /api/convert-score

Request:
{
    "score": 92.5,
    "system": "a_plus_e"
}

Response:
{
    "original_score": 92.5,
    "converted_grade": "A",
    "system": "a_plus_e"
}
```

### 学生评价API

#### 生成评价
```http
POST /api/evaluations/generate

Request:
{
    "student_id": 123,
    "semester": "2024-2025第一学期"
}
```

#### 查询评价
```http
GET /api/evaluations/{student_id}?semester=2024-2025第一学期
```

#### 批量生成
```http
POST /api/evaluations/batch-generate

Request:
{
    "student_ids": [123, 124, 125],
    "semester": "2024-2025第一学期"
}
```

## 开发规范

### 代码规范

1. **命名规范**
   - 变量：小写字母 + 下划线（如：`student_name`）
   - 函数：小写字母 + 下划线（如：`get_student_info()`）
   - 类：大驼峰命名（如：`StudentModel`）
   - 常量：全大写字母 + 下划线（如：`MAX_SCORE`）

2. **代码格式**
   - 使用4个空格缩进
   - 行长度不超过120字符
   - 函数和类之间空2行
   - 方法之间空1行

3. **注释规范**
   - 函数必须有docstring
   - 复杂逻辑需要注释说明
   - 重要参数需要类型注解

### Git规范

1. **提交信息**
   ```
   类型: 简短描述
   
   详细说明（可选）
   
   关联issue: #123
   ```

   类型包括：
   - feat: 新功能
   - fix: 修复bug
   - docs: 文档更新
   - style: 格式调整
   - refactor: 重构
   - test: 测试相关
   - chore: 杂项

2. **分支管理**
   - main: 主分支，稳定版本
   - develop: 开发分支
   - feature/xxx: 功能分支
   - bugfix/xxx: 修复分支
   - hotfix/xxx: 紧急修复分支

### 安全规范

1. **密码安全**
   - 使用Werkzeug的`generate_password_hash`加密
   - 密码复杂度要求（8位以上，包含大小写字母和数字）

2. **输入验证**
   - 所有用户输入必须验证
   - 使用WTForms进行表单验证
   - 防止SQL注入和XSS攻击

3. **权限控制**
   - 所有敏感操作需要权限验证
   - 使用`@login_required`和`@permission_required`装饰器
   - 实施最小权限原则

## 部署指南

### 开发环境部署

1. **克隆项目**
   ```bash
   git clone https://github.com/djrolin2023/qmcsms/QMCSMS.git
   cd QMCSMS
   ```

2. **创建虚拟环境**
   ```bash
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   venv\Scripts\activate     # Windows
   ```

3. **安装依赖**
   ```bash
   pip install -r requirements.txt
   ```

4. **初始化数据库**
   ```bash
   python init_db.py
   ```

5. **启动应用**
   ```bash
   python app.py
   ```

6. **访问系统**
   打开浏览器访问：`http://localhost:8081`

### 生产环境部署

#### Linux部署

1. **创建系统用户**
   ```bash
   sudo useradd -r -s /bin/false -d /opt/qmcsms qmcsms
   sudo mkdir -p /opt/qmcsms
   sudo chown qmcsms:qmcsms /opt/qmcsms
   ```

2. **部署应用文件**
   ```bash
   cd /opt/qmcsms
   sudo -u qmcsms git clone https://github.com/djrolin2023/qmcsms/QMCSMS.git .
   ```

3. **创建虚拟环境**
   ```bash
   sudo -u qmcsms python3 -m venv venv
   sudo -u qmcsms /opt/qmcsms/venv/bin/pip install -r requirements.txt
   ```

4. **创建Systemd服务**
   ```bash
   sudo nano /etc/systemd/system/qmcsms.service
   ```

   添加以下内容：
   ```ini
   [Unit]
   Description=QMCSMS Student Management System
   After=network.target
   
   [Service]
   Type=simple
   User=qmcsms
   Group=qmcsms
   WorkingDirectory=/opt/qmcsms
   Environment=PATH=/opt/qmcsms/venv/bin
   ExecStart=/opt/qmcsms/venv/bin/python app.py
   Restart=always
   
   [Install]
   WantedBy=multi-user.target
   ```

5. **启动服务**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable qmcsms
   sudo systemctl start qmcsms
   ```

#### Docker部署

```dockerfile
# Dockerfile
FROM python:3.8-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple/

COPY . .

EXPOSE 8081

CMD ["python", "app.py"]
```

构建和运行：
```bash
docker build -t qmcsms .
docker run -d -p 8081:8081 --name qmcsms qmcsms
```

## 常见问题

### 1. 数据库初始化失败

**问题**：运行`python init_db.py`时出错

**解决**：
- 检查Python版本（3.8+）
- 检查依赖是否安装完整
- 删除旧的`qmcsms.db`文件后重试

### 2. 端口被占用

**问题**：启动时提示端口8081被占用

**解决**：
- 查找占用进程：`lsof -i :8081`
- 终止进程：`kill -9 PID`
- 或修改应用端口：修改`app.py`中的`port`参数

### 3. 权限不足

**问题**：无法访问某些功能

**解决**：
- 检查用户角色和权限
- 确保已正确配置`assigned_classes`和`teaching_subjects`
- 管理员用户拥有所有权限

### 4. 静态文件加载失败

**问题**：CSS、JS文件无法加载

**解决**：
- 检查静态文件路径配置
- 确保`static`目录存在且可访问
- 生产环境配置Nginx反向代理

### 5. 数据库连接错误

**问题**：SQLAlchemy连接错误

**解决**：
- 检查数据库文件权限
- 确保磁盘空间充足
- 检查数据库URL配置

### 6. 性能问题

**优化建议**：
- 使用Redis缓存（可选）
- 配置数据库索引
- 优化查询语句
- 使用CDN加速静态资源

### 7. 安全问题

**安全建议**：
- 生产环境使用HTTPS
- 配置强密码策略
- 定期更新依赖包
- 限制管理员IP访问
- 启用日志监控

## 开发建议

### 1. 代码组织
- 遵循MVC设计模式
- 保持函数和类的单一职责
- 合理使用蓝图（Blueprint）组织路由

### 2. 性能优化
- 使用分页处理大量数据
- 避免N+1查询问题
- 合理使用缓存
- 优化静态资源

### 3. 测试
- 编写单元测试
- 编写集成测试
- 测试边界条件
- 测试权限控制

### 4. 文档
- 更新API文档
- 更新使用手册
- 添加代码注释
- 记录设计决策

## 贡献指南

欢迎贡献代码、提出问题或建议！

### 提交Bug
1. 使用GitHub Issues提交
2. 提供详细的复现步骤
3. 提供环境信息
4. 提供错误日志

### 提交功能
1. 先创建Issue讨论
2. Fork项目并创建功能分支
3. 编写代码和测试
4. 提交Pull Request

### 代码审查
- 确保代码符合规范
- 确保功能测试通过
- 确保文档更新
- 确保安全性检查

## 许可证

本项目采用GPL v3.0许可证。详见LICENSE文件。

## 联系方式

- **项目地址**: https://github.com/djrolin2023/qmcsms/QMCSMS
- **技术支持**: 139606886@qq.com
- **问题反馈**: 通过GitHub Issues提交

---

*本文档最后更新：2025年3月*
