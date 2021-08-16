# 算法

## 排序和搜索算法

### 排序算法

#### 冒泡排序

```
this.bubbleSort = function(){
    var length = array.length;
	for (var i=0; i<length; i++){
	    for (var j=0; j<length-1-i; j++){
		    if (array[j] > array[j+1]){
			    swap(j, j+1}
			}
		}
	}
};
```

#### 选择排序

```
this.selectionSort = function(){
    var length = array.length,
	    indexMin;
	for (var i=0; i<length-1; i++){
	    indexMin = i;
		for (var j=i; j<length; j++){
		    if(array[indexMin]>array[j]){
			    indexMin = j;
			}
		}
		if (i !== indexMin){
		    swap(i, indexMin);
		}
	}
};
```

#### 插入排序

```
this.insertionSort = function(){
    var length = array.length,
	    j, temp;
	for (var i=1; i<length; i++){
	    j = i;
		temp = array[i];
		while (j>0 && array[j-1] > temp){
		    array[j] = array[j-1];
			j--;
		}
		array[j] = temp;
	}
};
```

#### 归并排序

```

```

#### 快速排序

```
this.quickSort = function(){
    quick(array, 0, array.length - 1);
};

var quick = function(array, left, right){
    
	var index;
	
	if (array,length > 1){
	    
		index = partition(array, left, right);
		
		if (left < index - 1){
		    quick(array, left, index - 1);
		}
		
		if (index < right){
		    quick(array, index, right);
		}
	}
};
```

### 搜索算法

#### 顺序搜索

```
this.sequentialSearch = function(item){
    for (var i=0; i<array.length; i++){
	    if (item === array[i])
		return i;
		}
	}
	return -1
};
```

#### 二分搜索

```
this.binarySearch = function(item){
    this.quickSort();
	
	var low = 0,
	    high = array.length-1,
		mid, element;
		
	while (low <= high){
	    mid = Math.floor((low + high)/2);
		element = array[mid];
		if (element < item){
		    low = mid + 1;
		} else if (element > item){
		    high = mid - 1;
		} else {
		    return mid;
		}
	}
	return -1;
};
```

## 算法补充知识

### 递归

#### 栈溢出

### 斐波那契数列

```
function fibonacci(num){
    if (num === 1 || num === 2){
	    return 1
	}
	return fibonacci(num - 1) + fibonacci(num - 2);
}
```

### 动态规划

背包问题 最长公共子序列 矩阵链相乘 硬币找零 图的全源最短路径

### 贪心算法

# 时间复杂度速查表

## 数据结构

数据结构| |一般情况| | |最差情况| |
--|--|--|--|--|--|--
-|插入|删除|搜索|插入|删除|搜索
数组/栈/队列|O(1)|O(1)|O(n)|O(1)|O(1)|O(n)
链表|O(1)|O(1)|O(n)|O(1)|O(1)|O(n)
双向链表|O(1)|O(1)|O(n)|O(1)|O(1)|O(n)
散列表|O(1)|O(1)|O(1)|O(n)|O(n)|O(n)
二分搜索树|O(logn)|O(logn)|O(logn)|O(n)|O(n)|O(n)

## 图

节点/边的管理方式|储存空间|增加节点|增加边|删除顶点|删除边|轮询
--|--|--|--|--|--|--
相邻列表|O(\|V\|+\|E\|)|O(1)|O(1)|O(\|V\|+\|E\|)|O(\|E\|)|O(\|V\|)
相邻矩阵|O(\|V\|^2)|O(\|V\|^2)|O(1)|O(\|V\|^2)|O(1)|O(1)

## 排序算法

排序算法|平均情况|最好情况|最差情况
--|--|--|--
冒泡排序|O(n^2)|O(n)|O(n^2)
选择排序|O(n^2)|O(n^2)|O(n^2)
插入排序|O(n^2)|O(n)|O(n^2)
归并排序|O(nlogn)|O(nlogn)|O(nlogn)
快速排序|O(nlogn)|O(nlogn)|O(n^2)

## 搜索算法

算法|数据结构|最差情况
--|--|--
顺序搜索|数组和链表|O(n)
二分搜索|排好序的数组或二分搜索树|O(logn)
深度优先搜索 DPS |\|V\|为顶点而\|E\|为边的图|O(\|V\|+\|E\|)
广度优先搜索 BFS |\|V\|为顶点而\|E\|为边的图|O(\|V\|+\|E\|)