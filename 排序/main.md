# 排序算法
[参考网站](https://www.geeksforgeeks.org/sorting-algorithms/)
<table>
<tr>
    <td>排序算法</td>
    <td>best case</td>
    <td>average case</td>
    <td>worest case</td>
    <td>空间复杂度</td>
    <td>是否稳定</td>
    <td>使用方法</td>
</tr>
<tr>
    <td>冒泡排序</td>
    <td>n</td>
    <td>n^2</td>
    <td>n^2</td>
    <td>1</td>
    <td>是</td>
    <td>交换</td>
</tr>
<tr>
    <td>插入排序</td>
    <td>n</td>
    <td>n^2</td>
    <td>n^2</td>
    <td>1</td>
    <td>是</td>
    <td>插入</td>
</tr>
<tr>
    <td>选择排序</td>
    <td>n^2</td>
    <td>n^2</td>
    <td>n^2</td>
    <td>1</td>
    <td>否</td>
    <td>选择</td>
</tr>
<tr>
    <td>归并排序</td>
    <td>Nlog(N)</td>
    <td>Nlog(N)</td>
    <td>Nlog(N)</td>
    <td>n</td>
    <td>是</td>
    <td>归并</td>
</tr>
<tr>
    <td>快速排序</td>
    <td>Nlog(N)</td>
    <td>Nlog(N)</td>
    <td>n^2</td>
    <td>log(N)</td>
    <td>否</td>
    <td>分治</td>
</tr>
<tr>
    <td>堆排序</td>
    <td>Nlog(N)</td>
    <td>Nlog(N)</td>
    <td>Nlog(N)</td>
    <td>1</td>
    <td>否</td>
    <td>选择</td>
</tr>
<tr>
    <td>桶排序</td>
    <td>N</td>
    <td>N</td>
    <td>n^2</td>
    <td>n+k(桶数量)</td>
    <td>否</td>
    <td>选择</td>
</tr>
</table>

### 注意点
**稳定性：相同值的次序在排序后是否有变化**
- 选择排序、快速排序、堆排序**不稳定**

**快速排序、堆排序、归并排序对比**
- 默认快速排序最好。相比于堆排序，有更快的速度；相比于数组的排序，不需要O(n)的额外空间，快速排序仅需要log(n)的空间用于递归调用
- 归并排序在链表排序中表现很好，不需要额外的O(n)空间
- 归并排序也是对存储在访问缓慢的媒体（例如磁盘存储或网络附加存储）上的超大数据集进行**外部排序**的首选算法

**外排序**：大文件的排序，即待排序的记录存储在外存储器上，待排序的文件无法一次装入内存
- 读入内存可以处理的大小的文件，创建新的临时文件存入k个小顺串
- 读入k个小顺串的头部部分进入**输入缓冲区**，并预留出输出缓冲区空间
- 对k个顺串进行多路归并排序，一旦输出缓冲区满，将缓冲区内输出至目标文件；一旦k个输入缓冲区的数据用完，再从关联的文件读入文件，直至文件读完

**外排序的K值问题**：大文件中的第k大值
- 选择合适的映射函数，创建新的k个临时桶文件
- 读入内存可以处理的大小的文件，使用映射依次将值写入桶文件，并在内存中记录每个桶的数据量
- 确定第k大的值在哪个桶中，若数据量仍然很大，无法在内存中排序，重复上述的三个步骤；若数据量较小，可以在内存中计算，直接计算出k大值

**外排序的最大重复值问题**：大文件中重复量最大的文件
- 选择合适的映射函数，创建新的k个临时桶文件
- 读入内存可以处理的大小的文件，使用映射依次将值写入桶文件
- 对每个桶文件进行重复值统计，若数据量仍然很大，无法在内存中排序，重复上述的三个步骤；若数据量较小，可以在内存中计算，直接几个桶中的计数
- 在每个桶中最大重复的计数对比，得到重复量最大的文件

**选择第k大的元素**：快速选择排序，最小堆排序

[大量数据的排序问题](https://www.cnblogs.com/end/archive/2011/06/01/2067207.html)

leetcode 相关题目

排序类（Sort）：

基础知识：快速排序（Quick Sort）， 归并排序（Merge Sort）的原理与代码实现。需要能讲明白代码中每一行的目的。快速排序时间复杂度平均状态下O（NlogN），空间复杂度O（1），归并排序最坏情况下时间复杂度O（NlogN），空间复杂度O（N）
入门题目：
~~Leetcode 148. Sort List~~
~~Leetcode 56. Merge Intervals~~
~~Leetcode 27. Remove elements~~
进阶题目：
~~Leetcode 179. Largest Number~~
~~Leetcode 75. Sort Colors~~
~~Leetcode 215. Kth Largest Element （可以用堆的解法替代）~~
~~ELeetcode 4. Median of Two Sorted Arrays~~
注意：后两题是与快速排序非常相似的快速选择（Quick Select）算法，面试中很常考


冒泡排序
```
// 数组没有size()函数，需将长度作为参数传递进来
// 若内层循环没有swap过，说明剩余数据有序，可以直接退出，
// 所以冒泡排序的时间复杂度最佳为n
void bubbleSort(int[] nums, int n) {
    for(int i = 0; i < n; i++) {
        bool swapped = false;
        for(int j = 1; j < n - i; j++) {
            if(nums[j] < nums[j-1]) {
                swapped = true;
                int tmp = nums[j-1];
                nums[j] = nums[j-1];
                nums[j-1] = tmp;
            }
        }
        if(!swapped) break;
    }
}
```
插入排序
```
void insertSort(int[] nums, int n) {
    for(int i = 1; i < n; i++) {
        int key = nums[i];
        int j = i - 1;
        while(j >= 0 && nums[j] > key) {
            nums[j+1] = nums[j];
            j--;
        }
        nums[j+1] = key;
    }
}
```
选择排序
```
void selectionSort(int[] nums, int n) {
    for(int i = 0; i < n; i++) {
        int min_index = i, min = nums[i];
        for(int j = i; j < n; j++) {
            if(nums[j] < min){
                min_index = j;
                min = nums[j];
            }
        }
        if(min_index != i) {
            int tmp = nums[i];
            nums[i] = nums[min_index];
            nums[min_index] = tmp;
        }
    }
}
```
归并排序
```
void merge(int[] nums, int start, int mid, int end) {
    int len1 = mid - start + 1, len2 = end - mid;
    int i, j, k = start;
    int arr1[len1], arr[len2];
    for(i = 0; i < len1; i++) {
        arr1[i] = nums[k];
        k++;
    }
    for(j = 0; j < len2; j++) {
        arr2[i] = nums[k];
        k++
    }
    i = 0; j = 0; k = start;
    while(i < len1 && j < len2) {
        if(arr1[i] < arr2[j]) {
            nums[k] = arr1[i];
            i++;
        } else {
            nums[k] = arr2[j];
            j++;
        }
        k++;
    }
    while(i < len1) {
        nums[k] = arr1[i];
        k++; i++;
    }
    while(j < len2) {
        nums[k] = arr2[j];
        k++; j++;
    }
}
void mergeSort(int[] nums, int start, int end) {
    if(start >=  end) return;
    int mid = (start + end) / 2;
    mergeSort(nums, start, mid);
    mergeSort(nums, mid+1, end);
    merge(nums, start, mid, end);
}
```
快速排序
```
int partition(int[] nums, int start, int end) {
    int pivot = nums[end];
    int index = start - 1;
    for(int i = start; i < end; i++) {
        if(nums[i] < pivot) {
            index++;
            int tmp = nums[i];
            nums[i] = nums[index];
            nums[index] = tmp;
        }
    }
    int tmp = nums[index+1];
    nums[index+1] = nums[end];
    nums[end] = tmp;
    return index + 1;
}
quickSort(int[] nums, int start, int end) {
    if(start < end) {
        int pivot = partition(nums, start, end);
        quickSort(nums, pivot, start, pivot-1);
        quickSort(nums, pivot, pivot+1, end);
    }
}
```
快速选择排序
```
int partition(vector<int>& nums, int start, int end) {
    int pivot = nums[end];
    int i = start - 1;
    for(int j = start; j < end; j++) {
        if(nums[j] < pivot) {
            i++;
            swap(nums[i], nums[j]);
        }
    }
    swap(nums[i+1], nums[end]);
    return i + 1;
}

int findKthLargest(vector<int>& nums, int k) {
    k = nums.size() - k;
    int start = 0, end = nums.size() - 1;
    while(start < end) {
        int pivot = partition(nums, start, end);
        if(pivot < k) {
            start = pivot + 1;
        } else if(pivot > k) {
            end = pivot - 1;
        } else {
            return nums[k];
        }
    }
    return nums[k];
}
```
堆排序
```
void heapify(int nums[], int N, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    if(left < N && nums[left] > nums[largest]) {
        largest = left;
    }
    if(right < N && nums[right] > nums[largest]) {
        largest = right;
    }
    if(largest != i) {
        int tmp = nums[i];
        nums[i] = nums[largest];
        nums[largest] = tmp;
        heapify(nums, N, largest);
    }
}

void heapSort(int nums[], int n) {
    for(int i = n/2-1; i >= 0; i--) {
        heapify(nums, n, i);
    }
    for(int i = n-1; i >= 0; i--) {
        int tmp = nums[0];
        nums[0] = nums[i];
        nums[i] = tmp;
        heapify(nums, i, 0);
    }
}
```
桶排序
```
// Sort a large set of floating point numbers which are in range from 0.0 to 1.0 
void bucketSort(float arr[], int n)
{
     
    // 1) Create n empty buckets
    vector<float> b[n];
 
    // 2) Put array elements
    // in different buckets
    for (int i = 0; i < n; i++) {
        int bi = n * arr[i]; // Index in bucket
        b[bi].push_back(arr[i]);
    }
 
    // 3) Sort individual buckets
    for (int i = 0; i < n; i++)
        sort(b[i].begin(), b[i].end());
 
    // 4) Concatenate all buckets into arr[]
    int index = 0;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < b[i].size(); j++)
            arr[index++] = b[i][j];
}
 
```