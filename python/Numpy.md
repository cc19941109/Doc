# Numpy

参考:

> [https://docs.scipy.org/doc/numpy-1.13.0/user/quickstart.html](https://docs.scipy.org/doc/numpy-1.13.0/user/quickstart.html)

## 基础和通用函数

1. 创建 N-dimensional array (ndarray)

	```
	import numpy as np

	a = np.array([[1,2],
	             [3,4]])
	
	b = np.array([[0,1],
	             [2,0]])
	             
	x = np.arange(15).reshape(3, 5)
	
	# [[ 0  1  2  3  4]
	#  [ 5  6  7  8  9]
	#  [10 11 12 13 14]]
	
	```
	
2. 属性
	
	> shape 维度
	> ndim 秩
	> dtype 数组元素的类型 int64 即64位int,float64即64位 float, 均为8字节
	> itemsize 每个元素的字节大小 
	> size 元素的总个数
	
	```
	a = np.arange(15).reshape(3, 5)
	
	print(a.shape)
	print(a.ndim)
	print(a.dtype.name)
	print(a.itemsize)
	print(a.size)
	
	# (3, 5)  2  int64  8  15 
	```
	
3. zeros,ones,empty
	
	> **返回** ```ndarray```
	
	> **默认** ```dtype``` **为** ```float64```
	
	> empty_like(),ones_like(),zeros_like() 
	
	> 比如 zeros_like(a, dtype=None, order='K', subok=True)
	
	```
	# zeros(shape, dtype=float, order='C')
	# dtype 其实是 float64
	
	s = (2,2)
	np.zeros(s)
   # array([[ 0.,  0.],
   #        [ 0.,  0.]])
	```

	```
	y = np.ones([3,2],dtype=np.int64)
	
	# [[1 1]
	#  [1 1]
	#  [1 1]]
	```	
	
	```
	# np.empty()
	#  Return a new array of given shape and type, without initializing entries.
	
	z = np.empty([3,4])
	
	```


4. arange

	```
	# arange([start,] stop[, step,], dtype=None)
	# return  Array of evenly spaced values.
	
	x = np.arange(0, 150, 2,dtype=np.float64)
	
	# 禁用自动省略
	# np.set_printoptions(threshold=np.nan)
	
	```

5. dot 

	```
	b = np.array([[1,2],[2,3]])
	print(b**2)
	
	#[[1 4]
 	  [4 9]]
	```
	
	> 点积
	
	```
	a = np.array([[1,2],
             [3,4]])

	b = np.array([[0,1],
	             [2,0]])
	
	print(a*b)
	# [[0 2]
	#  [6 0]]
	
	x = np.dot(a,b)
	print(x)
	# [[4 1]
 	#  [8 3]]
	
	```

6. dtype = complex

	> 默认 complex128

	```
	c = np.array([1+1.j, 2-2.3j], [3, 4]], dtype=complex)
	print(c)
	
	# [[ 1.+1.j   2.-2.3j]
 	#  [ 3.+0.j    4.+0.j]]
	```

7. strides

	```
	a = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.int64, order='c')
	print(a.strides)
	
	b = np.array([[1, 2, 3], [4, 5, 6]], dtype=np.int64, order='f')
	print(b.strides)
	
	c = np.array([[1, 2, 3,4], [4, 5, 6,7]], dtype=np.int64, 	order='f')
	print(c.strides)
	
	# (24, 8)
	# (8, 16)
	# (8, 16)
	```

8. *= +=

	```
	a = np.ones((2,3), dtype=int)
	b = np.random.random((2,3))
	
	a *=3
	#  a 变成全是3 的矩阵
	
	b +=a
	# b 的每个元素加3
	
	a+=b
	# TypeError: Cannot cast ufunc add output from dtype('float64') 
	# to dtype('int64') with casting rule 'same_kind'

	```


9. exp

	> 欧拉公式e^(ix)=cosx+isinx

	```
	a = np.ones((3,4))
	x = np.exp(a)-1
	print(x)
	# 每个元素 均为 e-1
	
	y = np.exp(a*2)
	print(y)
	# 每个元素 =  e 的(2*原始元素)次方
	
	z = np.exp(a*1j)
	# 这里要使用欧拉公式
	```

10. linspace 均分,产生数组

	> linspace(start, stop, num=50, endpoint=True, retstep=False, dtype=None)
	
	> Return evenly spaced numbers over a specified interval.

	```
	start : scalar
        The starting value of the sequence.
    stop : scalar
        The end value of the sequence, unless `endpoint` is set to False.
        In that case, the sequence consists of all but the last of ``num + 1``
        evenly spaced samples, so that `stop` is excluded.  Note that the step
        size changes when `endpoint` is False.
    num : int, optional
        Number of samples to generate. Default is 50. Must be non-negative.
    endpoint : bool, optional
        If True, `stop` is the last sample. Otherwise, it is not included.
        Default is True.
    retstep : bool, optional
        If True, return (`samples`, `step`), where `step` is the spacing
        between samples.
    dtype : dtype, optional
        The type of the output array.  If `dtype` is not given, infer the data
        type from the other input arguments.		
	```

	```
	a = np.linspace(0,np.pi,4)
	print(a)
	# [ 0.          1.04719755  2.0943951   3.14159265]

	b = np.linspace(0,np.pi,3,endpoint=False)
	print(b)
	# [ 0.          1.04719755  2.0943951 ]

	```

11. ndarray: sum,max,min,cumsum

	> cumsum: cumulative sum along each row 沿行累计和
	
	> axis=0 一列一列算 axis=1 一行一行算
	
	```
	a = np.arange(0, 15).reshape((3, 5))
	print(a)
	# [[ 0  1  2  3  4]
 	#  [ 5  6  7  8  9]
  	#  [10 11 12 13 14]]
	
	print(a.max(axis=1))
	# [ 4  9 14]
	
	print(a.min(axis=0))
	# [0 1 2 3 4] 
	
	print(a.sum(axis=0))
	# [15 18 21 24 27]
	
	print(a.sum(axis=1))
	# [10 35 60]
	
	print(a.cumsum(axis=1))
	# [10 21 33 46 60]]
	
	```

12. Universal Functions (ufunc)

	> sin cos exp sqrt add dot
	


	
##  Indexing, Slicing and Iterating 索引切片和迭代

1. 一维 索引/分片/迭代

	> One-dimensional arrays can be indexed, sliced and iterated over, much like lists and other Python sequences.
	
	
	```
	# 索引
	print(a[8])
	# 8
	
	# 分片
	a = np.arange(10)
	# [0 1 2 3 4 5 6 7 8 9]
	
	print(a[1:7:2])
	# [1 3 5]
	
	print(a[::-2])
	# [9 7 5 3 1]
	```


	```
	<!--迭代-->
	for i in a:
   		print(i**2,end=" ")	
   	
   	# 0 1 4 9 16 25 36 49 64 81
	```

	```
	# 赋值
	a[:6:2] = -10
	# [-10   1 -10   3 -10   5   6   7   8   9]
	
	```


2. 多维数组

	> Multidimensional arrays can have one index per axis. These indices are given in a tuple separated by commas:


	```
	
	def lin(x, y):
    	return 10 * x + y
	b = np.fromfunction(lin,(5,4),dtype=int)
	
	#  [[ 0  1  2  3]
	#   [10 11 12 13]
	#   [20 21 22 23]
	#   [30 31 32 33]
	#   [40 41 42 43]]
	
	```

	```
	print(b[2,3])
	# 23
	
	print(b[0:3,2])
	# [2 12 22]
	
	print(b[:,1])
	# [ 1 11 21 31 41]
		
	print(b[1:3,1:3])
	#[[11 12]
 	# [21 22]]
	```
	
	> 当使用 b[i]时会被当做 b[i,:]
	
	> 当使用 ```...``` 时会代表许多行
	
	> The dots (...) represent as many colons as needed to produce a complete indexing tuple. 
	
	```
	print(b[-1])
	# [40 41 42 43]
		
	print(b[-2,...])
	# [30 31 32 33]
	
	```
	
	```
	x[1,2,...] is equivalent to x[1,2,:,:,:],
	x[...,3] to x[:,:,:,:,3] and
	x[4,...,5,:] to x[4,:,:,5,:].

	```

3.  三维数组

	```
	c = np.array([[[0,1,2],[10,12,13]],
               [[100,101,102],[110,112,113]]])
	c.shape
	# (2,2,3)
	
	```
	
	```
	# c
	[[[  0   1   2]
	  [ 10  12  13]]
	
	 [[100 101 102]
	  [110 112 113]]]
	
	```
	
	
	```
	# 分片
	print(c[0,...])
	print(" - - - - - - - - ")
	print(c[:,0,:])
	print(" - -- -- - -- --  ")
	print(c[:,:,0])
	
	print(' -- - - - --  --  ')
	print(c[...,2])
	
	```
	
	```
	# 结果
	[ 0  1  2]
	 [10 12 13]]
	 - - - - - - - - 
	[[  0   1   2]
	 [100 101 102]]
	 - -- -- - -- --  
	[[  0  10]
	 [100 110]]
	 -- - - - --  --  
	[[  2  13]
	 [102 113]]
	```

	```
	# 遍历,迭代
	for element in c.flat:
    	print(element,end=" ")
	
	# 0 1 2 10 12 13 100 101 102 110 112 113 
	```
	
## Shape Manipulation 操作形状


1. 创建随机数组

	```
	# 注意这里 3,4 外的括号要加上
	a = np.floor(10*np.random.random((3,4)))

	```

	```
	# such as 由于是随机数组每次生成的数组都不一样
	[[ 6.  4.  9.  0.]
 	[ 1.  2.  7.  0.]
 	[ 9.  9.  1.  8.]]
	
	```


2. 返回改变的数组,不改变原数组

	> The shape of an array can be changed with various commands. Note that the following three commands all return a modified array, but do not change the original array:
	
	1. ravel

		>  returns the array, flattened

		```
		print(a.ravel())
		# such as ... 
		#  [ 3.  6.  1.  3.  2.  0.  2.  8.  2.  1.  9.  3.]
		```
		
		> The order of the elements in the array resulting from ravel() 		is normally “C-style”, that is, the rightmost index “changes 		the fastest”, so the element after a[0,0] is a[0,1]. If the 		array is reshaped to some other shape, again the array is 		treated as “C-style”. NumPy normally creates arrays stored in 		this order, so ravel() will usually not need to copy its 		argument, but if the array was made by taking slices of another 		array or created with unusual options, it may need to be 		copied. The functions ravel() and reshape() can also be 		instructed, using an optional argument, to use FORTRAN-style 		arrays, in which the leftmost index changes the fastest.



	2.  reshape
		
		> returns the array with a modified shape
		
		```
		x = a.reshape(6,2)
		print(x)
		
		# such as ...
		# [[ 8.  2.  0.  2.  8.  2.]
		#	[ 4.  4.  8.  0.  0.  0.]]
		
		```
	
	3. T 转置矩阵

		> returns the array, transposed
		
		```
		print(a.T)
		# [[ 2.  4.  5.]
			[ 6.  5.  8.]
			[ 8.  3.  9.]
			[ 0.  1.  1.]]
		
		```

3. 改变原数组

	1. resize 

		> the ndarray.resize method modifies the array itself:
		
		> 如果给定的形状过大,会用0填充,过小,则舍去剩余元素 
		
		```
		a = np.floor(10*np.random.random((3,4)))
		x = a.resize((6,2))

		print("\n - -- - -- - - \n")
		print(x)
		print(a)
		
		```

		```
		# 输出:
		None
		[[ 4.  2.]
		 [ 2.  3.]
		 [ 3.  8.]
		 [ 4.  8.]
		 [ 1.  6.]
		 [ 4.  3.]]
		
		```

4. 堆叠矩阵  Stacking together different arrays

	1. vstack,hstack 

		> 不改变原来的 array

		```
		a = np.floor(10 * np.random.random((2, 2)))
		b = np.floor(10 * np.random.random((2, 2)))
		
		x = np.vstack((a,b))
		print(x.shape)
		print(x)
				
		y = np.hstack((a,b))
		print(y.shape)
		print(y)
		```
		
		```
		# output:
		(4, 2)
		[[ 2.  5.]
		 [ 8.  7.]
		 [ 1.  0.]
		 [ 5.  5.]]
		(2, 4)
		[[ 2.  5.  1.  0.]
		 [ 8.  7.  5.  5.]]
		
		```
		
		```
    	vstack: Stack arrays in sequence vertically (row wise) 
		vstack(tup)
			tup : sequence of ndarrays
		
		hstack: Stack arrays in sequence horizontally (column wise).
		hstack(tup)
			tup : sequence of ndarrays
		```

	2. column_stack

		> The function column_stack stacks 1D arrays as columns into a 			2D array. It is equivalent to hstack only for 2D arrays:
		
		```
		q = np.array([1,2])
		p = np.array([3,4])
		
		qp = np.column_stack((q,p))
		print(qp)
		
		```
		
		```
		output:
		[[1 3]
		 [2 4]]
		```
		
		but use hstack()
		
		```
		print(np.hstack((q,p)))
		```
		
		```
		output:
		[1 2 3 4]
		```
		
		other 第二种和第三种一样
		
		```
		print(q[:,np.newaxis])
		
		test = np.column_stack((q[:,np.newaxis],p[:,np.newaxis]))
		print(test)
		
		print(np.hstack((a[:,newaxis],b[:,newaxis])))
		```
		
		```
		# output:
		
		[[1]
 		 [2]]
 		# 分割线...... 
 		[[1 3]
 		 [2 4]]
		# 分割线......
 		[[1 3]
		 [2 4]]
		```
		
	3. row_stack

		> On the other hand, the function **row_stack** is equivalent 		to **vstack** for any input arrays. In general, for arrays of 		with more than two dimensions, hstack stacks along their second 		axes, vstack stacks along their first axes, and concatenate 		allows for an optional arguments giving the number of the axis 		along which the concatenation should happen.
		
		
	4.  concatenate

		```
		a = np.array([[1, 2], [3, 4]])
		b = np.array([[5, 6]])
		
		x = np.concatenate((a, b), axis=0)
		print(x)
		
		y  = np.concatenate((a, b.T), axis=1)
		print(y)
		```
		
		```
		output:
		[[1 2]
		 [3 4]
		 [5 6]]
		[[1 2 5]
		 [3 4 6]]
		
		```
		
	5. r_ and c_
		
		
		```
		a = np.r_[1:4,0,4]
		print(a)		
		
		b = np.c_[np.array([1,2,3]), np.array([4,5,6])]
		print(b)
		
		c = np.c_[np.array([1,2,3])]
		print(c)

		# 注意这里多了个方括号
		d = np.c_[np.array([[1, 2, 3]]), 0, 0, np.array([[4, 5, 6]])]
		print(d)
		```
		
		```
		output:
		# 分割线 a:
		[1 2 3 0 4]
		# 分割线 b:
		[[1 4]
		 [2 5]
		 [3 6]]
		 # 分割线 c:
		[[1]
		 [2]
		 [3]]
		 # 分割线 d:
		 [[1 2 3 0 0 4 5 6]]
		```
	
5. 分割矩阵 Splitting one array into several smaller ones
	
	1. hsplit(a,?)

		```
		a = np.floor(10*np.random.random((2,12)))
		
		print(a)
		
		b = np.hsplit(a,3)
		print(type(b))
		
		for key in b:
		    print(key)
		    print(" - -- - - -- - - ")
		```
	
		```
		output:
		[[ 7.  9.  6.  3.  2.  1.  3.  5.  3.  1.  3.  7.]
		 [ 9.  6.  6.  3.  1.  5.  6.  5.  6.  6.  6.  1.]]
		<class 'list'>
		[[ 7.  9.  6.  3.]
		 [ 9.  6.  6.  3.]]
		 - -- - - -- - - 
		[[ 2.  1.  3.  5.]
		 [ 1.  5.  6.  5.]]
		 - -- - - -- - - 
		[[ 3.  1.  3.  7.]
		 [ 6.  6.  6.  1.]]
		 - -- - - -- - - 
		
		```
	
	2. hsplit(a,(?,?))
		
		```
		a = np.floor(10 * np.random.random((2, 12)))

		print(a)
		
		c = np.hsplit(a, (3, 4))
		for item in c:
		    print(item)
		    print(" -- - - - -- - ")
		```


		```
		output:
		[[ 1.  9.  9.  6.  0.  9.  0.  0.  7.  3.  4.  9.]
		 [ 9.  7.  7.  4.  5.  5.  2.  6.  4.  0.  6.  8.]]
		[[ 1.  9.  9.]
		 [ 9.  7.  7.]]
		 -- - - - -- - 
		[[ 6.]
		 [ 4.]]
		 -- - - - -- - 
		[[ 0.  9.  0.  0.  7.  3.  4.  9.]
		 [ 5.  5.  2.  6.  4.  0.  6.  8.]]
		
		```
		
	3. vsplit splits along the vertical axis, and array_split allows one to specify along which axis to split.
		
	
	4. split

		```
		x = np.arange(9.0)
		print(x)
		y = np.split(x,3)
		print(type(y))
		for key in y :
		    print(key)
		    print(" -- - - -")

		```
		
		```
		output:
		
		[ 0.  1.  2.  3.  4.  5.  6.  7.  8.]
		<class 'list'>
		[ 0.  1.  2.]
		 -- - - -
		[ 3.  4.  5.]
		 -- - - -
		[ 6.  7.  8.]
		 -- - - -
		```
		
		```
		x = np.arange(8.0)

		print(x)
		y = np.split(x,[3,5,6,10])
		print(type(y))
		
		for key in y:
		    print(key)
		    print("- - - - --  - ")

		```
		
		```
		output:
		[ 0.  1.  2.  3.  4.  5.  6.  7.]
		<class 'list'>
		[ 0.  1.  2.]
		- - - - --  - 
		[ 3.  4.]
		- - - - --  - 
		[ 5.]
		- - - - --  - 
		[ 6.  7.]
		- - - - --  - 
		[]
		- - - - --  - 		
		```


6. 复制和视图 Copies and Views 

	**When operating and manipulating arrays, their data is sometimes copied into a new array and sometimes not. This is often a source of confusion for beginners**
	
	当我们操作数组时,有些操作会返回一个新的数组,有些则是不返回(而是改变他们自身),这常常是初学者混淆的根源之一.

	1. Simple assignments make no copy of array objects or of their data

	```
	a = np.arange(12)

	b = a
	print(b is a)
	
	b.shape = 3,4
	print(b.shape)
	print(a.shape)

	```
	```
	True
	(3, 4)
	(3, 4)
	```

	> Python passes mutable objects as references, so function calls make no copy.
	
	python 将可变对象作为引用传递
	
	
	2. View or Shallow Copy 浅复制

		```
		a = np.arange(12.0)

		# shallow copy
		c =a.view()
		
		print(a)
		print(" - - -- - - ")
		print(c)
		print(' - -- - - -')
		
		print(c is a)
		
		# c is a view of the data owned by a
		print(c.base is a )

		```

		```
		output:
		[  0.   1.   2.   3.   4.   5.   6.   7.   8.   9.  10.  11.]
		 - - -- - - 
		[  0.   1.   2.   3.   4.   5.   6.   7.   8.   9.  10.  11.]
		 - -- - - -
		False
		True
		
		```
		
		> ndarray.flags.owndata : The array owns the memory it uses or borrows it from another object.

		```
		print(c.flags)
		b =a 
		print(b.flags)
		
		```

		```
		outpt:
		
		  C_CONTIGUOUS : True
		  F_CONTIGUOUS : True
		  OWNDATA : False
		  WRITEABLE : True
		  ALIGNED : True
		  UPDATEIFCOPY : False
		  
		  C_CONTIGUOUS : True
		  F_CONTIGUOUS : True
		  OWNDATA : True
		  WRITEABLE : True
		  ALIGNED : True
		  UPDATEIFCOPY : False
		```

		当 c 为 a.view() 时,改变 c 的 shape 并不会影响 a, 但是改变 c 中的元素内容,会影响 a ; 相同地,改变 a 中的元素内容,也会影响 c
		
		```
		a = np.arange(12)
		a.shape = 3,4
		c = a.view()
		
		print(c.shape)
		
		c.shape = 2,6
		
		print(c)
		print(c.shape)
		
		print('- - - -- - ')
		
		print(a)
		print(a.shape)
		
		print('- - - -- - ')
		
		c[0,4] = 123
		print(c)
		print('- - - -- - ')
		
		print(a)
		
		print('- - - -- - ')
		
		```
		
		```
		output:
		(3, 4)
		[[ 0  1  2  3  4  5]
		 [ 6  7  8  9 10 11]]
		(2, 6)
		- - - -- - 
		[[ 0  1  2  3]
		 [ 4  5  6  7]
		 [ 8  9 10 11]]
		(3, 4)
		- - - -- - 
		[[  0   1   2   3 123   5]
		 [  6   7   8   9  10  11]]
		- - - -- - 
		[[  0   1   2   3]
		 [123   5   6   7]
		 [  8   9  10  11]]
		- - - -- - 
		```
		
	3. deep copy

		The copy method makes a complete copy of the array and its data.

		无论 d 怎样改变, a 都不会发生变化
		
				
		```
		a = np.arange(12).reshape(3,4)

		d = a.copy()
		print(d.base is a)
		
		```
		
		```
		output:
		
		False
		```
		
		
## 进阶 Less Basic

### 广播机制 Broadcasting rules

> **Broadcasting allows universal functions to deal in a meaningful way with inputs that do not have exactly the same shape.**

> 广播允许通用函数以有意义的方式处理不完全相同形状的输入。

> 参考: [broadcaset rules](https://docs.scipy.org/doc/numpy-1.13.0/user/basics.broadcasting.html)

1. 让所有输入数组都向其中shape最长的数组看齐，shape中不足的部分都通过在前面加1补齐
2. 输出数组的shape是输入数组shape的各个轴上的最大值
3. 如果输入数组的某个轴和输出数组的对应轴的长度相同或者其长度为1时，这个数组能够用来计算，否则出错
4. 当输入数组的某个轴的长度为1时，沿着此轴运算时都用此轴上的第一组值

示例参考:[点击链接](http://scipy.github.io/old-wiki/pages/EricsBroadcastingDoc)


### 花式索引及其技巧 Fancy indexing and index tricks

#### argmax(axis= ?)

对于二维的数组来说
	
1. axis = 0 求每一列之中最大值的索引 同:axis = -2
2. axis = 1 求每一行之中最大值的索引 同:axis = -1

#### 快速改变一个数组中的值

```
def assign():
    a = np.arange(5)
    a[[1,2]] = 111
    print(a)
```

或者

```
def assign():
    a = np.arange(5)
    a[[1,2]] = [112,113]
    print(a)

```

#### 使用 += , *= 等

```
def add_equal_array():
    a = np.arange(5)
    a[[0,1,2]]+=10
    print(a)

add_equal_array()

# output:
[10 11 12  3  4]

```

```
def mul_equal_array():
    a = np.arange(5)
    a[[1,2]]*=11
    print(a)

mul_equal_array()

# output:
[ 0 11 22  3  4]
```

#### 使用布尔类型的数组 Indexing with Boolean Arrays

例一:

```
def boolean_array():
    a = np.arange(12).reshape(3,4)
    b = a%3==0
    print(b)
	 print(a[b])
  	 a[b] = 9999
    print(a)

boolean_array()

# output:
[[ True False False  True]
 [False False  True False]
 [False  True False False]]
[0 3 6 9]
[[9999    1    2 9999]
 [   4    5 9999    7]
 [   8 9999   10   11]]
```

例二:

```
def boolean_array2():
    a  =np.arange(12).reshape(3,4)
    print(a)

    b1 = np.array([False,True,True])
    b2 = np.array([True,False,True,False])
    x = a[b1]
    print(x)
    print(' -- - - -- - -')
    print(a[b1,:])
    print(' - -- -- - -')
    print(a[:,b2])
    print('-  --- -- - - -- - ')
    print(a[b1,b2])

boolean_array2()

# output:
[[ 0  1  2  3]
 [ 4  5  6  7]
 [ 8  9 10 11]]
[[ 4  5  6  7]
 [ 8  9 10 11]]
 -- - - -- - -
[[ 4  5  6  7]
 [ 8  9 10 11]]
 - -- -- - -
[[ 0  2]
 [ 4  6]
 [ 8 10]]
-  --- -- - - -- - 
[ 4 10]
```

#### The ix_() function

The ix_ function can be used to combine different vectors so as to obtain the result for each n-uplet. 

This function takes N 1-D sequences and returns N outputs with N dimensions each, such that the shape is 1 in all but one dimension and the dimension with the non-unit shape value cycles through all N dimensions.

```
def ix_fuc_test():
    a = np.array([1,2,3])
    b = np.array([4,5,6])
    c = np.array([7,8,9])

    ax,bx,cx = np.ix_(a,b,c)
    print(ax,ax.shape)
    print('  -- -- - --  - -  ')
    print(bx,bx.shape)
    print(' - -- -- - - - - -- ')
    print(cx,cx.shape)
    print('- - - - -- - -')
    result = ax+bx*cx
    print(result)
    print(result[1,2,2],a[1]+b[2]*c[2])


# output:
[[[1]]

 [[2]]

 [[3]]] (3, 1, 1)
  -- -- - --  - -  
[[[4]
  [5]
  [6]]] (1, 3, 1)
 - -- -- - - - - -- 
[[[7 8 9]]] (1, 1, 3)
- - - - -- - -
[[[29 33 37]
  [36 41 46]
  [43 49 55]]

 [[30 34 38]
  [37 42 47]
  [44 50 56]]

 [[31 35 39]
  [38 43 48]
  [45 51 57]]]
56 56
```


### 线性代数 Linear Algebra

#### Simple Array Operations

```
import numpy as np

a = np.array([[1, 2],
              [3, 4]])

# 转置
print(a.transpose())

# 计算矩阵的逆
x = np.linalg.inv(a)
print(x)

# 单位矩阵
# Return a 2-D array with ones on the diagonal and zeros elsewhere.
u = np.eye(4)
print(u)

# 矩阵 点乘
j = np.array([[0.0, -1.0], [1.0, 0.0]])
j1 = np.dot(j, j)
print(j1)

# 返回对角线元素之和
trace1 = np.trace(u)
print(trace1)


# 解线性矩阵方程或线性标量方程组
y = np.array([[5.], [7.]])
result_y = np.linalg.solve(a, y)

print(result_y)

# 正方形数组的特征值和右特征向量的计算
result_eig = np.linalg.eig(j)
print(result_eig)

```


### Tricks and Tips


#### “Automatic” Reshaping

```
a = np.arange(30)

# -1 means "whatever is needed"
a.shape = 2,-1,3
print(a.shape)
print(a)
```

#### Vector Stacking 矢量叠加

```
x = np.arange(0,10,2)                     # x=([0,2,4,6,8])
y = np.arange(5)                          # y=([0,1,2,3,4])
m = np.vstack([x,y])                      # m=([[0,2,4,6,8],
                                          #     [0,1,2,3,4]])
xy = np.hstack([x,y])                    
# xy =([0,2,4,6,8,0,1,2,3,4])

print(m)
print(xy)
```

#### Histograms 直方图

```

def test2():
    mu, sigma = 2, 0.5
    v = np.random.normal(mu, sigma, 10000)
    plt.hist(v, bins=50, normed=1)
    plt.show()

```









		
		

		
		
		
		


