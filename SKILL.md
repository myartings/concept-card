---
name: concept-card
description: 概念卡片生成器。当用户发送「/concept 概念词」或「概念卡片 概念词」时触发。流程：搜索本地 wiki 知识库 → 判断有无记录 → 有记录直接生成 4 页 HTML 卡片并发布到 Artifact Library；无记录则引导用户补充内容后重跑。支持 vibe-coding、agentic-engineering、harness-engineering 等技术概念的快速理解和记忆。
---

# Concept Card — 概念卡片生成器

## 触发条件

用户输入满足以下任一条件时触发本 skill：
- `/concept <概念词>`
- `概念卡片 <概念词>`
- `/concept-card <概念词>`

## 工作流程

### Step 1: 解析概念词

从用户输入中提取概念词（去掉命令前缀）。

### Step 2: 搜索 Wiki

执行 `concept-to-html <概念词>` 搜索本地知识库 wiki，输出到 `/tmp/concept-<slug>.md`。

### Step 3: 判断 Wiki 是否有内容

检查搜索结果：
- `[高]` 或 `文件名匹配` 出现 2 次以上 → 有内容，**走快速通道**
- `[中]` 出现 1 次以上 → 有内容，**走快速通道**
- 无匹配 → **走降级通道**

### Step 4a: 快速通道（Wiki 有内容）

```bash
# 直接渲染 + 发布
concept-card-gen "<概念词>"
```

### Step 4b: 降级通道（Wiki 无内容）

告知用户需要补充内容，给出搜索命令：

```bash
# 依次执行，按需补充
python3 ~/.openclaw/skills/v2ex/scripts/v2ex_client.py search "<概念词>"
web_fetch("https://cn.bing.com/search?q=<概念词>")
# 补充完成后重新运行
concept-card-gen "<概念词>"
```

## 卡片结构（4页）

| 页 | 类型 | 内容 |
|---|---|---|
| 1 | cover | 封面：概念名 + 分类树 + 一句话定义 |
| 2 | thesis | 类比：像什么（4 条切入点） |
| 3 | comparison | 对比：本概念 vs 最相似概念（各 4 条） |
| 4 | closing | 收束：对 iOS 独立开发者的意义 + 行动建议 |

## 模板与输出

- **默认模板**：`deck-simple-fast`
- **输出路径**：`~/Sync/docs/concept-<slug>.html`
- **Artifact 发布**：自动发布到 Artifact Library，输出 viewUrl

## 相关脚本

- `concept-card-gen` — 主脚本：生成 → 渲染 → 发布
- `concept-to-html` — 搜索 wiki 并输出分析 markdown
- `concept-card` — 纯搜索 wiki（不渲染）

所有脚本位于 `~/.local/bin/`（已加入 PATH）。