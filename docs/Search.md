
> 简书地址: https://www.jianshu.com/p/45445f69e5fa

### 1. 顺序查找

（1）查找某值
```js
function seqSearch(arr, target) {
  const len = arr && arr.length
  if (!len) return -1
  for (let i = 0; i < len; i++) {
    if (arr[i] === target) {
      return i
    }
  }
  return -1
}

// test
const arr1 = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
const result1 = seqSearch(arr1, 82)
console.log(result1)	// 8
```

（2）查找最小值

```js
function findMin(arr) {
  const len = arr && arr.length
  if (!len) return null
  let min = arr[0]
  for(let i = 1; i < len; i++) {
    if (arr[i] < min) {
      min = arr[i]
    }
  }
  return min
}

// test
const arr2 = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
const result2 = findMin(arr2)
console.log(result2)	// 2
```

（3）查找最大值

```js
function findMax(arr) {
  const len = arr && arr.length
  if (!len) return null
  let max = arr[0]
  for (let i = 1; i < len; i++) {
    if (arr[i] > max) {
      max = arr[i]
    }
  }
  return max
}

// test
const arr3 = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
const result3 = findMax(arr3)
console.log(result3)	// 82
```

（4）包含**自组织方式**的顺序查找
> 使用自组织（数据的位置并非由程序员在执行之前就组织好，而是**在程序运行过程中由程序自动组织的**）数据
通过将频繁查找到的元素置于数据集的起始位置来**最小化查找次数**。

a. 包含自组织方式的seqSearch（1）：不断检查确认已找到的数据是否已经排在最前面。

```js
function seqSearch(arr, target) {
  const len = arr.length
  for (let i = 0; i < len; i++) {
    if (arr[i] === target) {
      if (i > 0) {
        [arr[i - 1], arr[i]] = [arr[i], arr[i - 1]]
      }
      return true
    }
  }
  return false
}

// test
const arr4 =  [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
for(let i = 1; i <= 4; i++) {
    const result4 =  seqSearch(arr4, 77)
    console.log(arr4)
    // [72, 54, 59, 30, 31, 78, 77, 2, 82, 72]
    // [72, 54, 59, 30, 31, 77, 78, 2, 82, 72]
    // [72, 54, 59, 30, 77, 31, 78, 2, 82, 72]
    // [72, 54, 59, 77, 30, 31, 78, 2, 82, 72]
}
```

b. 包含自组织方式的seqSearch（2）：将找到的元素移动到数据集的**起始**位置，但是如果这个元素已经很接近起始位置，则不会对它的位置进行交换。

> 80-20原则：对某一数据集执行的80%的查找操作都是对其中20%的数据元素进行查找。

```js
function seqSearch(arr, target) {
  const len = arr.length
  for (let i = 0; i < len; i++) {
    if (arr[i] === target) {
      if (i > (arr.length * 0.2)) {
        // 仅当数据位于数据集的前20%元素之外时，该数据才需要被重新移动到数据集的起始位置。
        [arr[0], arr[i]] = [arr[i], arr[0]]
      }
      return true
    }
  }
  return false
}

// test
const arr5 =  [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
for(let i = 1; i <= 4; i++) {
    const result5 =  seqSearch(arr5, 77)
    console.log(arr5)
    // [77, 54, 59, 30, 31, 78, 2, 72, 82, 72]
    // [77, 54, 59, 30, 31, 78, 2, 72, 82, 72]
    // [77, 54, 59, 30, 31, 78, 2, 72, 82, 72]
    // [77, 54, 59, 30, 31, 78, 2, 72, 82, 72]
}
```

### 2. 二分查找算法【已排序数据】

> 算法描述：
（1）将数组的第一个位置设置为下边界（0）
（2）将数组最后一个元素所在的位置设置为上边界（数组的长度减1）
（3）若下边界等于或小于上边界，则做如下操作：
a. 将中点设置为（上边界加上下边界）除以2
b. 如果中点的元素小于查询的值，则将下边界设置为中点元素所在下标加1
c. 如果中点的元素大于查询的值，则将上边界设置为中点元素所在下标减1
d. 否则中点元素即为要查找的数据，可以进行返回

实现：

```js
function binSearch(arr, target) {
  const len = arr.length
  let upperBound = len - 1
  let lowBound = 0
  while (lowBound <= upperBound) {
    // 将中点设置为 (上边界 + 下边界) / 2
    let mid = Math.floor((lowBound + upperBound) / 2)
    console.log(`当前的中点值：${arr[mid]}`)
    if (arr[mid] < target) {
      // 如果中点的元素小于查询的值，则将下边界设置为中点元素所在的下标加一
      lowBound = mid + 1
    } else if (arr[mid] > target) {
      // 如果中点的元素大于查询的值，则将上边界设置为中点元素所在的下标减一
      upperBound = mid - 1
    } else {
      // 否则中点元素即为要查找的数据，可以进行返回
      return mid
    }
  }
  return -1
}

// test
const arr6 = [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
const result6 = binSearch(arr6, 78)
console.log(result6)
// 当前的中点值：59
// 当前的中点值：77
// 当前的中点值：78
// 8
```

示例：计算一个数组中某个数的重复次数

```js
function count(arr, target) {
  const len = arr.length
  let count = 0
  arr.sort((a, b) => { return a - b })
  let pos = binSearch(arr, target)
  if (pos > -1) {
    count++
    // 向下（左）遍历数组，统计找到的值出现的次数，当下一个值与要查找的值不匹配时停止计数
    for (let i = pos - 1; i > 0; i--) {
      if (arr[i] === target) {
        count++
      } else {
        break
      }
    }
    // 向上(右)遍历数组，统计找到的值出现的次数，当下一个值与要查找的值不匹配时停止计数
    for (let j = pos + 1; j < len; j++) {
      if (arr[j] === target) {
        count++
      } else {
        break
      }
    }
  }
  return count
}

// test
const arr7 = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
const result7 = count(arr7, 72)
console.log(`重复次数为：${result7}次`)

// 当前的中点值：59
// 当前的中点值：77
// 当前的中点值：72
// 重复次数为：2次
```