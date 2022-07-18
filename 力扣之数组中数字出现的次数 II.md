## 题目描述

在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

**示例 1：**

```
输入： nums = [3,4,3,3]
输出： 4
```

**示例 2：**

```
输入： nums = [9,1,7,9,7,9,7]
输出： 1
```

> 力扣原题目地址：https://leetcode.cn/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-ii-lcof/

## 思路分析

### 使用对象统计数字出现的次数

思路分两步：

-   **第一步，统计值出现的次数**
-   **第二步，看看谁出现的次数是一次**

```js
var singleNumber = function (nums) {
    /** 第一步 统计值出现的次数 */
    let obj = {} // 1. 定义一个对象用来存储 '谁' 出现了 '几次'
    for (let i = 0; i < nums.length; i++) {
        if (nums[i] in obj) { // 2. 若对象中有，就把对应次数加一，当然一开始是没有的
            obj[nums[i]] = obj[nums[i]] + 1 // 4. 比如{9:1} 变成了 {9:2} 由原来的一次，变成两次了
        } else {
            obj[nums[i]] = 1 // 3. 本来是空对象，现在变成{9:1} 后续若还出现9 就次数加一，变成{9:2}
        }
    }
    /** 第二步，看看谁出现的次数是一次 */
    for (const key in obj) {
        if (obj[key] == 1) {
            return key
        }
    }
};
```

注意obj计数：我们以`nums = [3,4,3,3]`为例，那么obj就变成了`{3:3,4:1}`

### 使用Map集合统计数字出现的次数

这种方式和对象方式思路类似，都是存储对应项出现的次数，代码如下：

```js
var singleNumber = function (nums) {
    /** 第一步 统计值出现的次数 */
    let map = new Map() // 1. 定义一个Map集合来存储
    for (let i = 0; i < nums.length; i++) {
        if (map.has(nums[i])) { // 2. 若Map集合中有，就把对应次数加一，当然一开始是没有的
            let count = map.get(nums[i]) // 4. 有的话，先看看次数是几
            map.set(nums[i], count + 1) // 5. 然后看把次数加上一即可
        } else {
            map.set(nums[i], 1) // 3. 没有就存一份
        }
    }
    /** 第二步，看看谁出现的次数是一次 */
    for (const [key, value] of map.entries()) { // 6. 最后再遍历看看谁出现的次数是1
        if (value == 1) {
            return key // 就把谁返回即可
        }
    }
};
```

大家想一下，能不能用数组去记录谁出现的次数呢？其实这里也可以使用数组去解决。不过需要搞二维数组，因为有两个变量，`谁、对应谁出现的次数` 大家可以按照这个思路尝试一下，大同小异

### 判断当前项是否不在余下的数组中出现

思路很简单，举例：原数组`[6,5,5,5]` 把原数组拆分成`[6],[5,5,5,]` 看看`6`是否在余下的数组中又出现了，如果没有在余下的数组中又出现了，说明`6`只出现了一次，说明我们要找到就是这个`6`。代码如下：

```js
var singleNumber = function (nums) {
    for (let i = 0; i < nums.length; i++) {
        let copyNums = nums.slice(0) // 1. 拷贝一份原数组
        copyNums.splice(i, 1) // 2. 拿到除去当前项以外余下项组成的数组
        let remainNums = copyNums // 3. 得到余下的数组
        if (!remainNums.includes(nums[i])) { // 4. 若当前项在余下的数组中没有再一次出现
            return nums[i] // 5. 说明就是它啦
        }
    }
};
```

不过这种方式不太好，数组的方法使用的有点多，所以性能不太高。这里提一下，只是给大家增加点思路

### 判断当前项的开始索引是否等于结束索引

思路：就当前题目而言，可以这样定义其中规律，例数组：`[5,5,6,5]`。因为`6`只有一个，所以`6`头一次出现的位置索引是`2`最后一次出现的索引也是`2` 所以判断某一项头一次出现的位置，是否等于某一项最后一次出现的位置即可，代码如下：

```js
var singleNumber = function (nums) {
    for (let i = 0; i < nums.length; i++) {
        let firstTimeIndex = nums.indexOf(nums[i]) // 1. 找到首次出现的位置
        let lastTimeIndex = nums.lastIndexOf(nums[i]) // 2. 找到最后一次出现的位置
        if (firstTimeIndex == lastTimeIndex) { // 3. 若 首次等于最后 则就找到啦
            return nums[i] // 4. 找到返回之即可
        }
    }
};
```

这种方式稍微好一点，不过笔者认为最好的方式，还是使用Map集合的方式，效率高、执行用时和内存消耗都还不错`^_^`