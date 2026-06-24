# ashima

> [English](README.md)

> *阿诗玛以歌闻名。*

`ashima` 是 AI Agent 编排的路由技能集。它检测用户意图，将任务分发到对应的专业技能——独唱、二重唱、或合唱。

## 三种声音

| 技能 | 模式 | 做什么 |
|------|------|--------|
| **aria** | 编码编排 | 编排 Codex 实现 + Claude 审查，作用域明确的委派与显式验证 |
| **duet** | 双视角推理 | 将问题分发给 Claude 和 GPT/Codex 两侧，展示双方观点，可选运行辩论轮次 |
| **chorus** | 文档编排 | 从仓库上下文收集变更信息，撰写文档/变更日志/发布说明，可选独立审查 |

## 名字由来

`ashima` —— 源自云南彝族传说《阿诗玛》。她以歌闻名。这些技能是她的三种声音：

- **aria**（咏叹调）— 一个声音，精准而结构化
- **duet**（二重唱）— 两个声音，对话与交锋
- **chorus**（合唱）— 所有声音，齐声记录

## 安装

这是 Hermes Agent 的技能集。优先将技能目录复制到共享 Hermes skill 路径：

```bash
mkdir -p ~/.agents/skills
cp -r ashima ~/.agents/skills/
cp -r aria ~/.agents/skills/
cp -r duet ~/.agents/skills/
cp -r chorus ~/.agents/skills/
```

## 使用

在对话中直接调用：

```
用 ashima aria 帮我修这个 bug，codex 编码 claude review
用 ashima duet 从两个角度分析这个架构方案
用 ashima chorus 根据最近 10 个 commit 写 release notes
```

### 自动路由

ashima 也可以自动判断该走哪条路：

```
加载 ashima，帮我判断这次任务该走 aria / duet / chorus 哪条路
```

路由规则：
- 主要产出是**代码** → `aria`
- 主要产出是**并列观点** → `duet`
- 主要产出是**文档** → `chorus`

## 工作原理

ashima 是路由器，不是执行器。它负责分类任务并加载对应技能：

1. **ashima**（路由器）检测用户意图
2. 加载匹配的专业技能
3. 执行该技能的完整工作流

每个子技能定义了完整的工作流，包括：
- 输入收集
- 任务分解与委派
- 验证与审查
- 最终输出

这些 skill 是流程定义，不保证当前环境一定已经同时配置好底层双引擎（例如 Claude 侧与 GPT/Codex 侧）。

## 项目结构

```
ashima/
├── ashima/SKILL.md    ← 路由入口，分类任务
├── aria/SKILL.md      ← 编码编排工作流
├── duet/SKILL.md      ← 双视角推理工作流
├── chorus/SKILL.md    ← 文档编排工作流
├── docs/              ← 文档
├── README.md          ← 英文说明
├── README_CN.md       ← 中文说明
├── CHANGELOG.md       ← 变更日志
└── LICENSE            ← Apache-2.0
```

## 仓库内工作流文档

当前仓库先在 `ashima` 内部试运行自己的项目治理工作流，成熟后再考虑向其它项目推广。

- [全局工作台](docs/global-workbench.md)
- [全局复利与踩坑日志](docs/global-compounding-and-pitfalls-log.md)
- [新项目 SOP](docs/new-project-sop.md)
- [AGENTS.md](AGENTS.md) —— 这些仓库内规则的最高优先级入口

这些治理规则当前只在 `ashima` 仓库内部生效。

## 许可证

Apache-2.0
