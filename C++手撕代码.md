### 多线程
- 头文件
  - #include \<thread\>  // c++11
- 声明多个thread
  - thread t[n];
- thread定义
  - t[i] = thread(func, func_arg0, func_arg1, ...);
- 阻塞主线程直到子线程结束
  - t[i].join()
- 注意事项： 
  - 传入的函数必须无返回值，不像pthread
  - thread函数传递参数时，会copy一个参数副本，实际上是值传参，需要引用的变量需要使用**std::ref**操作包裹

### 文件读写
- 头文件 
  - #include \<fstream\>   // 同时包含ifstream,ofstream
- 声明+定义
  - fstream fs(file_path);  // 省参了读写模式参数
- 使用
  - int tmp; //定义一个临时变量
  - while(fs >> tmp) //循环读入

### 锁
在多线程里进行同步，可以使用mutex加锁

- 头文件
  - #include \<mutex\>
- mutex声明定义
  - mutex my_mutex; //定义成全局变量
- mutex使用
  - my_mutex.lock()
  - my_mutex.unlock()
- 在上锁期间程序异常，mutex不会自动解锁，会导致死锁情况，所以引入了lock_guard和unique_lock类型，lock_guard和unique_lock在构造时进行上锁，析构时进行解锁，所以只需要在上锁时，进行定义，然后在定义域内结束执行时自动解锁
  - unique_lock<mutex> ul(my_mutex);



## 例子

### 多线程读文件值
```
#include <thread>
#include <iostream>
#include <vector>
#include <fstream>

using namespace std;

void read_file(string path, int &res) {
    fstream fs(path);
    res = 0;
    int tmp;
    while(fs >> tmp) {
        res += tmp;
    }
    fs.close();
}

int main() {
    vector<string> file_paths = { "1.txt", "2.txt", "3.txt"};
    int n = file_paths.size();
    thread t[n];
    int res_list[n];
    
    for(int i = 0; i < n; i++) {
        t[i] = thread(read_file, file_paths[i], ref(res_list[i]));
    }
    for(int i = 0; i < n; i++) {
        t[i].join();
    }
    int res = 0;
    for(int i = 0; i < n; i++) {
        res += res_list[i];
    }
    cout << res << endl;
    return res;
}
```

#### 多线程上锁
```
#include <mutex>
#include <thread>
#include <iostream>

using namespace std;

int counter = 0;
mutex my_mutex;

void increase() {
    for(int i = 0; i < 10000; i++) {
        unique_lock<mutex> ul(my_mutex);
        counter++;
    }
}

int main() {
    thread t1(increase);
    thread t2(increase);
    t1.join();
    t2.join();
    cout << counter << endl;
    return 0;
}
```


#### 单例模式
《Effective C++》提出了一种更优雅的单例模式实现，使用函数内的 local static 对象。这样，只有当第一次访问getInstance()方法时才创建实例。这种方法也被称为Meyers' Singleton。C++0x之后该实现是线程安全的，C++0x之前仍需加锁。

```
#include <iostream>

using namespace std;

class Singleton
{
private:
	Singleton() { };
	~Singleton() { };
	Singleton(const Singleton&);
	Singleton& operator=(const Singleton&);
public:
	static Singleton& getInstance() 
  {
		static Singleton instance;
		return instance;
	}
};
int main() {
    Singleton::getInstance();
    return 0;
}
```

### async使用
#include <bits/stdc++.h>

using namespace std;

string get_data_from_file(string path) {
    fstream fs(path);
    string res;
    fs >> res; 
    fs.close();
    return res;
};

int main() {
    future<string> data_from_file = async(launch::async, get_data_from_file, "1.txt");
    cout << data_from_file.get() << endl;
}