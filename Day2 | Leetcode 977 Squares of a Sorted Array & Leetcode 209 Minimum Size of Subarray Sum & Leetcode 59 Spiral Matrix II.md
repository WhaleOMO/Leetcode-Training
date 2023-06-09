## 977 Squares of a Sorted Array 

**Link:** [977 Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/description/)

**Thoughts:** 

* Two pointers because the biggest element after squaring could only be from the front or end of the vector. So two pointers: one starts from the first, and one starts from the last element, moves toward the middle, and compares every step.
* the return type is a new vector so that the new vector can be filled from the last element to the first (no need to swap... my first idea is to swap elements)

**Code:**

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
       vector<int> res(nums.size());    // new vector
       int left = 0;  int right = nums.size()-1;    
       int k = nums.size()-1; // backward filling
       while(left <= right)
       {
           if(fabs(nums[left]) > fabs(nums[right]))
           {
               res[k] = nums[left] * nums[left];
               left++;
           }
           else
           {
               res[k] = nums[right] * nums[right];
               right--;
           }
           k--;
       }
       return res;
    }
};
```

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(1) 

<br/>

## 209 Minimum Size of Subarray Sum 

**Link:** [209 Minimum Size of Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)

**Thoughts:**

* Sliding window => reduce operating on the same element multiple times.
* The key is iterating the **right pointer** instead of the left. The window first expands, then shrinks, and keeps expanding and shrinking...... while the entire window shifts towards the end.

**Code**

```C++
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
       int left = 0;
       int sum = 0;
       int minLen = INT32_MAX;
       for(int right = 0; right < nums.size(); right++)
       {
           sum += nums[right];
           // window fulfills the requirement, so
           // shrink window by moving left ptr
           while(sum >= target)     
           {
               
               minLen = min(minLen, right-left+1);
               // be careful the order here
               // subtract current, then move
               sum -= nums[left];
               left++; 
           }
       }
       return minLen == INT32_MAX ? 0: minLen;
    }
};
```

**Time Complexity:**  each element will be included in the window once and thrown out  from the window once, so two operations for one element, thus 2 * n = O(n)

**Spcae Complexity:**  O(1)

<br/>

## 59 Spiral Matrix II 

**Link:** [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)

**Thoughts:** 

* Problem 1: consider **when to update the pointers**. It is better to update pointers after a full cycle so that the first for loop will not affect the next one... and it is easier to consider the edge cases. 
* Problem 2: (Inclusive/Exclusive) If iterating non-inclusively (meaning not including the bound), it is necessary to check if n is an odd number, so we have to fill in the last middle element manually (since it belongs to a new cycle and the loop does not consider that)
* Problem 2: when initializing a new vector and filling with a default value, do like `` vector<int> v(n, 0))`` where n is the amount, and 0 is the default value.

**Code**

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        // new 2D array filled with 0s
        vector<vector<int>> result(n, vector<int>(n,0));
        
        int left = 0, right = n-1;
        int top = 0, bottom = n-1;
        int val = 0;

        while(left <= right && top <= bottom)
        {
            // right direction
            for(int cur = left; cur <= right-1; cur++)
              	// self increase then assign 
                result[top][cur] = ++val;  
            
            // down direction
            for(int cur = top; cur <= bottom-1; cur++)
                result[cur][right] = ++val;

            // left direction
            for(int cur = right; cur >= left+1; cur--)
                result[bottom][cur] = ++val;
            
            // up direction
            for(int cur = bottom; cur >= top+1; cur--)
                result[cur][left] = ++val;
            
            // update pointers after a full cycle
            // so if n is odd, there would be one missing
            // element that needs to be filled after the loop
            top++;
            left++;
            bottom--;
            right--;
        }

        // edge case, when n is odd, 
        // the last element will not be in a full cycle
        // so fill this value outside the loop
        if(n==1 || n%3 == 0)
        {
            result[n/3][n/3] = ++val;
        }

        return result;
    }
};
```

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(1) 