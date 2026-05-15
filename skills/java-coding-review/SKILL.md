---
name: java-coding-review
description: "Java 代码验证与审查。编译检查 + 基于编码规范的 diff 审查 + 自动修复。TRIGGER when: 任何 Java 代码变更完成后需要验证时，或用户主动要求审查代码。SKIP: 纯阅读分析、不涉及代码变更的任务。"
---

# Java 代码验证与审查

对当前工作目录中的 Java 代码变更进行编译检查、规范审查和自动修复。

---

## 执行流程

### Step 1：编译检查

运行 `mvn clean compile` 确认编译通过。

- 编译失败时，分析错误信息，定位问题文件并修复
- 修复后重新编译，直到通过

### Step 2：规范审查

1. 运行 `git diff HEAD` 获取本次所有新增/修改的代码（包含已暂存和未暂存的变更）
2. 调用 `java-coding-standards` 技能加载编码规范
3. 逐文件审查 diff 内容，检查是否符合规范
4. 重点关注：
   - 依赖注入方式（构造器注入 vs 字段注入）
   - Controller 是否混入业务逻辑
   - Service 异常处理方式
   - DTO 转换是否规范
   - 查询方式（MyBatis-Plus 用法）
   - 响应格式是否使用 CommResp
5. 发现问题直接修复，不只是报告

### Step 3：汇总

简要列出：
- 实现/修改的内容
- 审查中发现并修复的问题（如有）
