---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: 路由和 ASP.NET Web API 中的操作选择 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: ce54181996376cb5dde3b91c10c16f33b3c6a570
ms.sourcegitcommit: 6548c19f345850ee22b50f7ef9fca732895d9e08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2018
ms.locfileid: "53425167"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>路由和 ASP.NET Web API 中的操作选择
====================
通过[Mike Wasson](https://github.com/MikeWasson)

本指南介绍了 ASP.NET Web API 将 HTTP 请求路由到控制器上的特定操作的方式。

> [!NOTE]
> 有关路由的高级别概述，请参阅[ASP.NET Web API 中的路由](routing-in-aspnet-web-api.md)。


本文探讨路由过程的详细信息。 如果你创建 Web API 项目，别让我打开某些请求的查找路由按你期望的方式，希望这篇文章将帮助。

路由具有三个主要阶段：

1. 匹配的路由模板的 URI。
2. 选择一个控制器。
3. 选择一个操作。

可以使用你自己的自定义行为来替换该过程的某些部分。 在本文中，我介绍的默认行为。 在结束时，我注意到在其中自定义行为的位置。

## <a name="route-templates"></a>路由模板

路由模板看起来类似于一个 URI 的路径，但它可以具有占位符值，指示用大括号：

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

当您创建一个路由时，您可以为某些或所有占位符提供默认值：

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

此外可以提供约束，限制 URI 段匹配占位符的方式：

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

该框架会尝试匹配中模板的 URI 路径段。 在模板中的文本必须完全匹配。 一个占位符匹配任何值，除非指定约束。 框架不匹配的 URI，例如主机名或查询参数的其他部分。 框架将与 URI 匹配的路由表中选择第一个路由。

有两个特殊的占位符:"{controller}"和"{action}"。

- "{controller}"提供的控制器的名称。
- "{action}"提供的操作的名称。 在 Web API 中，常规约定，是忽略"{action}"。

### <a name="defaults"></a>默认值

如果提供的默认值，该路由将匹配缺少这些段的 URI。 例如：

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Uri`http://localhost/api/products/all`和`http://localhost/api/products`匹配前面的路由。 在后一种 URI 中，缺少`{category}`段分配默认值`all`。

### <a name="route-dictionary"></a>路由字典

如果框架找到一个 URI 的匹配项，则会创建一个字典，其中包含每个占位符的值。 键是占位符名称，不包括大括号。 值会从 URI 路径或默认值。 字典存储在**IHttpRouteData**对象。

在路由匹配此阶段中，特殊"{controller}"和"{action}"占位符就像其他占位符一样处理。 它们只存储在字典中，其他值。

默认值可以具有特殊值**RouteParameter.Optional**。 如果占位符获取分配此值，该值是不会添加到路由字典中。 例如：

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

URI 路径的"api/产品"中，将包含路由字典：

- 控制器:"产品"
- 类别:"all"

对于"api/产品/toys/123"，但是，路由字典将包含：

- 控制器:"产品"
- 类别:"toys"
- id:"123"

默认值还可以在路由模板中包含一个值，未显示任何位置。 如果路由与匹配，此值存储在字典中。 例如：

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

如果 URI 路径为"根/api/8"，该字典将包含两个值：

- 控制器:"客户"
- id:"8"

## <a name="selecting-a-controller"></a>选择控制器

控制器选择处理通过**IHttpControllerSelector.SelectController**方法。 此方法采用**HttpRequestMessage**实例，并返回**HttpControllerDescriptor**。 提供的默认实现**DefaultHttpControllerSelector**类。 此类使用简单算法：

1. 在"控制器"的键的路由字典中查找。
2. 此密钥的值和追加"Controller"来获取控制器的类型名称的字符串。
3. 查找具有此类型名称的 Web API 控制器。

例如，如果路由字典包含键 / 值对"controller"="产品"，则控制器类型是"ProductsController"。 如果没有匹配的类型或多个匹配项，框架将返回到客户端错误。

步骤 3 中，对于**DefaultHttpControllerSelector**使用**IHttpControllerTypeResolver**接口，用于获取 Web API 控制器类型的列表。 默认实现**IHttpControllerTypeResolver**返回的所有公共类 （a） 实现**IHttpController**，（b） 是不抽象，和 （c） 具有以"Controller"结尾的名称。

## <a name="action-selection"></a>操作选择

后选择的控制器，则框架将选择该操作通过调用**IHttpActionSelector.SelectAction**方法。 此方法采用**HttpControllerContext** ，并返回**HttpActionDescriptor**。

提供的默认实现**ApiControllerActionSelector**类。 若要选择一项操作，它会查看以下：

- 请求的 HTTP 方法。
- 在路由模板中，如果存在"{action}"占位符。
- 在控制器上的操作的参数。

在查看选择算法时之前, 需要了解关于控制器操作的一些内容。

**在控制器上的方法被认为是"操作"？** 当选择一个操作，框架仅查看公共实例方法的控制器上。 此外，它不包括["特殊名称"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) （构造函数、 事件、 运算符重载等） 的方法和方法继承自**ApiController**类。

**HTTP 方法。** 框架仅选择匹配的请求，确定，如下所示的 HTTP 方法的操作：

1. 可以使用属性来指定 HTTP 方法：**AcceptVerbs**， **HttpDelete**， **HttpGet**， **HttpHead**， **HttpOptions**， **HttpPatch**， **HttpPost**，或**HttpPut**。
2. 否则，如果控制器方法的名称以"Get"、"Post"、"Put"、"删除"、"Head"、"选项"或"修补"开头，然后按照约定操作支持的 HTTP 方法。
3. 如果上述任何订阅，该方法支持 POST。

**参数绑定。** 参数绑定是如何将 Web API 创建某个参数的值。 下面是参数绑定的默认规则：

- 从 URI 中获取简单类型。
- 从请求正文中获取复杂类型。

简单类型包含的所有[.NET Framework 基元类型](https://msdn.microsoft.com/library/system.type.isprimitive)，加上**DateTime**，**十进制**， **Guid**，**字符串**，并**TimeSpan**。 对于每个操作，最多一个参数可以读取请求正文。

> [!NOTE]
> 它是可以重写默认绑定规则。 请参阅[WebAPI 参数绑定实质上](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)。


这种背景，下面是操作选择算法。

1. 匹配的 HTTP 请求方法的控制器中创建的所有操作的列表。
2. 如果路由字典具有"操作"条目，删除其名称与此值不匹配的操作。
3. 尝试匹配到 URI 操作参数，如下所示： 

    1. 对于每个操作，获取一系列是一种简单类型，其中，绑定将参数从 URI 参数。 排除的可选参数。
    2. 从此列表中，尝试路由字典中或在 URI 查询字符串中查找每个参数名称的匹配项。 匹配是不区分大小写，不依赖于参数顺序。
    3. 选择列表中的每个参数在 URI 中存在匹配项的其中一项操作。
    4. 如果有多个操作满足这些条件，选择其中一个的大多数参数匹配项。
4. 忽略操作 **[NonAction]** 属性。

步骤 #3 是可能最容易引起混淆。 基本理念是 URI 中，从请求正文中，或者从自定义绑定，参数可以获取其值。 对于来自 URI 的参数，我们想要确保 URI 实际上包含该参数中 （通过路由字典） 的路径或查询字符串中的值。

例如，考虑以下操作：

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

*Id*参数绑定到的 URI。 因此，此操作只能匹配一个 URI，包含"id"，在路由字典中或在查询字符串中的值。

可选参数是个例外，因为它们是可选的。 可选参数，它将是确定，如果绑定不能从 URI 中获取的值。

复杂类型是由于其他原因的异常。 通过自定义绑定的复杂类型只能绑定到的 URI。 但在这种情况下，该框架不能提前知道参数将绑定到特定的 URI。 若要了解，它需要调用绑定。 选择算法的目的是从静态的说明，然后再调用任何绑定选择一项操作。 因此，复杂类型不从匹配算法。

选择操作后，会调用参数的所有绑定。

摘要:

- Action 必须与请求的 HTTP 方法匹配。
- 如果存在，则操作名称必须与匹配路由字典中的"操作"条目。
- 对于操作的每个参数，如果该参数来自 URI，则参数名称必须找到路由字典中或在 URI 查询字符串中。 （可选参数和复杂类型的参数被排除。）
- 尝试匹配最大数量的参数。 最佳匹配项可能是不带任何参数的方法。

## <a name="extended-example"></a>扩展的示例

路由：

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

控制器:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP 请求：

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>路由匹配

URI 匹配名为"DefaultApi"的路由。 路由字典包含以下各项：

- 控制器:"产品"
- id:"1"

路由字典不包含查询字符串参数、"版本"和"详细信息"，但它们仍被视为在操作选择过程。

### <a name="controller-selection"></a>控制器所选内容

控制器类型是从路由字典中的"控制器"条目， `ProductsController`。

### <a name="action-selection"></a>操作选择

HTTP 请求是 GET 请求。 支持 GET 的控制器操作`GetAll`， `GetById`，和`FindProductsByName`。 路由字典不包含"操作"的项，因此我们无需与操作名称匹配。

接下来，我们尝试匹配参数名称的操作，只查看在 GET 操作。

| 操作 | 匹配的参数 |
| --- | --- |
| `GetAll` | 无 |
| `GetById` | "id" |
| `FindProductsByName` | "name" |

请注意，*版本*参数的`GetById`不被视为，因为它是一个可选参数。

`GetAll`方法完全匹配。 `GetById`方法也与匹配，因为路由字典包含"id"。 `FindProductsByName`方法不匹配。

`GetById`方法 wins，因为它与一个参数，而不是任何参数匹配`GetAll`。 使用以下参数值调用该方法：

- *id* = 1
- *版本*= 1.5

请注意，即使*版本*不使用在选择算法参数的值来自 URI 查询字符串。

## <a name="extension-points"></a>扩展点

Web API 路由过程的某些部分提供了扩展点。

| 接口 | 描述 |
| --- | --- |
| **IHttpControllerSelector** | 选择控制器。 |
| **IHttpControllerTypeResolver** | 获取控制器类型的列表。 **DefaultHttpControllerSelector**从此列表中选择控制器类型。 |
| **IAssembliesResolver** | 获取项目的程序集的列表。 **IHttpControllerTypeResolver**接口使用此列表以找到控制器的类型。 |
| **IHttpControllerActivator** | 创建新的控制器实例。 |
| **IHttpActionSelector** | 选择操作。 |
| **IHttpActionInvoker** | 调用的操作。 |

若要为任何这些接口提供您自己的实现，使用**Services**上的收集**HttpConfiguration**对象：

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
