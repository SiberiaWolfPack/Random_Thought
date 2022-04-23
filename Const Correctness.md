### Const Correctness
1. 一些为什么使用const传参的情景
	* 检查错误
	```c++
	void f(const int x, const int y)
	{
        if ((x==2 && y==3)||(x==1)) cout << 'a' << endl;
        if ((y==x-1)&&(x==-1||y=-1)) cout << 'b' << endl;
        if ((x==3)&&(y==2*x)) cout << 'c' << endl;
	}
	```
	注意那个赋值语句，如果没有const限制，这个错误很难发现。
	
2. 防止函数中出现恶意修改（&传参的时候）

3. const限定的对象只能访问const函数，不能访问非const函数，这也是为了安全

   ```c++
   #include <iostream>
   #include <cstring>
   #include <algorithm>
   #include <string>
   using namespace std;
   class Student
   {
       private:
       string name;
       
       public:
       string getname()
       {
           return name;
       }
   }
   string get(const Student& s)
   {
       return s.getname() + "，这是我的名字"; // 编译出错
   }
   int main()
   {
       Student stu{"jyf"};
       cout<< get(stu);
       
       return  0;
   }
   ```

   

   