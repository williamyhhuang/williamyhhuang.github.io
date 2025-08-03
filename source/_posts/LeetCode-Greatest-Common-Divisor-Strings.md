title: '[LeetCode] Greatest Common Divisor of Strings'
author: ''
date: 2023-07-27 09:00:00
tags:

- C#
- LeetCode
- Easy
- Data Structure
- Algorithm

categories:

- LeetCode

---

題目：[Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
難度：Easy

<!--more-->

看題目知道是要求最大公因數，原本的想法是：1.將兩個字串各別轉為 ASCII 後求最大公因數，2.再由最大公因數反求得字串，但在第二步時卡關
最後在看解答時才恍然大悟，如果兩個字串有最大公因數字串的話，則 `str1 + str2 == str2 + str1`，所以只要用兩字串長度取最大公因數即可

{% codeblock lang:cs %}
public class Solution {
    public string GcdOfStrings(string str1, string str2) {
        var result = (str1 + str2) == (str2 + str1) ?
                     str1.Substring(0, gcd(str1.Length, str2.Length)) : string.Empty;

        return result;
    }

    public static int gcd(int m, int n) {
        return n == 0 ? m : gcd(n, m % n);
    }
}
{% endcodeblock %}
