title: 如何解決因為 DateTime.Now 導致無法通過單元測試
author: williamyhhuang
tags:
  - C#
  - UnitTest
categories: [UnitTest]
date: 2021-06-19 15:00:00
update: 2021-09-05 22:20:00

---
在寫程式時，我們很常用 DateTime.Now 來取得現在的時間。但這樣會遇到一個問題：若要為該方法寫測試時，會因為使用 DateTime.Now，每次取得的時間都不同，導致測試無法通過。

現在有個方法單純的回傳字串，但因為 timeNow 是不固定的，導致測試無法通過，這邊介紹兩種方法，都可以解決此問題。
{% codeblock lang:cs %}
public string CreateMessage()
{
    DateTime timeNow = DateTime.Now;
    string result = "Time now is " + timeNow.ToString(); 

    return result;
}
{% endcodeblock %}

<!--more-->

---

## 1. 用抽象方法取得現在時間

我們可以用一個抽象方法取得現在時間，寫測試時只要複寫該方法，就能讓取得時間為固定值。這樣做會需要用 StubTimeLogger 去繼承 TimeLogger，並覆寫抽象方法 GetTimeNow。

### TimeLogger.cs:

{% codeblock lang:cs %}
using System;
namespace Test.UnitTest.SystemTime
{
    public class TimeLogger
    {
        public TimeLogger()
        {
        }

        /// <summary>
        /// 取得現在時間字串
        /// </summary>
        /// <returns></returns>
        public string CreateMessage()
        {
            DateTime timeNow = this.GetTimeNow();
            string result = $"Time now is {timeNow:yyyy/MM/dd H:mm:ss}";

            return result;
        }
        /// <summary>
        /// 取得現在時間
        /// </summary>
        /// <returns></returns>
        public virtual DateTime GetTimeNow()
        {
            return DateTime.Now;
        }
    }
}
{% endcodeblock %}

### TimeLoggerTest.cs:

{% codeblock lang:cs %}
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
namespace Test.UnitTest.SystemTime
{
    [TestClass]
    public class TimeLoggerTest
    {
        public TimeLoggerTest()
        {
        }

        [TestMethod]
        public void 印出現在時間_使用StubTimeLogger()
        {
            //// Arrange
            DateTime dateTime = new DateTime(2021, 6, 6, 13, 13, 13);
            StubTimeLogger timeLogger = new StubTimeLogger(dateTime);

            //// Act
            var actual = timeLogger.CreateMessage();
            var expected = "Time now is 2021/06/06 13:13:13";

            //// Assert
            Assert.AreEqual(actual, expected);
        }

        /// <summary>
        /// StubTimeLogger
        /// </summary>
        public class StubTimeLogger : TimeLogger
        {
            private DateTime _dateTime;
            public StubTimeLogger(DateTime dateTime)
            {
                this._dateTime = dateTime;
            }
            public override DateTime GetTimeNow()
            {
                return this._dateTime;
            }
        }
    }
}
{% endcodeblock %}

---

## 2. 建立 SystemTime 類別來取得時間

使用 SystemTime 類別來取得時間，這種寫法就不用建立一個 StubTimeLogger 再複寫抽象方法。

### TimeLoggerUsingSystemTime.cs:

{% codeblock lang:cs %}
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
namespace Test.UnitTest.SystemTime
{
    public class TimeLoggerUsingSystemTime
    {
        /// <summary>
        /// SystemTime
        /// </summary>
        private SystemTime _systemTime;

        /// <summary>
        /// TimeLogger
        /// </summary>
        /// <param name="systemTime"></param>
        public TimeLoggerUsingSystemTime(SystemTime systemTime)
        {
            this._systemTime = systemTime;
        }

        /// <summary>
        /// 取得現在時間字串，使用 SystemTime
        /// </summary>
        /// <returns></returns>
        public string CreateMessage_SystemTime()
        {
            DateTime timeNow = this._systemTime.Now;
            string result = $"Time now is {timeNow:yyyy/MM/dd H:mm:ss}";

            return result;
        }
    }
}

{% endcodeblock %}

### SystemTime.cs:

{% codeblock lang:cs %}
using System;
namespace Test.UnitTest.SystemTime
{
    public class SystemTime
    {
        private DateTime _date;

        public SystemTime()
        {
        }

        /// <summary>
        /// 設定時間
        /// </summary>
        /// <param name="custom"></param>
        public void Set(DateTime custom)
        {
            this._date = custom;
        }

        /// <summary>
        /// 重置時間
        /// </summary>
        public void Reset()
        {
            this._date = DateTime.MinValue;
        }

        /// <summary>
        /// 取得現在時間
        /// </summary>
        public DateTime Now
        {
            get
            {
                if(this._date != DateTime.MinValue)
                {
                    return this._date;
                }
                return DateTime.Now;
            }
        }
    }
}
{% endcodeblock %}

### TimeLoggerUsingSystemTimeTest.cs:

{% codeblock lang:cs %}
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System;
namespace Test.UnitTest.SystemTime
{
    [TestClass]
    public class TimeLoggerUsingSystemTimeTest
    {
        public TimeLoggerUsingSystemTimeTest()
        {
        }
        [TestMethod]
        public void 印出現在時間_使用SystemTime()
        {
            //// Arrange
            SystemTime systemTime = new SystemTime();
            DateTime customDateTime = new DateTime(2021, 6, 6, 13, 13, 13);
            systemTime.Set(customDateTime);
            TimeLoggerUsingSystemTime timeLogger = new TimeLoggerUsingSystemTime(systemTime);

            //// Act
            var actual = timeLogger.CreateMessage_SystemTime();
            var expected = "Time now is 2021/06/06 13:13:13";
            
            //// Assert
            Assert.AreEqual(expected, actual);
        }
    }
}
{% endcodeblock %}

[完整程式碼請參考](https://github.com/williamyhhuang/DemoCode)

---

## 結論

今天介紹了兩種方法，以解決因為使用 DateTime.Now 而無法通過單元測試的問題，我個人是比較喜歡方法一，方法二需注意是否有適時的 Reset，否則可能會取得非預期的時間。

---

## 參考

Chapter 7 測試階層與組織．*單元測試的藝術 第二版*
