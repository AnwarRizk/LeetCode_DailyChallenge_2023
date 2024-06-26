# LeetCode Daily Challenge Problems for March

<br><br>

## Workflow Checking

<div align="center">
<img src="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/Author_Line.yml/badge.svg" alt="Checkers" width="150">
<a href="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/Author_Line.yml" taget="_blank"/>
</img>
&nbsp;
<img src="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/File_Names.yml/badge.svg" alt="Checkers" width="150">
<a href="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/File_Names.yml" taget="_blank"/>
</img>
&nbsp;
<img src="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/Daily_Problem.yml/badge.svg" alt="Checkers" width="150">
<a href="https://github.com/7oSkaaa/LeetCode_DailyChallenge_2023/actions/workflows/Daily_Problem.yml" taget="_blank"/>
</img>
</div>

<br><br>

## Problems:

1. **[Sort an Array](#1--sort-an-array)**
1. **[String Compression](#2--string-compression)**
1. **[Find the Index of the First Occurrence in a String](#3--find-the-index-of-the-first-occurrence-in-a-string)**
1. **[Count Subarrays With Fixed Bounds](#4--count-subarrays-with-fixed-bounds)**
1. **[Jump Game IV](#5--jump-game-iv)**
1. **[Kth Missing Positive Number](#06--kth-missing-positive-number)**
1. **[Minimum Time to Complete Trips](#07--minimum-time-to-complete-trips)**
1. **[Koko Eating Bananas](#08--koko-eating-bananas)**
1. **[Linked List Cycle II](#09--linked-list-cycle-ii)**

<hr>

<br><br>

## 1)  [Sort an Array](https://leetcode.com/problems/sort-an-array/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Array` `Divide and Conquer` `Sorting` `Heap (Priority Queue)` `Merge Sort` `Bucket Sort` `Radix Sort` `Counting Sort`

### Code


```cpp
class Solution {
public:

    void Merge(int l, int m, int r, vector < int >& nums){
        // [l, m] and [m + 1, r] are sorted
        int sz_1 = m - l + 1, sz_2 = r - m;
        
        // left and right are temporary arrays
        
        vector < int > left(sz_1), right(sz_2);
        // copy the elements to the temporary arrays
        for(int i = 0; i < sz_1; i++) left[i] = nums[l + i];
        for(int i = 0; i < sz_2; i++) right[i] = nums[m + 1 + i];
        
        // merge the temporary arrays to be sorted array
        int i = 0, j = 0, k = l;
        while(i < sz_1 && j < sz_2)
            nums[k++] = (left[i] <= right[j] ? left[i++] : right[j++]);
        
        // copy the remaining elements
        while(i < sz_1) nums[k++] = left[i++];
        while(j < sz_2) nums[k++] = right[j++];
        // the array is now sorted
    }

    void Merge_Sort(vector < int >& nums, int l, int r){
        // base case is when the array has only one element
        if(l >= r) return;

        // divide the array into two parts
        int m = l + (r - l) / 2;

        // sort the two parts
        Merge_Sort(nums, l, m);
        Merge_Sort(nums, m + 1, r);

        // merge the two parts
        Merge(l, m, r, nums);
    }

    vector<int> sortArray(vector<int>& nums) {
        int n = nums.size();
        Merge_Sort(nums, 0, n - 1);
        return nums;
    }
};
```

<hr>

<br><br>

## 2)  [String Compression](https://leetcode.com/problems/string-compression/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`String` `Two Pointers`

### Code


```cpp
class Solution {
public:
    int compress(vector<char>& chars) {
        int cnt = 1, index = 0;
        
        auto add_new_group = [&](char c){
            // make the current char of the new array equal c
            chars[index++] = c;

            // the number of element in the group more than 1 add the freq
            if(cnt > 1){
                for(auto& cc : to_string(cnt))
                    chars[index++] = cc;
            }

            // reset the number of elements in the group
            cnt = 1;
        };

        // loop over the chars and checks the consecutive characters
        for(int i = 1; i < chars.size(); i++){
            // if the current char equal the last one so we will add to the current group
            // otherwise we will start new group
            if(chars[i] == chars[i - 1])
                cnt++;
            else 
                add_new_group(chars[i - 1]);
        }

        // add the remainig characters
        add_new_group(chars.back());

        return index;
    }
};
```

<hr>

<br><br>

## 3)  [Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Two Pointers` `String` `String Matching`

### Code


```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        // just use find builtin function
        int idx = haystack.find(needle);

        // string::npos means that we cannot find the subtring
        return (idx == string::npos ? -1 : idx);
    }
};
```
<hr>

<br><br>

## 4)  [Count Subarrays With Fixed Bounds](https://leetcode.com/problems/count-subarrays-with-fixed-bounds/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Array` `Queue` `Sliding Window` `Monotonic Queue`

### Code


```cpp
class Solution {
public:
    long long countSubarrays(vector<int>& nums, int minK, int maxK) {
        int n = nums.size(), minPos = -1, maxPos = -1, leftBound = -1;
        long long res = 0;

        for (int i = 0; i < n; ++i) {
            
            // the index of the last invalid bound
            if (nums[i] < minK || nums[i] > maxK)
                leftBound = i;
            
            // update min position
            if (nums[i] == minK) 
                minPos = i;
            
            // update max position
            if (nums[i] == maxK)
                maxPos = i;

            // add the number of subarrays that i will be the right bound of them
            res += max(0, min(maxPos, minPos) - leftBound);
        }
        
        return res;
    }
};
```
<hr>

<br><br>

## 5)  [Jump Game IV](https://leetcode.com/problems/jump-game-iv/)

### Difficulty

![](https://img.shields.io/badge/Hard-red?style=for-the-badge)

### Related Topic

`Array` `Hash Table` `Breadth-First Search`

### Code


```cpp
class Solution {
public:
    int minJumps(vector<int>& arr) {
        // number of elements
        int n = arr.size();

        // adjacent list for each number with the indices that appears in it
        map < int, vector < int > > adj;
        for(int i = 0; i < n; i++)
            adj[arr[i]].push_back(i);

        // make distance vector to get the minimum distance for each index
        vector < int > dist(n, 1e9);

        // make bfs started from index 0
        queue < int > bfs;
        bfs.push(0);
        dist[0] = 0;

        // to check it's valid indices to add to the current queue or not
        auto add = [&](int u, int v) -> void {
            if(v < 0 || v >= n) return;
            if(dist[v] > dist[u] + 1)
                dist[v] = dist[u] + 1, bfs.push(v);
        };

        // make bfs to get the minimum distance for each index
        while(!bfs.empty()){
            // the current index
            int u = bfs.front();
            bfs.pop();

            // check can i add the next index to me
            add(u, u + 1);

            // check can i add the prev index to me
            add(u, u - 1);

            // try to move to the index of any element same number of me
            for(auto& v : adj[arr[u]])
                add(u, v);

            // clear the adjacent list for the current element to avoid redundancy
            adj[arr[u]].clear();
        }

        // return the minimum distance to reach last element
        return dist[n - 1];
    }
};
```

<hr>
<br><br>

## 06)  [Kth Missing Positive Number](https://leetcode.com/problems/kth-missing-positive-number/)

### Difficulty

![](https://img.shields.io/badge/Easy-green?style=for-the-badge)

### Related Topic

`Array` `Binary Search`

### Code


```cpp
class Solution {
public:
    int findKthPositive(vector<int>& a, int k) {
        /*
            we will make BS on asnwer to get the kth missing number
            I try to make sequence of consecutive number it will be 
            - 1 2 3 4 5 6 7 8 9 .......
            my array will be 
            - 2 3 4 7 11

            i will try to check the second element in my array the difference between it
            and my second element in the sequence will be the number of missing elements
            untill this element

            if the answer will be -1 so the answer will be out of the array 
        */

        // the bounds of the search
        int n = a.size(), l = 1, r = n, ans = 0;
        while(l <= r){
            int m = l + (r - l) / 2;
            (a[m - 1] - m < k ? l = m + 1, ans = m : r = m - 1);
        }
        return ans + k;
    }
};
```
    

<hr>
<br><br>

## 07)  [Minimum Time to Complete Trips](https://leetcode.com/problems/minimum-time-to-complete-trips/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Array` `Binary Search`

### Code


```cpp
class Solution {
public:
    long long minimumTime(vector<int>& time, int totalTrips) {
        // check the current time
        auto is_good = [&](long long t){
            // to store number of trips can make with this time
            long long ach_trips = 0;
            for(auto& bus : time)
                ach_trips += t / bus;
            // if the number of trips greater than totalTrips so it's good time
            return ach_trips >= totalTrips;
        };
        
        // search for the minimum time to make total trips with buses
        long long l = 1, r = 1, min_time = -1;

        // to get best r
        while(!is_good(r)) r *= 2;

        while(l <= r){
            long long m = l + (r - l) / 2;
            // if the time m can acheive so try number less than it to achieve best time
            (is_good(m) ? r = m - 1, min_time = m : l = m + 1);
        }

        // the minimum time for all buses to complete totalTrips
        return min_time;
    }
};
```
    
<hr>
<br><br>

## 08)  [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Array` `Binary Search`

### Code


```cpp
class Solution {
public:

    // return the ceiling of the division between a and b 
    inline int ceil(const int& a, const int& b){
        return (a + b - 1) / b;
    }

    int minEatingSpeed(vector<int>& piles, int& h) {
        auto is_good = [&](int k){
            int total_hours = 0;
            /*
                loop over the piles and calculate the total hours needed to eat
                all the bananas such that each hour you will eat number of bananas <= k
            */
            for(auto& p : piles){
                total_hours += ceil(p, k);
                if(total_hours > h)
                    return false;
            }
            
            // if the number of hours less than h so k is valid choice
            return total_hours <= h;
        };

        // make binary search to get the least valid choice
        int l = 1, r = 1, k = -1;

        // make r less power of 2 valid number
        while(!is_good(r)) r *= 2;

        while(l <= r){
            int m = l + (r - l) / 2;
            (is_good(m) ? r = m - 1, k = m : l = m + 1);
        }

        // the minimum valid k
        return k;
    }
};
```
    
<hr>
<br><br>

## 09)  [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

### Difficulty

![](https://img.shields.io/badge/Medium-orange?style=for-the-badge)

### Related Topic

`Hash Table` `Linked List` `Two Pointers`

### Code


```cpp
class Solution {
public:
    
    ListNode* find_cycle(ListNode* head, unordered_map < ListNode*, bool >& occ){
        // return the node if it is visited before
        if(!head || occ[head]) return head;
        
        // mark this node as visited node
        occ[head] = true;

        return find_cycle(head -> next, occ);
    }
    
    ListNode *detectCycle(ListNode *head) {
        // map to store the visited nodes
        unordered_map < ListNode*, bool > occ;

        // the node that make cycle
        return find_cycle(head, occ);
    }
};
```
