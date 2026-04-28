**==接入本地大模型的配置==**
1. **编辑配置文件：nano ~/.bashrc**
2. **在末尾添加：**
```
export ANTHROPIC_BASE_URL="http://localhost:11434/v1"
export ANTHROPIC_AUTH_TOKEN="ollama"
```
3. **保存并生效：source ~/.bashrc**
4. **之后只需运行：claude --model <你的模型名称>**
```
claude --model qwen:7b
```
