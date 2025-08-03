title: '[LeetCode] Longest Palindromic Substring'
author: ''
date: 2023-02-28 21:00:00
tags:
  - C#
  - LeetCode
  - Medium
  - Data Structure
  - Algorithm
categories:
  - LeetCode
---

題目：[Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring)
難度：Medium

暴力解效能超差

<img src="https://imgur.com/luirXRb.png">

<!--more-->

## 暴力解

因為要遍歷找尋迴文，例字串為 `abc`，則會找出所有組合： a、ab、abc、bc、c，其時間複雜度為 O(N^2)
又因為每個組合要確認其是否為迴文，即下面的方法 `CheckIsPalindrome`，故總共時間複雜度為 O(N^3)

{% codeblock lang:cs %}
public class Solution {
    public string LongestPalindrome(string s) {
        string result = string.Empty;

        for (int i = 0; i < s.Length; i++)
        {
            string currentString = string.Empty;

            for (int j = i; j < s.Length; j++)
            {
                currentString = currentString += s[j].ToString();

                bool isPalindrome = this.CheckIsPalindrome(currentString);
                if (isPalindrome == false)
                {
                    continue;
                }

                result = currentString.Length > result.Length ?
                         currentString : result;
            }     
        }

        return result;
    }

    private bool CheckIsPalindrome(string s)
    {
        for (int i = 0, j = s.Length - 1; j > i; i++, j--)
        {
            if (s[i] != s[j])
            {
                return false;
            }
        }
        return true;
    }
}
{% endcodeblock %}

## 其他解

從字串的中間當作中心，往左右邊擴散尋找迴文，直到找出迴文或是遍歷整個字串
要注意的是奇數、偶數個字的字串都要考量進去
最佳時間複雜度：O(N)，即字串中所有字母皆相同
最差時間複雜度：O(N^2)，即字串中所有字母皆不同

{% codeblock lang:cs %}
public class Solution {
    public string LongestPalindrome(string s) {
        int maxLength = 0;
        int startIndex = 0;

        for (int i = 0; i < s.Length; i++)
        {
            int odd = this.GetLength(s, i, i);
            int even = this.GetLength(s, i, i+1);
            
            int curr = Math.Max(odd, even);

            if (curr > maxLength)
            {
                maxLength = curr;
                startIndex = i - (curr-1)/2;
            }
        }

        return s.Substring(startIndex, maxLength);
    }

    private int GetLength(string s, int leftIndex, int rightIndex)
    {
        int stringLength = s.Length;

        while(leftIndex >= 0 && 
              rightIndex < stringLength &&
              s[leftIndex] == s[rightIndex])
        {
            leftIndex -= 1;
            rightIndex += 1;
        }

        // 因是要回傳長度，要包含頭尾，故需要 +1
        // 因斷開迴圈時，左右兩邊的指標會落在非迴文的位置，故需要 -2
        return rightIndex - leftIndex + 1 - 2;
    }
}
{% endcodeblock %}

## 其他解：Dynamic Programming

待補

## 其他解：Manacher's Algorithm

待補

## 參考

[YouTube - 花花酱 LeetCode 5. Longest Palindromic Substring - 刷题找工作 EP292](https://www.youtube.com/watch?v=g3R-pjUNa3k&ab_channel=HuaHua)
[[LeetCode] 5. Longest Palindromic Substring 最长回文子串](https://www.cnblogs.com/grandyang/p/4464476.html)
