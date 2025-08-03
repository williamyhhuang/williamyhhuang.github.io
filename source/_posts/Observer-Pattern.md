title: '[Design Pattern] Observer 模式'
author: ''
tags:
  - C#
  - Design Pattern
categories:
  - Design Pattern
date: 2021-07-12 01:21:00
---
訂閱者模式在生活中處處可見，例如讀者訂閱新聞。而我是這麼理解觀察者模式的；當我在意的「新聞中心」有更新時，它會通知我，我再去看它的更新為何，在參考書中通常會將新聞中心這個角色稱為「主題」，讀者稱為「觀察者」，下面的範例就會以讀者訂閱新聞去做解釋。主要會分為三段：

<ol>
	<li>主題</li>
  <li>觀察者</li>
  <li>註冊及發送通知</li>
</ol>

<!--more-->

---

### 主題 (Subject)

首先先來定義「新聞中心」會做什麼事，假設它有以下的三個功能：

<ol>
	<li>接受讀者訂閱</li>
  <li>增加新聞</li>
  <li>通知讀者</li>
</ol>

我們將這三個功能定義成一個介面，然後簡單定義新聞類別，最後就是實作新聞中心：

#### ISubject.cs

{% codeblock lang:cs %}
/*ISubject.cs*/

namespace DemoCode.DesignPattern.Observer
{
    public interface ISubject
    {
        /// <summary>
        /// 訂閱者註冊
        /// </summary>
        /// <param name="observer">訂閱者</param>
        void RegisterObserver(IObserver observer);

        /// <summary>
        /// 通知訂閱者
        /// </summary>
        void NotifyObservers();

        /// <summary>
        /// 增加新聞
        /// </summary>
        /// <param name="news"></param>
        void AddNews(News news);
    }
}
{% endcodeblock %}

#### NewsCenter.cs

{% codeblock lang:cs %}
/*NewsCenter*/

using System.Collections.Generic;

namespace DemoCode.DesignPattern.Observer
{
    public class NewsCenter : ISubject
    {
        /// <summary>
        /// 訂閱者清單
        /// </summary>
        private List<IObserver> _itsObservers;

        /// <summary>
        /// 新聞清單
        /// </summary>
        public List<News> _newsList;

        public NewsCenter()
        {
            this._itsObservers = new List<IObserver>();
            ``this._newsList = new List<News>();
        }

        /// <summary>
        /// 通知訂閱者
        /// </summary>
        public void NotifyObservers()
        {
            foreach(var observer in this._itsObservers)
            {
                observer.Update();
            }
        }

        /// <summary>
        /// 訂閱者註冊
        /// </summary>
        /// <param name="observer">訂閱者</param>
        public void RegisterObserver(IObserver observer)
        {
            this._itsObservers.Add(observer);
        }

        /// <summary>
        /// 增加新聞
        /// </summary>
        /// <param name="news"></param>
        public void AddNews(News news)
        {
            this._newsList.Add(news);
            this.NotifyObservers();
        }
    }
}
{% endcodeblock %}

#### News.cs

{% codeblock lang:cs %}
/*News.cs*/

namespace DemoCode.DesignPattern.Observer
{
    /// <summary>
    /// 新聞
    /// </summary>
    public class News
    {
        /// <summary>
        /// 分類
        /// </summary>
        public CategoryEnum Category { get; set; }

        /// <summary>
        /// 作者
        /// </summary>
        public string Author { get; set; }

        /// <summary>
        /// 標題
        /// </summary>
        public string Title { get; set; }

        /// <summary>
        /// 內容
        /// </summary>
        public string Content { get; set; }
    }
}
{% endcodeblock %}

#### CategoryEnum.cs

{% codeblock lang:cs %}
/*CategoryEnum.cs*/

namespace DemoCode.DesignPattern.Observer
{
    /// <summary>
    /// 新聞分類
    /// </summary>
    public enum CategoryEnum
    {
        /// <summary>
        /// 政治
        /// </summary>
        Politics,

        /// <summary>
        /// 體育
        /// </summary>
        Sports
    }
}
{% endcodeblock %}

---

### 觀察者 (Observer)

現實生活中，通常是讀者收到新聞更新的通知後，再決定要做什麼，我們先簡單定義讀者只會做 `Update()` 這個動作

#### IObserver.cs

{% codeblock lang:cs %}
/*IObserver.cs*/

namespace DemoCode.DesignPattern.Observer
{
    public interface IObserver
    {
        /// <summary>
        /// 更新訊息
        /// </summary>
        void Update();
    }
}
{% endcodeblock %}

#### Reader.cs

{% codeblock lang:cs %}
/*Reader.cs*/

using System;
namespace DemoCode.DesignPattern.Observer
{
    public class Reader : IObserver
    {
        /// <summary>
        /// 訂閱者
        /// </summary>
        private string _readerName;

        /// <summary>
        /// 訂閱類型
        /// </summary>
        private CategoryEnum _registeredCategory;

        /// <summary>
        /// 
        /// </summary>
        /// <param name="readerName">訂閱者</param>
        /// <param name="category">訂閱類型</param>
        public Reader(string readerName, CategoryEnum category)
        {
            this._readerName = readerName;
            this._registeredCategory = category;

            this.RegisterMessage(readerName, category);
        }

        /// <summary>
        /// 更新訊息
        /// </summary>
        public void Update()
        {
                Console.WriteLine($"{this._readerName} 訂閱的新聞中心有新的新聞，但不知道新聞類型");
        }

        /// <summary>
        /// 訂閱訊息
        /// </summary>
        /// <param name="readerName">訂閱者</param>
        /// <param name="category">訂閱類型</param>
        private void RegisterMessage(string readerName, CategoryEnum category)
        {
            Console.WriteLine($"{readerName} 訂閱了新聞類型：{category}");
        }
    }
}
{% endcodeblock %}

---

### 註冊及發送通知

最後我們來建立「新聞中心」及「讀者」，並新增幾則新聞看看結果：

#### Program.cs

{% codeblock lang:cs %}
/*Program.cs*/

using System;
using DemoCode.DesignPattern.Factory;
using DemoCode.DesignPattern.Observer;

namespace DemoCode
{
    class Program
    {
        static void Main(string[] args)
        {
            // Observer
            ISubject newsCenter = new NewsCenter();

            IObserver reader1 = new Reader("Jack", CategoryEnum.Politics);
            IObserver reader2 = new Reader("Lily", CategoryEnum.Sports);

            newsCenter.RegisterObserver(reader1);
            newsCenter.RegisterObserver(reader2);

            News news1 = new News(){ Category = CategoryEnum.Sports, Author = "Leo", Title = "2021東京奧運" };
            News news2 = new News() { Category = CategoryEnum.Politics, Author = "Jerry", Title = "莫德納疫苗抵台" };

            newsCenter.AddNews(news1);
            newsCenter.AddNews(news2);
        }
    }
}
{% endcodeblock %}

---

## 拉模型與推模型

### 拉模型 (pull-model)

以上的範例我們可以得知，讀者只收到新聞中心通知說有新的新聞，但不知道新聞類型為何，收到通知後讀者需要去「拉」 `newsList` 才知道該新聞為何，所以此類型稱為拉模型 (pull-model)。

### 推模型 (push-model)

如果只想在收到特定新聞類型時才做相對應的事情，可以在 `NotifyObservers()` 時多傳入一個參數，讓訂閱者根據傳入參數決定要做什麼事，這種傳入參數的方式即為推模型 (push-model)。Github 的範例即為以推模型做示範，並以 `NotifyObservers(CategoryEnum category)` 通知讀者新增的新聞類型為何。

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

觀察者模式解決了「主題」更新後通知多個「觀察者」，且因為使用介面解了「主題」及「觀察者」間的耦合，每個訂閱者可以在收到通知後決定後續的動作。簡單介紹了拉模型與推模型，決定該使用何者取決於使用情境。

---

## 參考

OBSERVER 模式．*無瑕的程式碼 敏捷完整篇：物件導向原則、設計模式與 C# 實踐*
[[Design Pattern] 觀察者模式 (Observer Pattern) 我也能夠辦報社](https://dotblogs.com.tw/joysdw12/2013/03/13/96531)
