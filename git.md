# Git学习笔记

Git指南：https://www.progit.cn


## 初始配置

```
$ git config --global user.name "用户名"
$ git config --global user.email 邮箱账号

// 查看配置信息
$ git config --list
```


## SSH 公钥

```
// 创建：
$ ssh-keygen -t rsa -C 邮箱账号

// 测试：
$ ssh git@github.com

The authenticity of host 'github.com (192.30.253.112)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,192.30.253.112' (RSA) to the list of known hosts.
PTY allocation request failed on channel 0
Hi wangchungeng/isio! You've successfully authenticated, but GitHub does not provide shell access.
Connection to github.com closed.
```

提取密钥文件`id_dsa.pub`，默认存储在"~/.ssh"目录下：
```
$ cat ~/.ssh/id_dsa.pub
```


## 管理流程

### 仓库文件

- **已跟踪**：被纳入版本控制的文件，其3种状态分别为“未修改”、“已修改”、“放入暂存区”。
- **未跟踪**：既不存在于上次快照的记录中，也没有放入暂存区。

### 文件的生命周期

[![查看Git指南的记录每次更新到仓库](https://www.progit.cn/images/lifecycle.png)][lifecycle]



## 忽略文件

`.gitignore` 文件的书写规范：
- 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（/）开头防止递归。
- 匹配模式可以以（/）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

> GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表，你可以在 https://github.com/github/gitignore 找到它.


## 命令的使用

### 查看命令手册，例如

```
$ git help config
// 或
$ git config --help
```

### 检查当前文件状态

```
$ git status

// 状态简览
$ git status -s
 M 标记已修改的文件
MM 标记已修改但未放入暂存区的文件
 A 标记新添加的且已放入暂存区中的文件
?? 标记未跟踪的文件
```

### 将文件添加到暂存区
```
// -A 或 --all 提交所有变化到暂存区
$ git add -A

// 添加被修改和新增的文件到暂存区
$ git add .

// 添加所有文件到暂存区，* 会忽略.gitignore把任何文件都加入
$ git add *
```

### 跳过暂存区提交文件

```
// 相当于执行了 "$ git add" 和 "$ git commmit"
// 这种方式只提交 ** 已被跟踪 ** 的文件
$ git commit -am "描述信息"
```
描述信息可参考[Angular：Commit Message Guidelines][commit-message-guidelines]

### 查看提交历史

```
// -p 显示每次提交的内容差异
// -2 仅显示最近两次提交
git log -p -2

// 查看简略信息
$ git log --stat

// 格式化输出
$ git log --pretty=oneline
$ git log --pretty=format:"%h %s" --graph
```

### 查看已暂存和未暂存的修改

```
// 对比与上一次提交的差异
$ git diff

// 对比暂存区与当前的差异
$ git diff --staged
```

### 删除文件，移出跟踪清单

```
// 删除暂存区域和工作目录中的文件，提交后文件不再被跟踪
$ git rm <file>

// 如果删除之前修改过并且已经放到暂存区域，必须要用强制删除选项
$ git rm -f <file>

// 只将文件从跟踪清单（暂存区）中删除，依然保留再工作目录中
git rm --cached <file>
```
**手动删除**工作目录中已被跟踪的文件，运行`$ git status`会看到"Changes not staged for commit"，此时应运行`$ git rm <file>`记录此操作，提交后文件不再被跟踪。

### 移动文件

```
$ git mv file_from file_to

// 重命名
$ git mv README.md README
```

### 撤销操作

保存到最后一次提交：
```

// 覆盖或者更改最后一次的提交信息
$ git commit --amend
$ git commit -amend -m "描述信息"

// 仅撤销提交行为，而保留其他内容
$ git reset --soft HEAD <hash>
```

取消暂存的文件：
```
// 此时文件已经是修改未暂存的状态了
$ git reset HEAD <file>
```

撤销对文件的修改，**慎用！**：
```
// 留意有2个短线 --
git checkout -- <file>
```

版本回退：
```
// 退回上一个版本
$ git reset HEAD~

// 退回指定版本
$ git reset HEAD <hash>

// 仅撤销提交行为，而保留其他内容
$ git reset --soft HEAD <hash>

// 完全撤销并抛弃所有更改，将内容重置为上一个提交
$ git reset --hard HEAD <hash>
```

使用reset命令时，**慎用 --hard 选项**，可能导致工作目录中所有当前进度丢失。在 [重置揭密][git_reset] 中了解 `reset` 的更多细节。

回退版本后，新版本依然存在git分支中，`git reflog` 可查看。阅读 [数据恢复][data_recovery] 了解数据恢复。


### 远程仓库
```
// 添加远程仓库
$ git remote add <remote-shortame> <url>

// 查看远程仓库，添加-v参数可查看仓库的读写权限
$ git remote

// 重命名remote-shortname
$ git remote rename <oldname> <newname>

// 将分支推送到远程仓库，多人开发时记得推送前先拉取合并文件
$ git push [remote-name] [branch-name]
```


### 创建标签

Git 使用两种主要类型的标签
- 轻量标签（lightweight）：本质上是将提交校验和存储到一个文件中，不保存任何其他信息。
- 附注标签（annotated）：存储在 Git 数据库中的一个完整对象，它们是可以被校验的。可以使用 GNU Privacy Guard （GPG）签名与验证，通常建议创建附注标签。

```
// 轻量标签
$ git tag v1.4

// -a 附注标签
// -m 指定了一条将会存储在标签中的信息。
$ git tag -a v1.4 -m 'my version 1.4'

// 查看标签
$ git tag

// 查看标签对应的提交信息
$ git show v1.4

// 给指定的版本打标签
$ git tag -a v1.2 <hash>

// 将标签推送到远程仓库
$ git push origin <tag>
// 或者同时推送多个标签
$ git push origin --tags
```

### 分支

```
// 新建分支
$ git branch <branch-name>

// 新建一个分支指向某版本
$ git branch new-branch <hash>

// 新建并切换分支
$ git checkout -b <branch-name>
```

### 合并

```
// 非快速合并
$ git merge --no-ff <branch-name>

// 合并指定的远程分支到本地分支，--squash：不更改HEAD
$ git merge --squash origin/branch-name
```

### 拉取远程分支

```
// 拉取远程分支，但不会自动合并或修改你当前的工作
$ git fetch [remote-shortame | remote-branch]
```

### 删除远程分支

```
$ git push <remote-shortname> --delete <branch-name>
```

### git stash

[暂存](https://www.jianshu.com/p/16adec527aed)

```
$ git stash list
$ git stash pop stash@{num}
$ git stash apply stash@{num}
$ git stash drop stash@{num}
$ git stash clear
```

## git alias

```
git config --global alias.vv "branch -vv"
git config --global alis.ss "status -s"
```

[lifecycle]: https://www.progit.cn/#_%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93

[commit-message-guidelines]: https://github.com/angular/angular/blob/22b96b9/CONTRIBUTING.md#-commit-message-guidelines

[git_reset]:https://www.progit.cn/#_git_reset

[data_recovery]:https://www.progit.cn/#_data_recovery



## 在github创建空仓库时的内容

…or create a new repository on the command line
```
echo "# re" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:wangchungeng/re.git
git push -u origin main
```

…or push an existing repository from the command line
```
git remote add origin git@github.com:wangchungeng/re.git
git branch -M main
git push -u origin main
```


# GitHub Pages

搭建可访问静态页面，[查看说明](https://pages.github.com)