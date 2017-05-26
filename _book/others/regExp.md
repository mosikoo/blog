## 正则表达式

#### 非贪婪模式
```
var str = "aaab",
    reg1 = /a+/, //贪婪模式
    reg2 = /a+?/;//非贪婪模式
console.log(str.match(reg1)); //["aaa"], 由于是贪婪模式, 捕获了所有的a
console.log(str.match(reg2)); //["a"], 由于是非贪婪模式, 只捕获到第一个a
```

#### 分组
通过小括号来实现

```
/(abc)+/.test("abc123") == true
```

#### 捕获性分组

js中主要是通过 $+编号 或者 \+编号 表示法进行引用

```
var url = "www.google.google.com";
var re = /([a-z]+)\.\1/;
console.log(url.replace(re,"$1"));//"www.google.com"
```

#### 非捕获性分组

(?:\d+) 表示一个非捕获性分组
由于分组不捕获任何内容, 所以, RegExp.$1 就指向了空字符串.

#### 命名分组

语法: (? …)

### 零宽断言

|字符|描述|示例|
|--|--|---|
|(?:pattern)|非捕获性分组, 匹配pattern的位置, 但不捕获匹配结果.也就是说不创建反向引用, 就好像没有括号一样.|‘abcd(?:e)匹配’abcde|
| (?=pattern) |顺序肯定环视, 匹配后面是pattern 的位置, 不捕获匹配结果.|‘Windows (?=2000)’匹配 “Windows2000” 中的 “Windows”; 不匹配 “Windows3.1” 中的 “Windows”|
|(?!pattern)|顺序否定环视, 匹配后面不是 pattern 的位置, 不捕获匹配结果.|‘Windows (?!2000)’匹配 “Windows3.1” 中的 “Windows”; 不匹配 “Windows2000” 中的 “Windows”|
|(?<=pattern)|逆序肯定环视, 匹配前面是 pattern 的位置, 不捕获匹配结果.|‘(?<=Office)2000’匹配 “ Office2000” 中的 “2000”; 不匹配 “Windows2000” 中的 “2000”|
|(?<!pattern)|逆序否定环视, 匹配前面不是 pattern 的位置, 不捕获匹配结果.|‘(?<!Office)2000’匹配 “ Windows2000” 中的 “2000”; 不匹配 “ Office2000” 中的 “2000”|

##### avaScript 中只支持前两种, 也就是只支持 顺序肯定环视 和 顺序否定环视.

##### exec与match的区别

1、当正则表达式无子表达式，并且定义为非全局匹配时，exec和match执行的结果是一样，均返回第一个匹配的字符串内容；

2、当正则表达式无子表达式，并且定义为全局匹配时，exec和match执行，做存在多处匹配内容，则match返回的是多个元素数组；

3、当正则表达式有子表示时，并且定义为非全局匹配，exec和match执行的结果是一样如上边的第5种情况；

4、当正则表达式有子表示时，并且定义为全局匹配，exec和match执行的结果不一样，此时match将忽略子表达式，只查找全匹配正则表达式并返回所有内容;

也就说，**exec与全局是否定义无关系，而match则于全局相关联，当定义为非全局，两者执行结果相同**
