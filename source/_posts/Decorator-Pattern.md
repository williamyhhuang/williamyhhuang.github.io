title: '[Design Pattern] Visitor 模式 (三)'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2022-03-03 22:30:00
---

Visitor 模式可以再不改變現有類別結構的情況下，像類別結構增加新方法。另一個可以達到相同目的的模式是 Decorator 模式。

想像假設要實作一個方法，乘客要下車時需要按下車鈴通知司機。如果將該方法宣告在介面中，並讓各個類別實作，這樣也不是不行，但是如果之後有更多需求，就會變得需要頻繁異動該類別。

所以可以這樣想：讓有該需求的使用者去呼叫實作該需求的類別即可，即我實作一個「下車按鈴」的類別，讓公車類別來使用，如果交通工具是機車的話就不會使用到該類別。

<!--more-->

---

## 按鈴下公車

{% codeblock lang:cs %}
/*Itransportation.cs*/

using System;
namespace DemoCode.DesignPattern.Visitor.Decorator
{
    /// <summary>
	/// 交通工具
	/// </summary>
    public interface ITransportation
    {
        /// <summary>
        /// 找座位
        /// </summary>
        void TakeASeat();

        /// <summary>
        /// 下車
        /// </summary>
        void TakeOff();
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*Bus.cs*/

using System;
namespace DemoCode.DesignPattern.Visitor.Decorator
{
    /// <summary>
    /// 公車
    /// </summary>
    public class Bus : ITransportation
    {
        /// <summary>
        /// 找座位
        /// </summary>
        public void TakeASeat()
        {
            Console.WriteLine("找個座位");
        }

        /// <summary>
        /// 下車
        /// </summary>
        public void TakeOff()
        {
            Console.WriteLine("司機先生我要下車");
        }
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*RingBeforeTakeOffBus.cs*/

using System;
namespace DemoCode.DesignPattern.Visitor.Decorator
{
    /// <summary>
    /// 下車前需要按鈴
    /// </summary>
    public class RingBeforeTakeOffBus : ITransportation
    {
        /// <summary>
        /// 交通工具
        /// </summary>
        private ITransportation itsTransportation;

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="transportation"></param>
        public RingBeforeTakeOffBus(ITransportation transportation)
        {
            this.itsTransportation = transportation;
        }

        /// <summary>
        /// 找座位
        /// </summary>
        public void TakeASeat()
        {
            itsTransportation.TakeASeat();
        }

        /// <summary>
        /// 下車
        /// </summary>
        public void TakeOff()
        {
            Console.WriteLine("按鈴下車");
            itsTransportation.TakeOff();
        }
    }
}
{% endcodeblock %}

---

## 收攏重複的程式碼

如果現在要實作其他的裝飾者類別，例如下車前要刷卡 (BeepBeforeTakeOff)，但是如果照前面的寫法，`TakeASeat()` 這個方法的程式碼會重複出現於 `BipBeforeTakeOff` 及 `RingBeforeTakeOff` 類別，我們可以把會重複的程式碼收攏在一個類別即可。

{% codeblock lang:cs %}
/*BusDecorator.cs*/

using System;
namespace DemoCode.DesignPattern.Visitor.Decorator
{
    public class BusDecorator : ITransportation
    {
        private ITransportation itsTransportation;

        public BusDecorator(ITransportation transportation)
        {
            this.itsTransportation = transportation;
        }

        public void TakeASeat()
        {
            itsTransportation.TakeASeat();
        }

        public void TakeOff()
        {
            itsTransportation.TakeOff();
        }
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*BeepBeforeTakeOffBus.cs*/

using System;
namespace DemoCode.DesignPattern.Visitor.Decorator
{
    public class BeepBeforeTakeOffBus : BusDecorator
    {
        private ITransportation itsTransportation;

        public BeepBeforeTakeOffBus(ITransportation transportation) : base(transportation)
        {
            itsTransportation = transportation;
        }

        public new void TakeOff()
        {
            Console.WriteLine("下車前刷卡");
            itsTransportation.TakeOff();
        }
    }
}
{% endcodeblock %}

輸出結果：

{% codeblock lang:cs %}
class Program
{
    static void Main(string[] args)
    {
        var bus = new DemoCode.DesignPattern.Visitor.Decorator.Bus();
        var decorator = new DemoCode.DesignPattern.Visitor.Decorator.BeepBeforeTakeOffBus(bus);

        decorator.TakeOff();
    }
}
{% endcodeblock %}

```
下車前刷卡
司機先生我要下車
```

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

裝飾者模式提供了另一種方法，來達到在不改變既有的類別下增加新的方法。裝飾者模式與其他模式的比較可以 [參考此篇](https://ithelp.ithome.com.tw/articles/10218692)

## 參考

VISITOR 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[iThome 鐵人競賽 - [Design Pattern] Decorator 裝飾者模式](https://ithelp.ithome.com.tw/articles/10218692)
