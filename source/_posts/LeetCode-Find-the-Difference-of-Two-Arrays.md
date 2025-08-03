title: '[LeetCode] Find the Difference of Two Arrays'
author: ''
date: 2023-09-26 09:30:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Find the Difference of Two Arrays](https://leetcode.com/problems/find-the-difference-of-two-arrays)
難度：Easy

<!--more-->

Given two 0-indexed integer arrays `nums1` and `nums2`, return a list `answer` of size `2` where:

- `answer[0]`is a list of all distinct integers in `nums1` which are not present in nums2.
- `answer[1]` is a list of all distinct integers in `nums2` which are not present in nums1.

**Note that the integers in the lists may be returned in any order.**

**Example 1:**
> **Input:** nums1 = [1,2,3], nums2 = [2,4,6]
> **Output:** [[1,3],[4,6]]
> **Explanation:**
> For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].
> For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6].

**Example 2:**
> **Input:** nums1 = [1,2,3,3], nums2 = [1,1,2,2]
> **Output:** [[3],[]]
> **Explanation:**
> For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3].
> Every integer in nums2 is present in nums1. Therefore, answer[1] = [].

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `-1000 <= nums1[i], nums2[i] <= 1000`

---

題目有說回傳的 list 可以為任何排序，故可以使用 HashSet 來解題

{% codeblock lang:cs %}
public class Solution {
    public IList<IList<int>> FindDifference(int[] nums1, int[] nums2) {
        ISet<int> SetNums1 = new HashSet<int>(nums1);
        ISet<int> SetNums2 = new HashSet<int>(nums2);

        SetNums1.ExceptWith(nums2);
        SetNums2.ExceptWith(nums1);

        IList<int> list1 = SetNums1.ToList();
        IList<int> list2 = SetNums2.ToList();

        return new List<IList<int>>{list1, list2};
    }
}
{% endcodeblock %}
