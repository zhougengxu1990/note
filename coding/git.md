![20250830100332](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/20250830100332.png)# git
<!-- TOC tocDepth:2..3 chapterDepth:2..6 -->

- [git](#git)
  - [git提交规范](#git提交规范)
  - [git config](#git-config)
  - [.gitignore](#gitignore)
  - [gitconfig script](#gitconfig-script)
  - [git基本命令](#git基本命令)
  - [git分支操作](#git分支操作)
  - [git tag](#git-tag)
- [github](#github)
- [gitlab](#gitlab)

<!-- /TOC -->
## git

### git提交规范
> Angular 规范

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer
```git
<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>
```
其中，Header 是必需的，Body 和 Footer 可以省略。**Header**只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。

***type***用于说明 commit 的类别，只允许使用下面7个标识。
- feat: 新功能(feature)
- fix: 修补bug
- docs: 文档变化(document)
- style: 代码格式(不影响代码运行的变动)
- refactor: 重构(不是feature也不是fix)
- perf: 性能优化
- test: 增加测试
- chore: 构建过程或辅助工具的变动
- revert: 回退
- build: 打包
![20250830100426](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/20250830100426.png)

<br/>

### git config
![20250830100453](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/20250830100453.png)

<br/>

- 命令帮助说明
```sh
# git config 格式
git config [location] [action] [type]

# 配置
git config [location] name value
如: git config --global http.proxy socks5://127.0.0.1:7890

# location: 指定配置文件位置
git config --local      # 本地仓库,优先级最高
git config --global     # 全局配置,优先级次之,位置在~/.gitignore
git config --system     # 系统配置,优先级最低
git config --worktree   # worktree
git config -f,--fileq   # 指定文件
git config --blob       # 读取blob对象

# action    name=section.key
git config -l,--list        # 获取配置列表
git config --get            # 获取指定name的value: name [value-pattern]
git config --get-all        # 获取指定name的values: name [value-pattern]
git config --get-regexp     # 正则匹配name获取values: name-pattern [value-pattern]
git config --get-urlmatch   # ur匹配获取,至少两个参数 section[.key] URL
git config --replace-all    # 替换value: name value [value-pattern]
git config --add            # 添加配置: name value
git config -e,--edit        # 打开编辑器进行编辑.gitignore文件
git config --unset          # 移除一个配置项: name [value-pattern]
git config --unset-all      # 移除所有匹配的配置项: name [value-pattern]
git config --rename-section # 重命名section: old-section new-section
git conifg --remove-section # 移除section: section
--fixed-value               # use string equality when comparing values to 'value-pattern'
--get-color                 # find the color configured: slot [default]
--get-colorbool             # find the color setting: slot [stdout-is-tty]

# type: 指定value的数据类型,按指定类型输入输出规范化
git config -t,--type <type> # 指定一个类型
git config --bool           # bool类型
git config --int            # int类型
git config --bool-or-int    # bool或int类型
git config --bool-or-str    # bool或string类型
git config --path           # 是一个文件或目录path
git config --expiry-date    # 有效期限

# other
-z, --null            terminate values with NUL byte
--name-only           show variable names only
--includes            respect include directives on lookup
--show-origin         show origin of config (file, standard input, blob, command line)
--show-scope          show scope of config (worktree, local, global, system, command)
--default <value>     with --get, use default value when missing entry
```
<br/>

- 一些例子
```sh
git config --global http.proxy socks5://127.0.0.1:7890

zhougengxu@192 ~ % git config --get http.https://github.com.proxy
socks5://127.0.0.1:7890
zhougengxu@192 ~ % git config --get-all http.https://github.com.proxy
socks5://127.0.0.1:7890
zhougengxu@192 ~ % git config --global --get http.https://github.com.proxy
socks5://127.0.0.1:7890

zhougengxu@192 note-life % git config --global --get-urlmatch http socks://127
zhougengxu@192 note-life % git config --global http.proxy http://127.0.0.1:7890
zhougengxu@192 note-life % git config --global --get-urlmatch http http://127  
http.proxy http://127.0.0.1:7890
zhougengxu@192 note-life % git config --global --get-regexp http
http.https://github.com.proxy socks5://127.0.0.1:7890
http.proxy http://127.0.0.1:7890
zhougengxu@192 note-life % git config --get user.name 1                        
zhougengxu@192 note-life % git config --get user.name zhou
zhougengxu

```
> 同一个name即section.key可以配置多个,使用--get只能获取最新的一个,使用--get-all可以全部获取并打印

<br/>

- git设置和取消代理
```sh
# http/https proxy
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy https://127.0.0.1:7890

# socks proxy
git config --global http.proxy socks5://127.0.0.1:7890
git config --global https.proxy socks5://127.0.0.1:7890

# 只对github代理
git config --global http.https://github.com.proxy socks5://127.0.0.1:7890

# 取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy
```
> 实测只配置http.proxy即可,不需要配置https.proxy,即使请求的地址是https://github.com/

<br/>

### .gitignore
> 忽略掉git提交的指定的文件格式.如*.xml/.DS_Store等

如何生效
- 放在仓库根目录,只对此仓库生效
- 创建.gitignore,全局指定此文件并全局生效 git config --global core.excludesfile ~/.gitignore
```
# Compiled source #
###################
*.com
*.class
*.dll
*.exe
*.o
*.so

# Packages #
############
# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip

# Logs and databases #
######################
*.log
*.sql
*.sqlite

# OS generated files #
######################
.DS_Store
.DS_Store?
*/.DS_Store
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
```

### gitconfig script
```bash
git config --global user.name zhougengu
git config --global user.email zhougengxu1990@icloud.com
git config --global http.https://github.com.proxy socks5://127.0.0.1:7890
git config --global core.excludesfile ~/.gitignore_global
```

<br/>

### git基本命令
> 其实一般直接使用git工具居多.目前自己使用sublime merge

`clone` `pull` `push`
```sh
# clone一个代码仓库
# rename-dir为可选.若为空则库名与代码库相同.若不为空,则新建rename的库名目录
git clone <url> [rename-dir]

# 拉取代码库
git pull

# 自己的提交push到代码库分支(commit之后才能push)
git push

git checkout

git cherrypick
```
`add` 与 `checkout`
```sh
# 添加本地文件到缓存中.同sublime merge的stage
git add <file>

# 方便操作添加所有文件
git add .

# 添加通配符符合的文件
git add *.ext

# discard file
git checkout -- <file>

```


`commit`
```sh
# 提交,-m 参数指定提交内容
git commit -m '提交内容'

# -a参数合并add操作,在commit前不需要手动add了
git commit -am '提交内容'
```


`status`
```sh
# 查询文件状态
git status

# -s参数对输出结果进行简化 
# M - modified 
# A - added
# D - deleted
# R - renamed
# ?? - untracked
git status -s
```
测试如下
- 修改了1.txt 新建了2.txt 删除了README.en.md (此时为红色)
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-09-02%2000.01.23.png)

- add(此时为绿色)
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-09-02%2000.04.49.png)

- commit(此时提示ahead远程分支,可以进行push操作了)
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-09-02%2000.06.35.png)

### git分支操作
```sh
# 分支名为可选参数.有则新建分支于本地,基于当前分支
# 无则列举所有分支
git branch [branch]

= e.g. =
带*表示为当前分支
zhougengxu@192 testgit % git branch
  dev
  develop
* master



# 切换分支
# 若无本地分支.则会从远程创建一份本地分支
git checkout <branch>



# 合并分支到当前分支(合并正常是commit状态,否则是有冲突状态)
git merge <branch>


# 删除分支(注意-D参数是大写)
# 如果是当前分支,无法删除,需要checkout到其他分支再操作
git branch -D <branch>
```

<br/>

### git tag
列举和打标签
```sh
# 列举所有tag
git tag

# 当前打标签,或指定某次之前的commit hash
git tag -a <tagname> [commit hash]

```
只拉取tag分支代码
- 先 git clone 整个仓库，然后 git checkout tag_name 就可以取得 tag 对应的代码了。 
- 但是这时候 git 可能会提示你当前处于一个“detached HEAD” 状态，因为 tag 相当于是一个快照，是不能更改它的代码的，如果要在 tag 代码的基础上做修改，你需要一个分支： 
` git checkout -b branch_name tag_name `
 这样会从 tag 创建一个分支，然后就和普通的 git 操作一样了。
其实要取得不同的branch的tag，只需要在相应的分支上打tag就行了。这样的tag就唯一对应了不同的分支。例如，你在master上打了tag为v1，在某个branch上打了tag为v2，则你取出v2代码的时候，自然就是对应的branch分支了

## github

## gitlab