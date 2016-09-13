---
title: Python 学习笔记-01
date: 2016-09-11 15:30:01
tags: python 编程 数据
---

>Life is short, use Python.

这几周每天都要到后台看直播的数据，后台的关键数据只有图表，没有原始数据可以导出，有时候想做个分析，只能自己手动将图表上的数据点录入 Excel 后再处理，相当麻烦。提需求去优化一下的话当然最好，但就目前来说优先级又不是那么的高，毕竟手工还是可以处理一下的……

然而如果每天将时间浪费在这上面，又的确是很浪费生命。没什么是一个 Geek 解决不了的，如果有，那就证明不够 Geek。于是果断利用上下班路上的时间（那来回的三个小时终于不用浪费了！），恶补了一下 Python，没想到还挺好玩的，真是居家旅行的必备神器。

---

笔记如下：

### 安装 Python

安装的话就太简单了，Mac 本身自带了 Python 2，什么都不用装直接就能用了。当然我还是建议使用 Py3 的，Py2 的中文处理体验实在是太酸爽了……Py3 的话，到官网上下载一个来装就可以了。两个甚至 N 个不同版本的 Python 在一台机上的兼容是完全没有问题的，网上会有很多教程教你如何切换版本。

我直接装了最新的 Python 3.5.2，别的什么都没有动，当我需要跑 Py2 或 Py3 的脚本时，分别输入不同的命令就可以了（反正我直接用 Py3，也就多输入一个“3”而已）：

```shell
python py2-script.py

python3 py3-script.py
```

包的管理也是相互独立的：

```shell
pip install package-name

pip3 install package-name
```

可以考虑安装一个虚拟的环境，利用 virtualenv 包来虚拟不同版本的 Py，并且包也是独立的，不会影响到系统里真正的 Py 包。但是在虚拟环境里用 IPython + matplotlib 来画图的话，图会出不来。网上查了好多资料，应该是 plt 用来绘图的 backend 设置的问题，有兴趣的可以尝试解决一下。我直接在系统里用的 IPython，并没有遇到这样的问题（其实是我试过了解决不了……）。

### IPython

IPython 其实就是一个高级版的 Py shell，尤其是配合 pandas 用来处理数据的话，体验不能更爽了。

安装也是非常简单：

```shell
pip3 install ipython
```

运行的话直接 ipython3 就可以了，当然也可以带上参数，更方便处理数据：

```shell
ipython3 --pylab
```

如果是使用 Py2 的话，把“3”去掉就可以了。

### requests 包——模拟登录并请求数据

工具都准备好了之后就要开工了，打开 Chrome，正常登录后台，通过 Dev-Tools 能看到登录时请求的 URL 及提交的表单数据，假设请求的地址为：`http://login_url`，提交的表单为：

```
usr_name: my_name
pwd: my_password
```

登录后正常查看后台的数据，，比方说要查看9月1日的数据，假设我们的 URL 为：`http://data_url?begin=2016-09-01&end=2016-09-01`

洞悉了一切之后，就可以通过 requests 包写一个获取数据的脚本啦：

```py
# 导入 requests 包
import requests

# 构造一个请求数据的表单，拼装需要提交的字段
query = {
    'begin': begin_date,
    'end': end_date
}

# 开启一个 session，保存登录后的 cookies
request = requests.Session()

def login():
    # 构造登录时提交的表单（用户名+密码）
    login_data = {'usr_name': 'my_name', 'pwd': 'my_password'}
    # 伪造一个 UA
    headers = {
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36',
        'Host': 'host_url',
        'Origin': 'origin_url',
        'Referer': 'referer_url'
    }
    # POST 请求登录
    request.post('login_url', data = login_data, headers = headers)

def get_data(query = query):
    # GET 请求数据
    response = request.get('data_url', params = query)
    # 请求回来的数据是 JSON 格式，将它转成 dict 返回
    return response.json()
```

简直不能更简单了，短短几行代码就完成了模拟登录和请求数据的工作，接下来就可以把请求回来的数据处理并保存下来。

### pandas 包——处理数据

在 IPython 中，可以对数据进行一个预处理，看看拿回来的数据长什么样子，再将处理的命令写成脚本方便以后执行。

拿回来的数据，假设是长下面这个样子的：

```json
{
  'id': [1001,1002,1003,1004],
  'play': [10086,10010,23333,10101],
  'comment': [2048,1024,2468,4399],
  'duration': [120,163,126,60]
}
```



通过对比页面上的数据可以知道，`id`、`play`、`comment`、`duration` 分别是 `节目 ID`、`播放量`、`评论量`、`时长` 的意思（假设有这几个数据），我们就可以利用 pandas 包来简单处理一下这些数据：

```py
# 导入 pandas 包
import pandas as pd
from pandas import DataFrame
# 假设上面模拟登录的脚本保存为相同目录下的 get_data.py，将其导入
import get_data

# 换一个能“看懂”的表头
headers = {
    'id': '节目 ID',
    'play': '播放',
    'comment': '评论',
    'duration': '时长'
}

def clean_data(data):
    # 直接将 dict 转成 DataFrame
    df = DataFrame(data)
    # 换个表头再返回啊
    return df.rename(columns = headers)

if __name__ == '__main__':
    # 模拟登录
    get_data.login()
    # 拿数据，存在 raw_data
    raw_data = get_data.get_data()
    # 通过 clean_data() 处理数据
    live_data = clean_data(raw_data)
    # 保存成 CSV 和 EXCEL 文件
    live_data.to_csv('live_data.csv', index = False, header = True)
    live_data.to_excel('live_data.xlsx', index = False, header = True)
```

将脚本存成 `main.py` 后，我们在 IPython 中运行一下：

```py
run main.py
live_data
```

就能看到处理好的数据啦：

```
        节目 ID         播放        评论        时长
0       1001        10086       2048        120
1       1002        10010       1024        163
2       1003        23333       2468        126
3       1004        10101       4399        60
```

同时也将数据保存在了本地，方便日后使用。

---

先记到这里吧。
