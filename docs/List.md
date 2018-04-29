
> 简书地址: https://www.jianshu.com/p/e7d1ec7b2999

### List
> 一组有序的数据集合，集合中的每个数据项叫做元素，可对这些数据项进行增删改查操作。 

应用：存储数据，进行数据的简单查找和排序

实现：
```js
class List {

  constructor() {
    this.elements = []
    this.pos = 0			// 列表的当前位置
  }
  
  // 返回列表中元素的个数
  get length() {
    return this.elements.length
  }
  
  // 清空列表中的所有元素
  clear() {
    this.elements = []
  }
  
  // 在列表中查找某一元素
  find(ele) {
    return this.elements.indexOf(ele)
  }
  
  // 返回列表的字符串形式
  toString() {
    return this.elements.toString()
  }
  
  // 在现有元素后插入新元素
  insert(after, ele) {
    if (this.find(after) > -1) {
      const index = this.find(after) + 1
      this.elements.splice(index, 0, ele)
      return true
    }
    return false
  }
  
  // 给列表添加元素
  append(ele) {
    this.elements.push(ele)
  }
  
  // 从列表中删除元素
  remove(ele) {
    if (this.find(ele) > -1) {
      this.elements.splice(this.find(ele), 1)
      return true
    }
    return false
  }
  
  // 返回列表的当前位置
  currPos() {
    return this.pos
  }
  
  // 返回当前位置的元素
  getElement() {
    return this.elements[this.pos]
  }
  
  // 将列表的当前位置设移动到第一个元素
  front() {
    this.pos = 0
  }
  
  // 将列表的当前位置移动到最后一个元素
  end() {
    this.pos = this.length - 1
  }
  
  // 将当前位置前移一位
  prev() {
    if (this.pos > 0) {
      this.pos--
      return this.getElement()
    }
  }
  
  // 将当前位置后移一位
  next() {
    if (this.pos < this.length - 1) {
      this.pos++
      return this.getElement()
    }
  }
  
  // 将当前位置移动到指定位置
  moveTo(pos) {
    if (pos >= 0 && pos < this.length) {
      this.pos = pos
      return this.getElement()
    }
  }
  
  // 实现一个迭代器（Iterator），可以使用for...of...循环
  /**
   * next(): 返回 { value: all_type, done: Boolean }
   * value: 列表中的某个值
   * done: 标明迭代是否结束
   */
  [Symbol.iterator]() {
    this.pos = 0
    return {
      next: () => {
        this.pos++
        if (this.pos > this.length) {
          return {
            value: null,
            done: true
          }
        } else {
          return {
            value: this.elements[this.pos - 1],
            done: false
          }
        }
      }
    }
  }

}
```

示例：

```js
// test 
var names = new List()
names.append('Clayton')
names.append('Raymond')
names.append('Cynthia')
names.append('Jennifer')
names.append('Bryan')
names.append('Danny')
names.insert('Cynthia', 'Hysunny')
console.log(names.toString())			// Clayton,Raymond,Cynthia,Hysunny,Jennifer,Bryan,Danny

console.log(names.currPos())			// 0
names.front()
console.log(names.getElement()) 	// Clayton
names.next()
console.log(names.getElement()) 	// Raymond
names.moveTo(4)										
console.log(names.getElement())		// Bryan

names.remove('Bryan')
console.log(names.toString())			// Clayton,Raymond,Cynthia,Hysunny,Jennifer,Danny

// 使用迭代器
for (var name of names) {
  console.log(name)
}
// Clayton,Raymond,Cynthia,Hysunny,Jennifer,Danny

names.clear()
console.log(names.length)					// 0
```