# 🖥️ 命令一、仅查询 Bash 别名（保存在bashrc中）（临时 + 永久）（通用）
## 0.打开bashrc
```bash
code .bashrc
```
## 1.单行极简版（无颜色）
```bash
echo "===== Bash 临时别名（当前会话，关闭终端消失） ====="; alias | grep -v "alias grep=" || echo "  暂无临时 Bash 别名"; echo -e "\n===== Bash 永久别名（~/.bashrc 配置，所有会话生效） ====="; grep -n "alias " ~/.bashrc | grep -v "# alias" || echo "  ~/.bashrc 中无永久 Bash 别名"
```
## 2.多行带颜色版（推荐，区分度高）
```bash
#!/bin/bash
BLUE='\033[1;34m'
YELLOW='\033[0;33m'
NC='\033[0m'

echo -e "${BLUE}===== Bash 别名全量查询 =====${NC}"
# 1. 临时别名（当前会话）
echo -e "\n${BLUE}1. 临时别名（仅当前终端会话生效）${NC}"
alias | grep -v "alias grep=" || echo "  暂无临时 Bash 别名"

# 2. 永久别名（~/.bashrc 持久化）
echo -e "\n${BLUE}2. 永久别名（~/.bashrc 配置，所有会话生效）${NC}"
grep -n "alias " ~/.bashrc | grep -v "# alias" || echo "  ~/.bashrc 中无永久 Bash 别名"

# 可选：若你的永久别名保存在 ~/.bash_aliases，添加以下内容
# echo -e "\n${BLUE}3. 永久别名（~/.bash_aliases 配置）${NC}"
# grep -n "alias " ~/.bash_aliases | grep -v "# alias" || echo "  ~/.bash_aliases 中无永久 Bash 别名"
```

# 二、Git 别名查询（通用化改造方案）
自动遍历指定目录下的所有 Git 仓库（批量查询）
```bash
#!/bin/bash
BLUE='\033[1;34m'
GREEN='\033[0;32m'
RED='\033[0;31m'
NC='\033[0m'

# 核心配置：指定要遍历的根目录（可改为任意路径，如 ~/projects、./ 等）
TARGET_ROOT="./"  # ./ 表示当前目录，可替换为绝对路径

echo -e "${BLUE}===== Git 别名查询（自动遍历 $TARGET_ROOT 下的所有仓库） =====${NC}"
echo -e "\n${BLUE}1. 全局别名（所有仓库生效）${NC}"
git config --global --get-regexp alias || echo "  暂无全局 Git 别名"

echo -e "\n${BLUE}2. 本地别名（自动识别的仓库）${NC}"
# 自动查找所有 .git 目录，提取仓库根路径
find "$TARGET_ROOT" -type d -name ".git" | while read -r git_dir; do
  repo_dir=$(dirname "$git_dir")
  echo -e ""
  echo -e "  ${GREEN}仓库：$repo_dir${NC}"
  cd "$repo_dir" && git config --local --get-regexp alias || echo "    暂无本地别名"
done
``` 
