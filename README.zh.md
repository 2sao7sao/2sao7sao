# 2sao7sao

**一条可审计的 Agent 能力栈：系统如何建模"人"、如何执行经过审查的知识、以及如何拒绝为没验证过的东西背书。**

<p align="center">
  <a href="README.md">English</a> ·
  <a href="#能力栈">能力栈</a> ·
  <a href="#项目">项目</a> ·
  <a href="#这里不证明什么">边界</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/%E8%83%BD%E5%8A%9B%E6%A0%88-%E8%AE%B0%E5%BF%86_%C2%B7_%E7%9F%A5%E8%AF%86_%C2%B7_%E5%88%A4%E6%8D%AE-b8eee4" alt="记忆、知识、判据" />
  <img src="https://img.shields.io/badge/%E9%98%B6%E6%AE%B5-%E7%A0%94%E7%A9%B6%E5%8E%9F%E5%9E%8B-ff5aa5" alt="研究原型" />
  <img src="https://img.shields.io/badge/%E5%85%AC%E5%BC%80%E4%BB%93%E5%BA%93-3-b8eee4" alt="三个公开仓库" />
</p>

## 能力栈

一个 Agent 同时受三件事限制：它对协作者的理解、它能执行的知识、以及"任务确实完成了"的证据。

下面三个仓库各占一层，共享同一条规则：**测试通过、CI 变绿、或一句自信的话，都不能证明它看起来在证明的东西。**

```text
人     --> 记忆      EvolveMemory        系统认为你是谁，以及什么时候必须改
知识   --> 编排      EvolveKB            经过审查的文档如何变成可执行流程
判据   --> 评估      Lean-4-Agent-Eval   哪些主张真的被验证了，证明在哪里停止
```

| 层 | 仓库 | 当前状态 | 明确边界 |
| --- | --- | --- | --- |
| 人的记忆 | [EvolveMemory](https://github.com/2sao7sao/EvolveMemory) | 研究原型：v2 治理存储、读取硬控制、有界真实 DeepSeek 宿主；`main` 上 157 个测试，宿主分支上 263 个 | 体验验收 U01–U08 未通过；真实 24 轮试验暴露了偏好纠正缺陷 |
| 可执行知识 | [EvolveKB](https://github.com/2sao7sao/EvolveKB) | 本地原型，已打 tag `v0.4.0`，[文档站](https://2sao7sao.github.io/EvolveKB/)已上线，97 个测试 | 尚无真实文档的全链路运行；不宣称生产可用或检索更优 |
| 判据 | [Lean-4-Agent-Eval](https://github.com/2sao7sao/Lean-4-Agent-Eval) | M0–M2 已实现（M2 为离线）：55 个场景、独立参考复算、11 个 Lean 定理 | 定理只覆盖判定聚合；真实 Agent 执行与 trace 采集属于 M3，尚未建设 |

## 项目

### EvolveMemory —— 会自我修正的记忆

面向长期协作的记忆运行时。写入走规则/模型提取、加权评估与操作规划，落到归一化存储；
读取过硬控制、检索与 gating 才能进入 prompt。**检索不等于使用。**

诚实状态：机制经过测试，语义尚未稳定。真实试验复现了"否定句变成正支持"，所以项目保留
Draft 宿主 PR 与开放的 issue，而不是宣布通过。

[仓库](https://github.com/2sao7sao/EvolveMemory) · [实测结果](https://github.com/2sao7sao/EvolveMemory/blob/feat/live-project-continuation-20261116/docs/live-evaluation-results.md) · [运行时参考](https://github.com/2sao7sao/EvolveMemory/blob/main/docs/runtime-reference.md)

### EvolveKB —— 可执行的知识，而不只是被检索的文档

把文档变成有证据支撑的 playbook，让 Agent 能执行、测试，并通过审查更新。playbook 决定
怎么用证据，检索只负责供给。变更以 proposal 形式进入审查，带影响信息、gates 与回滚路径。

诚实状态：框架足够做演示，证据还很薄。还没有一份真实文档跑通
evidence → playbook → trace → proposal → rollback 全链路；`v0.4.0` 是源码 tag，不是生产承诺。

[仓库](https://github.com/2sao7sao/EvolveKB) · [文档站](https://2sao7sao.github.io/EvolveKB/) · [发布 v0.4.0](https://github.com/2sao7sao/EvolveKB/releases/tag/v0.4.0)

### Lean-4-Agent-Eval —— 把验收变成可核对的合同

从一段简短沟通生成验收合同，据此构建版本化评估集，再用形式化模型核对判定。重点不是分数，
而是说清**哪个主张依赖哪份证据**。

诚实状态：M0–M2 是真实且离线的。目前还没有 Agent 在这个框架下真实执行，所以它证明的是
自己的账能对上，而不是它会给 Agent 打分。

[仓库](https://github.com/2sao7sao/Lean-4-Agent-Eval) · [评估合同](https://github.com/2sao7sao/Lean-4-Agent-Eval/blob/main/docs/evaluation-contract.md) · [验证边界](https://github.com/2sao7sao/Lean-4-Agent-Eval/blob/main/docs/verification-boundary.md)

## 这里不证明什么

- **测试全绿不等于理解正确。** EvolveMemory 在 `main` 上 157 个测试通过，真实 24 轮对话仍生成了错误画像；修复只存在于 Draft 分支，尚未进入 release。
- **框架发布不等于质量。** EvolveKB 只是原型，它自己的 README 也这么写。
- **形式化定理不等于实现正确。** Lean-4-Agent-Eval 的 11 个定理只覆盖判定聚合，Python 实现、
  数据真实性、PPT 质量都在证明范围之外。
- **三者都未达到生产标准**：没有鉴权、租户隔离、交付回执，也没有纵向用户研究。

## 工作原则

| 原则 | 具体表现 |
| --- | --- |
| 先能复现，再谈结论 | 仓库里每个数字都配着产生它的命令和所属 commit |
| 分层报告证据 | 测试、模型调用、用户验收、业务结果分开记录，从不加总成一个分数 |
| 说明证明在哪里停止 | 每份 README、release 和结果文档都写明证据没覆盖什么 |

## 其他

- 文档站：[2sao7sao.github.io/EvolveKB](https://2sao7sao.github.io/EvolveKB/)
- 联系方式：在对应主题的仓库里开 issue；我只在 issue 里回复。
