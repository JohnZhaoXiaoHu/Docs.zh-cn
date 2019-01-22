---
title: 启用 ASP.NET Core 中的跨域请求 (CORS)
author: rick-anderson
description: 了解如何作为一种标准允许或拒绝在 ASP.NET Core 应用中的跨域请求的 CORS。
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458538"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a>启用 ASP.NET Core 中的跨域请求 (CORS)

作者：[Mike Wasson](https://github.com/mikewasson)、[Shayne Boyer](https://twitter.com/spboyer) 和 [Tom Dykstra](https://github.com/tdykstra)

浏览器安全性以防止网页从发出请求到不同的域的 web 页面提供服务。 此限制称为*同域策略*。 同域策略可阻止恶意站点读取另一个站点中的敏感数据。 有时，你可能想要允许其他站点对您的应用程序进行跨域请求。

[跨域资源共享](https://www.w3.org/TR/cors/)(CORS) 是一种 W3C 标准，允许服务器放宽同源策略。 使用 CORS，服务器可以在显式允许某些跨域请求时拒绝其他跨域请求。 CORS 是更安全、 更灵活比早期技术，如[JSONP](https://wikipedia.org/wiki/JSONP)。 本主题演示如何在 ASP.NET Core 应用中启用 CORS。

## <a name="same-origin"></a>相同的原点

如果它们具有相同方案、 主机和端口，两个 Url 将具有相同的原点 ([RFC 6454](https://tools.ietf.org/html/rfc6454))。

以下两个 Url 具有相同的原点：

* `https://example.com/foo.html`
* `https://example.com/bar.html`

这些 Url 具有不同的源，比以前的两个 Url:

* `https://example.net` &ndash; 不同的域
* `https://www.example.com/foo.html` &ndash; 不同的子域
* `http://example.com/foo.html` &ndash; 不同的方案
* `https://example.com:9000/foo.html` &ndash; 不同的端口

> [!NOTE]
> 比较来源时，Internet Explorer 不会考虑该端口。

## <a name="register-cors-services"></a>注册的 CORS 服务

::: moniker range=">= aspnetcore-2.1"

引用[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)或添加到的包引用[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)包。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

引用[Microsoft.AspNetCore.All 元包](xref:fundamentals/metapackage)或添加到的包引用[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)包。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

添加到包引用[Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/)包。

::: moniker-end

调用<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>在`Startup.ConfigureServices`将 CORS 服务添加到应用程序的服务容器：

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a>启用 CORS

注册之后的 CORS 服务，使用以下方法之一在 ASP.NET Core 应用中启用 CORS:

* [CORS 中间件](#enable-cors-with-cors-middleware)&ndash;全局到通过中间件应用程序的应用的 CORS 策略。
* [在 MVC 中的 CORS](#enable-cors-in-mvc) &ndash;每个操作或每个控制器应用 CORS 策略。 不会使用 CORS 中间件。

### <a name="enable-cors-with-cors-middleware"></a>启用 CORS 的 CORS 中间件

CORS 中间件处理向应用程序的跨域请求。 若要在请求处理管道中启用 CORS 中间件，请调用<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>中的扩展方法`Startup.Configure`。

CORS 中间件必须位于之前定义的任何终结点应用程序中你想要支持跨域请求 (例如，在对调用之前`UseMvc`MVC/Razor 页中间件)。

一个*跨域策略*添加 CORS 中间件使用时可以指定<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>类。 有两种方法用于定义 CORS 策略：

* 调用`UseCors`lambda:

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  Lambda 采用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>对象。 [配置选项](#cors-policy-options)，如`WithOrigins`，本主题后面所述。 在前面的示例中，该策略允许跨域请求从`https://example.com`和任何其他来源。

  不含尾部斜杠，必须指定的 URL (`/`)。 如果 URL 以终止`/`，则比较操作返回`false`并返回无标头。

  `CorsPolicyBuilder` 具有 fluent API，因此您可以将方法调用链接：

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* 定义一个或多个命名的 CORS 策略并按名称在运行时选择的策略。 下面的示例添加名为的用户定义 CORS 策略*AllowSpecificOrigin*。 若要选择的策略，将名称传递给`UseCors`:

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a>启用在 MVC 中的 CORS

或者，可以使用 MVC 应用特定 CORS 策略针对每个操作或控制器。 在使用 MVC 启用 CORS，使用了已注册的 CORS 服务。 不会使用 CORS 中间件。

### <a name="per-action"></a>每个操作

若要指定特定操作的 CORS 策略，添加[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性为该操作。 指定策略名称。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a>每个控制器

若要指定特定控制器的 CORS 策略，添加[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)属性到控制器类。 指定策略名称。

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

优先顺序是：

1. action
1. 控制器

### <a name="disable-cors"></a>禁用 CORS

若要禁用 CORS 的控制器或操作，请使用[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)属性：

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a>CORS 策略选项

本部分介绍您可以在 CORS 策略设置的各种选项。 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>方法调用`Startup.ConfigureServices`。

* [设置允许的来源](#set-the-allowed-origins)
* [设置允许的 HTTP 方法](#set-the-allowed-http-methods)
* [设置允许的请求标头](#set-the-allowed-request-headers)
* [设置公开的响应标头](#set-the-exposed-response-headers)
* [跨域请求中的凭据](#credentials-in-cross-origin-requests)
* [将预检过期时间设置](#set-the-preflight-expiration-time)

有关一些选项，可能会有帮助读取[如何 CORS 工作](#how-cors-works)部分第一次。

### <a name="set-the-allowed-origins"></a>设置允许的来源

ASP.NET Core MVC 中的 CORS 中间件具有几种方法来指定允许的来源：

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; 允许指定一个或多个 Url。 URL 可能包括方案、 主机名和端口不包含任何路径信息。 例如 `https://example.com`。 不含尾部斜杠，必须指定的 URL (`/`)。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允许来自任何方案使用所有来源的 CORS 请求 (`http`或`https`)。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  允许任何来源的请求之前，请仔细考虑。 允许任何来源的请求意味着*的任何网站*可以对您的应用程序进行跨域请求。

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的配置，可能会导致跨站点请求伪造。 使用这两种方法配置应用程序时，CORS 服务返回了无效的 CORS 响应。

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > 指定`AllowAnyOrigin`和`AllowCredentials`是不安全的配置，可能会导致跨站点请求伪造。 请考虑指定确切的来源列表，如果客户端必须先授权本身访问服务器资源。

  ::: moniker-end

  此设置会影响的预检请求和`Access-Control-Allow-Origin`标头。 有关详细信息，请参阅[预检请求](#preflight-requests)部分。

::: moniker range=">= aspnetcore-2.0"

* <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 集<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略允许来源以匹配配置的通配符域，如果允许原点在评估时的函数的属性。

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a>设置允许的 HTTP 方法

若要允许所有 HTTP 方法，调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

此设置会影响的预检请求和`Access-Control-Allow-Methods`标头。 有关详细信息，请参阅[预检请求](#preflight-requests)部分。

### <a name="set-the-allowed-request-headers"></a>设置允许的请求标头

若要允许特定标头发送在 CORS 请求中，调用*创作请求标头*，调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>并指定允许的标头：

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允许所有作者的请求标头调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

此设置会影响的预检请求和`Access-Control-Request-Headers`标头。 有关详细信息，请参阅[预检请求](#preflight-requests)部分。

::: moniker range=">= aspnetcore-2.2"

CORS 中间件策略匹配所指定的特定标头`WithHeaders`时，才可以发送标头`Access-Control-Request-Headers`与中所述的标头完全匹配`WithHeaders`。

例如，假设配置，如下所示的应用：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中间件拒绝包含以下请求标头的预检请求，因为`Content-Language`([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 中未列出`WithHeaders`:

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

应用程序返回*200 确定*响应但不会发送回的 CORS 标头。 因此，在浏览器不会尝试执行跨域请求。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

CORS 中间件始终允许在四个标头`Access-Control-Request-Headers`发送而不考虑 CorsPolicy.Headers 中配置的值。 此列表的标头包括：

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

例如，假设配置，如下所示的应用：

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

CORS 中间件成功响应以下请求标头的预检请求因为`Content-Language`始终是已列入允许列表：

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a>设置公开的响应标头

默认情况下，在浏览器不会公开所有向应用程序的响应标头。 有关详细信息，请参阅[W3C 跨域资源共享 （术语）： 简单的响应标头](https://www.w3.org/TR/cors/#simple-response-header)。

默认情况下可用的响应标头是：

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

CORS 规范调用这些标头*简单响应标头*。 若要使其他标头可用于应用程序，请调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a>跨域请求中的凭据

凭据要求特殊处理 CORS 请求中。 默认情况下，在浏览器不会发送与跨域请求的凭据。 凭据包括 cookie 和 HTTP 身份验证方案。 若要将发送具有跨源请求的凭据，客户端必须设置`XMLHttpRequest.withCredentials`到`true`。

使用`XMLHttpRequest`直接：

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

在 jQuery 中：

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

此外，服务器必须允许凭据。 若要允许跨域凭据，调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

HTTP 响应包括`Access-Control-Allow-Credentials`标头，服务器允许跨域请求凭据将告诉浏览器。

如果浏览器发送凭据，但响应不包含有效`Access-Control-Allow-Credentials`标头，在浏览器不会公开的响应应用程序中，并且跨域请求失败。

允许跨域凭据时要小心。 在另一个域的网站可以向应用而无需在用户不知情的用户的名义发送登录的用户的凭据。

CORS 规范还会说明该设置到来源`"*"`（所有来源） 是无效的如果`Access-Control-Allow-Credentials`标头。

### <a name="preflight-requests"></a>预检请求

对于某些 CORS 请求，在浏览器发出实际请求之前发送其他请求。 调用此请求*预检请求*。 在浏览器可以跳过预检请求，如果满足以下条件：

* 请求方法是 GET、 HEAD 或 POST。
* 应用程序不会设置以外的其他请求标头`Accept`， `Accept-Language`， `Content-Language`， `Content-Type`，或`Last-Event-ID`。
* `Content-Type`标头，如果设置，具有以下值之一：
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

在请求标头的规则集的客户端请求适用于通过调用来设置应用程序的标头`setRequestHeader`上`XMLHttpRequest`对象。 CORS 规范调用这些标头*创作请求标头*。 该规则不能应用于标头设置可以在浏览器，如`User-Agent`， `Host`，或`Content-Length`。

下面是预检请求的示例：

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

预检请求使用 HTTP OPTIONS 方法。 它包括两个特殊的标头：

* `Access-Control-Request-Method`: 将用作实际请求的 HTTP 方法。
* `Access-Control-Request-Headers`： 设置实际请求中应用的请求标头的列表。 如前面所述，这不包括标头的浏览器设置，如`User-Agent`。

CORS 预检请求可能包括`Access-Control-Request-Headers`标头，指示服务器与实际请求一起发送的标头。

若要允许的特定标头，请调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

若要允许所有作者的请求标头调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

浏览器不是在设置方式完全一致`Access-Control-Request-Headers`。 如果将标头设置到的任何内容以外`"*"`(或使用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>)，至少应包含`Accept`， `Content-Type`，和`Origin`，以及你想要支持任何自定义标头。

下面是预检请求 （假设服务器允许请求） 的示例响应：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

响应包括`Access-Control-Allow-Methods`标头，其中列出了允许的方法和 （可选）`Access-Control-Allow-Headers`标头，其中列出了允许的标头。 如果预检请求成功，则浏览器发送实际请求。

如果预检请求被拒绝，该应用程序将返回*200 确定*响应但不会发送回的 CORS 标头。 因此，在浏览器不会尝试执行跨域请求。

### <a name="set-the-preflight-expiration-time"></a>将预检过期时间设置

`Access-Control-Max-Age`标头指定可以缓存预检请求的响应的时间。 若要设置此标头，请调用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a>CORS 的工作原理

本部分介绍在 CORS 请求的 HTTP 消息级别中会发生什么情况。 请务必了解，以便可以正确配置和调试时出现意外的行为的 CORS 策略 CORS 的工作原理。

CORS 规范引入了几个新的 HTTP 标头启用跨域请求。 如果浏览器支持 CORS，则将设置自动跨域请求这些标头。 自定义 JavaScript 代码不需要启用 CORS。

下面是跨域请求的示例。 `Origin`标头提供发出请求的站点的域：

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

如果服务器允许该请求，它会设置`Access-Control-Allow-Origin`在响应中的标头。 此标头的值或者与匹配`Origin`请求标头或为通配符值`"*"`，允许任何源的含义：

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

如果响应不包括`Access-Control-Allow-Origin`标头，跨域请求会失败。 具体而言，在浏览器不允许该请求。 即使服务器返回成功的响应，请在浏览器不使响应可供客户端应用程序。

## <a name="additional-resources"></a>其他资源

* [跨域资源共享 (CORS)](https://developer.mozilla.org/docs/Web/HTTP/CORS)
