## 题目描述
给定一个字符串 `s` ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

**示例 1：**

```js
输入：s = "Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"
```


**示例 2:**

```js
输入： s = "God Ding"
输出："doG gniD"
```

> 力扣原题目地址：https://leetcode.cn/problems/reverse-words-in-a-string-iii/

## 思路分析
### 解法一 字符串转数组反转再转字符串
这种方式可读性可以，即先把字符串以空格为分割转换成数组，然后map循环把数组中的每一项（字符串），再转成一个个的小数组反转再转回来即可。代码如下：
```js
let s = "abc fgg kks"
var reverseWords = function (s) {
    let arr = s.split(' ') // 此时arr为： ['abc', 'fgg', 'kks']
    arr = arr.map((item) => {
        return item.split('').reverse().join('') // 把每一项字符串转成数组，在反转颠倒，再转回字符串
    }) // map循环加工以后的arr为： ['cba', 'ggf', 'skk']
    return arr.join(' ') // 最终变成：'cba ggf skk'
};
```

### 解法二 倒叙遍历分堆数组头部追加
思路：

比如：`let s = "abc fgg kks"`，当我们倒叙遍历的时候，就会得到`'k','k','s',' ','g','g','f',' ','c','b','a'`，此时我们发现，反转的确是反转了，不过位置也反转了。

于是乎，我们有这样一个思路：分堆数组头部追加，这个案例，我们以空格为界限把字符串分为三堆，分别是：`abc`一堆、`fgg`一堆、`kks`一堆。当我们倒叙遍历的时候，就会拿到反转的三堆`skk`、`ggf`、`abc`，拿到一堆，就把这一堆给头部追加`unshift`到数组中去，这样的话，每一堆的顺序还是不变的。然后再转成字符串即可。代码如下：


```js
let s = "abc fgg kks"
var reverseWords = function (ss) {
    // 1. 因为是以空格为区分，见到空格说明这一堆字符串完毕，才会装填到数组中去，所以这里手动再在原有字符串头部添加一个空格，用于区分一堆一堆的结束
    let s = ' ' + ss
    let arr = [] // 2. 创建一个数组用于装填一堆一堆的字符串
    let tempStr = '' // 3. 由于遍历得到的是一个一个的字符串，所以再创建一个空字符串用于拼接'堆'
    for (let i = s.length - 1; i >= 0; i--) {
        // 4. 倒叙第一个肯定不是空格的，所以看else语句中的代码
        if (s[i] === ' ') { // 5. 当遇到空格的时候，说明这一堆结束啦，一堆结束，那就一堆装填呗
            arr.unshift(tempStr)  // 6. 把这一堆数据头部追加到数组中，这样位置就不会变化咯
            arr.unshift(' ') // 7. 注意这里需要手动再头部追加一个空格，因为原来的字符串也有空格哦，空格不能漏掉哦
            tempStr = '' // 8. 然后把用于拼接堆的字符串重置，以便继续重新拼接堆
        } else {
            tempStr = tempStr + s[i] // 5.1 拼接归拢一堆，注意因为是反转，所以是tempStr + s[i]拼接，不要写反了哦
        }
    }
    console.log('数组', arr); // 9. 打印看看 [' cba', ' ggf', ' skk']
    return arr.join('').trimStart() // 10. 转成字符串以后，别忘了把左侧的空格给去掉哦
};
console.log(  reverseWords(s)  );
```

### 解法三 倒叙遍历分堆字符串头部追加
这个和上方类似，就是把一堆一堆的字符串使用字符串头部拼接（原来是使用数组头部追加，再转字符串），代码如下：

```js
var reverseWords = function (ss) {
    let s = ' ' + ss // 1. 加空格做区分一堆一堆的字符串
    let resultStr = '' // 2. 结果字符串
    let tempStr = '' // 3. 拼接堆字符串
    for (let i = s.length - 1; i >= 0; i--) {
        if (s[i] === ' ') { // 4. 倒叙第一个肯定不是空格
            resultStr = ' ' + tempStr + resultStr // 6. 把拼接好的一堆字符串加到结果字符串上
            tempStr = '' // 7. 然后把拼接字符串重置为原来的状态
        } else {
            tempStr = tempStr + s[i] // 5. 一堆字符串拼接
        }
    }
    return resultStr.trimStart() // 8. 返回结果的时候，也要注意去除左侧之前手动添加的空格哦
};
```
> 好记性不如烂笔头，记录一下吧^_^