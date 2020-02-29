<!-- GFM-TOC -->
* [1. 有序数组的 Two Sum](#1-有序数组的-two-sum)
* [2. 两数平方和](#2-两数平方和)
* [3. 反转字符串中的元音字符](#3-反转字符串中的元音字符)
* [4. 回文字符串](#4-回文字符串)
* [5. 归并两个有序数组](#5-归并两个有序数组)
* [6. 判断链表是否存在环](#6-判断链表是否存在环)
* [7. 最长子序列](#7-最长子序列)
<!-- GFM-TOC -->

# 1. 有序数组的 Two Sum

167\. Two Sum II - Input array is sorted (Easy)

[Leetcode](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/description/)
```html
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2
```

```C++
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int i=0, j=numbers.size()-1;
        int sum=0;
        vector<int> res;
        while(i<j){
            sum=numbers[i]+numbers[j];
            if(sum==target){
                res.push_back(i+1);
                res.push_back(j+1);
                break;
            }
            else if(sum<target)
                i++;
            else
                j--;
        }
        return res;
    }
};
```
# 2. 两数平方和

633\. Sum of Square Numbers (Easy)

[Leetcode](https://leetcode.com/problems/sum-of-square-numbers/description/)
```html
Input: 5
Output: True
Explanation: 1 * 1 + 2 * 2 = 5
```

```C++
class Solution {
public:
    bool judgeSquareSum(int c) {
        if(c<0) return false;
        int tmp=(int)sqrt(c);
        long long a=0, b=tmp;   //注意使用int会溢出
        long long sum=0;    //注意使用int会溢出
        while(a<=b){    //a和b可能相等
            sum=a*a+b*b;
            if(sum==c)
                return true;
            else if(sum<c)
                a++;
            else
                b--;
        }
        return false;
    }
};
```
# 3. 反转字符串中的元音字符

345\. Reverse Vowels of a String (Easy)

[Leetcode](https://leetcode.com/problems/reverse-vowels-of-a-string/description/)
```html
Given s = "leetcode", return "leotcede".
```
```C++
class Solution {
public:
    string reverseVowels(string s) {
        unordered_set vowel={'a','e','i','o','u','A','E','I','O','U'};
        int i=0, j=s.length()-1;
        while(i<j){
            while(i<j && vowel.find(s[i])==vowel.end()) i++; //如果没找到
            while(i<j && vowel.find(s[j])==vowel.end()) j--;
            char tmp;
            tmp=s[i];
            s[i]=s[j];
            s[j]=tmp;
            i++;
            j--;
        }
        return s;
    }
};
```
# 4. 回文字符串

680\. Valid Palindrome II (Easy)

[Leetcode](https://leetcode.com/problems/valid-palindrome-ii/description/)
```html
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

```C++
class Solution {
public:
    bool validPalindrome(string s) {
        for(int i=0,j=s.length()-1;i<j;i++,j--){
            if(s[i]!=s[j])
                return (isPalindrome(s,i+1,j)||isPalindrome(s,i,j-1));
        }
        return true;
    }
    
    bool isPalindrome(string s, int i, int j){
        while(i<j){
            if(s[i++]!=s[j--])
                return false;
        }
        return true;
    }
};
```
# 5. 归并两个有序数组

88\. Merge Sorted Array (Easy)

[Leetcode](https://leetcode.com/problems/merge-sorted-array/description/)
```html
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

方法一：
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int index1=m-1, index2=n-1;
        int indexMerge=m+n-1;
        while(index1>=0 || index2>=0){
            if(index1<0)
                nums1[indexMerge--]=nums2[index2--];
            else if(index2<0)
                nums1[indexMerge--]=nums1[index1--];
            else if(nums1[index1]<nums2[index2])
                nums1[indexMerge--]=nums2[index2--];
            else
                nums1[indexMerge--]=nums1[index1--];
        }
    }
};
```
方法二：思路一样，只是换一种写法
```C++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        if(!n) return;
        if(!m) nums1=nums2;
        while(n){
            if(!m || nums1[m-1]<nums2[n-1]){
                nums1[m+n-1]=nums2[n-1];
                n--;
            }
            else{
                nums1[m+n-1]=nums1[m-1];
                m--;
            }
        }
        return;
    }
};
```
# 6. 判断链表是否存在环

141\. Linked List Cycle (Easy)

[Leetcode](https://leetcode.com/problems/linked-list-cycle/description/)
```html
Input: head=[3,2,0,-4], pos=1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
```

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if(head==NULL) return false;
        ListNode *p=head, *q=head->next;
        while(p!=NULL && q!=NULL && q->next!=NULL){
            if(p==q)
                return true;
            p=p->next;
            q=q->next->next;
        }
        return false;
    }
};
```
# 7. 最长子序列

524\. Longest Word in Dictionary through Deleting (Medium)

[Leetcode](https://leetcode.com/problems/longest-word-in-dictionary-through-deleting/description/)
```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output:
"apple"
```
```C++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        string ans;
        for(int i=0;i<d.size();i++){
            if(isSubString(s,d[i])){ //若d[i]是s的子串
                if(ans==""||ans.length()<d[i].length()) //初始值ans为空或d[i]子串比现有子串更长
                    ans = d[i];
                else if(ans.length()==d[i].length()) //当d中出现一样长的子串,取字典序最小的
                    ans=min(ans,d[i]);
            }
        }
        return ans;
    }
    
    bool isSubString(string s, string t){ //判断t是否为s的子串
        int j=0;
        for(int i=0;i<s.length();i++){ //若t中与s相匹配的字符长度等于t.length()说明t是s的子串
            if(s[i]==t[j])
                j++;
        }
        return (j==t.length());
    }
};
```
