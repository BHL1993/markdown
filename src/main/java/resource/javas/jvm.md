Q：GC Roots包含哪些
A：
    1、Java栈 和 Native栈中所引用的对象
    2、方法区中的常量和静态变量
    3、所有运行中的线程对象
    4、所有跨代引用对象
    5、和已知GC Roots对象同属一个CardTable的其他对象

