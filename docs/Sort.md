
> 简书地址: https://www.jianshu.com/p/b14c78e91571

### 1. 冒泡排序算法
> 是**最慢**的排序算法之一，但也是一种最容易实现的排序算法。
按照升序排列：依次比较相邻的数据，较小的值冒泡到左侧，较大的值冒泡到右侧
按照降序排列：依次比较相邻的数据，较小的值冒泡到右侧，较大的值冒泡到左侧

实现：

```js
function bubbleSort(data) {
  const len = data.length
  for (let outer = len; outer >= 2; outer--) {
    for (let inner = 0; inner <= outer - 1; inner++) {
      if (data[inner] > data[inner + 1]) {
        // 若前一个值大于后一个值，则交换两个值
        [data[inner], data[inner + 1]] = [data[inner + 1], data[inner]]
      }
    }
  }
}

// test
const a = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
bubbleSort(a)
console.log(a)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```
下图展示了上例中的数据进行冒泡排序的过程（红框里的数据为我们为了观察冒泡过程而设定的两个特定值）

> ![冒泡排序](/images/bubble-sort.png)

### 2. 选择排序
> 从数组的开头开始，将第一个元素和其他元素进行比较。检查完所有元素后，**最小**的元素会被放到数组的**第一个位置**，然后算法会从第二个位置继续。这个过程一直进行，当进行到数组的倒数第二个位置时，所有的数据便完成了排序。

实现：

```js
function selectionSort(data) {
  const len = data.length
  let min 
  for (let outer = 0; outer < len - 1; outer++) {
    min = outer
    for (let inner = outer + 1; inner < len; inner++) {
      if (data[inner] < data[min]) {
        min = inner
      }
    }
    [data[outer], data[min]] = [data[min], data[outer]]
  }
}

// test
const b = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
selectionSort(b)
console.log(b)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

下图展示了上例中的数据进行选择排序的过程（红框里的数据为每次循环中最小的元素）

> ![选择排序](/images/selection-sort.png)

### 插入排序
> `插入排序`类似于人类按数字或字母顺序对数据进行排序。
`插入排序`有两个循环。`外循环`将数组元素挨个移动，而`内循环`则对外循环选中的元素及它后面的那个元素进行比较。如果外循环中选中的元素比内循环中的元素**小**，那么数组元素会**向右**移动，为内循环中的这个元素腾出位置。

>步骤：
（1）从第一个元素开始，该元素可以认为已经被排序
（2）取出下一个元素，在已经排序的元素序列中从后向前扫描
（3）如果该元素（已排序）大于新元素，将该元素移到下一位置
（4）重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
（5）将新元素插入到该位置后
（6）重复步骤2~5

实现：

方法一：

```js
function insertSort(data) {
  const len = data.length
  let temp, inner
  // 外循环：待排序的数据
  for (let outer = 1; outer < len; outer++) {
    temp = data[outer]
    inner = outer
    while(inner > 0 && data[inner - 1] >= temp) {
      // 内循环：已完成排序的数据
      // 比较待排序和已完成排序数据（从右至左），如果已排序的数据大于temp，则已排序数据向右移一位
      data[inner] = data[inner - 1]
      inner--
    }
    // 空出来的 原已排序的位置 放置待排序的数据
    data[inner] = temp
  }
}

// test
const c = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
insertSort(c)
console.log(c)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

方法二：

```js
function insertSort(data) {
  const len = data.length
  // 外循环：待排序的数据
  for (let outer = 1; outer < len; outer++) {
    // 内循环：已完成排序的数据
    for (let inner = 0; inner < outer; inner++) {
      // 如果外循环小于内循环（待排序小于已排序），则数组元素将会向右移动
      if (data[inner] > data[outer]) {
        // 将待排序数据插入到已排序的位置
        data.splice(inner, 0, data[outer])
        // 删除原先待排序的数据
        data.splice(outer + 1, 1)
      }
    }
  }
}

// test
const d = [72, 54, 59, 30, 31, 78, 2, 77, 82, 72]
insertSort(d)
console.log(d)	// [2, 30, 31, 54, 59, 72, 72, 77, 78, 82]
```

下图展示了上例中的数据进行插入排序的过程（红框里的数据为每次外循环执行完后被插入的元素）
> ![插入排序](/images/insert-sort.png)

### 4. 三种基本排序的效率

（1）首先，实现一个测试平台，进行测试数据的生成。

```js
// 测试平台
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

（2）声明排序算法

```js
// 冒泡排序
function bubbleSort(data) {
  const len = data.length
  for (let outer = len; outer >= 2; outer--) {
    for (let inner = 0; inner <= outer - 1; inner++) {
      if (data[inner] > data[inner + 1]) {
        // 若前一个值大于后一个值，则交换两个值
        [data[inner], data[inner + 1]] = [data[inner + 1], data[inner]]
      }
    }
  }
}

// 选择排序
function selectionSort(data) {
  const len = data.length
  let min 
  for (let outer = 0; outer < len - 1; outer++) {
    min = outer
    for (let inner = outer + 1; inner < len; inner++) {
      if (data[inner] < data[min]) {
        min = inner
      }
    }
    [data[outer], data[min]] = [data[min], data[outer]]
  }
}

// 插入排序
function insertSort(data) {
  const len = data.length
  let temp, inner
  // 外循环：待排序的数据
  for (let outer = 1; outer < len; outer++) {
    temp = data[outer]
    inner = outer
    while(inner > 0 && data[inner - 1] >= temp) {
      // 内循环：已完成排序的数据
      // 比较待排序和已完成排序数据（从右至左），如果已排序的数据大于temp，则已排序数据向右移一位
      data[inner] = data[inner - 1]
      inner--
    }
    // 空出来的 原已排序的位置 放置待排序的数据
    data[inner] = temp
  }
}
```


（3）进行测试

```js
var arr = new CArray(1000)
arr.setData()
var data1 = arr.data
var data2 = data1.concat()
var data3 = data1.concat()
// 冒泡排序
console.time('bubbleSort')
bubbleSort(data1)
console.log(data1)
console.timeEnd('bubbleSort') 

// 选择排序
console.time('selectionSort')
bubbleSort(data2)
console.log(data2)
console.timeEnd('selectionSort')

// 插入排序
console.time('insertSort')
bubbleSort(data3)
console.log(data3)
console.timeEnd('insertSort')
```

多执行几次测试代码，我们可以看到测试结果
```
bubbleSort: 29.56201171875ms
selectionSort: 24.248046875ms
insertSort: 15.97119140625ms

bubbleSort: 25.317138671875ms
selectionSort: 20.39404296875ms
insertSort: 13.625ms

bubbleSort: 28.112060546875ms
selectionSort: 21.611083984375ms
insertSort: 16.8720703125ms
```

我们可以得出如下结论：

效率： `插入排序` > `选择排序` > `冒泡排序`