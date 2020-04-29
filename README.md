# Minimalist-Web-Notepad-API
 Minimalist Web Notepad Add API

相信大家都会试过寻找地方来进行临时记录，或者是传输一个文本给其它设备或者他人。
最近我在github发现了一个非常不错的项目[Minimalist Web Notepad][1]。
十分的轻量简洁代码仅5KB，用于临时记录与传输文本非常方便，简直是极简主义者必备品！
我在原项目的基础上加入了API接口，日常使用更加方便。

安装教程
--
在index.php文件顶部，更改$base_url变量以指向您的站点。
确保允许Web服务器写入_notes目录。

**在Apache上**

您可能需要启用mod_rewrite并.htaccess在站点配置中设置文件。

**在Nginx上**

要启用URL重写，请将以下内容放入配置文件中：
如果记事本在根目录中：

    location / {
        rewrite ^/([a-zA-Z0-9_-]+)$ /index.php?note=$1;
    }

如果记事本在子目录中：

    location ~* ^/notes/([a-zA-Z0-9_-]+)$ {
        try_files $uri /notes/index.php?note=$1;
    }

API文档
-----
**获取指定笔记文本**

接口地址：/{note}
请求方式：get
请求参数：raw
返回数据：指定笔记的内容(string)
示例参数：/demo?raw

**新建指定地址笔记并写入文本**
**或修改指定地址笔记文本**

接口地址：/{note}
请求方式：get post
请求参数：text
返回数据：saved(string)
示例参数：/demo?text=test

**新建随机地址笔记并添加文本**

接口地址：/?new
请求方式：get post
请求参数：text
返回数据：新建的网址url(string)
示例参数：/?new&text=test

Demo
----
**通过网站进行文本传输**

![5.png][7]

    https://note.166167.xyz


**通过IOS快捷指令进行剪切板同步**
![1.jpg][3]
![2.jpg][4]

**通过Python应用进行Win剪切板同步**

    import win32clipboard as w
    import win32con
    import requests
    
    def copy():
        w.OpenClipboard()
        data = w.GetClipboardData(win32con.CF_UNICODETEXT)
        w.CloseClipboard()
        body = {
            "text": data,
            }
        r = requests.post('https://note.166167.xyz/demo', data = body)
        print(r.status_code,data)
    
    def paste():
        r = requests.get('https://note.166167.xyz/demo?raw')
        data = r.text
        w.OpenClipboard()
        w.SetClipboardData(win32con.CF_UNICODETEXT,data)
        w.CloseClipboard()
        print(r.status_code,data)
    
    def new():
        w.OpenClipboard()
        data = w.GetClipboardData(win32con.CF_UNICODETEXT)
        w.CloseClipboard()
        body = {
            "text": data,
            }
        r = requests.post('https://note.166167.xyz/?new', data = body)
        print(r.status_code,data)


  [1]: https://github.com/pereorga/minimalist-web-notepad
  [3]: https://search.pstatic.net/common?type=origin&src=https://www.mrchung.cn/usr/uploads/2020/04/1156753274.jpg
  [4]: https://search.pstatic.net/common?type=origin&src=https://www.mrchung.cn/usr/uploads/2020/04/2992297654.jpg
  [5]: https://search.pstatic.net/common?type=origin&src=https://www.mrchung.cn/usr/uploads/2020/04/3170686476.png
  [6]: https://search.pstatic.net/common?type=origin&src=https://www.mrchung.cn/usr/uploads/2020/04/1102607478.png
  [7]: https://search.pstatic.net/common?type=origin&src=https://www.mrchung.cn/usr/uploads/2020/04/3535009824.png
