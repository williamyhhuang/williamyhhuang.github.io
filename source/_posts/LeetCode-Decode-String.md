title: '[LeetCode] Decode String'
author: ''
date: 2023-09-30 12:00:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
- Stack
categories:
- LeetCode

---

題目：[Decode String](https://leetcode.com/problems/decode-string)
難度：Medium

<!--more-->

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed `10^5`.

**Example 1:**
> **Input:** s = "3[a]2[bc]"
> **Output:** "aaabcbc"

**Example 2:**
> **Input:** s = "3[a2[c]]"
> **Output:** "accaccacc"

**Example 3:**
> **Input:** s = "2[abc]3[cd]ef"
> **Output:** "abcabccdcdcdef"

**Constraints:**

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
- `s` is guaranteed to be a valid input.
- All the integers in s are in the range `[1, 300]`.

---

先觀察 input 是否有規律，可以發現

1. 數字緊鄰著 `[`
2. `[` 要遇到 `]` 才是完整的一次迭代

下面是我的寫法，比較直覺式的一步一步做

1. 分兩個 stack 儲存，一個裝數字，一個裝字母及中括號
2. 如果當前 char 不是 `]`，代表還沒完成一次迭代，開始做素材的收集
3. 如果當前 char 為數字，則累加
4. 如果當前 char 為 `[`，代表數字的部分結束了，將數字放進 stackForNum 並歸零
5. 如果當前 char 為 `[` 或字母，則放進 stackForChar
6. 如果當前 char 為 `]`，代表素材收集完了，開始做字母的處理
7. 將存在 stackForChar、stackForNum 的 char 取出處理，處理完後再一一將 char 放回 stackForChar
8. 重複 2 ~ 7，直到遍歷整個字串

{% codeblock lang:cs %}
public class Solution {
    public string DecodeString(string s) {
        Stack<char> stackForChar = new Stack<char>();
        Stack<int> stackForNum = new Stack<int>();
        int index = 0;

        for(int i = 0; i < s.Length; i++)
        {
            if (s[i] != ']')
            {
                if (Char.IsDigit(s[i]))
                {
                    index = index * 10 + (s[i] - '0');
                    continue;
                }

                if (s[i] == '[')
                {
                    stackForNum.Push(index);
                    index = 0;
                }

                if (Char.IsDigit(s[i]) == false)
                {
                    stackForChar.Push(s[i]);
                    continue;
                }
            }

            // find all alphabets within square bracket
            List<char> list = new List<char>();
            while (stackForChar.Any() && stackForChar.Peek() != '[')
            {
                char c = stackForChar.Peek();
                list.Add(c);

                stackForChar.Pop();
            }
            list.Reverse();

            // remove '['
            stackForChar.Pop();

            // find the number
            int n = stackForNum.Pop();         

            for (int j = 0; j < n; j++)
            {
                for(int k = 0; k < list.Count(); k++)
                {
                    stackForChar.Push(list[k]);
                }
            }
        }

        return string.Join("",stackForChar.Reverse().ToArray());
    }
}
{% endcodeblock %}

---

以下是其他人的答案，寫的漂亮許多，不過要花些時間理解
觀察字串 s，可以將 char 分類成四種情境，再個別處理
我想應該很難一開始就能想到這個寫法吧，至少對我來說
對於題目的敏銳度及邏輯歸納還需要再練習練習

{% codeblock lang:cs %}
public class Solution {
    public string DecodeString(string s) {
        var strStack = new Stack<string>();
        var istack = new Stack<int>();
        int index = 0;
        string currentStr = "";

        foreach (char c in s)
        {
            if (char.IsDigit(c))
            {
                index = index * 10 + (c - '0');
            }
            else if (c == '[')
            {
                istack.Push(index);
                strStack.Push(currentStr);
                index = 0;
                currentStr = "";
            }
            else if (c == ']')
            {
                int repeatTimes = istack.Pop();
                string previousStr = strStack.Pop();
                var strings =  Enumerable.Repeat(currentStr, repeatTimes);
                currentStr = previousStr;
                foreach (String str in strings)
                {
                    currentStr +=str;
                }
            }
            else
            {
                currentStr += c;
            }
        }

        return currentStr;
    }
}
{% endcodeblock %}
