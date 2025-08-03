title: '[Design Pattern] Adapter 模式'
author: ''
date: 2021-09-04 12:31:52
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
---

<img src="https://imgur.com/Cedxi9D.gif">

Wiki 上是這麼定義 Adapter 模式的:
> 將一個類別的介面轉接成使用者所期待的。一個適配使得因介面不相容而不能在一起工作的類別能在一起工作。

舉個例子，我有養狗，但是我喜歡的女生比較喜歡貓，為了吸引他的注意力，我決定把我的狗假扮成一隻貓。套用到 Adapter 模式，可以想像成是這樣: 我要把我的狗 (Adaptee)，藉由外觀的打扮 (Adapter)，變成一隻貓 (Target)

<!--more-->

---

### 看看自己的狗

先來看我們既有的狗類別，以及他既有的方法。

#### Dog.cs

{% codeblock lang:cs %}
/*Dog.cs*/
using System;
namespace DemoCode.DesignPattern.Adapter
{
    /// <summary>
    /// Dog
    /// </summary>
    public class Dog
    {
        /// <summary>
        /// 狗叫
        /// </summary>
        public void Bark()
        {
            Console.WriteLine("I'm a dog, bark");
        }
    }
}
{% endcodeblock %}

---

### 看看要假扮的貓

再來定義貓的介面，要求不多，只要我的狗會學貓叫即可。

#### ICat.cs

{% codeblock lang:cs %}
/*ICat.cs*/
namespace DemoCode.DesignPattern.Adapter
{
    /// <summary>
    /// ICat
    /// </summary>
    public interface ICat
    {
        /// <summary>
        /// 貓叫
        /// </summary>
        void Meow();
    }
}
{% endcodeblock %}

---

### Adapter 模式

先定義一個類別 `CatAdapter` ，其會繼承介面 `ICat` 並需要實作 `ICat` 的方法 。建構子參數為我們既有的狗類別。
現在要實作方法 `Meow()`，實作內容我們使用 Dog 類別的方法 `Bark()`，讓人以為狗叫是貓叫。(現實中是有點強人所難...)

#### CatAdapter.cs

{% codeblock lang:cs %}
/*CatAdapter.cs*/
namespace DemoCode.DesignPattern.Adapter
{
    /// <summary>
    /// CatAdapter
    /// </summary>
    public class CatAdapter : ICat
    {
        /// <summary>
        /// Dog
        /// </summary>
        private Dog _dog;
        /// <summary>
        /// CatAdapter
        /// </summary>
        /// <param name="dog"></param>
        public CatAdapter(Dog dog)
        {
            this._dog = dog;
        }
        /// <summary>
        /// 貓叫
        /// </summary>
        public void Meow()
        {
            this._dog.Bark();
        }
    }
}
{% endcodeblock %}

---

### 類別形式的 Adapter 模式

上一個 Adapter 模式，我們是用建構子注入的方式，將我們的狗類別放進 CatAdapter 類別，另一種方式是直接繼承 Dog 類別，這樣的好處是我只要建立一個 CatAdapterClassType 類別就好，壞處是其造成 Dog 類別與 CatAdapterClassType 類別的強耦合。

#### CatAdapterClassType.cs

{% codeblock lang:cs %}
/*CatAdapterClassType*/
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace DemoCode.DesignPattern.Adapter
{
    /// <summary>
    /// CatAdapterClassType
    /// </summary>
    public class CatAdapterClassType : Dog, ICat
    {
        /// <summary>
        /// 貓叫
        /// </summary>
        public void Meow()
        {
            this.Bark();
        }
    }
}
{% endcodeblock %}

---

最後來看結果:

#### Program.cs

{% codeblock lang:cs %}
class Program
{
    static void Main(string[] args)
    {
        CatAdapter catAdapter = new CatAdapter(new Dog());
        catAdapter.Meow();
        CatAdapterClassType catAdapterClassType = new CatAdapterClassType();
        catAdapterClassType.Meow();
        Console.Read();
    }
}
{% endcodeblock %}

<img src="https://i.imgur.com/SMqjQQP.png">

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

如果因為介面的改變，造成與既有的系統不相容時，可以使用 Adapter 模式，將其轉換為既有系統可使用的格式。

## 參考

- [江湖走跳，轉接頭很重要! (Adapter 適配器模式)](https://ithelp.ithome.com.tw/articles/10194158)
- [Design Pattern: Structural Patterns- Adapter Pattern (適配器模式)](https://medium.com/bucketing/structural-patterns-adapter-pattern-d7889417cff)
- Adapter 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
