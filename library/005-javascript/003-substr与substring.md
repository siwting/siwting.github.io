1. substr

> 用于提取字符串中介于两个指定下标之间的字符

``` js

substring(start,end)

```

- 开始和结束的位置，从零开始的索引
>  - substring 方法返回的子串包括 start 处的字符，但不包括 end 处的字符。
- 如果 start 与 end 相等，那么该方法返回的就是一个空串（即长度为 0 的字串）。
- 如果 start 比 end 大，那么该方法在提取子串之前会先交换这两个参数。
- 如果 start 或 end 为负数，那么它将被替换为 0。

2. substring

> 用于返回一个从指定位置开始的指定长度的子字符串

``` js

stringObject.substr(start [, length ])

```

> - start   必需。所需的子字符串的起始位置。字符串中的第一个字符的索引为 0。
 - length 可选。在返回的子字符串中应包括的字符个数。
- 如果start为负数，则start=str.length+start。
- 如果 length 为 0 或负数，将返回一个空字符串。
- 如果没有指定该参数，则子字符串将延续到stringObject的最后。
