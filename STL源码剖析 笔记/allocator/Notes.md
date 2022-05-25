# 内容

STL 是库，研究一个库要从两个角度来看：其一，功能以及对应的接口是什么；其二，其底层是如何实现的。同时，STL 是标准库，还要看标准规定了什么，以及具体的库版本与之有何细微区别。

## 接口

+ 标准规定了如下必要接口：
```cpp
	//allocator作为类本身需要的构造、析构函数
	allocator::allocator()	
	allocator::allocator(const allocator&)
	template<class C>allocator::allocator(const allocator<c>&)
	allocator::~allocator();
	
	//从引用提取地址
	pointer const_pointer::address(reference x)const
	const_pointer allocator::address(const_reference x)const
	
	//allocate 与 deallocate
	pointer allocator::allocate(size_type n,const void*=0)
	void allocator::deallocate(pointer p,size_type n)
	size_type allocator::max_size()const
	
	//构造与析构
	void allocator::construct(pointer p,const T&x)
	void allocator::destroy(pointer p)
```

+ 从接口可见，标准规定，allocator 要负责空间的 allocate/ deallocate，另外还要负责对象的构造与析构
	+ 前者只管空间的配置和收回，后者负责在已配置的空间上构建/销毁对象

## 实现

1. 标准规定的 allocator 名字叫 allocator，SGI STL 虽然对其进行了实现，但这实现的十分简陋，仅仅是对 operator new 和 operator delete 的调用
	+奇怪的是，既然这一版本是按标准来的，那为什么没有 construct 和 destroy 两个函数？

2. SGI STL 自己设计了一个另一个 allocator，名字叫 alloc，其有特点在于：
	+ alloc 自己设计 allocate/deallocate 过程，至于 construct/destroy 过程，由标准规定的全局函数 construct() 和 destroy() 来执行
	+ 对于 allocate/deallocate，对不同大小的内存用不同的策略：
		+ 对于大于 128B 的内存请求，直接调用 malloc
		+ 对于较小的内存请求，用内存池并维护 free list 来响应
			+ free list 中用链表结构维护了一系列不同尺寸的内存块，方便拿来即用，用完也直接还给 free list
			+ free list 中的内存块来自于内存池，内存池的内存来源于 malloc
			+ free list 管理内存是有粒度的，这一粒度对于节省内存、free list 的维护带来了十分巧妙的便利

# Q&As

1. **原书分析的源码是在 Windows 下，书中提到有 *stl\_config.h* 文件，内容是针对不同编译器及可能的版本予以常量设定，在 Ubuntu 的 */usr/include/c++* 下的源码中却没有找到该文件，这是何故？**
	+ 已经确认了 Ubuntu 所用的也是 SGI STL，因为其中各个文件开头的版权声明中都有 Silicon Graphics Computer Systems 公司的声明

2. **P45 代码中，\_construct 函数调用的貌似是拷贝构造函数，难道不能调用其他类型的构造函数吗？**

3. **SGI STL 为什么要设两级配置器，即维护 *free\_list* 满足小空间需求，而对大块请求直接交付给 malloc，这样做有什么好处？**

4. **set\_new\_handlder() 是什么？**

5. **什么是 rebind 以及什么是仿函数？**

# Notes

+ allocator 被翻译成空间配置器而不是内存配置器，是因为空间不一定只是内存，还可以是磁盘或者其他辅助存储设备

+ free list 的巧妙之处：
	+ 用 enum 方式来链接内存块，不用为了维护链表花费额外空间
	+ free list 是链表数组结构
		+ 数组结构方便调取和收回
		+ 将内存块按一定粒度（书中是8），并且安排递增的尺寸类型，使得只要有内存，就一定可以 refill 一种尺寸的free list，不会浪费
	+ free list 的设计十分巧妙，具体翻阅书籍体会
