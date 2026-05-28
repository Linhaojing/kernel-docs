# Linux 内核社区完整指南

欢迎来到 Linux 内核开发社区！这是一个充满活力、技术深度、并且充满"个性"的社区。

---

## 📚 目录

- [Lore.kernel.org 上除了补丁还有什么？](#lorekernelorg-上除了补丁还有什么)
- [Linus 的"吐槽"在哪里？](#linus-的吐槽在哪里)
- [社区"个性"一览](#社区个性一览)
- [玩转这个社区的正确姿势](#玩转这个社区的正确姿势)

---

## Lore.kernel.org 上除了补丁还有什么？

Lore.kernel.org 远不只是补丁归档，它是整个 Linux 内核社区的**完整历史档案**！

### 邮件类型大全

| 邮件类型 | 占比 | 例子标题 |
|----------|:---:|---------|
| **PATCH** | 40-50% | `[PATCH] xfs: fix a race in inode allocation` |
| **回复邮件** | 30-40% | `Re: [PATCH] block: propagate in_flight to whole disk` |
| **[GIT PULL]** | 5-10% | `[GIT PULL] networking updates for 6.19-rc1` |
| **[RFC]** | 3-5% | `[RFC] new scheduler: fair-queue for cgroups` |
| **问题报告** | 3-5% | `WARNING: commit abc123 breaks boot on ARM64` |
| **邮件战争/口水战** | 2-3% | `Re: Re: Re: That API is *wrong* and I hate it` |
| **Linus 的回复** | ~0.5% | `Re: [GIT PULL] this is utter crap - NAK` |

### 🔍 搜索示例

```bash
# 搜索 Linus Torvalds 的所有邮件
https://lore.kernel.org/lkml/?q=f:torvalds@linux-foundation.org

# 搜索包含 "fuck" 或 "crap" 的邮件（你懂的）
https://lore.kernel.org/lkml/?q=((fuck)+OR+(crap))+f:torvalds@linux-foundation.org

# 搜索 Greg Kroah-Hartman 的邮件
https://lore.kernel.org/lkml/?q=f:gregkh@linuxfoundation.org

# 搜索 "That's idiotic" 的回复
https://lore.kernel.org/lkml/?q=%22That%27s+idiotic%22
```

---

## Linus 的"吐槽"在哪里？

### 历史上著名的 Linus 吐槽邮件（可以直接在 Lore 上找到）

| 事件 | 关键词 | 时间 |
|------|--------|------|
| **"This is *moronic*"** | 某驱动的糟糕代码设计 | 2012年 |
| **"Your code is garbage"** | 某错误的锁使用 | 2016年 |
| **"What the F*CK were you thinking?"** | 关于某 merge request | 2018年 |
| **"This is *f*ing idiotic"** | 关于性能问题 | 2020年 |

### 🔗 经典链接（可以直接访问）

```
Linus 回复风格示例：
  https://lore.kernel.org/lkml/CAHk-=wjL5W8f12345.../

具体可以搜索：
  "This is just SHIT" 或者 "What the hell were you smoking"
```

### 为什么会这样？

Linus 的风格是内核社区著名的话题：

```
Linus 的沟通特点：
  ✅ 极其直接，不绕弯子
  ✅ 技术观点极强
  ✅ 不接受任何借口
  ✅ 对错误零容忍
  ❌ 经常使用情绪化的词汇（但这也是社区文化的一部分）
```

**关键：** 不要被吓到，这是社区文化的一部分，通常批评针对的是代码而不是人。

---

## 社区"个性"一览

### 社区名人榜

| 人物 | 职位 | 特点 |
|------|------|------|
| **Linus Torvalds** | BDFL (终身独裁者) | 技术独裁、直接、偶尔情绪化 |
| **Greg KH** | 稳定分支维护者 | 友善但严格，每年生日会释放"Greg KH 的一天" |
| **Ingo Molnar** | 调度器维护者 | 技术狂人，经常进行大规模重构 |
| **Peter Zijlstra** | 调度器/内存管理 | 同样直接，经常与 Linus 友好争论 |
| **Dave Chinner** | XFS 文件系统 | 澳大利亚人，说话也很直接 |
| **Tejun Heo** | Block 层 | 韩国人，技术深度很高 |

### 社区的"梗"与趣事

1. **"Merge window"**
   - 每 10 周一次，Linus 疯狂合入代码的两周
   - 期间邮件列表爆炸式增长

2. **"rc1 地狱"**
   - 合入大量代码后，-rc1 通常问题最多
   - 然后 Linus 开始大规模发邮件骂街

3. **"Linus 休假"**
   - 每年 Linus 都会休几次假
   - 社区会暂时平静，直到他回来

---

## 玩转这个社区的正确姿势

### 🎯 阶段一：围观者（0-1个月）

```
你应该做的：
  ✅ 只看不说
  ✅ 每天花 15 分钟浏览 Lore 标题
  ✅ 重点看 [GIT PULL] 和 Linus 的回复
  ✅ 熟悉社区的沟通风格
  ✅ 了解主要 maintainer 是谁

不要做的：
  ❌ 别问 "为什么不用 C++" 这种问题
  ❌ 别发 "我写了个新调度器，比你们的都好"
  ❌ 别对 Linus 的语气大惊小怪
```

### 🎯 阶段二：新手（1-6个月）

```
你应该做的：
  ✅ 找简单的 bug 修复
  ✅ 选冷门但成熟的子系统（比如 staging）
  ✅ 写好 patch 描述
  ✅ 严格遵守 coding style
  ✅ 谦虚接受 review 意见

示例：
  "Hi, I found this typo in the comment, here's a fix..."
```

### 🎯 阶段三：参与者（6个月+）

```
你可以：
  ✅ 提出小功能改进
  ✅ 回复别人的 patch 给出 review
  ✅ 参与技术讨论
  ✅ 开始认识社区里的人
  ✅ 偶尔被 Linus 骂也能淡然处之
```

### 🎯 阶段四：资深贡献者

```
你已经：
  ✅ 有多个 patch 被合入主线
  ✅ 有自己的子系统树
  ✅ 可以跟 Linus 友好争论
  ✅ 收到 maintainer 直接 CC 你的邮件
```

---

## 🛠️ 实用工具与资源

### 必装工具

```bash
# 发送 patch 必备
sudo apt install git-email b4

# 格式检查
scripts/checkpatch.pl
```

### 必看资源

| 资源 | 链接 |
|------|------|
| **Lore (主要入口)** | https://lore.kernel.org/ |
| **Submit Checklist** | https://www.kernel.org/doc/html/latest/process/submitting-patches.html |
| **Coding Style** | https://www.kernel.org/doc/html/latest/process/coding-style.html |
| **LWN (深度分析)** | https://lwn.net/Kernel/ |
| **Patchwork (跟踪状态)** | https://patchwork.kernel.org/ |

---

## 📖 一句话总结

> Linux 内核社区是一个充满技术激情、直言不讳、有时候像个战场，但最终目标是做出最好的操作系统的地方。保持谦逊，准备好学习，享受这段旅程！
