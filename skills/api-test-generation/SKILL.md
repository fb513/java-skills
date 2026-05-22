---
name: api-test-generation
description: "基于 java-writing-plans 生成的设计文档，生成 pytest 接口测试（api-tests/）。分析接口间依赖关系，构建 fixture 链和测试执行顺序。TRIGGER when: 用户指定一个设计文档目录（如 docs/plans/xxx/）并要求生成接口测试。SKIP: 单个接口的简单测试、不涉及设计文档的任务。"
---

# 基于设计文档的接口测试生成

读取设计文档目录，生成 pytest 接口测试套件到 `api-tests/` 目录。

---

## 输入

用户提供设计文档目录路径（如 `docs/plans/20260501-xxx/`）。如果用户未指定，主动询问。

---

## 约束

1. **使用现有测试框架** — 必须使用 `core/client.py`（ApiClient）和 `core/assertions.py`，不引入新依赖
2. **测试数据自给自足** — 前置数据通过 fixture 创建，不依赖数据库预置数据
3. **SSE 接口标记跳过** — 流式接口用 `@pytest.mark.requires_ai`，只验证连接建立

---

## 执行流程

### Phase 1：读取设计文档 + 学习现有测试风格

1. 读取 `00-main-design.md` 和所有切片文件，理解接口列表和依赖关系
2. 读取现有测试代码学习项目约定：
   - `api-tests/conftest.py` — CLI 选项、配置加载、全局 fixture 和跳过标记
   - `api-tests/tests/conftest.py` — 测试级 fixture 结构
   - `api-tests/core/assertions.py` — 可用断言
   - `api-tests/core/client.py` — ApiClient API
   - 1-2 个现有测试文件 — 命名惯例和组织方式

### Phase 2：生成测试代码

输出位置：`api-tests/tests/test_NN_<module>.py`（编号接续现有最大编号）

如需新增共享 fixture，追加到 `api-tests/tests/conftest.py`。

### Phase 3：验证语法正确，向用户汇报并提示运行命令

优先运行 `cd api-tests && python -m pytest --collect-only` 验证测试可被收集；如果环境缺依赖或服务不可用，至少运行语法检查并说明阻塞原因。

---

## 下一步

测试生成完成后，建议调用 `/api-test-verify` 运行测试并验证。
