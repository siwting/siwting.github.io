### re模块是一个强大的正则表达式的模块，当其配合切片操作的时候威力更加强大
#1.re模块来匹配字符串
``` python
import re #导入re模块才能使用re中的功能

text = "The Attila the Hun Show"

# a single character 单个字符
m = re.match(".", text)
if m: print repr("."), "=>", repr(m.group(0))

# any string of characters 任何字符串
m = re.match(".*", text)
if m: print repr(".*"), "=>", repr(m.group(0))

# a string of letters (at least one) 只包含字母的字符串(至少一个)
m = re.match("\w+", text)
if m: print repr("\w+"), "=>", repr(m.group(0))

# a string of digits 只包含数字的字符串
m = re.match("\d+", text)
if m: print repr("\d+"), "=>", repr(m.group(0))

*B* '.' => 'T'
'.*' => 'The Attila the Hun Show'
'\\w+' => 'The'*b*
```
  search 函数会字符串内查找匹配，它在所有可能的位置上进行匹配，从最左边开始进行查找，一旦找到，返回第一个匹配的值，没有找到匹配值就返回Null

#2.使用 re 模块抽出匹配的子字符串
```python

import re

text ="10/15/99"

m = re.match("(\d{2})/(\d{2})/(\d{2,4})", text)
if m:
    print m.group(1, 2, 3)

*B*('10', '15', '99')*b*
```
