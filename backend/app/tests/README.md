# Echo AI 后端自动化API测试

## 📋 概述

本测试套件为Echo AI后端服务提供完整的自动化API测试，基于产品需求文档(PRD)和API规范，系统性地验证后端功能完整性，识别不可用接口，确保系统质量。

## 🏗️ 测试结构

```
app/tests/
├── __init__.py                    # 测试包初始化
├── conftest.py                    # 测试配置和共享fixture
├── test_auth_permissions.py       # 认证与权限测试
├── test_core_ai_flow.py          # 核心AI交互流程测试
├── test_developer_features.py     # 开发者功能测试
├── test_performance_stability.py  # 性能与稳定性测试
├── test_error_handling.py         # 错误处理测试
└── README.md                      # 本文件
```

## 🚀 快速开始

### 1. 环境准备

```bash
# 激活虚拟环境
cd backend
source venv/bin/activate

# 安装测试依赖
pip install pytest pytest-cov pytest-asyncio
```

### 2. 运行测试

#### 使用便捷脚本（推荐）
```bash
# 运行所有测试
python run_tests.py

# 运行特定类型测试
python run_tests.py --auth          # 认证测试
python run_tests.py --core          # 核心功能测试
python run_tests.py --dev           # 开发者功能测试
python run_tests.py --performance   # 性能测试
python run_tests.py --errors        # 错误处理测试

# 快速测试模式
python run_tests.py --quick

# 详细输出
python run_tests.py --verbose

# 生成覆盖率报告
python run_tests.py --coverage
```

#### 使用pytest直接运行
```bash
# 运行所有测试
pytest app/tests/

# 运行特定测试文件
pytest app/tests/test_auth_permissions.py

# 运行特定测试类
pytest app/tests/test_auth_permissions.py::TestUserAuthentication

# 运行特定测试方法
pytest app/tests/test_auth_permissions.py::TestUserAuthentication::test_user_login_success

# 详细输出
pytest -v app/tests/

# 生成覆盖率报告
pytest --cov=app --cov-report=html app/tests/
```

## 📊 测试覆盖范围

### 1. 认证与权限测试 (`test_auth_permissions.py`)

**测试目标**: 验证JWT认证机制和权限控制

**测试用例**:
- ✅ 用户认证流程（登录、注册）
- ✅ 权限控制验证（角色层级、接口访问控制）
- ✅ Token验证（有效、无效、过期）
- ✅ 边界情况（空凭据、特殊字符、Unicode）

**验收标准**:
- 三种角色登录成功率 ≥ 95%
- 权限验证准确率 = 100%
- 认证错误处理覆盖率 ≥ 90%

### 2. 核心AI交互流程测试 (`test_core_ai_flow.py`)

**测试目标**: 验证AI理解用户意图和工具调用功能

**测试用例**:
- ✅ 意图解析接口（天气查询、翻译、无法识别意图）
- ✅ 确认执行流程（用户确认机制）
- ✅ 直接工具执行（翻译、天气查询）
- ✅ 端到端流程（完整AI交互）

**验收标准**:
- 常见意图识别准确率 ≥ 80%
- 工具调用成功率 ≥ 90%
- 意图解析时间 ≤ 200ms
- 工具执行时间 ≤ 300ms
- 端到端响应时间 ≤ 500ms

### 3. 开发者功能测试 (`test_developer_features.py`)

**测试目标**: 验证开发者控制台功能

**测试用例**:
- ✅ 工具管理（创建、更新、删除、测试）
- ✅ 应用管理（创建、更新、删除、发布）
- ✅ MCP服务器状态监控
- ✅ 权限边界测试

**验收标准**:
- 开发者功能可用性 = 100%
- 权限控制有效性 = 100%

### 4. 性能与稳定性测试 (`test_performance_stability.py`)

**测试目标**: 验证系统性能符合PRD要求

**测试用例**:
- ✅ 并发请求测试（≥10 QPS）
- ✅ 响应时间要求验证
- ✅ 系统稳定性测试
- ✅ 资源使用监控

**验收标准**:
- 并发处理能力 ≥ 10 QPS
- 系统可用性 ≥ 99%
- 长时间运行稳定性 ≥ 90%

### 5. 错误处理测试 (`test_error_handling.py`)

**测试目标**: 验证异常情况得到正确处理

**测试用例**:
- ✅ 认证错误处理
- ✅ 参数验证错误
- ✅ 业务逻辑错误
- ✅ 边界条件测试
- ✅ 错误响应格式验证

**验收标准**:
- 异常情况处理覆盖率 ≥ 90%
- 错误响应格式一致性 = 100%

## 🔧 测试配置

### 测试环境设置

测试使用内存数据库，避免影响生产数据：

```python
# conftest.py
SQLALCHEMY_DATABASE_URL = "sqlite:///:memory:"
```

### 测试用户配置

测试使用预置的测试账号：

```python
test_users = {
    "user": {
        "username": "testuser_5090",
        "password": "8lpcUY2BOt",
        "role": "user"
    },
    "developer": {
        "username": "devuser_5090",
        "password": "mryuWTGdMk",
        "role": "developer"
    },
    "admin": {
        "username": "adminuser_5090",
        "password": "SAKMRtxCjT",
        "role": "admin"
    }
}
```

### 测试数据隔离

每个测试使用独立的session_id，避免数据冲突：

```python
def generate_session_id():
    return f"test-session-{datetime.now().strftime('%Y%m%d%H%M%S')}"
```

## 📈 测试报告

### 覆盖率报告

```bash
# 生成HTML覆盖率报告
pytest --cov=app --cov-report=html app/tests/

# 查看报告
open htmlcov/index.html
```

### 测试结果分析

```bash
# 运行测试并显示最慢的测试
pytest --durations=10 app/tests/

# 生成JUnit XML报告
pytest --junitxml=test-results.xml app/tests/
```

## 🚨 常见问题

### 1. 测试环境问题

**问题**: 数据库连接失败
**解决**: 确保使用内存数据库，检查conftest.py配置

**问题**: 依赖包缺失
**解决**: 安装测试依赖 `pip install pytest pytest-cov pytest-asyncio`

### 2. 测试执行问题

**问题**: 测试超时
**解决**: 使用 `--quick` 模式跳过耗时测试

**问题**: 权限错误
**解决**: 检查测试用户配置和JWT密钥设置

### 3. 性能测试问题

**问题**: 性能测试失败
**解决**: 检查系统资源，调整并发数量

## 🔄 持续集成

### GitHub Actions配置示例

```yaml
name: API Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install pytest pytest-cov
      - name: Run tests
        run: |
          cd backend
          python run_tests.py --coverage
```

## 📚 相关文档

- [后端集成测试用例与验收标准](../docs/后端集成测试用例与验收标准.md)
- [前后端对接与API规范](../docs/前后端对接与API规范.md)
- [产品需求文档(PRD)](../docs/产品开发需求文档PRD.md)

## 🤝 贡献指南

### 添加新测试

1. 在相应的测试文件中添加测试方法
2. 遵循命名规范：`test_<功能>_<场景>`
3. 添加详细的文档字符串
4. 确保测试数据隔离

### 测试最佳实践

- ✅ 每个测试应该独立且可重复
- ✅ 使用描述性的测试名称
- ✅ 验证成功和失败情况
- ✅ 清理测试数据
- ✅ 添加适当的断言消息

## 📞 支持与反馈

如有问题或建议，请：

1. 检查测试日志和错误信息
2. 查看相关文档
3. 联系后端开发团队

---

**最后更新**: 2025-01-14
**测试版本**: v1.0.0
**维护团队**: 后端开发团队
