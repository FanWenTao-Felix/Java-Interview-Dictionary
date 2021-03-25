## 1.List：可重复

List是非常常用的数据类型，是有序的Collection，一共有三个实现类，分别是ArrayList、Vector和LinkedList。

**1.ArrayList：基于数组实现，增删慢，查询快，线程不安全**

ArrayList是使用最广泛的List实现类，其内部数据结构基于数组实现，提供了对List的增加（add）、删除（remove）和访问（get）功

能。

ArrayList的缺点是对元素必须连续存储，当需要在ArrayList的中间位置插入或者删除元素时，需要将待插入或者删除的节点后的所有元素

进行移动，其修改代价较高，因此，ArrayList不适合随机插入和删除的操作，更适合随机查找和遍历的操作。

ArrayList不需要在定义时指定数组的长度，在数组长度不能满足存储要求时，ArrayList会创建一个新的更大的数组并将数组中已有的数据

复制到新的数组中。

**2.Vector：基于数组实现，增删慢，查询快，线程安全**

Vector的数据结构和ArrayList一样，都是基于数组实现的，不同的是Vector支持线程同步，即同一时刻只允许一个线程对Vector进行写操

作（新增、删除、修改），以保证多线程环境下数据的一致性，但需要频繁地对Vector实例进行加锁和释放锁操作，因此，Vector的读写

效率在整体上比ArrayList低。

**3.LinkedList：基于双向链表实现，增删快，查询慢，线程不安全**

LinkedList采用双向链表结构存储元素，在对LinkedList进行插入和删除操作时，只需在对应的节点上插入或删除元素，并将上一个节点

元素的下一个节点的指针指向该节点即可，数据改动较小，因此随机插入和删除效率很高。但在对LinkedList进行随机访问时，需要从链

表头部一直遍历到该节点为止，因此随机访问速度很慢。除此之外，LinkedList还提供了在List接口中未定义的方法，用于操作链表头部

和尾部的元素，因此有时可以被当作堆栈、队列或双向队列使用。

## 2.ArrayList和LinkedList和Vector的区别

都实现了List 接口。

ArrayList 是一个可改变大小的数组.当更多的元素加入到ArrayList中时,其大小将会动态地增长.内部的元素可以直接通过get与set方法进行访问,因为ArrayList本质上就是一个数组.

LinkedList 是一个双链表,在添加和删除元素时具有比ArrayList更好的性能.但在get与set方面弱于ArrayList.

Vector和ArrayList在更多元素添加进来时会请求更大的空间。Vector每次请求其大小的双倍空间，而ArrayList每次对size增长50%.

而 LinkedList 还实现了 Queue 接口,该接口比List提供了更多的方法,包括 offer(),peek(),poll()等.



