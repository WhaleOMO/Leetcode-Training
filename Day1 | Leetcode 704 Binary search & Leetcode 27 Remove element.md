## 704 Binary search 

**Link:** [704 Binary Search](https://leetcode.com/problems/binary-search/)

**Thoughts:** 

 - If we set r = len(nums) - 1, then we need to use l<=r in the while loop: (both end inclusive)

```C++
    int l = 0; int r = nums.size()-1;
    
    while(l <= r)   
```  
```C++
    mid = r - 1
    mid = l + 1
```

- int / int truncate towards zero (向零取整), ``floor()`` will always truncate towards negative infinity (向负无穷取整) [Ref](https://stackoverflow.com/questions/3300290/cast-to-int-vs-floor)
- sum of two big int could cause potential overflow, so its better to use subtract if possible

```C++
 int mid = (left + right) / 2;
 int mid = left + ((right - left) / 2);    // this is better
```

**Time Complexity:**  O(logN)

**Spcae Complexity:**  O(1) 

<br/>

## 27 Remove element 

**Link** [27 Remove Element](https://leetcode.com/problems/remove-element/)

**Thoughts:**

- Use two pointers, but one starts from beginning, one starts from the end (since we want to move the "deleted" elements to the end).
- Similar to Binary search, the boundary condition will be when left <= right (because inclusive)

**Code**

```C++
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int left = 0; int right = nums.size()-1;
        while(left <= right)
        {
            if(nums[right] == val)
            {
                right--;                    // if the right is val, proceed to previous element
            }
            else if(nums[left] == val)      // nums[right] is not val so it is safe to swap if needed
            {
                swap(nums[left], nums[right]);
                left++;                     // after swap, proceed to next element
            }
            else{                           // left not equal to val, no need to swap, proceed
                left++;
            }
        }
        return left;
    }
};
```

**Other Possible solution:** two pointers start from beginning, one slow one fast, the fast moves ahead slow and when slow's val is equal to val, replace its val with fast's val. Iterate untill fast reach the end of vector. 

```C++
class Solution{
public:
   int removeElement (vector<int>& nums, int val) {
       int slow = 0;
       for(int fast = 0; fast < nums.size(); fast++){
           if(val != nums[fast])
           {
               nums[slow++] = nums|fast];
           }
        }
        return slow;
   }
}
```

**Time Complexity:**  O(n)

**Spcae Complexity:**  O(1)
