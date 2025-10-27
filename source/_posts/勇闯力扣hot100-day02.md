---
title: 勇闯力扣hot100-day02
date: 2025-10-15 10:15:31
tags: 算法
---

> 2025.10.15

# 勇闯力扣hot100-day02

## 1.移动零

### 题目

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

### 解题过程

首先这题一看主要的操作就是数组元素的移动，二话不说，直接暴力求解，判断当前元素是否为0，为0就将其他元素往前移，再把0插入到最后。

代码如下：

```java
class Solution {
    public void moveZeroes(int[] nums) {
		int d = nums.length - 1;
        for(int i = 0;i < nums.length;i++){
            if(nums[i] == 0){
                for(int j = i;j < d;j++){
                    nums[j] = nums[j+1];
                }
                if(d > i){
                    nums[d] = 0;
                    d -= 1;
                    i = -1;
                }
            }
        }
    }
}
```

成功通过所有案例，但是我们可以发现，暴力求解虽然可以得出答案，但是并不是最优解，耗时为221ms。

我们可以发现，当前的时间复杂度O(n^2)，主要是因为数组元素的移动，如果不移动数组元素，时间复杂度就可以降很多。

此时考虑到通过双指针进行解决，一个指针用来判断元素，一个用来指向要被替换的元素。即a指针判断是否为0，如果不为0就将a指针的元素替换到b指针对应的元素，然后b指针移动；如果为0就a指针移动，b指针不动。具体代码如下：

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int a,b=0;
        for(a = 0;a < nums.length;a++){
            if(nums[a] != 0) {
                nums[b++] = nums[a];
            }
        }
        for ( int i = b;i < nums.length;i++){
            nums[i] = 0;
        }
    }
}
```

## 2. 盛最多水的容器

### 题目

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。

找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

### 解题过程

要装下最多的水其实就是算宽度乘以较低的高度，这让我们想到木桶效应中的，一个木桶能装多少水取决于最短的那块板。

在看到图片后我们会发现，使用双指针从两边往中间靠拢的时候，一开始指向的分别是0和8对应的元素，我们会发现，如果移动最右侧的柱子，宽度减少了，但是高度还是不会发生变化，依旧还是较矮的板，反而导致能装的水变少了。所有我们要改变的应该是两侧中较矮的一侧。

```java
class Solution {
    public int maxArea(int[] nums) {
        int a = 0,b = nums.length - 1,max = 0;
        while(a < b){
            int now = (b-a)*Math.min(nums[a],nums[b]);
            max = Math.max(max,now);
            if(nums[a] < nums[b]){
                a++;
            }else{
                b--;
            }
        }
        return max;
    }
}
```

## 3.三数之和

### 题目

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

### 解题过程

一开始看到这套题目的时候，依旧暴力解法开局，三个for循环尝试解决，但是会发现三个for循环的时间复杂度太高，会超出时间限制。

于是尝试用双指针解决，如果要用双指针就需要对数组先进行一次排序，这样才知道三数和是大了还是小了才能进行调整。

代码入下：

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> arr = new ArrayList<>();
        // 将传入的数组进行排序
        Arrays.sort(nums);

        for(int i = 0;i < nums.length;i++){
            // 当i对应元素大于0时，三个数相加不可能为0了
            if(nums[i] > 0) break;
            // 去掉重复的元素避免重复运算
            if(i > 0 && nums[i] == nums[i-1]){
                continue;
            }
            // 双指针 le从小值到大 ri从大值到小
            int le = i+1;
            int ri = nums.length-1;

            while (le < ri){
                int sum = nums[i]+nums[le]+nums[ri];
                // 如果三数和大于0 说明ri需要往左移
                if (sum > 0){
                    ri--;
                // 如果三数和小于0 说明le需要往右移
                }else if(sum < 0) {
                    le++;
                }else {
                    // 当三数和为0时间存入list中。
                    arr.add(Arrays.asList(nums[i],nums[le],nums[ri]));

                    // 防止le溢出并且跳过重复部分
                    while(le+1 < nums.length && nums[le+1] == nums[le]){
                        le++;
                    }
                    // 防止ri溢出并且跳过重复部分
                    while(ri-1 > 0 && nums[ri-1] == nums[ri]){
                        ri--;
                    }
                    le++;
                    ri--;
                }
            }
        }
        return arr;
    }
}
```

## 4. 接雨水

### 题目

给定 `n` 个非负整数表示每个宽度为 `1` 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/22/rainwatertrap.png)

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```

**示例 2：**

```
输入：height = [4,2,0,3,2,5]
输出：9
```

### 解题过程

这道题比较难，直接上大佬解法。

#### 解法一（2025.10.16超时）

一层一层的进行计算，如果当前格的两侧有比他高的部分那么就能存一点水。

```java
class Solution {
    public int trap(int[] nums) {
        int sum = 0;
        // 找到最高的柱子，水的高度不会超过这个柱子
        int max_height = 0;
        for(int i = 0;i < nums.length;i++){
            max_height = Math.max(max_height,nums[i]);
        }

        // 外层循环水的高度
        for(int i = 1;i <= max_height;i++){
            int f = 0;
            int temp = 0;
            // 内层循环每一个柱子
            for(int j = 0;j < nums.length;j++){
                // 如果当前格柱子大于等于水高，那么开始计算或结束计算，将存入的水登记。
                if(nums[j] >= i){
                    f = 1;
                    sum += temp;
                    temp=0;
                // 如果当前格柱子小于水高，那么说明可以留住水。
                }else if(nums[j] < i && f == 1) {
                    temp++;
                }
            }
        }
        return sum;
    }
}
```

#### 解法二（2025.10.16 超时）

一列一列的判断过去，通过两个指向两侧最高柱子的指针来计算能存多少水。

```java
class Solution {
public int trap(int[] nums) {
        int sum = 0;
    	// 外层循环数组的每一个元素
        for(int i = 1;i < nums.length;i++){
            int max_L = 0;
            int max_R = 0;
            // 找到当前格左边最高的柱子
            for(int l = 0; l < i;l++){
                if (nums[l] > max_L){
                    max_L = nums[l];
                }
            }
            // 找到当前格右边最高的柱子
            for(int r = i+1;r < nums.length;r++){
                if(nums[r] > max_R){
                    max_R = nums[r];
                }
            }
			
            // 找出左右两边较矮的柱子
            int min_LR = Math.min(max_R,max_L);
            // 如果当前格的柱子比两侧较矮的柱子低那么就可以留住水
            if(nums[i] < min_LR){
                // 能留住的水量为当前格柱子和两侧较矮柱子的差值
                sum += nums[i] - min_LR;
            }
        }
        return sum;
    }
}
```

#### 解法3（对解法二进行优化）

解法二在每次下一个元素的判断的时候都会对当前个两侧最高的柱子重新进行两次遍历分别求最高的。会浪费很多时间，我们通过一次循环将每一个格的左侧最高柱子和右侧最高柱子存在两个数组对应的下标当中。这样就只需要2次循环就能算出两侧的最高值，大大节省时间。

```java
class Solution {
    public int trap(int[] height) {
            int sum = 0;
    int[] max_left = new int[height.length];
    int[] max_right = new int[height.length];
    // 遍历所有元素计算当前格左侧最高格。
    for (int i = 1; i < height.length - 1; i++) {
        max_left[i] = Math.max(max_left[i - 1], height[i - 1]);
    }
    // 遍历所有元素计算当前格右侧最高格
    for (int i = height.length - 2; i >= 0; i--) {
        max_right[i] = Math.max(max_right[i + 1], height[i + 1]);
    }
    // 循环所有元素，计算能存多少水
    for (int i = 1; i < height.length - 1; i++) {
        // 去两侧较矮的墙
        int min = Math.min(max_left[i], max_right[i]);
        // 如果当前格小于两侧较矮的墙那么水的数量就增加
        if (min > height[i]) {
            sum = sum + (min - height[i]);
        }
    }
    return sum;
    }
}
```

