---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: 在 ASP.NET MVC 和 Web Pages 中的 XSRF/CSRF 防护 |Microsoft Docs
author: Rick-Anderson
description: 跨站点请求伪造 （也称为 XSRF 或 CSRF） 是针对恶意网站凭此可以影响 interacti web 托管应用程序的攻击...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 5db661cccc58d1101f95091b069ab5cbfe78a378
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577933"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>在 ASP.NET MVC 和 Web Pages 中的 XSRF/CSRF 防护
====================
通过[Rick Anderson]((https://twitter.com/RickAndMSFT))

> 跨站点请求伪造 （也称为 XSRF 或 CSRF） 是针对恶意网站凭此可以影响客户端浏览器和该浏览器的受信任的网站之间的交互的 web 托管的应用程序的攻击。 这些攻击的原因可能 web 浏览器会将自动随每个请求的身份验证令牌发送到 web 站点。 典型示例是身份验证 cookie，如 ASP。NET 的窗体身份验证票证。 然而，使用任何持久身份验证机制 （如 Windows 身份验证、 Basic 等） 的网站可能受攻击目标。
> 
> XSRF 攻击与钓鱼攻击不同。 网络钓鱼攻击需要与受害者进行交互。 在网络钓鱼攻击中，恶意网站将模拟目标网站，并在受害者受骗向攻击者提供敏感信息。 在 XSRF 攻击中，没有通常无需交互与受害者。 相反，攻击者依靠浏览器自动向目标网站发送所有相关 cookie。
> 
> 有关详细信息，请参阅[打开 Web 应用程序安全项目](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))。


## <a name="anatomy-of-an-attack"></a>一种攻击的剖析

若要遍历 XSRF 攻击时，请考虑想要执行一些联机银行事务的用户。 此用户第一次访问 WoodgroveBank.com 和日志中，此时响应标头将包含其身份验证 cookie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

由于身份验证 cookie 的会话 cookie，它将自动清除浏览器时在浏览器进程退出。 但是，在此之前，在浏览器将自动包括与 WoodgroveBank.com 的每个请求的 cookie。 用户现在想要将 1000 美元传输到另一个帐户，因此她表单中填写银行站点上并在浏览器向服务器发出此请求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

此操作具有副作用 （它开始货币的事务），因为已选择银行站点需要 HTTP POST，才能启动此操作。 服务器从请求中读取身份验证令牌、 查找当前用户的帐户数、 验证充足的资金存在，然后启动到目标帐户的事务。

她联机银行完成后，用户离开银行站点，并在 web 上访问的其他位置。 这些站点 – fabrikam.com – 之一包含嵌入在页面上的以下标记&lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

它会导致浏览器发出此请求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻击者利用这一事实，用户可能仍具有有效的身份验证令牌为目标 web 站点，并且她正在使用 Javascript 一小段代码会导致浏览器自动发出 HTTP POST 到目标站点。 如果仍有效的身份验证令牌，银行站点将启动的 250 美元到攻击者选择的帐户的复制。

### <a name="ineffective-mitigations"></a>无效的缓解措施

值得注意的是在上面的方案中，则表明 WoodgroveBank.com 正在通过 SSL 访问，并且必须仅 SSL 身份验证 cookie 已不足以防止攻击。 攻击者是能够指定[URI 方案](http://en.wikipedia.org/wiki/URI_scheme)(https) 在她&lt;窗体&gt;元素，并在浏览器将继续将未过期的 cookie 发送到目标站点，只要这些 cookie 是一致的 uri预期目标的方案。

有人可能会认为，用户应只需不访问不受信任的站点，作为访问仅受信任的站点是能够安全在线帮助。 为此，一些真实但遗憾的是这一建议并不总是可行。 用户可能是"信任"ConsolidatedMessenger 的当地新闻站点。 ConsolidatedMessenger.com 并转到相反，站点的访问，但该站点具有 XSS 漏洞允许攻击者注入 fabrikam.com 运行的代码的同一代码片段。

可以验证传入的请求具有[Referer 标头](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)引用你的域。 这将停止从第三方域明智地提交请求。 但是，有些人禁用其浏览器的引用站点标头，出于隐私原因，并且如果受害者计算机未安装某些不安全软件，攻击者有时可以欺骗该标头。 验证[Referer 标头](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)不被视为一种阻止 XSRF 攻击的安全方法。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web 堆栈运行时 XSRF 缓解措施

ASP.NET Web 堆栈运行时使用的一个变体[同步器标记模式](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)来抵御 XSRF 攻击。 同步器令牌模式的一般形式为的两个的 ANTI-XSRF 令牌提交到每个 HTTP POST （除了身份验证令牌） 的服务器： 一个令牌作为 cookie，另一个用作窗体值。 由 ASP.NET 运行时生成的令牌值不是确定性的也可由攻击者预测。 当提交令牌时，则服务器将允许继续仅当这两个令牌通过比较检查请求。

XSRF 请求验证*会话令牌*HTTP cookie 作为存储和当前包含其有效负载中的以下信息：

- 安全令牌中，包含一个随机的 128 位标识符。   
 下图显示了使用 Internet Explorer F12 开发人员工具显示 XSRF 请求验证会话令牌: (请注意这是当前的实现，并且受，甚至有可能，若要更改。)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*字段令牌*存储为`<input type="hidden" />`和包含其有效负载中的以下信息：

- 登录的用户的用户名 （如果经过身份验证）。
- 提供的任何其他数据[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)。

ANTI-XSRF 令牌的有效负载进行加密和签名，以便使用工具来检查令牌时，不能查看的用户名。 当 web 应用程序面向 ASP.NET 4.0 时，由提供加密服务[MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx)例程。 当 web 应用程序面向 ASP.NET 4.5 或更高版本、 加密服务提供的[MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110))例程，它提供了更好的性能、 可扩展性和安全性。 请参阅以下博客文章的更多详细信息：

- [在 ASP.NET 4.5 中的加密改进、 pt。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [在 ASP.NET 4.5 中的加密改进、 pt。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [在 ASP.NET 4.5 中的加密改进、 pt。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>生成令牌

若要生成的 ANTI-XSRF 令牌，调用[ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) MVC 视图中的方法或@AntiForgery.GetHtml从 Razor 页面 （)。 然后，运行时将执行以下步骤：

1. 如果当前的 HTTP 请求中已包含的 ANTI-XSRF 会话令牌 (的 ANTI-XSRF cookie \_ \_RequestVerificationToken)，从其提取的安全令牌。 如果 HTTP 请求不包含的 ANTI-XSRF 会话令牌或安全令牌提取失败，将生成新的随机的 ANTI-XSRF 令牌。
2. 使用从上面的步骤 (1) 和当前登录的用户的标识的安全令牌生成的 ANTI-XSRF 字段令牌。 (有关确定用户标识的详细信息，请参阅 **[提供特殊支持的方案](#_Scenarios_with_special)** 下面一节。)此外，如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx)是配置，则运行时会调用其[GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)方法，包括字段令牌中返回的字符串。 (请参阅**[配置和可扩展性](#_Configuration_and_extensibility)** 部分，了解详细信息。)
3. 如果在步骤 (1) 中生成新的 ANTI-XSRF 令牌，新的会话令牌将创建以包含它，并且将添加到的出站的 HTTP cookie 集合。 步骤 (2) 中的字段标记将包装在`<input type="hidden" />`将返回值的元素，并且此 HTML 标记`Html.AntiForgeryToken()`或`AntiForgery.GetHtml()`。

## <a name="validating-the-tokens"></a>验证的令牌

若要验证传入的 ANTI-XSRF 令牌，开发人员包括[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx)她 MVC 操作或控制器，则她调用属性`@AntiForgery.Validate()`从她 Razor 页面。 在运行时将执行以下步骤：

1. 读取传入会话令牌和字段标记，并从每个提取的 ANTI-XSRF 令牌。 ANTI-XSRF 令牌必须为每个步骤 (2) 生成例程中完全相同。
2. 如果当前用户进行身份验证，其用户名进行比较的字段标记中存储的用户名。 用户名必须与匹配。
3. 如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)配置，则运行时会调用其*ValidateAdditionalData*方法。 该方法必须返回布尔值 *，则返回 true*。

如果验证成功，则允许该请求以继续。 如果验证失败，框架将引发*HttpAntiForgeryException*。

## <a name="failure-conditions"></a>失败条件

从任何 ASP.NET Web 堆栈运行时版本 2 开始*HttpAntiForgeryException*期间引发验证将包含有关发生的问题的详细的信息。 当前定义的失败条件包括：

- 会话令牌或窗体令牌不存在请求中。
- 会话令牌或窗体令牌不可读。 最可能的原因是运行不匹配的版本的 ASP.NET Web 堆栈运行时或场的场其中&lt;machineKey&gt;机之间不同，在 Web.config 中的元素。 您可以使用 Fiddler 之类的工具篡改其中任意一个的 ANTI-XSRF 令牌强制此异常。
- 会话令牌和字段令牌被互换。
- 会话令牌和字段令牌包含不匹配的安全令牌。
- 嵌入的字段标记中的用户名与当前登录的用户的用户名不匹配。
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* 方法返回*false*。

ANTI-XSRF 功能还可以执行附加检查在令牌生成或验证，过程和这些检查期间出现的故障可能会导致引发异常。 请参阅[WIF / ACS / 基于声明的身份验证](#_WIF_ACS)并**[配置和可扩展性](#_Configuration_and_extensibility)** 部分，了解详细信息。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>提供特殊支持的方案

### <a name="anonymous-authentication"></a>匿名身份验证

ANTI-XSRF 系统包含为匿名用户，其中"匿名"定义为用户的特殊支持其中*IIdentity.IsAuthenticated*属性将返回*false*。 方案包括提供 XSRF 保护到登录页 （在之前的用户进行身份验证） 和应用程序而不使用一种机制，其中的自定义身份验证方案*IIdentity*标识的用户。

若要支持这些方案，请记住的会话和字段的令牌已加入由安全令牌，这是一个 128 位随机生成不透明标识符。 此安全令牌用于跟踪单个用户的会话，当她导航站点，因此它有效地起到作用的匿名标识符。 空字符串将代替上面所述的生成和验证例程的用户名。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / 基于声明的身份验证

通常情况下， *IIdentity*到.NET Framework 中内置的类具有属性的*IIdentity.Name*足以唯一地标识特定的应用程序内的特定用户。 例如， *FormsIdentity.Name*返回在成员资格数据库中 （这是唯一的具体取决于该数据库的所有应用程序），存储的用户名*WindowsIdentity.Name*返回域限定标识的用户，依此类推。 这些系统提供不仅身份验证;它们还*标识*向应用程序的用户。

基于声明的身份验证，但是，不一定需要标识特定用户。 相反， *ClaimsPrincipal*并*ClaimsIdentity*都有一组类型相关*声明*实例，其中的各个声明可能是"为 18 + 年的保留时间"或"是管理员"为其他值。 由于尚未一定标识该用户，不能使用运行时*ClaimsIdentity.Name*属性设置为该特定用户的唯一标识符。 团队已发现实际示例其中*ClaimsIdentity.Name*返回*null*、 返回友好 （显示） 的名称，或否则将返回一个字符串，并不适合用作唯一标识符为用户。

许多使用基于声明的身份验证的部署使用的[Azure 访问控制服务](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx)(ACS) 特别。 ACS 允许开发人员可以配置单个*标识提供者*（例如 ADFS，Microsoft 帐户提供程序，OpenID 提供程序类似于 yahoo ！，等等），并标识提供程序返回*命名标识符*. 这些名称标识符可能包含个人身份信息 (PII)，为电子邮件地址，也可以为匿名等专用个人标识符 (PPID)。 无论如何，元组 （标识提供者、 名称标识符） 足够作为特定用户的相应的跟踪令牌，而她正在浏览站点，因此，生成时，ASP.NET Web 堆栈运行时可以使用代替用户名元组和正在验证字段的 ANTI-XSRF 令牌。 标识提供者和名称标识符的特定 Uri 是：

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(请参阅此[ACS 文档页](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx)的详细信息。)

当生成或验证令牌时，ASP.NET Web 堆栈运行时将在运行时尝试绑定到类型：

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` （适用于 WIF SDK。)
- `System.Security.Claims.ClaimsIdentity` （适用于.NET 4.5)。

如果这些类型存在，并且当前用户的*IIIIdentity*实现或子类其中一种类型，（标识提供者、 名称标识符），将使用的 ANTI-XSRF 设施代替用户名时生成的元组和验证的令牌。 如果存在任何此类元组，不则请求将失败并出错向开发人员说明如何配置的 ANTI-XSRF 系统，以了解在使用特定的基于声明的身份验证机制。 请参阅**[配置和可扩展性](#_Configuration_and_extensibility)** 部分，了解详细信息。

### <a name="oauth--openid-authentication"></a>OAuth / OpenID 身份验证

最后的 ANTI-XSRF 设施提供特殊支持使用 OAuth 或 OpenID 身份验证的应用程序。 这种支持是基于启发式方法的： 如果当前*IIdentity.Name*开始 http:// 或 https:// 开头，然后将完成用户名比较使用序号比较器而不是默认 OrdinalIgnoreCase 比较器。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>配置和可扩展性

有时，开发人员可能希望严格控制的 ANTI-XSRF 生成和验证行为。 例如，可能会自动将 HTTP cookie 添加到响应的 MVC 和 Web Pages 帮助程序的默认行为是不可取，和开发人员可能想要持久保存在其他地方的令牌。 存在两个 Api，以帮助解决这个问题：

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*方法的输入现有 XSRF 请求验证会话令牌 （这可能为 null），并为生成输出新 XSRF 请求验证会话令牌和字段令牌。 令牌是不带修饰符; 只是不透明字符串*formToken*值实例不会包装在&lt;输入&gt;标记。 *NewCookieToken*值可能为 null; 如果发生这种情况，则*oldCookieToken*值仍然有效，并且需要设置任何新的响应 cookie。 调用方*GetTokens*负责保持任何必要的响应 cookie 或生成任何必要的标记; *GetTokens*方法本身不会更改产生了负面影响的响应。 *验证*方法将传入会话和字段令牌并对它们运行前面提到的验证逻辑。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

开发人员可以配置应用程序中的 ANTI-XSRF 系统\_开始。 以编程方式配置。 属性的静态*AntiForgeryConfig*类型如下所述。 使用声明的大多数用户将希望将 UniqueClaimTypeIdentifier 属性设置。

| **Property** | **说明** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) ，在令牌的生成过程中提供额外的数据和在令牌验证期间会占用更多数据。 默认值是*null*。 有关详细信息，请参阅[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)部分。 |
| **CookieName** | 提供用于存储的 ANTI-XSRF 会话令牌的 HTTP cookie 的名称的字符串。 如果未设置此值，将自动生成名称基于应用程序的已部署的虚拟路径。 默认值是*null*。 |
| **RequireSsl** | 一个布尔值，指示是否需要通过 SSL 安全通道提交的 ANTI-XSRF 令牌。 如果此值为 *，则返回 true*、 自动生成的任何 cookie 将具有"安全"标志设置，并且从通过 SSL 未提交的请求中调用，则会引发的 ANTI-XSRF Api。 默认值为“false”。 |
| **SuppressIdentityHeuristicChecks** | 一个布尔值，指示的 ANTI-XSRF 系统是否应停用它对基于声明的标识的支持。 如果此值为 *，则返回 true*，系统将假定*IIdentity.Name*适合用作唯一的每个用户标识符并且不会尝试使用特殊处理*IClaimsIdentity*或*ClClaimsIdentity*中所述[WIF / ACS / 基于声明的身份验证](#_WIF_ACS)部分。 默认值为 `false`。 |
| **UniqueClaimTypeIdentifier** | 一个字符串，指示哪些声明类型适合用作唯一的每个用户标识符。 如果此值是组和当前*IIdentity*基于声明的则系统将尝试提取声明的类型由指定*UniqueClaimTypeIdentifier*，并且将使用相应的值代替时生成的字段标记的用户的用户名。 如果找不到声明类型，系统将无法发送请求。 默认值是*null*，指示系统应使用 （标识提供者、 名称标识符） 代替用户的用户名按前面所述的元组。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 类型允许开发人员能够通过往返其他数据中的每个标记扩展的 ANTI-XSRF 系统的行为。 *GetAdditionalData*每次调用方法生成的字段标记，并返回值嵌入生成的令牌中。 实现器可能从此方法返回一个时间戳、 nonce 或希望在她的任何其他值。

同样， *ValidateAdditionalData*每次调用方法的字段的令牌进行验证，并嵌入令牌中的"其他数据"字符串传递给方法。 验证例程可以 （通过检查当前时间针对令牌在创建时存储的时间） 来实现超时，nonce 检查例程，或任何其他所需的逻辑。

## <a name="design-decisions-and-security-considerations"></a>设计决策和安全注意事项

链接的会话和字段的令牌的安全令牌从技术上讲时才是必需尝试匿名 / 未经身份验证用户免受 XSRF 攻击。 当用户进行身份验证时，身份验证令牌本身 （可能 cookie 的形式提交） 可用作一个半同步器令牌对。 但是，有有效保护受到未经授权的用户的登录页的方案和的 ANTI-XSRF 逻辑始终生成和验证安全令牌，即使对于身份验证的用户通过更加简单。 它还提供了一些额外的保护的事件中字段令牌曾入侵攻击者，为设置或猜测会话令牌攻击者能够克服的另一个障碍。

当在单个域承载多个应用程序时，开发人员应小心。 例如，即使*example1.cloudapp.net*并*example2.cloudapp.net*不同主机下的所有主机之间没有隐式信任关系 *\*。 cloudapp.net*域。 此隐式信任关系[允许可能不受信任的主机以影响彼此的 cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) （控制 AJAX 请求的同域策略不一定适用于 HTTP cookie）。 ASP.NET Web 堆栈运行时，用户名嵌入到该字段标记，因此即使恶意子域是能够覆盖会话令牌将无法生成有效的字段标记，以该用户提供一些缓解措施。 但是，在这种环境中托管时内置的 ANTI-XSRF 例程仍不能抵御会话劫持或登录名 XSRF。

ANTI-XSRF 例程当前执行不抵御[点击劫持](https://www.owasp.org/index.php/Clickjacking)。 想要自行防范点击劫持的应用程序可以轻松完成此操作发送 X 帧选项： SAMEORIGIN 标头以及每个响应。 所有最新的浏览器都支持此标头。 有关详细信息，请参阅[IE 博客](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)，则[SDL 博客](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)，并[OWASP](https://www.owasp.org/index.php/Clickjacking)。 ASP.NET Web 堆栈运行时可能会在一些将来的版本使 MVC 和网页的 ANTI-XSRF 帮助程序自动设置此标头，以便应用程序遭受此种攻击会自动受到保护。

Web 开发人员应继续以确保它们的站点不是很容易受到 XSS 攻击。 XSS 攻击是功能非常强大，并成功利用此漏洞也会破坏 ASP.NET Web 堆栈运行时对防御 XSRF 攻击。

## <a name="acknowledgment"></a>确认

[@LeviBroderick](https://twitter.com/LeviBroderick)谁写入 ASP.NET 安全代码的大部分此信息的大容量。
