 

本章节主要会去学习在爬虫中的如何去[解析数据](https://so.csdn.net/so/search?q=%E8%A7%A3%E6%9E%90%E6%95%B0%E6%8D%AE&spm=1001.2101.3001.7020)的方法，要学习的内容有：

*   响应数据的分类
*   结构化数据如何提取
*   非结构化数据如何提取
*   正则表达式的语法以及使用
*   jsonpath解析嵌套层次比较复杂的json数据
*   XPath语法
*   在Python代码中借助lxml模块使用XPath语法提取非结构化数据
*   BeautifulSoup模块如何提取非结构化数据

### 数据提取概述

##### 知识点：

*   了解 响应内容的分类
*   了解 xml和html的区别
*   了解 常用的数据解析方法

#### 1\. 响应内容的分类

> 在发送请求获取响应之后，可能存在多种不同类型的响应内容；而且很多时候，我们只需要响应内容中的一部分数据

**结构化的响应内容**

*   json字符串
    
    *   可以使用json、jsonpath等模块来提取特定数据
        
    *   json字符串的例子如下图
        

![](https://i-blog.csdnimg.cn/direct/a924faa3ada54e0e8b73bf5c794d0760.png)

*   xml字符串
    
    *   可以使用re、lxml等模块来提取特定数据
        
    *   xml字符串的例子如下
        
        ```cobol
        <bookstore><book category="COOKING">  <title lang="en">Everyday Italian</title>   <author>Giada De Laurentiis</author>   <year>2005</year>   <price>30.00</price> </book><book category="CHILDREN">  <title lang="en">Harry Potter</title>   <author>J K. Rowling</author>   <year>2005</year>   <price>29.99</price> </book><book category="WEB">  <title lang="en">Learning XML</title>   <author>Erik T. Ray</author>   <year>2003</year>   <price>39.95</price> </book></bookstore>非结构化的响应内容
        ```
        

**非结构化的响应内容**

*   html字符串
    
    *   可以使用re、lxml等模块来提取特定数据
        
    *   html字符串的例子如下图
        

![](https://i-blog.csdnimg.cn/direct/2b8bcabb35fa46068c3c7b99c1945e70.png)

#### 2\. 认识xml以及和html的区别

> 要搞清楚html和xml的区别，首先需要我们来认识xml

##### 2.1 认识xml

> xml是一种可扩展标记语言，样子和html很像，功能更专注于对数据的传输和存储

```cobol
<bookstore><book category="COOKING">  <title lang="en">Everyday Italian</title>   <author>Giada De Laurentiis</author>   <year>2005</year>   <price>30.00</price> </book><book category="CHILDREN">  <title lang="en">Harry Potter</title>   <author>J K. Rowling</author>   <year>2005</year>   <price>29.99</price> </book><book category="WEB">  <title lang="en">Learning XML</title>   <author>Erik T. Ray</author>   <year>2003</year>   <price>39.95</price> </book></bookstore>
```

**上面的xml内容可以表示为下面的树结构:**

![](https://i-blog.csdnimg.cn/direct/b1f8906a6a6441738a6c811174b040d2.png)

##### 2.2 xml和html的区别

> 二者区别如下图

| 数据格式 | 描述 | 设计目标 |
| --- | --- | --- |
| XML | Extensible Markup Language(可扩展标记语言) | 被设计为传输和存储数据，其焦点是数据的内容 |
| HTML | HyperText Markup Language(超文本标记语言) | 显示数据以及更好的显示数据 |

*   html：
    *   超文本标记语言
    *   为了更好的显示数据，侧重点是为了显示
*   xml：
    *   可扩展标记语言
    *   为了传输和存储数据，侧重点是在于数据内容本身

#### 3\. 常用数据解析方法

![](https://i-blog.csdnimg.cn/direct/bc5d173a44f941239e30a36fd9a35407.png)

### 总结：

*   响应数据内容分为：
    *   结构化数据(json、xml)
    *   非结构化数据(html)
*   xml: 可扩展标记语言，主要用于数据的传输和存储。
*   html: 超文本标记语言，主要用于数据的展示。
*   常见的数据解析方法：
    *   结构化数据
        *   json： json模块、jsonpath模块
        *   xml：re正则表达式、lxml模块
    *   非结构化数据
        *   html： re正则表达式、lxml模块、beautifulsoup4

### **正则表达式**

##### **知识点：**

*   了解 什么是正则表达式
*   掌握 正则表达式的使用
*   掌握 在Python中使用正则表达式

#### **1\. 正则表达式介绍**

正则表达式(regular expression)描述了一种字符串匹配的模式，可以用来检查一个串是否含有某种子串、将匹配的子串做替换或者从某个串中取出符合某个条件的子串等。

模式：**一种特定的字符串模式，这个模式是通过一些特殊的符号组成的。**

正则表达式的功能：

*   数据验证（表单验证、如手机、邮箱、IP地址）
    
*   数据检索（数据检索、数据抓取）
    
*   数据隐藏（`135****6235` 王先生）
    
*   数据过滤（论坛敏感关键词过滤）
    

**正则表达式并不是Python所特有的，在Java、PHP、Go以及JavaScript等语言中都是支持正则表达式的。**

#### **2\. 正则表达式语法**

以下正则表达式练习可以先使用在线正则表达式测试工具进行练习：[在线正则表达式测试](https://tool.oschina.net/regex/ "在线正则表达式测试")

##### **2.1 匹配单个字符**

| 正则语法 | 描述 |
| --- | --- |
| . | 匹配任意1个字符（除了\\n） |
| \[\] | 匹配\[ \]中列举的字符 |
| \\d | 匹配数字，即0-9 |
| \\D | 匹配非数字，即不是数字 |
| \\s | 匹配空白，即 空格，tab键 |
| \\S | 匹配非空白 |
| \\w | 匹配非特殊字符，即a-z、A-Z、0-9、\_、汉字 |
| \\W | 匹配特殊字符，即非字母、非数字、非汉字 |

练习：

*   在字符串 "abcd123" 中匹配 a : `a`
*   在字符串 "abcd123" 中匹配任意一个字符： `.`
*   在字符串 "abcd123" 中匹配 b 和 d ：`[b, d]`
*   在字符串 "abcd123" 中匹配数字: `\d`
*   在字符串 "abcd123" 中匹配非数字内容： `\D`
*   在字符串 "abcd 123" 中匹配空白字符串： `\s`
*   在字符串 "abcd 123" 中匹配非空白字符串： `\S`
*   在字符串 "abcd\_123" 中匹配非特殊字符： `\w`
*   在字符串 "abcd&%123" 中匹配特殊字符： `\W`

##### **2.2 匹配多个字符**

| 正则语法 | 描述 |
| --- | --- |
| \* | 匹配前一个字符出现0次或者无限次，即可有可无 |
| + | 匹配前一个字符出现1次或者无限次，即至少有1次 |
| ? | 匹配前一个字符出现1次或者0次，即要么有1次，要么没有 |
| {m} | 匹配前一个字符出现m次 |
| {m,n} | 匹配前一个字符出现从m到n次 |

练习：

*   在字符串 "油头少年\_python\_666" 中匹配非数字内容： `\D*` 返回： 油头少年`_python_`
*   在字符串 "油头少年\_python\_666" 中匹配数字： `\d+` 返回： `666`
*   在字符串 "油头少年\_python\_666" 中匹配y或py： `[p]?y` 返回： `py`
*   在字符串 "油头少年\_python\_666" 中匹配2个数字： `\d{2}` 返回： `66`
*   在字符串 "油头少年\_python\_666" 中匹配英文字母出现1-3次： `[a-z]{1,3}` 返回：`pyt` `hon`

##### **2.3 匹配开头和结尾**

| 正则语法 | 描述 |
| --- | --- |
| ^ | 匹配字符串开头 |
| $ | 匹配字符串结尾 |
| \[^指定字符\] | 匹配除了指定字符以外的所有字符 |

练习：

*   在字符串 "abc\_python\_666" 中匹配以a开头： `^a` 返回： `a`
*   在字符串 "abc\_python\_666" 中匹配以数字结尾：`\d$` 返回： `6`
*   在字符串 "abc\_python\_666" 中匹配除了数字以外的字符： `[^\d]+` 返回： `abc_python_`

##### **2.4 匹配分组**

| 正则语法 | 描述 |
| --- | --- |
| | | 匹配左右任意一个表达式 |
| (ab) | 将括号中字符作为一个分组 |
| `(?P<name>)` | 分组起别名 |
| \\num | 引用分组num匹配到的字符串 |
| `(?P=name)` | 引用别名为name分组匹配到的字符串 |

练习：

*   在字符串 "abc-python-666" 中匹配数字和特殊字符：`\d+|\W+` 返回：`-` `-` `666`
*   **分组练习我们在下面re模块中进行演示**

#### **3\. re模块**

在Python中需要通过正则表达式对字符串进行匹配的时候，可以使用一个re模块，re模块是Python中内置模块，不需要我们去安装，直接导入使用即可，导入方式：

`import re`
AI写代码

##### **3.1 re.match()**

**re.match() 尝试从字符串的起始位置匹配一个模式，如果不能从起始位置匹配成功，match()就返回None。**

函数语法格式：

`re.match(pattern, string, flags=0)`
AI写代码

函数参数说明：

| 参数 | 描述 |
| --- | --- |
| pattern | 匹配的正则表达式 |
| string | 要匹配的字符串 |
| flags | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等, [正则表达式修饰符-可选标志](#flags) |

**匹配成功re.match方法返回一个匹配的对象，否则返回None。**

我们可以使用匹配对象的 group(num) 或 groups() 函数来获取匹配表达式。

| 匹配对象方法 | 描述 |
| --- | --- |
| group(num) | 默认返回匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。 |
| groups() | 返回一个包含所有小组字符串的元组。 |

实例应用：

`   1.  import re       3.  my_str = "abc_123_DFG_456"       5.  # 匹配任意的一个非特殊字符 从头开始匹配      6.  re_obj = re.match('\w', my_str)      7.  # 打印内容      8.  print("匹配任意的一个非特殊字符 从头开始匹配-->", re_obj.group())       10.  # 使用分组进行匹配      11.  re_obj = re.match('abc_(.*?)_(.*?)_(\d+)', my_str)      12.  print('re_obj.groups() --> ', re_obj.groups())      13.  print('re_obj.group(1) --> ', re_obj.group(1))      14.  print('re_obj.group(2) --> ', re_obj.group(2))      15.  print('re_obj.group(3) --> ', re_obj.group(3))        `

AI写代码

上述代码运行结果:

`   1.  匹配任意的一个非特殊字符 从头开始匹配--> a      2.  re_obj.groups() -->  ('123', 'DFG', '456')      3.  re_obj.group(1) -->  123      4.  re_obj.group(2) -->  DFG      5.  re_obj.group(3) -->  456        `

AI写代码

##### **3.2 re.search()**

**re.search() 扫描整个字符串并返回第一个成功的匹配。**

函数语法格式；

`re.search(pattern, string, flags=0)`
AI写代码

函数参数说明：

| 参数 | 描述 |
| --- | --- |
| pattern | 匹配的正则表达式 |
| string | 要匹配的字符串 |
| flags | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等 |

**匹配成功re.search方法返回一个匹配的对象，否则返回None。**

我们可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式

| 匹配对象方法 | 描述 |
| --- | --- |
| group(num) | 默认返回匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。 |
| groups() | 返回一个包含所有小组字符串的元组。 |

实例应用:

`   1.  import re       3.  my_str = "abc_123_DFG_456"       5.  # 匹配任意的一个非特殊字符 从头开始匹配      6.  re_obj = re.search('\d', my_str)      7.  # 打印内容      8.  print("匹配任意的一个非特殊字符 从头开始匹配-->", re_obj.group())       10.  # 使用分组进行匹配      11.  re_obj = re.search('abc_(.*?)_(.*?)_(\d+)', my_str)      12.  print('re_obj.groups() --> ', re_obj.groups())      13.  print('re_obj.group(1) --> ', re_obj.group(1))      14.  print('re_obj.group(2) --> ', re_obj.group(2))      15.  print('re_obj.group(3) --> ', re_obj.group(3))        `

AI写代码

上述代码运行结果:

`   1.  匹配任意的一个非特殊字符 从头开始匹配--> 1      2.  re_obj.groups() -->  ('123', 'DFG', '456')      3.  re_obj.group(1) -->  123      4.  re_obj.group(2) -->  DFG      5.  re_obj.group(3) -->  456        `

AI写代码

###### **re.match()与re.search()的区别**

**re.match从字符串开始匹配，如果字符串开始不匹配正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配，最终找不到返回None。**

##### **3.3 re.findall()**

**在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。**

**注意：match 和 search 是匹配一次，findall 匹配所有。**

函数语法格式：

`re.findall(pattern, string, flags=0)`
AI写代码

函数参数说明：

| 参数 | 描述 |
| --- | --- |
| pattern | 匹配的正则表达式 |
| string | 要匹配的字符串 |
| flags | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等 |

实例应用：

`   1.  import re       3.  my_str = "abc_123_DFG_456"       5.  # 匹配到字符串中的所有数字      6.  result = re.findall('\d+', my_str)       8.  print(result)        `

AI写代码

上述代码运行结果:

`['123', '456']`
AI写代码

##### **3.4 re.sub()**

**Python 的 re 模块提供了re.sub()用于替换字符串中的匹配项。**

函数语法格式：

`re.sub(pattern, repl, string, count=0, flags=0)`
AI写代码

函数参数说明：

| 参数 | 描述 |
| --- | --- |
| pattern | 匹配的正则表达式 |
| repl | 替换的字符串，也可为一个函数。 |
| string | 要被查找替换的原始字符串 |
| count | 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。 |

实例代码:

`   1.  import re       3.  my_str = "油头少年-Python-666"       5.  # 需求： 将字符串中的 - 替换成 _      6.  new_str = re.sub('-', '_', my_str)      7.  print(new_str)        `

AI写代码

上述代码运行结果油头少年`_Python_666`

repl参数也可以是一个函数：

`   1.  import re       3.  my_str = 'ABC333DEF'       6.  def double(matched):      7.      # 从正则中获取到 匹配的的数字      8.      value = int(matched.group('num'))      9.      return str(value * 2)       12.  result = re.sub('(?P<num>\d+)', double, my_str)      13.  print(result)        `

AI写代码

上述代码运行结果:

`ABC666DEF`
AI写代码

##### **3.5 re.split()**

**re.split() 方法按照能够使用匹配的子串将字符串分割后返回列表**

函数语法格式：

`re.split(pattern, string, maxsplit=0, flags=0)`
AI写代码

函数参数说明:

| 参数 | 描述 |
| --- | --- |
| pattern | 匹配的正则表达式 |
| string | 要匹配的字符串。 |
| maxsplit | 分隔次数，maxsplit=1 分隔一次，默认为 0，不限制次数。 |
| flags | 标志位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。 |

实例应用:

`   1.  import re       3.  my_str = '油头少年, Python, 数据分析'      4.  # 按照 逗号进行分割      5.  result = re.split('\W+', my_str)      6.  print(result)        `

AI写代码

上述代码运行结果:

`['油头少年', 'Python', '数据分析']`
AI写代码

##### **3.6 re.compile()**

**compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 等函数使用。**

函数语法格式：

`re.compile(pattern[, flags])`
AI写代码

函数参数说明:

| 参数 | 描述 |
| --- | --- |
| pattern | 一个字符串形式的正则表达式 |
| flags | 可选，表示匹配模式，比如忽略大小写，多行模式等 |

实例代码:

`   1.  import re       3.  my_str = 'Abc123def456ghi789'       5.  # 生成正则表达式对象      6.  pattern = re.compile('\d+')      7.  # 从开始匹配数字，不会匹配到      8.  res = pattern.match(my_str)      9.  print(res)       11.  # 扫描整个字符串进行匹配      12.  res = pattern.search(my_str)      13.  print(res.group())       15.  # 匹配所有      16.  res = pattern.findall(my_str)      17.  print(res)        `

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)

AI写代码

上述代码运行结果：

`   1.  None      2.  123      3.  ['123', '456', '789']        `

AI写代码

##### **3.7 分组使用演示**

给定字符串： `"<div><a href="https://www.itcast.cn" target="_blank">油头少年</a><p>Python</p></div>"`

要求： 匹配到超链接中的文本内容

实例代码:

`   1.  import re       3.  my_str = '"<div><a href="https://www.itcast.cn" target="_blank">油头少年</a><p>Python</p></div>"'       5.  # 生成正则表达式对象      6.  pattern = re.compile('(<a.*>)(.*)(</a>)')       8.  res = pattern.search(my_str)      9.  print(res.group(2))        `

AI写代码

上述代码运行结果:

`油头少年`

AI写代码

按照上面的分组匹配以后，我们就可以拿到我们想拿到的字符串，但是如果我们正则表达式中括号比较多，那我们在拿我们想要的字串时，要去挨个数我们想要的字串时第几个括号，这样会很麻烦，这个时候Python又引入了另一种分组，那就是命名分组，上面的叫无名分组。

命名分组就是给具有默认分组编号的组另外再给一个别名。命名分组的语法格式如下：

`(?P<name>正则表达式)`
AI写代码

实例代码如下:

`   1.  import re       3.  my_str = '"<div><a href="https://www.itcast.cn" target="_blank">油头少年</a><p>Python</p></div>"'       5.  # 生成正则表达式对象      6.  pattern = re.compile('(<a.*>)(?P<text>.*)(</a>)')       8.  res = pattern.search(my_str)      9.  # 使用分组的名字 获取内容      10.  print(res.group('text'))        `

AI写代码

上述代码运行结果:

`油头少年`

AI写代码

在正则表达式中，放在圆括号 () 中表示的是一个组，我们可以对整个组使用一些正则操作，例如重复操作符。

当使用 () 定义了一个正则表达式组后，正则引擎会将匹配到的组按照顺序编号，存入缓存，这样的话我们想在后面对已经匹配过的内容进行引用时，既可以使用 `\数字` 的方式或者通过命名分组 `(?P=name)` 进行引用， `\1` 表示引用第一个分组，`\2` 表示引用第二个分组，`\n` 表示引用第n个分组。这些引用必须在正则表达式中才有效，用于匹配一些重复的字符串。

实例代码:

`   1.  import re       3.  my_str = '123 123 123'       5.  # 生成正则表达式对象      6.  pattern = re.compile(r'(?P<num>\d+)\s+(?P=num)\s+(?P=num)')       8.  res = pattern.search(my_str)      9.  # 使用分组的名字 获取内容      10.  print(res.group())       12.  # 使用分组编号就行引用      13.  # 生成正则表达式对象      14.  pattern = re.compile(r'(?P<num>\d+)\s+\1\s+\1')      15.  res = pattern.search(my_str)      16.  print(res.group())        `

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)

AI写代码

上述的代码运行结果:

`   1.  123 123 123      2.  123 123 123        `

AI写代码

**练习： 使用分组，交互字符串的位置，比如： `abc.123` 交换完毕之后是 `123.abc`**

完整代码如下：

`   1.  import re       3.  my_str = 'abc.123'       5.  new_str = re.sub(r'(.*)\.(.*)', r'\2.\1', my_str)      6.  print(new_str)        `

AI写代码

上述代码运行结果:

`123.abc`
AI写代码

#### **4. 正则表达式修饰符 - 可选标志**

**正则表达式可以包含一些可选标志修饰符来控制匹配的模式。修饰符被指定为一个可选的标志。多个标志可以通过按位 OR(|) 它们来指定。如 re.I | re.M 被设置成 I 和 M 标志：**

| 修饰符 | 描述 |
| --- | --- |
| re.I | 使匹配对大小写不敏感 |
| re.M | 多行匹配，影响 ^ 和 $ |
| re.S | 使 . 匹配包括换行在内的所有字符 |

`   1.  import re       3.  my_str = 'aB'      4.  # 匹配时不区分大小写      5.  ret = re.match('ab', my_str, flags=re.I)      6.  print(bool(ret))       8.  my_str = 'aabb\nbbcc'      9.  # 多行匹配，影响 ^ 和 $      10.  ret = re.match('^a{2}b{2}$', my_str, flags=re.M)      11.  print(bool(ret))       13.  my_str = '\nabc'      14.  # re.S：影响 . 符号，设置之后，.符号就能匹配\n了      15.  ret = re.match('.', my_str, flags=re.S)      16.  print(bool(ret))        `

![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCodeMoreWhite.png)

AI写代码

数据提取-jsonpath模块
---------------

###### 知识点

*   了解 jsonpath模块的使用场景
*   掌握 jsonpath模块的使用

### 1\. jsonpath模块的使用场景

> 如果有一个多层嵌套的复杂字典，想要根据key和下标来批量提取value，这是比较困难的。jsonpath模块就能解决这个痛点，接下来我们就来学习jsonpath模块

**jsonpath可以按照key对python字典进行批量数据提取**

例如：

有一个多层嵌套的字典，我们需要取其中的一个key对应的value：

```cobol
my_dict = {"key1": {"key2": {"key3": {"key4": {"key5": {"key6": "这是一条数据"}}}}}} # 取到 key6 键对应的值print(my_dict["key1"]["key2"]["key3"]["key4"]["key5"]["key6"])
```

运行结果如下:

```undefined
这是一条数据
```

### 2\. jsonpath的使用方法

#### 2.1 jsonpath语法规则

| JSONPath | 描述 |
| --- | --- |
| $ | 表示根元素 |
| @ | 当前元素 |
| . or \[\] | 子元素 |
| .. | 不管位置，选择符合条件的元素 |
| \* | 匹配所有元素节点 |
| \[\] | 迭代器标示，可以在里面做简单的迭代操作，如数组下标、根据内容选值等。 |
| \[,\] | 支持迭代器做多选 |
| ?() | 支持过滤操作 |
| () | 支持表达式计算 |

#### 2.2 jsonpath语法示例

jsonpath在线测试网址：

```cobol
book_dict = {   "store": {    "book": [       { "category": "reference",        "author": "Nigel Rees",        "title": "Sayings of the Century",        "price": 8.95      },      { "category": "fiction",        "author": "Evelyn Waugh",        "title": "Sword of Honour",        "price": 12.99      },      { "category": "fiction",        "author": "Herman Melville",        "title": "Moby Dick",        "isbn": "0-553-21311-3",        "price": 8.99      },      { "category": "fiction",        "author": "J. R. R. Tolkien",        "title": "The Lord of the Rings",        "isbn": "0-395-19395-8",        "price": 22.99      }    ],    "bicycle": {      "color": "red",      "price": 19.95    }  }}
```

| JSONPath | 描述 |
| --- | --- |
| $.store.book\[\*\].author | 获取store中所有的book的作者 |
| $..author | 获取所有的作者 |
| $.store.\* | 获取store下的所有的元素 |
| $.store.book\[\*\].price | 获取store中的所有的book的价格 |
| $..book\[2\] | 获取第三本书 |
| $..book\[(@.length-1)\] | $..book\[-1:\] | 获取最后一本书 |
| $..book\[0,1\] | $..book\[:2\] | 获取前两本书 |
| $..book\[?(@.isbn)\] | 获取有isbn的所有书 |
| $..book\[?(@.price>10)\] | 获取价格大于10的所有图书 |
| $..\* | 获取所有的数据 |

#### 2.3 jsonpath模块的安装

> 在 python 中使用 jsonpath 语法提取数据，需要安装 jsonpath 模块

```cobol
pip install jsonpath 或者 pip install jsonpath -i https://pypi.tuna.tsinghua.edu.cn/simple
```

安装过程如下图所示：

![](https://i-blog.csdnimg.cn/direct/ced8162f97414c1aa68755ee6e95624e.png)

#### 2.4 jsonpath模块提取数据的方法

```coffeescript
from jsonpath import jsonpath# ret 是提取的结果ret = jsonpath(json_dict, 'jsonpath语法规则字符串')
```

### 3\. jsonpath练习

> 我们以拉勾网城市JSON文件 [http://www.lagou.com/lbs/getAllCitySearchLabels.json](http://www.lagou.com/lbs/getAllCitySearchLabels.json "http://www.lagou.com/lbs/getAllCitySearchLabels.json") 为例，获取所有城市的名字的列表，并写入文件。

参考代码：

```cobol
import requestsimport jsonpathimport json # 获取拉勾网城市json字符串url = 'http://www.lagou.com/lbs/getAllCitySearchLabels.json'headers = {"User-Agent": "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)"}response =requests.get(url, headers=headers)html_str = response.content.decode() # 把json格式字符串转换成python对象jsonobj = json.loads(html_str) # 从根节点开始，获取所有key为name的值city_list = jsonpath.jsonpath(jsonobj,'$..name') # 写入文件with open('city_name.txt','w', encoding='utf-8') as f:    content = json.dumps(city_list, ensure_ascii=False)    f.write(content)
```

运行结果如下所示（数据保存到city\_name.txt文件中）：

![](https://i-blog.csdnimg.cn/direct/5faf9b0ac900440fad32937a473fd96d.png)

### 总结

*   jsonpath常用于json数据结构嵌套层次比较复杂的时候，如果比较简单的结果，直接使用 json模块即可。
    
*   jsonpath模块的使用
    
    ```typescript
    from jsonpath import jsonpathret = jsonpath(json_dict, 'jsonpath语法规则字符串')
    ```
    

### 数据提取之xpath

##### 知识点：

*   掌握 xpath获取节点属性的方法
*   掌握 xpath获取文本的方法
*   掌握 xpath查找特定节点的方法

#### 1\. 什么是xpath

XPath (XML Path Language) 是一门在 HTML\\XML 文档中查找信息的**语言**，可用来在 HTML\\XML 文档中对**元素和属性进行遍历**。

W3School官方文档：[XPath 教程](http://www.w3school.com.cn/xpath/index.asp "XPath 教程")

#### 2\. xpath的节点关系

##### 2.1 xpath中的节点是什么

每个XML的标签我们都称之为节点，其中最顶层的节点称为根节点。

![](https://i-blog.csdnimg.cn/direct/c7ffbdcee0b947ddba90d4a84403725c.png)

##### 2.2 xpath中节点的关系

![](https://i-blog.csdnimg.cn/direct/7eac04bad0f643058da1f7a7d3af75a6.png)

#### 3\. 谷歌浏览器xpath helper插件的安装和使用

> 要想利用lxml模块提取数据，需要我们掌握xpath语法规则。接下来我们就来了解一下xpath helper插件，它可以帮助我们练习xpath语法

##### 3.1 谷歌浏览器xpath helper插件的作用

> 在谷歌浏览器中对当前页面测试xpath语法规则

##### 3.2 谷歌浏览器xpath helper插件的安装和使用

我们以windows为例进行xpath helper插件的安装

###### 3.2.1 xpath helper插件的安装

*   下载Chrome插件 XPath Helper
    
    *   可以在chrome应用商城进行下载，如果无法下载，也可以从下面的链接进行下载
    *   下载地址：[百度网盘-链接不存在](https://pan.baidu.com/s/1-pKX0KL7jCMin4A6zBFHLg "百度网盘-链接不存在") 密码:cvyh
*   把文件的后缀名crx改为zip，然后解压到同名文件夹中
    

![](https://i-blog.csdnimg.cn/direct/5a51e57f2c394ad8a586a371663f3e1c.png)

*   把解压后的文件夹拖入到已经开启开发者模式的chrome浏览器扩展程序界面
    

![](https://i-blog.csdnimg.cn/direct/c6d4c6ca5f6c4994b56b1f66a69dc1eb.png)

![](https://i-blog.csdnimg.cn/direct/24c0abee3a5749eda61d290438b3ce84.png)

*   重启浏览器后，访问url之后在页面中点击xpath图标，就可以使用了
    

![](https://i-blog.csdnimg.cn/direct/a486a848772b4f15a91f4011c0d8190a.png)

> 如果是**linux或macOS操作系统**，无需操作上述的步骤2，直接将crx文件拖入已经开启开发者模式的chrome浏览器扩展程序界面

#### 4\. xpath语法

我们将在下面的例子中使用这个 XML 文档：

```cobol
<bookstore> <book>  <title lang="eng">Harry Potter</title>  <price>29.99</price></book> <book>  <title lang="eng">Learning XML</title>  <price>39.95</price></book> </bookstore>
```

##### 4.1 选取节点

XPath 使用路径表达式来选取 XML 文档中的节点或者节点集。这些路径表达式和我们在常规的**电脑文件系统中看到的表达式**非常相似。

**使用chrome插件选择标签时候，选中时，选中的标签会添加属性class="xh-highlight"**

###### 下面列出了最有用的表达式：

| 表达式 | 描述 |
| --- | --- |
| nodename | 选中该元素 |
| / | 从根节点选取、或者是元素和元素间的过度 |
| // | 从匹配选择的当前节点选择文档中的节点，而不考虑他们的位置。 |
| . | 选取当前节点 |
| .. | 选择当前节点的父节点 |
| @ | 选取属性 |
| text() | 选取文本 |

###### 实例

在下面的表格中，我们已列出了一些路径表达式以及表达式的结果：

| 路径表达式 | 结果 |
| --- | --- |
| bookstore | 选择bookstore元素 |
| /bookstore | 选择bookstore元素，假如路径起始于正斜杠(/) 则此路径始终代表到某元素的绝对路径。 |
| bookstore/book | 选择bookstored的子元素的所有book元素 |
| //book | 选择所有book子元素，而不管他们在文档中的位置 |
| bookstore//book | 选择属于bookstore元素的后代的所有book元素，而不管他们位于bookstore之下的什么位置。 |
| //book/title/@lang | 选择所有的book下面的title中的lang属性的值 |
| //book/title/text() | 选择所有的book下面的title的文本。 |

###### xpath基础语法练习：

接下来我们听过豆瓣电影top250的页面来练习上述语法：[豆瓣电影 Top 250](https://movie.douban.com/top250 "豆瓣电影 Top 250")

*   选择所有的h1下的文本
    *   `//h1/text()`
    *   ![](https://i-blog.csdnimg.cn/direct/ca3c1d8021644a5291d52faa555953d3.png)
*   获取所有的a标签的href
    *   `//a/@href`
    *   ![](https://i-blog.csdnimg.cn/direct/f57238612cc54e26870e98bdbad05eb6.png)
*   获取html下的head下的title的文本
    *   `/html/head/title/text()`
    *   ![](https://i-blog.csdnimg.cn/direct/b5812b36dbf64082b22a79b7cb15336c.png)
*   获取html下的head下的link标签的href
    *   `/html/head/link/@href`
    *   ![](https://i-blog.csdnimg.cn/direct/4bc4e116fcb742d0b91394a96df231fb.png)

但是当我们需要选择所有的电影名称的时候会特别费力，通过下一小节的学习，就能够解决这个问题

##### 4.2 查找特定的节点

| 路径表达式 | 结果 |
| --- | --- |
| //title\[@lang='eng'\] | 选择lang属性值为eng的所有title元素 |
| /bookstore/book\[1\] | 选择属于bookstore子元素的第一个book元素 |
| /bookstore/book\[last()\] | 选择属于bookstore子元素最后一个book元素 |
| /bookstore/book\[last()-1\] | 选择属于bookstore子元素的倒数第二个book元素 |
| /bookstore/book\[position()>1\] | 选择bookstore下面的book元素，从第二个开始选择 |
| //book/title\[text()='Harry Potter'\] | 选择所有book下的title元素，仅选择文本为Harry Potter的title元素 |
| /bookstore/book\[price>35.00\]/title | 选择bookstore元素中book元素的所有title元素，且其中的price元素的值须大于35 |

注意点: **在xpath中，第一个元素的位置是1，最后一个元素的位置是last(),倒数第二个是last()-1**

###### xpath基础语法练习2：

从豆瓣电影top250的页面中：选择所有的电影的名称，href，评分，评价人数

*   选取所有的电影的名字：
    
    ```cobol
    # 先定位到 class=hd的div标签，再取下面的a标签下面的第一个span标签//div[@class='hd']/a/span[1]
    ```
    
*   ![](https://i-blog.csdnimg.cn/direct/7f79f4dba5364fbcaab428eda0654d8f.png)
    
*   选取所有的href：
    
    ```cobol
    # 先定位到 class=hd的div标签，再取下面的a标签中href属性的值//div[@class='hd']/a/@href
    ```
    
*   ![](https://i-blog.csdnimg.cn/direct/b3c689eba80f4527b27a018eafa37df8.png)
    
*   选取所有的评分：
    
    ```cobol
    # 评分的标签是一个span标签，并且对应的有class属性，先定位到 class=rating_num 的span标签，再取标签下的文本即可//span[@class='rating_num']/text()
    ```
    
*   ![](https://i-blog.csdnimg.cn/direct/3095b2f5b2b2468ea881c134b2affa04.png)
    
*   选取所有的评价人数：
    
    ```css
    # 评分标签是一个span标签，有class属性，但是没有值，我们可以先定位到他的父级div，# 因为评分是最后一个span标签，所以使用last() 取最后一个span标签即可//div[@class='star']/span[last()]
    ```
    

        ![](https://i-blog.csdnimg.cn/direct/2448c6c56a9948e4a506f259e28767e6.png)

##### 4.3 选取未知节点

XPath 通配符可用来选取未知的 XML 元素。

| xpath通配符 | 描述 |
| --- | --- |
| \* | 匹配任何元素节点 |
| @\* | 匹配任何属性节点 |
| node() | 匹配任何类型的节点 |

###### 实例

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式 | 描述 |
| --- | --- |
| /bookstore/\* | 选取bookstore元素的所有子元素 |
| //\* | 选取文档中所有元素 |
| //title\[@\*\] | 选取所有带有属性的title元素 |

##### 4.4 选取若干路径

通过在路径表达式中使用“|”运算符，您可以选取若干个路径。

###### 实例

在下面的表格中，我们列出了一些路径表达式，以及这些表达式的结果：

| 路径表达式 | 结果 |
| --- | --- |
| //book/title | //book/price | 选取book元素所有的title和price元素 |
| //title | //price | 选取文档中所有的title和price元素 |
| /bookstore/book/title | //price | 选取属于bookstore元素的book元素的所有title元素，以及文档中所有的price元素 |

### 总结

*   xpath中常用的获取节点的表达式
    
    *   `/` 常用于元素和元素之间的过度
    *   `//` 选取节点，不考虑其位置，常用
    *   `.` 表示选取当前节点
    *   `..` 表示选取当前节点的上一级节点(父节点)
    *   `@属性名` 根据属性名，获取标签中属性的值。
        
    *   `text()` 获取标签下的文本内容(字符串内容)
        
*   xpath中常用的获取特定节点的表达式:
    
    *   `//标签名[@属性名=值]` 根据标签的属性以及属性的值获取特定的标签
    *   `//标签名[num]` 获取选取到的所有标签的第几个标签，num从 1 开始。
    *   `//标签名[last()]` 获取选取到的所有的标签的最后一个标签，注意：最后一个不是 -1。
    *   `//标签名[text()=值]` 根据标签中的文本内容的值，获取到某个标签。
    *   `//标签名[position()>num]` 表示从第num个标签获取
    *   `//标签名[position()<num]` 表示从第一个获取到第num个标签

### lxml模块的学习

##### 知识点:

*   应用 lxml库提取数据方法
*   了解 lxml对数据处理和提取之后的数据类型
*   了解 lxml把element转化为字符串的方法

在前面学习了xpath的语法，那么在[python爬虫](https://so.csdn.net/so/search?q=python%E7%88%AC%E8%99%AB&spm=1001.2101.3001.7020)代码中我们如何使用xpath呢? 对应的我们需要lxml模块。

> lxml是一款高性能的 Python HTML/XML 解析器，我们可以利用XPath，来快速的定位特定元素以及获取节点信息

#### 1\. lxml的安装

```cobol
pip install lxml 或者 pip install lxml -i https://pypi.tuna.tsinghua.edu.cn/simple
```

安装过程如下图所示:

![](https://i-blog.csdnimg.cn/direct/be66d0c8502d4697a14dfc46bf4361de.png)

#### 2\. lxml的使用

##### 2.1 lxml模块的入门使用

*   导入lxml 的 etree 库 (导入没有提示不代表不能用)
    
    ```typescript
    from lxml import etree
    ```
    
*   利用etree.HTML方法将字符串转化为Element对象，Element对象具有xpath的方法，参数能够接受bytes类型的数据和str类型的数据，返回结果的为列表
    
    ```cobol
    html = etree.HTML(text) ret_list = html.xpath("xpath字符串")
    ```
    
*   把转化后的element对象转化为字符串，返回bytes类型结果 `etree.tostring(element)`
    

假设我们现有如下的html字符换，尝试对他进行操作

```cobol
<div> <ul> <li class="item-1"><a href="link1.html">first item</a></li> <li class="item-1"><a href="link2.html">second item</a></li> <li class="item-inactive"><a href="link3.html">third item</a></li> <li class="item-1"><a href="link4.html">fourth item</a></li> <li class="item-0"><a href="link5.html">fifth item</a> # 注意，此处缺少一个 </li> 闭合标签 </ul> </div>
```

```xml
from lxml import etree text = """ <div> <ul>         <li class="item-1"><a href="link1.html">first item</a></li>         <li class="item-1"><a href="link2.html">second item</a></li>         <li class="item-inactive"><a href="link3.html">third item</a></li>         <li class="item-1"><a href="link4.html">fourth item</a></li>         <li class="item-0"><a href="link5.html">fifth item</a>         </ul> </div> """ # 1. 将html字符串转换成Element对象element = etree.HTML(text) print(element, type(element)) # 2. 可以使用 etree.tostring(element).decode() 将Element对象转换成字符串str = etree.tostring(element).decode()print(str)
```

输出结果为：

```cobol
<Element html at 0x20f93f4d680> <class 'lxml.etree._Element'><html><body><div> <ul> <li class="item-1"><a href="link1.html">first item</a></li> <li class="item-1"><a href="link2.html">second item</a></li> <li class="item-inactive"><a href="link3.html">third item</a></li> <li class="item-1"><a href="link4.html">fourth item</a></li> <li class="item-0"><a href="link5.html">fifth item</a> </li></ul> </div></body></html>
```

可以发现，lxml确实能够把缺失的标签补充完成，但是请注意**lxml是人写的，很多时候由于网页不够规范，或者是lxml的bug，即使参考url地址对应的响应去提取数据，仍然获取不到，这个时候我们需要使用etree.tostring的方法，观察etree到底把html转化成了什么样子，即根据转化后的html字符串去进行数据的提取**。

##### 2.2 lxml的深入练习

> 接下来我们继续操作，假设每个class为item-1的li标签是1条新闻数据，如何把这条新闻数据组成一个字典

```cobol
from lxml import etreetext = """ <div> <ul>         <li class="item-1"><a href="link1.html">first item</a></li>         <li class="item-1"><a href="link2.html">second item</a></li>         <li class="item-inactive"><a href="link3.html">third item</a></li>         <li class="item-1"><a href="link4.html">fourth item</a></li>         <li class="item-0"><a href="link5.html">fifth item</a>         </ul> </div> """ element = etree.HTML(text) # 获取href的列表和title的列表href_list = element.xpath("//li[@class='item-1']/a/@href")title_list = element.xpath("//li[@class='item-1']/a/text()") # 组装成字典for i, href in enumerate(href_list):    item = {}    item['href'] = href    item['title'] = title_list[i]    print(item)
```

输出为

```csharp
{'href': 'link1.html', 'title': 'first item'}{'href': 'link2.html', 'title': 'second item'}{'href': 'link4.html', 'title': 'fourth item'}
```

> 假设在某种情况下，某个新闻的href没有，那么会怎样呢？

```xml
from lxml import etreetext = """ <div> <ul>         <li class="item-1"><a>first item</a></li>         <li class="item-1"><a href="link2.html">second item</a></li>         <li class="item-inactive"><a href="link3.html">third item</a></li>         <li class="item-1"><a href="link4.html">fourth item</a></li>         <li class="item-0"><a href="link5.html">fifth item</a>         </ul> </div> """
```

输出为：

```csharp
{'href': 'link2.html', 'title': 'first item'}{'href': 'link4.html', 'title': 'second item'}
```

数据的对应全部错了，这不是我们想要的，接下来通过2.3小节的学习来解决这个问题

##### 2.3 lxml模块的进阶使用

> 前面我们取到属性，或者是文本的时候，返回字符串。但是如果我们取到的是**一个节点**，返回什么呢?

**返回的是element对象，可以继续使用xpath方法**，对此我们可以在后面的数据提取过程中：**先根据某个标签进行分组，分组之后再进行数据的提取**

示例如下：

```xml
from lxml import etreetext = """ <div> <ul>         <li class="item-1"><a>first item</a></li>         <li class="item-1"><a href="link2.html">second item</a></li>         <li class="item-inactive"><a href="link3.html">third item</a></li>         <li class="item-1"><a href="link4.html">fourth item</a></li>         <li class="item-0"><a href="link5.html">fifth item</a>         </ul> </div> """ element = etree.HTML(text) li_list = element.xpath("//li[@class='item-1']")print(li_list)
```

结果为：

```cobol
[<Element li at 0x11106cb48>, <Element li at 0x11106cb88>, <Element li at 0x11106cbc8>]
```

可以发现结果是一个element对象，这个对象能够继续使用xpath方法

先根据li标签进行分组，之后再进行数据的提取

```cobol
from lxml import etreetext = """ <div> <ul>         <li class="item-1"><a>first item</a></li>         <li class="item-1"><a href="link2.html">second item</a></li>         <li class="item-inactive"><a href="link3.html">third item</a></li>         <li class="item-1"><a href="link4.html">fourth item</a></li>         <li class="item-0"><a href="link5.html">fifth item</a>         </ul> </div> """ # 根据li标签进行分组element = etree.HTML(text)li_list = element.xpath("//li[@class='item-1']") # 在每一组中继续进行数据的提取for li in li_list:    item = {}    item["href"] = li.xpath("./a/@href")[0] if len(li.xpath("./a/@href"))>0 else None    item["title"] = li.xpath("./a/text()")[0] if len(li.xpath("./a/text()"))>0 else None    print(item)
```

结果是：

```rust
{'href': None, 'title': 'first item'}{'href': 'link2.html', 'title': 'second item'}{'href': 'link4.html', 'title': 'fourth item'}
```

前面的代码中，进行数据提取需要判断，可能某些一面不存在数据的情况，对应的可以使用三元运算符来解决

> **以上提取数据的方式：先分组再提取，会是我们进行数据的提取的主要方法。**

#### 3\. 案例-百度贴吧

##### 3.1 需求：

**给定一个贴吧的名字，抓取该贴吧中，第一页中帖子的标题、帖子的详情页url地址，以及帖子详情页中图片的链接，最终要将图片保存到本地。**

需要抓取的字段：

*   帖子标题(title)
*   帖子详情页url地址(detail\_url)
*   详情页所有图片url地址(img\_url\_list)

##### 3.2 分析

在前面的百度贴吧案例中，我们已经知道，贴吧的url地址是: `https://tieba.baidu.com/f?kw=贴吧名字`，在这里，我们暂时只需要抓取第一页的数据，我们就直接去请求要抓取的url地址即可。

确定好url地址之后，我们要分析一下要抓取数据的在响应html中的位置。

右键检查 --> 定位每一条帖子的位置。

![](https://i-blog.csdnimg.cn/direct/e92f7c8c0cf9414eae68fb0ee3cd4596.png)

经过分析，我们发现，每一条帖子的信息就是一个 `li` 标签，我们就可以先使用 `xpath` 定位到所有的 li 标签。然后再去遍历获取到帖子的标题以及详情页的url地址。

获取所有的li标签： `//ul[@id="thread_list"]/li[contains(@class, "j_thread_list")]`

帖子标题：`//a[@class="j_th_tit"]/text()`

帖子详情页url地址： `//a[@class="j_th_tit"]/@href`

但是在抓取帖子详情页url地址的时候，源码中的url地址并不是完整的url地址，缺少了前面的域名： `https://tieba.baidu.com/`：

![](https://i-blog.csdnimg.cn/direct/fd9e415ef7ec4bf8b6cdadc1ce1032c2.png)

我们要注意，需要将详情页的url地址构造完整之后，再去请求。

进入到详情页中，找到图片对应的标签：

![](https://i-blog.csdnimg.cn/direct/2b86c0d7fdf4469b8be52177391c57fa.png)

图片都有一个class值为： `BDE_Image`，我们可以使用xpath语法： `//img[@class="BDE_Image"]/@src` 获取到img标签中的src属性的值，也就获取到了图片的url地址。

获取到图片的url地址之后，我们再对图片的url地址去发送请求，获取到图片的响应内容，将图片的响应内容保存到文件中即可。

##### 3.3 代码实现

完整代码如下：

```python
import requestsfrom lxml import etree  class TieBaSpider(object):    def __init__(self, tb_name):        # 准备贴吧名字        self.url = 'https://tieba.baidu.com/f?kw={}'.format(tb_name)        # 准备请求头        self.headers = {            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36",            "Cookie": "从浏览器复制的cookie信息"        }        self.base_url = "https://tieba.baidu.com"        # 存储所有数据的列表        self.data_list = []     def send_request(self, url):        """        发送请求 获取响应的方法        :param url: 发送请求的url地址        :return: 响应的二进制内容（因为要保存图片，图片在保存的时候需要保存二进制内容）        """        response = requests.get(url, headers=self.headers)        return response.content     def parse_data(self, html):        """        解析列表页数据的方法        :param html: 响应的二进制内容        """        # 将源码中注释符号去掉        html_str = html.decode().replace('<!--', '').replace('-->', '')        # 创建element对象        element = etree.HTML(html_str)        # 获取到帖子的列表        li_list = element.xpath('//ul[@id="thread_list"]/li[contains(@class, "j_thread_list")]')        # 遍历每一个li标签，提取每一个帖子的信息        for li in li_list:            # 创建字典，存储每一条数据            item = {}            # 获取帖子的标题            item['title'] = li.xpath('.//a[@class="j_th_tit "]/text()')[0]            # 获取帖子详情页的url地址            item['detail_url'] = li.xpath('.//a[@class="j_th_tit "]/@href')[0]            item['detail_url'] = self.base_url + item['detail_url']            # 对详情页进行解析            img_url_list = self.parse_detail(item['detail_url'])            item['img_url_list'] = img_url_list            print(item)             # 将一条帖子的数据添加到数据列表中            self.data_list.append(item)     def parse_detail(self, detail_url):        """        解析详情页图片的方法        :param detail_url: 详情页的url地址        :return: 详情页图片url地址的列表        """        # 对详情页的url地址发送请求，获取详情页url地址的响应        detail_response = self.send_request(detail_url)        # 创建element对象        detail_element = etree.HTML(detail_response)        # 获取图片url地址的列表        img_url_list = element.xpath('//img[@class="BDE_Image"]/@src')        return img_url_list     def save_data(self):        # 获取图片的url地址，发送请求 获取响应 并保存到文件中        for item in self.data_list:            url_list = item['img_url_list']            for img_url in url_list:                img_response = self.send_request(img_url)                # 定义文件的名字                file_name = img_url[-20:]                with open(file_name, 'wb') as f:                    f.write(img_response)                print(f'图片--> {file_name}  保存成功......')     def start(self):        # 1. 发送请求获取响应        html = self.send_request(self.url)        # 2. 解析数据        self.parse_data(html)        # 3. 保存数据        self.save_data()  if __name__ == '__main__':    # 获取要抓取的贴吧名字：    tb_name = input('请输入要抓取的贴吧名字：')    # 创建对象    tieba = TieBaSpider(tb_name)    tieba.start()
```

项目运行效果如下所示:

![](https://i-blog.csdnimg.cn/direct/ad9d47ca05e644f29b03955f4d924ace.png)

### 总结

*   lxml 模块可以让我们在Python代码中去使用 xpath 语法表达式来进行对 html/xml 数据进行提取
*   lxml 的使用步骤：
    *   导包: `from lxml import etree`
    *   将html字符串转换成Element对象： `element = etree.HTML(html_str)`
    *   使用Element对象中的xpath方法书写xpath表达式，来定位或者获取标签中的内容: `element.xpath(xpath_str)`
    *   使用xpath语法如果定义到标签，返回的就是一个列表，列表中的每个元素是一个Element对象，可以遍历再去使用 Element.xpath() 方法继续提取数据。
*   将转换后的Element对象再转换成html\_str, 可以使用 `etree.tostring(element).decode()`
*   **一般使用xpath来提取数据，都是会先对数据进行分组，然后再遍历提取。**

### BeautifulSoup4的学习

###### 学习目标

*   知道 bs4警告的原因
*   掌握 bs4的使用流程
*   掌握 bs4的find\_all、find、select方法解析数据

> 由于xpath解析数据需要对html结构有深刻的理解,可能对部分同学产生了学习压力, 那么是不是还有其他的解析方法呢?接下来我们学习使用css选择器解析数据的操作库：BeautifulSoup4

#### 1\. CSS 选择器：BeautifulSoup4的介绍和安装

和 lxml 一样，Beautiful Soup 也是一个HTML/XML的解析器，主要的功能也是如何解析和提取 HTML/XML 数据。

lxml 只会局部遍历，而Beautiful Soup 是基于HTML DOM的，会载入整个文档，解析整个DOM树，因此时间和内存开销都会大很多，所以性能要低于lxml。

BeautifulSoup 用来解析 HTML 比较简单，API非常人性化，支持CSS选择器、Python标准库中的HTML解析器，也支持 lxml 的 XML解析器。

Beautiful Soup 3 目前已经停止开发，推荐现在的项目使用Beautiful Soup 4。使用 pip 安装即可：

```cobol
pip install beautifulsoup4 或者 pip install beautifulsoup4 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

安装过程如下图所示：

![](https://i-blog.csdnimg.cn/direct/2f0f237b1de640f99b474fb2edd59a59.png)

官方文档：[Beautiful Soup 4.4.0 文档 — Beautiful Soup 4.2.0 中文 文档](http://beautifulsoup.readthedocs.io/zh_CN/v4.4.0 "Beautiful Soup 4.4.0 文档 — Beautiful Soup 4.2.0 中文 文档")

| 抓取工具 | 速度 | 使用难度 | 安装难度 |
| --- | --- | --- | --- |
| 正则表达式 | 最快 | 困难 | 无(内置) |
| BeautifulSoup | 慢 | 最简单 | 简单 |
| lxml | 快 | 简单 | 一般 |

##### 1.1 bs4的基本使用示例：

首先必须要导入 bs4 库

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html) # 打开本地 HTML 文件的方式来创建对象# soup = BeautifulSoup(open('index.html')) # 格式化输出 soup 对象的内容print soup.prettify()
```

运行结果：

```cobol
<html> <head>  <title>   The Dormouse's story  </title> </head> <body>  <p class="title" name="dromouse">   <b>    The Dormouse's story   </b>  </p>  <p class="story">   Once upon a time there were three little sisters; and their names were   <a class="sister" href="http://example.com/elsie" id="link1">    <!-- Elsie -->   </a>   ,   <a class="sister" href="http://example.com/lacie" id="link2">    Lacie   </a>   and   <a class="sister" href="http://example.com/tillie" id="link3">    Tillie   </a>   ;and they lived at the bottom of a well.  </p>  <p class="story">   ...  </p> </body></html>
```

###### 注意：

如果我们在 Python 下执行，会看到这样一段警告：

![](https://i-blog.csdnimg.cn/direct/63871607a7d44059bc495a98ccf88e64.png)

意思是，如果我们没有显式地指定解析器，所以默认使用这个系统的最佳可用HTML解析器("lxml")。

但是我们可以通过`soup = BeautifulSoup(html, "lxml")`方式指定lxml解析器。

#### 2\. 搜索文档树

##### 2.1 find\_all(name, attrs, recursive, text, \*\*kwargs)

###### 2.1.1 name 参数

name 参数可以查找所有名字为 name 的tag

###### 2.1.1.1 传字符串

最简单的过滤器是字符串。在搜索方法中传入一个字符串参数，Beautiful Soup会查找与字符串完整匹配的内容，下面的例子用于查找文档中所有的**标签:**

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 找到所有的b标签print(soup.find_all('b'))# 找到所有的a标签print(soup.find_all('a'))
```

运行结果如下所示：

```xml
[<b>The Dormouse's story</b>][<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.1.1.2 传正则表达式

如果传入正则表达式作为参数，Beautiful Soup会通过正则表达式的 match() 来匹配内容。下面例子中找出所有以b开头的标签，这表示**b开头的标签都应该被找到**

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 找到以b开始的标签import refor tag in soup.find_all(re.compile("^b")):    print(tag.name)
```

运行结果如下所示:

```less
bodyb
```

###### 2.1.1.3 传列表

如果传入列表参数，Beautiful Soup会将与列表中任一元素匹配的内容返回。下面代码找到文档中所有**a标签和b标签**：

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 找到所有的a 和 b标签print(soup.find_all(["a", "b"]))
```

运行结果如下所示:

```xml
[<b>The Dormouse's story</b>, <a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.1.2 keyword 参数

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 找到 class 是 sister 的所有标签print(soup.find_all(class_="sister"))# 找到 id 为 link2 的标签print(soup.find_all(id='link2'))
```

运行结果如下所示:

```cobol
[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>][<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```

###### 2.1.3 text 参数

通过 text 参数可以搜索文档中的字符串内容，与 name 参数的可选值一样，text 参数接受字符串、正则表达式、列表：

```cobol
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 找到文本内容是 Lacie 的标签print(soup.find_all(text="Lacie")) # 找到标签中文本内容为 "Tillie", "Elsie", "Lacie" 的所有标签print(soup.find_all(text=["Tillie", "Elsie", "Lacie"])) # 根据正则找到 文本内容有 Dormouse 的内容import reprint(soup.find_all(text=re.compile("Dormouse")))
```

运行结果如下所示:

```less
[]['Lacie', 'Tillie']["The Dormouse's story", "The Dormouse's story"]
```

##### 2.2 find

find的用法与find\_all一样，区别在于find返回 第一个符合匹配结果，find\_all则返回 所有匹配结果的列表

##### 2.3 CSS选择器

这就是另一种与 find\_all 方法有异曲同工之妙的查找方法，也是返回所有匹配结果的列表。

*   写 CSS 时，标签名不加任何修饰，类名前加.，id名前加#
*   在这里我们也可以利用类似的方法来筛选元素，用到的方法是 soup.select()，返回类型是 list

###### 2.3.1 通过标签选择器查找

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 通过标签选择器，获取到所有的title标签print(soup.select('title')) # 通过标签选择器，获取到所有的a标签print(soup.select('a')) # 通过标签选择器，获取到所有的b标签print(soup.select('b'))
```

运行结果如下所示:

```xml
[<title>The Dormouse's story</title>][<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>][<b>The Dormouse's story</b>]
```

###### 2.3.2 通过类选择器查找

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 通过类选择器，选择所有的 class='sister' 的标签print(soup.select('.sister'))
```

```cobol
[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.3.3 通过 id 选择器查找

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 通过id选择器，获取 id 是 link3 的标签print(soup.select('#link3'))
```

运行结果如下所示:

```cobol
[<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.3.4 层级选择器 查找

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 通过层级选择器，获取 p 标签下 id 为 link3 的标签print(soup.select('p #link3'))
```

运行结果如下所示:

```cobol
[<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.3.5 通过属性选择器查找

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 通过属性选择器，获取 class 为 sister 的标签print(soup.select("a[class='sister']"))
```

运行结果如下所示：

```cobol
[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```

###### 2.3.6 获取文本内容 get\_text()

以上的 select 方法返回的结果都是列表形式，可以遍历形式输出，然后用 get\_text() 方法来获取它的内容。

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 获取文本内容# 获取到所有的a标签a_list = soup.select('a')# 遍历，获取每一个a标签下的文本内容for a in a_list:    print(a.get_text())
```

运行结果如下所示:

```undefined
 LacieTillie
```

> 注意： 第一个a标签中的内容是被注释掉的，所以在获取的时候是无法获取到的。

###### 2.3.7 获取属性 get('属性的名字')

```xml
from bs4 import BeautifulSoup html = """<html><head><title>The Dormouse's story</title></head><body><p class="title" name="dromouse"><b>The Dormouse's story</b></p><p class="story">Once upon a time there were three little sisters; and their names were<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;and they lived at the bottom of a well.</p><p class="story">...</p>""" # 创建 Beautiful Soup 对象soup = BeautifulSoup(html, "lxml") # 获取文本内容# 获取到所有的 a 标签a_list = soup.select('a')# 遍历，获取每一个 a 标签的所有的 href 属性的值for a in a_list:    print(a.get('href'))
```

运行结果如下所示:

```cobol
http://example.com/elsiehttp://example.com/laciehttp://example.com/tillie
```

#### 3\. 案例-糗事百科

##### 3.1 需求

*   要抓取的目标 url地址： [https://www.qiushibaike.com/text/](https://www.qiushibaike.com/text/ "https://www.qiushibaike.com/text/")
    
*   需要抓取的数据字段:
    
    *   作者名
    *   段子内容
    *   好笑数
    *   评论数
*   抓取完的数据整体是一个列表，列表中的每个元素是一个字典，每个字典是一个段子的内容，例如:
    
    ```cobol
    [    {        'author': '尼古拉斯-赵四',         'content': '这是一条段子的内容',         'funny_num': 100,         'comment_num': 100    },    {        'author': '王大拿',         'content': '这是一条段子的内容',         'funny_num': 100,         'comment_num': 100    },    ...]
    ```
    

##### 3.2 分析

**确定url地址**

我们首先要确定的就是，地址栏中请求的地址 [https://www.qiushibaike.com/text/](https://www.qiushibaike.com/text/ "https://www.qiushibaike.com/text/") 的响应内容中是否包含了我们想要抓取的数据内容。

验证方式： 页面中鼠标右键，查看网页源码，打开源码页面，从页面中复制一个段子内容或者作者名字，在源码中进行搜索，看看能不能搜索到相对应的内容：

![](https://i-blog.csdnimg.cn/direct/cd1c856633af44fa86d61e6536200813.png)

通过搜索，我们发现，要抓取的数据就在我们通过浏览器地址栏请求的url地址对应的响应中，所以我们在程序中就可以直接对该地址栏中的url地址发起请求，拿到响应的数据。

**确定数据所在的位置**

选择一个段子的内容，使用右键--> 检查，查看一下 html 的结构关系：

![](https://i-blog.csdnimg.cn/direct/b7ab75f89b7e44d8afa7adea45147ec8.png)

通过浏览器工具我们发现，每一个段子对应的是一个 div 标签，这些div标签都是属于 `class="col1 old-style-col1"` div 下的子标签，那么我们可以先获取都该div下面的所有div标签，然后遍历每一个段子的div标签，获取每一条数据。

当前根据分析，我们已经可以获取到第一页的内容，我们再寻找一下翻页的规律，将所有的段子内容抓取到。

```cobol
第一页的url地址： https://www.qiushibaike.com/text/第二页的url地址： https://www.qiushibaike.com/text/page/2/第三页的url地址： https://www.qiushibaike.com/text/page/3/第四页的url地址： https://www.qiushibaike.com/text/page/4/
```

通过观察，我们发现，每一页的url地址的规律是参数 `/page/num/` num 的值不断的变化，第一页的url地址中参数并没有 `/page/num/`, 我们可以尝试的在第一页的url地址的后面加上 `/page/1/` 尝试一下看看是否可以请求到第一页的数据。

![](https://i-blog.csdnimg.cn/direct/a88d3c358ac54f30bacce6ac3a382f0b.png)

经过验证，我们发现，第一页的url地址其实是： `https://www.qiushibaike.com/text/page/1/`，我们就可以统一url地址的格式了。

段子总共有 13 页，那我们就可以先将13页的 url地址全部构造出来，然后再遍历分别去进行请求。

##### 3.3 完整代码实现

以下是案例的完整代码：

```python
import jsonimport time import requestsfrom bs4 import BeautifulSoup  class QiuBaiSpider(object):     def __init__(self):        # url模板        self.base_url = 'https://www.qiushibaike.com/text/page/{}'        # 请求头信息        self.headers = {            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/90.0.4430.93 Safari/537.36"        }        # 存储url地址的列表        self.url_list = []        # 存储数据的列表        self.data_list = []     def get_url(self):        # 构造所有url地址的方法        self.url_list = [self.base_url.format(i) for i in range(1,14)]     def send_request(self, url):        """        发送请求 获取响应数据的方法        :param url: 要发送请求的url地址        :return: 响应的html字符串        """        response = requests.get(url, headers=self.headers)        html_str = response.content.decode()        return html_str     def parse_data(self, html_str):        """        解析数据的方法        :param html_str: 要解析数据的html_str        :return: 数据列表        """        # 创建 Beautiful Soup 对象        soup = BeautifulSoup(html_str, "lxml")         # 先获取到所有的包含段子数据的div标签        div_list = soup.select('div.col1.old-style-col1 > div')        # 遍历每一个div标签，获取每一个div标签        for div in div_list:            # 定义空字典，存储每一条数据的内容            item = {}            # 获取段子作者            item['author'] = div.find('h2').get_text().strip()            # 获取段子内容            item['content'] = div.find('div', {'class': 'content'}).get_text().strip()            # 获取好笑数            item['funny_num'] = div.select('span.stats-vote > i')[0].get_text().strip()            # 获取评论数            item['comment_num'] = div.select('a.qiushi_comments > i')[0].get_text().strip()            # 将获取到的每一条段子内容的字典，添加到列表中            self.data_list.append(item)     def save_data(self):        # 保存数据的方法        with open('qiubai.json', 'w', encoding='utf-8') as f:            json.dump(self.data_list, f, ensure_ascii=False, indent=2)     def start(self):        # 1. 构造url地址        self.get_url()        # 2. 发送请求 获取响应        for url in self.url_list:            html_str = self.send_request(url)            # 为了防止被封IP，设置延迟访问            time.sleep(2)            # 3. 解析响应            self.parse_data(html_str)            print(f'{url} ---> 解析完毕......')         # 4. 保存数据        self.save_data()  if __name__ == '__main__':    # 创建对象    qiubai = QiuBaiSpider()    qiubai.start()
```

上述代码运行结果如下：

```cobol
https://www.qiushibaike.com/text/page/1 ---> 解析完毕......https://www.qiushibaike.com/text/page/2 ---> 解析完毕......https://www.qiushibaike.com/text/page/3 ---> 解析完毕......https://www.qiushibaike.com/text/page/4 ---> 解析完毕......https://www.qiushibaike.com/text/page/5 ---> 解析完毕......https://www.qiushibaike.com/text/page/6 ---> 解析完毕......https://www.qiushibaike.com/text/page/7 ---> 解析完毕......https://www.qiushibaike.com/text/page/8 ---> 解析完毕......https://www.qiushibaike.com/text/page/9 ---> 解析完毕......https://www.qiushibaike.com/text/page/10 ---> 解析完毕......https://www.qiushibaike.com/text/page/11 ---> 解析完毕......https://www.qiushibaike.com/text/page/12 ---> 解析完毕......https://www.qiushibaike.com/text/page/13 ---> 解析完毕......
```

保存到文件的数据内容：

![](https://i-blog.csdnimg.cn/direct/55a343ac680841ebbbe69bfb09801afa.png)

### 总结

*   bs4 是专门用于解析 xml/html的解析器。
*   bs4 使用的步骤
    
    ```haskell
    from bs4 import BeautifulSoup # 创建 Beautiful Soup 对象soup = BeautifulSoup(html_str, "lxml")
    ```
    
*   注意： 在创建 BeautifulSoup 对象的时候，如果不明确指定解析器，会有警告，解决警告的方式就是在创建对象的时候，指定一下。
*   soup.find\_all(name,keyword,text)
    
    *   name 可以接收 字符串(标签名)、正则表达式、列表
    *   keyword 一般是用于根据属性名已经对应的属性值选择标签
    *   text 根据标签中文本的内容，获取数据。
*   soup.find() 和 soup.findall() 用法一样，区别在于，find()返回满足条件的第一个，find\_all()返回满足条件的所有。
    
*   bs4中使用css选择器
    
    *   标签选择器： soup.select('标签名')
    *   类选择器： soup.select('.类名')
    *   id选择器： soup.select('#id值')
    *   属性选择器： soup.select('标签名\[属性名=值\]')
    *   层级选择器： soup.select('选择器1 选择器2 ...')
*   获取标签中的文本内容： 标签.get\_text()
*   获取标签中属性的内容： 标签.get('属性名')

本文转自 <https://blog.csdn.net/m0_63845988/article/details/148412375?spm=1001.2100.3001.7377&utm_medium=distribute.pc_feed_blog.none-task-blog-hot-1-148412375-null-null.nonecase&depth_1-utm_source=distribute.pc_feed_blog.none-task-blog-hot-1-148412375-null-null.nonecase>，如有侵权，请联系删除。