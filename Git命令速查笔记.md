# Git命令速查笔记（17条）

## 一、基础配置（只需做一次）

### 1. git config
**作用**：配置Git的身份信息
**干什么用**：每次提交代码都要记录"谁提交的"，不配就提交不了
**语法**：
```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
git config --global http.sslverify false   # 跳过SSL验证（国内网络问题）
```
- `--global` 表示全局配置，整台电脑所有仓库生效，只需配一次

---

## 二、仓库初始化

### 2. git init
**作用**：初始化一个Git仓库
**干什么用**：把一个普通文件夹变成Git能管理的项目，才能开始用版本控制
**语法**：
```bash
git init
```
- 在你想管理的项目文件夹里敲，会生成一个隐藏的 `.git` 目录
- **这个 `.git` 目录就是仓库的全部历史记录，千万别删**

### 3. git clone
**作用**：从远程服务器下载整个项目
**干什么用**：入职第一天拉代码，自动创建项目文件夹和 `.git` 目录
**语法**：
```bash
git clone https://公司内网地址/项目名.git          # 默认文件夹名=项目名
git clone https://公司内网地址/项目名.git 自定义名  # 指定文件夹名
```
- 在你想要存放项目的目录下敲，自动创建同名文件夹
- 公司里几乎不用 `git init`，直接 `git clone` 就开始干活

---

## 三、日常操作（每天重复）

### 4. git status
**作用**：查看当前仓库的状态
**干什么用**：随时看哪些文件改了、哪些还没提交、当前在哪个分支，**最常用的命令**
**语法**：
```bash
git status
```
- 红色文件 = 改了但还没暂存（在工作区）
- 绿色文件 = 已暂存，准备提交（在暂存区）
- nothing to commit = 干干净净，都提交了

### 5. git add
**作用**：把文件的修改放进暂存区
**干什么用**：告诉Git"我改了这些文件，下次提交时要带上它们"，不加就不会被提交
**语法**：
```bash
git add 文件名        # 暂存某个文件
git add .            # 暂存所有修改（最常用）
```
- `.` 代表当前目录下所有改动
- 只有add过的文件才会进入下一次commit
- add后 `git status` 变绿色

### 6. git commit
**作用**：把暂存区的内容正式保存为一个版本
**干什么用**：给当前代码拍一张"快照"，以后随时可以回到这个版本
**语法**：
```bash
git commit -m "说明你干了什么"
```
- `-m` 后面的字符串就是这次改动的说明，必须写
- commit message规范：`feat: xxx`（新功能）、`fix: xxx`（修bug）、`refactor: xxx`（重构）
- 一次commit = 一个版本

### 7. git log
**作用**：查看提交历史
**干什么用**：看之前都提交了什么、谁提交的、什么时候提交的
**语法**：
```bash
git log                # 详细历史
git log --oneline      # 简洁版，一行一个commit（最常用）
```
- 从新到旧排列，最新的在最上面
- 每行前面的7位字符是commit的唯一编号（SHA哈希值）

### 8. git diff
**作用**：查看文件具体改了哪些行
**干什么用**：commit之前看看自己到底改了什么，确认没问题再提交
**语法**：
```bash
git diff              # 看所有修改的详细内容
git diff 文件名       # 只看某个文件
```
- 绿色 `+` 开头的行 = 新增的内容
- 红色 `-` 开头的行 = 删除的内容

---

## 四、分支操作

### 9. git branch
**作用**：创建和查看分支
**干什么用**：公司里不能直接在主代码上乱改，每个人开一条分支独立开发
**语法**：
```bash
git branch              # 查看所有分支（当前分支前面有*号）
git branch 分支名       # 创建新分支
```
- 常见命名：`feature/xxx`（新功能）、`fix/xxx`（修bug）、`hotfix/xxx`（紧急修复）

### 10. git checkout
**作用**：切换分支
**干什么用**：从一个分支跳到另一个分支，就像换一条工作线
**语法**：
```bash
git checkout -b 新分支名    # 创建新分支并直接切过去（最常用）
git checkout 分支名         # 切到已有分支
```
- 不管有多少分支，敲哪个名字就跳哪个

### 11. git merge
**作用**：把一个分支的代码合并到当前分支
**干什么用**：功能分支开发完了，要把代码合回主分支
**语法**：
```bash
git merge 要合并进来的分支名
```
- **先切到接收方分支**（通常是main），再merge
- merge是本地操作，代码还在你电脑上，别人看不到

---

## 五、远程操作

### 12. git remote
**作用**：关联远程仓库地址
**干什么用**：告诉本地Git"代码要推到哪个服务器上去"
**语法**：
```bash
git remote add origin 仓库地址     # 关联远程仓库（origin是别名）
git remote -v                       # 查看已关联的远程地址
```
- `origin` 是远程服务器的别名，约定俗成
- 一个项目只需要敲一次

### 13. git branch -M
**作用**：把当前分支改名
**干什么用**：GitHub要求主分支叫 `main`，本地可能是 `master`，名字不一致会push失败
**语法**：
```bash
git branch -M main
```
- `-M` = 强力改名（Move/重命名）

### 14. git push
**作用**：把本地代码推到远程服务器
**干什么用**：commit只存在本地，push之后别人才能看到你的代码
**语法**：
```bash
git push origin main          # 把main分支推到origin服务器
git push -u origin main      # 第一次push加-u，以后只用git push就行
git push                     # 设了上游分支后直接用
```
- `-u` = 记住这条线路，以后不用再写完整地址
- push是对外的，merge是内部的，两个都要用但干的事不一样

### 15. git pull
**作用**：从远程服务器拉最新代码到本地
**干什么用**：别人push了新代码，你得拉下来才能看到最新版本
**语法**：
```bash
git pull origin main          # 从origin拉main分支的最新代码
git pull                      # 设了上游分支后直接用
```
- **每天开始干活前先pull**，保证本地代码是最新的
- `git pull` = `git fetch`（拉数据）+ `git merge`（合并），一步搞定

---

## 六、紧急情况

### 16. git stash
**作用**：把当前改动暂存起来
**干什么用**：改了一半要切分支，但不想commit半成品代码
**语法**：
```bash
git stash          # 把当前改动藏起来
git stash pop       # 恢复暂存的改动
```
- stash = 藏进抽屉，pop = 拿出来继续用
- Git会自动尝试合并恢复的改动

### 17. git commit --amend
**作用**：修改上一次commit
**干什么用**：commit message写错了，或者漏了文件没add
**语法**：
```bash
git commit --amend -m "新的说明"
```
- 只能改最近一次commit，不能改历史commit
- **已经push过的commit千万别amend**，会导致冲突

---

## 完整工作日流程

```bash
# 早上开始
git pull                          # 1. 拉最新代码

# 开新功能
git checkout -b feature/xxx       # 2. 开个新分支

# 写代码...

# 提交
git add .                         # 3. 暂存改动
git commit -m "feat: 干了什么"    # 4. 提交

# 合并推送
git checkout main                 # 5. 切回主分支
git merge feature/xxx             # 6. 合并
git push                          # 7. 推到服务器
```

**7步，每天重复，就是Git的全部日常。**