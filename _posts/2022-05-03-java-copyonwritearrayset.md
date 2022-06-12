---
title: Java学习笔记（三）CopyOnWriteArraySet
tags: Java
---

CopyOnWriteArraySet相对CopyOnWriteArrayList用来存储不重复的对象，是线程安全的。虽然继承了AbstractSet类，但CopyOnWriteArraySet与HashMap 完全不同，内部是用CopyOnWriteArrayList实现的，实现不重复的特性也是直接调用CopyOnWriteArrayList的方法实现的，感觉加的最有用的函数就是eq函数判断对象是否相同。

# 成员变量

```java
    private final CopyOnWriteArrayList<E> al;
```

# 构造函数

```java
    public CopyOnWriteArraySet() {
        al = new CopyOnWriteArrayList<E>();
    }

    public CopyOnWriteArraySet(Collection<? extends E> c) {
        al = new CopyOnWriteArrayList<E>();
        al.addAllAbsent(c);
    }
```

构建CopyOnWriteArrayList对象不再多说，addAllAbsent源码如下

```java
    public int addAllAbsent(Collection<? extends E> c) {
        Object[] cs = c.toArray();
        if (cs.length == 0)
            return 0;
          //缓存要加入队列的元素
        Object[] uniq = new Object[cs.length];
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
          //获取原数组
            Object[] elements = getArray();
            int len = elements.length;
            int added =  0;
            //直接遍历数组
            for (int i = 0; i < cs.length; ++i) { // scan for duplicates
                Object e = cs[i];
                //若原队列中不含e  则将其加入uniq中
                if (indexOf(e, elements, 0, len) < 0 &&
                    indexOf(e, uniq, 0, added) < 0)
                    uniq[added++] = e;
            }
            //若存在原队列中不含有的元素
            if (added > 0) {
             //生成新数组，容量扩大added  将旧数据拷贝进去
                Object[] newElements = Arrays.copyOf(elements, len + added);
                //将不重复的数据拷贝进新数组里面
                System.arraycopy(uniq, 0, newElements, len, added);
                //替换数据为新数组
                setArray(newElements);
            }
            return added;
        } finally {
            lock.unlock();
        }
    }
```

# add 添加元素

```java
    public boolean add(E e) {
        return al.addIfAbsent(e);
    }

    public boolean addIfAbsent(E e) {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            Object[] elements = getArray();
            int len = elements.length;
            //创建新数组
            Object[] newElements = new Object[len + 1];
            //遍历数组  不存在元素e 则将其加入新数组里面
            for (int i = 0; i < len; ++i) {
                if (eq(e, elements[i]))
                    return false; // exit, throwing away copy
                else
                    newElements[i] = elements[i];
            }
            newElements[len] = e;
            setArray(newElements);
            return true;
        } finally {
            lock.unlock();
        }
    }
```
