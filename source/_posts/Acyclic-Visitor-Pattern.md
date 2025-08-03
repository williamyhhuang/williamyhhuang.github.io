title: '[Design Pattern] Visitor 模式 (二)'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2022-02-03 22:30:00
---

在上一篇介紹了 Visitor 模式，在不常新增衍生類別時，它會是一個好方法。但如果今天要新增一個飛機類別，要在 `IVisitor` 定義一個新方法及在 `Visitor` 類別中實作該方法，新增完後會需要將既有的類別重新編譯，很不方便。

Acyclic Visitor Pattern 解決了這個問題，我們可以將 `IVisitor` 介面退化，讓它不包含任何方法，然後各個類別各自建立自己的介面。

<!--more-->

---

## Acyclic Visitor 模式

相較於上一篇的 `IVisitor`，`IAcyclicVisitor` 沒有包含任何方法，而是各個類別建立各自的介面，如下所示：

{% codeblock lang:cs %}
/*IAcyclicVisitor.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// IAcyclicVisitor
    /// </summary>
    public interface IAcyclicVisitor
    {
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*IMotorVisitor.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// IMotorVisitor
    /// </summary>
    public interface IMotorVisitor : IAcyclicVisitor
    {
        /// <summary>
        /// Visit for Motor
        /// </summary>
        /// <param name="motor"></param>
        void Visit(AcyclicMotor motor);
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*IBusVisitor.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// IBusVisitor
    /// </summary>
    public interface IBusVisitor : IAcyclicVisitor
    {
        /// <summary>
        /// Visit for Bus
        /// </summary>
        /// <param name="bus"></param>
        void Visit(AcyclicBus bus);
    }
}
{% endcodeblock %}

---

巴士、機車類別的實作跟上一篇差不多，只是在呼叫 `Visit` 方法前，要先判斷是否為該類別之介面：

{% codeblock lang:cs %}
/*IAcyclicTransportation.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// IAcyclicTransportation
    /// </summary>
    public interface IAcyclicTransportation
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
        void Accept(IAcyclicVisitor visitor);
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*AcyclicMotor.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// 機車
    /// </summary>
    public class AcyclicMotor : IAcyclicTransportation
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
        public AcyclicMotor(decimal power, int numberOfPassenger, bool hasAirConditioner)
        {
            this.Power = power;
            this.NumberOfPassenger = numberOfPassenger;
            this.HasAirConditioner = hasAirConditioner;
        }

        /// <summary>
        /// Accept
        /// </summary>
        /// <param name="visitor"></param>
        public void Accept(IAcyclicVisitor visitor)
        {
            if (visitor is IMotorVisitor)
            {
                (visitor as IMotorVisitor).Visit(this);
            }
        }
    }
}
{% endcodeblock %}

{% codeblock lang:cs %}
/*AcyclicBus.cs*/

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// 巴士
    /// </summary>
    public class AcyclicBus : IAcyclicTransportation
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
        public AcyclicBus(decimal power, int numberOfPassenger, bool hasAirConditioner)
        {
            this.Power = power;
            this.NumberOfPassenger = numberOfPassenger;
            this.HasAirConditioner = hasAirConditioner;
        }

        /// <summary>
        /// Accept
        /// </summary>
        /// <param name="visitor"></param>
        public void Accept(IAcyclicVisitor visitor)
        {
            if (visitor is IBusVisitor)
            {
                (visitor as IBusVisitor).Visit(this);
            }
        }
    }
}
{% endcodeblock %}

---

最後是實作 AcyclicVisitor 類別，除了要繼承巴士、機車相對應的介面並實作，其他地方跟之前的一樣：

{% codeblock lang:cs %}
/*AcyclicVisitor.cs*/

using System;

namespace DemoCode.DesignPattern.Visitor.AcyclicVisitor
{
    /// <summary>
    /// AcyclicVisitor
    /// </summary>
    public class AcyclicVisitor : IBusVisitor, IMotorVisitor
    {
        /// <summary>
        /// 建構子
        /// </summary>
        public AcyclicVisitor()
        {
        }

        /// <summary>
        /// Visit for Motor
        /// </summary>
        /// <param name="motor"></param>
        public void Visit(AcyclicMotor motor)
        {
            Console.WriteLine($"這是台機車，我只在意馬力:{motor.Power}");
        }

        /// <summary>
        /// Visit for Bus
        /// </summary>
        /// <param name="bus"></param>
        public void Visit(AcyclicBus bus)
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
        var bus = new AcyclicBus(1000, 20, true);
        var motor = new AcyclicMotor(100, 2, false);

        var visitor = new AcyclicVisitor();
        bus.Accept(visitor);
        motor.Accept(visitor);
    }
}
{% endcodeblock %}

最後輸出結果如下，跟前一篇結果相同：

```
我是台公車，我只在意限乘人數:20,以及有沒有冷氣:有冷氣
我是台機車，我只在意馬力:100
```

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

Acyclic Visitor 模式將巴士、機車的介面獨立出來，未來若要新增飛機類別，只要新增飛機的介面、類別，以及在 AcyclicVisitor 實作飛機的 Visit 方法即可。

在【無瑕的程式碼】中提到，因為有使用轉型(在巴士、機車類別中的 `Accept` 方法)，會花費大量的執行時間，所以 Acyclic Visitor 模式不適用於硬即時模式(hard real-time system)。查了一下是什麼意思，將參考附圖於下方。

<img src="https://imgur.com/AjNgTEZ.png">

## 參考

VISITOR 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[16.即時作業系統](https://sls.weco.net/node/21340)