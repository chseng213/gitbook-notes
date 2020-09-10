# Centos

\[TOC\]

## 定时任务`crontab`

### 命令格式

```text
crontab [-u user] file crontab [-u user] [ -e | -l | -r ]
```

> * -u user：用来设定某个用户的crontab服务；
> * file：file是命令文件的名字,表示将file做为crontab的任务列表文件并载入crontab。如果在命令行中没有指定这个文件，crontab命令将接受标准输入（键盘）上键入的命令，并将它们载入crontab。
> * -e：编辑某个用户的crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件。
> * -l：显示某个用户的crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容。
> * -r：从/var/spool/cron目录中删除某个用户的crontab文件，如果不指定用户，则默认删除当前用户的crontab文件。
> * -i：在删除用户的crontab文件时给确认提示。

### `crontab`的文件格式

分 时 日 月 星期 要运行的命令

* 第1列分钟0～59
* 第2列小时0～23（0表示子夜）
* 第3列日1～31
* 第4列月1～12
* 第5列星期0～7（0和7表示星期天）
* 第6列要运行的命令

### 常用命令

```text
# 编辑用户的定时任务
crontab -e
# 显示当前定时任务
crontab -l
```

### 使用实例

#### 实例1：每1分钟执行一次myCommand

```text
* * * * * myCommand
```

#### 实例2：每小时的第3和第15分钟执行

```text
3,15 * * * * myCommand
```

实例3：在上午8点到11点的第3和第15分钟执行

```text
3,15 8-11 * * * myCommand
```

#### 实例4：每隔两天的上午8点到11点的第3和第15分钟执行

```text
3,15 8-11 */2  *  * myCommand
```

#### 实例5：每周一上午8点到11点的第3和第15分钟执行

```text
3,15 8-11 * * 1 myCommand
```

#### 实例6：每晚的21:30重启smb

```text
30 21 * * * /etc/init.d/smb restart
```

#### 实例7：每月1、10、22日的4 : 45重启smb

```text
45 4 1,10,22 * * /etc/init.d/smb restart
```

#### 实例8：每周六、周日的1 : 10重启smb

```text
10 1 * * 6,0 /etc/init.d/smb restart
```

#### 实例9：每天18 : 00至23 : 00之间每隔30分钟重启smb

```text
0,30 18-23 * * * /etc/init.d/smb restart
```

#### 实例10：每星期六的晚上11 : 00 pm重启smb

```text
0 23 * * 6 /etc/init.d/smb restart
```

#### 实例11：每一小时重启smb

```text
* */1 * * * /etc/init.d/smb restart
```

#### 实例12：晚上11点到早上7点之间，每隔一小时重启smb

```text
0 23-7 * * * /etc/init.d/smb restart
```

