title: '[LeetCode] Asteroid Collision'
author: ''
date: 2023-09-28 23:30:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
- stack
categories:
- LeetCode

---

題目：[Asteroid Collision](https://leetcode.com/problems/asteroid-collision)
難度：Medium

<!--more-->

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

**Example 1:**
> **Input:** asteroids = [5,10,-5]
> **Output:** [5,10]
> **Explanation:** The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

**Example 2:**
> **Input:** asteroids = [8,-8]
> **Output:** []
> **Explanation:** The 8 and -8 collide exploding each other.

**Example 3:**
> **Input:** asteroids = [10,2,-5]
> **Output:** [10]
> **Explanation:** The 2 and -5 collide resulting in -5. The 10 and -5 collide resulting in 10.

**Constraints:**

- `2 <= asteroids.length <= 10^4`
- `-1000 <= asteroids[i] <= 1000`
- `asteroids[i] != 0`

---

這是我寫的版本，把所有可能都列出來並一個一個處理

{% codeblock lang:cs %}
public class Solution {
    public int[] AsteroidCollision(int[] asteroids) {
        Stack<int> stack = new Stack<int>();

        for (int i = 0; i < asteroids.Length; i++)
        {
            // 如果 stack 中有行星，則處理stack 中最上面的行星 及 當前的行星
            if (stack.Count > 0)
            {
                int lastNum = stack.Peek();
                int currentNum = asteroids[i];

                Process(stack, lastNum, currentNum);
            }
            else
            {
                stack.Push(asteroids[i]);
            }
        }

        return stack.Reverse().ToArray();        
    }

    private void Process(Stack<int> stack, int lastNum, int currentNum)
    {
        // 如果同方向 或 不會相撞，則加入該行星後繼續
        if ((lastNum * currentNum > 0) || (lastNum < 0 && currentNum > 0))
        {
            stack.Push(currentNum);
        }
        // 如果會相撞
        else
        {
            // 如果 stack 中最上面的行星 大於 當前的行星
            // 則當前的行星會爆炸，直接返回
            if (Math.Abs(lastNum) > Math.Abs(currentNum))
            {
                return;
            }
            // 如果 stack 中最上面的行星 等於 當前的行星
            // 則從 stack 中移除最上面的行星
            else if (Math.Abs(lastNum) == Math.Abs(currentNum))
            {
                stack.Pop();
            }
            // 如果 stack 中最上面的行星 小於 當前的行星
            else
            {
                // 則從 stack 中移除最上面的行星
                stack.Pop();

                // 如果 stack 裡還有行星，則一直移除，直到遇到 同方向 或 比當前行星 的行星
                if (stack.Count > 0)
                {
                    lastNum = stack.Peek();

                    Process(stack, lastNum, currentNum);
                }
                // 如果 stack 中沒有行星了，則加入該行星後繼續
                else
                {
                    stack.Push(currentNum);
                }
            }
        }
    }
}
{% endcodeblock %}

---

參考其他答案，這個寫法簡潔多了

時間複雜度：O(N) `遍歷整個 asteroids` + O(1) `對 stack 的操作` = O(N)，N 為矩陣長度
空間複雜度：用 stack 儲存結果，為 O(N)，N 為矩陣長度

{% codeblock lang:cs %}
public class Solution {
    public int[] AsteroidCollision(int[] asteroids) {
        var stack = new Stack<int>();

        foreach (var a in asteroids)
        {
            // 如果行星往右，則加入 stack
            if (a > 0)
            {
                stack.Push(a);
            }
            else
            {
                // 如果 stack 中有行星 且 stack 中最上面的行星往右 且 當前行星大於 stack 中最上面的行星
                // 則從 stack 中移除該行星
                while (stack.Count != 0 && stack.Peek() > 0 && -a > stack.Peek())
                {
                    stack.Pop();
                }

                // 如果 stack 中沒有行星 或 stack 中最上面的行星往左
                // 則加入 stack
                if (stack.Count == 0 || stack.Peek() < 0)
                {
                    stack.Push(a);
                }
                // stack 中最上面的行星 等於 當前行星
                // 則移除 stack 中最上面的行星
                else if (stack.Peek() == -a)
                {
                    stack.Pop();
                }
            }
        }

        return stack.Reverse().ToArray();
    }
}
{% endcodeblock %}
