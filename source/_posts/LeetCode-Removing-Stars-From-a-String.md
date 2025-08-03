title: '[LeetCode] Removing Stars From a String'
author: ''
date: 2023-09-27 23:30:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)
難度：Medium

<!--more-->

You are given a string `s`, which contains stars `*`.

In one operation, you can:

Choose a star in `s`.
Remove the closest non-star character to its left, as well as remove the star itself.
Return the string after all stars have been removed.

**Note:**

- The input will be generated such that the operation is always possible.
- It can be shown that the resulting string will always be unique.

**Example 1:**
> **Input:** s = "leet**cod*e"
> **Output:** "lecoe"
> **Explanation:** Performing the removals from left to right:
> - The closest character to the 1st star is 't' in "leet**cod*e". s becomes "lee*cod*e".
> - The closest character to the 2nd star is 'e' in "lee*cod*e". s becomes "lecod*e".
> - The closest character to the 3rd star is 'd' in "lecod*e". s becomes "lecoe".
> There are no more stars, so we return "lecoe".

**Example 2:**
> **Input:** s = "erase*********"
> **Output:** ""
> **Explanation:** The entire string is removed, so we return an empty string.

**Constraints:**

- `1 <= s.length <= 10^5`
- `s` consists of lowercase English letters and stars `*`.
- The operation above can be performed on `s`.

---

用一個 stack 來裝字母，然後遍歷字串，如果遇到 *，則從 stack 中移除最後一個字母
也可以用 list 代替 stack 去裝字母，意思是一樣的

時間複雜度：O(N) `遍歷字串` + O(1) `加入、移除字母` = O(N)，N 為字串長度
空間複雜度：O(N)，N 為字串長度

{% codeblock lang:cs %}
public class Solution {
    public string RemoveStars(string s)
        {
            Stack<char> stack = new Stack<char>();

            foreach (var c in s)
            {
                if (item == '*')
                {
                    stack.Pop();
                }
                else
                {
                    stack.Push(c);
                }
            }

            return new string(stack.Reverse().ToArray());
        }
}
{% endcodeblock %}
