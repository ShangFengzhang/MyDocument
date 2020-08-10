## 查找

<h3>1.二分查找（Binary Search）</h3>

>二分查找也称折半查找（Binary Search），它是一种效率较高的查找方法。但是，折半查找要求线性表必须采用**顺序存储结构**，而且表中元素按关键字**有序**排列。


Arrays类中实现了binarySearch方法并重载了多种形式

二分查找的基本实现：  
1. 预处理——首先对无序的集合进行排序  
2. 二分查找——使用循环或递归在每次比较后将空间划分为两半（分治）  
3. 后处理——选定查找对象

- 模版一

``` java
int binarySearch(int[] nums, int target){
  if(nums == null || nums.length == 0)
    return -1;

  int left = 0, right = nums.length - 1;
  while(left <= right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid - 1; }
  }

  // End Condition: left > right
  return -1;
}
```  

重点：   
1. left=0;right=nums.length-1;  
2. 循环条件 left<=right  
3. 比较条件 right=mid-1;
4. 比较返回结果 nums[mid]==target



- 模版二

``` java
int binarySearch(int[] nums, int target){
  if(nums == null || nums.length == 0)
    return -1;

  int left = 0, right = nums.length;
  while(left < right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid; }
  }

  // Post-processing:
  // End Condition: left == right
  if(left != nums.length && nums[left] == target) return left;
  return -1;
}
```

重点：   
1. left=0;right=nums.length;  
2. 循环条件 left < right   
3. 比较条件 right=mid  
4. 后处理 left != nums.length && nums[left] == target



- 模版三

``` java
int binarySearch(int[] nums, int target) {
    if (nums == null || nums.length == 0)
        return -1;

    int left = 0, right = nums.length - 1;
    while (left + 1 < right){
        // Prevent (left + right) overflow
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }

    // Post-processing:
    // End Condition: left + 1 == right
    if(nums[left] == target) return left;
    if(nums[right] == target) return right;
    return -1;
}
```

重点：   
1. left=0;right=nums.length-1;  
2. 循环条件 left+1 < right   
3. 比较条件 left=mid;right=mid  
4. 后处理  nums[left] == target nums[right] == target


