基础知识：（https://zh.wikipedia.org/wiki/Trie）；多数情况下可以通过用一个set来记录所有单词的prefix来替代，时间复杂度不变，但空间复杂度略高
常见题目：
~~Leetcode 208 Implement Trie (Prefix Tree)~~
~~Leetcode 211 Design Add and Search Words Data Structure~~
~~Leetcode 1268 Search Suggestions System~~
~~Leetcode 212 Word Search II~~
~~Leetcode 1166 Design File System~~

字典树：
leetcode 1032. 字符流
Leetcode 212 Word Search II （I, II）
Leetcode 472 Concatenated Words



struct Node {
    Node* next[26];
    bool is_end;
    Node() {
        is_end = false;
        for(auto &it : next) {
            it = nullptr;
        }
    }
};

class Trie {
private:
    Node* root;
public:
    Trie() {
        root = new Node();
    }
    
    void insert(string word) {
        Node* tmp = root;
        for(auto c: word) {
            if(tmp->next[c-'a'] == nullptr) {
                tmp->next[c-'a'] = new Node();
            }
            tmp = tmp->next[c-'a'];
        }
        tmp->is_end = true;
    }
    
    bool search(string word) {
        Node* tmp = root;
        for(auto c: word) {
            if(tmp->next[c-'a'] == nullptr) {
                return false;
            }
            tmp = tmp->next[c-'a'];
        }
        return tmp->is_end;
    }
    
    bool startsWith(string prefix) {
        Node* tmp = root;
        for(auto c: prefix) {
            if(tmp->next[c-'a'] == nullptr) {
                return false;
            }
            tmp = tmp->next[c-'a'];
        }
        return true;
    }
};