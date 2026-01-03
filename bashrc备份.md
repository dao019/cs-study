# 一、创建 bashrc-backup 子文件夹（保存在 dao 主文件夹）
```bash
# 在 dao 主文件夹下创建 bashrc-backup 子文件夹（-p 确保文件夹不存在时创建，存在时不报错）
mkdir -p ~/bashrc-backup
```
路径：子文件夹位置为 ~/bashrc-backup（等价于 /home/dao/bashrc-backup）。
特点：非隐藏文件夹，直接 ls ~ 就能看到。

# 二、备份 .bashrc 到该子文件夹，且备份文件带日期
推荐使用 年-月-日 格式（清晰易读），命令如下：
```bash
# 备份 ~/.bashrc 到 ~/bashrc-backup 文件夹，文件名带日期：bashrc-2026-01-03
cp ~/.bashrc ~/bashrc-backup/bashrc-$(date +%F)
```
备份文件路径：~/bashrc-backup/bashrc-2026-01-03（子文件夹内 + 带日期）。
核心优势：每次备份文件名唯一，绝对不会覆盖旧备份；所有备份都集中在一个文件夹里，方便管理。

# 三、验证备份是否成功（简单直观）
```bash
# 查看子文件夹内所有带日期的备份文件
ls ~/bashrc-backup/
```

# 四、从子文件夹的备份恢复 .bashrc（误修改时使用）
```bash
ls ~/bashrc-backup/
# 示例：恢复 2026-01-03 的备份
cp ~/bashrc-backup/bashrc-2026-01-03 ~/.bashrc
source ~/.bashrc
```

# 五、删除
```bash
# 删除单个旧备份（比如 2026-01-02 的备份）
rm -i ~/bashrc-backup/bashrc-2026-01-02
# 删除整个备份文件夹（谨慎使用，会删除所有备份）
rm -ri ~/bashrc-backup
```