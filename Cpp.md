## 头文件:

```c++
#include <initializer_list>
#include <iostream>
#include <istream>
#include <ostream>
#include <stdexcept>
#include <string>
#include <system_error>
#include <vector>    // 使用vector容器
#include <string>
#include <cstdlib>
#include <cctype>    // 使用nullptr
#include <cstddef>   // 使用size_t,ptrdiff_t
#include <iterator>  // 使用begin(),end()
#include <stdexcept> // 异常类，如runtime_error,p176
```

## 全局区

```c++
#include <cassert>  // assert,预处理宏
using std::cin;     // using声明，当我们使用cin时，意味着使用的std::cin
using namespace std;// 头文件内容会拷贝到所有引用他的文件里，一般不应包含using声明（易造成名字冲突）

// 如果定义了预处理变量MIN,就继续到#endif，#indef和#indef叫预处理百年
#ifdef MIN
#define MKSTR(x) #x       // "x"
#endif

// 如果没有定义预处理变量MIN,就继续到#endif，一般头文件的预处理变量定义都需要这样写
#ifndef MIN   
#define MIN(a, b) (a < b ? a : b)
#define concat(a, B) a##b // xy
#define NDEBUG // 定了他 assert什么都不做，没定义则指执行运行时检查
#endif

extern const int i = 123; // a文件里定义并初始化i，加extern使其能被其他文件使用
```

### 数据结构

1. stack 容器
       	stack 容器适配器的模板有两个参数。第一个参数是存储对象的类型，第二个参数是底层容器的类型。stack<T> 的底层容器默认是 deque<T> 容器，因此模板类型其实是 stack<typename T, typename Container=deque<T>>。通过指定第二个模板类型参数，可以使用任意类型的底层容器，只要它们支持 back()、push_back()、pop_back()、empty()、size() 这些操作。下面展示了如何定义一个使用 list<T> 的堆栈：

    ```c++
    using namespace std
    stack<string> words1;   // 省略第一个参数，用默认的底层容器deque<T>实现
    stack<string, list<string>> words2  // 底层容器使用list<T>
    ```
	​		创建堆栈时，不能用对象来初始化，但是可以用另一个容器来初始化，只要堆栈的底层容器类型和这个容器的类型相同，且必须使用圆括号。例如：
  
    ```c++
    using namespace std
    list<double> values {1.414, 3.14159265, 2.71828};
    stack<double, list<double>> my_stack (values);
    ```
  
    ​		第二条语句生成了一个包含 value 元素副本的 my_stack。这里不能在 stack 构造函数中使用初始化列表，必须使用圆括号。如果没有在第二个 stack 模板类型参数中将底层容器指定为 list，那么底层容器可能是 deque，这样就不能用 list 的内容来初始化 stack；只能接受 deque。
  
    ​		stack<T> 模板定义了拷贝构造函数，因而可以复制现有的 stack 容器：
  
    ```c++
    using namespace std
    stack<double, list<double>> copy_stack {my_stack}
    ```
  
    堆栈操作：(T为栈中元素类型)
  
    - push(const T& obj)  可以将对象副本压入栈顶。这是通过调用底层容器的 push_back() 函数完成的。
    - pop()                        弹出栈顶元素但没有返回它。
    - top()                         返回一个栈顶元素的引用但没有弹出，类型为 T&。如果栈为空，返回值未定义。
    - size()                       返回栈中元素的个数。
    - empty()                    在栈中没有元素的情况下返回 true。
    - emplace()                用传入的参数调用构造函数，在栈顶生成对象。
    - swap(stack<T> & other_stack)将当前栈中的元素和参数中的元素交换。参数所包含元素的类型必须和当前栈的相同。对于 stack 对象有一个特例化的全局函数 swap() 可以使用。

 2. vector 容器

    vector定义在<vector>头文件中，需要包含，并位于std命名空间中。

    ```C++
    #include<vector>
    using namespace std;
    vector<double> values;   // 创建空容器
    vector<double> values1(20， 1.0); // 开始就有20个int,初始值均为1.0,没有第二个参数初始值默认均为0
    vector<int> values2{1,2,3,4,2,1}; // 指定元素个数和初始值
    vector<int> values3(values2);  // 创建和alces相同的容器
    vector<int> values4(begin(value2,begin(v。alue2)+3)) // 使用指针或者迭代器来指定初始值范围
    ```
    
    vector容器包含的成员函数：
    
    ```c++
    begin()	    // 返回指向容器中第一个元素的迭代器。
    end()	    // 返回指向容器最后一个元素所在位置后一个位置的迭代器。
    front()	    // 返回第一个元素的引用。
    back()	    // 返回最后一个元素的引用。
    data()	    // 返回指向容器中第一个元素的指针。
    assign()    // 用新元素替换原有内容。
    push_back()	// 在序列的尾部添加一个元素。
    pop_back()	// 移出序列尾部的元素。
    insert()	// 在指定的位置插入一个或多个元素。
    erase()	    // 移出一个元素或一段元素。
    clear()	    // 移出所有的元素，容器大小变为 0。
    rbegin()  // 返回指向最后一个元素的迭代器。
    rend()	  // 返回指向第一个元素所在位置前一个位置的迭代器。
    cbegin()  // 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。
    cend()	  // 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。
    crbegin() // 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。
    crend()	  // 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。
    size()	  // 返回实际元素个数。
    max_size()// 返回元素个数的最大值。这通常是一个很大的值，一般是 2^32-1，所以我们很少会用到这个函数。
    resize()  // 改变实际元素的个数。
    capacity()// 返回当前容量。
    empty()	  // 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。
    reserve() // 增加容器的容量。
    shrink _to_fit()  // 将内存减少到等于当前元素实际所使用的大小。
    operator[ ]	      // 重载了 [ ] 运算符，可以向访问数组中元素那样，通过下标即可访问甚至修改 vector 容器中的元素。
    at()	    // 使用经过边界检查的索引访问元素。
    swap()	    // 交换两个容器的所有元素。
    emplace()	// 在指定的位置直接生成一个元素。
    emplace_back()	// 在序列尾部生成一个元素。
    ```
    
    
