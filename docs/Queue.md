### 顺序队列
> 一种遵循**先进先出**的(FIFO，first-in-first-out)的有序列表。队列只能在**队尾**插入元素（`入队`），在队首删除元素（`出队`）。

应用：舞伴问题、基数排序等

实现：

```
class Queue {

	constructor() {
		this.elements = []
	}

	get length() {
		return this.elements.length
	}

	get isEmpty() {
		return !this.length
	}

	// 入队
	enqueue(ele) {
		this.elements.push(ele)
	}

	// 出队
	dequeue() {
		return this.elements.shift()
	}

	// 获取队首元素
	front() {
		return this.elements[0]
	}

	// 获取队尾元素
	back() {
		return this.elements[this.length - 1]
	}

	toString() {
		return this.elements.toString()
	}
}
```

示例：基数排序

```
/*
 * @param { Array } arr
 * @param { Number } maxDigit	// 数组中最大数的位数
 * @return { Array }
 */
function radixSort(arr, maxDigit) {
	let mod = 10	
  let base = 1
  // init queue
	const queues = []
	for (let i = 0; i < 10; i++) {
		queues.push(new Queue())
	}
	for (let i = 0; i < maxDigit; i++, base *= 10, mod *= 10) {
		// distribute —— 分配数字到相应队列
		for (let j = 0; j < arr.length; j++) {
			const index = Math.floor((arr[j] % mod) / base)
			queues[index].enqueue(arr[j])
		}
		// collect —— 从队列中收集数字
		let k = 0
		for (let j = 0; j < 10; j++) {
			while(!queues[j].isEmpty) {
				arr[k++] = queues[j].dequeue()
			}
		}
	}
	return arr
}

// test
var arr = [4, 7, 3, 9, 10, 2, 324, 100]
radixSort(arr, 3)
console.log(arr)
```

### 优先队列
> 元素的添加和删除基于优先级。如急诊室中会根据患者病情的严重程度放号。

实现：
```
class PriorityQueue {

	constructor() {
		this.elements = []
	}

	get length() {
		return this.elements.length
	}

	get isEmpty() {
		return !this.length
	}

	// 入队
	enqueue(ele, priority) {
		const queueEle = { ele, priority }
		if (this.isEmpty) {
			this.elements.push(queueEle)
		} else {
			const preIndex = this.elements.findIndex(item => queueEle.priority < item.priority)
			if (preIndex > -1) {
				this.elements.splice(preIndex, 0, queueEle)
			} else {
				this.elements.push(queueEle)
			}
		}
	}

	// 出队
	dequeue() {
		return this.elements.shift()
	}

	// 获取队首元素
	front() {
		return this.elements[0]
	}

	// 获取队尾元素
	back() {
		return this.elements[this.length - 1]
	}

	toString() {
		console.table(this.elements)
	}
}
```

示例：急诊室排号

```
const patient = new PriorityQueue()
patient.enqueue('Clayton', 3)
patient.enqueue('Raymond', 2)
patient.enqueue('Cynthia', 3)
patient.enqueue('Jennifer', 1)
patient.enqueue('Danny', 4)

patient.toString()
```

### 双向队列
> 和队列类似的数据结构，允许从队列**两端**添加和删除元素。

实现：

```
class Deque {

	constructor() {
		this.elements = []
	}

	get length() {
		return this.elements.length
	}

	get isEmpty() {
		return !this.length
	}

	// 队首入队
	enqueueFront(ele) {
		this.elements.unshift(ele)
	}

	// 队尾出队
	dequeueBack() {
		return this.elements.pop()
	}

	// 队尾入队
	enqueueBack(ele) {
		this.elements.push(ele)
	}

	// 队首出队
	dequeueFront() {
		return this.elements.shift()
	}

	// 获取队首元素
	front() {
		return this.elements[0]
	}

	// 获取队尾元素
	back() {
		return this.elements[this.length - 1]
	}

	toString() {
		return this.elements.toString()
	}
}
```
示例：回文检测

```
function isPalindrome(str) {
	const dqueue = new Deque()
	for (let i = 0; i < str.length; i++) {
		dqueue.enqueueBack(str[i])
	}
	while(!dqueue.isEmpty) {
		if (dqueue.front() !== dqueue.back()) {
			return false
		} else {
			dqueue.dequeueFront()
			dqueue.dequeueBack()
		}
	}
	return true
}


// test
const str1 = isPalindrome('hello')
console.log(str1)		// false

const str2 = isPalindrome('racecar')
console.log(str2)		// true
```