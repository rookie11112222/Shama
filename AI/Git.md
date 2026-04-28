★ **==所有命令都是在termux里运行==**


==**基础查看**==
```
git status
```
- 查看文件变动
```
git log             
```
- 查看提交记录
- 输入q退出
```
git branch          
```
- 查看当前分支
```
git remote -v       
```
- 查看绑定的github仓库地址


==**初始配置**==
1. **配置身份**
```
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"
```
2. **初始化本地仓库**
```
git init
```
3. **切换到main分支**
```
git checkout -b main
```
4. **绑定云端仓库**
```
git remote add origin https://github.com/rookie11112222/Shama.git
```


**==上传文件（本地 → GitHub）==**
```
cd storage/shared/1. 业务文件/2.其他/0.obsidian/Alibaba/0.Github repo/Shama
```
- 移动到本地git仓库目录
```
git add .                    
```
- 全部文件加入上传
```
git commit -m "update"     
```
- 提交保存
- "update": git log里的备注
```
git push origin main         
```
- 正常上传
```
git push -f origin main       
```
- 强制本地覆盖github仓库


**==下载文件（GitHub → 本地）==**
```
git pull origin main              
```
- 拉取最新文件
```
git checkout main                 
```
- 切换分支，显示仓库文件
```
git fetch --all && git reset --hard origin/main
```
- 强制github文件全覆盖本地，解决已最新但没文件


**==克隆其他仓库的方法（GitHub → 本地）==**
```
git clone https://github.com/rookie11112222/Shama.git
```
- https后面是要克隆的仓库的地址


==**版本回退方法**==
1. **查看git id**
```
git log
```
2. **复制需到回退版本的的ID**
3. **进行回退**
```
git reset --hard 02ccddf5ef16f83239c89582abb4aa45dbebb1c
```
- hard后面一长串是第二步确认的ID


**==删除所有git log==**
```
rm -rf .git
git init
```
- 删除完log后，要重新绑定git仓库