title: '[.NET Core] 用微軟的 DI 工具優雅地註冊多個服務'
author: ''
date: 2021-12-08 02:21:58
tags:
  - C#
  - .NET Core
  - Dependency Injection
categories:
  - Dependency Injection
---
.NET Core 支援相依性注入 (Depedency Injection) 的設計模式，以往的 .NET Framework 在不支援 DI 的時候，我們會利用一些套件幫助我們完成這件事，例如 Autofac。假設現在我們有多個服務要註冊，使用 Autofac 來做 DI，可以使用以下的方法實現：

{% codeblock lang:cs %}
public class ServiceDI : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        /// 一個一個註冊
        builder.RegisterType<ServiceA>().As<IServiceA>().WithAttributeFiltering();
        builder.RegisterType<ServiceB>().As<IServiceB>().WithAttributeFiltering();
        builder.RegisterType<ServiceC>().As<IServiceC>().WithAttributeFiltering();

        /// 一次註冊多個
        var serviceTypes = Assembly.Load("Where.My.Services.At");
        builder.RegisterAssemblyTypes(serviceTypes)
               .AsImplementedInterfaces()
               .WithAttributeFilter();
    }    
}
{% endcodeblock %}

<!--more-->

---

現在我們想用微軟的 DI 工具來達到一樣的事情，若參考微軟官方的教學文件，也是教大家一個一個註冊，後來翻了一下文件，發現其實也有跟上面 Autofac 一樣，可以用參數型別為 Type 的方法來進行註冊：`AddScoped(IServiceCollection, Type, Type)`。

{% codeblock lang:cs %}
using System.Linq;
using System.Reflection;
using Microsoft.Extensions.DependencyInjection;

public static class ServiceMicrosoftDI
{
    public static void RegisterService(this IServiceCollection services)
    {
        var serviceTypes = Assembly.Load("Where.My.Services.At");
        var implementationTypes = serviceTypes.GetTypes().Where(i => i.Name.StartsWith("Service") && i.IsClass);

        foreach(var implementationType in implementationTypes)
        {
            var serviceName = string.Format("I{0}", implementationType.Name);

            var serviceType = implementationType.GetInterface(serviceName);

            services.AddScoped(serviceType, implementationType);
        }
    }
}
{% endcodeblock %}

最後在 `Startup.cs` 中註冊

{% codeblock lang:cs %}
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers();
    services.RegisterService();
}
{% endcodeblock %}

---
上面的寫法有個前提是 Interface 和實作的 Class，命名有一個 Pattern 存在 (即開頭差一個 `I` )，如果有其他更好的寫法歡迎提供。

## 參考

・[How to register a service with multiple interfaces in ASP.NET Core DI](https://andrewlock.net/how-to-register-a-service-with-multiple-interfaces-for-in-asp-net-core-di/)
・微軟官方文件 - [.NET 中的相依性插入](https://docs.microsoft.com/zh-tw/dotnet/core/extensions/dependency-injection)
・微軟官方文件 - [ServiceCollectionServiceExtensions.AddScoped 方法](https://docs.microsoft.com/zh-tw/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions.addscoped?view=dotnet-plat-ext-6.0&viewFallbackFrom=netcore-3.1)
