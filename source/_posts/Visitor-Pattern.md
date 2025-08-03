title: '[Design Pattern] Visitor 模式 (一)'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2022-02-02 22:30:00
---
有時會需要對既有的類別新增方法，但該方法又會因為不同的類別有些微的差異，最土炮的方法是在各個類別中實作該方法，但如果下次又有類似的需求，又要再次修改各個類別，這樣無法遵守開放封閉原則。

這時可以使用訪問者模式，在不改變既有類別的情況下，將欲新增的方法收攏至訪問者類別中，【無瑕的程式碼】書中介紹的訪問者種類有以下四種，我會各自發一篇文做介紹：

<ol>
    <li> Visitor 模式 </li>
    <li> Acyclic Visitor 模式 </li>
    <li> Decorator 模式 </li>
    <li> Extenstion Object 模式 </li>
</ol>

<!--more-->

---

## Visitor 模式

現在有兩種交通工具；機車和巴士，假設乘客只在意三個項目：馬力、限乘人數、有沒有冷氣．將這三個屬性定義在介面，另外還需定義一個方法 `Accept`，使機車和巴士類別能夠讓 Visitor 訪問。

{% codeblock lang:cs %}
/*IVisitor.cs*/

namespace DemoCode.DesignPattern.Visitor.OriginVisitor
{
    /// <summary>
    /// IVisitor
    /// </summary>
    public interface IVisitor
    {
        /// <summary>
        /// Visit for Motor
        /// </summary>
        /// <param name="motor"></param>
        void Visit(Motor motor);

        /// <summary>
        /// Visit for Bus
        /// </summary>
        /// <param name="bus"></param>
        void Visit(Bus bus);
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*ITransportation.cs*/

namespace DemoCode.DesignPattern.Visitor.OriginVisitor
{
    /// <summary>
    /// 交通工具
    /// </summary>
    public interface ITransportation
    {
        /// <summary>
        /// 馬力
        /// </summary>
        decimal Power { get; set; }

        /// <summary>
        /// 限乘人數
        /// </summary>
        int NumberOfPassenger { get; set; }

        /// <summary>
        /// 是否有冷氣
        /// </summary>
        bool HasAirConditioner { get; set; }

        /// <summary>
        /// Accept
        /// </summary>
        /// <param name="visitor"></param>
        void Accept(IVisitor visitor);
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*Motor.cs*/

namespace DemoCode.DesignPattern.Visitor.OriginVisitor
{
    /// <summary>
    /// 機車
    /// </summary>
    public class Motor : ITransportation
    {
        /// <summary>
        /// 馬力
        /// </summary>
        public decimal Power { get; set; }

        /// <summary>
        /// 限乘人數
        /// </summary>
        public int NumberOfPassenger { get; set; }

        /// <summary>
        /// 是否有冷氣
        /// </summary>
        public bool HasAirConditioner { get; set; }

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="power"></param>
        /// <param name="numberOfPassenger"></param>
        /// <param name="hasAirConditioner"></param>
        public Motor(decimal power, int numberOfPassenger, bool hasAirConditioner)
        {
            this.Power = power;
            this.NumberOfPassenger = numberOfPassenger;
            this.HasAirConditioner = hasAirConditioner;
        }

        /// <summary>
        /// Accept
        /// </summary>
        /// <param name="visitor"></param>
        public void Accept(IVisitor visitor)
        {
            visitor.Visit(this);
        }
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*Bus.cs*/

namespace DemoCode.DesignPattern.Visitor.OriginVisitor
{
    /// <summary>
    /// 巴士
    /// </summary>
    public class Bus : ITransportation
    {
        /// <summary>
        /// 馬力
        /// </summary>
        public decimal Power { get; set; }

        /// <summary>
        /// 限乘人數
        /// </summary>
        public int NumberOfPassenger { get; set; }
        
        /// <summary>
        /// 是否有冷氣
        /// </summary>
        public bool HasAirConditioner { get; set; }

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="power"></param>
        /// <param name="numberOfPassenger"></param>
        /// <param name="hasAirConditioner"></param>
        public Bus(decimal power, int numberOfPassenger, bool hasAirConditioner)
        {
            this.Power = power;
            this.NumberOfPassenger = numberOfPassenger;
            this.HasAirConditioner = hasAirConditioner;
        }

        /// <summary>
        /// Accept
        /// </summary>
        /// <param name="visitor"></param>
        public void Accept(IVisitor visitor)
        {
            visitor.Visit(this);
        }
    }
}
{% endcodeblock %}

---

## 實作 Visitor 類別

我們使用名為雙重分發 (dual dispatch) 的技術，這項技術是 Visitor 模式的核心機制，這個雙重分發涉及了兩個多型分發，第一個是機車及巴士類別的 `Accept` 方法，我們可以根據該分發辨別出是機車還是巴士類別呼叫了 `Accept` 方法；第二個是 `Accept` 方法所呼叫的 `Visit` 方法，根據上一個多型分發，判斷出是哪一個類別，再呼叫該類別所要執行的特定函式。
接著我們根據不同的類別，實作不同的 `Visit` 方法：

{% codeblock lang:cs %}
/*Visitor.cs*/

using System;

namespace DemoCode.DesignPattern.Visitor.OriginVisitor
{
    /// <summary>
    /// Visitor
    /// </summary>
    public class Visitor : IVisitor
    {
        /// <summary>
        /// Visit for Motor
        /// </summary>
        /// <param name="motor"></param>
        public void Visit(Motor motor)
        {
            Console.WriteLine($"這是台機車，我只在意馬力:{motor.Power}");
        }

        /// <summary>
        /// Visit for Bus
        /// </summary>
        /// <param name="bus"></param>
        public void Visit(Bus bus)
        {
            var ifHasAirConditioner = bus.HasAirConditioner == true ? "有冷氣" : "沒有冷氣";

            Console.WriteLine($"這是台公車，我只在意限乘人數:{bus.NumberOfPassenger}," +
                              $"以及有沒有冷氣:{ifHasAirConditioner}");
        }
    }
}
{% endcodeblock %}

---

{% codeblock lang:cs %}
/*Program.cs*/
class Program
{
    static void Main(string[] args)
    {
        var bus = new Bus(1000, 20, true);
        var motor = new Motor(100, 2, false);

        var visitor = new Visitor();
        bus.Accept(visitor);
        motor.Accept(visitor);
    }
}
{% endcodeblock %}

最後輸出結果如下：

```
我是台公車，我只在意限乘人數:20,以及有沒有冷氣:有冷氣
我是台機車，我只在意馬力:100
```

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

Visitor 模式讓我們在不修改既有類別的情形下，替巴士及機車類別新增了方法。
維基百科說道：訪問者模式是一種將算法與對象結構分離的軟體設計模式。若類別不常異動 (e.g. 新增一個飛機類別)，但常常需要新增方法，則可以使用 Visitor 模式。

## 參考

VISITOR 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[Wiki - 訪問者模式](https://zh.wikipedia.org/wiki/%E8%AE%BF%E9%97%AE%E8%80%85%E6%A8%A1%E5%BC%8F)
[每個人關心的點都不同 - 訪問者模式 (Visitor Pattern)](https://ithelp.ithome.com.tw/articles/10208766)
[訪問者模式 (Visitor Pattern)](http://corrupt003-design-pattern.blogspot.com/2017/02/visitor-pattern.html)
