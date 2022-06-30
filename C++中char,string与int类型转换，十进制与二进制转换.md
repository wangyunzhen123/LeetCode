# C++中char,string与int类型转换，十进制与二进制转换

下面是几种常用的方法更详细的见[C++中char,string与int类型转换 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/107453404)





**1.char与string**

char是基础数据类型，string是封装了一些操作的标准类



char *或者char [ ]转换为 string时，可以直接赋值。

```cpp
    string x;
    string y;
    char *ptr1 = "sakura";
    char ptr2[]= "waseda";
    x = ptr1;
    y = ptr2;
```



string转换为char*或者char[ ]时

使用string内置c_str()函数。注意不直接赋值，因为string类对象最后会析构导致左值成为空指针。附加结束符\0

```cpp
    string x = "waseda";
    char *ptr;
    strcpy(ptr,x.c_str());
```



**2.char与int**

char数字 转int ，直接减'0'就好。char数组则使用atoi



int 转char数字，直接加'0。char数组可以使用atoi





**3.string与int**

 int 转string,有std内置to_string函数

to_string

```cpp
    string str ;
    int num=233;
    str = to_string(num);
```





string转int，std内置stoi

```c++
	string str = “111”;
    //当字符串仅有0，1字符组成时，字符串可以表示二进制（即字符串为二进制模式），但int是十进制结果用十进制打印出来
    cout << stoi(str, nullptr, 2) << endl;
```

stoi的完整参数形式int  stoi( const [std::string](http://en.cppreference.com/w/cpp/string/basic_string)& str, [std::size_t](http://en.cppreference.com/w/cpp/types/size_t)* pos = nullptr, int base = 10 );

| str  | -    | the string to convert（要转化的字符串）                      |
| ---- | ---- | ------------------------------------------------------------ |
| pos  | -    | address of an integer to store the number of characters processed |
| base | -    | the number base（字符串菠萝表示的进制，默认是十进制）        |

除此之外根据所需要类型还有stod，stold，stof，stoul，stol，stoll等



**4.char与char**

ascell码的转化，如



# c++ bitset类用法

详细见https://blog.csdn.net/qll125596718/article/details/6901935?，这里主要讲解二进制与十进制之间的转换





 有些程序要处理二进制位的有序集，每个位可能包含的是0（关）或1（开）的值。位是用来保存一组项或条件的yes/no信息（有时也称标志）的简洁方法。标准库提供了bitset类使得处理位集合更容易一些。要使用bitset类就必须要包含相关的头文件。在本书提供的例子中，假设都使用了std::bitset的using声明：

```cpp
#include <bitset>
using std::bitset;
```





## bitset定义和初始化

​     以下列出了bitset的构造函数：



```cpp
bitset<n> b;	          //b有n位，每位都为0
bitset<n> b(u);	          //b是unsigned long型u的一个副本
bitset<n> b(s);	          //b是string对象s中含有的位串的副本
bitset<n> b(s, pos, n);	  //b是s中从位置pos开始的n个位的副本
```

​     类似于vector，bitset类是一种类模板；而与vector不一样的是bitset类型对象的区别仅在其长度而不在其类型。在定义bitset时，要明确bitset含有多少位，须在尖括号内给出它的长度值：

```cpp
bitset<32> bitvec; //32位，全为0。
```

​     给出的长度值必须是常量表达式。正如这里给出的，长度值必须定义为整型字面值常量或是已用常量值初始化的整数类型的const对象。
​    这条语句把bitvec定义为含有32个位的bitset对象。和vector的元素一样，bitset中的位是没有命名的，程序员只能按位置来访问它们。位集合的位置编号从0开始，因此，bitvec的位序是从0到31。以0位开始的位串是低阶位（low-order bit），以31位结束的位串是高阶位(high-order bit)。





十进制转化为二进制

```c++
unsign long a = 10
bitset<8> b(10);	       //b是unsigned long型u的一个副本
cout << b << endl;        //b就是一个八位的二进制数  
```



二进制转化为十进制

```c++
bitset<8> b(8)
unsigned long val = bit.to_ulong();
cout<<val;

```



二进制模式字符串转化为二进制

```c++
string str = "01011"
bitset<8> bit(str);
bitset<8> b(str, 1, 4);
```



二进制转化为二进制模式的字符串

```c++
bitset<6> s("001")
string s = bit.to_string();
```







#### [1980. 找出不同的二进制字符串](https://leetcode-cn.com/problems/find-unique-binary-string/)



我们可以将长度为 nn 的二进制字符串看作 [0, 2^n - 1][0,2 n −1] 闭区间内正整数的二进制表示，这样就建立起了字符串和整数之间的一一映射。

我们可以将 nums 中所有字符串转化为对应的整数放在哈希集合中。由于该哈希集合中有 n 个元素，因此根据鸽巢原理，在 [0, n][0,n] 闭区间的 n+1 个整数中一定存在一个整数，它不在该哈希集合中。换言之，该整数对应的字符串一定没有在 nums 中出现。

因此，在预处理哈希集合后，我们只需要遍历 [0, n][0,n] 闭区间中的整数，当找到第一个不在哈希集合中的整数时，我们将它转化为对应的二进制字符串返回即可。



```c++
string findDifferentBinaryString(vector<string>& nums) {
	unordered_set<int> set;
	//将二进制模式的字符串映射到十进制
	for (auto str : nums) {
		set.insert(stoi(str, nullptr, 2));
	}
	int n = pow(2, nums.size());
	int val = 0;
	for (int i = 0; i < n; ++i) {
		if (set.find(i) == set.end()) {
			val = i;
			break;
		}
	}
	string res = bitset<16>(val).to_string().substr(16 - nums.size(), nums.size());
	return res;
}
```

