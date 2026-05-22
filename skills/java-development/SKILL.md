---
name: java-development
description: "Java 后端专用。基于设计文档进行 Java 后端开发。TRIGGER when: 用户指定一个设计文档目录（如 docs/plans/xxx/）并要求实现其中的接口。SKIP: 单个小改动、纯阅读分析、不涉及设计文档的任务。"
---

# 基于设计文档的 Java 后端开发

基于技术设计文档目录，按依赖关系完成 Java 后端接口实现。

---

## 执行流程

### Phase 1：分析与规划

1. 读取 `00-main-design.md` 和所有切片文件，理解实体、接口、依赖关系
2. 将任务分为：
   - **基础层**（必须先完成）：Entity、Mapper、公共 Model
   - **接口层**（可并行）：各 Controller + Service
3. 生成任务列表，明确实现顺序、可并行部分和验证方式

### Phase 2：基础层实现

先完成 Entity、Mapper、公共 Model。这步必须先完成，后续接口都依赖这些类。

### Phase 3：接口实现

按依赖关系实现各 Controller + Service。独立接口可并行推进；共享同一文件或存在明确依赖的接口串行处理。

### Phase 4：验证

调用 `java-coding-review` 技能，完成编译检查和规范审查。

---

## 并行分组原则

- 操作同一文件的接口放在同一批次处理，避免写冲突
- 有明确依赖的接口串行执行
- 独立接口尽量并行

---

## 下一步

代码实现完成后，建议调用 `/api-test-verify` 运行接口测试验证。
