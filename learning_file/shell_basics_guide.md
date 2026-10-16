# Shell Operations & Modern Tooling Guide | Shell 基础与现代开发者工具指南

> 一份覆盖 Linux Bash 与 Windows PowerShell 的双栈 Shell 入门指南，从基础命令到自动化脚本，附 AI 开发与 GPU 监控实用指令。

---

## 目录

1. [Shell 是什么：Bash vs PowerShell](#一shell-是什么bash-vs-powershell)
2. [命令的基本结构](#二命令的基本结构)
3. [文件与目录操作](#三文件与目录操作)
4. [查看文件内容](#四查看文件内容)
5. [查找文件与搜索内容](#五查找文件与搜索内容)
6. [管道与重定向](#六管道与重定向)
7. [变量与环境变量](#七变量与环境变量)
8. [进程与系统管理](#八进程与系统管理)
9. [压缩与归档](#九压缩与归档)
10. [网络常用命令](#十网络常用命令)
11. [脚本编写基础](#十一脚本编写基础)
12. [常用命令速查表](#十二常用命令速查表)
13. [常见坑与最佳实践](#十三常见坑与最佳实践)
14. [AI 与现代开发常用命令扩展](#十四ai-与现代开发常用命令扩展)

---

## 一、Shell 是什么：Bash vs PowerShell

**Shell** 是用户与操作系统内核之间的"翻译官"——你输入命令，它负责解析并执行，再把结果返回给你。它既是**交互式命令行**，也是**脚本语言**。

```
┌──────────┐    命令     ┌──────────┐   系统调用  ┌──────────┐
│   你     │ ─────────→  │  Shell   │ ──────────→ │   内核   │
│          │ ←─────────  │ (bash /  │ ←────────── │ (Linux / │
└──────────┘    结果     │   pwsh)  │    输出     │  Windows)│
                         └──────────┘             └──────────┘
```

| 特性 | Bash（Linux/macOS） | PowerShell（Windows/跨平台） |
|---|---|---|
| 设计哲学 | 一切皆**文本**（字节流） | 一切皆**对象** |
| 命令风格 | 简短小写：`ls`、`cp` | 动词-名词：`Get-ChildItem` |
| 大小写 | **区分**大小写（文件名） | 命令不区分，Linux 文件系统区分 |
| 路径分隔符 | `/`（正斜杠） | `\`（反斜杠，`/` 也兼容） |
| 默认提示符 | `$`（普通用户）`#`（root） | `PS C:\>` |
| 脚本扩展名 | `.sh` | `.ps1` |
| 帮助命令 | `man <cmd>` / `<cmd> --help` | `Get-Help <cmd>` / `<cmd> -?` |

> 💡 **选择建议**：Linux 服务器开发学 Bash；Windows 日常管理与自动化用 PowerShell；两者底层思想相通，学透一个，另一个对照速查表即可上手。

---

## 二、命令的基本结构

两个 Shell 的命令都遵循同一模式：

```
命令 [选项] [参数]

  │       │        │
  │       │        └─ 操作的对象（文件、目录、字符串……）
  │       └─ 调整行为的开关（-a、--all、-Force）
  └─ 要执行的程序名
```

```bash
# Bash
ls -la /home            # ls 命令，-l -a 选项（可合并为 -la），/home 是参数
```

```powershell
# PowerShell
Get-ChildItem -Force -Path C:\Users    # 完整写法
ls -Force C:\Users                     # 别名写法（ls 是 Get-ChildItem 的别名）
```

### 关键差异：选项的写法

| | Bash | PowerShell |
|---|---|---|
| 短选项 | `-l`（可合并：`-la`） | `-Force`（一般不合并） |
| 长选项 | `--all` | 动词化参数：`-Recurse` |
| 赋值 | `--width=80` 或 `-w 80` | `-Width 80`（空格分隔） |

> 💡 PowerShell 的参数名几乎都是**完整单词**，可缩写为无歧义前缀：`-Recurse` 可写 `-rec`，但不推荐，可读性差。

### 获取帮助

```bash
# Bash
man ls                  # 完整手册（按 q 退出，/关键词 搜索）
ls --help               # 快速摘要
which ls                # 查看命令的安装路径
type ls                 # 查看命令类型（别名/内置/外部程序）
```

```powershell
# PowerShell
Get-Help Get-ChildItem              # 查看帮助
Get-Help Get-ChildItem -Examples    # 只看示例（最实用）
Get-Help Get-ChildItem -Online      # 打开在线文档
Get-Command ls                      # 查看命令对应的真实名称与路径
Get-Member                          # 用管道查看对象有哪些属性和方法（见第六节）
```

---

## 三、文件与目录操作

### 3.1 定位：我在哪 / 我是谁

```bash
# Bash
pwd                     # 当前目录（print working directory）
whoami                  # 当前用户名
hostname                # 主机名
```

```powershell
# PowerShell
Get-Location            # 当前目录（别名：pwd）
whoami                  # 当前用户（域\用户名）
hostname                # 主机名
```

### 3.2 查看目录内容

```bash
# Bash
ls                      # 列出文件
ls -l                   # 长格式（权限、大小、时间）
ls -a                   # 包含隐藏文件（以 . 开头）
ls -lh                  # 人类可读的大小（K/M/G）
ls -lt                  # 按修改时间排序
ls -R                   # 递归列出子目录
```

```powershell
# PowerShell
Get-ChildItem                       # 列出文件（别名：ls / dir / gci）
Get-ChildItem -Force                # 包含隐藏文件（注意：不是 -a）
Get-ChildItem -Recurse              # 递归
Get-ChildItem | Sort-Object Length -Descending   # 按大小排序
```

> ⚠️ **易错点**：Bash 的隐藏文件以 `.` 开头，用 `-a` 显示；PowerShell 的隐藏由**文件属性**决定，用 `-Force` 显示。`ls -a` 在 PowerShell 里等于普通 `ls`，没有"显示隐藏"效果。

### 3.3 目录跳转

```bash
# Bash
cd /etc/nginx           # 绝对路径
cd projects             # 相对路径
cd ..                   # 上一级
cd ../..                # 上两级
cd ~ 或 cd              # 回到家目录
cd -                    # 回到上一次所在的目录
```

```powershell
# PowerShell
Set-Location C:\logs    # 别名：cd / sl
cd ..                   # 上一级
cd ~                    # 回到用户主目录（cd 不带参数会显示当前位置，不跳转！）
cd -                    # 回到上一次所在目录
```

> ⚠️ **易错点**：PowerShell 中单独输入 `cd` **不**会回家目录（Bash 会），必须写 `cd ~`。

### 3.4 创建 / 复制 / 移动 / 删除

```bash
# Bash —— 创建
mkdir demo                    # 创建目录
mkdir -p a/b/c                # 递归创建多级目录
touch note.txt                # 创建空文件 / 更新时间戳

# 复制
cp file.txt backup.txt        # 复制文件
cp -r src/ dst/               # 递归复制目录（-r 必不可少）
cp -i file.txt dst/           # 覆盖前询问

# 移动 / 重命名（同一操作）
mv old.txt new.txt            # 重命名
mv file.txt /tmp/             # 移动
mv -i a.txt dst/              # 覆盖前询问

# 删除
rm file.txt                   # 删除文件
rm -r dir/                    # 递归删除目录
rm -rf dir/                   # 强制递归删除（危险！不可恢复）
rmdir empty_dir               # 只能删除空目录
```

```powershell
# PowerShell —— 创建
New-Item -ItemType Directory demo         # 创建目录（别名：mkdir）
mkdir -p a/b/c 不存在 → New-Item -ItemType Directory a/b/c -Force   # -Force 递归创建
New-Item -ItemType File note.txt          # 创建空文件
ni note.txt                               # ni 是 New-Item 别名，默认创建文件

# 复制
Copy-Item file.txt backup.txt             # 别名：cp / copy
Copy-Item src dst -Recurse                # 递归复制目录
Copy-Item src dst -Recurse -Force         # 覆盖不提示（Bash 的 cp 默认覆盖，PS 默认在目标存在目录时提示）

# 移动 / 重命名
Move-Item old.txt new.txt                 # 别名：mv / move
Rename-Item old.txt new.txt               # 专职重命名

# 删除
Remove-Item file.txt                      # 别名：rm / del
Remove-Item dir -Recurse                  # 递归删除
Remove-Item dir -Recurse -Force           # 连隐藏/只读文件一起删（同样危险！）
```

> ⚠️ **最危险命令**：`rm -rf /`（Bash）与 `Remove-Item C:\ -Recurse -Force`（PowerShell）都会试图删除整个系统。删除前先 `pwd` / `Get-Location` 确认当前位置，能用 `-i`（确认模式）就用。

### 3.5 路径中的特殊符号

| 符号 | Bash 含义 | PowerShell 含义 |
|---|---|---|
| `.` | 当前目录 | 当前目录 |
| `..` | 上一级目录 | 上一级目录 |
| `~` | 家目录 `/home/user` | 用户主目录 `C:\Users\name` |
| `/` | 根目录 | 盘符根（如 `C:\`） |
| `-` | 上一次目录（配合 `cd`） | 上一次目录（配合 `cd`） |
| `*` | 通配任意字符 | 通配任意字符 |

```bash
cp *.log /tmp/backup/       # Bash：复制所有 .log 文件
ls ~/Documents               # 家目录下的 Documents
```

```powershell
Copy-Item *.log C:\temp\backup\    # PowerShell 同样支持通配符
ls ~\Documents                      # 注意 PowerShell 用反斜杠连接
```

---

## 四、查看文件内容

```bash
# Bash
cat file.txt                # 全部输出（小文件）
less file.txt               # 分页查看（q 退出，/ 搜索，大文件首选）
head file.txt               # 前 10 行
head -n 20 file.txt         # 前 20 行
tail file.txt               # 后 10 行
tail -f app.log             # 实时追踪文件新增内容（看日志神器，Ctrl+C 退出）
wc -l file.txt              # 统计行数
```

```powershell
# PowerShell
Get-Content file.txt                    # 全部输出（别名：cat / gc）
Get-Content file.txt -TotalCount 20     # 前 20 行（无 less 分页，可 piped 到 Out-Host -Paging）
Get-Content file.txt -Tail 10           # 后 10 行
Get-Content app.log -Wait               # 实时追踪（等价 tail -f，Ctrl+C 退出）
(Get-Content file.txt).Count            # 统计行数
```

> 💡 **查看大文件**：Bash 用 `less`；PowerShell 可用 `Get-Content big.log | Out-Host -Paging` 分页，或直接用 `more` 命令。

### 查看文件的另一种思路：PowerShell 一切皆对象

```powershell
# PowerShell 可以把文件读成对象数组，直接用下标和属性操作
$lines = Get-Content file.txt
$lines[0]                       # 第一行
$lines[-1]                      # 最后一行
$lines[10..20]                  # 第 11 到 21 行
$lines | Where-Object { $_ -match 'error' }   # 过滤包含 error 的行
```

Bash 要实现同样效果需要组合 `head`、`tail`、`sed` 等文本工具——这是"文本流"与"对象流"哲学差异的典型体现。

---

## 五、查找文件与搜索内容

### 5.1 按名字找文件

```bash
# Bash —— find
find . -name "*.log"                # 当前目录递归找 .log 文件
find /etc -name "nginx*"            # 指定目录下按名字找
find . -type d -name "node_modules" # -type d 找目录，f 找文件
find . -size +100M                  # 找大于 100MB 的文件
find . -mtime -7                    # 7 天内修改过的文件
find . -name "*.tmp" -delete        # 找到即删除（谨慎！先去掉 -delete 确认列表）
```

```powershell
# PowerShell —— Get-ChildItem
Get-ChildItem -Recurse -Filter *.log            # 递归找 .log
Get-ChildItem C:\logs -Filter "nginx*"          # 指定目录按名字找
Get-ChildItem -Recurse -Directory -Filter node_modules   # 只找目录
Get-ChildItem -Recurse | Where-Object Length -gt 100MB   # 大于 100MB
Get-ChildItem -Recurse | Where-Object LastWriteTime -gt (Get-Date).AddDays(-7)
```

### 5.2 按内容搜文本

```bash
# Bash —— grep
grep "error" app.log                # 在文件中搜索
grep -i "error" app.log             # 忽略大小写
grep -n "error" app.log             # 显示行号
grep -r "TODO" ./src                # 递归搜索目录
grep -v "debug" app.log             # 反向：排除匹配行
grep -c "error" app.log             # 只统计匹配行数
grep -E "err(or|or2)" app.log       # 扩展正则
```

```powershell
# PowerShell —— Select-String
Select-String "error" app.log           # 别名：sls
Select-String "error" app.log -CaseSensitive    # 默认就忽略大小写，此参数反过来开启区分
Select-String "TODO" -Path .\src\*      # 搜索多文件
Select-String "error" app.log | Measure-Object   # 统计
Select-String "error" -Path .\logs\*.log -Context 2   # 带上下文 2 行（类似 grep -C）
```

### 5.3 经典组合：找文件 → 搜内容

```bash
# Bash
grep -r "apiKey" --include="*.js" ./src
find . -name "*.conf" -exec grep "listen" {} \;
```

```powershell
# PowerShell
Get-ChildItem -Recurse -Filter *.js | Select-String "apiKey"
```

---

## 六、管道与重定向

这是 Shell 最强大的能力：把一个命令的输出"喂"给下一个命令。

```
┌────────┐  stdout  ┌────────┐  stdout  ┌────────┐
│ 命令 A │ ───────→ │ 命令 B │ ───────→ │ 命令 C │ → 屏幕/文件
└────────┘  (管道)  └────────┘  (管道)  └────────┘
```

### 6.1 管道

```bash
# Bash：文本流管道
ls -l | grep ".log"             # 只看 .log 文件
cat access.log | sort | uniq -c | sort -rn | head    # 统计日志中出现最多的行
ps aux | grep nginx             # 查找 nginx 进程
```

```powershell
# PowerShell：对象管道（更强大）
Get-ChildItem | Where-Object Name -match "log"          # 过滤对象
Get-Process | Where-Object CPU -gt 100 | Sort-Object CPU -Descending
Get-Content app.log | Group-Object | Sort-Object Count -Descending | Select-Object -First 10
```

> 💡 **本质区别**：Bash 管道传递的是**纯文本**，接收方需要自己解析；PowerShell 管道传递的是**完整对象**（带属性和方法），下游命令直接访问 `.Name`、`.Length` 等属性，无需解析、不怕空格。

### 6.2 重定向

| 符号 | Bash | PowerShell |
|---|---|---|
| 覆盖写入文件 | `>` | `>` 或 `Out-File` |
| 追加写入 | `>>` | `>>` |
| 标准错误 | `2>` | `2>` |
| 合并输出与错误 | `&>` 或 `> file 2>&1` | `*>&1` |
| 丢弃输出 | `> /dev/null` | `> $null` |
| 从文件读入 | `< file` | `Get-Content file \| 命令` |

```bash
# Bash
echo "hello" > out.txt          # 覆盖写入
echo "world" >> out.txt         # 追加
ls /notexist 2> err.log         # 错误信息单独写入
command > out.log 2>&1          # 输出和错误都进 out.log
grep "error" app.log > result.txt 2> /dev/null
```

```powershell
# PowerShell
"hello" > out.txt
"world" >> out.txt
Get-ChildItem C:\notexist 2> err.txt
command *> out.log              # 所有流（输出+错误+警告+详细信息）全进文件
command > $null                 # 丢弃输出（等价 /dev/null）
```

> ⚠️ **易错点**：PowerShell 5.1 中 `>` 默认是 **UTF-16** 编码，跨工具处理文本时建议显式 `Out-File -Encoding utf8`。PowerShell 7+ 已默认 UTF-8。

---

## 七、变量与环境变量

### 7.1 普通变量

```bash
# Bash：赋值等号两边【不能有空格】
name="Alice"
echo "Hello, $name"             # 双引号内变量会被展开
echo 'Hello, $name'             # 单引号内【不】展开，原样输出
echo "Hello, ${name}ly"         # 用花括号界定变量名边界
unset name                      # 删除变量
```

```powershell
# PowerShell：变量总是以 $ 开头，等号两边有空格无所谓
$name = "Alice"
"Hello, $name"                  # 双引号内展开（输出时可省略 Write-Output）
'Hello, $name'                  # 单引号内不展开
"Hello, $($name.Length) chars"  # $($表达式) 可内嵌任意表达式
Remove-Variable name            # 删除变量
```

### 7.2 环境变量

```bash
# Bash
echo $PATH                      # 查看环境变量
echo $HOME                      # 家目录
export MY_VAR="hello"           # 设置（仅当前会话及其子进程有效）
export PATH=$PATH:/opt/tools/bin    # 追加 PATH（冒号分隔）
env                             # 列出所有环境变量
source ~/.bashrc                # 重新加载配置文件
```

```powershell
# PowerShell（通过 Env: 驱动器访问）
$env:PATH                       # 查看
$env:HOME / $env:USERPROFILE    # 家目录
$env:MY_VAR = "hello"           # 设置（仅当前会话有效）
$env:PATH += ";C:\tools\bin"    # 追加 PATH（Windows 用【分号】分隔！）
Get-ChildItem Env:              # 列出所有环境变量
```

> ⚠️ **永久设置的区别**：
> - Bash：写入 `~/.bashrc` / `~/.profile`，然后 `source` 生效
> - PowerShell（当前用户永久）：`[Environment]::SetEnvironmentVariable("MY_VAR", "hello", "User")`
> - 注意 Bash 的 PATH 分隔符是 `:`，Windows 是 `;`——写跨平台脚本时最容易踩的坑之一。

---

## 八、进程与系统管理

### 8.1 查看进程

```bash
# Bash
ps aux                          # 所有进程快照
ps aux | grep nginx             # 过滤指定进程
top                             # 实时监控（q 退出）
htop                            # 更友好的 top（需安装）
```

```powershell
# PowerShell
Get-Process                     # 所有进程（别名：ps）
Get-Process nginx               # 按名字过滤
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10   # CPU 占用前 10
Get-Process | Where-Object WorkingSet64 -gt 500MB                     # 内存超 500MB
```

### 8.2 结束进程

```bash
# Bash
kill 12345                      # 发送 TERM 信号（温和终止）
kill -9 12345                   # 强制杀死（SIGKILL，最后手段）
pkill nginx                     # 按名字杀
killall node                    # 杀所有同名进程
```

```powershell
# PowerShell
Stop-Process -Id 12345          # 别名：kill
Stop-Process -Name nginx        # 按名字杀
Stop-Process -Id 12345 -Force   # 强制
Get-Process node | Stop-Process -Force    # 管道批量杀
```

### 8.3 磁盘与系统信息

```bash
# Bash
df -h                           # 磁盘空间
du -sh ./project                # 目录总大小（-s 汇总，-h 人类可读）
free -h                         # 内存使用（Linux）
uname -a                        # 系统内核信息
```

```powershell
# PowerShell
Get-PSDrive -PSProvider FileSystem          # 磁盘空间
"{0:N2} GB" -f ((Get-ChildItem .\project -Recurse | Measure-Object Length -Sum).Sum / 1GB)   # 目录大小
Get-ComputerInfo | Select-Object OsName, CsTotalPhysicalMemory   # 系统信息（较慢）
$PSVersionTable                             # PowerShell 自身版本
```

### 8.4 权限（Linux 特有，简述）

```bash
# Bash —— 仅 Linux/macOS
ls -l file.txt                  # -rwxr-xr-- ：属主/属组/其他人 的读r写w执行x
chmod 755 script.sh             # rwxr-xr-x：属主全权，其他人读+执行
chmod +x script.sh              # 给所有者加执行权限（运行脚本前的常见操作）
sudo command                    # 以管理员身份执行
chown user:group file           # 改变文件属主
```

PowerShell 下对应的提权操作是**以管理员身份运行终端**后执行命令；细粒度 ACL 用 `Get-Acl` / `Set-Acl`（日常少用）。

---

## 九、压缩与归档

```bash
# Bash —— tar（归档）+ gzip（压缩），最常见组合
tar -czf archive.tar.gz dir/        # 创建：c 创建 z gzip 压缩 f 文件名
tar -xzf archive.tar.gz             # 解压：x 解开
tar -xzf archive.tar.gz -C /opt/    # 解压到指定目录
tar -tzf archive.tar.gz             # 只看内容不解压（先确认再解压，好习惯）

# zip / unzip
zip -r archive.zip dir/
unzip archive.zip -d dst/
```

```powershell
# PowerShell —— 内置 zip 支持
Compress-Archive -Path dir -DestinationPath archive.zip
Compress-Archive -Path *.log -Update archive.zip    # 追加
Expand-Archive -Path archive.zip -DestinationPath dst
Expand-Archive -Path archive.zip -DestinationPath dst -Force    # 覆盖已有文件
```

> 💡 PowerShell 5.1 只支持 zip 格式；要处理 `.tar.gz` 需用 `tar` 命令——**Windows 10+ 已自带 tar**，直接 `tar -xzf archive.tar.gz` 即可。

---

## 十、网络常用命令

| 用途 | Bash | PowerShell |
|---|---|---|
| 测试连通性 | `ping example.com` | `ping example.com` / `Test-Connection example.com` |
| 域名解析 | `nslookup example.com` / `dig example.com` | `Resolve-DnsName example.com` |
| HTTP 请求 | `curl https://api.example.com` | `Invoke-RestMethod https://api.example.com`（别名 `curl` 但行为不同！） |
| 查看本机 IP | `ip addr` / `ifconfig` | `Get-NetIPAddress` / `ipconfig` |
| 端口监听 | `ss -tlnp` / `netstat -tlnp` | `Get-NetTCPConnection -State Listen` |
| 远程登录 | `ssh user@host` | `ssh user@host`（Win10+ 自带 OpenSSH） |
| 文件传输 | `scp file user@host:/path` | `scp file user@host:C:/path` |
| 追踪路由 | `traceroute example.com` | `tracert example.com` |
| 下载文件 | `wget url` / `curl -O url` | `Invoke-WebRequest url -OutFile file` |

```bash
# Bash：curl 查 API
curl -s https://api.github.com/zen              # -s 静默模式
curl -X POST -H "Content-Type: application/json" \
     -d '{"name":"test"}' https://api.example.com/users
```

```powershell
# PowerShell：Invoke-RestMethod 直接返回对象，无需手动解析 JSON
$r = Invoke-RestMethod https://api.github.com/users/octocat
$r.name          # 直接访问属性 —— 对象哲学的又一次体现
$r.blog
```

> ⚠️ **易错点**：在 Windows PowerShell 里 `curl` 是 `Invoke-WebRequest` 的**别名**，不是真正的 curl，参数不兼容！需要真 curl 时用 `curl.exe`（Win10+ 自带）。

---

## 十一、脚本编写基础

### 11.1 第一个脚本

```bash
#!/bin/bash
# Bash 脚本：hello.sh
# 第一行 shebang 指定解释器；保存后需赋执行权限

echo "Hello, Shell!"
```

```bash
chmod +x hello.sh       # 赋执行权限（只需一次）
./hello.sh              # 用相对路径执行（注意必须带 ./）
bash hello.sh           # 或直接用 bash 执行（无需权限）
```

```powershell
# PowerShell 脚本：hello.ps1 —— 无需 shebang，按扩展名识别
Write-Output "Hello, PowerShell!"
```

```powershell
.\hello.ps1             # 执行（同样必须带 .\）
```

> ⚠️ **PowerShell 执行策略**：Windows 默认禁止运行脚本，首次运行会报错。放行当前用户：
> ```powershell
> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
> ```
> `RemoteSigned` 表示本地脚本可直接运行，网上下载的需签名——安全与便利的平衡点。

### 11.2 变量与用户输入

```bash
#!/bin/bash
name="World"
read -p "你的名字: " yourname       # 交互式输入
echo "Hello, $yourname!"

# 命令替换：把命令的输出存进变量
today=$(date +%Y-%m-%d)             # 推荐 $() 写法
files=$(ls | wc -l)
echo "今天是 $today，当前目录有 $files 个文件"
```

```powershell
# PowerShell
$name = "World"
$yourname = Read-Host "你的名字"    # 交互式输入
"Hello, $yourname!"

# 命令替换：直接把命令结果赋给变量（天然支持）
$today = Get-Date -Format "yyyy-MM-dd"
$files = (Get-ChildItem).Count
"今天是 $today，当前目录有 $files 个文件"
```

### 11.3 条件判断

```bash
#!/bin/bash
# Bash：注意 if 后的空格、方括号内的空格，-fi 结构固定
score=85

if [ $score -ge 90 ]; then
    echo "优秀"
elif [ $score -ge 60 ]; then
    echo "及格"
else
    echo "不及格"
fi

# 文件判断
if [ -f "config.txt" ]; then        # -f 文件存在 / -d 目录存在 / -z 空字符串
    echo "配置存在"
fi

# 字符串与逻辑
if [ "$name" = "admin" ] && [ -d "/data" ]; then
    echo "管理员且数据目录存在"
fi
```

```powershell
# PowerShell：真正的语言级 if/elseif/else，无需 fi 收尾
$score = 85

if ($score -ge 90) {
    "优秀"
} elseif ($score -ge 60) {
    "及格"
} else {
    "不及格"
}

# 比较运算符对照：-eq 等于  -ne 不等  -gt 大于  -lt 小于  -ge 大于等于  -le 小于等于
# 字符串匹配：-like（通配符）  -match（正则）
if (Test-Path "config.txt" -PathType Leaf) {
    "配置存在"
}

if ($name -eq "admin" -and (Test-Path "C:\data")) {
    "管理员且数据目录存在"
}
```

| 逻辑含义 | Bash | PowerShell |
|---|---|---|
| 等于 | `[ "$a" = "$b" ]`（数字用 `-eq`） | `-eq` |
| 大于 | `-gt` | `-gt` |
| 且 / 或 / 非 | `&&` / `||` / `!` | `-and` / `-or` / `-not`（语句内） |
| 文件存在 | `[ -f x ]` | `Test-Path x` |
| 目录存在 | `[ -d x ]` | `Test-Path x -PathType Container` |

### 11.4 循环

```bash
#!/bin/bash
# Bash：遍历列表
for fruit in apple banana cherry; do
    echo "I like $fruit"
done

# 遍历命令输出
for f in *.log; do
    echo "处理 $f"
done

# C 风格
for ((i=1; i<=5; i++)); do
    echo "第 $i 次"
done

# while：逐行读文件（经典写法）
while read -r line; do
    echo "行内容: $line"
done < config.txt
```

```powershell
# PowerShell
foreach ($fruit in "apple", "banana", "cherry") {
    "I like $fruit"
}

# 管道遍历（更 PowerShell 化，大数据量更省内存）
Get-ChildItem *.log | ForEach-Object {
    "处理 $($_.Name)"
}

# 数字循环
1..5 | ForEach-Object { "第 $_ 次" }

# while
$i = 1
while ($i -le 5) {
    "第 $i 次"
    $i++
}

# 逐行读文件
Get-Content config.txt | ForEach-Object {
    "行内容: $_"
}
```

> 💡 `$_` 是 PowerShell 管道中的"当前对象"，相当于 Bash 循环变量 `$f`，出现在 `ForEach-Object`、`Where-Object` 的脚本块里。

### 11.5 函数

```bash
#!/bin/bash
# Bash：函数内用 $1 $2 接收参数，$# 是参数个数
greet() {
    local name=$1              # local 限制变量作用域
    echo "Hello, $name!"
}

greet "Alice"                  # 调用时不加括号
```

```powershell
# PowerShell：param 块声明参数，支持类型和默认值
function Greet {
    param(
        [string]$Name = "World",
        [int]$Times = 1
    )
    for ($i = 1; $i -le $Times; $i++) {
        "Hello, $Name!"
    }
}

Greet -Name "Alice" -Times 2    # 调用用【空格】分隔参数，不加逗号不加括号！
```

> ⚠️ **易错点**：PowerShell 函数调用是 `Greet -Name "Alice"`（空格分隔），写成 `Greet("Alice")` 会把整串当第一个位置参数且不支持命名参数——这是从其他语言转来的开发者最常犯的错误。

### 11.6 退出码与错误处理

```bash
#!/bin/bash
ls /notexist
echo $?                 # 0 表示成功，非 0 表示失败

set -e                  # 任何命令失败立即终止脚本（严谨脚本的标配）
set -u                  # 使用未定义变量时报错
set -euo pipefail       # 常见组合写法

# 主动退出
if [ ! -f "required.txt" ]; then
    echo "缺少必需文件" >&2     # 错误信息输出到 stderr
    exit 1
fi
```

```powershell
# PowerShell
$LASTEXITCODE           # 上一条【外部程序】的退出码
$?                      # 上一条命令是否成功（True/False，含 cmdlet）

$ErrorActionPreference = "Stop"    # 让非终止错误升级为终止错误（类似 set -e）

if (-not (Test-Path "required.txt")) {
    Write-Error "缺少必需文件"
    exit 1
}

# try/catch
try {
    Get-Content "may-not-exist.txt" -ErrorAction Stop
} catch {
    "出错了: $($_.Exception.Message)"
}
```

---

## 十二、常用命令速查表

按用途分类，左 Bash 右 PowerShell，建议收藏：

### 文件与目录

| 用途 | Bash | PowerShell |
|---|---|---|
| 当前目录 | `pwd` | `pwd` / `Get-Location` |
| 列出文件 | `ls` / `ls -la` | `ls` / `ls -Force` |
| 跳转 | `cd` / `cd ..` / `cd -` | `cd` / `cd ..` / `cd -` |
| 家目录 | `cd ~` / `cd` | `cd ~` |
| 创建目录 | `mkdir` / `mkdir -p a/b/c` | `mkdir a/b/c`（自动递归） |
| 创建文件 | `touch f.txt` | `ni f.txt` |
| 复制 | `cp` / `cp -r` | `cp` / `cp -Recurse` |
| 移动/重命名 | `mv` | `mv` / `Rename-Item` |
| 删除 | `rm` / `rm -rf` | `rm` / `rm -Recurse -Force` |
| 链接 | `ln -s target link` | `New-Item -ItemType SymbolicLink` |

### 查看与搜索

| 用途 | Bash | PowerShell |
|---|---|---|
| 输出文本 | `echo` | `Write-Output` / `echo` / 直接写字符串 |
| 查看文件 | `cat` / `less` | `cat` / `gc` / `Out-Host -Paging` |
| 前/后 N 行 | `head -n 20` / `tail -n 20` | `gc -TotalCount 20` / `gc -Tail 20` |
| 实时日志 | `tail -f app.log` | `gc app.log -Wait` |
| 找文件 | `find . -name "*.log"` | `gci -Recurse -Filter *.log` |
| 搜内容 | `grep -rn "x" .` | `gci -Recurse \| sls "x"` |
| 统计行数 | `wc -l` | `(gc f).Count` |
| 排序/去重 | `sort` / `uniq` | `Sort-Object` / `Select-Object -Unique` |

### 系统与进程

| 用途 | Bash | PowerShell |
|---|---|---|
| 进程列表 | `ps aux` | `ps` / `Get-Process` |
| 杀进程 | `kill PID` / `pkill name` | `kill -Name name` / `Stop-Process -Id PID -Force` |
| 磁盘空间 | `df -h` | `Get-PSDrive` |
| 目录大小 | `du -sh dir` | 递归求和（见 8.3） |
| 提权 | `sudo cmd` | 管理员终端运行 |
| 清屏 | `clear` / `Ctrl+L` | `clear` / `cls` |
| 历史 | `history` | `Get-History` / F7 |
| 退出 | `exit` | `exit` |

### 快捷键（两个 Shell 通用或等价）

| 按键 | 作用 |
|---|---|
| `Tab` | 自动补全（PowerShell 支持参数名补全，更强大） |
| `↑` / `↓` | 翻阅历史命令 |
| `Ctrl+C` | 终止当前命令 |
| `Ctrl+R`（Bash） | 搜索历史命令（PowerShell 7: `Ctrl+R` 进入交互搜索） |
| `Ctrl+A` / `Ctrl+E` | 光标移到行首 / 行尾 |
| `Ctrl+U` / `Ctrl+K` | 删除光标前 / 后内容 |

---

## 十三、常见坑与最佳实践

### 13.1 新手高频错误

| 错误 | 说明 | 正确做法 |
|---|---|---|
| Bash 赋值带空格 | `name = "x"` 会被当作命令执行 | `name="x"`，等号两侧无空格 |
| Bash 执行脚本不带 `./` | `hello.sh` 提示找不到命令 | `./hello.sh` |
| PowerShell 调函数加括号 | `Greet("x")` 传参错误 | `Greet -Name x`，空格分隔 |
| PS 中 `curl` 不是 curl | 是 `Invoke-WebRequest` 别名 | 用 `curl.exe` |
| PS 5.1 重定向编码 | `>` 产出 UTF-16 文件 | `Out-File -Encoding utf8` 或用 PS7 |
| Windows PATH 分隔符 | 用了 `:` 分隔 | Windows 用 `;` |
| `rm -rf` 前不确认 | 删错目录无法恢复 | 先 `pwd` 确认位置；先列出再删除 |
| Bash 忘记 `cp -r` | 复制目录报错 | 目录必须加 `-r`（PS 的 `cp -Recurse` 同理） |

### 13.2 Bash 最佳实践

```bash
#!/bin/bash
set -euo pipefail           # 1. 严格模式：出错即停、变量必须定义、管道错误传播

readonly BACKUP_DIR="/var/backup"   # 2. 常量用 readonly，集中定义

main() {                    # 3. 逻辑放进 main 函数，脚本底部调用
    backup "$1"
}

backup() {
    if [ ! -d "$1" ]; then  # 4. 输入先校验再使用
        echo "目录不存在: $1" >&2
        exit 1
    fi
    tar -czf "$BACKUP_DIR/$(date +%F).tar.gz" "$1"   # 5. 变量永远加双引号，防空格炸裂
}

main "$@"                   # "$@" 把脚本参数原样传给 main
```

### 13.3 PowerShell 最佳实践

```powershell
#Requires -Version 5.1           # 1. 声明最低版本要求

[CmdletBinding()]                # 2. 让脚本支持 -Verbose 等通用参数
param(
    [Parameter(Mandatory)]       # 3. 必填参数自动校验 + 提示输入
    [string]$Path,
    [string]$BackupDir = "C:\backup"
)

Set-StrictMode -Version Latest   # 4. 类似 set -u：未定义变量报错

if (-not (Test-Path $Path)) {    # 5. 用完整 cmdlet 名写脚本（别名仅限交互使用）
    Write-Error "路径不存在: $Path"
    exit 1
}

Compress-Archive -Path $Path -DestinationPath "$BackupDir\$(Get-Date -Format yyyy-MM-dd).zip" -Force
Write-Verbose "备份完成: $Path -> $BackupDir"   # 6. 过程信息用 Write-Verbose，不污染正常输出
```

### 13.4 学习路线建议

```
第一步：交互使用 —— 把日常 GUI 文件操作换成命令行（本指南第三~五节）
   ↓
第二步：管道思维 —— 学会用 | 组合命令解决问题（第六节）
   ↓
第三步：脚本化 —— 把重复操作写成脚本（第十一节）
   ↓
第四步：工程化 —— 错误处理、参数化、日志，脚本进化为小工具（第十三节）
```

> 💡 **验证学习成果的小测验**：不查资料，试着只用命令行完成——
> 1. 在临时目录创建三级目录结构并放几个文件；
> 2. 找出其中所有 `.log` 文件里含 `error` 的行并统计数量；
> 3. 把这些文件打包压缩，再删掉原目录；
> 4. 把以上四步写成一个可复用的脚本。
>
> 能独立完成，本指南的目的就达到了。

---

## 十四、AI 与现代开发常用命令扩展

在进行深度学习训练、大模型部署与后端联调时，以下命令是高频必备技能：

### 14.1 GPU 与算力状态监控

```bash
# 查看 NVIDIA 显卡显存占用、驱动与 CUDA 版本
nvidia-smi

# 每隔 1 秒动态刷新监控显卡占用（Linux）
watch -n 1 nvidia-smi

# PowerShell 下的等效动态刷新：
while ($true) { Clear-Host; nvidia-smi; Start-Sleep -Seconds 1 }
```

### 14.2 现代 Python 包管理工具 `uv` 常用命令

```bash
# 1. 极速创建独立虚拟环境（耗时 < 1 秒）
uv venv

# 2. 激活虚拟环境
# Linux / macOS:
source .venv/bin/activate
# Windows PowerShell:
.venv\Scripts\Activate.ps1

# 3. 极速安装依赖（比旧 pip 快 10-100 倍）
uv pip install torch torchvision fastapi uvicorn streamlit

# 4. 免激活直接运行脚本或工具
uv run python main.py
```

### 14.3 网络诊断与 AI 流式接口测试

```bash
# 检查某个端口是否被占用（例如 8000 端口）
# Linux:
lsof -i :8000   # 或 netstat -tulnp | grep 8000
# Windows PowerShell:
Get-NetTCPConnection -LocalPort 8000

# 测试大模型 SSE (Server-Sent Events) 打字机流式接口（关闭缓冲逐字输出）
curl -N -s -X POST http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{"model": "qwen2.5", "messages": [{"role": "user", "content": "hi"}], "stream": true}'
```

### 14.4 Windows WSL2 (Ubuntu) 常用命令

```powershell
# 查看已安装的 WSL 分发版状态
wsl -l -v

# 启动默认 Ubuntu 终端
wsl

# 关闭正在运行的 WSL 实例（释放内存）
wsl --shutdown
```
