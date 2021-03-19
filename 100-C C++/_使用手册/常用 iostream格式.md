[link: 官网|IO](http://www.cplusplus.com/reference/iomanip/setw/)

- std::cin
- std::cout
- std::endl => flushes the buffer

```c++
using namespace std;

//也可以针对使用
using std::cout; 
using std::endl;
```



## std::setw()

```c++
// setw example
#include <iostream>     // std::cout, std::endl
#include <iomanip>      // std::setw

int main () {
  std::cout << std::setw(10);	//包括后面的数据在内，空出10个字符
  std::cout << 77 << std::endl;
  return 0;
}
```

