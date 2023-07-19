# c++语法

#### struct重写小于操作符
操作符const，出现在参数位置时，表明参数值不能被修改，与引用传参&（无需copy参数）共同使用
在方法参数括号后的const，表明操作"<" 不会修改当前变量，如果需要将struct放入优先队列进行比较必须将方法定义成const
```
struct Ratio {
    int pass, total;
    bool operator < (const Ratio& oth) const {
        return (pass + 1.0) / (total + 1.0) - ((double)pass / total) < (oth.pass + 1.0) / (oth.total + 1.0) - ((double)oth.pass / oth.total);
    }
};
```

#### lambda表达式
[capture list] (parameter list) throw() -> return type { function body }
- capture list: lambda所在函数定义的局部变量的列表（通常为空）
- parameter list/return type/fucntion 与正常函数相同
- 常常简写中不写 -> return type，返回类型可以表达式推断出来: [](parameter list) {function body}
```
#include <algorithm>
#include <cmath>

void abssort(float* x, unsigned n) {
    std::sort(x, x + n,
        // Lambda expression begins
        [](float a, float b) {
            return (std::abs(a) < std::abs(b));
        } // end of lambda expression
    );
}
```

#### priority_queue的自定义函数
- 使用lambda表达式先定义函数com
- 在priority_queue声明时，添加第二个参数，**vector<第一个参数>**
- 添加第三个参数，decltype(com)
- 初始化参数时增加函数名称
```
auto com = [](pair<int, int> &a, pair<int, int> &b) {
    return a.second > b.second;
};
priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(com)> pq(com);
```

struct cmp {
    bool operator()(const A &a1, const A &a2) {
        if(a1.xxx < a2.xxx) {
            return false;
        }
    } 
}

priority_queue<A, vector<A>, cmp> pq;

#### sort函数的反序
```
vector<int> a = { 1, 45, 54, 71, 76, 12 };
sort(vec.begin(), vec.end(), greater<int>());
```


#### pair的排序
pair默认排序为，先比较first，后比较second
  
#### reverse函数
reverse(myvector.begin(), myvector.end())
// reverse 0-k-1: The objects in the range [first,last) are modified.
reverse(myvector.begin(), myvector.begin()+k)

### todo
常用函数的头文件