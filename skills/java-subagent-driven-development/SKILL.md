---
name: java-subagent-driven-development
description: "Java 后端专用。基于设计文档派生子代理进行并行 Java 后端开发。TRIGGER when: 用户指定一个设计文档目录（如 docs/plans/xxx/）并要求实现其中的接口。SKIP: 单个小改动、纯阅读分析、不涉及设计文档的任务。"
---

# 子代理驱动的 Java 后端开发

基于技术设计文档目录，将接口实现任务分派给并行子代理。

---

## 执行流程

### Phase 1：分析与规划

1. 读取 `00-main-design.md` 和所有切片文件，理解实体、接口、依赖关系
2. 将任务分为：
   - **基础层**（必须先完成）：Entity、Mapper、公共 Model
   - **接口层**（可并行）：各 Controller + Service
3. 生成任务列表（TaskCreate），向用户展示执行计划，确认后开始

### Phase 2：基础层实现

用子代理完成 Entity、Mapper、公共 Model。这步必须先完成，后续接口都依赖这些类。

### Phase 3：并行接口实现

对独立的接口，用 Agent tool 并行派发子代理。

**子代理 prompt 要点：**
- 要求先调用 `java-coding-standards` 技能加载编码规范
- 包含该接口的设计文档全文
- 列出已创建的基础类路径（直接使用，不要重新创建）

### Phase 4：验证

调用 `java-code-review` 技能，完成编译检查和规范审查。

---

## 并行分组原则

- 操作同一文件的接口放同一个子代理（避免写冲突）
- 有明确依赖的接口串行执行
- 独立接口尽量并行

---

## 子代理配置

| 参数 | 值 |
|------|-----|
| subagent_type | `general-purpose` |
| isolation | 不使用 worktree（所有子代理写入同一工作目录） |

---

## 下一步

代码实现完成后，建议调用 `/api-test-verify` 运行接口测试验证。
