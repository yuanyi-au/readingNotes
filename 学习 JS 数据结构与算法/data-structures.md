# 数据结构

## 数组

### 增加和删除元素

push()|unshift()|pop()|shift()|splice()
--|--|--|--|--

### 二维和多维数组

JS 只支持一维数组，并不支持矩阵

可以用 for 语句嵌套输出多维数组

```
//输出二维数组
function printMatrix(myMatrix) {
    for (var i=0; i<myMatrix.length; i++){
	    for (var j=0; j<mtMatrix[i].length; j++){
		    console.log(myMatrix[i][j]);
		}
	}
}
```

### 数组合并

concat()|
--|

### 迭代器函数

every()|some()|forEach()|map()|filter()|reduce()
--|--|--|--|--|--

### 搜索和排序

reverse()|sort()|indexOf()|lastIndexOf()
--|--|--|--

字符串排序比较的是字符的 ASCII 值

### 输出数组为字符串

toString()|join()
--|--

## 栈

### 栈的实现

```
function Stack() {
    var items = [];
	
	this.push = function(element){
	    items.push(element);
	};
	
	this.pop = function() {
	    return items.pop();
	};
	
	this.peek = function() {
	    return items[items.length-1];
	};
	
	this.isEmpty = function() {
	    return items.length == 0;
	};
	
	this.size = function() {
	    return items.length;
	};
	
	this.clear = function() {
	    items = [];
	};
	
	this.print = function() {
	    console.log(items.toString());
	};
}
```

## 队列

### 队列的实现

```
function Queue() {
    var items = [];
	
	this.enqueue = function(element){
	    items.push(element);
	};
	
	this.dequeue = function() {
	    return items.shift();
	};
	
	this.front = function() {
	    return items[0];
	};
	
	this.isEmpty = function() {
	    return items.length == 0;
	};
	
	this.size = function() {
	    return items.length;
	};
	
	this.clear = function() {
	    items = [];
	};
	
	this.print = function() {
	    console.log(items.toString());
	};
}
```

### 优先队列

```
function PriortyQueue(){

    var items = [];
	
	function QueueElement (element, priority){
	    this.element = element;
		this.priority = priority;
	}
	
	this.enqueue = function(element,priority){
	    var queueElement = new QueueElement(element,priority);
		
		if (this.isEmpty()){
		    items.push(queueElement);
		} else{
		    var added = false;
			for (var i=0; i<items.length; i++){
			    if (queueElement.priority < items[i].priority){
				    items.splice(i,0,queueElement);
					added = true;
					break;
				}
			}
		    if (!added){
	    	    items.push(queueElement);
	    	}
		}
	};
	
//其他方法和默认的 Queue 实现相同
}
```

### 循环队列

```
// 模拟击鼓传花
function hotPotato (nameList, num){
    
	var queue = new Queue();
	
	for (var i=0; i<nameList.length; i++){
	    queue.enqueue(nameList[i]);
	}
	
	var eliminated = '';
	while (queue.size() > 1){
	    for (var i=0; i<num; i++){
		    queue.enqueue(queue.dequeue());
		}
		eliminated = queue.dequeue();
		console.log(eliminated + '在击鼓传花游戏中被淘汰');
	}
	
	returm queue.dequeue();
}

var names = ['John','Jack];
var winner = hotPotato(name, 7);
console.log('胜利者：' + winner);`
```

## 链表  

### 创建一个链表

```
//链表的结构骨架
function LinkedList(){
    var Node = function(element){
	    this.element = element;
		this.next = null;	
	};
	
	var length = 0;
	var head = null; 
	
	this.append = function(element){};
	this.insert = function(position, element){};
	//其他方法
}
```

```
//向链表尾部追加元素
this.append = function(element){
    
	var node = new Node(element),
	    current;
		
	if (head === null){ // 列表中第一个节点
	    head = node;
	} else{
	    current = head;
		
		//循环列表，直到找到最后一项
		while(current.next){
		    current = current.next;
		}
		
		//找到最后一项，将其 next 赋为 node，建立连接
		current.next = node;
	}
	
	length++; //更新列表长度
};
```

### 双向链表

和普通链表的区别在于每个节点（除第一个和最后一个节点）可以链向前一个节点和后一个节点两个方向

### 循环链表

和普通链表的区别在于最后一个元素指向下一个元素的指针 (tail.next) 不是引用 null，而是指向第二个元素 (head) 

## 集合

集合是由一组无序且唯一（即不能重复）的项组成的，也就是 ES6 中的 Set 类

### 集合操作

并集 交集 差集 子集

## 字典和散列表

### 字典

字典存储的是 [键,值] 对，也就是 ES6 中的 Map 类

### 散列表

#### 解决散列表中的冲突

## 树

### 二叉树和二叉搜索树

二叉搜索树：左子节点比父节点小，右子节点比父节点大

### 树的遍历

#### 中序遍历

```
this.inOrderTraverse = function(callback){
    inOrderTraverseNode(root, callback);
};

var inOrderTraverseNode = function (node, callback){
    if (node !== null){
	    inOrderTraverseNode(node.left, callback);
		callback(node, key);
		inOrderTraverseNode(node.right, callback);
	}
};

function printNode(value){
    console.log(value);
}
tree.inOrderTraverse(printNode);
```

#### 前序遍历

```
this.preOrderTraverse = function(callback){
    preOrderTraverseNode(root, callback);
};

var preOrderTraverseNode = function (node, callback){
    if (node !== null){
		callback(node, key);
	    preOrderTraverseNode(node.left, callback);
		preOrderTraverseNode(node.right, callback);
	}
};

function printNode(value){
    console.log(value);
}
tree.preOrderTraverse(printNode);
```

#### 后序遍历

```
this.postOrderTraverse = function(callback){
    postOrderTraverseNode(root, callback);
};

var postOrderTraverseNode = function (node, callback){
    if (node !== null){
	    postOrderTraverseNode(node.left, callback);
		postOrderTraverseNode(node.right, callback);
		callback(node, key);
	}
};

function printNode(value){
    console.log(value);
}
tree.postOrderTraverse(printNode);
```

### 搜索树的值

#### 最小值和最大值

#### 特定值

#### 移除节点

### 其他树

平衡二叉树

## 图

图是一组由边连接而成的节点，V 为顶点，E 为边，G = (V, E)

### 图的表示

邻接矩阵 邻接表 关联矩阵

### 图的实现

### 图的遍历

#### 广度优先搜索 BFS

最短路径

#### 深度优先搜索 DFS

拓扑排序
