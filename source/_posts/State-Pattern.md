title: '[Design Pattern] STATE 模式'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2023-02-28 20:00:00
---

我們就以工程師的一天：Eat、Coding、Sleep 三種狀態，來示範 STATE 模式
STATE 模式是由以下三個部分組成：
<ol>
<li>Context：用來控制所有的狀態，其會是與客戶端的接口，客戶端只會與 Context 互動</li>
<li>State：定義各個狀態的抽象方法，ConcreteState 會繼承 State 並實作其方法</li>
<li>ConcreteState：Eat、Coding、Sleep，三種狀態的實作</li>
</ol>

<img src="https://imgur.com/amjGppM.png">

<!--more-->

## 定義 Context

### 用列舉定義狀態

因我們有三種 State，所以我用列舉定義出 State，待會 Context 裡的 GetState 方法會用到

{% codeblock lang:cs %}
namespace DemoCode.DesignPattern.State
{
    /// <summary>
    /// 狀態定義
    /// </summary>
    public enum StateEnum
    {
        Eat,
        Sleep,
        Coding
    }
}
{% endcodeblock %}

### 定義 Context

定義客戶端的接口 Context，其為所有狀態的控制中心，客戶端只能透過它執行各狀態的行為

{% codeblock lang:cs %}
using System;
namespace DemoCode.DesignPattern.State
{
    /// <summary>
    /// 工程師
    /// </summary>
    public class SoftwareEngineer
    {
        private State _itsState;
        private Eat _eatState;
        private Sleep _sleepState;
        private Coding _codeingState;

        /// <summary>
        /// 工程師
        /// </summary>
        public SoftwareEngineer()
        {
            this._eatState = new Eat();
            this._sleepState = new Sleep();
            this._codeingState = new Coding();

            // 預設狀態從吃飯開始
            this._itsState = this._eatState;
        }

        /// <summary>
        /// 取得當前狀態
        /// </summary>
        /// <returns></returns>
        public string GetCurrentStateName()
        {
            Console.WriteLine($"現在狀態：{this._itsState.StateName()}");

            return this._itsState.StateName();
        }

        /// <summary>
        /// 取得狀態
        /// </summary>
        /// <param name="state"></param>
        /// <returns></returns>
        public State GetState(StateEnum state)
        {
            switch (state)
            {
                case StateEnum.Eat:
                    return this._eatState;

                case StateEnum.Sleep:
                    return this._sleepState;

                case StateEnum.Coding:
                    return this._codeingState;

                default:
                    throw new ArgumentException();
            }
        }

        /// <summary>
        /// 設定狀態
        /// </summary>
        /// <param name="state"></param>
        public void SetState(State state)
        {
            this._itsState = state;
        }

        /// <summary>
        /// 行為
        /// </summary>
        public void Action()
        {
            this._itsState.Action(this);
        }
    }
}
{% endcodeblock %}

## 定義 State

定義基底類別，各狀態類別會繼承此類別，並實作其方法

{% codeblock lang:cs %}
namespace DemoCode.DesignPattern.State
{
    public abstract class State
    {
        /// <summary>
        /// 狀態名稱
        /// </summary>
        /// <returns></returns>
        public abstract string StateName();

        /// <summary>
        /// 行為
        /// </summary>
        public virtual void Action(SoftwareEngineer softwareEngineer)
        {

        }
    }
}
{% endcodeblock %}

## 實作 ConcreteState

這邊放上 Eat.cs 的程式碼，其他兩種狀態差不多
每個狀態類別實作基底類別宣告的方法，並自行決定完成動作後，下個狀態為何

{% codeblock lang:cs %}
using System;
namespace DemoCode.DesignPattern.State
{
    /// <summary>
    /// 吃飯
    /// </summary>
    public class Eat : State
    {
        /// <summary>
        /// 當前狀態名稱
        /// </summary>
        /// <returns>當前狀態名稱</returns>
        public override string StateName()
        {
            return "Eat";
        }

        /// <summary>
        /// 開始動作
        /// </summary>
        /// <param name="softwareEngineer"></param>
        public override void Action(SoftwareEngineer softwareEngineer)
        {
            Console.WriteLine("該吃飯了");

            // 更改狀態
            Console.WriteLine("吃完飯該寫程式了");
            var nextState = softwareEngineer.GetState(StateEnum.Coding);
            softwareEngineer.SetState(nextState);
        }
    }
}
{% endcodeblock %}

## 結果

{% codeblock lang:cs %}
namespace DemoCode
{
    class Program
    {
        static void Main(string[] args)
        {
            var engineer = new SoftwareEngineer();
            Console.WriteLine("Step01:");
            engineer.GetCurrentStateName();
            engineer.Action();

            Console.WriteLine("Step02:");
            engineer.GetCurrentStateName();
            engineer.Action();

            Console.WriteLine("Step03:");
            engineer.GetCurrentStateName();
            engineer.Action();

            Console.WriteLine("Step04:");
            engineer.GetCurrentStateName();
            engineer.Action();
        }
    }
}
{% endcodeblock %}

```
Step01:
現在狀態：Eat
該吃飯了
吃完飯該寫程式了
Step02:
現在狀態：Coding
該寫程式了
寫完程式該睡覺了
Step03:
現在狀態：Sleep
該睡覺了
睡完覺該吃飯了
Step04:
現在狀態：Eat
該吃飯了
吃完飯該寫程式了
```

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

## 結論

STATE 模式的好處是將各 State 的邏輯分離，在管理複雜的系統行為時很有幫助
而 STATE 模式與 STRATEGY 模式看起來很相近，但差別在於，前者知道各 State 的存在，且能透過 Context 進行修改，後者則相反。如下圖可看出，Context 並不知道是哪個 Contrete Strategy 實作 method 的，而 State 會知道

<img src="https://imgur.com/2CgMVFT.png">
Strategy 模式

## 參考

STATE 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[wiki - state pattern](https://en.wikipedia.org/wiki/State_pattern)
[狀態模式-State Pattern](https://dotblogs.com.tw/bda605/2020/03/26/001403)
[狀態模式 | State Pattern](https://ianjustin39.github.io/ianlife/design-pattern/state-pattern/)
[[Design Pattern] State 狀態模式](https://ithelp.ithome.com.tw/articles/10223418)
