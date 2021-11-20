# 编码/字节流

现在最大最全的字符集就是unicode,unicode仅仅是做字符的标识。你可以将世界上所有的字符收集起来然后0,1,2,3索引排序放入数组中，但是排序方式每个人都不一样。所以就出了一套规范，大家都来遵守，那就是unicode也叫"万国码"，看这名字就觉得很大很全。

什么是字符：包括数字,字母,符号，汉字等等的组成。

如果你想知道一个字符在unicode中的位置，那么可以用js的charCodeAt函数。下面说几个会用到的字符. 【a->97,中->20013】97,20013可以理解为规范的排名，索引都行。为了统一描述，后面使用官方命名 "码点"code point,码点是一个由标准的Unicode编码规定的数字。所以码点20013就是汉字“中”,97就是字母“a”,你会发现97也是ascii的对应码点(暂时记住，unicode可兼容ascii)。

知道了每个字符的码点位置，那还需要计算机认识才行。计算机只认二进制，也就是0和1，你跟他说"a",计算机也不清楚是啥。 单位是字节byte,1字节有8位。足够可以描述0-255范围内的码点。也就是2^8,256个字符。00000000->11111111变化范围就是0-255。可以使用[进制转换](https://www.sojson.com/hexconvert/64to10.html) 自行查看，后面会常用。

因为计算机是一个字节一个字节的。所以1100001(97)代表字符a，(1100001 1100001 97 97)那么是两个a a呢，还是♅字符呢,其实都行的通，你只要能统一存储方式规则与读取方式规则就行。下面介绍UTF中大名鼎鼎的UTF-8，具体的规则如下。

| unicode码点对应位数 | 码点起值                 | 码点止值                    | 对应UTF-8字节 | UTF-8规则(套模板)                                          |          |
| ------------- | -------------------- | ----------------------- | --------- | ----------------------------------------------------- | -------- |
| 7位(1字节)       | U+0000      0        | U+007F     127          | 1字节       | 0xxxxxxx                                              |          |
| 11位(2字节       | U+0080      128      | U+07FF      2047        | 2字节       | 110xxxxx 10xxxxxx                                     |          |
| 16位(2字节)      | U+0800      2048     | U+FFFF      65535       | 3字节       | 1110xxxx 10xxxxxx 10xxxxxx                            | 1        |
| 21位(3字节)      | U+10000      65536   | U+1FFFFF    2097151     | 4字节       | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx                   | 10xxxxxx |
| 26位(4字节)      | U+200000  2097152    | U+3FFFFFF   67108863    | 5字节       | 111110xx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx          | 10xxxxxx |
| 31位(4字节)      | U+4000000   67108864 | U+7FFFFFFF   2147483647 | 6字节       | 1111110x 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx 10xxxxxx | 10xxxxxx |

最大2^31-1=  也就是21亿。基本上地球上的字符是没问题了，全宇宙的话，多少有点悬。

怎么个套法呢.a字符97，在7位内，对应UTF-8 一个字节。转化为二进制1100001，只有7位，刚好对应UTF-8一个字节的0xxxxxxx中的x,组合起来就是01100001。

汉字"中",20013。在16位内，使的UTF-8就是3字节模板。转化为二进制100111000101101，一点点往模板的x里面套。从后向前。11100100 10111000 10101101,最前面不够的x用0放入。 这个东西，计算机就能看懂了,下面就告诉计算机一个模板规则，UTF-8的，这样上面的二进制对应的就是二进制数组(这数组认10进制了)\[228,184,173]。这样就能从二进制数组里面解出"中"字。

```
//中-》 [231, 160, 129] 
 const bf = new ArrayBuffer(3);//设置大小
 const view = new DataView(bf);//创建视图
 view.setUint8(0,228);
 view.setUint8(1,184);
 view.setUint8(2,173);
 const de = decode.decode(bf);//解析
 console.log(de);
```