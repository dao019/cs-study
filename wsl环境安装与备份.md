`安装参考：https://www.sysgeek.cn/install-wsl-2-windows/ 和ai即可`
# 一、备份软件源配置文件备份（仅护源地址，极小体积）
适用场景
仅保护 /etc/apt/sources.list.d/ubuntu.sources 源配置文件，防止修改源时出错，与已安装软件无关。
## 1. 执行备份（当前已完成，可重复执行覆盖旧备份）
```bash
# 备份 ubuntu.sources 到同目录，后缀 .bak
sudo cp /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak
```

## 2. 验证备份成功（无需 sudo）
```bash
# 验证备份文件存在
ls /etc/apt/sources.list.d/ubuntu.sources.bak
# 进阶验证：内容与原文件一致（无输出则一致）
diff /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/ubuntu.sources.bak
```

## 3. 恢复方法（源配置出错时使用）
```bash
# 用备份文件覆盖出错的源配置文件
sudo cp /etc/apt/sources.list.d/ubuntu.sources.bak /etc/apt/sources.list.d/ubuntu.sources
# 恢复后更新源，验证是否正常
sudo apt update
```

## 4. 删除备份（仅当确认源配置永久稳定且追求整洁时）
```bash
sudo rm /etc/apt/sources.list.d/ubuntu.sources.bak
```


# 二、备份 2：WSL 完整环境备份（含所有软件 / 配置 / 文件，完整镜像）
适用场景
备份当前 已安装的所有软件、系统配置、用户文件、源配置等完整 WSL 环境，用于系统崩溃、重装 WSL 或迁移到其他电脑时恢复。
## 1. 执行备份（需在 Windows PowerShell 中运行，推荐存到 D 盘）
### 步骤（1）关闭所有 WSL 实例，确保数据一致性
```powershell
wsl --shutdown
```

### 步骤（2）导出完整环境到备份文件（替换为你的存储路径）
```powershell
# 格式：wsl --export <发行版名称> <备份文件路径.tar>
wsl --export Ubuntu-24.04 D:\WSL-Backups\ubuntu-24.04-full-backup-$(Get-Date -Format "yyyyMMdd").tar
```
说明：$(Get-Date -Format "yyyyMMdd") 会自动添加日期后缀（如 20251231），避免覆盖旧备份。
耗时：根据环境大小（如你当前 1.9G），约 5-10 分钟。

## 2. 验证备份成功
```powershell
# 检查备份文件是否存在且大小合理（应与你的 ext4.vhdx 实际大小接近）
Get-Item D:\WSL-Backups\ubuntu-24.04-full-backup-*.tar
```

## 3. 恢复方法（WSL 环境损坏或迁移时使用）
### 步骤（1）（可选）注销损坏的旧发行版（数据会丢失，谨慎操作）
```powershell
wsl --unregister Ubuntu-24.04
```

### 步骤（2）从备份文件导入并创建新的 WSL 实例
```powershell
# 格式：wsl --import <新发行版名称> <安装路径> <备份文件路径.tar> --version 2
wsl --import Ubuntu-24.04 D:\WSL\Ubuntu-24.04 D:\WSL-Backups\ubuntu-24.04-full-backup-20251231.tar --version 2
```

### 步骤（3）设置默认用户（恢复后默认可能为 root，需重新指定 dao）
```powershell
# 进入恢复的WSL实例
wsl -d Ubuntu-24.04
# 在WSL终端中执行（设置默认用户为dao）
echo "[user]" | sudo tee -a /etc/wsl.conf
echo "default=dao" | sudo tee -a /etc/wsl.conf
# 退出并重启WSL
exit
wsl --shutdown
```

## 4. 删除备份（仅当备份文件过多占用空间时）
```powershell
# 删除指定日期的备份文件
Remove-Item D:\WSL-Backups\ubuntu-24.04-full-backup-20251231.tar
```