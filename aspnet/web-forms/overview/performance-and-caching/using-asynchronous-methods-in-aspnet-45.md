---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: 在 ASP.NET 4.5 中使用异步方法 |Microsoft Docs
author: Rick-Anderson
description: 本教程将讲述构建异步 ASP.NET Web 窗体应用程序使用 Visual Studio Express 2012 for Web，这是一种免费的基础知识...
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: c36749f82051ee8965035eca9c2e4e57a5dbd616
ms.sourcegitcommit: e1cc4c1ef6c9e07918a609d5ad7fadcb6abe3e12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2019
ms.locfileid: "53997339"
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>在 ASP.NET 4.5 中使用异步方法
====================
通过[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 本教程将教您构建异步 ASP.NET Web 窗体应用程序中使用的基础知识[Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11)，这是 Microsoft Visual Studio 的免费版本。 此外可以使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/11)。 在本教程中包含以下各节。
> 
> - [线程池处理请求的方式](#HowRequestsProcessedByTP)
> - [选择同步或异步方法](#ChoosingSyncVasync)
> - [示例应用程序](#SampleApp)
> - [Gizmos，请同步页](#GizmosSynch)
> - [创建异步 gizmos，请页](#CreatingAsynchGizmos)
> - [并行执行多个操作](#Parallel)
> - [使用取消标记](#CancelToken)
> - [针对高并发/高延迟 Web 服务调用的服务器配置](#ServerConfig)
> 
> 为在本教程提供的完整示例  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) 上[GitHub](https://github.com/)站点。


结合使用的 ASP.NET 4.5 Web Pages [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx)使你能够注册返回类型的对象的异步方法[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 .NET Framework 4 引入了异步编程概念，称为[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)和 ASP.NET 4.5 支持[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)。 任务由**任务**类型和中的相关的类型[System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx)命名空间。 与此异步支持生成.NET Framework 4.5 [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)和[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)使用的关键字[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)比以前少得多复杂的对象异步方法。 [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字是速记语法用于指示在某些其他代码段以异步方式等待一段代码。 [异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字都表示一个提示，可用于将方法标记为基于任务的异步方法。 组合**await**，**异步**，并**任务**对象可大大简化了.NET 4.5 中编写异步代码。 调用异步方法的新模型*基于任务的异步模式*(**点击**)。 本教程假定你已具备一定的使用异步编程[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)并[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字并[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间。

有关使用的详细信息[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)并[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字和[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间，请参阅以下参考资料。

- [白皮书：在.NET 中实现异步](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await FAQ](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Visual Studio 异步编程](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  线程池处理请求的方式

在 web 服务器上，.NET Framework 维护一个用于服务 ASP.NET 请求的线程池。 当请求到达时，将调度池中的线程以处理该请求。 如果以同步方式处理该请求，处理请求的线程是繁忙时请求正在处理，，线程不能另一个请求服务。   
  
这可能不是问题，因为线程池可以设置得足够大，以容纳多个忙碌线程数。 但是，在线程池中的线程数会受到限制 （默认为.NET 4.5 最大值为 5,000）。 在大型应用程序使用高并发的长时间运行的请求，所有可用线程可能正忙。 这种情况称为线程资源不足。 当达到这种情况时，web 服务器请求进行排队。 如果在请求队列已满，web 服务器会拒绝请求并处于 HTTP 503 状态 （服务器太忙）。 CLR 线程池将限制对新线程注入。 如果并发性突发 （也就是说，您的网站突然可以获取大量的请求） 和所有可用请求线程处于忙碌状态后端调用由于具有高延迟、 有限的线程注入速率可以使应用程序响应效果非常差。 此外，添加到线程池中每个新线程开销 （例如 1 MB 的堆栈内存)。 使用到服务高延迟调用同步方法的线程池增长到.NET 4.5 默认最多 5 个 web 应用程序，000 线程将占用大约为 5 GB 较多内存的应用程序可以比服务相同请求使用异步方法，并仅有 50 个线程。 当执行异步工作时，你不始终使用线程。 例如，当你进行的异步 web 服务请求，ASP.NET 将不使用任何线程之间**异步**方法调用和**await**。 使用为请求提供服务的线程池具有高延迟可能会导致内存占用量大和的服务器硬件的利用率不佳。

## <a name="processing-asynchronous-requests"></a>处理异步请求

在 web 应用程序看到大量在启动时的并发请求或具有突发负载 （其中会增加并发情况突然），进行异步 web 服务调用将增加你的应用程序的响应能力。 异步请求采用相同量的时间来处理与同步请求。 例如，如果某个请求生成 web 服务调用，则需要两秒钟来完成，请求所执行的两秒内是否执行同步或异步。 但是，在异步调用，线程则不将无法从等待第一个请求完成时响应其他请求。 因此，异步请求可以防止出现请求排队和线程池增长时有许多并发请求调用长时间运行的操作。

## <a id="ChoosingSyncVasync"></a>  选择同步或异步方法

本部分列出了有关何时使用同步或异步方法的指导原则。 这些是只是一些准则;检查每个应用程序分别以确定是否异步方法能帮助提高性能。

一般情况下，使用同步方法满足以下条件：

- 操作很简单或短时间运行。
- 简单性比效率更重要。
- 操作是主要是 CPU 操作而不是涉及大量的磁盘或网络开销的操作。 对 CPU 密集型操作使用异步方法未提供任何好处，并导致更多的开销。

一般情况下，使用异步方法在以下条件：

- 您要调用服务，可通过异步方法，并使用.NET 4.5 或更高版本。
- 操作是网络绑定的或绑定 I/O 而不是受 CPU 限制。
- 并行度不比代码的简单性更重要。
- 你想要提供一种机制，允许用户取消长时间运行的请求。
- 当切换线程的好处远远超过上下文切换的成本。 一般情况下，您应使方法成为异步如果同步方法不执行任何工作时阻止 ASP.NET 请求线程。 可以执行异步调用，ASP.NET 请求线程不被阻止等待完成的 web 服务请求时任何工作。
- 测试显示阻塞操作是在站点的性能瓶颈和 IIS 可以通过使用异步方法对这些阻塞调用服务更多的请求。

  可下载的示例演示如何有效地使用异步方法。 提供的示例旨在提供 ASP.NET 4.5 中的异步编程的简单演示。 该示例不是要在 ASP.NET 中的异步编程的参考体系结构。 在示例程序调用[ASP.NET Web API](../../../web-api/index.md)方法，用于反过来调用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)以模拟长时间运行 web 服务调用。 大多数生产应用程序不会显示使用异步方法对这种明显的优势。   
  
几个应用程序要求所有方法都是异步。 通常情况下，将几个同步方法转换为异步方法提供了显著增加所需的工作量。

## <a id="SampleApp"></a>  示例应用程序

您可以下载的示例应用程序[ https://github.com/RickAndMSFT/Async-ASP.NET ](https://github.com/RickAndMSFT/Async-ASP.NET)上[GitHub](https://github.com/)站点。 存储库包含三个项目：

- *WebAppAsync*:ASP.NET Web 窗体使用的项目的 Web API **WebAPIpwg**服务。 本教程是从该代码的大多数项目。
- *WebAPIpgw*:实现的 ASP.NET MVC 4 Web API 项目`Products, Gizmos and Widgets`控制器。 它提供的数据*WebAppAsync*项目并*Mvc4Async*项目。
- *Mvc4Async*:包含在另一个教程中使用的代码的 ASP.NET MVC 4 项目。 Web API 调用**WebAPIpwg**服务。

## <a id="GizmosSynch"></a>  Gizmos，请同步页

 下面的代码演示`Page_Load`用于显示 gizmos，请一系列的同步方法。 （对于本文，零件是虚构的机械设备。） 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

下面的代码演示`GetGizmos`零件服务的方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos`方法将 URI 传递到 ASP.NET Web API HTTP 服务返回的 gizmos，请数据列表。 *WebAPIpgw*项目中包含的 Web API 实现`gizmos, widget`和`product`控制器。  
下图显示了示例项目从 gizmos，请页。

![Gizmos，请](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  创建异步 gizmos，请页

该示例使用新[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)并[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字 （在.NET 4.5 和 Visual Studio 2012 中提供） 让编译器负责维护复杂的转换的必要条件异步编程。 编译器可用于编写代码使用 C# 的同步的控制流构造，编译器会自动应用需使用回调，以避免阻塞线程的转换。

ASP.NET 异步页面必须包括[页上](https://msdn.microsoft.com/library/ydy4x04a.aspx)指令与`Async`属性设置为"true"。 下面的代码演示[页上](https://msdn.microsoft.com/library/ydy4x04a.aspx)指令与`Async`属性设置为"true"的*GizmosAsync.aspx*页。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

下面的代码演示`Gizmos`同步`Page_Load`方法和`GizmosAsync`异步页面。 如果你的浏览器支持[HTML 5&lt;标记&gt;元素](http://www.w3.org/wiki/HTML/Elements/mark)，您会看到在更改`GizmosAsync`黄色突出显示框中。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

异步版本：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 应用了以下更改，以允许`GizmosAsync`页是异步的。

- [页上](https://msdn.microsoft.com/library/ydy4x04a.aspx)指令必须具有`Async`属性设置为"true"。
- `RegisterAsyncTask`方法用来注册异步任务包含以异步方式运行的代码。
- 新`GetGizmosSvcAsync`方法将标有[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字，它会告知编译器将生成的正文部分的回调并自动创建`Task`返回。
- &quot;异步&quot;追加到异步方法名称。 追加"Async"不是必需的但编写异步方法时，是的约定。
- 新的返回类型`GetGizmosSvcAsync`方法是`Task`。 返回类型`Task`表示正在进行的工作，并提供方法的调用方与用来等待异步操作完成的句柄。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字已应用于 web 服务调用。
- 调用了异步 web 服务 API (`GetGizmosAsync`)。

内部`GetGizmosSvcAsync`方法正文另一种异步方法，`GetGizmosAsync`调用。 `GetGizmosAsync` 立即返回`Task<List<Gizmo>>`，最终会完成时有可用的数据。 因为您不想要将零件数据之前执行任何其他操作，该代码等待任务 (使用**await**关键字)。 可以使用**await**关键字仅在方法中使用批注**异步**关键字。

**Await**关键字不会阻止线程，直到任务完成。 它以在任务中，回调注册方法的剩余部分，并立即返回。 等待的任务最终完成时，它将调用该回调，并因此恢复中断的位置的方法权限执行。 有关使用的详细信息[await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)并[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字并[任务](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx)命名空间，请参阅[异步引用](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async)。

下面的代码演示`GetGizmos`和`GetGizmosAsync`方法。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 异步更改是对所做的内容相似**GizmosAsync**上面。 

- 方法签名使用批注[异步](https://msdn.microsoft.com/library/hh156513(VS.110).aspx)关键字，返回类型已更改为`Task<List<Gizmo>>`，和*异步*追加到方法名称。
- 异步[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)类使用而不是同步[WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx)类。
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx)关键字已应用于[HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx)异步方法。

下图显示了异步零件视图。

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

浏览器表示法的 gizmos，请数据是相同的同步调用创建的视图。 唯一的区别是异步版本可能是在重负载下的性能更高。

## <a name="registerasynctask-notes"></a>RegisterAsyncTask 说明

方法将与`RegisterAsyncTask`后立即会再运行[PreRender](https://msdn.microsoft.com/library/ms178472.aspx)。 您还可以使用 async void 页面事件直接，如下面的代码中所示：

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Async void 事件的缺点是，开发人员不再包含对事件时执行的完全控制。 例如，如果这两个.aspx 和。Master 定义`Page_Load`事件和一个或这两个值异步的就不能保证执行顺序。 非事件处理程序的相同 indeterminiate 顺序 (如`async void Button_Click`) 应用。 对于大多数开发人员，这应是一种可接受的但那些需要进行完全控制的执行顺序只应使用等 Api`RegisterAsyncTask`的使用方法返回任务对象。

## <a id="Parallel"></a>  并行执行多个操作

操作必须执行几个独立的操作时，异步方法具有通过同步方法的一个明显的优势。 在示例中提供，同步页*PWG.aspx*（适用于产品、 小组件和 gizmos，请） 会显示三个 web 服务调用，以获取的产品、 小组件和 gizmos，请列表的结果。 [ASP.NET Web API](../../../web-api/index.md)项目提供了这些服务使用[Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx)模拟延迟或慢速网络调用。 当延迟设置为 500 毫秒，异步*PWGasync.aspx*页需稍有超过 500 毫秒就能完成的同步`PWG`版本高于 1500 毫秒。 同步*PWG.aspx*页显示在下面的代码。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

异步`PWGasync`背后的代码如下所示。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

下图显示了异步返回的视图*PWGasync.aspx*页。

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>  使用取消标记

返回的异步方法`Task`是否可取消，即它们采用[CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx)参数在与提供`AsyncTimeout`的属性[页](https://msdn.microsoft.com/library/ydy4x04a.aspx)指令。 下面的代码演示*GizmosCancelAsync.aspx*网页上秒的超时。

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

下面的代码演示*GizmosCancelAsync.aspx.cs*文件。

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

中提供的示例应用程序，选择*GizmosCancelAsync*链接调用*GizmosCancelAsync.aspx*页上，并演示异步调用的 （通过超时） 取消。 由于延迟时间是随机的范围内，你可能需要刷新页面多次才能收到超时错误消息。

## <a id="ServerConfig"></a>  针对高并发/高延迟 Web 服务调用的服务器配置

若要实现异步 web 应用程序的好处，可能需要对默认服务器配置进行一些更改。 请记住以下配置时的考虑和压力测试异步 web 应用程序中。

- Windows 7、 Windows Vista、 Window 8 和所有 Windows 客户端操作系统具有最多 10 个并发请求。 你将需要 Windows 服务器操作系统，若要查看在高负载下的异步方法的优势。
- 向 IIS 注册.NET 4.5，从提升的命令提示符使用以下命令：  
  %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis-i  
  请参阅[ASP.NET IIS 注册工具 (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- 你可能需要增加[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)队列限制从 1,000 到 5,000 个的默认值。 如果设置得太低，可能会看到[HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture)拒绝请求的 HTTP 503 状态。 若要更改 HTTP.sys 队列限制：

    - 打开 IIS 管理器并导航到应用程序池窗格中。
    - 在目标应用程序池上右键单击并选择**高级设置**。  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - 在中**高级设置**对话框中，更改*队列长度*从 1,000 到 5,000。  
        ![队列长度](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
  请注意，在更高版本，映像的.NET framework 被列为 v4.0，即使应用程序池使用的.NET 4.5 也是如此。 若要了解这种差异，请参阅以下：

- [.NET 版本控制和多重目标 —.NET 4.5 是.NET 4.0 的就地升级](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
- [如何设置要使用 ASP.NET 3.5 而不是 2.0 的 IIS 应用程序或应用程序池](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
- [.NET Framework 版本和依赖关系](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)

- 如果你的应用程序使用 web 服务或 System.NET 使用后端通过 HTTP 进行通信，可能需要增加[connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx)元素。 对于 ASP.NET 应用程序，这被受 12 倍的 Cpu 数的自动配置功能。 这意味着，在四处理器，您可以最多 12 \* 4 = 48 到 IP 终结点的并发连接。 因为这与[autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx)，增加的最简单办法`maxconnection`应用程序是 ASP.NET 中设置[System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx)以编程方式在从`Application_Start`中的方法*global.asax*文件。 请参阅下载有关示例的示例。
- 在.NET 4.5 中，默认值为 5000 [maxconcurrentrequestspercpu 配置](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)可以正常工作。

## <a name="contributors"></a>参与者

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
