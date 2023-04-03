# 并查集
刷题时399题首次用到**并查集**方法


leetcode并查集相关经典题目(思路、分析、代码)
easy
 - [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/description/)
 - [684. 冗余链接](https://leetcode.cn/problems/redundant-connection/)
 - [1319. 连通网络的操作次数](https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected/)
 - [990. 等式方程的可满足性](https://leetcode-cn.com/problems/satisfiability-of-equality-equations/)
 - [305. 岛屿数量II](https://leetcode.cn/problems/number-of-islands-ii/)
 - [737. Sentence Similarity II](https://leetcode.cn/problems/sentence-similarity-ii/)


hard
- [399. 除法求值(带有权重的并查集)](https://leetcode.cn/problems/evaluate-division/description/)
- [721. 账户合并(应用题)](https://leetcode.cn/problems/accounts-merge/)

并查集（Union Find）：把两个或者多个集合合并为一个集合


### 并查集代码框架
```
#include <iostream>
using namespace std;

vector<int> father;
vector<int> size; // 作为根节点的大小，用于merge过程中选择根节点
int set; // 集合数量
void init(int n) {
    father.resize(n);
    size.resize(n);
    for(int i = 0; i < n; i++) {
        father[i] = i;
        size[i] = 1;
    }
    set = n;
}

int find(int x) {
    if(father[x] == x) return x;
    return father[x] = find(father[x]);
}

void merge(int x, int y) {
    int rootX = find(x);
    int rootY = find(y);
    if(rootX == rootY) return;
    if(size[rootX] > size[rootY]) {
        father[rootY] = rootX;
        size[rootX] += size[rootY];
    } else {
        father[rootX] = rootY;
        size[rootY] += size[rootX];
    }
    set--;
}
```