# 内容

+ 所谓 sequence containers 中的 sequence， 是指容器中的元素以线性排列存储，其特点是虽然未必有序但是可序
	+ "线性排列" 与 "可序"的要求是等价的，因为所谓的排序，就是让元素在线性空间上有序
		+ 更进一步，"排序"是让一种顺序与另一种顺序相一致

+ 对于 sequence container，要从以下几个方面去考察：
	+ 底层是怎样实现的？
	+ 支持什么样的迭代器？
	+ 支持哪些操作、那些操作的复杂度是怎样的？

## vector

+ 
## deque

+ deque 和 vector 本质上没有区别，其实 deque 就是 vector 管理内存的粒度更大了一些，但 deque 由于引入了分段连续，其对外表现相比于 vector 有几个好处：
	+ 分段连续使得 deque 可以避免迭代器失效的情况
	+ 同样也是由于分段连续，deque 可以在两端插入，而 vector 只能单端插入

## list

+ list 是双向链表

## stack & queue

## heap & priority\_queue

+ heap 不是容器，是一族基于堆的算法
+ priority\_queue 是基于 sequence containers、利用 heap 算法实现的一种 adaptor
	+ 之所以称其为 priority\_queue，priority 来自于 heap 有序特点，queue 来自于从 heap 中取元素时只能挨个从顶端弹出，像个queue

## slist

## Q&As

+ **vector 需要扩容时，要把原有的元素平移到另一片空间中去，如果元素是 POD 类型正常移动即可，然而当元素对象中包含对外部资源的引用时，如果还是调用拷贝构造函数来进行移动，那么外部资源就有可能被重复构造，造成效率低下，实际上这时只需要调用 memmove 就可以了，STL 是如何处理这种情景的？**
	+ vector 移动时调用了 uninitialized\_copy，而 uninitialized\_copy 又调用了 copy，copy 中对被拷贝的对象类型做了详尽的考量并针对效率进行了最佳方案选择

