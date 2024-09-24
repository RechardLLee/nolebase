

# 1.两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** _`target`_  的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

**示例 1：**

**输入：**nums = [2,7,11,15], target = 9
**输出：**[0,1]
**解释：**因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

**示例 2：**

**输入：**nums = [3,2,4], target = 6
**输出：**[1,2]

**示例 3：**

**输入：**nums = [3,3], target = 6
**输出：**[0,1]

**提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

## 解法
### 一

---
class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        int a = target;

        vector<int> output;

        for (int i=0; i<nums.size(); i++) {

            for (int j=0;j<nums.size(); j++) {

                if (i==j){

                    break;

                }

                if (nums[i]+nums[j]==a) {

                    output = {i,j};

                }

            }

        }

    return output;

    }

};

---
这种方法是暴力破解，直接循环所有的。

181ms 击败5.01%
12.72MB 击败66.64%
### 二
优化无意义的内存消耗

---
class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        vector<int> output;

        for (int i=0; i<nums.size(); i++) {

            for (int j=0;j<nums.size(); j++) {

                if (i==j){

                    break;

                }

                if (nums[i]+nums[j]==target) {

                    output = {i,j};

                }

            }

        }

    return output;

    }

};

---
相比于解法一，内存占用更少，减少了`int a = target`。事实上可以直接使用`target`

168ms 击败5.22%
12.68MB 击败72.21%

### 三
找到后直接break循环

---
class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        vector<int> output;

        for (int i=0; i<nums.size(); i++) {

            for (int j=0;j<nums.size(); j++) {

                if (i==j){

                    break;

                }

                if (nums[i]+nums[j]==target) {

                    output = {i,j};

                    break;

                }

            }

        }

    return output;

    }

};

---
减少内存占用的同时，减少执行时间，尽管时间复杂度$O(N^2)$一样，但是由于使用了类似[哨兵值](../哨兵值——优化你的循环与算法.md)的中断方法，减少了时间消耗，同时也减少了部分内存消耗。

65ms 击败24.60%
12.52MB 击败90.94%

### 四
突然发现，并不需要将`j`从0开始循环，直接从`i+1`开始即可，因为，如果在前面找得到的话，之前的循环中就应该找出来了。

---

class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        vector<int> output;

        for (int i=0; i<nums.size(); i++) {

            for (int j=i+1;j<nums.size(); j++) {

                if (nums[i]+nums[j]==target) {

                    output = {i,j};

                    break;

                }

            }

        }

    return output;

    }

};

---

67ms 击败21.17%
12.49MB 击败92.51%

### 五
使用排序+查找（快排+二分查找）

---

class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

        // 记录每个元素的值和原始索引

        vector<pair<int, int>> numIndexPairs;

        for (int i = 0; i < nums.size(); ++i) {

            numIndexPairs.push_back({nums[i], i});

        }

        // 对数组进行快速排序，只排序元素的值

        sort(numIndexPairs.begin(), numIndexPairs.end());

  

        int left = 0;

        int right = numIndexPairs.size() - 1;

        while (left < right) {

            int sum = numIndexPairs[left].first + numIndexPairs[right].first;

            if (sum == target) {

                // 找到匹配的和，返回原始索引

                return {numIndexPairs[left].second, numIndexPairs[right].second};

            } else if (sum < target) {

                // 和小于目标值，移动左指针

                left++;

            } else {

                // 和大于目标值，移动右指针

                right--;

            }

        }

        return {}; // 如果没有找到结果，返回空vector

    }

};

---

10ms 击败61.99%
13.66MB 击败60.62%
### 六
深度优化——将时间复杂度从$O(n\log n)$降低
但是由于使用了`哈希表Hash table`，通过空间换时间
对于每一个 `x`，我们首先查询哈希表中是否存在 `target - x`，然后将 `x` 插入到哈希表中。

---

class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {
    
        unordered_map<int, int> hashtable;
        
        for (int i = 0; i < nums.size(); ++i) {
        
            auto it = hashtable.find(target - nums[i]);
            
            if (it != hashtable.end()) {
            
                return {it->second, i};
                
            }
            
            hashtable[nums[i]] = i;
            
        }
        
        return {};
        
    }
    
};

---

4ms 击败93.13%
13.93MB 击败48.17%


---
# 两数相加

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

**示例 1：**

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

**输入：**l1 = [2,4,3], l2 = [5,6,4]
**输出：**[7,0,8]
**解释：**342 + 465 = 807.

**示例 2：**

**输入：**l1 = [0], l2 = [0]
**输出：**[0]

**示例 3：**

**输入：**l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
**输出：**[8,9,9,9,0,0,0,1]

**提示：**

- 每个链表中的节点数在范围 `[1, 100]` 内
- `0 <= Node.val <= 9`
- 题目数据保证列表表示的数字不含前导零

## 解法
### 一

---

/**

 * Definition for singly-linked list.

 * struct ListNode {

 *     int val;

 *     ListNode *next;

 *     ListNode() : val(0), next(nullptr) {}

 *     ListNode(int x) : val(x), next(nullptr) {}

 *     ListNode(int x, ListNode *next) : val(x), next(next) {}

 * };

 */

class Solution {

public:

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

  

    }

};

---