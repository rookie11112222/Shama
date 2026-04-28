**==手机termux运行大模型的方法==**
1. **运行termux**
2. **进入debian系统**
```
proot-distro login debian
```
3. **运行ollama**
```
OLLAMA_KEEP_ALIVE=-1 nohup ollama serve > ollama.log 2>&1 &
```
4. **运行大模型**
```
ollama run qwen2.5:7b
```
- run命令无法在后台静默运行，过几分钟会自动被kill掉


**==基础操作==**
```
ollama list
```
- 查看所有本地模型
```
ollama ps
```
- 查看正在运行的模型
```
ollama stop all
```
- 停止所有大模型
```
ollama -v
```
- 查看版本
```
pkill ollama
```
- 停止ollama服务
```
ps aux | grep ollama
```
- 查看ollama服务是否在运行


**==模型下载 & 运行==**
```
ollama pull qwen2.5:7b
```
- 拉取下载模型
```
ollama run qwen2.5:7b
```
- 直接运行对话模型
```
ollama rm qwen2.5:7b
```
- 删除模型
```
ollama cp 原模型名 新名字
```
- 复制/重命名模型


**==模型信息==**
```
ollama show qwen2.5:7b
```
- 查看模型详情、参数大小
```
ollama show qwen2.5:7b --modelfile
```
- 查看模型 prompt 模板
```
ollama show qwen2.5:7b --parameters
```
- 查看模型参数