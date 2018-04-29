
> 简书地址: https://www.jianshu.com/p/b670efe417e8

### 字典
> 一种以key-value形式存储数据的数据结构。

实现：

```js
class Dictionary {

  constructor() {
    this.elements = []
  }
  
  // 字典中元素个数
  get length() {
    return Object.keys(this.elements).length
  }
  
  // 查找元素
  find(key) {
    return this.elements[key]
  }
  
  // 添加元素
  add(key, val) {
    this.elements[key] = val
  }
  
  // 移除元素
  remove(key) {
    delete this.elements[key]
  }
  
  // 清空字典
  clear() {
    this.elements = []
  }
  
  
  // 展示字典中的所有键值对
  display() {
    // 按字典顺序显示所有元素
         for (let key of Object.keys(this.elements).sort()) {
        console.log(key + ' -> ' + this.elements[key])
    }
  }
}
```

示例：按字母顺序显示来一段文本中各个单词出现的次数

```js
function wordCount(s) {
  const words = s.split(' ')
  var d = new Dictionary()
  words.forEach(item => {
    if (d.find(item)) {
        d.add(item, d.find(item) + 1)
    } else {
        d.add(item, 1)
    }
  })
  d.display()
}

// test
wordCount('the brown fox jumped over the blue fox') // blue -> 1 brown -> 1 fox -> 2 jumped -> 1 over -> 1 the -> 2

```