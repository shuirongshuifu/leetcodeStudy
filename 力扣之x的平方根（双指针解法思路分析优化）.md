## 题目描述

给你一个非负整数 `x` ，计算并返回 `x` 的 **算术平方根** 。

由于返回类型是整数，结果只保留 **整数部分** ，小数部分将被 **舍去 。**

**注意：** 不允许使用任何内置指数函数和算符，例如 `pow(x, 0.5)` 或者 `x ** 0.5` 。

**示例 1：**

```
输入： x = 4
输出： 2
```

**示例 2：**

```
输入： x = 8
输出： 2
解释： 8 的算术平方根是 2.82842..., 由于返回类型是整数，小数部分将被舍去。
```

**提示：**

- `0 <= x <= 231 - 1`

> 力扣原题目地址：https://leetcode.cn/problems/sqrtx/description/

## 思路解法

### 分析

- 求一个数的平方根的值，由数学常识知，那么这个值，一定是小于等于这个数的，如：
- 0的平方根为0，等于0
- 1的平方根为1，等于1
- 2的平方根约为1.414，小于2
- 3的平方根约为1.732，小于3
- 4的平方根为2，小于4
- ......
- x的平方根为m，m小于等于x
- 所以x的平方根的值，一定是介于0和x之间的一个数

所以，我们可以使用双指针的方式，定义两个指针，左边指针为0，右边指针为x，然后通过中位数的积，去与x的值进行判断，等于说明直接找到了；大于x那就把右侧指针往左移动，即减小减一；小于x那就把左侧指针往右移动，即增大加一；

如下代码：

### 实现方式一（耗时略长）

```js
var mySqrt = function (x) {
    let left = 0 // 左侧指针初始为0
    let right = x // 右侧指针初始为x本身
    while (left <= right) { // 只要left小于等于right就一直执行，直至大于才停止
            // 保留整数部分，以9为例，中位数4.5保留4即可
            let middle = Math.floor((left + right) / 2) 
            if (middle * middle == x) { // 若等于则是刚好找到
                    return middle // 刚好找到则直接返回即可
            } else if (middle * middle > x) { // 若大于超过了
                    right = right - 1 // 那就减小一些
            } else if (middle * middle < x) { // 若小于为达到
                    left = left + 1 // 那就增大一些
            }
    }
    return right // 返回指针值即为平方根（四舍五入）
};
```

### 提交截图（超出时间限制）

写完以后，我们直接提交LeetCode，发现：超时了！！！

![12321.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f0d372a126d54cccaab7a661eb6b2c3d~tplv-k3u1fbpfcp-watermark.image?)

实际上这种写法，对于比较小的x的值来说，是完全没有问题的，但是对于比较大的值来说，运算需要很长时间。因为`right = right - 1`和`left = left + 1`。一次减少一个，这个缩小范围太慢。

因为LeetCode对于时间有要求，所以直接给出一个警告：**超出时间限制**

那么当这个测试用例X等于2147395600时，用上述的方式，需要多长时间才能运算出来呢？如下图：

![456.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/676ac66eb33d4d9abae9a4b73947d577~tplv-k3u1fbpfcp-watermark.image?)

所以到这里，这个题目也算是解决了，但是时间过长，接下来需要做的就是优化，优化，优化

### 实现方式二（优化时间）

既然每次`right = right - 1`和`left = left + 1`缩小范围太小了，那我们就把缩小范围扩大一些。直接`right = middle - 1以及left = middle + 1`即可，我们以中位数`middle`为基准，这样就减少了很多不必要的运算。

如下代码：

```js
var mySqrt = function (x) {
    let left = 0
    let right = x
    while (left <= right) {
        let middle = Math.floor((left + right) / 2)
        if (middle * middle == x) {
            return middle
        } else if (middle * middle > x) {
            right = middle - 1 // 因为中位数右侧的数值肯定是不符合要求的，干脆直接跳过
        } else if (middle * middle < x) {
            left = middle + 1 // 同上类似
        }
    }
    return right
};
```
这样的话，就减少很多不必要的运算了，这样就能够彻底的完成任务了，如下图：

### 提交截图（搞定啦...）

![789.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/84942f18e5f2417ca45f29681da27b6a~tplv-k3u1fbpfcp-watermark.image?)

## 总结

我们在LeetCode刷题的时候，一次不能直接写出答案也没事的，我们可以先写出一个差一些的答案，然后再优化，最终解决问题。

即为：`曲线救国`的大概意思吧

以上就是笔者的`力扣之x的平方根`的解题思路