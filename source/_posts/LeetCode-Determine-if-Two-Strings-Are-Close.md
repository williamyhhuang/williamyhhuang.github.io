title: '[LeetCode] Determine if Two Strings Are Close'
author: ''
date: 2023-09-26 21:30:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
categories:
- LeetCode

---

題目：[Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close)
難度：Medium

<!--more-->

Two strings are considered close if you can attain one from the other using the following operations:

- Operation 1: Swap any two existing characters.
  - For example, `abcde -> aecdb`
Operation 2: Transform every occurrence of one existing character into another existing character, and do the same with the other character.
  - For example, `aacabb -> bbcbaa` (all `a`'s turn into `b`'s, and all `b`'s turn into `a`'s)

You can use the operations on either string as many times as necessary.

Given two strings, `word1` and `word2`, return `true` if `word1` and `word2` are close, and `false` otherwise.

**Example 1:**
> **Input:** word1 = "abc", word2 = "bca"
> **Output:** true
> **Explanation:** You can attain word2 from word1 in 2 operations.
> Apply Operation 1: "abc" -> "acb"
> Apply Operation 1: "acb" -> "bca"

**Example 2:**
> **Input:** word1 = "a", word2 = "aa"
> **Output:** false
> **Explanation:** It is impossible to attain word2 from word1, or vice versa, in any number of operations.

**Example 3:**
> **Input:** word1 = "cabbba", word2 = "abbccc"
> **Output:** true
> **Explanation:** You can attain word2 from word1 in 3 operations.
> Apply Operation 1: "cabbba" -> "caabbb"
> Apply Operation 2: "caabbb" -> "baaccc"
> Apply Operation 2: "baaccc" -> "abbccc"

**Constraints:**

- `1 <= word1.length, word2.length <= 10^5`
- `word1` and `word2` contain only lowercase English letters.

---

這題的重點不在 Operation 1、2，這題在問 word1 是否能經由 Operation 1、2 變成 word2，如果題目是問 word1 要操作 Operation 1、2 幾次才能變成 word2，這才會跟 Operation 有關

所以現在來觀察 word1 轉換成 word2 是否有什麼相似之處，由範例3：word1 = "cabbba", word2 = "abbccc"，可以觀察到：

1. word1 word2 的字串長度都相同
2. word1 word2 都只出現三個相同的字母：a、b、c
3. 各字母出現的次數都一樣
    - word1: c 1 次，a 2 次，b 3 次
    - word2: a 1 次，b 2 次，c 3 次

所以要知道 word1 與 word2 是否為 close，就要滿足三個條件：

1. 字串長度相同
2. 字母相同
3. 字母出現的次數相同，以範例3為例，word1 的 a 出現一次， word2 的 c 出現一次

時間複雜度：O(N) `word1、word2 統計各字母出現次數` + O(1) `確認字母是否都有出現在 word1 & word2` + O(NlogN) `排序` + O(N) `確認字母出現的次數都一樣` = O(NlogN)
空間複雜度：使用 arr1、arr2 統計各字母出現的次數，因字母為 26 個，矩陣長度已固定，故為 O(1)

{% codeblock lang:cs %}
public class Solution {
    public bool CloseStrings(string word1, string word2) {
        // 字串長度是否相同
        if (word1.Length != word2.Length)
        {
            return false;
        }

        int[] arr1 = new int[26];
        int[] arr2 = new int[26];

        foreach (char c in word1)
        {
            arr1[c-'a']++;
        }

        foreach (char c in word2)
        {
            arr2[c-'a']++;
        }

        // a ~ z 26 個字母逐一確認
        // 確認字母是否都有出現在 word1 & word2
        for (int i = 0; i < 26; i++)
        {
            if ((arr1[i] > 0 && arr2[i] == 0) || 
                (arr1[i] == 0 && arr2[i] > 0))
            {
                return false;
            }
        }

        Array.Sort(arr1);
        Array.Sort(arr2);

        // 確認字母出現的次數都一樣
        for (int i = 0; i < 26; i++)
        {
            if (arr1[i] != arr2[i])
            {
                return false;
            }
        }

        return true;                   
    }
}
{% endcodeblock %}
