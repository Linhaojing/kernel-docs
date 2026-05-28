# Linux 内核贡献完整流程指南

本文档为新手提供从环境搭建到提交第一个 patch 的完整步骤。

---

## 📋 目录

1. [环境准备](#环境准备)
2. [获取内核源码](#获取内核源码)
3. [选择你的第一个贡献](#选择你的第一个贡献)
4. [开发修改](#开发修改)
5. [提交前检查](#提交前检查)
6. [发送 Patch](#发送-patch)
7. [处理 Review](#处理-review)

---

## 环境准备

### 1. 安装依赖

```bash
# Ubuntu/Debian
sudo apt install git-email b4 build-essential flex bison libssl-dev libelf-dev \
    dwarves pahole

# Fedora/RHEL
sudo dnf install git-email b4 make gcc flex bison openssl-devel elfutils-devel \
    pahole
```

### 2. 配置 Git

```bash
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

# 推荐配置（更符合内核社区）
git config --global core.editor vim  # 或者你的编辑器
git config --global format.pretty "%h %s (%an)"
```

---

## 获取内核源码

### 方法一：从 Torvalds 的树克隆（推荐）

```bash
git clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
cd linux
```

### 方法二：浅克隆（快，节省空间）

```bash
git clone --depth 1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
cd linux
```

### 查看当前版本

```bash
git log --oneline -10  # 看最近 10 个 commit
make kernelversion     # 查看当前版本号
```

---

## 选择你的第一个贡献

### 推荐新手切入点

| 难度 | 类型 | 说明 | 成功率 |
|------|------|------|--------|
| ⭐ | **拼写/注释错误** | 改个 typo，补个注释 | 95%+ |
| ⭐⭐ | **警告消除** | `make W=1` 扫出来的警告 | 80%+ |
| ⭐⭐ | **staging 驱动** | drivers/staging/ 下的代码 | 70%+ |
| ⭐⭐⭐ | **小 bug 修复** | 简单的逻辑问题 | 50%+ |

### 查找拼写/注释错误

```bash
# 方法一：手动找
grep -r "FIXME" --include="*.c" --include="*.h" .
grep -r "TODO" --include="*.c" --include="*.h" .

# 方法二：看最近的 commit 学
git log --grep="typo" --oneline
```

---

## 开发修改

### 1. 创建工作分支

```bash
# 从最新的 master 创建分支
git checkout -b fix-typo-in-comment
```

### 2. 修改代码

```bash
# 找到目标文件并修改
vim drivers/net/ethernet/realtek/r8169.c

# 查看修改
git diff
```

### 3. 编译测试（重要！）

```bash
# 第一步：生成配置文件
make defconfig  # 或者 localmodconfig（推荐）
make menuconfig  # 图形化配置

# 第二步：编译
make -j$(nproc)  # 多核编译

# 可选：只编译模块（如果改的是驱动）
make modules -j$(nproc)
```

### 4. 运行 checkpatch

```bash
# 检查格式错误（这是必须的！）
scripts/checkpatch.pl your-patch.patch
# 或者对 git 修改进行检查
git diff --cached | scripts/checkpatch.pl -
```

---

## 提交前检查

### 1. 确保符合 Coding Style

Linux 内核有严格的编码风格要求：

```c
// 正确示例
if (condition) {
        do_something();
} else {
        do_another_thing();
}

// 错误示例（会被 checkpatch 骂）
if( condition )
  {
  do_something();
  }
```

### 2. 写好 Commit Message

```
# 格式规则：
# 第一行：子系统名: 简短描述（不超过 72 字符）
#
# 空一行
#
# 详细描述（可以分段落）
#
# Signed-off-by: Your Name <your-email@example.com>

# 好的例子：
# net: r8169: fix typo in register description comment
#
# Fix a simple typo "recvieve" -> "receive" in the RX register
# description comment. No functional change.
#
# Signed-off-by: John Doe <john.doe@example.com>
```

### 3. 提交你的修改

```bash
git add your-modified-file.c
git commit -s

# 会打开编辑器，写入 commit message
```

---

## 发送 Patch

### 方法一：使用 `git send-email`（官方推荐）

#### 第一步：配置 SMTP

```bash
# ~/.gitconfig
[sendemail]
        smtpuser = your-email@gmail.com
        smtpserver = smtp.gmail.com
        smtpencryption = tls
        smtpport = 587
```

#### 第二步：生成 Patch

```bash
# 如果是单个 commit
git format-patch -1

# 如果是多个 commit
git format-patch -3  # 最近 3 个
```

#### 第三步：找到该联系谁（MAINTAINERS 文件）

```bash
# 查看 MAINTAINERS 文件，或者用 scripts/get_maintainer.pl
./scripts/get_maintainer.pl your-patch.patch

# 会输出类似：
# John Maintainer <john.maintainer@example.com> (maintainer:NET ETHERNET REALTEK)
# linux-kernel@vger.kernel.org (open list)
```

#### 第四步：发送 Patch

```bash
# 发送给 maintainer + LKML
git send-email --to "john.maintainer@example.com" \
    --cc "linux-kernel@vger.kernel.org" \
    0001-fix-typo.patch
```

---

## 处理 Review

### 1. 等待并查收回复

邮件会发到 LKML，Maintainer 可能会回复你。

### 2. 常见回复类型

| 回复 | 含义 | 下一步 |
|------|------|--------|
| `Reviewed-by: xxx` | 通过了！ | 等合入 |
| `Acked-by: xxx` | 认可（但不一定是 maintainer） | 等合入 |
| `Can you please ...?` | 有问题需要改 | 重新发 v2 |
| `NAK` | 明确拒绝 | 别再发了 |

### 3. 如果被要求修改，发 V2

```bash
# 修改代码
git commit --amend  # 修改之前的 commit

# 生成带 V2 的 patch
git format-patch -1 -v2

# 发送时标明是 V2
git send-email --to ... 0001-v2-fix-typo.patch
```

---

## 进阶：从 Patchwork 跟踪状态

```
https://patchwork.kernel.org/

这是一个补丁跟踪系统，可以看到：
  - 你的 patch 有没有被看到
  - 有没有 review
  - maintainer 有没有准备合入
```

---

## 📖 推荐阅读

| 资源 | 链接 |
|------|------|
| **官方提交指南** | https://www.kernel.org/doc/html/latest/process/submitting-patches.html |
| **Coding Style** | https://www.kernel.org/doc/html/latest/process/coding-style.html |
| **内核新人指南** | https://kernelnewbies.org/ |

---

## ⚠️ 新手避坑指南

```
不要做的：
  ❌ 不要改 Linux/ 下的核心文件（调度器、内存管理）
  ❌ 不要同时修改多个子系统
  ❌ 不要用 HTML 邮件
  ❌ 不要 top-post（回复时写在引用上面）
  ❌ 不要没有测试就发

要做的：
  ✅ 从小改动开始
  ✅ 严格遵守 coding style
  ✅ 测试你的修改
  ✅ 检查 MAINTAINERS 该发给谁
  ✅ 写清晰的 commit message
```
