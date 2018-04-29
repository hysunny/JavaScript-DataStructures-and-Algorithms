
> 简书地址: https://www.jianshu.com/p/d5d7fb58bf5f

### 散列
> 散列是一种常用的数据存储技术，散列后的数据可以快速地插入或取用。散列使用的数据结构叫做`散列表`。在散列表上插入、删除和取用数据都非常快，但是对于查找操作来说却效率低下，比如查找一组数据中的最大值和最小值。

散列表的实现基于数组，键为整型时，一般使用**除留余数法**（以数组的长度对键取值）来实现散列函数，数组的大小一般为**质数**。尽量避免键为字符串或其他类型。

 散列表中两个键映射成同一个值的情况，叫作碰撞（collision）。

 碰撞处理的方法：

1. 开链法（使用多维数组）
2. 线性检测法（开放寻址散列）
    当发生碰撞时，线性探测法检查散列表中的下一个位置是否为空。如果为空，就将数据存入该位置；如果不为空，则继续检查下一个位置，直到找到一个空的位置为止。

如何选择？
如果数组的大小是待存储数据的1.5倍，那么使用开链法；如果数组大小是待存储数据的两倍及两倍以上时，那么使用线性探测法。

#### 开链法实现HashTable

```js
class HashTable {

  constructor() {
    this.table = new Array(137)
  }
  
  // 除留余数（简单方法）：当发生碰撞时，后面的值会覆盖之前的值
  simpleHash(data) {
    let total = 0
    for (let i = 0; i < data.length; i++) {
      total += data.charCodeAt(i)
    }
    return total % this.table.length
  }
  
  // 除留余数（霍纳算法）
  // 数组的长度最好在100以上，这样会让数据在散列表中分布得更加均匀
  // 为了避免碰撞，在给散列表一个合适的大小后，新的散列函数仍然先计算字符串中各字符的 ASCII 码值，不过求和时每次要乘以一个质数
  betterHash(data) {
    const H = 37
    let total = 0
    for (let i = 0; i < data.length; i++) {
      total += H * total + data.charCodeAt(i)
    }
    total = total % this.table.length
    if (total < 0) {
      total += this.table.length - 1
    }
    return total
  }
  
  // 显示散列表中的数据
  showDistro() {
    console.table(this.table)
  }
  
  // 将数据存入散列表
  put(data) {
    const pos = this.betterHash(data)
    if (this.table[pos]) {
      this.table[pos].push(data)
    } else {
      this.table[pos] = []
      this.table[pos].push(data)
    }
  }
}

// test
const names = ['David', 'Jennifer', 'Donnie', 'Hysunny', 'Raymond', 'Cynthia', 'Mike', 'Clayton', 'Danny', 'Jonathan']
const table = new HashTable()
names.forEach(item => {
	table.put(item)
})
table.showDistro()
```



#### 线性检测法实现HashTable

```js
class HashTable {

  constructor() {
    this.table = new Array(137)
  }
  
  // 除留余数（简单方法）：当发生碰撞时，后面的值会覆盖之前的值
  simpleHash(data) {
    let total = 0
    for (let i = 0; i < data.length; i++) {
      total += data.charCodeAt(i)
    }
    return total % this.table.length
  }
  
  // 除留余数（霍纳算法）
  // 数组的长度最好在100以上，这样会让数据在散列表中分布得更加均匀
  // 为了避免碰撞，在给散列表一个合适的大小后，新的散列函数仍然先计算字符串中各字符的 ASCII 码值，不过求和时每次要乘以一个质数
  betterHash(data) {
    const H = 37
    let total = 0
    for (let i = 0; i < data.length; i++) {
      total += H * total + data.charCodeAt(i)
    }
    total = total % this.table.length
    if (total < 0) {
      total += this.table.length - 1
    }
    return total
  }
  
  // 显示散列表中的数据
  showDistro() {
    console.table(this.table)
  }
  
  // 将数据存入散列表
  put(data) {
    let pos = this.betterHash(data)
    if (this.table[pos]) {
      while(this.table[pos] !== undefined) {
        pos++
      }
    } 
    this.table[pos] = data
  }
}


// test
const names = ['David', 'Jennifer', 'Donnie', 'Hysunny', 'Raymond', 'Cynthia', 'Mike', 'Clayton', 'Danny', 'Jonathan']
const table = new HashTable()
names.forEach(item => {
  table.put(item)
})
table.showDistro()
```