---
title="程序结构导论(3): 递归"
subtitle="从前有座山, 山里有座庙, 庙里有个老和尚给小和尚讲故事, 讲的什么故事呢？从前有座山..."
description="递归函数是函数最强大的功能, 没有之一.
许多数据结构也都需递归的定义, 图灵机/可计算理论也依赖于递归的概念.
所以到底什么是递归?"
last_modified="2020-11-13 09:50:43"
avatar="/avatar/xhe.png"
publish=true
author="xhe"
license="by-nc-sa"
---

当一件事物的定义, 是由定义本身组成的, 我们就说这是递归. 一个常见的例子是[自相似](//baike.baidu.com/item/%E8%87%AA%E7%9B%B8%E4%BC%BC):

![wikipedia](//upload.wikimedia.org/wikipedia/commons/f/fd/Von_Koch_curve.gif)

上图来自维基百科. 当然这是一个空而大的概念, 举些实际的例子:

1. 上图中的自相似, 常见于 **分形(fractal)** 图形
2. **递归函数(recursive function)**, 重复利用代码的典范
3. 递归数据结构, 包括但不限于链表, 树等
4. **递归可枚举语言(recursive enumerable language)**, 计算理论中图灵机可运行的语言, 和语言学/计算理论/类型论紧密相关.
5. 递推数列, 递推级数, $f(a) = f(a-1) + f(a-2)$

递归的终极目标是用很少的定义, 描述一个可能有无限情况的事物.

数学归纳法中就隐藏着递归: 证明$N > 1$的情况假设了$N-1$的情况成立. 所以要证明$N = 12$成立, 就要证明$N = 11$成立, 所以需要$N = 10$成立, 以此类推, 直到已经证明过的$N = 1$. 本来有12个证明, 可你用数学归纳法, 只证明了$N = 1$和$N > 1$.

## 递归函数

你可能经常听说 **算法(algorithm)** 这个词, 它是指解决一个问题需要的步骤, 这节就来介绍一下如何用递归写算法. 下图是一个求和数学函数:

$$
sum(x) = \sum _ {i=1}^x i
$$

经过恒等变换, 可得递推式:

$$
sum(x) = \sum _ {i=1}^{x-1} i + x
= sum(x-1) + x \quad x > 1
$$

递推式只在$x > 1$时成立. 我们可以用递归函数来表示这个递归式, 完成求和功能:

```py
def sum(x):
	if x == 1:
		return 1
	return sum(x - 1) + x

sum(3) # 3 + 2 + 1 = 6
```

现在来看看它对应的程序框图:

![flow8](./flow_8.svg)

这个框图$x \neq 1$的时候, 完美的反应了递推式. 递推式只有在$x > 1$时成立, 因此, 我们在x减少到$x = 1$时, 不再使用递推关系, 而是直接返回$1$. 因为$sum(1) = 1$.

可以说递归函数的核心就是:

1. 大问题化小问题
2. 通过递归调用解决小问题
3. 额外几步计算, 通过小问题的答案给出大问题的答案

而递推就是一个典型的可以这么做的情况. 动态规划这类算法问题就常写出递推公式, 所以动态规划的问题可以用递归解决.

### 斐波那契

```py
def fib(x):
	if x == 1 or x == 2:
		return 1
	return fib(x-1) + fib(x-2)
```

对应的递推公式:

$$
f(1) = 1
\\
f(2) = 1
\\
f(x) = f(x-1) + f(x-2) \quad x > 2
$$

### 爬楼梯

有n级楼梯, 可以1次跨一级台阶, 或1次二级台阶, 有多少种爬法? 对应的递推公式:

$$
f(1) = 1
\\
f(2) = 1 + 1 = 2
\\
f(n) = f(n-1) + f(n-2)
$$

你可以看到这个公式就是fibnacci.

这个问题可以到着来思考, 假设你已经站在第n级. 爬楼梯要么一级, 要么两级, 所以要么是从n-1级迈一步到n级, 要么是直接从n-2跨两级到n级.

所以从n-1到n只有一种爬法, n-2级到n级只有一种爬法. 假设你已经知道, n-1级的爬法次数, 还有n-2级的爬法次数, 相加即得到n级的爬法次数.

你可能会说n-2到n级, 还可以迈两次两个一级, 那种情况迈一级之后就变成n-1到n级的情况, 已经重叠了, 所以不需要重复考虑.

### 练习

1 . (easy) 本节中的爬楼梯, 写出代码, 画出程序框图

有n级楼梯, 可以1次跨一级台阶, 或1次二级台阶, 有多少种爬法?

**答案**:

此处有[leetcode 原题](//leetcode-cn.com/problems/climbing-stairs/)

c写法:

```c
int f(int n) {
	if (n == 1 || n == 2) return n;
	return f(n-1) + f(n-2);
}

printf("res = %d\n", f(3));
// res = 3
```

py写法:

```python
def f(n):
	if n == 1 or n == 2:
		return n
	return f(n-1) + f(n-2)

print("res = ", f(3));
 res = 3
```

框图:

![sec3_1](./sec3_1.svg)

2 . (easy) 阶乘$f(x) = x!$, 写出代码

递推式:

$$
f(1) = 1
\\
f(x) = f(x-1) * x \quad x > 1
$$

c写法:

```c
int f(int n) {
	if (n == 1) return n;
	return f(n-1) * n;
}

printf("res = %d\n", f(3));
// res = 6
```

py写法:

```python
def f(n):
	if n == 1:
		return 1
	return f(n-1) * n

print("res = ", f(3))
 res = 6
```

3 . (easy) 有n级楼梯, 可以1次跨一级,二级,三级台阶, 有多少种爬法? 求代码

递推式:

$$
f(1) = 1
\\
f(2) = 2
\\
f(3) = 4
\\
f(x) = f(x-1) + f(x-2) + f(x-3) \quad x > 3
$$

跨一级只有一种走法, 跨两级有$1+1$和$2$两种走法. 跨三级: $1+1+1$, $1+2$, $2+1$, $3$.

c写法:

```c
int f(int n) {
	if (n == 1) return 1;
	if (n == 2) return 2;
	if (n == 3) return 4;
	return f(n-1) + f(n-2) + f(n-3);
}

printf("res = %d\n", f(4));
// res = 7
```

py写法:

```python
def f(n):
	if n == 1:
		return 1
	if n == 2:
		return 2
	if n == 3:
		return 4
	return f(n-1) + f(n-2) + f(n-3)

print("res = ", f(4))
 res = 7
```

4 . (easy) 有n级楼梯, 可以1次跨一级,三级台阶, 有多少种爬法? 写出代码

递推式:

$$
f(1) = 1
\\
f(2) = 1
\\
f(3) = 2
\\
f(x) = f(x-1) + f(x-3) \quad x > 3
$$

跨一级只有一种走法, 跨两级: $1+1$, 跨三级: $1+1+1$, $3$.

c写法:

```c
int f(int n) {
	if (n == 1 || n == 2) return 1;
	if (n == 3) return 2;
	return f(n-1) + f(n-3);
}

printf("res = %d\n", f(4));
// res = 3
```

py写法:

```python
def f(n):
	if n == 1 or n == 2:
		return 1
	if n == 3:
		return 2
	return f(n-1) + f(n-3)

print("res = ", f(4))
 res = 3
```

5 . (normal) 求杨辉三角, 第m行, 第n个数字(m/n从0开始)? 写出代码

递推式, 杨辉三角的某个数等于上一行左右两个数之和:

$$
f(m, n) = 1 \text{ if } n = 0
\\
f(m, n) = 1 \text{ if } m = n
\\
f(m, n) = f(m-1, n-1) + f(m-1, n) \text{ if }  n > 0 \cap m > 0
$$

c写法:

```c
int f(int m, int n) {
	if (n == 0 || m == n) return 1;
	return f(m-1, n-1) + f(m-1, n);
}

printf("res = %d\n", f(4, 3)); // 第五行第四个
// res = 4
```

py写法:

```c
def f(m, n):
	if n == 0 or m == n:
		return 1
	return f(m-1, n-1) + f(m-1, n)

print("res = ", f(4, 3))
" res = 4
```

6 . (normal) 从n个球中取出m个$C_n^m$，一共有多少可能性? 写出代码

递推式:

$$
C_n^m = 1 \text { if } n = m
\\
C_n^m = \frac{n!}{m!(n-m)!}
\\
= \frac{(n-1)!}{m!(n-1-m)!} \times \frac{n}{n-m}
\\
= C _ {n-1}^m \times \frac{n}{n-m} \text{ if } n-1 \geq m
$$

c写法:

```c
int f(int n, int m) {
	if (n == m) return 1;
	return f(n-1, m) * n / (n-m);
}

printf("res = %d\n", f(4, 3));
// res = 4
```

py写法:

```python
def f(n, m):
	if n == m:
		return 1
	return f(n-1, m) * n / (n-m)

print("res = ", f(4, 3))
" res = 4.0
```

7 . (normal) 汉诺塔最少移动次数, 写出代码

三个柱子：A, B, C. A柱子上有n个环，将n个环全部移动到C上最少移动次数. (大环不能放在小环上, A柱子上的环符合本要求)

**答案**:

一个环可直接移动.

两个环需要先将小的从A移动到B上, 大的从A移动到C上, 最后小的从B到C, 一共3次.

考虑有3个环, 我们需要先将前两个从A移动到B, 第三个从A移动到C上, 最后前两个从B到C, 一共$3 + 1 + 3 = 7$次. 这其中, 前两个从A移动到B, 从B移动到C的过程, 和两个环A移动到C的步骤是高度相似的.

考虑n个环, 将前n-1个从A移动到B, 第n个从A移动到C, 最后前n-1个B移动到C. 如果忽略第n个环, 你可以认识到将n-1个环从A移动到B, 从B移动到C的过程是一样的, 只是起始柱子, 目的柱子, 用来临时放环的空闲柱子不同而已.

因此, 可以得出递推式:

$$
f(1) = 1
\\
f(n) = f(n-1) * 2 + 1 \text{ if } n > 1
$$

为什么是最少呢? 因为若想将第n个环挪到C上, 第n个环上必然没有其他环, 且C上也没有其他环(第n个环是最大的). 所以要想实现这个过程, 必定需要将前n-1个环挪动到B上, 才能开始挪动第n个环, 而挪动第n个环后, 也只剩挪动前n-1个环到C上一种操作. 因此, 操作方法是唯一的(排除你一直小环来回挪不做正事的可能性), 所以也一定是最少的.

c写法:

```c
int f(int n) {
	if (n == 1) return 1;
	return f(n-1) * 2 + 1;
}

printf("res = %d\n", f(4));
// res = 15
```

py写法:

```python
def f(n):
	if n == 1:
		return 1
	return f(n-1) * 2 + 1

print("res = ", f(4))
" res = 15
```

8 . (hard) 爬楼梯的递归代码中存在重复计算吗? 可以在仍然使用递归的方法下避免重复计算吗? 若可以, 给出代码

存在, 可以避免.

c写法:

```c
int computed[1024] = {1, 2, 0};

int f(int n) {
	if (computed[n] != 0) return computed[n];
	int r = f(n-1) + f(n-2);
	computed[n] = r;
	return r;
}

printf("res = %d\n", f(3));
// res = 3
```

9 . (hard) 下面的c语言代码是不是未定义行为? 为什么

```c
int a[3] = {1, 2, 3};
int i = 1;
a[i++] = i;
```

**答案**:

是. 根据c语言标准, `a[i++] = i;`只有两个序列点, 赋值`=`和`;`. 赋值后直接通过`;`结束语句, 这两个序列点之间没有其他操作, 所以不存在问题(`a=3,c=4;`这样写就有其他操作了).

在向数组赋值前, 需要计算`i++`和`i`的值, 但因为赋值前没有其他序列点, 所以`i++`和`i`的求值顺序不确定先后, 而且本行为未定义.

这行代码可能执行为`a[2] = 1`, 也可能是`a[1] = 2`, 或者直接让你的电脑死机, 这都是执行预期内的行为.

10 . (hard) 补完下列c程序:

```c
int sum(int a) {
	// 写出代码
}
```

函数接受参数a, 返回结果$sum(a) = \sum_{i=1}^a i$, 只许使用递归, 变量, `&&`, `||`, `==`, `=`, `+`, `-`.

提示: 试画出`&&`和`||`的程序框图.

**答案**:

```c
int sum(int a) {
	int r = 1;
	a == 1 || (r = sum(a-1) + a);
	return r;
}
```

## 递归数据结构

这里说的树是指一种**数据结构(data structrue)**, 树是一类数据结构的总称, 因结构图形似树而得名. 下图展示一个**二叉树(binary search tree)**.

![BST](./sec3_2.svg)

二叉树从最顶部的根节点(上图的30)出发, 每个节点至多两个子节点, 最少零个; 且含有一个值, 本例中是一个数字. 而**二叉搜索树(binary search tree)**的节点, 还要满足, 左子节点的值 < 父节点的值 < 右子节点的值 (20 < 30 < 45). 这个性质使得可以通过二分的方法查找树中存不存在某个值.

上图的树有7个节点, 但每个节点除了它的数字不同, 定义都是类似的.

### 构建, 插入, 删除

```python
class Node:
	def __init__(self, data):
		self.left = None
		self.right = None
		self.data = data

	def Print(self)
		print(self.data)

a = Node(3)
a.Print() # 3
```

上面的程序定义了一个二叉树节点 **类(class)**, 这属于面向对象编程. 一个类有自己的成员: 一些变量, 还有一些方法. `self`是指类本身, 用`self.xxx`就可以访问/修改一个类的成员. 类本身也是一个变量, 可以赋给它一个名字, 这时新名字`a`和`self`等价.

通过`def xxx(self, xxx)``的形式, 即第一个参数为self的函数, 就定义一个类的方法. 之后可以通过`self.xxx()`来调用这个函数, 第一个参数, 也就是类本身被自动传入.

你不需要太深入, 能照猫画虎就好. 下面来定义一个简单的二叉树, 首先是根节点:

```python
root = Node(3)
```

根节点右侧指向5:

```python
root.right = Node(5)
```

删除5, 也就是根节点右侧没有指向:

```python
root.right = None
```

### 遍历

```python
root = Node(4)
root.left = Node(2)
root.left.left = Node(1)
root.left.right = Node(3)
root.right = Node(10)
```

构建如上二叉树, 我要进行遍历, 也就是访问树上每一个节点:

```python
class Node:
	# ...

	def Traversal(self):
		if self.left is not None:
			self.left.Traversal()
		self.Print()
		if self.right is not None:
			self.right.Traversal()

root.Traversal() # 1 2 3 4 10
```

这是一个递归方法/递归函数. 它首先检查当前节点是不是指向一个节点, 如果是, 就对左节点和它下面所有的节点, 或者说左子树进行遍历. 接着是打印当前节点的值, 最后再检查右节点. 如果对上面的二叉树调用, 就是打印出左子树1/2/3后, 打印4, 最后是右子树10. 因为当前节点在左子树和右子树中间被打印, 所以叫 **中序遍历(in-order traversal)**.

实际上还有 **前序遍历(pre-order traversal)**:

```python
class Node:
	# ...

	def Traversal(self):
		self.Print()
		if self.left is not None:
			self.left.Traversal()
		if self.right is not None:
			self.right.Traversal()

root.Traversal() # 4 1 2 3 10
```

以及 **后序遍历(post-order traversal)**:

```python
class Node:
	# ...

	def Traversal(self):
		if self.left is not None:
			self.left.Traversal()
		if self.right is not None:
			self.right.Traversal()
		self.Print()

root.Traversal() # 1 2 3 10 4
```

### 二分搜索

上一节我们构建的二叉树, 符合二叉搜索树的定义.你可以看到以中序遍历的时候, 输出的数字都是顺序的, 这样我们就能做二分了.

比如你有一个有序数组, 而你要查找数组中存不存在1: 假设你拿到的数组是`[1 2 3 4 5]`, 你可以先拿出来数组的中位数3, 发现它比1大, 所以结果必定在前两个`[1 2]`中, 这样我们就把问题规模一步缩小了一半. 所以由于二叉搜索树也是有序的, 你也可以做类似的事情:

```python
class Node:
	# ...

	def Search(self, data):
		if data < self.data:
			if self.left is None:
				print("not found")
				return
			return self.left.Search(data)
		elif data == self.data:
			return self
		else:
			if self.right is None:
				print("not found")
				return
			return self.right.Traversal()

root.Search(2) # Node(2)
```

如果搜索值小于当前节点值, 那他一定在左子树中; 等于当前值, 那就是当前节点; 大于当前值, 就是右子树. 如果在寻找过程中, 发现要进入的左子树, 右子树是None, 那就说明树中没有这个值.

### 平衡

虽然二叉搜索树是有序的, 但不一定是平衡的:

```python
root = Node(4)
root.left = Node(3)
root.left.left = Node(2)
root.left.left.left = Node(1)
```

![no-balance](./sec3_nobtree.svg)

上面的二叉树有四层, 四层总共可以存 $1 + 2 + 4 + 8 = 15$ 个节点, 但只存了4个节点. 这样如果要查找1, 就需要从第一层, 走到第四层. 如果是下面这样的树:

```python
root = Node(3)
root.left = Node(2)
root.left.left = Node(1)
root.right = Node(4)
```

![balance](./sec3_btree.svg)

就只需要从第一层走到第三层. 所以二叉树的 **平衡(balance)** 也很重要: 左右子树的树高不应该相差超过一层. 我们有自动化平衡的方法, AVL和红黑树. 这些树叫做 **自平衡树(self-balance tree)**.

### 练习

1 . (easy) 定义一个二叉树节点对象

2 . (normal) 添加前序遍历的方法.

3 . (normal) 添加中序遍历的方法.

4 . (normal) 添加后序遍历的方法.

5 . (normal) 添加二分搜索方法.

6 . (hard) 非递归前序遍历

7 . (hard) AVL树的插入和删除
