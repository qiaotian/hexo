title: unordered_set
data: 2016-02-18
tag: STL
---

## 利用迭代器删除**unordered_set**成员的陷阱
在处理leetcode的Word Ladder问题的过程中，出现了在的**dict**（unordered_set类型）寻找与**word**（string）相邻（only one letter can be changed at a time）单词的需求。有两种方案，其一是用**a~z**替换**word**中的某一位，然后在**dict**查找该单词；另一种是遍历dict中的每一个单词，检查该单词是否与**word**相邻。在使用第二种方式的过程中遇到如下*algorithm 1*中遇到的问题：
### algorithm 1
```c++
#include <unordered_set>
#include <iostream>
int main() 
{
	std::unordered_set<int> c = {1, 2, 3, 4, 5, 6, 7, 8, 9};
	// erase all odd numbers from c
	for(auto it = c.begin(); it !=  c.end(); )
		if(*it % 2 == 1) 
			c.erase(it);
		else
			++it;
	for(int n : c)
		std::cout << n << ' '; 
}
```
运行输出：
```
runtime error
```
我们知道，unordered_set是一种关联容器（associated container），用于存储Key互异的对象，插入和删除操作平均下来具有O(1)的时间复杂度，最坏情况和容器容量具有线性关系。**unordered_set**使用红黑树作为存储结构，因此并非顺序存储。我们希望**it**指向被删除元素的后续元素或者前序元素，然而当我们使用**c.erase(it)**后，**it**将可能指向一个错误内存地址，而不会像**vector**那样仍然指向一个有效地址。正如在**algorithm 2**中，我们将**unordered_set**容器更换为**vector**，输出正确。
```c++
#include <vector>
#include <iostream>
int main() 
{
	std::vector<int> c = {1, 2, 3, 4, 5, 6, 7, 8, 9}; // pay attention here
	// erase all odd numbers from c
	for(auto it = c.begin(); it !=  c.end(); )
		if(*it % 2 == 1) 
			c.erase(it);
		else
			++it;
	for(int n : c)
		std::cout << n << ' '; 
}
```
运行输出：
```c++
2 4 6 8 
```
针对**unordered_set**的这种特性，将迭代器指向**erase()**的返回值，也就是被删除元素的后继元素，就可以解决该问题，如下所示：
### algorithm 2
```c++
#include <unordered_set>
#include <iostream>
int main()
{
    std::unordered_set<int> c = {1, 2, 3, 4, 5, 6, 7, 8, 9};
    // erase all odd numbers from c
    for(auto it = c.begin(); it != c.end(); )
        if(*it % 2 == 1)
            it = c.erase(it); // pay attentio to this difference
        else
            ++it;
    for(int n : c)
        std::cout << n << ' ';
}
```
运行输出：
```c++
2 4 6 8
```