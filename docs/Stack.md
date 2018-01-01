> 一种遵循**后进先出**(LIFO，last-in-first-out)的有序列表。仅允许在表的一端进行插入和删除操作，这一端称为`栈顶`，把另一端称为`栈底`。

> 向一个栈中插入新元素叫作`入栈`，它将新元素放在栈顶。从一个栈中删除一个元素叫作`出栈`，它是把栈顶元素删除掉，使其相邻的元素称为新的栈顶元素。

![栈的模型](/images/stack.png)

应用：进制转换、实现回文、递归演示等

实现：

```
class Stack {

	constructor() {
		this.elements = []
	}

	// 入栈
	push(ele) {
		this.elements.push(ele)
	}

	// 出栈
	pop() {
		return this.elements.pop()
	}

	// 返回栈顶元素
	peek() {
		return this.elements[this.length - 1]
	}

	get length() {
		return this.elements.length
	}

	// 清空栈内元素
	clear() {
		this.length = 0
	}

}
```

示例1：进制转换

```
function mulBase(num, base) {
    const s = new Stack()
    do {
        s.push(num % base)
        num = Math.floor(num /= base)
    } while (num > 0)
    let converted = ''
    while(s.length > 0) {
        converted += s.pop()
    }
    return converted
}

// test
const num1 = mulBase(32, 2)
console.log(num1)			// 100000

const num2 = mulBase(125, 8)
console.log(num2)			// 175
```

示例2：判断回文

```
function isPalindrome(word) {
	const s = new Stack()
	for (let i = 0; i < word.length; i++) {
		s.push(word[i])
	}
	let rWord = ''
	while(s.length > 0) {
		rWord += s.pop()
	}
	if (word === rWord) {
		return true
	} 
	return false
}

// test
const str1 = isPalindrome('hello')
console.log(str1)		// false

const str2 = isPalindrome('racecar')
console.log(str2)		// true

```

示例3： 递归演示 —— 阶乘函数

```
function factorial(n) {
	if (n === 0) return 1
	else return n * factorial(n - 1)
}

// test
const num = factorial(5)
console.log(num)		// 120
```