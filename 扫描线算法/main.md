扫描线算法（Sweep Line）

基础知识：一个很巧妙的解决时间安排冲突的算法，本身比较容易些也很容易理解
常见题目：
~~ Leetcode 253 Meeting Room II（Meeting Room I也可以使用）~~
~~Leetcode 1094 Car Pooling~~
~~Leetcode 218 The Skyline Problem~~
~~Leetcode 759 Employee Free Time~~


给你输入若干形如 [begin, end] 的区间，代表若干会议的开始时间和结束时间，请你计算至少需要申请多少间会议室。
```
int minMeetingRooms(vector<vector<int>>& intervals) {
    int n = intervals.size();
    vector<int> starts(n);
    vector<int> ends(n);
    for(int i = 0; i < n; i++) {
        starts[i] = intervals[i][0];
        ends[i] = intervals[i][1];
    }
    sort(starts.begin(), starts.end());
    sort(ends.begin(), ends.end());
    int count = 0, res = 0;
    int i1 = 0, i2 = 0;
    while(i1 < n && i2 < n) {
        if(starts[i1] < ends[i2]) {
            count++;
            i1++;
        } else {
            count--;
            i2++;
        }
        res = max(res, count);
    }
    return res;
}
```
