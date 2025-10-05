# Linux Shell编程：grep、sed、awk三剑客详解 - 命令详细解释



## 第一部分：grep - 文本搜索利器

### 1.3.1 基础选项

```bash
# 基本搜索 - 在file.txt中查找包含"pattern"的行
echo "=== 基本搜索 ==="
grep "pattern" file.txt
```

**详细解释**：

- `grep`：文本搜索命令
- `"pattern"`：搜索模式，查找包含"pattern"字符串的行
- `file.txt`：目标文件
- 区分大小写，只匹配小写"pattern"

  

```bash
# 忽略大小写 - 查找包含"pattern"的行（忽略大小写）
echo -e "\n=== 忽略大小写 ==="
grep -i "pattern" file.txt
```

**详细解释**：

- `-i`：忽略大小写选项
- 匹配"pattern"、"Pattern"、"PATTERN"等所有大小写变体
- `echo -e "\n"`：输出换行符，美化输出格式

  

```bash
# 显示行号 - 显示匹配行的行号
echo -e "\n=== 显示行号 ==="
grep -n "pattern" file.txt
```

**详细解释**：

- `-n`：显示行号选项
- 输出格式为"行号:匹配行内容"
- 便于定位匹配内容在文件中的位置

  

```bash
# 只显示匹配次数 - 统计匹配行的数量
echo -e "\n=== 只显示匹配次数 ==="
grep -c "pattern" file.txt
```

**详细解释**：
- `-c`：计数选项
- 只输出匹配行的总数，不显示具体内容
- 用于快速统计出现频率

  

```bash
# 反向匹配 - 显示不包含"pattern"的行
echo -e "\n=== 反向匹配 ==="
grep -v "pattern" file.txt
```

**详细解释**：

- `-v`：反向匹配选项（invert match）
- 输出所有不包含指定模式的行
- 用于过滤不需要的内容

```bash
# 精确匹配整个单词 - 只匹配完整的单词"line"
echo -e "\n=== 精确匹配整个单词 ==="
grep -w "line" file.txt
```

**详细解释**：
- `-w`：单词边界匹配选项
- 只匹配完整的"line"单词，不匹配"line"作为其他单词的一部分（如"inline"）
- 使用单词边界`\b`实现

  

```bash
# 递归搜索目录 - 在当前目录及子目录中搜索
echo -e "\n=== 递归搜索目录 ==="
mkdir -p testdir
echo "pattern in subdirectory" > testdir/subfile.txt
grep -r "pattern" ./
```

**详细解释**：

- `mkdir -p testdir`：创建测试目录
- `echo "..." > testdir/subfile.txt`：在子目录创建测试文件
- `-r`：递归搜索选项
- 在当前目录（./）及其所有子目录中搜索
- 输出格式包含文件路径

  

```bash
# 只显示文件名 - 显示包含匹配内容的文件名
echo -e "\n=== 只显示文件名 ==="
grep -l "pattern" *.txt
```

**详细解释**：
- `-l`：只显示文件名选项
- 不输出匹配内容，只列出包含匹配项的文件名
- 通配符`*.txt`匹配所有txt文件

  

### 1.3.2 高级选项

```bash
# 显示匹配行的前后几行
echo "=== 显示匹配行前后3行 ==="
grep -C 3 "pattern" file.txt
```

**详细解释**：
- `-C 3`：上下文选项，显示匹配行前后各3行
- 类似`-A 3 -B 3`的组合
- 用于查看匹配内容的上下文环境

  

```bash
# 静默模式 - 只返回退出状态，不显示输出
echo -e "\n=== 静默模式 ==="
grep -q "pattern" file.txt && echo "Pattern found" || echo "Pattern not found"
```

**详细解释**：
- `-q`：静默模式（quiet）
- 不输出任何内容，只通过退出状态码表示是否找到
- `&&`：前一个命令成功（退出码0）则执行后一个
- `||`：前一个命令失败（退出码非0）则执行后一个
- 用于脚本中的条件判断

  

```bash
# 显示匹配部分并高亮
echo -e "\n=== 高亮显示匹配 ==="
grep --color=always "pattern" file.txt
```

**详细解释**：
- `--color=always`：始终高亮匹配部分
- 匹配的文本会以不同颜色显示（通常是红色）
- `always`确保即使输出重定向也保持颜色

  

### 1.4.1 基本正则表达式（BRE）

```bash
# 行首匹配 - 查找以"This"开头的行
echo "=== 行首匹配 ==="
grep "^This" file.txt
```

**详细解释**：
- `^`：行首锚点
- 匹配以"This"开头的行
- 不匹配行中间的"This"

  

```bash
# 行尾匹配 - 查找以"file."结尾的行
echo -e "\n=== 行尾匹配 ==="
grep "file\.$" file.txt
```

**详细解释**：

- `\.`：转义点号，匹配字面意义的"."（否则.匹配任意字符）
- `$`：行尾锚点
- 匹配以"file."结尾的行

  

```bash
# 任意字符 - 查找以"T"开头，第二个字符任意，第三个字符是"h"的模式
echo -e "\n=== 任意字符 ==="
grep "T.h" file.txt
```

**详细解释**：
- `.`：匹配任意单个字符（换行符除外）
- 匹配"Thh"、"Tah"、"T h"等
- 三个字符的模式：T + 任意字符 + h

  

```bash
# 重复字符 - 查找包含0个或多个"a"的行
echo -e "\n=== 重复字符 ==="
grep "a*" file.txt
```

**详细解释**：
- `*`：匹配前面的字符0次或多次
- "a*"匹配空字符串或连续的a
- 几乎所有行都会匹配，因为*可以匹配0个a

  

```bash
# 字符集 - 查找包含a、b或c的行
echo -e "\n=== 字符集 ==="
grep "[abc]" file.txt
```

**详细解释**：
- `[abc]`：字符集合，匹配其中任意一个字符
- 匹配包含a或b或c的行
- 方括号内的字符是"或"关系

  

```bash
# 字符范围 - 查找包含小写字母的行
echo -e "\n=== 字符范围 ==="
grep "[a-z]" file.txt
```

**详细解释**：
- `[a-z]`：字符范围，匹配a到z之间的任意小写字母
- 等价于[abcdefghijklmnopqrstuvwxyz]
- 匹配包含任何小写字母的行

  

```bash
# 排除字符集 - 查找不包含a、b、c的行
echo -e "\n=== 排除字符集 ==="
grep "[^abc]" file.txt
```

**详细解释**：
- `[^abc]`：排除字符集，匹配不包含a、b、c的任意字符
- 方括号内以^开头表示否定
- 匹配包含除a、b、c外其他字符的行




```bash
# 查找空行
echo -e "\n=== 查找空行 ==="
grep -n "^$" file.txt
```

**详细解释**：

- `^$`：行首和行尾之间无任何字符

- 匹配完全空白的行

- 用于查找空行

  

```bash
# 查找包含数字的行
echo -e "\n=== 查找包含数字的行 ==="
grep "[0-9]" file.txt
```

**详细解释**：

- `[0-9]`：匹配任意数字
- 查找包含至少一个数字的行
- 等同于`[[:digit:]]`



### 1.4.2 扩展正则表达式（ERE）

```bash
# 使用egrep或grep -E
echo "=== 扩展正则表达式 - 一个或多个a ==="
egrep "a+" file.txt
```

**详细解释**：
- `egrep`：使用扩展正则表达式
- `a+`：匹配一个或多个连续的a
- "+"表示前面字符至少出现一次
- 等同于`grep -E "a+" file.txt`

  

```bash
echo -e "\n=== 扩展正则表达式 - 2到5个a ==="
egrep "a{2,5}" file.txt
```

**详细解释**：

- `a{2,5}`：匹配连续出现2到5次的a
- `{m,n}`表示前面字符出现m到n次
- 区别于基本正则表达式，扩展正则中{}不需要转义

  

```bash
echo -e "\n=== 扩展正则表达式 - a或b ==="
egrep "a|b" file.txt
```

**详细解释**：
- `a|b`：或操作符，匹配a或b
- "|"表示选择关系
- 匹配包含a或b的行

  

```bash
echo -e "\n=== 扩展正则表达式 - ab组合 ==="
egrep "(ab)+" file.txt
```

**详细解释**：
- `(ab)+`：匹配一个或多个连续的"ab"组合
- `()`用于分组
- "+"作用于整个分组
- 匹配"ab"、"abab"等




```bash
# 查找IP地址
echo -e "\n=== 查找IP地址 ==="
grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" access.log
```

**详细解释**：

- `-E`：使用扩展正则表达式
- `[0-9]{1,3}`：匹配1到3位数字
- `\.`：匹配字面意义的点号
- 简单的IP地址模式，实际可能匹配不完全准确



```bash
# 查找邮箱地址（模拟）
echo -e "\n=== 查找邮箱地址 ==="
echo "Contact: john@example.com, mary@test.org" | grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}"
```

**详细解释**：

- `echo "..." |`：将字符串通过管道传递给grep
- `[a-zA-Z0-9._%+-]+`：用户名部分，匹配字母、数字和特殊字符
- `@`：字面匹配@符号
- `[a-zA-Z0-9.-]+`：域名部分
- `\.`：字面匹配点号
- `[a-zA-Z]{2,}`：顶级域名，至少2个字母
- 简单的邮箱格式验证




### 1.5.1 系统状态监控脚本

```bash
# 创建监控日志文件
cat > system_monitor.log << 'EOF'
2023-10-01 08:00:01 INFO System started successfully
2023-10-01 08:15:23 WARNING High memory usage detected
2023-10-01 08:30:45 ERROR Database connection failed
2023-10-01 09:00:12 CRITICAL System overload - CPU 95%
2023-10-01 09:15:33 INFO Database reconnected
2023-10-01 09:30:56 WARNING Disk space low - 85% used
2023-10-01 10:00:21 ERROR Network timeout
2023-10-01 10:15:44 CRITICAL Memory exhausted - 98% used
2023-10-01 10:30:07 WARNING High CPU usage - 80%
2023-10-01 11:00:33 INFO System stable
EOF

# 监控脚本：分析系统日志并生成报告
echo "=== 系统监控分析脚本 ==="
cat > monitor_system.sh << 'EOF'
#!/bin/bash

LOG_FILE="system_monitor.log"

# 检查日志文件是否存在
if [ ! -f "$LOG_FILE" ]; then
    echo "错误：日志文件 $LOG_FILE 不存在"
    exit 1
fi

echo "=== 系统监控报告 ==="
echo "生成时间：$(date)"
echo "----------------------------------------"

# 统计各类日志条目数量
echo "日志级别统计："
ERROR_COUNT=$(grep -c "ERROR" "$LOG_FILE")
WARNING_COUNT=$(grep -c "WARNING" "$LOG_FILE")
CRITICAL_COUNT=$(grep -c "CRITICAL" "$LOG_FILE")
INFO_COUNT=$(grep -c "INFO" "$LOG_FILE")

echo "  错误(ERROR)：$ERROR_COUNT 条"
echo "  警告(WARNING)：$WARNING_COUNT 条"
echo "  严重(CRITICAL)：$CRITICAL_COUNT 条"
echo "  信息(INFO)：$INFO_COUNT 条"

# 检查是否存在严重问题
echo -e "\n严重问题检查："
if [ $CRITICAL_COUNT -gt 0 ]; then
    echo "⚠️  发现严重问题："
    grep "CRITICAL" "$LOG_FILE" | while read line; do
        echo "  $line"
    done
else
    echo "✅ 无严重问题"
fi

# 检查资源使用警告
echo -e "\n资源使用警告："
WARNING_LINES=$(grep "WARNING" "$LOG_FILE" | grep -c "usage\|space")
if [ $WARNING_LINES -gt 0 ]; then
    echo "⚠️  发现资源使用警告："
    grep "WARNING" "$LOG_FILE" | grep "usage\|space" | while read line; do
        echo "  $line"
    done
else
    echo "✅ 资源使用正常"
fi

# 检查错误类型并给出建议
echo -e "\n错误分析："
if [ $ERROR_COUNT -gt 0 ]; then
    echo "发现以下错误："
    # 循环处理每种错误类型
    ERROR_TYPES=("Database connection failed" "Network timeout")
    
    for error_type in "${ERROR_TYPES[@]}"; do
        count=$(grep -c "$error_type" "$LOG_FILE")
        if [ $count -gt 0 ]; then
            echo "  $error_type: $count 次"
            # 根据错误类型给出建议
            case "$error_type" in
                "Database connection failed")
                    echo "    建议：检查数据库服务状态和网络连接"
                    ;;
                "Network timeout")
                    echo "    建议：检查网络配置和防火墙设置"
                    ;;
            esac
        fi
    done
fi

# 计算问题严重程度
TOTAL_ISSUES=$((ERROR_COUNT + WARNING_COUNT * 2 + CRITICAL_COUNT * 3))
echo -e "\n问题严重程度评分：$TOTAL_ISSUES"

# 根据评分给出总体建议
if [ $TOTAL_ISSUES -eq 0 ]; then
    echo "✅ 系统运行良好"
elif [ $TOTAL_ISSUES -lt 5 ]; then
    echo "⚠️ 系统存在轻微问题，建议关注"
elif [ $TOTAL_ISSUES -lt 10 ]; then
    echo "⚠️ 系统存在中等问题，建议检查"
else
    echo "❌ 系统存在严重问题，需要立即处理"
fi
EOF

chmod +x monitor_system.sh
./monitor_system.sh
```



### 1.5.2  网络安全日志分析

```bash
# 创建网络安全日志文件
cat > security.log << 'EOF'
Oct  1 08:00:01 server sshd[1234]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:00:15 server sshd[1235]: Failed password for admin from 192.168.1.101 port 22 ssh2
Oct  1 08:00:30 server sshd[1236]: Failed password for user from 192.168.1.102 port 22 ssh2
Oct  1 08:01:01 server sshd[1237]: Accepted password for john from 192.168.1.50 port 22 ssh2
Oct  1 08:01:15 server sshd[1238]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:01:30 server sshd[1239]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:02:01 server sshd[1240]: Failed password for root from 192.168.1.103 port 22 ssh2
Oct  1 08:02:15 server sshd[1241]: Accepted password for mary from 192.168.1.51 port 22 ssh2
Oct  1 08:02:30 server sshd[1242]: Failed password for admin from 192.168.1.104 port 22 ssh2
Oct  1 08:03:01 server sshd[1243]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:03:15 server sshd[1244]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:03:30 server sshd[1245]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:04:01 server sshd[1246]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:04:15 server sshd[1247]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:04:30 server sshd[1248]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:05:01 server sshd[1249]: Failed password for root from 192.168.1.100 port 22 ssh2
Oct  1 08:05:15 server sshd[1250]: Failed password for root from 192.168.1.105 port 22 ssh2
Oct  1 08:05:30 server sshd[1251]: Failed password for root from 192.168.1.100 port 22 ssh2
EOF

# 安全日志分析脚本
echo -e "\n=== 网络安全日志分析脚本 ==="
cat > analyze_security.sh << 'EOF'
#!/bin/bash

SECURITY_LOG="security.log"

# 检查日志文件
if [ ! -f "$SECURITY_LOG" ]; then
    echo "错误：安全日志文件 $SECURITY_LOG 不存在"
    exit 1
fi

echo "=== 网络安全分析报告 ==="
echo "分析时间：$(date)"
echo "----------------------------------------"

# 统计失败登录尝试
FAILED_ATTEMPTS=$(grep -c "Failed password" "$SECURITY_LOG")
SUCCESSFUL_LOGINS=$(grep -c "Accepted password" "$SECURITY_LOG")

echo "登录统计："
echo "  失败登录尝试：$FAILED_ATTEMPTS 次"
echo "  成功登录：$SUCCESSFUL_LOGINS 次"

# 提取攻击IP地址
echo -e "\n攻击源IP分析："
ATTACK_IPS=$(grep "Failed password" "$SECURITY_LOG" | grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" | sort | uniq -c | sort -nr)

if [ -n "$ATTACK_IPS" ]; then
    echo "$ATTACK_IPS" | while read count ip; do
        # 条件判断：标记高风险IP
        if [ $count -ge 5 ]; then
            RISK_LEVEL="🔴 高风险"
        elif [ $count -ge 3 ]; then
            RISK_LEVEL="🟡 中风险"
        else
            RISK_LEVEL="🟢 低风险"
        fi
        echo "  $ip: $count 次尝试 ($RISK_LEVEL)"
        
        # 对高风险IP进行详细分析
        if [ $count -ge 5 ]; then
            echo "    详细信息："
            grep "$ip" "$SECURITY_LOG" | head -3 | while read line; do
                echo "      $line"
            done
            echo "    建议：考虑在防火墙中阻止此IP"
        fi
    done
else
    echo "  未发现攻击尝试"
fi

# 分析目标用户
echo -e "\n攻击目标用户分析："
TARGET_USERS=$(grep "Failed password" "$SECURITY_LOG" | grep -oE "for [a-zA-Z0-9]+" | cut -d' ' -f2 | sort | uniq -c | sort -nr)

if [ -n "$TARGET_USERS" ]; then
    echo "$TARGET_USERS" | while read count user; do
        # 条件判断：标记高风险用户
        if [ $count -ge 5 ]; then
            RISK_STATUS="⚠️ 高频攻击目标"
        elif [ $count -ge 3 ]; then
            RISK_STATUS="⚠️ 中频攻击目标"
        else
            RISK_STATUS="ℹ️ 低频攻击目标"
        fi
        echo "  $user: $count 次尝试 ($RISK_STATUS)"
    done
else
    echo "  未发现针对特定用户的攻击"
fi

# 时间模式分析
echo -e "\n攻击时间模式分析："
HOUR_PATTERN=$(grep "Failed password" "$SECURITY_LOG" | cut -d: -f1 | rev | cut -d' ' -f1 | rev | sort | uniq -c)

echo "按小时统计攻击频率："
echo "$HOUR_PATTERN" | while read count hour; do
    # 条件判断：标记攻击高峰时段
    if [ $count -ge 5 ]; then
        ACTIVITY_LEVEL="🔴 攻击高峰"
    elif [ $count -ge 3 ]; then
        ACTIVITY_LEVEL="🟡 攻击活跃"
    else
        ACTIVITY_LEVEL="🟢 攻击较少"
    fi
    echo "  $hour点: $count 次 ($ACTIVITY_LEVEL)"
done

# 安全建议生成
echo -e "\n安全建议："

# 检查是否需要加强安全措施
HIGH_RISK_IPS=$(echo "$ATTACK_IPS" | awk '$1 >= 5 {print $2}' | wc -l)
if [ $HIGH_RISK_IPS -gt 0 ]; then
    echo "  1. 立即阻止高风险IP地址"
    echo "  2. 考虑启用fail2ban等防护工具"
fi

ROOT_ATTACKS=$(grep "Failed password for root" "$SECURITY_LOG" | wc -l)
if [ $ROOT_ATTACKS -gt 0 ]; then
    echo "  3. 禁用root用户直接SSH登录"
    echo "  4. 使用sudo替代root权限"
fi

FREQUENT_TARGETS=$(echo "$TARGET_USERS" | awk '$1 >= 5 {print $2}' | wc -l)
if [ $FREQUENT_TARGETS -gt 0 ]; then
    echo "  5. 为高频攻击目标用户加强密码策略"
fi

# 总体风险评估
TOTAL_ATTACKS=$FAILED_ATTEMPTS
if [ $TOTAL_ATTACKS -eq 0 ]; then
    echo "  ✅ 系统安全状况良好"
elif [ $TOTAL_ATTACKS -lt 10 ]; then
    echo "  ⚠️ 系统面临轻微安全威胁"
elif [ $TOTAL_ATTACKS -lt 20 ]; then
    echo "  ⚠️ 系统面临中等安全威胁"
else
    echo "  ❌ 系统面临严重安全威胁"
fi

echo -e "\n自动化防护建议："
echo "  可以创建以下cron任务定期运行此脚本："
echo "  0 */6 * * * /path/to/analyze_security.sh >> /var/log/security_report.log"
EOF

chmod +x analyze_security.sh
./analyze_security.sh
```



## 第二部分：sed - 流编辑器

### 2.4.1 替换命令（s）

```bash
# 基本替换 - 只替换每行第一个匹配项
echo "=== 基本替换 ==="
sed 's/pattern/REPLACEMENT/' file.txt
```

**详细解释**：
- `sed`：流编辑器命令
- `'s/pattern/REPLACEMENT/'`：替换命令
- `s`：替换操作符
- 第一个`/`后是搜索模式"pattern"
- 第二个`/`后是替换内容"REPLACEMENT"
- 最后一个`/`结束命令
- 默认只替换每行第一个匹配项

  

```bash
# 全局替换 - 替换每行所有匹配项
echo -e "\n=== 全局替换 ==="
sed 's/line/LINE/g' file.txt
```

**详细解释**：
- `g`：全局标志，替换每行中所有匹配项
- 不带g只替换第一个匹配
- 将"line"全部替换为"LINE"

  

```bash
# 只替换第几个匹配项
echo -e "\n=== 替换第二个匹配项 ==="
sed 's/line/LINE/2' multiline.txt
```

**详细解释**：
- `/2`：只替换第二个匹配项
- 数字标志指定替换第几个匹配
- 每行独立计算匹配次数

  

```bash
# 忽略大小写替换
echo -e "\n=== 忽略大小写替换 ==="
sed 's/pattern/REPLACEMENT/gi' file.txt
```

**详细解释**：
- `i`：忽略大小写标志
- `gi`：全局且忽略大小写
- 匹配"pattern"、"Pattern"等所有大小写形式

  

```bash
# 使用不同分隔符
echo -e "\n=== 使用不同分隔符 ==="
sed 's|/bin/bash|/bin/sh|g' passwd
```

**详细解释**：
- 使用`|`代替`/`作为分隔符
- 当模式或替换内容包含/时，避免转义
- `/bin/bash`替换为`/bin/sh`
- 常用于路径替换

  

```bash
# 替换并保存到文件（演示，但不实际修改原文件）
echo -e "\n=== 替换并显示（不修改原文件） ==="
sed 's/pattern/REPLACEMENT/g' file.txt
```

**详细解释**：
- sed默认输出到stdout，不修改原文件
- 这是安全的演示方式
- 如需修改文件，使用`-i`选项

  

### 2.4.2 删除命令（d）

```bash
# 删除第3行
echo "=== 删除第3行 ==="
sed '3d' file.txt
```

**详细解释**：
- `3`：地址，指定第3行
- `d`：删除命令
- 删除第3行，输出其他所有行

  

```bash
# 删除第3到第5行
echo -e "\n=== 删除第3到第5行 ==="
sed '3,5d' file.txt
```

**详细解释**：

- `3,5`：地址范围，第3行到第5行
- `d`：删除这些行
- 范围用逗号分隔

  

```bash
# 删除最后一行
echo -e "\n=== 删除最后一行 ==="
sed '$d' file.txt
```

**详细解释**：
- `$`：表示最后一行
- `d`：删除最后一行
- `$`是行地址的特殊符号

  

```bash
# 删除包含特定模式的行
echo -e "\n=== 删除包含'pattern'的行 ==="
sed '/pattern/d' file.txt
```

**详细解释**：
- `/pattern/`：模式地址，匹配包含"pattern"的行
- `d`：删除这些行
- 模式用斜杠包围

  

```bash
# 删除空行
echo -e "\n=== 删除空行 ==="
sed '/^$/d' file.txt
```

**详细解释**：
- `/^$/`：匹配空行的正则表达式
- `^`行首，`$`行尾，中间无内容
- `d`：删除所有空行

  

```bash
# 删除从匹配行到文件末尾的所有行
echo -e "\n=== 删除从包含'last'的行到文件末尾 ==="
sed '/last/,$d' file.txt
```

**详细解释**：
- `/last/,$`：从包含"last"的行到文件末尾($)
- `d`：删除这个范围内的所有行
- 模式到行尾的范围

  

### 2.4.3 插入命令（i、a、c）

```bash
# 在第2行前插入文本
echo "=== 在第2行前插入文本 ==="
sed '2i\This is inserted line' file.txt
```

**详细解释**：
- `2`：地址，第2行
- `i`：insert，在指定行前插入
- `\`：续行符，后跟要插入的文本
- 插入文本在新行

  

```bash
# 在第2行后追加文本
echo -e "\n=== 在第2行后追加文本 ==="
sed '2a\This is appended line' file.txt
```

**详细解释**：
- `a`：append，在指定行后追加
- 与`i`相反，`a`在行后添加
- 文本在新行

  

```bash
# 替换第2行内容
echo -e "\n=== 替换第2行内容 ==="
sed '2c\This replaces line 2' file.txt
```

**详细解释**：
- `c`：change，替换整行内容
- 整个第2行被替换为指定文本
- 不是插入或追加

  

```bash
# 在匹配行前插入
echo -e "\n=== 在包含'pattern'的行前插入 ==="
sed '/pattern/i\--- INSERTED BEFORE PATTERN ---' file.txt
```

**详细解释**：
- `/pattern/`：模式地址，匹配包含"pattern"的行
- `i`：在匹配行前插入
- 插入分隔线用于标识

  

### 2.4.4 打印命令（p）

```bash
# 打印第3行
echo "=== 打印第3行 ==="
sed -n '3p' file.txt
```

**详细解释**：
- `-n`：抑制默认输出
- `3p`：只打印第3行
- 没有`-n`会打印所有行，匹配的行重复

  

```bash
# 打印第3到第5行
echo -e "\n=== 打印第3到第5行 ==="
sed -n '3,5p' file.txt
```

**详细解释**：
- `3,5p`：打印第3到第5行
- `-n`配合`p`实现范围选择
- 类似`head`和`tail`的组合

  

```bash
# 打印匹配行
echo -e "\n=== 打印包含'pattern'的行 ==="
sed -n '/pattern/p' file.txt
```

**详细解释**：
- `/pattern/p`：打印包含"pattern"的行
- `-n`抑制其他行输出
- 实现类似grep的功能

  

```bash
# 打印包含数字的行
echo -e "\n=== 打印包含数字的行 ==="
sed -n '/[0-9]/p' file.txt
```

**详细解释**：
- `/[0-9]/`：正则表达式匹配数字
- `p`：打印匹配行
- `-n`：只输出匹配行

  

### 2.5.1 行号范围

```bash
# 单行替换
echo "=== 单行替换（第5行）==="
sed '5s/line/LINE/' file.txt
```

**详细解释**：
- `5`：地址，只对第5行操作
- `s/line/LINE/`：替换命令
- 仅在第5行进行替换

  

```bash
# 范围替换
echo -e "\n=== 范围替换（第3到第7行）==="
sed '3,7s/line/LINE/g' file.txt
```

**详细解释**：
- `3,7`：地址范围，第3到第7行
- `s/line/LINE/g`：全局替换
- 只在指定范围内执行替换

  

```bash
# 从某行到文件末尾
echo -e "\n=== 从第5行到末尾替换 ==="
sed '5,$s/line/LINE/g' file.txt
```

**详细解释**：
- `5,$`：从第5行到文件末尾
- `$`表示最后一行
- 在大范围进行替换

  

### 2.5.2 模式范围

```bash
# 从第一个匹配到第二个匹配
echo "=== 从第一个'first'到第一个'last'之间替换 ==="
sed '/first/,/last/s/line/LINE/g' multiline.txt
```

**详细解释**：
- `/first/,/last/`：模式范围，从包含"first"的行到包含"last"的行
- `s/line/LINE/g`：在范围内全局替换
- 范围是动态的，基于内容

  

### 2.6.1 文本处理

```bash
# 删除HTML标签
echo "=== 删除HTML标签 ==="
sed 's/<[^>]*>//g' sample.html
```

**详细解释**：
- `<[^>]*>`：匹配HTML标签
- `<`：字面匹配
- `[^>]*`：非>字符的任意长度
- `>`：字面匹配
- `g`：全局替换为空
- 提取HTML中的纯文本

  

```bash
# 合并多行到一行
echo -e "\n=== 合并多行 ==="
sed ':a;N;$!ba;s/\n/ /g' file.txt | head -1
```

**详细解释**：
- `:a`：定义标签a
- `N`：读取下一行到模式空间
- `$!ba`：如果不是最后一行，跳转到a
- `s/\n/ /g`：将所有换行符替换为空格
- 将整个文件合并为一行

  

```bash
# 给文件添加行号
echo -e "\n=== 添加行号 ==="
sed '=' file.txt | sed 'N; s/\n/ /'
```

**详细解释**：
- `=`：打印当前行号
- `sed '='`输出行号和内容分行
- `| sed 'N; s/\n/ /'`：读取两行，用空格替换换行
- 实现类似`nl`命令的功能

  

```bash
# 将每行的第一个单词转为大写
echo -e "\n=== 首单词转大写 ==="
sed 's/^\([a-z]\)\([a-zA-Z]*\)/\U\1\E\2/' file.txt
```

**详细解释**：

- `^\([a-z]\)`：捕获第一个小写字母
- `\([a-zA-Z]*\)`：捕获剩余字母
- `\U\1`：将第一个捕获组转为大写
- `\E`：结束大写转换
- `\2`：引用第二个捕获组
- 使用sed的大小写转换功能

  

### 2.6.2 配置文件处理

```bash
# 注释掉配置项
echo "=== 注释掉配置项 ==="
sed 's/^\(app\.debug=\)/#\1/' config.txt
```

**详细解释**：
- `^\(app\.debug=\)`：捕获以"app.debug="开头的行
- `\.`：转义点号
- `#\1`：替换为#加上捕获的内容
- 实现注释功能

  

```bash
# 取消注释
echo -e "\n=== 取消注释 ==="
sed 's/^#\+\(.*app\.maintenance.*\)/\1/' config.txt
```

**详细解释**：
- `^#\+`：行首一个或多个#
- `\(.*app\.maintenance.*\)`：捕获包含app.maintenance的内容
- `\1`：只保留捕获组内容
- 移除注释符号

  

```bash
# 修改配置值
echo -e "\n=== 修改配置值 ==="
sed '/^app\.debug=/s/=.*/=false/' config.txt
```

**详细解释**：
- `/^app\.debug=/`：选择以app.debug=开头的行
- `s/=.*/=false/`：将=后所有内容替换为=false
- 先选择行，再执行替换
- 精确修改配置值

  

## 第三部分：awk - 文本分析神器

### 3.3.1 记录和字段

```bash
# 打印所有记录
echo "=== 打印所有记录 ==="
awk '{print}' data.txt
```

**详细解释**：
- `awk`：文本分析工具
- `{print}`：动作块，打印当前记录
- 默认打印整行内容
- 等同于`{print $0}`

  

```bash
# 打印第一字段
echo -e "\n=== 打印第一字段 ==="
awk '{print $1}' data.txt
```

**详细解释**：
- `$1`：第一个字段
- 默认以空白字符（空格、制表符）分隔
- 输出每行的第一个字段

  

```bash
# 打印第一和第三字段
echo -e "\n=== 打印第一和第三字段 ==="
awk '{print $1, $3}' data.txt
```

**详细解释**：
- `$1, $3`：打印第一和第三字段
- 字段间自动用OFS（输出字段分隔符，默认空格）分隔
- 多字段输出

  

```bash
# 打印所有字段
echo -e "\n=== 打印所有字段 ==="
awk '{print $0}' data.txt
```

**详细解释**：
- `$0`：整个记录（整行）
- 与`{print}`效果相同
- 显式引用整行

  

```bash
# 打印字段数量
echo -e "\n=== 打印字段数量 ==="
awk '{print NF}' data.txt
```

**详细解释**：
- `NF`：Number of Fields，当前记录的字段数
- 对每行输出其字段数量
- data.txt中每行都是3个字段

  

```bash
# 打印记录号
echo -e "\n=== 打印记录号 ==="
awk '{print NR}' data.txt
```

**详细解释**：
- `NR`：Number of Records，记录号（行号）
- 从1开始递增
- 全局行号，跨文件累计

  

### 3.3.2 特殊变量

```bash
# 设置字段分隔符为冒号
echo "=== 使用冒号分隔符 ==="
awk -F: '{print $1, $7}' passwd
```

**详细解释**：
- `-F:`：设置输入字段分隔符为冒号
- `$1`：用户名
- `$7`：shell
- 处理passwd文件格式

  

```bash
# 设置多个分隔符（逗号和空格）
echo -e "\n=== 使用多个分隔符 ==="
echo "name,age job,salary" | awk -F'[ ,]' '{print $1, $2, $3, $4}'
```

**详细解释**：
- `-F'[ ,]'`：使用正则表达式作为分隔符，逗号或空格
- `[ ,]`：字符集合，匹配逗号或空格
- 将混合分隔符的文本正确分割

  

```bash
# 查看特殊变量的值
echo -e "\n=== 查看特殊变量 ==="
awk '{print "NF=" NF ", NR=" NR ", FILENAME=" FILENAME}' data.txt | head -3
```

**详细解释**：
- `NF`：当前行字段数
- `NR`：当前记录号
- `FILENAME`：当前处理的文件名
- 字符串拼接输出

  

### 3.4.1 正则表达式模式

```bash
# 匹配包含特定模式的行
echo "=== 匹配包含'Engineer'的行 ==="
awk '/Engineer/ {print}' data.txt
```

**详细解释**：
- `/Engineer/`：正则表达式模式
- 只处理包含"Engineer"的行
- 模式匹配作为条件

  

```bash
# 匹配字段内容
echo -e "\n=== 匹配第一字段包含'J'的行 ==="
awk '$1 ~ /^J/ {print}' data.txt
```

**详细解释**：
- `$1 ~ /^J/`：第一字段匹配以J开头的正则
- `~`：匹配操作符
- `!~`：不匹配操作符
- 字段级别的正则匹配

  

```bash
# 不匹配
echo -e "\n=== 不匹配包含'Designer'的行 ==="
awk '$3 !~ /Designer/ {print}' data.txt
```

**详细解释**：
- `$3 !~ /Designer/`：第三字段不包含"Designer"
- `!~`：不匹配操作符
- 排除特定内容

  

```bash
# 行号范围
echo -e "\n=== 处理第3到第5行 ==="
awk 'NR>=3 && NR<=5 {print}' data.txt
```

**详细解释**：
- `NR>=3 && NR<=5`：行号在3到5之间
- `&&`：逻辑与
- 基于行号的范围选择

  

```bash
# 字段数量条件
echo -e "\n=== 字段数量等于3的行 ==="
awk 'NF==3 {print}' data.txt
```

**详细解释**：
- `NF==3`：字段数等于3的行
- `==`：等于比较
- 基于结构的筛选

  

### 3.4.2 关系表达式

```bash
# 数值比较
echo "=== 年龄大于30的员工 ==="
awk '$2 > 30 {print}' data.txt
```

**详细解释**：
- `$2 > 30`：第二字段（年龄）大于30
- awk自动识别数字进行数值比较
- 不是字符串比较

  

```bash
# 字符串比较
echo -e "\n=== 职位是'Engineer'的员工 ==="
awk '$3 == "Engineer" {print}' data.txt
```

**详细解释**：
- `$3 == "Engineer"`：第三字段等于"Engineer"
- `==`：字符串相等比较
- 区分大小写

  

```bash
# 复合条件
echo -e "\n=== 年龄大于30且职位包含'Engineer'的员工 ==="
awk '$2 > 30 && $3 ~ /Engineer/ {print}' data.txt
```

**详细解释**：
- `$2 > 30`：年龄大于30
- `&&`：逻辑与
- `$3 ~ /Engineer/`：职位包含Engineer
- 多条件组合

  

### 3.5.1 print和printf

```bash
# 基本print
echo "=== 基本print ==="
awk '{print $1, $2}' data.txt
```

**详细解释**：
- `print`：输出字段，自动添加换行
- `$1, $2`：字段列表，用OFS分隔
- 简单的字段选择输出

  

```bash
# 格式化输出printf
echo -e "\n=== 格式化输出 ==="
awk '{printf "%-10s %3d %15s\n", $1, $2, $3}' data.txt
```

**详细解释**：
- `printf`：格式化输出，不自动换行
- `%-10s`：左对齐10字符宽字符串
- `%3d`：右对齐3字符宽整数
- `%15s`：右对齐15字符宽字符串
- `\n`：手动添加换行
- 精确控制输出格式

  

```bash
# printf格式说明符演示
echo -e "\n=== 不同格式输出 ==="
awk 'BEGIN {printf "整数: %d\n浮点数: %.2f\n字符串: %s\n", 42, 3.14159, "Hello"}'
```

**详细解释**：
- `BEGIN`：在处理任何输入前执行
- `%d`：十进制整数
- `%.2f`：保留2位小数的浮点数
- `%s`：字符串
- 演示各种格式说明符

  

### 3.5.2 变量和数组

```bash
# 用户定义变量
echo "=== 计算平均年龄 ==="
awk '{sum += $2; count++} END {print "Average age:", sum/count}' data.txt
```

**详细解释**：
- `sum += $2`：累加年龄
- `count++`：计数器递增
- `END`：所有输入处理后执行
- 计算平均值

  

```bash
# 数组操作
echo -e "\n=== 使用数组统计职位 ==="
awk '{positions[$3]++} END {for (pos in positions) print pos, positions[pos]}' data.txt
```

**详细解释**：
- `positions[$3]++`：以职位为键的关联数组，计数
- `END`：处理完成后
- `for (pos in positions)`：遍历数组
- `print pos, positions[pos]`：输出键值对
- 词频统计

  

```bash
# 数组排序
echo -e "\n=== 排序数组 ==="
awk '{arr[$1] = $2} END {n = asorti(arr, sorted); for (i=1; i<=n; i++) print sorted[i], arr[sorted[i]]}' data.txt
```

**详细解释**：
- `arr[$1] = $2`：姓名为键，年龄为值
- `asorti`：按索引排序，返回排序后的索引数组
- `for`循环输出排序结果
- 数组排序功能

  

### 3.5.3 控制结构

```bash
# if语句
echo "=== 使用if语句分类员工 ==="
awk '{if ($2 > 30) print $1, "Senior"; else print $1, "Junior"}' data.txt
```

**详细解释**：
- `if ($2 > 30)`：条件判断
- `print $1, "Senior"`：条件成立时执行
- `else`：否则执行
- `print $1, "Junior"`：条件不成立时执行
- 条件分支

  

```bash
# for循环
echo -e "\n=== for循环演示 ==="
awk 'BEGIN {for (i=1; i<=5; i++) print "Number:", i}'
```

**详细解释**：
- `BEGIN`：预处理块
- `for (i=1; i<=5; i++)`：C风格for循环
- `print "Number:", i`：循环体
- 生成序列

  

```bash
# while循环
echo -e "\n=== while循环演示 ==="
awk 'BEGIN {i=1; while (i<=3) {print "While loop:", i; i++}}'
```

**详细解释**：
- `while (i<=3)`：当条件成立时循环
- `{...}`：循环体，多条语句
- `i++`：循环变量递增
- while循环结构

  

```bash
# for-in循环
echo -e "\n=== for-in循环处理字段 ==="
awk '{for (i=1; i<=NF; i++) print "Field " i ":", $i}' data.txt | head -6
```

**详细解释**：
- `for (i=1; i<=NF; i++)`：从1到字段数循环
- `$i`：第i个字段
- 动态访问字段

  

### 3.6.1 字符串函数

```bash
# length：字符串长度
echo "=== 字符串长度 ==="
awk '{print $1, "length:", length($1)}' data.txt
```

**详细解释**：
- `length($1)`：计算第一个字段的字符数
- 返回字符串长度

  

```bash
# substr：子字符串
echo -e "\n=== 子字符串 ==="
awk '{print $1, "first 3 chars:", substr($1, 1, 3)}' data.txt
```

**详细解释**：
- `substr($1, 1, 3)`：从第一个字符开始取3个字符
- 提取子字符串

  

```bash
# split：分割字符串
echo -e "\n=== 分割字符串 ==="
awk 'BEGIN {n=split("apple,banana,cherry", arr, ","); for (i=1; i<=n; i++) print arr[i]}'
```

**详细解释**：
- `split("...", arr, ",")`：按逗号分割字符串到数组
- 返回分割的段数
- `for`循环输出数组元素

  

```bash
# toupper/tolower：大小写转换
echo -e "\n=== 大小写转换 ==="
awk '{print toupper($1), tolower($3)}' data.txt
```

**详细解释**：
- `toupper($1)`：转为大写
- `tolower($3)`：转为小写
- 字符串大小写转换

  

### 3.6.2 数值函数

```bash
# int：取整
echo "=== 数值函数演示 ==="
awk 'BEGIN {print "Square root of 16:", sqrt(16); print "Integer division:", int(10/3)}'
```

**详细解释**：
- `sqrt(16)`：计算平方根
- `int(10/3)`：取整，结果为3
- 数值计算函数

  

```bash
# rand：随机数
echo -e "\n=== 随机数 ==="
awk 'BEGIN {srand(); for (i=1; i<=3; i++) print "Random:", rand()}'
```

**详细解释**：
- `srand()`：设置随机数种子
- `rand()`：生成0-1之间的随机数
- `for`循环生成多个随机数

  

### 3.7.1 多文件处理

```bash
# 处理多个文件
echo "=== 处理多个文件 ==="
awk '{print FILENAME, $1}' data.txt file.txt | head -15
```

**详细解释**：
- `FILENAME`：当前处理的文件名
- 同时处理多个文件
- 输出文件名和第一字段

  

```bash
# 文件间比较（创建辅助文件）
echo "John 5000" > salaries.txt
echo "Mary 4500" >> salaries.txt
echo "Bob 6000" >> salaries.txt

echo -e "\n=== 文件间数据合并 ==="
awk 'NR==FNR {salary[$1]=$2; next} {print $1, $2, $3, salary[$1]}' salaries.txt data.txt
```

**详细解释**：
- `NR==FNR`：处理第一个文件时为真
- `salary[$1]=$2`：建立姓名到薪水的映射
- `next`：跳过后续动作，处理下一行
- `{print ...}`：处理第二个文件，输出合并数据
- 经典的多文件处理模式

  

### 3.7.2 BEGIN和END块

```bash
# 初始化和清理
echo "=== BEGIN和END块 ==="
awk 'BEGIN {print "=== 开始处理 ==="} {print $0} END {print "=== 处理完成 ==="}' data.txt | head -5
```

**详细解释**：
- `BEGIN`：处理输入前执行
- `{print $0}`：对每行执行
- `END`：所有输入处理后执行
- 程序结构控制

  

```bash
# 统计示例
echo -e "\n=== 统计信息 ==="
awk 'BEGIN {sum=0; print "Processing employee data..."} {sum += $2; count++} END {print "Total employees:", count; print "Average age:", sum/count}' data.txt
```

**详细解释**：

- `BEGIN`：初始化变量，输出提示
- 主体：累加年龄，计数
- `END`：输出统计结果
- 完整的统计程序

  

### 3.8.1 数据分析

```bash
# 计算平均值
echo "=== 计算员工平均年龄 ==="
awk '{sum += $2; count++} END {print "Average age:", sum/count}' data.txt
```

**详细解释**：
- 累加年龄和计数
- 在END块计算平均值
- 基本统计功能

  

```bash
# 查找最大值
echo -e "\n=== 查找年龄最大的员工 ==="
awk 'BEGIN {max=0} {if ($2 > max) {max=$2; name=$1}} END {print "Oldest employee:", name, max}' data.txt
```

**详细解释**：
- `BEGIN`：初始化最大值
- `{if ...}`：比较并更新最大值和对应姓名
- `END`：输出结果
- 最大值查找

  

```bash
# 统计词频
echo -e "\n=== 统计职位词频 ==="
awk '{freq[$3]++} END {for (word in freq) print word, freq[word]}' data.txt
```

**详细解释**：
- `freq[$3]++`：以职位为键的计数数组
- `END`：遍历数组输出词频
- 频率统计

  

```bash
# 生成报表
echo -e "\n=== 生成员工报表 ==="
awk 'BEGIN {printf "%-10s %5s %15s\n", "Name", "Age", "Position"; print "------------------------------"} {printf "%-10s %5d %15s\n", $1, $2, $3}' data.txt
```

**详细解释**：

- `BEGIN`：输出表头和分隔线
- 主体：格式化输出每行数据
- 生成美观的报表

  

### 3.8.2 日志分析

```bash
# 分析Apache日志
echo "=== 分析访问IP频率 ==="
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

**详细解释**：
- `awk '{print $1}'`：提取IP地址
- `sort`：排序
- `uniq -c`：去重并计数
- `sort -nr`：数值逆序排序
- IP访问频率分析

  

```bash
# 统计HTTP状态码
echo -e "\n=== 统计HTTP状态码 ==="
awk '{print $9}' access.log | sort | uniq -c
```

**详细解释**：
- `$9`：HTTP状态码字段
- 统计各种状态码的出现次数
- 错误率分析

  

```bash
# 分析响应大小
echo -e "\n=== 分析平均响应大小 ==="
awk '{sum += $10; count++} END {print "Average response size:", sum/count}' access.log
```

**详细解释**：
- `$10`：响应大小字段
- 计算平均响应大小
- 性能分析

  

### 3.8.3 CSV数据处理

```bash
# 处理CSV文件（跳过标题行）
echo "=== 处理销售数据 ==="
awk -F',' 'NR>1 {total += $3*$4} END {print "Total sales:", total}' sales.csv
```

**详细解释**：

- `-F','`：设置逗号为字段分隔符
- `NR>1`：跳过第一行（标题）
- `$3*$4`：价格×数量=销售额
- 累加总销售额

  

```bash
# 按类别统计
echo -e "\n=== 按类别统计销售额 ==="
awk -F',' 'NR>1 {sales[$2] += $3*$4} END {for (category in sales) print category, sales[category]}' sales.csv
```

**详细解释**：
- `sales[$2] += $3*$4`：以类别为键的销售额累加
- `END`：输出各类别销售额
- 分组统计

  

```bash
# 查找最高价格产品
echo -e "\n=== 查找最高价格产品 ==="
awk -F',' 'NR>1 {if ($3 > max) {max=$3; product=$1}} END {print "Most expensive:", product, max}' sales.csv
```

**详细解释**：
- `NR>1`：跳过标题行
- 比较价格，记录最高价格和对应产品
- 最大值查找

  

### 3.8.4 系统管理

```bash
# 计算所有挂载点的使用率（排除临时文件系统）
echo "=== 磁盘使用分析 ==="
df -h | awk '!/tmpfs|devtmpfs/ && NR>1 {print $1 ": " $5 " used"}'
```

**详细解释**：

- !/tmpfs|devtmpfs/ 排除 tmpfs 和 devtmpfs（内存文件系统）
- NR>1 跳过标题行
- $5 是 df -h 输出的使用百分比（如 63%）

  

```bash
# 分析内存使用
echo -e "\n=== 内存使用分析 ==="
free | awk '/^Mem:/ {printf "Memory usage: %.2f%%\n", ($2-$4-$7)*100/$2}'
```

**详细解释**：
- 在内存计算中，已用内存 = 总内存$2 - (空闲内存$4 + 可用内存$7)
- `%.2f`：保留2位小数
- 内存使用率计算

  

```bash
# 处理用户信息
echo -e "\n=== 处理普通用户信息 ==="
awk -F: '$3 >= 1000 && $3 < 65534 {print $1, $6}' passwd
```

**详细解释**：

- `-F:`：冒号分隔
- `$3 >= 1000 && $3 < 65534`：普通用户UID范围
- `$1`：用户名，`$6`：主目录
- 系统用户管理

  

## 综合示例

### 管道操作示例

```bash
# 组合使用三个工具
echo "=== 组合使用示例 ==="
echo "=== 查找包含'pattern'的行，转换为大写，然后统计字符数 ==="
grep "pattern" file.txt | sed 's/pattern/PATTERN/g' | awk '{print $0, "Length:", length($0)}'
```

**详细解释**：
- `grep`：筛选包含"pattern"的行
- `sed`：将"pattern"替换为"PATTERN"（大写）
- `awk`：输出原行内容和字符长度
- 三剑客协同工作

  

```bash
# 复杂的数据分析管道
echo -e "\n=== 复杂数据分析管道 ==="
cat sales.csv | grep -v "^Product" | sed 's/,/ /g' | awk '{sum+=$3*$4} END {print "Total revenue:", sum}'
```

**详细解释**：
- `cat`：输出文件内容
- `grep -v "^Product"`：排除以"Product"开头的行（标题）
- `sed 's/,/ /g'`：将逗号替换为空格，便于awk处理
- `awk`：计算总销售额
- 典型的数据处理管道

  

### 实际应用场景

```bash
# 系统监控脚本示例
echo "=== 系统监控脚本示例 ==="
echo '#!/bin/bash' > monitor.sh
echo 'echo "=== Top 5 processes by memory ==="' >> monitor.sh
echo 'ps aux --sort=-%mem | head -6 | awk '\''{print $1, $2, $4, $11}'\''' >> monitor.sh
chmod +x monitor.sh
cat monitor.sh
```

**详细解释**：
- 创建监控脚本monitor.sh
- `ps aux --sort=-%mem`：按内存使用率排序进程
- `head -6`：取前6行（含标题）
- `awk`：提取用户、PID、内存%、命令
- 自动化监控

  

```bash
# 日志分析脚本示例
echo -e "\n=== 日志分析脚本示例 ==="
echo '#!/bin/bash' > analyze_log.sh
echo 'LOG_FILE="access.log"' >> analyze_log.sh
echo 'echo "=== HTTP Status Code Analysis ==="' >> analyze_log.sh
echo 'awk '\''{print $9}'\'' $LOG_FILE | sort | uniq -c | sort -nr' >> analyze_log.sh
chmod +x analyze_log.sh
cat analyze_log.sh
```

**详细解释**：

- 创建日志分析脚本analyze_log.sh
- 定义日志文件变量
- 使用awk分析HTTP状态码分布
- 可重用的分析脚本






西安引领云科技有限公司

2025.08