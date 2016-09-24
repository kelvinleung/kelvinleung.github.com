---
title: Python 学习笔记-02
date: 2016-09-18 22:41:17
tags: python 编辑 数据
---

中秋在家无聊，除了教会老妈用 iPad 以外，还撸了几天代码（当然也睡了几天）。

直播的后台，有一个问题，就是查看所有直播的那个页面，非常不友好。

所有直播以一个不知名的排序方式排列着，正在直播的，有预告的，结束了的，还没有节目的，乱七八糟地排在一起。没有分类没有筛选，当你想知道当前正在直播的有哪些，或者说想把一些还不错的预告放到首页时，只能一页页地翻，现在已经足足40+页了！

虽然我平时不怎么用到这个页面，但运营哥哥已经过来吐槽过好多次了，每次他们想推东西的时候，都要找半天才能弄好。这个问题已经跪过开发好多次了，但由于新迭代在赶进度，没有人手来完善这个后台，于是，就这么一直晾着了……

这不能忍啊！不懂代码的产品不是好运营，果断利用三天小长假自己撸了个工具解决之。

### 页面分析

后台页面很简单，除去一些头头尾尾，主要就是一个表格，而我要的所有数据，都包含在这个表格中：

|项目一|项目二|项目三|项目四
|----|----|----|----|
|值一|值二|值三|值四|

```html
<table>
  <thead>
    <tr>
      <th>项目一</th>
      <th>项目二</th>
      <th>项目三</th>
      <th>项目四</th>
    </tr>
  </thead>
    <tr>
      <td>值一</td>
      <td>值二</td>
      <td>值三</td>
      <td>值四</td>
    </tr>
  <tbody>
  </tbody>
</table>
```

所以整个项目的思路就很简单了，想办法把页面上的表格爬下来，然后进行清理提取出要的数据，最后再根据需要进行展示。

### 爬取页面

爬取页面最简单的方法就是使用之前用过的 `requests` 包，模拟登录后，用循环就能把所有要的页面 GET 回来。假设我们的页面 URL 是： `http://xxx.com/data?pageNum=1`，`pageNum` 是当前的页码，只要提交不同的页码，就能获取所有的页面。

```py
# 模拟登录的部分省略

# 页码
n = 1

# Python 没有 do-while，只能用 while-break 来实现
while True:
    response = request.get(url, params = {'pageNum': n})
    n += 1
    # statement 用来判断是否已经到了最后一页，是就退出循环
    if statement:
        break
```

### 处理数据

爬数据是简单，量处理数据却是个大问题，虽然有一大堆轮子能用，但都比较麻烦，需要自己去解析 html，从各种标签里面把需要的数据抽取出来……

最开始我用的是 **BeautifulSoup**，光是把数据拿出来就已经烦死人了，还要将拿出来的数据格式化后保存起来，那酸爽真是……后来我才知道，原来 **Pandas 是可以直接读 HTML 的**，**Pandas 是可以直接读 HTML 的**，**Pandas 是可以直接读 HTML 的**！！！

```py
import pandas as pd

df = pd.read_html(response.text, flavor = 'html5lib')[0]
```

其中 `flavor` 用于指定解析器，默认是 lxml，可以自行替换。返回值是一个 DataFrame 的列表，对应页面里的表格，这里取第一个。

能把数据取回来，但是存在哪里呢？当然是数据库啦，但由于我还没搞懂 python 的数据库怎么弄，于是我用了个比较挫的方法，通过循环写入文件来保存数据。

最终爬取数据那部分的代码大概是这样的：

```py
import pandas as pd
import requests

# 模拟登录的部分省略

n = 1

# 用 with 来读写文件就不用关来关去了
with open('data.csv', 'w') as f
  while True:
      response = request.get(url, params = {'pageNum': n})
      df = pd.read_html(response.text, flavor = 'html5lib')[0]
      n += 1
      # 用拿回来的表格数据是否为空来判断是否已经是最后一页
      if len(df) > 0:
          f.write(df.to_csv(header = False, index = False))
      else:
          break
```

存好的数据处理起来就比较简单了，添加表头，替换掉空值，去掉没有用的列，整理一下列的顺序什么的就不说了。

### 展示数据

最后就是把处理好的数据进行展示了，最简单的是在 terminal 中直接展示，可以利用 `input()` 来获取不同的展示需求，展示对应的数据，比如显示不同直播状态的数据。

```py
# 定义一个打印函数，根据类型输出不同的结果
def print_data(data, type):
    status = '直播中' if type == 1 else '预告中'
    # 选取满足条件的，返回值是 df
    print_data = data[data['直播状态'] == status]
    # 使用 tabulate 美化输出的表格，有不同的 tablefmt 可以选
    print(tabulate(print_data, headers = data.columns, tablefmt = 'grid'))
```

推荐使用 `tabulate` 这个包，它能将给 pandas 直接打印的表格加上真正的“表格”，显示上更加清晰。

### todo

脚本在我本机上运行没有问题，还挺方便的，然而当我想把写好的脚本给运营他们使用的时候，总是报错。查了许多资料发现还是编码的问题，utf-8、GBK 什么的，试了很多次还是没能解决。

接下来要把这个问题弄好，另外还要把写本地文件的方式，重构为用数据库去储存数据，顺便了解一下基本的数据库的知识。
