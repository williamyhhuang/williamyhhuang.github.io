title: '[Design Pattern] Factory 模式'
author: ''
date: 2021-04-04 12:31:52
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
---
假設我們今天要開一間玩具工廠，工廠生產的玩具有：金剛、哥吉拉，寫法可能像這樣：

#### Program.cs

{% codeblock lang:cs %}
/*Program.cs*/

using System;
using DemoCode.DesignPattern.Factory;

namespace DemoCode
{
    class Program
    {
        static void Main(string[] args)
        {
            IToy toy1 = new Gozilla();
            IToy toy2 = new KingKong();

            Console.WriteLine($"Toy I made: {toy1.ToyName}");
            Console.WriteLine($"Toy I made: {toy2.ToyName}");
        }
    }
}
{% endcodeblock %}

<!--more-->

#### IToy.cs

{% codeblock lang:cs %}
/*IToy.cs*/

namespace DemoCode.DesignPattern.Factory
{
    public interface IToy
    {
        public string ToyName { get; set; }
    }
}
{% endcodeblock %}

#### Gozilla.cs

{% codeblock lang:cs %}
/*Gozilla.cs*/

namespace DemoCode.DesignPattern.Factory
{
    public class Gozilla : IToy
    {
        public string ToyName { get; set; }

        public Gozilla()
        {
            ToyName = "Gozilla";
        }
    }
}
{% endcodeblock %}

#### KingKong.cs

{% codeblock lang:cs %}
/*KingKong.cs*/

namespace DemoCode.DesignPattern.Factory
{
    public class KingKong : IToy
    {
        public string ToyName { get; set; }

        public KingKong()
        {
            ToyName = "KingKong";
        }
    }
}
{% endcodeblock %}

架構圖會像是這樣子，我們會在 Program 裡直接建立 `KingKong`、`Gozilla` 執行個體，如果這個類別不常變更，其實直接建立是沒有問題的，但如果是在開發階段，我們應當依賴於抽象介面，避免受類別變化帶來的影響。

<img src="https://i.imgur.com/KNUVYxB.png">

---

## Factory 模式示範

DIP 原則告訴我們：應該優先依賴於抽象類別，避免依賴於實體類別。
所以我們新增一個工廠介面 `IFactory`，以及實作該介面的類別 `Factory`，藉由這個工廠來幫我們生產玩具：

#### Program.cs

{% codeblock lang:cs %}
/*Program.cs*/

using System;
using DemoCode.DesignPattern.Factory;

namespace DemoCode
{
    class Program
    {
        static void Main(string[] args)
        {
            IFactory factory = new Factory();

            IToy toy1 = factory.Make("KingKong");
            IToy toy2 = factory.Make("Gozilla");

            Console.WriteLine($"Toy I made is: {toy1.ToyName}");
            Console.WriteLine($"Toy I made is: {toy2.ToyName}");
        }
    }
}
{% endcodeblock %}

#### IFactory.cs

{% codeblock lang:cs %}
/*IFactory.cs*/

namespace DemoCode.DesignPattern.Factory
{
    public interface IFactory
    {
        IToy Make(string toyName);
    }
}
{% endcodeblock %}

#### Factory.cs

{% codeblock lang:cs %}
/*Factory.cs*/
using System;

namespace DemoCode.DesignPattern.Factory
{
    public class Factory : IFactory
    {
        public Factory()
        {
        }

        public IToy Make(string toyName)
        {
            switch (toyName)
            {
                case "Gozilla":
                    return new Gozilla();
                    
                case "KingKong":
                    return new KingKong();
                    
                default:
                    throw new ApplicationException($"沒有生產該玩具: {toyName}");
            }
        }
    }
}
{% endcodeblock %}

我們在 Program 中建立一個 `Factory` 的類別實作，並將製作玩具的工作交給 `Factory`，這樣的話職責就很明確，工廠專職製造玩具，Program 只需直接使用 `Factory` 所回傳的類別實作。

<img src="https://i.imgur.com/MR6zSEw.png">

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

## 結論

Factory 模式使得高層模組不依賴於類別的實作，並遵守 DIP 原則，在程式的職責管理上也較為清楚明白。但如果你的類別是不常變更且穩定的，那也不需使用 Factory 模式，直接建立類別實作會省事許多。

## 參考

Factory 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
