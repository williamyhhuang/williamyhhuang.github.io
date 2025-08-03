title: '[LeetCode] Equal Row and Column Pairs'
author: ''
date: 2023-09-27 00:30:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs/)
難度：Medium

<!--more-->

Given a 0-indexed `n x n` integer matrix `grid`, return the number of pairs `(ri, cj)` such that row `ri` and column `cj` are equal.

A row and column pair is considered equal if they contain the same elements in the same order (i.e., an equal array).

**Example 1:**

<img src="https://imgur.com/jAGcx1A.png">

> **Input:** grid = [[3,2,1],[1,7,6],[2,7,7]]
> **Output:** 1
> **Explanation:** There is 1 equal row and column pair:
>
> - (Row 2, Column 1): [2,7,7]

**Example 2:**

<img src="https://imgur.com/gSBSdKI.png">

> **Input:** grid = [[3,1,2,2],[1,4,4,5],[2,4,2,2],[2,4,2,2]]
> **Output:** 3
> **Explanation:** There are 3 equal row and column pairs:
>
> - (Row 0, Column 0): [3,1,2,2]
> - (Row 2, Column 2): [2,4,2,2]
> - (Row 3, Column 2): [2,4,2,2]

**Constraints:**

- `n == grid.length == grid[i].length`
- `1 <= n <= 200`
- `1 <= grid[i][j] <= 10^5`

---

將各 row、column 的數字用符號串接起來並轉換成字串，經比對確認是否相同

時間複雜度：因要遍歷整個 grid，故為 O(N^2)
空間複雜度：因要儲存所有 row、column 的值，故為 O(N^2)

{% codeblock lang:cs %}
public class Solution {
    public int EqualPairs(int[][] grid) {
        int n = grid.Length;
        string SEPERATOR = "_";
        IDictionary<int, string> rowDict = new Dictionary<int, string>();
        IDictionary<int, string> colDict = new Dictionary<int, string>();
        int count = 0;

        // 找出所有 row 的字串
        for (int i = 0; i < n; i++)
        {
            string s = string.Join(SEPERATOR, grid[i]);
            rowDict[i] = s;
        }

        // 找出所有 column 的字串
        for (int i = 0; i < n; i++)
        {
            List<int> arr = new List<int>();

            for (int j = 0; j < n; j++)
            {
                arr.Add(grid[j][i]);
            }

            string s = string.Join(SEPERATOR, arr);

            colDict.Add(i, s);
        }

        // 遍歷整個 grid，比對 row 跟 column 的值
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                if (rowDict[i] == colDict[j])
                {
                    count++;
                }
            }
        }

        return count;
    }
}
{% endcodeblock %}

---

有看到網友的[解答](https://leetcode.com/problems/equal-row-and-column-pairs/solutions/3633584/solution-using-linq-dictionary/?envType=study-plan-v2&envId=leetcode-75)，用 LINQ 寫起來真漂亮
這邊有個技巧在於，求 hashes 時轉成 dict，其 value 用 `Count()`
以 [[13,13],[13,13]] 為例，它存成 dict 時為（"13|13", 2），所以在組 colString 時比對，等於 grid[0] 跟 colString 比一次、grid[1] 跟 colString 比一次

{% codeblock lang:cs %}
public class Solution
{
    private const string SEPARATOR = "|";

    public int EqualPairs(int[][] grid)
    {
        Dictionary<string, int> hashes = grid
            .Select(row => string.Join(SEPARATOR, row))
            .GroupBy(hash => hash)
            .ToDictionary(h => h.Key, h => h.Count());
        
        int countPairs = 0;
        for (int colIX = 0; colIX < grid[0].Length; colIX++)
        {
            string strForHash = string.Join(SEPARATOR, grid.Select(row => row[colIX]));
            countPairs += hashes.GetValueOrDefault(strForHash);
        }
        
        return countPairs;
    }
}
{% endcodeblock %}
