
> 简书地址: https://www.jianshu.com/p/d99183687cf1

### 1. 希尔排序
> `希尔排序`的核心理念与`插入排序`不同，它会首先比较距离**较远**的元素，而非相邻的元素。
和简单地比较相邻元素相比，使用这种方案可以**使离正确位置很远的元素更快地回到合适的位置**。
当开始用这个算法遍历数据集时，所有元素之间的距离会不断减小，直到处理到数据集的末尾，这时算法比较的就是相邻元素了。

间隔序列：701, 301, 132, 57, 23, 10, 4, 1
参照：[Best Increments for the Average Case of Shell Sort](http://sun.aei.polsl.pl/~mciura/publikacje/shellsort.pdf)

（1）自定义间隔值（硬编码间隔序列）

 实现：

```js
function shellSort(data, gaps) {
  const dLen = data.length
  const gLen = gaps.length
  // 根据间隔序列循环数组数据
  for (let g = 0; g < gLen; g++) {
    // 循环间隔下的分组数据，从每个分组的第二个数开始
    for (let i = gaps[g]; i < dLen; i++) {
      let temp = data[i]
      let j = i
      // 将 data[j] 和 data[j - gaps[g]] 进行比较，也就是比较间隔 gaps[g] 的两个数字的大小向右移 gaps[g] 位
      // 从右至左，如果 data[j - gaps[g]] 大于temp，则已排序数据
      while ((j >= gaps[g]) && (data[j - gaps[g]] > temp)) {
        data[j] = data[j - gaps[g]]
        j -= gaps[g]
      }
      // 空出来的位置放置 temp
      data[j] = temp
    }
  }
}

// test
const a = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
shellSort(a, [5, 3, 1])
console.log(a)  // [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

下图展示了上例中的数据进行`硬编码间隔序列希尔排序`的过程

> ![希尔排序](/images/shell-sort.png)


（2）动态计算间隔值（动态间隔序列）

实现：

```js
function dShellSort(data) {
  const len = data.length
  let gap = 1
  // 计算间隔最大值
  while (gap < len / 3) {
    gap = 3 * gap + 1
  }
  // 循环间隔值
  while (gap >= 1) {
    // 循环间隔下的分组数据，从每个分组的第二个数开始
    for (let i = gap; i < len; i++) {
      // 从右至左，如果 data[j - gap] 大于 data[j] 并且 j > gap，则交换两值
      for (let j = i; j >= gap && data[j] < data[j - gap]; j -= gap) {
        [data[j], data[j - gap]] = [data[j - gap], data[j]]
      }
    }
    gap = (gap - 1) / 3
  }
}

// test
const b = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
dShellSort(b)
console.log(b) // [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

（3）测试两种希尔排序效率

a. 实现一个测试平台

```js
class CArray {

  constructor(count) {
    this.data = []
    this.count = count
  }
  
  setData() {
    for (let i = 0; i < this.count; i++) {
      this.data[i] = Math.floor(Math.random() * (this.count + 1))
    }
  }
}
```
b. 进行测试

```js
var arr = new CArray(1000)
arr.setData()
var data1 = arr.data
var data2 = data1.concat()
console.time('硬编码间隔序列shellSort')
shellSort(data1, [5, 3, 1])
console.log(data1)
console.timeEnd('硬编码间隔序列shellSort')

console.time('动态间隔序列shellSort')
dShellSort(data2)
console.log(data2)
console.timeEnd('动态间隔序列shellSort')
```
多执行几次，得出测试结果：

```
硬编码间隔序列shellSort: 2.715087890625ms
动态间隔序列shellSort: 2.251220703125ms

硬编码间隔序列shellSort: 3.259765625ms
动态间隔序列shellSort: 3.214111328125ms

硬编码间隔序列shellSort: 2.001953125ms
动态间隔序列shellSort: 2.109130859375ms

硬编码间隔序列shellSort: 1.935791015625ms
动态间隔序列shellSort: 3.258056640625ms

硬编码间隔序列shellSort: 2.023193359375ms
动态间隔序列shellSort: 2.18994140625ms
```
可以看出：`硬编码间隔序列shellSort`和`动态间隔序列shellSort`效率差不多。

### 2. 归并排序
> 原理：把一系列排好序的子序列**合并**成一个大的完整有序序列 —— **自底向上**的`归并排序`。

实现：

```js
function mergeSort(data) {
  const len = data.length
  if (len < 2) {
    return
  }
  let step = 1
  let left, right
  // 从 step = 1开始，分割 2 * step 个子序列
  while (step < len) {
    left = 0
    right = step
    // 循环数据
    while (right + step <= len) {
      // 合并左右子序列
      mergeArrays(data, left, left + step, right, right + step)
      left = right + step
      right = left + step
    }
    if (right < len) {
      mergeArrays(data, left, left + step, right, len)
    }
  
    step *= 2
  }
}

// 合并数组
function mergeArrays(arr, startLeft, stopLeft, startRight, stopRight) {
  const leftArr = []
  const rightArr = []
  // 生成左子序列数组
  for (let i = startLeft; i < stopLeft; i++) {
    leftArr.push(arr[i])
  }
  // 生成右子序列数组
  for (let j = startRight; j < stopRight; j++) {
    rightArr.push(arr[j])
  }
  leftArr.push(Infinity) // 添加哨兵值
  rightArr.push(Infinity) // 添加哨兵值
  
  // 合并两个子序列数组并替换原数组序列
  let m = 0
  let n = 0
  for (let k = startLeft; k < stopRight; k++) {
    if (leftArr[m] <= rightArr[n]) {
      arr[k] = leftArr[m]
      m++
    } else {
      arr[k] = rightArr[n]
      n++
    }
  }
}


// test
const e = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
mergeSort(e)
console.log(e)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

下图展示了上例中的数据进行`归并排序`的过程

> ![归并排序](/images/merge-sort.png)

### 3. 快速排序

> `快速排序`是处理大数据集**最快**的排序算法之一。
它是一种**分而治之**的算法，通过递归的方法将数据依次分解为包含较小元素和较大元素的不同子序列。
该算法不断重复这个步骤直到所有数据都是有序的。

> 步骤：
（1）选择一个元素作为**基准值**(pivot)，将列表分割成两个子序列。
（2）将列表重新排序，将所有**小于基准值的元素放在基准值的前面，所有大于基准值的元素放在基准值的后面**；
（3）分别对较小元素的子序列和较大元素的子序列重复步骤1和2

实现：

```js
function qSort(data) {
  const len = data.length
  if (len < 2) return data
  const leftArr = []
  const rightArr = []
  // 设数组第一个元素为基准值(pivot)
  let pivot = data[0]
  // 将列表重新排序，将所有小于基准值的元素放在基准值的前面。所有大于基准值的元素放在基准值的后面
  for (let i = 1; i < len; i++) {
    if (data[i] < pivot) {
      leftArr.push(data[i])
    } else {
      rightArr.push(data[i])
    }
  }
  console.log(data)
  return qSort(leftArr).concat(pivot, qSort(rightArr))
}

// test 
const f = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
const g = qSort(f)
console.log(g)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

下图展示了上例中的数据进行`快速排序`的过程

> ![快速排序](/images/qSort.png)
