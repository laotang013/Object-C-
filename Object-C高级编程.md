###Object-C 高级编程

>自己生成的对象，自己所持有。

>非自己生成的对象，自己也能持有

>不再需要自己持有的对象时释放

>非自己持有的对象无法释放

因为id类型和对象类型的所有权修饰符默认为__strong 修饰符所以默认不需要写上__strong 使ARC有效及简单的编程遵循了Object-C内存管理的思考方式。

**循环引用:** 循环引用容易出现内存泄漏，所谓内存泄漏就是应当废弃的对象在超出其生存周期后继续存在。
**__weak** 提供弱引用 弱引用不能持有对象实例，若该对象被废弃，则此弱引用将自动失效且处于nil被赋值的状态(空弱引用)通过检查附有__weak修饰符的变量是否为nil，可以判断赋值的对象是否已废弃。

**__autoreleasing** 用@autoreleasepool块来替代NSAutoreleasePool类对象生成、持有以及废弃这一范围

无效时
NSAutoreleasePool *pool =  [[NSAutoreleasePool alloc]init];
[obj autorelease];
[pool drain];
有效时
@autoreleasepool
{
id __autoreleasing obj2;
obj2 = obj;
}
问题:

为什么在访问附有__weak修饰符的变量时，必须访问注册到autoreleasepool的对象呢。
因为__weak修饰符只持有对象的弱引用，而在访问引用对象的过程中，该对象可能被废弃。如果把要访问的对象注册到autoreleasepool中，那么在@autoreleasepool块结束之前都能确保该对象存在。
因此在附有__weak修饰符的变量时，就必须要使用注册到autoreleasepool中的对象。
对象指针型赋值时，其所有权修饰符必须一致，

只有作为alloc/new/copy/mutableCopy方法的返回值而取得的对象时，能够自己生成并持有对象，其他情况即为 取得非自己生成并持有的对象

CoreFoundataion对象与Object-C对象的区别很小，不同之处只在于是由哪一个框架所生成。
Fountdataion框架的API生成并持有的对象可以用CoreFounation框架的API释放。当然反过来也可以。

**属性**

copy 属性不是简单的赋值，它赋值的是通过NSCopying接口的copyWithZone:方法复制赋值源所生成的对象。
另外在声明类变量时，如果同属性声明中的属性不一致则会引起编译错误。

**数组**

在静态数组中，编译器能股根据变量的作用域自动插入释放赋值对象的代码，而在动态数组中，编译器补鞥呢确定数组的生存周期，所以无从处理。所以一定要将nil赋值给所有的元素中，使得元素赋值对象的强引用失效，从而释放那些对象，在此之后使用free 函数废弃内存块。


