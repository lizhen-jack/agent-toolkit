# AI Agent Toolkit - 测试与使用示例

本文档提供详细的测试用例和使用示例，帮助快速上手和使用 AI Agent Toolkit。

---

## 📦 快速测试

### 1. Token优化器测试

```python
from ai_agent_toolkit import TokenOptimizer

# 创建优化器
optimizer = TokenOptimizer()

# 测试1: 优化prompt
long_prompt = "请帮助我理解这个复杂的代码，你能给我一些详细的解释吗？"
optimized = optimizer.optimize_prompt(long_prompt)
print(f"原始: {long_prompt}")
print(f"优化后: {optimized}")

# 测试2: Token估算
text = "这是一段测试文本，用于验证Token估算功能。"
estimate = optimizer.estimate_tokens(text)
print(f"Token估算: {estimate}")
```

**预期输出**:
```
原始: 请帮助我理解这个复杂的代码，你能给我一些详细的解释吗？
优化后: 理解这个复杂的代码，我一些详细的解释吗？
Token估算: 24
```

---

### 2. 多模态理解增强测试

```python
from pathlib import Path
from ai_agent_toolkit import MultiModalEnhancer

# 创建增强器
enhancer = MultiModalEnhancer()

# 测试1: 检测文件模态
image_path = Path("test_image.jpg")
modality = enhancer.detect_modality(image_path)
print(f"文件模态: {modality}")

# 测试2: 分析图片（模拟）
analysis = enhancer.analyze_image(image_path)
print(f"图片分析: {analysis}")
```

**预期输出**:
```
文件模态: image
图片分析: {
    'type': 'image',
    'path': 'test_image.jpg',
    'analysis': {
        'detected_objects': ['screenshot', 'code', 'documentation'],
        'text_regions': 3,
        'dominant_colors': ['#ffffff', '#282c34']
    }
}
```

---

### 3. API Mock服务生成器测试

```python
from ai_agent_toolkit import APIMockGenerator

# 创建Mock生成器
mock_gen = APIMockGenerator()

# 测试OpenAPI规范
openapi_spec = {
    "openapi": "3.0.0",
    "paths": {
        "/api/users": {
            "get": {
                "summary": "获取用户列表",
                "responses": {
                    "200": {"description": "成功"}
                }
            }
        }
    }
}

# 解析端点
endpoints = mock_gen.parse_openapi(openapi_spec)
print(f"解析到的端点: {len(endpoints)}个")

# 生成Mock代码
mock_server_code = mock_gen.generate_mock_server(endpoints)
print(mock_server_code)
```

**预期输出**:
```
解析到的端点: 1个

# AI生成的Mock服务器
from flask import Flask, jsonify

app = Flask(__name__)


@app.route("/api/users", methods=["GET"])
def mock_get_api_users():
    """获取用户列表"""
    return {
        "success": True,
        "message": "Mock response for GET /api/users",
        "data": {
            "id": "mock_id_123",
            "timestamp": "2026-02-24T16:35:00"
        }
    }


if __name__ == "__main__":
    print("🚀 Mock Server starting on port 5000")
    app.run(port=5000)
```

---

### 4. 代码补全助手测试

```python
from ai_agent_toolkit import CodeCompletionAssistant

# 创建补全助手
assistant = CodeCompletionAssistant()

# 测试1: 基于前缀的建议
suggestions = assistant.suggest_completion("fprint", "python")
print(f"代码补全建议: {len(suggestions)}个")
for s in suggestions:
    print(s)

# 测试2: 上下文感知建议
context = {
    "function_name": "main",
    "parameters": []
}
suggestion = assistant.generate_context_aware_suggestion(context)
print(f"上下文建议:\n{suggestion}")
```

**预期输出**:
```
代码补全建议: 1个
print(f"{text}")

上下文建议:

def main():
    {cursor}

if __name__ == "__main__":
    main()
```

---

## 🧪 完整测试套件

```python
#!/usr/bin/env python3
"""
AI Agent Toolkit 完整测试套件
运行所有测试并报告结果
"""

from ai_agent_toolkit import (
    TokenOptimizer,
    MultiModalEnhancer,
    APIMockGenerator,
    CodeCompletionAssistant,
    AIAgentToolkit
)

def test_token_optimizer():
    """测试Token优化器"""
    print("\n🧪 测试 Token优化器...")

    optimizer = TokenOptimizer()
    long_prompt = "请帮助我理解这个复杂的代码"

    # 测试优化
    optimized = optimizer.optimize_prompt(long_prompt)
    assert len(optimized) < len(long_prompt), "优化后应该更短"
    print(f"  ✅ Prompt优化工作正常")

    # 测试估算
    estimate = optimizer.estimate_tokens(optimized)
    assert estimate > 0, "Token估算应该大于0"
    print(f"  ✅ Token估算工作正常: {estimate} tokens")

def test_multimodal_enhancer():
    """测试多模态增强器"""
    print("\n🧪 测试 多模态增强器...")

    enhancer = MultiModalEnhancer()

    # 测试检测
    from pathlib import Path
    modality = enhancer.detect_modality(Path("test.jpg"))
    assert modality == "image", "应该检测为图片"
    print(f"  ✅ 文件模态检测工作正常")

def test_api_mock_generator():
    """测试API Mock生成器"""
    print("\n🧪 测试 API Mock生成器...")

    mock_gen = APIMockGenerator()
    spec = {"paths": {"/api/test": {"get": {"summary": "测试"}}}}

    # 测试解析
    endpoints = mock_gen.parse_openapi(spec)
    assert len(endpoints) == 1, "应该解析到1个端点"
    print(f"  ✅ OpenAPI解析工作正常")

    # 测试生成
    code = mock_gen.generate_mock_server(endpoints)
    assert "def mock_" in code, "生成的代码应该包含mock函数"
    print(f"  ✅ Mock代码生成工作正常")

def test_code_completion():
    """测试代码补全助手"""
    print("\n🧪 测试 代码补全助手...")

    assistant = CodeCompletionAssistant()

    # 测试补全
    suggestions = assistant.suggest_completion("fprint", "python")
    assert len(suggestions) > 0, "应该有补全建议"
    assert "print(" in suggestions[0], "补全应该包含print"
    print(f"  ✅ 代码补全工作正常")

def test_toolkit_integration():
    """测试工具集集成"""
    print("\n🧪 测试 AI Agent Toolkit集成...")

    toolkit = AIAgentToolkit()

    # 测试工具列表
    tools = toolkit.list_tools()
    assert tools["total_tools"] == 4, "应该有4个工具"
    print(f"  ✅ 工具列表工作正常")

    # 测试工具调用
    result = toolkit.use_tool(
        "token_optimizer",
        "optimize_prompt",
        "请帮助我"
    )
    print(f"  ✅ 工具调用工作正常")

def run_all_tests():
    """运行所有测试"""
    print("=" * 60)
    print("🚀 AI Agent Toolkit 完整测试套件")
    print("=" * 60)

    try:
        test_token_optimizer()
        test_multimodal_enhancer()
        test_api_mock_generator()
        test_code_completion()
        test_toolkit_integration()

        print("\n" + "=" * 60)
        print("✅ 所有测试通过！")
        print("=" * 60)

    except AssertionError as e:
        print(f"\n❌ 测试失败: {e}")
        return False
    except Exception as e:
        print(f"\n❌ 测试错误: {e}")
        return False

    return True

if __name__ == "__main__":
    run_all_tests()
```

---

## 📖 使用示例

### 示例1: 简易对话优化器

```python
from ai_agent_toolkit import AIAgentToolkit

# 创建工具集
toolkit = AIAgentToolkit()

# 用户输入
user_message = "请帮助我理解这个代码，你能解释吗？"

# 优化输入
optimized = toolkit.tools["token_optimizer"].optimize_prompt(user_message)
print(f"优化后的输入: {optimized}")
```

### 示例2: 生成Mock API服务器

```python
from ai_agent_toolkit import AIAgentToolkit

# 你的OpenAPI规范
openapi_spec = {...}

# 生成Mock服务器
toolkit = AIAgentToolkit()
endpoints = toolkit.tools["api_mock_generator"].parse_openapi(openapi_spec)
mock_code = toolkit.tools["api_mock_generator"].generate_mock_server(endpoints)

# 保存到文件
with open("mock_server.py", "w") as f:
    f.write(mock_code)

# 运行: python3 mock_server.py
```

---

## 🎯 下一步

1. 运行完整测试套件: `python3 test_agent_toolkit.py`
2. 将test_agent_toolkit.py保存到项目目录
3. 运行测试并报告结果

---

_创建时间: 2026-02-24_
_作者: 小龙（Little Dragon）_
_版本: v1.0_
