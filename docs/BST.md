
> 简书地址: https://www.jianshu.com/p/45952104878a

### 二叉查找树

> 二叉查找树（BST）：一种特殊的二叉树，相对较小的值保存在左节点中，较大的值保存在右节点中。

实现：

```js
class Node {

  constructor(data) {
    // Node既保存数据也保存和其他节点的链接
    this.data = data
    this.left = null
    this.right = null
  }
  
  toString() {
    return this.data.toString()
  }
}

class BST {

  constructor() {
    this.root = null
  }
  
  // 向树中加入新节点
  insert(...datalist) {
    for (let data of datalist) {
      const node = new Node(data)
      if (this.root === null) {
        this.root = node
      } else {
        let current = this.root
        let parent
        while(true) {
          parent = current
          if (data < current.data) {
            current = current.left
            if (current === null) {
              parent.left = node
              break
            }
          } else {
            current = current.right
            if (current === null) {
              parent.right = node
              break
            }
          }
        }
      }
    }
  }
  
  // 前序遍历：根节点 -> 左子树 -> 右子树
  preOrder() {
    const order = node => {
      if (node !== null) {
        console.log(`${node.data} `)
        order(node.left)
        order(node.right)
      }
    }
    order(this.root)
  }
  
  // 中序遍历：左子树 -> 根节点 -> 右子树
  inOrder() {
    const order = node => {
      if (node !== null) {
        order(node.left)
        console.log(`${node.data} `)
        order(node.right)
      }
    }
    order(this.root)
  }
  
  // 后序遍历：左子树 -> 右子树 -> 根节点
  postOrder() {
    const order = node => {
      if (node !== null) {
        order(node.left)
        order(node.right)
        console.log(`${node.data} `)
      }
    }
    order(this.root)
  }
  
  // 找到最小值
  findMin() {
    let current = this.root
    while(current.left !== null) {
      current = current.left
    }
    return current.data
  }
  
  // 找到最大值
  findMax() {
    let current = this.root
    while(current.right !== null) {
      current = current.right
    }
    return current.data
  }
  
  // 找到指定值
  find(data) {
    let current = this.root
    while(current !== null) {
      if (data === current.data) {
        return current
      } else if (data < current.data) {
        current = current.left
      } else if (data > current.data) {
        current = current.right
      }
    }
  }
  
  // 找到指定值
  find(data) {
    let current = this.root
    while(current !== null) {
      if (data === current.data) {
        return current
      } else if (data < current.data) {
        current = current.left
      } else if (data > current.data) {
        current = current.right
      }
    }
    return null
  }
  
  // 删除指定值
  remove(data) {
    const findMin = node => {
      let current = node
      while(current.left !== null) {
        current = current.left
      }
      return current
    }
    const removeNode = (node, data) => {
      if (node === null) {
        return null
      }
      if (data === node.data) {
        // 没有子节点的节点
        if (node.left === null &&
            node.right === null) {
          return null
        }
        // 没有左子节点的节点
        if (node.left === null) {
          return node.right
        }
        // 没有右子节点的节点
        if (node.right === null) {
          return node.left
        }
        // 有两个子节点的节点
        // 找到右子树上的最小值，并用该值创建一个临时节点
        const tempNode = findMin(node.right)
        // 将临时节点上的值复制到待删除节点
        node.data = tempNode.data
        // 然后再删除临时节点
        node.right = removeNode(node.right, tempNode.data)
        return node
      } else if (data < node.data) {
        // 移至当前节点的左子节点进行比较
        node.left = removeNode(node.left, data)
        return node
      } else {
        // 移至当前节点的右子节点进行比较
        node.right = removeNode(node.right, data)
        return node
      }
    }
    this.root = removeNode(this.root, data)
  }
  
  // 返回BST中节点的个数
  count() {
    let arr = []
    const order = node => {
      if (node !== null) {
        order(node.left, arr)
        order(node.right, arr)
        arr.push(node.data)
      }
    }
    order(this.root)
    return arr.length
  }
  
  // 返回BST中边的个数
  edgesCount() {
    let count = 0
    const edgesCount = node => {
      if (node.left !== null && node.right !== null) {
        count++
        edgesCount(node.left)
        count++
        edgesCount(node.right)
      } else if (node.left !== null) {
        count++
        edgesCount(node.left)
      } else if (node.right !== null) {
        count++
        edgesCount(node.right)
      }
    }
    edgesCount(this.root)
    return count
  }
}

// test
const bst = new BST()
bst.insert(23,16,99,30,18,3,120,45)
console.log(bst.count())	// 8
bst.preOrder() // 23 16 3 18 99 30 45 120
bst.inOrder() // 3 16 18 23 30 45 99 120
bst.postOrder()	// 3 18 16 45 30 120 99 23
bst.remove(99)
console.log(bst.count())	// 7
console.log(bst.findMin())	// 3
console.log(bst.findMax())	// 120
console.log(bst.find(30))		// Node {data: 30, left: null, right: Node}
console.log(bst.edgesCount())	// 6
```