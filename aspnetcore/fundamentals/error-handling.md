---
title: 处理 ASP.NET Core 中的错误
author: tdykstra
description: 了解如何处理 ASP.NET Core 应用中的错误。
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: fundamentals/error-handling
ms.openlocfilehash: f4358cba81d2aa47a26f90a8d5f4e77310bcad00
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637633"
---
# <a name="handle-errors-in-aspnet-core"></a>处理 ASP.NET Core 中的错误

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)

本文介绍了处理 ASP.NET Core 应用中常见错误的一些方法。

[查看或下载示例代码](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample)（[如何下载](xref:index#how-to-download-a-sample)）

## <a name="the-developer-exception-page"></a>开发人员异常页

::: moniker range=">= aspnetcore-2.1"

要将应用配置为显示有关异常的详细信息的页面，请使用开发人员异常页。 该页面通过 [Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中的 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包提供。 向 `Startup.Configure` 方法添加代码行：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

要将应用配置为显示有关异常的详细信息的页面，请使用开发人员异常页。 该页面通过 [Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)中的 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包提供。 向 `Startup.Configure` 方法添加代码行：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

要将应用配置为显示有关异常的详细信息的页面，请使用开发人员异常页。 该页面通过在项目文件中添加 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包的包引用提供。 向 `Startup.Configure` 方法添加代码行：

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

将对 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) 的调用放在要对其捕获异常的任何中间件（例如，`app.UseMvc`）前面。

> [!WARNING]
> 仅当应用程序在开发环境中运行时才启用开发人员异常页。 否则当应用程序在生产环境中运行时，详细的异常信息会向公众泄露 了解[环境配置](xref:fundamentals/environments)。

要查看开发人员异常页，请将环境设置为 `Development`，运行示例应用，并向应用的基 URL 添加 `?throw=true`。 该页面包括几个选项卡，这些选项卡中包含关于异常和请求的信息。 第一个选项卡包括堆栈跟踪：

![堆栈跟踪](error-handling/_static/developer-exception-page.png)

下一个选项卡显示查询字符串参数（如有）：

![查询字符串参数](error-handling/_static/developer-exception-page-query.png)

如果请求具有 cookie，它们在“Cookie”选项卡上显示。标头出现在最后一个选项卡中：

![标头](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a>配置自定义异常处理页

配置当应用未在 `Development` 环境中运行时要使用的异常处理程序页：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

在 Razor Pages 应用中，[dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages 模板在 Pages 文件夹中提供“错误”页面和错误 `PageModel` 类。

在 MVC 应用中，不要使用 HTTP 方法特性（如 `HttpGet`）修饰错误处理程序操作方法。 显式谓词可阻止某些请求访问方法。 允许匿名访问方法，以便未经身份验证的用户能够接收错误视图。

例如，以下错误处理程序方法由 [dotnet new](/dotnet/core/tools/dotnet-new) MVC 模板提供并在主控制器中显示：

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a>配置状态代码页

默认情况下，应用不会为 HTTP 状态代码提供丰富状态代码页，例如 404 未找到。 要提供状态代码页，请使用状态代码页中间件。

::: moniker range=">= aspnetcore-2.1"

该中间件通过 [Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中的 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包提供。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

该中间件通过 [Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)中的 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包提供。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

该中间件通过在项目文件中添加 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 包的包引用提供。

::: moniker-end

向 `Startup.Configure` 方法添加代码行：

```csharp
app.UseStatusCodePages();
```

应在管道中的请求处理中间件之前调用 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>（例如，静态文件中间件和 MVC 中间件）。

默认情况下，状态代码页中间件为常见状态代码（如 404）添加纯文本处理程序：

![404 页面](error-handling/_static/default-404-status-code.png)

该中间件支持多种扩展方法。 一种方法采用 Lambda 表达式：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

`UseStatusCodePages` 重载需要使用内容类型和格式字符串：

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```
### <a name="redirect-re-execute-extension-methods"></a>重定向重新执行扩展方法

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>：

* 向客户端发送“302 - 已找到”状态代码。
* 将客户端重定向到 URL 模板中的位置。 

该模板可能包括状态代码的 `{0}` 占位符。 模板必须以正斜杠 (`/`) 开头。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>：

* 向客户端返回原始状态代码。
* 指定应使用备用路径重新执行请求管道，从而生成响应正文。 

该模板可能包括状态代码的 `{0}` 占位符。 模板必须以正斜杠 (`/`) 开头。

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

可禁用 Razor 页处理程序方法或 MVC 控制器中的特定请求的状态代码页。 要禁用状态代码页，请尝试从请求的 [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) 集合中检索 [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature)，并在功能可用时禁用该功能：

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

若要在应用中使用指向终结点的 `UseStatusCodePages*` 重载，请为终结点创建 MVC 视图或 Razor Page。 例如，Razor Pages 应用的 [dotnet new](/dotnet/core/tools/dotnet-new) 模板将生成以下页面和页面模型类：

Error.cshtml：

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

Error.cshtml.cs：

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>异常处理代码

异常处理页中的代码可能会引发异常。 建议在生产错误页面中包含纯静态内容。

此外还要注意，一旦发送响应的标头，则无法更改响应的状态代码，也不能运行任何异常页或处理程序。 必须完成响应或中止连接。

## <a name="server-exception-handling"></a>服务器异常处理

除应用程序中的异常处理逻辑外，托管应用程序的服务器[服务器](xref:fundamentals/servers/index)也会执行一些异常处理。 如果服务器在发送标头之前捕获异常，服务器将发送没有正文的 500 内部服务器错误响应。 如果服务器在已发送标头后捕获异常，服务器会关闭连接。 应用程序无法处理的请求将由服务器进行处理。 发生的任何异常都将由服务器进行处理。 任何配置的自定义错误页面或异常处理中间件或筛选器都不会影响此行为。

## <a name="startup-exception-handling"></a>启动异常处理

应用程序启动期间发生的异常仅可在承载层进行处理。 利用 [Web](xref:fundamentals/host/web-host) 主机，你可以使用 `captureStartupErrors` 和 `detailedErrors` 键[配置主机在响应启动期间出现错误时的行为方式](xref:fundamentals/host/web-host#detailed-errors)。

仅当捕获的启动错误发生在主机地址/端口绑定之后，承载层才会为该错误显示错误页。 如果绑定因任何原因而失败，则承载层会记录关键异常，dotnet 进程崩溃，且在 [Kestrel](xref:fundamentals/servers/kestrel) 服务器上运行应用时，不会显示任何错误页。

在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上运行应用时，如果无法启动进程，[ASP.NET Core 模块](xref:host-and-deploy/aspnet-core-module)将返回 502.5 进程失败。 要了解在通过 IIS 托管时如何排查启动问题，请参阅 <xref:host-and-deploy/iis/troubleshoot>。 要了解如何排查 Azure 应用服务的启动问题，请参阅 <xref:host-and-deploy/azure-apps/troubleshoot>。

## <a name="aspnet-core-mvc-error-handling"></a>ASP.NET Core MVC 错误处理

[MVC](xref:mvc/overview) 应用还有一些其他的错误处理选项，例如配置异常筛选器和执行模型验证。

### <a name="exception-filters"></a>异常筛选器

在 MVC 应用中，异常筛选器可以进行全局配置，也可以为每个控制器或每个操作单独配置。 这些筛选器处理在执行控制器操作或其他筛选器时出现的任何未处理的异常。 不会以其他方式调用这些筛选器。 要了解详细信息，请参阅[筛选器](xref:mvc/controllers/filters)。

> [!TIP]
> 异常筛选器非常适用于捕获 MVC 操作内出现的异常，但灵活性不如错误处理中间件。 一般情况下最好使用中间件；仅在需要根据所选 MVC 操作以不同方式执行错误处理时，才使用筛选器。

### <a name="handling-model-state-errors"></a>处理模型状态错误

[模型验证](xref:mvc/models/validation)在每个控制器操作被调用之前发生，操作方法负责检查 `ModelState.IsValid` 并相应地作出反应。

某些应用使用标准约定处理模型验证错误，在这种情况下，使用[筛选器](xref:mvc/controllers/filters)可以更好地实施这种策略。 你需要使用无效模型状态测试操作的行为。 查看[测试控制器逻辑](xref:mvc/controllers/testing)了解详情。

## <a name="additional-resources"></a>其他资源

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
