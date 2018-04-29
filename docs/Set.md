
> 简书地址: https://www.jianshu.com/p/5c76b3dc41bb

### 集合
> 一组无序但彼此之间又有一定相关性的成员构成，每个成员在集合中只能出现一次。

#### 集合相关的一些基本概念
1. 集合的定义：

>（1）不包含任何成员的集合称为空集，全集则是包含一切可能成员的集合
>（2）如果两个集合的成员完全相同，则两个集合相等
>（3）如果一个集合中所有的成员都属于另外一个集合，则前一集合称为后一集合的子集。


2. 集合的操作：

>（1）并集（合并）
>（2）交集（共同存在的）
>（3）补集（属于一个集合不属于另一个集合的成员组成的集合）


实现：

```js
class Set {

  constructor() {
    this.elements = []
  }
  
  get size() {
    return this.elements.length
  }
  
  get isEmpty() {
    return !this.elements.length
  }
  
  // 添加元素，确保元素是唯一的
  add(...eles) {
    for (let ele of eles) {
      if (this.contains(ele)) {
        console.warn(`集合中已存在该元素: ${ele}`)
        continue
      } else {
        this.elements.push(ele)
      }
    }
  }
  
  // 删除元素
  remove(...eles) {
    for (let ele of eles) {
      const pos = this.elements.indexOf(ele)
      if (pos > -1) {
        this.elements.splice(pos, 1)
      } else {
        console.warn(`集合中不存在元素: ${ele}， 无法删除`)
      }
    }
  }
  
  // 判断集合中是否包含某个元素
  contains(ele) {
    return this.elements.includes(ele)
  }
  
  // 并集
  union(set) {
    const tempSet = new Set()
    this.elements.forEach(item => {
      tempSet.add(item)
    })
    set.elements.forEach(item => {
      if (!this.contains(item)) {
        tempSet.add(item)
      }
    })
    return tempSet
  } 
  
  // 交集
  intersect(set) {
    const tempSet = new Set()
    set.elements.forEach(item => {
      if (this.contains(item)) {
        tempSet.add(item)
      }
    })
    return tempSet
  }
  
  // 判断子集，当前集合是不是入参集合的一个子集
  subsetOf(set) {
    if (set.size < this.size) return false
    for (let i = 0; i < this.size; i++) {
      if (!set.contains(this.elements[i])) {
        return false
      }
    }
    return true
  }
  
  // 补集，属于当前集合，而不属于入参集合的元素
  difference(set) {
    const tempSet = new Set()
    this.elements.forEach(item => {
      if (!set.contains(item)) {
        tempSet.add(item)
      }
    })
    return tempSet
  }
  
  // 返回比传入元素大的元素中最小的那个
  higher(ele) {
    let temp = null
    let i = this.size
    while(i >= 0) {
      if (this.elements[i] > ele && 
        (temp !== null && this.elements[i] < temp ||
          temp === null)
        ) {
        temp = this.elements[i]
      }
      i--
    }
    return temp
  }
  
  // 返回比传入元素小的元素中最大的那个
  lower(ele) {
    let temp = null
    let i = this.size
    while(i >= 0) {
      if (this.elements[i] < ele && 
        (temp !== null && this.elements[i] > temp ||
        temp === null)
        ) {
        temp = this.elements[i]
      }
      i--
    }
    return temp 
  }
  
  // 元素的字符串形式显示
  toString() {
    return this.elements.toString()
  }
}

// test
const set = new Set()
set.add(49, 100, 2, 30, 18, 256, 99)
console.log(set.toString())		// 49,100,2,30,18,256,99
set.remove(49, 18)
console.log(set.toString())		// 100,2,30,256,99
console.log(set.size)					// 5

const set1 = new Set()
set1.add(5, 8, 100, 30, 256)	
console.log(set1.toString())  // 5,8,100,30,256

// 并集
console.log(set.union(set1).toString())	// 100,2,30,256,99,5,8
// 交集
console.log(set.intersect(set1).toString())		// 100,30,256
// 子集
console.log(set.subsetOf(set1).toString())		// false
// 补集
console.log(set.difference(set1).toString())	// 2,99

console.log(set.toString())		// 100,2,30,256,99

console.log(set.higher(99))		// 100

console.log(set.lower(99))		// 30
```