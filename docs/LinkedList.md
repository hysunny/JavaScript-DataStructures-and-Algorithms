### 单向链表
> 链表是由一组节点组成的集合。每个节点都使用一个对象的引用指向它的后继。指向另一 个节点的引用叫做链。

实现：

```
class Node {
	constructor(val) {
		this.val = val   	// 节点数据
		this.next = null 		// 指向下一个节点的链接
	}
}

class LinkedList {

	constructor() {
		this.head = new Node('head')
		this.length = 0			// 链表的长度，默认为0，不算头节点
	}

	get isEmpty() {
		return !this.length
	}

	// 遍历链表，查找指定索引节点
	find(val) {
		let curNode = this.head
		while(curNode.val !== val) {
			curNode = curNode.next
		}
		return curNode
	}

	// 查找指定节点的索引
	findIndex(val) {
		let curNode = this.head
		let index = -1
		while(curNode.val !== val) {
			curNode = curNode.next
			index++
		}
		if (index == this.length) return -1
		return index
	}

	// 在任意位置插入节点
	insert(position, val) {
		if (position < 0 || position > this.length) return false
		const newNode = new Node(val)
		let curNode = this.head
		let index = 0
		// 找到插入点的节点
		while(index < position) {
			curNode = curNode.next
			index++
		}
		newNode.next = curNode.next
		curNode.next = newNode
		this.length++
		return true
	}

	// 在链表最后插入一个新元素
	append(val) {
		const newNode = new Node(val)
		let curNode = this.head
		while(curNode.next !== null) {
			curNode = curNode.next
		}
		curNode.next = newNode
		this.length++
	}

	// 删除指定位置的节点
	removeAt(position) {
		if (position < 0 || position > this.length) return null
		let curNode = this.head
		let index = 0
		let prevNode = null
		// 找到待删除节点
		while(index <= position) {
			prevNode = curNode
			curNode = curNode.next
			index++
		}
		prevNode.next = curNode.next
		this.length--
		return curNode.val
	}

	// 删除节点
	remove(val) {
		// 找到待删除节点前面的节点，然后修改它的next属性，使其不再指向待删除节点，而是指向待删除节点的下一个节点
		const index = this.findIndex(val)
		return this.removeAt(index)
	}

	toString() {
		let curNode = this.head
		let str = ''
		while(curNode.next !== null) {
			str += curNode.next.val + ', '
			curNode = curNode.next
		}
		return str.slice(0, -2)
	}
}
```

测试：

```
// test
const cities = new LinkedList()
cities.insert(0, 'BeiJing')
cities.insert(1, 'ShangHai')
cities.insert(0, 'ChongQing')
console.log(cities.toString())		// ChongQing, BeiJing, ShangHai
cities.remove('BeiJing')
console.log(cities.toString())		// ChongQing, ShangHai
console.log(cities.length)				// 2
cities.removeAt(1)
console.log(cities.toString())		// ChongQing
cities.append('GuangZhou')
console.log(cities.toString())		// ChongQing, GuangZhou
```

### 双向链表
> 与普通列表类似，只是多了一个指向前驱节点的链接。

实现：

```
class Node {
	constructor(val) {
		this.val = val   	// 节点数据
		this.next = null 		// 指向下一个节点的链接
		this.prev = null	// 指向前一个节点
	}
}

class DbLinkedList {

	constructor() {
		this.head = new Node('head')
		this.current = this.head
		this.length = 0			// 链表的长度，默认为0，不算头节点
	}

	get isEmpty() {
		return !this.length
	}

	// 遍历链表，查找指定索引节点
	find(val) {
		let curNode = this.head
		while(curNode.val !== val) {
			curNode = curNode.next
		}
		return curNode
	}

	// 查找最后的节点
	findLast() {
		let curNode = this.head
		while(curNode.next !== null) {
			curNode = curNode.next
		}
		return curNode
	}

	// 查找指定节点的索引
	findIndex(val) {
		let curNode = this.head
		let index = -1
		while(curNode.val !== val) {
			curNode = curNode.next
			index++
		}
		if (index == this.length) return -1
		return index
	}

	// 在任意位置插入节点
	insert(position, val) {
		if (position < 0 || position > this.length) return false
		const newNode = new Node(val)
		let curNode = this.head
		let index = 0
		// 找到插入点的节点
		while(index < position) {
			curNode = curNode.next
			index++
		}
		newNode.next = curNode.next
		newNode.prev = curNode
		if (curNode.next && curNode.next.prev) {
			curNode.next.prev = newNode
		}
		curNode.next = newNode
		this.length++
		return true
	}

	// 在链表最后插入一个新元素
	append(val) {
		const newNode = new Node(val)
		let curNode = this.head
		while(curNode.next !== null) {
			curNode = curNode.next
		}
		curNode.next = newNode
		newNode.prev = curNode
		this.length++
	}

	// 删除指定位置的节点
	removeAt(position) {
		if (position < 0 || position > this.length) return null
		let curNode = this.head
		let index = 0
		let prevNode = null
		// 找到待删除节点
		while(index <= position) {
			prevNode = curNode
			curNode = curNode.next
			index++
		}
		prevNode.next = curNode.next
		curNode.next.prev = prevNode
		curNode.prev = null
		curNode.next = null
		this.length--
		return curNode.val
	}

	// 删除节点
	remove(val) {
		// 找到待删除节点前面的节点，然后修改它的next属性，使其不再指向待删除节点，而是指向待删除节点的下一个节点
		const index = this.findIndex(val)
		return this.removeAt(index)
	}

	toString() {
		let curNode = this.head
		let str = ''
		while(curNode.next !== null) {
			str += curNode.next.val + ', '
			curNode = curNode.next
		}
		return str.slice(0, -2)
	}

	display() {
		console.log(this.toString())
	}

	displayReverse() {
		const data = this.toString().split(', ').reverse().join(', ')
		console.log(data)
	}

	// 在链表中向前移动 n 个节点
	advance(n) {
		while(n > 0 && this.current.next !== null) {
			this.current = this.current.next
			n--
		}
	}

	// 在双向链表中后退 n 个节点
	back(n) {
		while(n > 0 && this.current.val !== 'head') {
			this.current = this.current.prev
			n--
		}
	}

	// 只显示当前节点
	show() {
		console.log(this.current)
	}
}
```

测试：

```
const cities = new DbLinkedList()
cities.insert(0, 'BeiJing')
cities.insert(1, 'ShangHai')
cities.insert(0, 'ChongQing')
cities.append('GuangZhou')
cities.append('TianJin')
cities.append('Taiwan')
cities.display()							// ChongQing, BeiJing, ShangHai, GuangZhou, TianJin, Taiwan
cities.remove('BeiJing')
cities.display()							// ChongQing, ShangHai, GuangZhou, TianJin, Taiwan
cities.removeAt(1)
cities.display()							// ChongQing, GuangZhou, TianJin, Taiwan
cities.displayReverse()				// Taiwan, TianJin, GuangZhou, ChongQing
cities.show()
cities.advance(2)
cities.show()									// Node {val: "GuangZhou", next: Node, prev: Node}
cities.back(1)
cities.show()									// Node {val: "ChongQing", next: Node, prev: Node}
```

### 循环链表
> 循环链表和单向链表相似，节点类型都是一样的。唯一的区别是，在创建循环链表时，让其头节点的 next 属性指向它本身，即: `head.next = head`

实现：

```
class Node {
	constructor(val) {
		this.val = val   	// 节点数据
		this.next = null 		// 指向下一个节点的链接
	}
}

class CircularLinkedList {
	constructor() {
		this.head = new Node('head')
		this.length = 0			// 链表的长度，默认为0，不算头节点
		this.current = this.head
	}

	get isEmpty() {
		return !this.length
	}

	// 遍历链表，查找指定索引节点
	find(val) {
		let curNode = this.head
		while(curNode.val !== val && curNode.next.val !== 'head') {
			curNode = curNode.next
		}
		return curNode
	}

	// 查找指定节点的索引
	findIndex(val) {
		let curNode = this.head
		let index = -1
		while(curNode.val !== val && curNode.next.val !== 'head') {
			curNode = curNode.next
			index++
		}
		if (index == this.length) return -1
		return index
	}

	// 查找最后的节点
	findLast() {
		let curNode = this.head
		while(curNode.next !== null && curNode.next.val !== 'head') {
			curNode = curNode.next
		}
		return curNode
	}

	// 在任意位置插入节点
	insert(position, val) {
		if (position < 0 || position > this.length) return false
		const newNode = new Node(val)
		let curNode = this.head
		let index = 0
		// 找到插入点的节点
		while(index < position) {
			curNode = curNode.next
			index++
		}
		newNode.next = curNode.next
		curNode.next = newNode
		// 链表的尾节点指向头节点
		const lastNode = this.findLast()
		lastNode.next = this.head
		this.length++
		return true
	}

	// 在链表最后插入一个新元素
	append(val) {
		const newNode = new Node(val)
		let curNode = this.head
		while(curNode.next !== null && curNode.next.val !== 'head') {
			curNode = curNode.next
		}
		curNode.next = newNode
		// 链表的尾节点指向头节点
		newNode.next = this.head
		this.length++
	}

	// 删除指定位置的节点
	removeAt(position) {
		if (position < 0 || position > this.length) return null
		let curNode = this.head
		let index = 0
		let prevNode = null
		// 找到待删除节点
		while(index <= position) {
			prevNode = curNode
			curNode = curNode.next
			index++
		}
		prevNode.next = curNode.next
		this.length--
		return curNode.val
	}

	// 删除节点
	remove(val) {
		// 找到待删除节点前面的节点，然后修改它的next属性，使其不再指向待删除节点，而是指向待删除节点的下一个节点
		const index = this.findIndex(val)
		return this.removeAt(index)
	}

	// 在链表中向前移动 n 个节点
	advance(n) {
		while(n > 0) {
        this.current = this.current.next
        if (this.current.val === 'head') {
            this.current = this.current.next
        }
        n--
    }
	}

	// 只显示当前节点
	show() {
		console.log(this.current)
	}

	display() {
		console.log(this.toString())
	}

	displayReverse() {
		const data = this.toString().split(', ').reverse().join(', ')
		console.log(data)
	}

	toString() {
		let curNode = this.head
		let str = ''
		while(curNode.next !== null && curNode.next.val !== 'head') {
			str += curNode.next.val + ', '
			curNode = curNode.next
		}
		return str.slice(0, -2)
	}
}
```

测试：

```
const cities = new CircularLinkedList()
cities.insert(0, 'BeiJing')
cities.insert(1, 'ShangHai')
cities.insert(0, 'ChongQing')
cities.append('GuangZhou')
cities.append('TianJin')
cities.append('Taiwan')
cities.display()							// ChongQing, BeiJing, ShangHai, GuangZhou, TianJin, Taiwan
cities.remove('BeiJing')
cities.display()							// ChongQing, ShangHai, GuangZhou, TianJin, Taiwan
cities.removeAt(1)
cities.display()							// ChongQing, GuangZhou, TianJin, Taiwan
cities.displayReverse()				// Taiwan, TianJin, GuangZhou, ChongQing
cities.advance(2)
cities.show()									// Node {val: "GuangZhou", next: Node}
```

示例：约瑟夫环问题

> 传说在公元 1 世纪的犹太战争中，犹太历史学家弗拉维奥·约瑟夫斯和他的 40 个同胞 被罗马士兵包围。犹太士兵决定宁可自杀也不做俘虏，于是商量出了一个自杀方案。他 们围成一个圈，从一个人开始，数到第三个人时将第三个人杀死，然后再数，直到杀光 所有人。约瑟夫和另外一个人决定不参加这个疯狂的游戏，他们快速地计算出了两个位 置，站在那里得以幸存。写一段程序将 n 个人围成一圈，并且第 m 个人会被杀掉，计算 一圈人中哪两个人最后会存活。使用循环链表解决该问题。

```
/**
 * @param { Number } n 	// 总人数
 * @param { Number } m	// 间隔人数
 * @return { String }
 */
function game(n, m) {
    var links = new CircularLinkedList()
    links.insert(0, 1)
    var sign = 2
    // 生成循环链表
    while(sign <= n) {
        links.insert(sign - 1, sign)
        sign++
    }
    // 循环遍历，直到剩余的人数少于间隔数
    while(n >= m) {
        links.advance(m)
        links.remove(links.current.val)
        n--
    }
    links.display()
}

// test
game(40, 3)        // 13, 28
```
