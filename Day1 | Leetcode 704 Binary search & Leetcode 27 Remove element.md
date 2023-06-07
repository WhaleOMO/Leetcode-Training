## 704 Binary search 

**Link:** [704 Binary Search](https://leetcode.com/problems/binary-search/)

**Thoughts:** 

 - If we set r = len(nums) - 1, then we need to use l<=r in the while loop: (both end inclusive)

```
    int l = 0; int r = nums.size()-1;
    
    while(l <= r):    
```  
```
    mid = r - 1
    mid = l + 1
```

- int / int truncate towards zero (向零取整), ``floor()`` will always truncate towards negative infinity (向负无穷取整) [Ref](https://stackoverflow.com/questions/3300290/cast-to-int-vs-floor)
- sum of two big int could cause potential overflow, so its better to use subtract if possible

```
 int mid = (left + right) / 2;
 int mid = left + ((right - left) / 2);    // this is better
```


**Time Complexity:**  O(logN)

**Spcae Complexity:**  O(1)
