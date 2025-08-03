title: '[LeetCode] CMaximum Number of Vowels in a Substring of Given Length'
author: ''
date: 2023-09-21 23:00:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
- Sliding Window

categories:

- LeetCode

---

題目：[Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length)
難度：Medium

<!--more-->

Given a string `s` and an integer `k`, return the maximum number of vowel letters in any substring of `s` with length `k`.

Vowel letters in English are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

Example 1:
> **Input:** s = "abciiidef", k = 3
> **Output:** 3
> **Explanation:** The substring "iii" contains 3 vowel letters.

Example 2:
> **Input:** s = "aeiou", k = 2
> **Output:** 2
> **Explanation:** Any substring of length 2 contains 2 vowels.

Example 3:
> **Input:** s = "leetcode", k = 3
> **Output:** 2
> **Explanation:** "lee", "eet" and "ode" contain 2 vowels.

Constraints:

- `1 <= s.length <= 10^5`
- `s` consists of lowercase English letters.
- `1 <= k <= s.length`

---

解題技巧為 Sliding Window，跟 [Maximum Average Subarray I](/2023/09/20/LeetCode-Maximum-Average-Subarray-I) 一樣，差別只在於這題要做母音的判斷

{% codeblock lang:cs %}
public class Solution {
    public int MaxVowels(string s, int k) {
        int currentCount = 0;
        int maxCount;

        for (int i = 0; i < k; i ++)
        {
            if (IsVowel(s[i]))
            {
                currentCount++;
            }
        }

        maxCount = currentCount;

        for (int j = 0; j+k < s.Length; j++)
        {
            if (IsVowel(s[j]))
            {
                currentCount--;
            }

            if (IsVowel(s[j+k]))
            {
                currentCount++;
            }

            maxCount = Math.Max(maxCount,currentCount);
        }

        return maxCount;
    }

    /// <Summary>
    /// 該字母是否為母音
    /// </Summary>    
    public bool IsVowel(char c)
    {
        if (c == 'a' ||c == 'e' ||c == 'i' ||c == 'o' ||c == 'u')
        {
            return true;
        }

        return false;
    }
}
{% endcodeblock %}
