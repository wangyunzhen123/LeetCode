# c++中int，double和flaot

[C++ double和float（浮点类型）详解 (biancheng.net)](http://c.biancheng.net/view/1321.html)



若int和double比较，int类型会转化为double类型

# C中各类数值型数据间的混合运算

在进行运算时，不同类型的数据要先转换成同一类型，然后进行运算

[(60条消息) C中各类数值型数据间的混合运算_蜗牛Running-CSDN博客_c语言各类数值型数据间的混合运算](https://blog.csdn.net/mapeng892020/article/details/40785995)

![img](https://img-blog.csdn.net/20141104155425468?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbWFwZW5nODkyMDIw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)





# C中double到int的转换、四舍五入

https://blog.csdn.net/lin200753/article/details/27952897?





  double类型给float\int等类型赋值时可能发生精度损失问题。

##### 四舍五入的情况

######   float\double按格式，限制小数点个数时会发生四舍五入。

######   float\double强转化为int类型时，只取整数部分不会四舍五入。

```c++
#include "stdio.h"
void main()
{
    float a,b;
    int d,d1;
    a=3.56;// warning const double to float
    b=(float)3.34;//这样就不警告了。
    
    printf("a=%f,b=%f\n",a,b);//默认情况下，小数点6位
    d=(int)a;//只取整数部分而不会四舍五入。
    d1=(int)(b);//只取整数部分而不会四舍五入
    printf("d=%d,d1=%d\n",d,d1);  
    printf("m.n的格式会四舍五入：%.1f,%.1f",a,b);    
}	
```





###  ceil \floor

```
	在一些算法或运算中可能要用到四舍五入、向上取整┌X┐、向下取整等操作.└X┘
	其中ceil()函数是天花板的意思，即向上取整。floor为地板，即向下取整。

```

```c++
#include <math.h>
double floor( double arg );
功能： 函数返回参数不大于arg的最大整数。
___________________________________
 
double ceil(double x);  
功 能: 返回大于或者等于指定表达式的最小整数
```





### 若double变量大于int所存储的范围时，强制转换的结果是不确定的











#### [面试题 05.02. 二进制数转字符串](https://leetcode-cn.com/problems/bianry-number-to-string-lcci/)

```c++
//模拟

string printBin(double num) {
    string str = "0.";
    double r = num * 2;
    while (r != 0) {
        //取整数部分
        int res = r;
        if (res == 1) {
            str.push_back('1');
            //小数部分
            r = r - 1;
        }
        if (res == 0) {
            str.push_back('0');
        }
        r = r * 2;
    }
    return str.size() <= 32 ? str : "ERROR";
}
```

