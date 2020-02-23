---
title: Redis
date: 2019-10-29 11:02:44
tags: Redis
---
# Redis 优势

性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。
# Redis 的 数据类型
Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。
String：Redis的基本数据类型，Redis字符是二进制安全的，这意味着Redis字符串可以包含任何类型的数据，例如图片和JSON格式的字符串
List(列表类型)：在List的头端或者尾端做百万次的插入和删除操作，也能保持稳定的很少的时间消耗。在List的两端访问元素是非常快的，但是如果要访问一个很大的List中的中间部分的元素就会比较慢了，时间复杂度是O(N)。
Set(集合类型)：Set的一个重要特性是不允许重复元素。向集合中添加多次相同的元素，集合中只存在一个该元素。在实际应用中，这意味着在添加一个元素前不需要先检查元素是否存在。
Hash(哈希类型)：Hash保存String域和String值之间的映射，所以它们是用来表示对象的数据类型。其存储方式占用很小的空间，所以在一个小的Redis实例中就可以存储上百万的这种对象。
Sorted-Set(有序集合类型)：Redis有序集合类型与Redis的集合类型类似，是非重复的String元素的集合。不同之处在于，有序集合中的每个成员都关联一个Score，Score是在排序时候使用的，按照Score的值从小到大进行排序。集合中每个元素是唯一的，但Score有可能重复。
