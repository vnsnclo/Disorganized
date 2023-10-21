# 修改Git初始分支名称
* [情况一: 仓库未初始化](#one)
* [情况二: 仓库已初始化(未推送)](#two)
* [情况三: 仓库已推送](#three)
---
### <a id="one">情况一:仓库未初始化</a>
* 修改Git全局配置
```
git config --global init.defaultBranch <defaultBranch>
#例
git config --global init.defaultBranch main
```
* 或者在执行git init时指定初始分支名称
```
git init -b <branch-name>
#例
git init -b main
```

### <a id="two">情况二:仓库已初始化(未推送)</a>
```
git branch -m <oldbranch> <newbranch>
#例
git branch -m master main
```

### <a id="three">情况三:仓库已推送</a>
1. 修改本地分支名称
```
git branch -m <oldbranch> <newbranch>
#例
git branch -m master main
```
2. 删除远程分支
```
git push origin --delete <branch-name>   
#例
git push origin --delete master

#要删除的分支可能是默认分支、受保护分支
#如果删除不成功，需要先在仓库管理平台设置
#然后再执行命令。
```
3. 推送本地分支到远程仓库
```
git push -u origin <branch-name>
#例
git push -u origin main
```