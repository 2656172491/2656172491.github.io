---
title: 勇闯力扣hot100—day01
date: 2025-10-13 21:39:03
tags: 算法
---

> 2025.10.13
奋斗一下，去年蓝桥杯拿了个省二，争取一下今年进国赛
# 勇闯力扣hot100—day01

## 1.两数之和
### 题目
给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。

你可以按任意顺序返回答案。

**示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

### 解题过程
首先，看到这个题目，最简单最暴力的做法就是将数组中的每两个元素相加，看看是否与目标值相同。
暴力解法为：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int a=0,b=0;
        for(int i = 0;i < nums.length-1;i++){
            for(int j = i+1;j < nums.length;j++){
                if(nums[i]+nums[j] == target && a == 0 && b == 0){
                    a = i;
                    b = j;
                    i = nums.length;
                    j = nums.length;
                } 
            }
        }
        int[] c = {a,b};
        return c;
    }
}
```

代码顺利的跑起来，并且成功通过了所有的案例，并且没有出现超时的情况。但是我们的执行用时为65ms，幺幺落后。

观察大佬解法，发现大佬都是通过哈希表来进行解决。基本思路为：数组2个元素之和其实可以理解为用目标值减去当前元素，然后在数组中寻找，看是否存在这个差值，没有就下一个。此时通过HashMap来解决，也是一个道理，如果在HashMap中的key值不存在这个差值，那么就将当前元素的值和下标插入到HashMap当中，直到查找到差值等于HashMap中的某个差值。那么这个方法能节省时间的最主要原因是因为在HashMap中查找key的时间复杂度为O(1);

代码如下：

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer,Integer> map = new HashMap<>();
        for(int i = 0;i < nums.length;i++){
            // 判断目标值减去数组对应元素后的差值是否存在哈希表当中。
            if(map.containsKey(target-nums[i])){
                return new int[] {map.get(target-nums[i]),i};
            }
            // 否则将该元素和他的下标存入到哈希表当中。
            map.put(nums[i],i);
        }
        return new int[] {1,2};
    }
}
```

成功将耗时减少到2ms。

## 2.字母异位词分组
### 题目
给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。
**示例 1:**

```
**输入:** strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

**输出:** [["bat"],["nat","tan"],["ate","eat","tea"]]
```

**解释：**

- 在 strs 中没有字符串可以通过重新排列来形成 `"bat"`。
- 字符串 `"nat"` 和 `"tan"` 是字母异位词，因为它们可以重新排列以形成彼此。
- 字符串 `"ate"` ，`"eat"` 和 `"tea"` 是字母异位词，因为它们可以重新排列以形成彼此。

**示例 2:**

```
**输入:** strs = [""]

**输出:** [[""]]
```
**示例 3:**
```
**输入:** strs = ["a"]

**输出:** [["a"]]
```
### 解题过程

在观察过题目和实例后，我们可以明白，这题是要将组成元素相同的单词进行归类。

此时我的第一个想法是：计算出每个单词每个字母的ASCII值然后累加起来存着，如果发现有相同ASCII值的就说明这两个单词组成是一样的。但是后来仔细想一想，3个不同值相加不见得只有一种情况，所有pass。

观摩大佬解法，第一种通过Stream流进行解题，看不懂pass;第二种方法，通过对组成单词的字母进行排序，那么排序后一样的单词就是字母异位词。

具体是实现过程为：创建一个`<String,List<String>>`的map用来存放所有的字符串，将传入的字符串转换为char数组，通过char数组的sort方法将数组元素重新进行排列。在排列完后，将排序后的char数组为作为HashMap的key来存放，将最初的字符串插入到value中的字符串列表。

具体代码：

```java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,List<String>> map = new HashMap<>();
        for(String s : strs){
            // 将字符串转化为char数组
            char[] sortedS = s.toCharArray();
            // 通过数组自带的排序算法进行排序
            Arrays.sort(sortedS);
            // 判断hashmap中是否存在一个String类型的sortedS的key，如果没有就创建一个键值对，其中String(sortedS)为key，_-> new ArrayList<>()为value；
            // _-> new ArrayList<>()是指创建一个ArrayList<>()，_的作用和()一致。
            // 最后将s加入到map的value中。
            map.computeIfAbsent(new String(sortedS), _-> new ArrayList<>()).add(s);
        }
        // values()方法返回map中存着的所有value。
        return new ArrayList<>(map.values());
    }
}
```

在这部分代码中`map.computeIfAbsent(new String(sortedS), _-> new ArrayList<>()).add(s);`相当于对map中的key进行判断，如果不存在排序后的字符串，那么创建一对键值对，随后将原字符串插入到value中。

## 3.最长连续序列
### 题目
给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。
示例 1：

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
示例 2：
```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```
示例 3：
```
输入：nums = [1,0,1,2]
输出：3
```
### 解题过程
看到题目，我首先想到的就是将数组进行排序，然后从左往右比较数组元素的差值，如果差值为1就累计，为0跳过，大于1就暂时记录为最大值，直到数组跑完。

注意：此题的`nums`数组有可能出入的是空数组。

大致过程：先将`nums`数组转为为`ArrayList`方便使用自带的sort方法进行排序,然后通过一层循环遍历所有元素。

暴力破解的代码为(屎山轻喷)：

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        if(nums.length == 0){
            return 0;
        }
        
        ArrayList<Integer> arr = new ArrayList<>();
        int number = 1;
        int maxNumber = 0;
        
        for(int num : nums){
            arr.add(num);
        }
        
        arr.sort(Comparator.naturalOrder());
        for(int i = 1;i < arr.size();i++){
            if(arr.get(i) - arr.get(i-1) == 1){
                number++;
            }else if(arr.get(i) - arr.get(i-1) == 0){
                continue;
            }else{
                maxNumber = number>maxNumber?number:maxNumber;
                number = 1 ;
            }
        }
        maxNumber = number>maxNumber?number:maxNumber;
        return maxNumber;
    }
}
```

代码顺利跑完，但是耗时48ms，明显不是最优解法。

观摩大佬思路：对于` nums `中的元素` x`，以` x `为起点，不断查找下一个数 *x*+1,*x*+2,⋯ 是否在 `nums` 中，并统计序列的长度。通过HashSet去除重复的元素，遍历set中的每个元素，寻找每个元素是否为起点，如果不是就直接跳过，如果是则累增判断是否连续，反之结束，判断当前的连续数是不是比之前的大，是则存着；接着开始新一轮判断，直到set中的每个元素都遍历完。但是我们可以发现，因为set中的元素不是重复的，那么当最长连续数超过set的大小的一半时就说明这个就是整个数组的最长连续值，可以直接结束循环。

```java
class Solution {
    public int longestConsecutive(int[] nums) {
        Set<Integer> hashset = new HashSet<>();
        // 因为在hashset中要求不允许有重复的值，所以在将nums中的元素存入到hashset中后，每个元素都是不重复的。
        for (int num : nums) {
            hashset.add(num); // 把 nums 转成哈希集合
        }

        int ans = 0;
        for (int x : hashset) { // 遍历哈希集合
            // contains(value) 查找hashset中是否存在value这个元素
            if (hashset.contains(x - 1)) { // 如果 x 不是序列的起点，直接跳过
                continue;
            }
            // x 是序列的起点
            int y = x + 1;
            // 循环判断y+1是否存在集合中。存在就将y累计
            while (hashset.contains(y)) { // 不断查找下一个数是否在哈希集合中
                y++;
            }
            // 循环结束后，y-1 是最后一个在哈希集合中的数
            // 将当前的连续数与记录中的最大连续数进行比较。
            ans = Math.max(ans, y - x); // 从 x 到 y-1 一共 y-x 个数
            // 因为都是不重复的数，那么当最长连续数超过set的一半时，直接可以结束循环。
            if(ans*2 >= hashset.size()){
                break;
            }
        }
        return ans;
    }
}
```

