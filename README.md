# git_learn 本地 Git 配置

本仓库使用 **仅作用于当前目录** 的 Git 本地配置（`--local`），不会修改全局 `~/.gitconfig`。

## 已执行的配置命令

在 `F:\knowledge\git` 目录下执行：

```powershell
# 基础配置
git config --local user.name "业繁"
git config --local user.email "roku_28@163.com"
git config --local remote.origin.url https://github.com/gitwpl/git_learn.git
git config --local pull.rebase false

# 代理与 SSL（解决 push 失败）
git config --local https.https://github.com.proxy http://127.0.0.1:7897
git config --local http.sslVerify false
```

## 配置说明

| 配置项 | 值 | 作用 |
|--------|-----|------|
| `user.name` | 业繁 | 本仓库提交记录中显示的作者名 |
| `user.email` | roku_28@163.com | 本仓库提交记录中显示的邮箱 |
| `remote.origin.url` | https://github.com/gitwpl/git_learn.git | 远程仓库地址（origin） |
| `pull.rebase` | false | `git pull` 时使用 merge 合并，而非 rebase |
| `https.https://github.com.proxy` | http://127.0.0.1:7897 | 访问 GitHub 时走本机 7897 代理 |
| `http.sslVerify` | false | 跳过 SSL 证书校验（代理环境下绕过 schannel 握手失败） |

## 代理配置说明

推送时曾遇到两类错误：

1. **`Failed to connect to 127.0.0.1 port 7897`** — 代理软件未启动，需先打开 Clash 等工具。
2. **`schannel: failed to receive handshake, SSL/TLS connection failed`** — 全局配置将 GitHub 代理指向了未监听的 `7890` 端口，且经代理的 HTTPS 在 Windows schannel 下校验失败。

因此在 **本仓库** 单独覆盖：

- 将 GitHub 专用代理改为当前可用的 `7897` 端口
- 关闭 SSL 校验，使 `git push` / `git pull` 可正常使用

> 以上配置写在 `.git/config` 中，**不会随 commit 提交**；本文档用于记录命令，便于在其他机器或新 clone 后重新设置。

## 查看当前配置

```powershell
# 列出本仓库全部本地配置
git config --local --list

# 查看某项配置及其来源
git config --show-origin user.name
git config --show-origin https.https://github.com.proxy
```

## 配置文件位置

上述配置保存在本仓库的 `.git/config` 中，例如：

```ini
[user]
    name = 业繁
    email = roku_28@163.com
[remote "origin"]
    url = https://github.com/gitwpl/git_learn.git
[pull]
    rebase = false
[branch "master"]
    remote = origin
    merge = refs/heads/master
[https "https://github.com"]
    proxy = http://127.0.0.1:7897
[http]
    sslVerify = false
```

## 常用后续操作

```powershell
# 首次推送（需先有提交）
git add .
git commit -m "Initial commit"
git push -u origin master

# 日常同步
git pull
git push
```

## 更稳妥的替代方案

若不想使用 `http.sslVerify false`，可改用 SSH 推送：

```powershell
git remote set-url origin git@github.com:gitwpl/git_learn.git
```

需在 GitHub 账户中配置 SSH 公钥。
