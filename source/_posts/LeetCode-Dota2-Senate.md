title: '[LeetCode] 649. Dota2 Senate'
author: ''
date: 2023-10-11 09:30:00
tags:

- C#
- LeetCode
- Medium
- Data Structure
- Algorithm
- Queue
categories:
- LeetCode

---

題目：[649. Dota2 Senate](https://leetcode.com/problems/dota2-senate)
難度：Medium

<!--more-->

In the world of Dota2, there are two parties: the Radiant and the Dire.

The Dota2 senate consists of senators coming from two parties. Now the Senate wants to decide on a change in the Dota2 game. The voting for this change is a round-based procedure. In each round, each senator can exercise one of the two rights:

- Ban one senator's right: A senator can make another senator lose all his rights in this and all the following rounds.
- Announce the victory: If this senator found the senators who still have rights to vote are all from the same party, he can announce the victory and decide on the change in the game.

Given a string `senate` representing each senator's party belonging. The character `'R'` and `'D'` represent the Radiant party and the Dire party. Then if there are `n` senators, the size of the given string will be `n`.

The round-based procedure starts from the first senator to the last senator in the given order. This procedure will last until the end of voting. All the senators who have lost their rights will be skipped during the procedure.

Suppose every senator is smart enough and will play the best strategy for his own party. Predict which party will finally announce the victory and change the Dota2 game. The output should be `"Radiant"` or `"Dire"`.

**Example 1:**
> **Input:** senate = "RD"
> **Output:** "Radiant"
> **Explanation:**
> The first senator comes from Radiant and he can just ban the next senator's right in round 1.
> And the second senator can't exercise any rights anymore since his right has been banned.
> And in round 2, the first senator can just announce the victory since he is the only guy in the senate who can vote.

**Example 2:**
> **Input:** senate = "RDD"
> **Output:** "Dire"
> **Explanation:**
> The first senator comes from Radiant and he can just ban the next senator's right in round 1.
> And the second senator can't exercise any rights anymore since his right has been banned.
> And the third senator comes from Dire and he can ban the first senator's right in round 1.
> And in round 2, the third senator can just announce the victory since he is the only guy in the senate who can vote.

**Constraints:**

`n == senate.length`
`1 <= n <= 10^4`
`senate[i]` is either `'R'` or `'D'`.

---

題目落落長，主要在說政黨可以 ban 掉對方政黨，最後剩下的政黨即為贏的政黨

觀察 Example 2：

1. R 的下一個為 D，故其 ban 掉 D 的投票權
2. 第一個 D 被 R ban 掉，什麼都不能做
3. 第二個 D 看到它的前面有 R，故把 R ban 掉
4. 最後只剩下第二個 D，故 D 獲勝

可以觀察到以下幾點：

- 政黨可以 ban 對方政黨的投票權力
- 投票方式為 round-based，即是一個循環，直到只剩一個政黨

---

參考[這個解答](https://leetcode.com/problems/dota2-senate/solutions/3353103/c-single-queue-single-ban-scale/?envType=study-plan-v2&envId=leetcode-75)，可以用 queue 來解這題

1. 將兩政黨放進 queue
2. 計算兩政黨數量 count
3. 定義一個指標 scale，用其表示誰有 ban 掉對方的權力
4. 每次取出一個政黨，判斷其有沒有 ban 掉對方的權力，如果有，則扣掉對方的 quota，並 insert 自己回 queue
5. 剩下的 count > 0 的即為勝利的政黨

以 Example 2 為例：

1. 將政黨放進 queue，順序為 RDD
2. 計算兩政黨數量 countR = 1、countD = 2
3. 從 queue 取出政黨為 R，因 scale = 0，故其有 ban 對方的權力，countD - 1 = 1，scale + 1 = 1
4. R insert 回 queue，現在 queue 的順序為 DDR
5. 從 queue 取出政黨為 D，scale = 1，所以 D 被 ban，R 政黨用掉一次 ban 的權力，故 scale - 1 = 0，現在 queue 的順序為 DR
6. 從 queue 取出政黨為 D，scale = 0，故其有 ban 對方的權力，countR - 1 = 0，scale - 1 = -1
7. D insert 回 queue，現在 queue 的順序為 RD
8. 從 queue 取出政黨為 R，scale = -1，所以 R 被 ban，D 政黨用掉一次 ban 的權力，故 scale + 1 = 0，現在 queue 的順序為 D
9. 最後 countR = 0、countD = 1，勝利的政黨為 D

{% codeblock lang:cs %}
public class Solution {
    public string PredictPartyVictory(string senate)
    {
        Queue<char> senators = new Queue<char>(senate);
        int countR = senate.Count(x => x == 'R');
        int countD = senate.Length - countR;

        // [Positive = Radiant Favor, Negative = Dire Favor]
        int scale = 0; 

        while (countR > 0 && countD > 0) 
        {
            char senator = senators.Dequeue();

            if (senator == 'R') 
            {
                if (scale >= 0) 
                {
                    countD--;
                    senators.Enqueue(senator);
                }

                scale++;
            }
            else 
            {
                if (scale <= 0) 
                {
                    countR--;
                    senators.Enqueue(senator);
                }

                scale--;
            }
        }

        return countR == 0 ? "Dire" : "Radiant";
    }
}
{% endcodeblock %}
