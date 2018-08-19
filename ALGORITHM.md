# 1. 做算法题步骤
1. 仔细审题，思考思路
2. 向面试官讲述思路，获得认同
3. 注意边界值，写bug free的代码

# 2. 准备算法题的步骤
1. 先掌握简单算法
2. 再掌握算法的主要解题思路
3. 针对一道简单的题，多思考它的follow up
4. 不断手写，保证没有错误

**算法分类：**
* 排序
  * 冒泡排序 O(N2)
  关键词：乌龟和兔子
  原理：两个for循环嵌套，让最大的那个一次遍历沉底，最小的多次遍历浮上来
  优化：1. 内部for循环没有执行数组变换，则排序结束
       2. 每次可以找到j+1的新内部循环基准点
       3. 可以同时做最大和最小的双重对向冒泡，叫鸡尾酒排序
  * 插入排序 O(N2)
  原理：构建一个有序的子数组，然后不断把新的数据插入到有序子数组的相应位置。
  * 快速排序 O(NlogN) 已知最快的排序，不稳定
  关键词：哨兵
  原理：随机取一个值作为哨兵，比哨兵大的放在右边，比哨兵小的放在左边。重复上述过程，直到选取了所有哨兵并划分。
  变形：快排可以用来求第K大的数
  * 选择排序 O(N2)
  关键词：谁大谁出列
  原理：两次for循环，谁大/小谁出列，放到队伍的最后，然后在剩余的部分中再来一遍
  * 希尔排序 O(NlogN) 不稳定
  关键词：增量
  原理：插入排序的延伸。将数据按照增量分成N个部分，然后对每个部分进行插入排序，不断减少增量，直到成为最初的数组，增量的初始值为arr.length/2，之后每次/2
  * 堆排序 O(NlogN) 不稳定
  原理：选择排序的延伸。构建初始大顶堆，交换堆顶元素和末尾元素，其他部分继续构建。


* 二叉树，分治算法
* 空间换时间
* 数组
* 字符串
  * str.charAt(char)
  * str.concat(str1,str2...)
  * str.indexOf()/str.lastIndexOf()/str.search(regexp)
  * str.match(regexp)/str.match(regexp, str2)
  * str.valueOf()返回最适合该对象类型的原始值，str.toString()该对象的原始值以字符串的形式返回
  * str.slice(start, stop)/str.substring(start, stop)/str.substr(start, length)
  * str.split(separator)
* DFS/BFS
* DP
* 利用Hash，Stack，Queue

**简单算法：**
* 手写快排
```
function quickSort(arr) {
  if(arr.length < 2) {
    return arr;
  }
  let pivotIndex = arr.length - 1; 
  let pivot = arr.splice(pivotIndex,1)[0]; // 关键，一定要把哨兵从原数组中扣出来
  let left = [];
  let right = [];
  for (let i = 0; i < arr.length; i++) {
    const item = arr[i];
    if (item < pivot) {
      left.push(item);
    } else {
      right.push(item);
    }
  }
  return quickSort(left).concat([pivot], quickSort(right));
}
```
* 二分查找
* 链表逆序
function reverse(headList){
  var pNode = headList;
  var pPrev = null;
  var pNext;

  while(pNode != null){
      pNext = pNode.next;
      pNode.next = pPrev; //reverse

      pPrev = pNode;
      pNode = pNext;
  }
  return pPrev ;  //return newHead
}
* 一点大数据量