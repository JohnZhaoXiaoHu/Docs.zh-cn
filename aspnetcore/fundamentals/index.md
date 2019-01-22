---
title: ASP.NET Core 基础知识
author: rick-anderson
description: 了解生成 ASP.NET Core 应用的基础概念。
ms.author: riande
ms.custom: mvc
ms.date: 01/06/2019
uid: fundamentals/index
ms.openlocfilehash: a56beebd796448705c7b84f47699e9739f451419
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099229"
---
# <a name="aspnet-core-fundamentals"></a>ASP.NET Core 基础知识

ASP.NET Core 应用是一个控制台应用，它在其 `Program.Main` 方法中创建 Web 服务器。 `Main` 方法是应用的托管入口点：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs)]

.NET Core 主机：

* 加载 [.NET Core 运行时](https://github.com/dotnet/coreclr)。
* 使用第一个命令行参数作为包含入口点 (`Main`) 的托管二进制文件的路径，并开始执行代码。

`Main` 方法调用 [WebHost.CreateDefaultBuilder](xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*)，后者按照[生成器模式](https://wikipedia.org/wiki/Builder_pattern)来创建 Web 主机。 生成器包含定义 Web 服务器（例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>）和启动类 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。 在前面的例子中，自动分配了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。 ASP.NET Core 的 Web 主机尝试在 [Internet Information Services (IIS)](https://www.iis.net/)（若有）上运行。 对于其他 Web 服务器（如 [HTTP.sys](xref:fundamentals/servers/httpsys)），可通过调用相应的扩展方法来使用。 [启动](#startup)部分进一步介绍了 `UseStartup`。

<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 是 `WebHost.CreateDefaultBuilder` 调用的返回类型，它提供了许多可选方法。 其中的一些方法包括用于在 HTTP.sys 中托管应用的 `UseHttpSys`，以及用于指定根内容目录的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 和 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法生成 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 对象，该对象托管应用并开始侦听 HTTP 请求。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs)]

.NET Core 主机：

* 加载 [.NET Core 运行时](https://github.com/dotnet/coreclr)。
* 使用第一个命令行参数作为包含入口点 (`Main`) 的托管二进制文件的路径，并开始执行代码。

`Main` 方法使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>，后者按照[生成器模式](https://wikipedia.org/wiki/Builder_pattern)来创建 Web 主机。 生成器提供定义 Web 服务器（例如，<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>）和启动类 (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderExtensions.UseStartup*>) 的方法。 在前面的示例中，使用了 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器。 对于其他 Web 服务器（如 [HTTP.sys](xref:fundamentals/servers/httpsys)），可通过调用相应的扩展方法来使用。 [启动](#startup)部分进一步介绍了 `UseStartup`。

`WebHostBuilder` 提供了许多可选方法，其中包括用于在 IIS 和 IIS Express 中进行托管的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*>，以及用于指定根内容目录的 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseContentRoot*>。 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder.Build*> 和 <xref:Microsoft.AspNetCore.Hosting.WebHostExtensions.Run*> 方法生成 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 对象，该对象托管应用并开始侦听 HTTP 请求。

::: moniker-end

## <a name="startup"></a>启动

`WebHostBuilder` 上的 `UseStartup` 方法为你的应用指定 `Startup` 类：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Program.cs?highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Program.cs?highlight=7)]

::: moniker-end

`Startup` 类用于在其中配置应用所需的任何服务和定义请求处理管道。 `Startup` 类必须是公共类，通常包含以下方法。 `Startup.ConfigureServices` 是可选的。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/snapshots/2.x/Startup.cs)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/snapshots/1.x/Startup.cs)]

::: moniker-end

<xref:Microsoft.AspNetCore.Hosting.IStartup.ConfigureServices*> 定义应用所使用的[服务](#dependency-injection-services)（如 ASP.NET Core MVC、Entity Framework Core 和标识）。 <xref:Microsoft.AspNetCore.Hosting.IStartup.Configure*> 定义在请求管道中调用的[中间件](xref:fundamentals/middleware/index)。

有关更多信息，请参见<xref:fundamentals/startup>。

## <a name="content-root"></a>内容根

内容根是应用所使用的任何内容的基路径，如 [Razor Pages](xref:razor-pages/index)、MVC 视图和静态资产。 默认情况下，内容根位置与用于托管应用的可执行文件的应用基路径相同。

## <a name="web-root-webroot"></a>Web 根 (webroot)

应用的 webroot 是项目中的目录，其中包含公共资源、CSS 等静态资源、JavaScript 和图形文件。 wwwroot 默认为 webroot。

对于 Razor (.cshtml) 文件，波浪号斜杠 `~/` 指向 webroot。 以 `~/` 开头的路径称为虚拟路径。

## <a name="dependency-injection-services"></a>依赖关系注入（服务）

服务是应用中常用的组件。 可以通过[依存关系注入 (DI)](xref:fundamentals/dependency-injection) 来获取服务。 ASP.NET Core 包括默认支持[构造函数注入](xref:mvc/controllers/dependency-injection#constructor-injection)的本机控制反转 (IoC) 容器。 可根据需要替换默认容器。 DI 除了具备[松散耦合优势](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation)以外，还可以使服务（例如[日志记录](xref:fundamentals/logging/index)）在整个应用中可用。

有关更多信息，请参见<xref:fundamentals/dependency-injection>。

## <a name="middleware"></a>中间件

在 ASP.NET Core 中，使用[中间件](xref:fundamentals/middleware/index)来撰写请求管道。 ASP.NET Core 中间件在 `HttpContext` 上执行异步操作，然后调用管道中的下一个中间件或终止请求。

按照惯例，通过在 `Configure` 方法中调用 `UseXYZ` 扩展方法，向管道添加名为“XYZ”的中间件组件。

ASP.NET Core 包含一组丰富的内置中间件，你也可以编写自己的自定义中间件。 ASP.NET Core 应用中支持 [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)，它将 Web 应用与 Web 服务器分离。

有关详细信息，请参阅 <xref:fundamentals/middleware/index> 和 <xref:fundamentals/owin>。

::: moniker range=">= aspnetcore-2.1"

## <a name="initiate-http-requests"></a>启动 HTTP 请求

<xref:System.Net.Http.IHttpClientFactory> 可访问 <xref:System.Net.Http.HttpClient> 实例以发出 HTTP 请求。

有关更多信息，请参见<xref:fundamentals/http-requests>。

::: moniker-end

## <a name="environments"></a>环境

环境（如“开发”环境和“生产”环境）是 ASP.NET Core 的高级概念，可使用环境变量、设置文件和命令行参数进行设置。

有关更多信息，请参见<xref:fundamentals/environments>。

## <a name="hosting"></a>宿主

ASP.NET Core 应用可配置和启动一个*主机*，负责应用启动和生存期管理。

有关更多信息，请参见<xref:fundamentals/host/index>。

## <a name="servers"></a>服务器

ASP.NET Core 托管模型不直接侦听请求。 托管模型依赖 HTTP 服务器实现将请求转发到应用。

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core 提供以下服务器实现：

* [Kestrel](xref:fundamentals/servers/kestrel) 服务器是跨平台托管 Web 服务器。 Kestrel 通常使用 [IIS](https://www.iis.net/) 在反向代理配置中运行。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。
* IIS HTTP 服务器 (`IISHttpServer`) 是 IIS 的[进程内服务器](xref:fundamentals/servers/index#in-process-hosting-model)。
* [HTTP.sys](xref:fundamentals/servers/httpsys) 服务器是 Windows 上适用于 ASP.NET Core 的 Web 服务器。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core 使用 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器实现。 Kestrel 是跨平台托管 Web 服务器。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core 使用 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器实现。 Kestrel 是跨平台托管 Web 服务器。 Kestrel 通常使用 [Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/) 在反向代理配置中运行。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

ASP.NET Core 提供以下服务器实现：

* [Kestrel](xref:fundamentals/servers/kestrel) 服务器是跨平台托管 Web 服务器。 Kestrel 通常使用 [IIS](https://www.iis.net/) 在反向代理配置中运行。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。
* [HTTP.sys](xref:fundamentals/servers/httpsys) 服务器是 Windows 上适用于 ASP.NET Core 的 Web 服务器。

# <a name="macostabmacos"></a>[macOS](#tab/macos)

ASP.NET Core 使用 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器实现。 Kestrel 是跨平台托管 Web 服务器。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

ASP.NET Core 使用 [Kestrel](xref:fundamentals/servers/kestrel) Web 服务器实现。 Kestrel 是跨平台托管 Web 服务器。 Kestrel 通常使用 [Nginx](http://nginx.org) 或 [Apache](https://httpd.apache.org/) 在反向代理配置中运行。 在 ASP.NET Core 2.0 或更高版本中，Kestrel 也可作为面向公众的边缘服务器运行，直接向 Internet 公开。

---

::: moniker-end

有关更多信息，请参见<xref:fundamentals/servers/index>。

## <a name="configuration"></a>配置

ASP.NET Core 基于名称/值对使用配置模型。 配置模型不基于 <xref:System.Configuration> 或 *web.config*。配置从一组有序的配置提供程序获取设置。 内置配置提供程序支持各种文件格式（XML、 JSON、INI）、环境变量和命令行参数。 也可以编写你自己的自定义配置提供程序。

有关更多信息，请参见<xref:fundamentals/configuration/index>。

## <a name="logging"></a>日志记录

ASP.NET Core 支持适用于各种日志记录提供程序的日志记录 API。 内置提供程序支持向一个或多个目标发送日志。 可使用第三方记录框架。

有关更多信息，请参见<xref:fundamentals/logging/index>。

## <a name="error-handling"></a>错误处理

ASP.NET Core 的内置方案可处理应用中的错误，包括开发人员异常页、自定义错误页、静态状态代码页和启动异常处理。

有关更多信息，请参见<xref:fundamentals/error-handling>。

## <a name="routing"></a>路由

ASP.NET Core 提供将应用请求路由到路由处理程序的方案。

有关更多信息，请参见<xref:fundamentals/routing>。

## <a name="background-tasks"></a>后台任务

后台任务作为*托管服务*实现。 托管服务是一个类，具有实现 <xref:Microsoft.Extensions.Hosting.IHostedService> 接口的后台任务逻辑。

有关更多信息，请参见<xref:fundamentals/host/hosted-services>。

## <a name="access-httpcontext"></a>访问 HttpContext

用 Razor Pages 和 MVC 处理请求时，`HttpContext` 自动可用。 当 `HttpContext` 不可立即使用时，可通过 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 接口及其默认实现 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 访问 `HttpContext`。

有关更多信息，请参见<xref:fundamentals/httpcontext>。
