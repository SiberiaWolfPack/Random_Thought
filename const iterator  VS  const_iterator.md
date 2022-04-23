### const iterator  VS  const_iterator
```c++
using iterator = string*; // 普通指针
using const_iterator = const string*; // 常量指针：指针可动，值不能动

const iterator it_c = vec.begin();
*it_c="hi"; // 这句是可以的
it_c++; // 这句不行

const_iterator c_it = vec.cbegin();
*c_it = "hi"； // 这句不行
c_it++; // 这句可以

const const_iterator c_it_c = vec.cbegin(); // 两句都不行
```

这是一个神奇的现象，一旦用了别名，原先的语法就倒了过来：
```c++
常量指针：const int * p = &a;
*p = 9; // 失败
p++; // 成功

指针常量：int* const = &a;
*p = 9; // 成功
p++; // 失败
```