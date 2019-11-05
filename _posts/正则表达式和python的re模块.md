---
title: 正则表达式和python的re模块
tags: python全栈开发，正则表达式，re模块
grammar_cjkRuby: true
---
##### **一 正则表达式**
正则表达式(Regular Expression)是一种文本模式，包括普通字符（例如，a 到 z 之间的字母）和特殊字符（称为"元字符"）。
正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串。正则表达式是繁琐的，但它是强大的，学会之后的应用会让你除了提高效率外，会给你带来绝对的成就感。
正则表达式本身也和python没有什么关系，就是匹配字符串内容的一种规则。许多程序设计语言都支持利用正则表达式进行字符串操作。

先看一个简单的例子，怎么判断一个电话号码是否是合法的呢？
根据手机号码一共11位并且是只以13、14、15、18开头的数字这些特点，我们用python写了如下代码：

``` python
while True:
    phone_number = input('please input your phone number ： ')
    if len(phone_number) == 11 \
            and phone_number.isdigit()\
            and (phone_number.startswith('13') \
            or phone_number.startswith('14') \
            or phone_number.startswith('15') \
            or phone_number.startswith('18')):
        print('是合法的手机号码')
    else:
        print('不是合法的手机号码')
```
如果用正则表达式来实现，代码就简单很多。代码如下：

``` python
import re
phone_number = input('please input your phone number ： ')
if re.match('^(13|14|15|17|18|19)[0-9]{9}$',phone_number):
        print('是合法的手机号码')
else:
        print('不是合法的手机号码')
```
第二种代码是不是超级简单，今天就学习下正则表达式和python的re模块。

> 不管以后你是不是去做python开发，只要你是一个程序员就应该了解正则表达式的基本使用。如果未来你要在爬虫领域发展，你就更应该好好学习这方面的知识。
但是你要知道，re模块本质上和正则表达式没有一毛钱的关系。re模块和正则表达式的关系 类似于 time模块和时间的关系
你没有学习python之前，也不知道有一个time模块，但是你已经认识时间了 12:30就表示中午十二点半（这个时间可好，一般这会儿就该下课了）。
时间有自己的格式，年月日时分秒，12个月，365天......已经成为了一种规则。你也早就牢记于心了。time模块只不过是python提供给我们的可以方便我们操作时间的一个工具而已

###### 正则表达式的语法

> 字符组 ： [字符组]
在同一个位置可能出现的各种字符组成了一个字符组，在正则表达式中用[]表示
字符分为很多类，比如数字、字母、标点等等。
假如你现在要求一个位置"只能出现一个数字",那么这个位置上的字符只能是0、1、2...9这10个数之一。

| 正则 |   待匹配字符  |  匹配结果  | 说明    |
| ---- | --- | --- | --- |
| [0123456789]     |  8   |   True  |在一个字符组里枚举合法的所有字符，字符组里的任意一个字符和"待匹配字符"相同都视为可以匹配     |
|    [0-9]  |   7  |    True |  也可以用-表示范围,[0-9]就和[0123456789]是一个意思   |
|  [a-z]    |   a  |  True   |  同样的如果要匹配所有的小写字母，直接用[a-z]就可以表示   |
|    [A-Z]  | A    |  True   |  [A-Z]就表示所有的大写字母   |
|   [0-9a-fA-F]   |  e   |  True   |  可以匹配数字，大小写形式的a～f，用来验证十六进制符 |
字符：

| 元字符 | 匹配内容                         |
| ------ | -------------------------------- |
| .      | 匹配除换行符以外的任意字符       |
| \w     | 匹配字母或数字或下划线(word)     |
| \s     | 匹配任意的空白符(space)          |
| \d     | 匹配数字(digital)                |
| \n     | 匹配一个换行符                   |
| \t     | 匹配一个制表符                   |
| \b     | 匹配一个单词的结尾               |
| ^      | 匹配字符串的开始，除非在方括号表达式中使用，此时它表示不接受该字符集合。要匹配 ^ 字符本身，请使用 \^。                 |
| $      | 匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 $ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用 \$。   |
| \W     | 匹配非字母或数字或下划线         |
| \D     | 匹配非数字                       |
| \S     | 匹配非空白符                     |
| a|b    | 匹配字符a或字符b                 |
| ()     | 匹配括号内的表达式，也表示一个组 |
| [...]  | 匹配字符组中的字符               |
| [^...] | 匹配除了字符组中字符的所有字符   |

量词：

> 限定符用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 * 或 + 或 ? 或 {n} 或 {n,} 或 {n,m}
> 共6种。

| 量词  | 用法说明         |
| ----- | ---------------- |
| *     | 重复零次或更多次 |
| +     | 重复一次或更多次 |
| ？    | 匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，请使用 \?。 |
| {n}   | 重复n次          |
| {n,}  | 重复n次或更多次  |
| {n,m} | 重复n到m次       |
看一些例子：* + ? { }

***、+限定符都是贪婪的，因为它们会尽可能多的匹配文字，只有在它们的后面加上一个?就可以实现非贪婪或最小匹配。**

例如，您可能搜索 HTML 文档，以查找括在 H1 标记内的章节标题。该文本在您的文档中如下：

==> \<H1>Chapter 1 - 介绍正则表达式\</H1>==

贪婪：下面的表达式匹配从开始小于符号 (<) 到关闭 H1 标记的大于符号 (>) 之间的所有内容。
==/<.*>/==
**非贪婪：如果您只需要匹配开始和结束 H1 标签，下面的非贪婪表达式只匹配 <H1>**
> /<.*?>/

###### ==分组 ()与 或 ｜［^］==
 身份证号码是一个长度为15或18个字符的字符串，如果是15位则全部🈶️数字组成，首位不能为0；如果是18位，则前17位全部是数字，末位可能是数字或x，下面我们尝试用正则来表示：
![enter description here](http://futuretechx.com/wp-content/uploads/2018/11/15426067731.png)
###### ==转义符 \==
在正则表达式中，有很多有特殊意义的是元字符，比如\n和\s等，如果要在正则中匹配正常的"\n"而不是"换行符"就需要对"\"进行转义，变成'\\'。在python中，无论是正则表达式，还是待匹配的内容，都是以字符串的形式出现的，在字符串中\也有特殊的含义，本身还需要转义。所以如果匹配一次"\n",字符串中要写成'\\n'，那么正则里就要写成"\\\\n",这样就太麻烦了。这个时候我们就用到了r'\n'这个概念，此时的正则是r'\\n'就可以了。在字符串之前加r，让整个字符串不转义
###### ==贪婪匹配==
贪婪匹配：在满足匹配时，匹配尽可能长的字符串，默认情况下，采用贪婪匹配
![enter description here](http://futuretechx.com/wp-content/uploads/2018/11/15426070921.png)
几个常用的非贪婪匹配Pattern

> *? 重复任意次，但尽可能少重复
+? 重复1次或更多次，但尽可能少重复
?? 重复0次或1次，但尽可能少重复
{n,m}? 重复n到m次，但尽可能少重复
{n,}? 重复n次以上，但尽可能少重复

.*?的用法

> . 是任意字符
> \* 是取 0 至 无限长度
> ? 是非贪婪模式。
何在一起就是 取尽量少的任意字符，一般不会这么单独写，他大多用在：
.*?x
就是取前面任意长度的字符，直到一个x出现
##### **二 re模块**
常用的方法

 **1. re.compile(pattern, flags=0)**
 编译正则表达式，返回RegexObject对象，然后可以通过RegexObject对象调用match()和search()方法。
``` python
obj = re.compile('\d{3}')  #将正则表达式编译成为一个 正则表达式对象，规则要匹配的是3个数字
ret = obj.search('abc123eeee') #正则表达式对象调用search，参数为待匹配的字符串
print(ret.group())  #结果 ： 123
```
**2. re.search(pattern, string, flags=0)**
在字符串中查找，是否能匹配正则表达式。返回_sre.SRE_Match对象，如果不能匹配返回None。
``` python
ret = re.search('a', 'eva egon yuan').group()
print(ret) #结果 : 'a'
# 函数会在字符串内查找模式匹配,只到找到第一个匹配然后返回一个包含匹配信息的对象,该对象可以
# 通过调用group()方法得到匹配的字符串,如果字符串没有匹配，则返回None。
```
**3. re.match(pattern, string, flags=0)**
从字符串的开头匹配正则表达式。返回_sre.SRE_Match对象，如果不能匹配返回None。
``` python
ret = re.match('a', 'abc').group()  # 同search,不过尽在字符串开始处进行匹配
print(ret)
#结果 : 'a'
```
**4. re.split(pattern, string, maxsplit=0)**
通过正则表达式将字符串分离。如果用括号将正则表达式括起来，那么匹配的字符串也会被列入到list中返回。maxsplit是分离的次数，maxsplit=1分离一次，默认为0，不限制次数。如果字符串不能匹配，将会返回整个字符串的list。

``` python
ret = re.split('[ab]', 'abcd')  # 先按'a'分割得到''和'bcd',在对''和'bcd'分别按'b'分割
print(ret)  # ['', '', 'cd']
```
**5. re.findall(pattern, string, flags=0)**
找到 RE 匹配的所有子串，并把它们作为一个列表返回。这个匹配是从左到右有序地返回。如果无匹配，返回空列表。
``` python
ret = re.findall('a', 'eva egon yuan')  # 返回所有满足匹配条件的结果,放在列表里
print(ret) #结果 : ['a', 'a']
```
**6. re.finditer(pattern, string, flags=0)**
找到 RE 匹配的所有子串，并把它们作为一个迭代器返回。这个匹配是从左到右有序地返回。如果无匹配，返回空列表。

``` python
import re
ret = re.finditer('\d', 'ds3sy4784a')   #finditer返回一个存放匹配结果的迭代器
print(ret)  # <callable_iterator object at 0x10195f940>
print(next(ret).group())  #查看第一个结果
print(next(ret).group())  #查看第二个结果
print([i.group() for i in ret])  #查看剩余的左右结果
Output:
<callable_iterator object at 0x032C85D0>
3
4
['7', '8', '4']
```
**7. re.sub(pattern, repl, string, count=0, flags=0)**
找到匹配的所有子串，并将其用一个不同的字符串替换。可选参数 count 是模式匹配後替换的最大次数；count 必须是非负整数。缺省值是 0 表示替换所有的匹配。如果无匹配，字符串将会无改变地返回。

``` python
ret = re.sub('\d', 'H', 'eva3egon4yuan4', 1)#将数字替换成'H'，参数1表示只替换1个
print(ret) #evaHegon4yuan4
```
**8. re.subn(pattern, repl, string, count=0, flags=0)**
与re.sub方法作用一样，但返回的是包含新字符串和替换执行次数的两元组。

``` python
ret = re.subn('\d', 'H', 'eva3egon4yuan4')#将数字替换成'H'，返回元组(替换的结果,替换了多少次)
print(ret)
Output:
('evaHegonHyuanH', 3)
```
原文参考：[Regular expression operations](https://docs.python.org/3/library/re.html)
注意：
==1 findall的优先级查询：==

``` python
import re

ret = re.findall('www.(baidu|futuretechx).com', 'www.futuretechx.com')
print(ret)  # ['futuretechx']     这是因为findall会优先把匹配结果组里内容返回,如果想要匹配结果,取消权限即可

ret = re.findall('www.(?:baidu|futuretechx).com', 'www.futuretechx.com')
print(ret)  # ['www.futuretechx.com']
Output:
['futuretechx']
['www.futuretechx.com']
```
==2 split的优先级查询==

``` python
ret=re.split("\d+","eva3egon4yuan")
print(ret) #结果 ： ['eva', 'egon', 'yuan']

ret=re.split("(\d+)","eva3egon4yuan")
print(ret) #结果 ： ['eva', '3', 'egon', '4', 'yuan']

#在匹配部分加上（）之后所切出的结果是不同的，
#没有（）的没有保留所匹配的项，但是有（）的却能够保留了匹配的项，
#这个在某些需要保留匹配部分的使用过程是非常重要的。
```
练习题：
1 标签匹配

``` python
import re

ret = re.search("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>")
#还可以在分组中利用?<name>的形式给分组起名字
#获取的匹配结果可以直接用group('名字')拿到对应的值
print(ret.group('tag_name'))  #结果 ：h1
print(ret.group())  #结果 ：<h1>hello</h1>

ret = re.search(r"<(\w+)>\w+</\1>","<h1>hello</h1>")
#如果不给组起名字，也可以用\序号来找到对应的组，表示要找的内容和前面的组内容一致
#获取的匹配结果可以直接用group(序号)拿到对应的值
print(ret.group(1))
print(ret.group())  #结果 ：<h1>hello</h1>
Output:
h1
<h1>hello</h1>
h1
<h1>hello</h1>

ret = re.search("<(?P<tag_name>\w+)>(?P<context>\w+)</(?P=tag_name)>","<h1>hello</h1>")
print(ret.group('tag_name'))
print(ret.group('context'))
print(ret.group())
Output:
h1
hello
<h1>hello</h1>
```
2、匹配整数

``` python
import re

ret=re.findall(r"\d+","1-2*(60+(-40.35/5)-(-4*3))")
print(ret) #['1', '2', '60', '40', '35', '5', '4', '3']
ret=re.findall(r"-?\d+\.\d*|(-?\d+)","1-2*(60+(-40.35/5)-(-4*3))")
print(ret) #['1', '-2', '60', '', '5', '-4', '3']
ret.remove("")
print(ret) #['1', '-2', '60', '5', '-4', '3']
```

3、数字匹配 
> 1、 匹配一段文本中的每行的邮箱
     http://blog.csdn.net/make164492212/article/details/51656638
> 2、 匹配一段文本中的每行的时间字符串，比如：‘1990-07-12’；
	>    分别取出1年的12个月（^(0?[1-9]|1[0-2])$）、   
	>  一个月的31天：^((0?[1-9])|((1|2)[0-9])|30|31)$
> 3、 匹配qq号。(腾讯QQ号从10000开始)  ［1,9］[0,9]{4,}
> 4、 匹配一个浮点数。       ^(-?\d+)(\.\d+)?$   或者  -?\d+\.?\d*
> 5、 匹配汉字。             ^[\u4e00-\u9fa5]{0,}$ 
> 6、 匹配出所有整数

4、爬虫练习

``` python
import re
import json
from urllib.request import urlopen

def getPage(url):
    response = urlopen(url)
    return response.read().decode('utf-8')

def parsePage(s):
    com = re.compile(
        '<div class="item">.*?<div class="pic">.*?<em .*?>(?P<id>\d+).*?<span class="title">(?P<title>.*?)</span>'
        '.*?<span class="rating_num" .*?>(?P<rating_num>.*?)</span>.*?<span>(?P<comment_num>.*?)评价</span>', re.S)

    ret = com.finditer(s)
    for i in ret:
        yield {
            "id": i.group("id"),
            "title": i.group("title"),
            "rating_num": i.group("rating_num"),
            "comment_num": i.group("comment_num"),
        }


def main(num):
    url = 'https://movie.douban.com/top250?start=%s&filter=' % num
    response_html = getPage(url)
    ret = parsePage(response_html)
    print(ret)
    f = open("move_info7", "a", encoding="utf8")

    for obj in ret:
        print(obj)
        data = str(obj)
        f.write(data + "\n")

count = 0
for i in range(10):
    main(count)
    count += 25
```
思路：
1 url从网页上把代码搞下来
2 bytes decode ——> utf-8 网页内容就是我的待匹配字符串 ret = re.findall(正则，带匹配的字符串)  
3 ret是所有匹配到的内容组成的列表
结果：会爬取豆瓣top250个电影，然后保存到jason文件中，文件的内容类似这样的：
``` json
{"id": "1", "title": "肖申克的救赎", "rating_num": "9.6", "comment_num": "1197203人"}
{"id": "2", "title": "霸王别姬", "rating_num": "9.6", "comment_num": "879286人"}
{"id": "3", "title": "这个杀手不太冷", "rating_num": "9.4", "comment_num": "1102451人"}
{"id": "4", "title": "阿甘正传", "rating_num": "9.4", "comment_num": "943674人"}
{"id": "5", "title": "美丽人生", "rating_num": "9.5", "comment_num": "551561人"}
{"id": "6", "title": "泰坦尼克号", "rating_num": "9.3", "comment_num": "879550人"}
{"id": "7", "title": "千与千寻", "rating_num": "9.3", "comment_num": "872969人"}
{"id": "8", "title": "辛德勒的名单", "rating_num": "9.5", "comment_num": "495794人"}
{"id": "9", "title": "盗梦空间", "rating_num": "9.3", "comment_num": "962117人"}
{"id": "10", "title": "机器人总动员", "rating_num": "9.3", "comment_num": "637442人"}
{"id": "11", "title": "忠犬八公的故事", "rating_num": "9.3", "comment_num": "621782人"}
{"id": "12", "title": "三傻大闹宝莱坞", "rating_num": "9.2", "comment_num": "860609人"}
{"id": "13", "title": "海上钢琴师", "rating_num": "9.2", "comment_num": "719303人"}
{"id": "14", "title": "放牛班的春天", "rating_num": "9.2", "comment_num": "591340人"}
{"id": "15", "title": "大话西游之大圣娶亲", "rating_num": "9.2", "comment_num": "652023人"}
{"id": "16", "title": "楚门的世界", "rating_num": "9.2", "comment_num": "632497人"}
{"id": "17", "title": "教父", "rating_num": "9.2", "comment_num": "436290人"}
{"id": "18", "title": "龙猫", "rating_num": "9.1", "comment_num": "534644人"}
{"id": "19", "title": "星际穿越", "rating_num": "9.2", "comment_num": "648404人"}
```
flags 详解：

> flags有很多可选值：
re.I(IGNORECASE)忽略大小写，括号内是完整的写法
re.M(MULTILINE)多行模式，改变^和$的行为
re.S(DOTALL)点可以匹配任意字符，包括换行符
re.L(LOCALE)做本地化识别的匹配，表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境，不推荐使用
re.U(UNICODE) 使用\w \W \s \S \d \D使用取决于unicode定义的字符属性。在python3中默认使用该flag
re.X(VERBOSE)冗长模式，该模式下pattern字符串可以是多行的，忽略空白字符，并可以添加注释