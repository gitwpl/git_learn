# git_learn 本地 Git 配置

本仓库配置均用 `--local`，只影响当前目录，不改全局 `~/.gitconfig`。

## 前置操作

1. 安装 Git：`git --version`
2. 进入项目目录，确保已是仓库：`git init` 或 `git clone ...`
3. GitHub 上建好远程仓库（新建项目时）
4. **先开代理**，确认端口可用：

```powershell
Test-NetConnection 127.0.0.1 -Port 7897   # True 再继续
```

## 本地配置

```powershell
cd F:\knowledge\git

git config --local user.name "业繁"
git config --local user.email "roku_28@163.com"
git config --local remote.origin.url https://github.com/gitwpl/git_learn.git
git config --local pull.rebase false

# 代理 + SSL（本机 GitHub 推送需要）
git config --local https.https://github.com.proxy http://127.0.0.1:7897
git config --local http.sslVerify false
```

| 配置项 | 说明 |
|--------|------|
| `user.*` | 本仓库提交者信息 |
| `remote.origin.url` | 远程地址 |
| `pull.rebase false` | pull 时用 merge |
| `https.https://github.com.proxy` | GitHub 走 7897 代理 |
| `http.sslVerify false` | 绕过代理下 SSL 握手失败 |

> 配置存在 `.git/config`，不会随 commit 提交；换机器后按上文重新执行。

## 分支操作

```powershell
git checkout -b feature/1      # 创建并切换到新分支
git branch                     # 查看本地分支（* 为当前分支）
git checkout master            # 切回 master
git checkout feature/1         # 切到指定分支

git add .
git commit -m "说明"
git push -u origin feature/1   # 新分支首次推送（-u 关联远程）
git push                       # 之后直接 push
```

| 报错 | 处理 |
|------|------|
| `has no upstream branch` | 首次推送用 `git push -u origin 分支名` |

合并回 master：`git checkout master` → `git pull` → `git merge feature/1` → `git push`

## 常用命令

```powershell
git config --local --list    # 查看配置
git add .
git commit -m "说明"
git push -u origin master    # 首次
git pull / git push          # 日常
```

## 推送失败速查

| 报错 | 处理 |
|------|------|
| `Failed to connect to 127.0.0.1:7897` | 开代理 |
| `SSL/TLS connection failed` | 执行上文代理 + SSL 两条配置 |

替代方案：改用 SSH `git@github.com:gitwpl/git_learn.git`（需在 GitHub 配公钥）。
