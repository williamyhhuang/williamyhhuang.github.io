title: '[Design Pattern] Strategy 模式'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2021-02-12 21:00:00
---
上一篇有提到，Template Method Pattern 違反了 DIP 原則，在寫程式時最需要注意的就是耦合性，倘若程式之間的耦合性高，修改一個類別結果造成所有繼承他的類別都需要修改，這樣的維護成本太高，而 Strategy Pattern 提供了解法。接續上一篇的範例，我們使用 Strategy 模式再重寫一次，會分為以下幾個步驟：

<ol>
	<li>定義料理的抽象介面</li>
  	<li>實作中式料理類別</li>
  	<li>實作料理類別</li>
</ol>

<!--more-->

---

## 定義料理的抽象介面

首先先定義一個介面 `ICooking`，可以想像一下，如果做中式料理跟西式料理，有什麼步驟是相同的，但是詳細實作方法卻是不同的。以我們之前的範例，我將 **準備材料** 跟 **烹飪** 這兩個方法抽成介面，另外，在做菜前一定會知道要做什麼料理，所以我把 **料理名稱** 這個變數也放在介面中，後續繼承 `ICooking` 介面的類別，則需要實作這幾個方法及宣告屬性：

{% codeblock lang:cs %}
/*ICooking.cs*/

namespace DemoCode.DesignPattern.Strategy
{
    /// <summary>
    /// 烹飪步驟
    /// </summary>
    public interface ICooking
    {
        /// <summary>
        /// 料理名稱
        /// </summary>
        string DishName { get; set; }

        /// <summary>
        /// 準備材料
        /// </summary>
        void Prepare();

        /// <summary>
        /// 烹飪
        /// </summary>
        void Cook();
    }
}
{% endcodeblock %}

---

## 實作中式料理類別

現在來實作中式料理，由於 `ChineseDish` 繼承了 `ICooking`，需要實作其方法，而料理名稱是在建立 `ChineseDish` 時注入。往後若要實作西式料理類別，則只要繼承 `ICooking` 並實作即可：

{% codeblock lang:cs %}
/*ChineseDish.cs*/

using System;
using System.Collections.Generic;

namespace DemoCode.DesignPattern.Strategy
{
    public class ChineseDish : ICooking
    {
        /// <summary>
        /// 料理名稱
        /// </summary>
        public string DishName { get; set; }

        /// <summary>
        /// 食材
        /// </summary>
        private List<string> _ingredients;

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="dishName"></param>
        public ChineseDish(string dishName)
        {
            this.DishName = dishName;
        }

        /// <summary>
        /// 準備材料
        /// </summary>
        public void Prepare()
        {
            Console.WriteLine($"今天要來做{this.DishName}");

            switch (this.DishName)
            {
                case "辣炒牛肉空心菜":
                    this._ingredients = new List<string>
                    {
                        "辣椒",
                        "空心菜",
                        "牛肉"
                    };
                    break;
            }

            //// 洗菜
            this.Clean();

            //// 切菜
            this.Cut();

            Console.WriteLine("材料準備好了");
        }

        /// <summary>
        /// 烹飪
        /// </summary>
        public void Cook()
        {
            Console.WriteLine("中式料理當然要用鍋炒，材料有:{0}", string.Join("，", this._ingredients));
            Console.WriteLine("菜煮好了");
        }

        /// <summary>
        /// 洗菜
        /// </summary>
        private void Clean()
        {
            Console.WriteLine("菜洗好了");
        }

        /// <summary>
        /// 切菜
        /// </summary>
        private void Cut()
        {
            Console.WriteLine("菜切好了");
        }

    }
}
{% endcodeblock %}

---

## 實作料理類別

接下來我們來實作料理類別 `Cooking`，你可以想像成這個類別是餐廳的內場，它可以製作中式料理或是西式料理，只管跟他點菜即可。另外像 `SetDish` 及 `Done` 這兩個方法，因為做任何料理都有這兩個步驟，且實作細節皆相同，所以我把它放在 `Cooking` 類別中並實作：

{% codeblock lang:cs %}
/*Cooking.cs*/

using System;
namespace DemoCode.DesignPattern.Strategy
{
    public class Cooking
    {
        /// <summary>
        /// ICooking
        /// </summary>
        private ICooking _cooking;

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="cooking"></param>
        public Cooking(ICooking cooking)
        {
            this._cooking = cooking;
        }

        /// <summary>
        /// 準備材料
        /// </summary>
        public void Prepare()
        {
            this._cooking.Prepare();
        }

        /// <summary>
        /// 烹飪
        /// </summary>
        public void Cook()
        {
            this._cooking.Cook();
        }

        /// <summary>
        /// 裝盤
        /// </summary>
        public void SetDish()
        {
            Console.WriteLine($"{this._cooking.DishName}好了，可以上菜囉");
        }

        /// <summary>
        /// 完成
        /// </summary>
        public void Done()
        {
            Console.WriteLine("完成上菜，來整理廚房");
            this.CleanUp();
        }

        /// <summary>
        /// 整理廚房
        /// </summary>
        private void CleanUp()
        {
            Console.WriteLine("廚房整理好了");
        }
    }
}
{% endcodeblock %}

---
最後來看結果：

{% codeblock lang:cs %}
namespace DemoCode
{
    class Program
    {
        static void Main(string[] args)
        {
            // 先定義要做中式料理，並注入料理名稱
            var dish = new DesignPattern.Strategy.ChineseDish("辣炒牛肉空心菜");
            // 將 dish 注入至 Cooking 類別
            var cook = new DesignPattern.Strategy.Cooking(dish);

            cook.Prepare();
            cook.Cook();
            cook.SetDish();
            cook.Done();
            
        }
    }
}
{% endcodeblock %}

輸出結果如下，跟我們在 Template Method Pattern 得到的結果一樣：

```
今天要來做辣炒牛肉空心菜
菜洗好了
菜切好了
材料準備好了
中式料理當然要用鍋炒，材料有:辣椒，空心菜，牛肉
菜煮好了
辣炒牛肉空心菜好了，可以上菜囉
完成上菜，來整理廚房
廚房整理好了
```

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

## 結論

Strategy Pattern 的好處在於可擴充性及靈活度，往後若需要製作印度料理或是法式料理，則只要建立一個繼承 `ICooking` 介面的類別，在要新增 `Cooking` 類別時再注入。

這兩種 Pattern 可以想像成你想開一間什麼料理都有的餐廳，若是使用 Template Method Pattern，你需要每個料理都建立一個攤位，然後依據你想要的料理去各別的攤位點餐；而 Strategy Pattern 就好比你只需要跟中央廚房說你要什麼料理，無需東奔西跑。

## 參考

TEMPLATE METHOD 模式和 STRATEGY 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
