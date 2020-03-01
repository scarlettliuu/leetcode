<!-- GFM-TOC -->
* [堆](#堆)
    * [1. Kth Element](#1-kth-element)
* [桶排序](#桶排序)
    * [1. 出现频率最多的 k 个元素](#1-出现频率最多的-k-个元素)
    * [2. 按照字符出现次数对字符串排序](#2-按照字符出现次数对字符串排序)
* [荷兰国旗问题](#荷兰国旗问题)
    * [1. 按颜色进行排序](#1-按颜色进行排序)
<!-- GFM-TOC -->


# 堆

## 1. Kth Element

215\. Kth Largest Element in an Array (Medium)

[Leetcode](https://leetcode.com/problems/kth-largest-element-in-an-array/description/)
```html
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```
```C++
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        //method 4
        /*topk(前k大)用小根堆,维护堆大小不超过k即可
          每次压入堆前和堆顶元素比较,如果比堆顶元素还小,直接扔掉;否则压入堆
          检查堆大小是否超过k,如果超过,弹出堆顶
          复杂度nlogk
          
          避免使用大根堆,因为需要把所有元素压入堆,复杂度nlogn,而且还浪费内存
          
          注意:
                求前k大,用小根堆;求前k小,用大根堆
        */
        priority_queue<int, vector<int>, greater<int> > pq;
        for(int num:nums){
            pq.push(num);
            if(pq.size()>k)
                pq.pop();
        }
        return pq.top();
    }
};
```
## 2. 出现频率最多的 k 个元素

347\. Top K Frequent Elements (Medium)

[Leetcode](https://leetcode.com/problems/top-k-frequent-elements/description/)
```html
Input: [1,1,1,2,2,3], k = 2 
Output: [1,2]
```
```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        if(k==0 || nums.empty()) return {};
        unordered_map<int,int> count;
        for(int num:nums) count[num]++; //统计nums中元素出现次数
        priority_queue<int,vector<int>,greater<int>> pq;
        for(auto it=count.begin();it!=count.end();it++){
            if(pq.size()<k) pq.push(it->second);
            else if(it->second > pq.top()){
                pq.pop();
                pq.push(it->second);
            }
        }
        int cutOff=pq.top();
        vector<int> res;
        for(auto it=count.begin();it!=count.end();it++){
            if(it->second >= cutOff)
                res.push_back(it->first);
        }
        return res;
    }
};
```
# 桶排序

## 1. 按照字符出现次数对字符串排序

451\. Sort Characters By Frequency (Medium)

[Leetcode](https://leetcode.com/problems/sort-characters-by-frequency/description/)
```html
Input: "tree"
Output: "eert"
Explanation:
'e' appears twice while 'r' and 't' both appear once.
So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.
```
**方法一: 桶排序**
```C++
class Solution {
public:
    string frequencySort(string s) {
        //桶排序
        unordered_map<char,int> freq; //统计每个字符出现次数
        vector<string> bucket(s.size()+1,""); //用s.size()+1个空字符串初始化
        string res;
        
        for(char c:s) freq[c]++;
        //把出现次数为i的字符都放在bucket[i]中
        //如"tree"中e出现2次,则bucket[2]处的字符串为"ee",同理bucket[1]处的字符串为"tr"
        for(auto &it:freq){
            char c=it.first;
            int i=it.second;
            bucket[i].append(i,c);
        }
        //倒序输出,由上面知出现频次高的字符必出现在大下标处
        for(int i=s.size();i>0;i--){
            if(!bucket[i].empty())
                res.append(bucket[i]);
        }
        return res;
    }
};
```
**方法二: 堆排序**
```C++
class Solution {
public:
    string frequencySort(string s) {
        //堆排序,此处使用大根堆
        string res;
        using  pair = pair<char,int>;
        unordered_map<char,int> hash;
        //定义优先队列的排序规则,使用匿名函数
        auto cmp = [](pair a,pair b){ return a.second<b.second;};
        priority_queue<pair,vector<pair>,decltype(cmp)> pq(cmp);
        //统计每个字符出现次数
        for(char c:s) hash[c]++;
        //所有键值对入优先队列,即按大根堆排序
        for(auto it=hash.begin();it!=hash.end();it++){
            pair temp = make_pair(it->first,it->second);
            pq.push(temp);
        }
        while(!pq.empty()){
            auto it=pq.top();
            while(it.second--)
                res += it.first;
            pq.pop();
        }
        return res;
    }
};```
# 荷兰国旗问题

## 1. 按颜色进行排序

75\. Sort Colors (Medium)

[Leetcode](https://leetcode.com/problems/sort-colors/description/)
```html
Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
**解法(力扣官方)**
   用三个指针P0, p2, curr分别追踪0的最右边界, 2的最左边界和当前考虑元素
   沿着数组移动curr指针,若```html nums[curr]==0```,将其与```html nums[p0]```互换;
   若```html nums[curr]==2```,则与``` html nums[p2]```互换
```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0=0, p2=nums.size()-1;
        int curr=0;
        while(curr<=p2){
            if(nums[curr]==0)
                swap(nums[curr++],nums[p0++]);
            else if(nums[curr]==2)
                swap(nums[curr],nums[p2--]);
            else
                curr++;
        }
    }
};```
