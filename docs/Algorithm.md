
> 简书地址: https://www.jianshu.com/p/371000cece4b

### 1. 动态规划

> 与递归相反的技术。
`递归`是从**顶部**开始将问题分解，通过解决掉所有分解出小问题的方式，来解决整个问题。**代码简洁，但效率不高**。
>
> 如计算`斐波那契数列`,存在很多值在递归调用中被**重复计算**。
>
> `动态规划方案`从**底部**开始解决问题，将所有小问题解决掉，然后合并成一个整体解决方案，从而解决掉整个大问题。
>
> `动态规划方案`通常会使用一个数组来建立一张表，用于存放被分解成众多子问题的解。
当算法执行完毕，最终的解将会在这个表中很明显的地方被找到。

(1) 例一: 计算斐波那契数列
> 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, ...

`递归方案`实现:

```js
function recurFib(n) {
  if (n < 2) {
    return n
  } else {
    return recurFib(n - 1) + recurFib(n - 2)
  }
}
```

`动态规划方案`实现:

```js
function dynFib(n) {
  if (n === 0) return 0
  if (n <= 2) return 1
  const rs = []
  for (let i = 0; i <= n; i++) {
    rs[i] = 0
  }
  rs[1] = 1
  rs[2] = 1
  for (let i = 3; i <= n; i++) {
    // 当前值为前两个值之和
    rs[i] = rs[i - 1] + rs[i - 2]
  }
  return rs[n]
}
```

测试:

```js
console.time('递归计算斐波那契数列')
var result1 = recurFib(40)
console.log(result1)
console.timeEnd('递归计算斐波那契数列')

console.time('动态规划斐波那契数列')
var result2 = dynFib(40)
console.log(result2)
console.timeEnd('动态规划斐波那契数列')
```

多执行几次，得出测试结果：

```js
递归计算斐波那契数列: 1730.80322265625ms
动态规划斐波那契数列: 0.30810546875ms

递归计算斐波那契数列: 2223.50390625ms
动态规划斐波那契数列: 0.34814453125ms

递归计算斐波那契数列: 1730.210693359375ms
动态规划斐波那契数列: 0.176025390625ms
```

可以看出：`动态规划斐波那契数列`比`递归计算斐波那契数列`效率高很多。

(2) 例二: 寻找两个字符串的公共子串（匹配同一位置）

`暴力方式`实现:
> 给出两个字符串A和B，
我们可以通过从A的第一个字符开始与B对应的**每一个字符**进行比对的方式找到他们的最长公共子串;
如果此时没有找到匹配的字母，则移动到A的第二个字符处，然后从B的第一个字符开始匹配，依次类推。

```js
function lcs(str1, str2) {
  const len1 = str1.length
  const len2 = str2.length
  let rs = ''
  for (let i = 0; i < len1; i++) {
    if (str1[i] === str2[i]) {
      let m = i
      let str = ''
      while (m < len1 && m < len2 && str1[m] === str2[m]) {
        str += str1[m]
        m++
      }
      if (rs.length < str.length) {
        rs = str
      }
    }
  }
  return rs
}
```

`动态规划方式`实现:

```js
function dynLcs(str1, str2) {
  const len1 = str1.length
  const len2 = str2.length
  let maxLen = 0	// 存储最长子串的长度
  let index = 0	// 存储最长子串的最后一个字符的索引
  const lcsarr = new Array(len1 + 1) // 初始化一个二维数组，来分别存储两个字符串的索引值
  for (let i = 1; i < len1 + 1; i++) {
    lcsarr[i] = new Array(len2 + 1) 
    for (let j = 1; j < len2 + 1; j++) {
      lcsarr[i][j] = 0
      if (str1[i - 1] !== str2[j - 1] || i !== j) {
        continue
      }
      // 如果两个字符串相应位置的字符进行了匹配，当前数组元素的值将被设置为前一次循环中数组元素保存的值加一
      lcsarr[i][j] = lcsarr[i - 1][j - 1] + 1
      if (maxLen < lcsarr[i][j]) {
        maxLen = lcsarr[i][j]
        index = i
      }
    }
  }
  
  // 构建最长子串
  let str = ''
  for (let i = 0; i < maxLen; i++) {
    str += str2[index - maxLen + i]
  }
  return str
}
```

测试:

```js
// 模拟数据
const arr = "abcdefghijklmnopqrstuvwxyz".split('')
function initData (nums) {
  let str = []
  for (let i = 0; i < nums; i++) {
      index = Math.floor(Math.random() * (arr.length + 1))
      str += arr[index] || arr[index - 1]
  }
  return str
}
var str1 = initData(1000)
var str2 = initData(1000)

// test
console.time('自定义lcs')
var result1 = lcs(str1, str2)
console.log(result1)	// ye
console.timeEnd('自定义lcs')

console.time('动态规划lcs')
var result2 = dynLcs(str1, str2)
console.log(result2)	// ye
console.timeEnd('动态规划lcs')
```

多执行几次，得出测试结果：

```js
自定义lcs: 0.76904296875ms
动态规划lcs: 46.73095703125ms

自定义lcs: 0.77197265625ms
动态规划lcs: 44.8828125ms

自定义lcs: 0.296142578125ms
动态规划lcs: 59.943115234375ms
```

可以看出：`自定义lcs`比`动态规划lcs`效率高很多。
**注:** 查了很多资料是说`动态规划lcs`要比`自定义lcs`效率高,但是经过自己测试后却是反的,不晓得是不是哪里出问题了ㄟ( ▔, ▔ )ㄏ

(3) 例三: 背包问题
> 试想你是一个保险箱大盗，打开了一个装满奇珍异宝的保险箱，但是你必须将这些宝贝放入你的一个小背包中
保险箱中的物品规格和价值不同，你希望自己的背包装进的宝贝总价值最大
> **关键思路**：计算装入背包的每一个物品的最大价值，直到背包装满

`递归方法`实现:

```js
function knapsack(capacity, size, value, n) {
  if (capacity === 0 || n === 0) {
    return 0
  }
  if (size[n - 1] > capacity) {
    return knapsack(capacity, size, value, n - 1)
  } else {
    const val1 = value[n - 1] + knapsack(capacity - size[n - 1], size, value, n - 1)
    const val2 = knapsack(capacity, size, value, n - 1)
    return val1 > val2 ? val1 : val2
  }
}
```

`动态规划方法`实现:

```js
function dKnapsack(capacity, size, value, n) {
  let K = []
  // 初始化一个二维数组，来分别存储物品个数和背包容量
  for (let i = 0; i < capacity + 1; i++) {
    K[i] = []
  }
  for (let i = 0; i <= n; i++) {
    for (let j = 0; j <= capacity; j++) {
      // j 为背包容量
      if (i === 0 || j === 0) {
        // 数组的第一个元素总被设置为0
        K[i][j] = 0
      } else if (size[i - 1] <= j) {
        const val1 = value[i - 1] + K[i - 1][j - size[i - 1]]
        const val2 = K[i - 1][j]
        K[i][j] = val1 > val2 ? val1 : val2
      } else {
        K[i][j] = K[i - 1][j]
      }
    }
  }
  return K[n][capacity]
}
```

测试:

```js
const capacity = 16 // 背包容积
const n = 5     // 保险箱中的物品数
const size = [3, 4, 7, 8, 9]    // 保险箱里的物品尺寸
const value = [4, 5, 10, 11, 13]    // 保险箱里的物品价值

console.time('递归解决knapsack')
const maxValue1 = knapsack(capacity, size, value, n)
console.log(maxValue1)	
console.timeEnd('递归解决knapsack')

console.time('动态规划解决knapsack')
const maxValue2 = dKnapsack(capacity, size, value, n)
console.log(maxValue2)	
console.timeEnd('动态规划解决knapsack')
```

得出测试结果：

```
递归解决knapsack: 0.77392578125ms
动态规划解决knapsack: 0.39697265625ms
```

可以看出：`动态规划解决knapsack`效率高于`递归解决knapsack`

### 2. 贪心算法
> 总是选择当下的**最优解**，而不去考虑这一次的选择会不会对未来的选择造成影响。
使用贪心算法通常表明，实现者希望做出的这一系列**局部“最优”**选择能够带来最终**整体“最优”**选择
如果是这样的话，该算法将会产生一个最优解，否则，则会得到一个次优解。

(1) 例一: 找零问题
> 你从商店购买了一些商品，找零 63 美分，店员要 怎样给你这些零钱呢

```js
function mackChange(origAmt) {
  const coins = []
  if (origAmt % .25 < origAmt) {
    coins[3] = parseInt(origAmt / .25)
    origAmt = origAmt % .25
    console.log(`25 美分的数量 - ${coins[3] } - $${coins[3] * .25}`)
  }
  if (origAmt % .1 < origAmt) {
    coins[2] = parseInt(origAmt / .1)
    origAmt = origAmt % .1
    console.log(`10 美分的数量 - ${coins[2] } - $${coins[2] * .1}`)
  }
  if (origAmt % .05 < origAmt) {
    coins[1] = parseInt(origAmt / .05)
    origAmt = origAmt % .05
    console.log(`5 美分的数量 - ${coins[1] } - $${coins[1] * .05}`)
  }
  coins[0] = parseInt(origAmt / .01)
  console.log(`1 美分的数量 - ${coins[0] } - $${coins[0] * .01}`)
}

// test
mackChange(.63)
// 25 美分的数量 - 2 - $0.5
// 10 美分的数量 - 1 - $0.1
// 1 美分的数量 - 3 - $0.03
```

(2) 例二: 贪心算法解决背包问题

```js
function ksack(capacity, size, value, n) {
  let load = 0	// 已放进背包的容量
  let i = 0		// 放进背包的物品个数
  let maxValue = 0	// 最大价值
  while (load < capacity && i < n) {
    if (size[i] <= (capacity - load)) {
      maxValue += value[i]
      load += size[i]
    } else {
      let r = (capacity - load) / size[i]
      maxValue += r * value[i]
      load += size[i]
    }
    i++
  }
  return maxValue
}

// test
const capacity = 16 // 背包容积
const n = 5     // 保险箱中的物品数
const size = [3, 4, 7, 8, 9]    // 保险箱里的物品尺寸
const value = [4, 5, 10, 11, 13]    // 保险箱里的物品价值
const maxValue = ksack(capacity, size, value, n)     
console.log(maxValue) // 21.75

```