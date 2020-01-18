HashMap：了解其数据结构、hash冲突如何解决（链表和红黑树）、扩容时机、扩容时避免rehash的优化
数据结构是由数组+链表的形式组成的，又称链表散列。可伸缩：1，数组扩容，变长。2，单线链表如果长度超过8就会变成红黑树。
数组默认大小为16。
哈希冲突：不同的对象得到的hash进行16取余，得到一个16位的余数，如果相同就会造成hash冲突。
hash冲突就会产生单线链表，如果单线链表达到一定长度就会变成红黑树。
数组扩容：如果数组存储比例达到了75%时触发数组扩容变成两倍。
避免rehash的优化：因为扩容是由2的幂次方的形式，所以在扩容的时候原来位置上的数据会在当前n和n+oldGap的位置，也就是在当前位置或者
在当前位置加上旧的容器长度的位置。


LinkedHashMap：了解基本原理、哪两种有序、如何用它实现LRU

TreeMap：了解数据结构、了解其key对象为什么必须要实现Compare接口、如何用它实现一致性哈希


hashmap如何解决hash冲突，为什么hashmap中的链表需要转成红黑树？
hashmap什么时候会触发扩容？
jdk1.8之前并发操作hashmap时为什么会有死循环的问题？
hashmap扩容时每个entry需要再计算一次hash吗？
hashmap的数组长度为什么要保证是2的幂？
如何用LinkedHashMap实现LRU？
如何用TreeMap实现一致性hash