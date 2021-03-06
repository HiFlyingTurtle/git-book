**Queue的特点和用法**

Java 的集合中有queue这个容器，是对数据结构队列的实现，其中有很多方法，需要注意下每种方法的区别

插入元素：offer\(\),add\(\)

```
 boolean add(E e)   
 boolean offer(E e);
```

两个方法都是向队列中插入一个元素，插入成功的时候，都是返回true，差异在于队列空间满无法进行插入的时候，add无法插入的时候直接抛出异常 **IllegalStateException，**而offer无法插入的时候则是直接返回false;

取队列元素或者删除队列中的 元素的方法有如下几种

```
E remove();//移除队首的元素，并且返回元素，如果队列为空则抛出异常;
E poll();//出队操作，取队首元素，并且删除元素并返回，如果为空则返回null
E element();//取队首元素,但是并不移除，如果为空抛出异常
E peek();// 取队首元素，并不删除，如果为空，返回null
```

Stack 集合的主要操作是push\(入栈\)，pop\(出栈\)，peek\(取栈顶元素\)

Stack是继承了Vector ，由于Vector是**线程安全的**，Stack的所有操作都是线程安全的，其主要操作的方法都是 synchronized

Queue的实现是继承了Collection,是非线程安全的 ，实现类是 LinkedList



**PriorityQueue的使用**



TreeSet

