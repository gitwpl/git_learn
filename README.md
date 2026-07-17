# git_learn 本地 Git 配置

本仓库使用 **仅作用于当前目录** 的 Git 本地配置（`--local`），不会修改全局 `~/.gitconfig`。

## 已执行的配置命令

在 `F:\knowledge\git` 目录下执行：

```powershell
git config --local user.name "业繁"
git config --local user.email "roku_28@163.com"
git config --local remote.origin.url https://github.com/gitwpl/git_learn.git
git config --local pull.rebase false
```

## 配置说明

| 配置项 | 值 | 作用 |
|--------|-----|------|
| `user.name` | 业繁 | 本仓库提交记录中显示的作者名 |
| `user.email` | roku_28@163.com | 本仓库提交记录中显示的邮箱 |
| `remote.origin.url` | https://github.com/gitwpl/git_learn.git | 远程仓库地址（origin） |
| `pull.rebase` | false | `git pull` 时使用 merge 合并，而非 rebase |

## 查看当前配置

```powershell
# 列出本仓库全部本地配置
git config --local --list

# 查看某项配置及其来源
git config --show-origin user.name
git config --show-origin pull.rebase
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
