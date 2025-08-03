title: '[Design Pattern] Template Method 模式'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2021-02-11 17:30:00
---
Template Method Pattern，顧名思義它就是一個模板，必須要在它指定的框架內完成實作。

所以要使用 Template Method Pattern，可以分為幾個步驟：

<ol>
    <li> 定義父類別抽象類型，也就是定義框架 </li>
    <li> 子類別類型繼承父類別 </li>
    <li> 子類別類型實作父類別的抽象方法 </li>
</ol>

<!--more-->

---

## 定義父類別抽象類型

讓我們以<b>「烹飪」</b>當作範例，首先要定義烹飪會需要完成哪些步驟：

{% codeblock lang:cs %}
/*Cooking.cs*/

using System;

namespace TemplateMethodPattern
{
    /// <summary>
    /// 烹飪
    /// </summary>
    public abstract class Cooking
    {
        /// <summary>
        /// 料理名稱
        /// </summary>
        public string _dishName;

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="dishName"></param>
        public Cooking(string dishName)
        {
            this._dishName = dishName;
        }

        /// <summary>
        /// 準備材料
        /// </summary>
        /// <param name="dishName"></param>
        public abstract void Prepare();

        /// <summary>
        /// 烹飪
        /// </summary>
        /// <returns></returns>
        public abstract void Cook();

        /// <summary>
        /// 裝盤
        /// </summary>
        public void SetDish()
        {
            Console.WriteLine($"{this._dishName}好了，可以上菜囉");
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

## 實作父類別的抽象方法

今天要做一道辣炒牛肉空心菜，我們建立一個子類別中式料理 `ChineseDish`，繼承父類別，並實作父類別的方法：

{% codeblock lang:cs %}
/*ChineseDish.cs*/

using System;
using System.Collections.Generic;

namespace TemplateMethodPattern
{
    /// <summary>
    /// 中式料理
    /// </summary>
    public class ChineseDish : Cooking
    {

        /// <summary>
        /// 食材
        /// </summary>
        private List<string> _ingredients;

        /// <summary>
        /// 建構子
        /// </summary>
        /// <param name="dishName"></param>
        public ChineseDish(string dishName) : base(dishName)
        {
        }

        //// 準備材料
        public override void Prepare()
        {
            Console.WriteLine($"今天要來做{this._dishName}");

            switch (this._dishName)
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
        public override void Cook()
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

最後來看看結果：
{% codeblock lang:cs %}
/*Program.cs*/

using System;
using System.Collections.Generic;

namespace TemplateMethodPattern
{
    class Program
    {
        static void Main(string[] args)
        {
            var dish = new ChineseDish("辣炒牛肉空心菜");

            dish.Prepare();
            dish.Cook();
            dish.SetDish();
            dish.Done();
        }
    }
}
{% endcodeblock %}

輸出結果如下：
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

---

## 結論

這樣就完成了我們的中式料理，也可以用相同的方式做其他類型的料理。
你可以想像成 Template Method Pattern 就像是你看著食譜做菜，只要照著“食譜”這個框架完成即可。
不過 Template Method Pattern 是一種強烈的繼承關係，違反了 DIP 原則：**高層模組不應該相依於低層模組，兩者都應該相依於抽象**。下一篇介紹的 Strategy Design Pattern 解決了這個問題。

## 參考

TEMPLATE METHOD 模式和 STRATEGY 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[Wiki - Template method pattern](https://en.wikipedia.org/wiki/Template_method_pattern)
[樣板方法模式](http://corrupt003-design-pattern.blogspot.com/2016/07/template-method-pattern.html)
