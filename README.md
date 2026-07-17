# git_learn 本地 Git 笔记

本仓库 Git 配置用 `--local`，只影响当前目录，不改全局 `~/.gitconfig`。

## 前置操作

1. 安装 Git：`git --version`
2. 进入项目目录：`git init` 或 `git clone ...`（`--local` 必须先有 `.git`）
3. GitHub 上建好远程仓库（新建项目时）
4. 访问 GitHub 前 **先开代理**，确认端口：

```powershell
Test-NetConnection 127.0.0.1 -Port 7897   # TcpTestSucceeded : True
```

## 本地配置

```powershell
cd F:\knowledge\git

git config --local user.name "你的名字"
git config --local user.email "你的邮箱"
git config --local remote.origin.url https://github.com/gitwpl/git_learn.git
git config --local pull.rebase false

# 代理 + SSL（本机 GitHub HTTPS 需要）
git config --local https.https://github.com.proxy http://127.0.0.1:7897
git config --local http.sslVerify false
```

| 配置项 | 说明 |
|--------|------|
| `user.*` | 本仓库 **新提交** 用的作者（不区分分支） |
| `pull.rebase false` | `git pull` 用 merge，不用 rebase |
| `https.https://github.com.proxy` | GitHub 走 7897（覆盖全局错误的 7890） |
| `http.sslVerify false` | 绕过代理下 schannel SSL 握手失败 |

> 写在 `.git/config`，不随 commit 提交；换机器后重新执行。

## 分支

```powershell
git checkout -b feature/1       # 创建并切换
git branch                      # 本地分支，* 为当前
git branch --show-current       # 只看当前分支名
git checkout master             # 切换分支

git add .
git commit -m "说明"
git push -u origin feature/1    # 新分支首次推送
git push                        # 已关联远程后
```

| 报错 | 处理 |
|------|------|
| `has no upstream branch` | `git push -u origin 分支名` |

### 历史图里「两条线」

- **一条线** = 一个分支上的 commit 链
- **从同一点分开** = 开分支后各自提交（如 `master` 与 `feature/1`）
- **汇到一点** = 执行过 `git merge` 后的合并提交

```powershell
git log --oneline --graph --all --decorate
```

### 合并：把 feature/1 合进 master

只在 **master** 上操作，`feature/1` **不需要** 再 merge：

```powershell
git checkout master
git pull                        # 可选
git merge feature/1             # 有分叉时会生成 merge commit
git push                        # 自行推送
```

| 目的 | 当前分支 | 命令 |
|------|----------|------|
| feature/1 → master | `master` | `git merge feature/1` |
| 让 feature/1 跟上 master | `feature/1` | `git merge master` |

合并后 `master` 含两边改动；`feature/1` 指针不变。不用时可 `git branch -d feature/1`。

## 提交者是谁

Git **没有按分支区分用户**；`user.name` / `user.email` 对本仓库所有分支生效。

```powershell
git config --local user.name
git config --local user.email
git config --show-origin user.name   # 看值来自 local 还是 global

git log --format="%h %an <%ae> %s" -10   # 历史每条 commit 的实际作者
```

改本地身份后，**新 commit** 用新身份；**旧 commit** 作者不变。

## 推送失败速查

| 报错 | 处理 |
|------|------|
| `Failed to connect to 127.0.0.1:7897` | 开代理 |
| `SSL/TLS connection failed` | 配置上文 GitHub 代理 + `http.sslVerify false` |

替代：SSH 远程 `git@github.com:gitwpl/git_learn.git`（GitHub 配公钥）。
