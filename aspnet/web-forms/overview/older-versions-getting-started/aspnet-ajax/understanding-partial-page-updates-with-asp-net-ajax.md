---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: 了解使用 ASP.NET AJAX 的部分页面更新 |Microsoft Docs
author: scottcate
description: 可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完全回发到 t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4883046aa16d5e67b7f0c92e15c897ef1a933b67
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098930"
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>使用 ASP.NET AJAX 的了解部分页面更新
====================
通过[Scott Cate](https://github.com/scottcate)

[下载 PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> 可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完全回发到服务器，使用无代码更改和最小标记更改。 优点是广泛 – 你多媒体内容 （例如 Adobe Flash 或 Windows Media） 的状态保持不变、 降低带宽成本，和客户端不会遇到通常与回发相关联的闪烁。


## <a name="introduction"></a>介绍

Microsoft 的 ASP.NET 技术带来了面向对象和事件驱动的编程模型，并将其与已编译的代码的好处结合在一起。 但是，其服务器端处理模型具有固有的技术中的几个缺点：

- 页更新需要往返服务器，这需要刷新页面。
- 往返，不会保留任何影响生成的 Javascript 或其他客户端技术 （如 Adobe Flash)
- 在回发时，Microsoft Internet Explorer 以外的浏览器不支持自动还原的滚动位置。 并且甚至在 Internet Explorer 中，没有仍闪烁如刷新页面。
- 回发可能涉及大量的带宽， \_ \_VIEWSTATE 窗体字段可能会增长，尤其是在处理控件，如 GridView 控件或重复字符。
- 没有用于访问 Web 服务通过 JavaScript 或其他客户端技术统一的模型。

输入 Microsoft 的 ASP.NET AJAX extensions 中。 AJAX 中，它代表**A**同步**J** avaScript**一个**nd **X**机器学习，是集成的框架，用于提供增量页通过跨平台 JavaScript 的更新组成，其中包括 Microsoft AJAX 框架和一个名为 Microsoft AJAX 脚本库的脚本组件的服务器端代码。 ASP.NET AJAX 扩展还提供用于访问通过 JavaScript 的 ASP.NET Web 服务的跨平台支持。

本白皮书探讨 ASP.NET AJAX Extensions，它包括 ScriptManager 组件、 UpdatePanel 控件和 UpdateProgress 控件，并考虑他们应该或不应为的方案的部分页面更新功能使用过度。

此白皮书基于 Visual Studio 2008 Beta 2 版本和.NET Framework 3.5 中，将 ASP.NET AJAX Extensions 集成到 Base Class Library （了以前适用于 ASP.NET 2.0 的外接程序组件）。 本白皮书还假设您正在使用 Visual Studio 2008 和不 Visual Web Developer 速成版;提到的一些项目模板可能无法供 Visual Web Developer 速成版的用户。

## <a name="partial-page-updates"></a>部分页面更新

可能是最明显的 ASP.NET AJAX Extensions 特色是能够执行部分或增量页更新操作而无需执行完全回发到服务器，使用无代码更改和最小标记更改。 优点是广泛-你多媒体内容 （例如 Adobe Flash 或 Windows Media） 的状态保持不变、 降低带宽成本，和客户端不会遇到通常与回发相关联的闪烁。

集成部分页面呈现的功能进行少量的更改到您的项目集成到 ASP.NET。

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>演练：将部分呈现集成到现有项目


1. 在 Microsoft Visual Studio 2008 中，通过转到创建新的 ASP.NET 网站项目<em>文件</em> <em>- &gt;新建</em> <em>- &gt;网站</em>并从对话框中选择 ASP.NET Web 站点。 您可以命名它您希望的任何内容，您可以将它安装到文件系统或到 Internet 信息服务 (IIS)。
2. 将显示与使用基本的 ASP.NET 标记的空默认页 (服务器端窗体和`@Page`指令)。 删除名为标签`Label1`和一个按钮调用`Button1`拖到窗体元素在页面。 可以设置它们的 text 属性为希望的任何内容。
3. 在设计视图中，双击`Button1`以生成代码隐藏事件处理程序。 在此事件处理程序中，设置`Label1.Text`到单击了按钮 ！ .

**列表 1:Default.aspx 之前启用了部分呈现的标记**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**列表 2:代码隐藏文件 default.aspx.cs 中 （剪裁）**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. 按 F5 以启动您的网站。 Visual Studio 会提示您添加一个 web.config 文件以启用调试;执行此操作。 当单击按钮时，请注意，刷新该页面可以更改的标签中的文本，并且没有简要闪烁如页面重绘。
2. 关闭浏览器窗口后，返回到 Visual Studio 和标记页上。 在 Visual Studio 工具箱中，向下滚动并找到标记为 AJAX 扩展选项卡。 （如果你还没有此选项卡上由于使用的较旧版本的 AJAX 或 Atlas 扩展，以便稍后在本白皮书中，注册 AJAX Extensions 工具箱项演练，请参阅或可下载 Windows 安装程序安装最新版本从网站）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>已知问题：</em>如果到已随 ASP.NET 2.0 AJAX Extensions 一起安装的 Visual Studio 2005 的计算机上安装 Visual Studio 2008，Visual Studio 2008 将导入的 AJAX 扩展工具箱项。 您可以确定是否是这种通过检查组件、 工具提示它们应显示版本 3.5.0.0。 如果他们说 2.0.0.0 版，然后导入在旧的工具箱项，并将需要手动使用 Visual Studio 中的选择工具箱项对话框中将其导入。 你将不能添加版本 2 通过设计器的控件。

2. 之前`<asp:Label>`标记开始，创建行的空格，然后双击工具箱中的 UpdatePanel 控件。 请注意，一个新`@Register`指令，则包含页上，，该值指示应使用导 System.Web.UI 命名空间中的控件的顶部`asp:`前缀。
3. 拖动结束`</asp:UpdatePanel>`标记末尾的按钮元素，以便使用包装的标签和按钮控件的元素是格式正确。
4. 打开之后`<asp:UpdatePanel>`标记中，开始打开一个新的标记。 请注意，IntelliSense 会提示你使用两个选项。 在这种情况下，创建`<ContentTemplate>`标记。 务必将此标记标签和按钮，以便标记的格式正确。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. 中的任意位置`<form>`元素中，通过双击包括 ScriptManager 控件`ScriptManager`工具箱中的项。
2. 编辑`<asp:ScriptManager>`标记，以便它包含属性`EnablePartialRendering= true`。

**代码清单 3:使用部分呈现启用 default.aspx 的标记**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. 打开 web.config 文件。 请注意，Visual Studio 会自动添加了对 System.Web.Extensions.dll 的编译引用。

1. 什么是 Visual Studio 2008 中的新增功能：提供与 ASP.NET 网站项目模板自动包括对 ASP.NET AJAX Extensions 中，所有必需的引用，包括 web.config 的注释可以取消注释，若要启用额外的配置信息的部分功能。 Visual Studio 2005 在已安装 ASP.NET 2.0 AJAX Extensions 有类似的模板。 但是，在 Visual Studio 2008 中，AJAX Extensions 将选择退出默认情况下 （即，它们引用的默认值，但可以删除作为引用）。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. 按 F5 启动你的网站。 请注意如何无需更改源代码所需支持部分呈现-仅标记已更改。

当启动你的网站时，应看到部分呈现是现在已启用，因为当您单击时按钮有将为无闪烁，也不会有页滚动位置 （此示例没有演示的） 中的任何更改。 如果要查看页面的呈现源，单击按钮后，它将确认实际上没有发生回发-原始标签文本仍是源标记的一部分并且标签已更改通过 JavaScript。

Visual Studio 2008 似乎未附带了一个支持 ASP.NET AJAX 的 web 站点的预定义模板。 但是，此类模板的 Visual Studio 2005 中可用，如果已安装 Visual Studio 2005 和 ASP.NET 2.0 AJAX Extensions。 因此，配置网站，然后使用启用了 ajax 网站模板启动可能会更简单了，因为该模板应包含 （支持所有 ASP.NET AJAX Extensions 中，包括 Web 服务的访问权限的完全配置 web.config 文件和 JSON 序列化的 JavaScript 对象表示法） 并且包含一个 UpdatePanel 和 ContentTemplate 主 Web 窗体页中，默认情况下。 启用与此默认页的部分呈现是再探本演练的步骤 10 放到页面上的控件一样简单。

## <a name="the-scriptmanager-control"></a>ScriptManager 控件

## <a name="scriptmanager-control-reference"></a>ScriptManager 控件引用

已启用标记的属性：

| **属性名称** | **Type** | **说明** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | 指定是否使用 web.config 文件的自定义错误部分处理错误。 |
| AsyncPostBackError-Message | String | 获取或设置发送到客户端，如果引发错误的错误消息。 |
| AsyncPostBack-Timeout | Int32 | 获取或设置默认的客户端应等待完成的异步请求的时间量。 |
| EnableScript-Globalization | Bool | 获取或设置是否启用了全球化脚本。 |
| EnableScript-Localization | Bool | 获取或设置是否启用脚本本地化。 |
| ScriptLoadTimeout | Int32 | 确定允许加载到客户端脚本的秒数 |
| ScriptMode | 枚举 （自动、 调试、 发布、 继承） | 获取或设置是否呈现脚本的发行版本 |
| ScriptPath | String | 获取或设置要发送到客户端的脚本文件的位置的根路径。 |

仅限代码的属性：

| **属性名称** | **Type** | **说明** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | 获取有关 ASP.NET 身份验证服务代理时，将发送到客户端代理的详细信息。 |
| IsDebuggingEnabled | Bool | 获取是否脚本并启用了调试代码。 |
| IsInAsyncPostback | Bool | 获取页面当前是否处于异步回发请求。 |
| ProfileService | ProfileService-Manager | 获取有关将发送到客户端的 ASP.NET 分析服务代理的详细信息。 |
| 脚本 | Collection&lt;Script-Reference&gt; | 获取将发送到客户端的脚本引用的集合。 |
| Services | Collection&lt;Service-Reference&gt; | 获取将发送到客户端的 Web 服务代理引用的集合。 |
| SupportsPartialRendering | Bool | 获取是否当前客户端支持部分呈现。 如果此属性返回**false**，则所有页面请求都将标准的回发。 |

公共代码的方法：

| **方法名称** | **Type** | **说明** |
| --- | --- | --- |
| SetFocus(string) | Void | 当请求已完成，客户端的焦点设置到特定控件。 |

标记后代：

| **标记** | **说明** |
| --- | --- |
| &lt;AuthenticationService&gt; | 提供有关 ASP.NET 身份验证服务代理的详细信息。 |
| &lt;ProfileService&gt; | 对 ASP.NET 分析服务提供有关代理的详细信息。 |
| &lt;脚本&gt; | 提供了其他脚本引用。 |
| &lt;asp:ScriptReference&gt; | 表示特定的脚本引用。 |
| &lt;服务&gt; | 提供了将具有生成的代理类的其他 Web 服务引用。 |
| &lt;asp:ServiceReference&gt; | 表示特定的 Web 服务引用。 |

ScriptManager 控件是 ASP.NET AJAX Extensions 的基本核心。 它提供对脚本库 （包括大量客户端脚本类型系统） 的访问、 支持部分呈现和为其他 ASP.NET 服务 （如身份验证和分析，但也有其他 Web 服务） 提供广泛支持。 ScriptManager 控件还提供了全球化和本地化支持的客户端脚本。

## <a name="providing-alternative-and-supplemental-scripts"></a>提供替代和补充脚本

Microsoft ASP.NET 2.0 AJAX Extensions 中这两个调试包含整个脚本代码，并作为资源嵌入在被引用程序集中发布版本，而开发人员可以随意将 ScriptManager 重定向到自定义的脚本文件，以及注册其他必要的脚本。

若要重写 （例如，支持 Sys.WebForms 命名空间和自定义的类型化系统） 通常包含脚本的默认绑定，可以注册为`ResolveScriptReference`ScriptManager 类的事件。 事件处理程序调用此方法时，有机会更改相关; 脚本文件的路径脚本管理器将然后将不同的或自定义脚本的副本发送到客户端。

此外，脚本引用 (表示`ScriptReference`类) 可以包含以编程方式或通过标记。 若要执行此操作，请以编程方式修改`ScriptManager.Scripts`集合，或包含`<asp:ScriptReference>`标记下`<Scripts>`标记，它是 ScriptManager 控件的第一级子项。

## <a name="custom-error-handling-for-updatepanels"></a>自定义错误处理 Updatepanel

尽管由触发器由 UpdatePanel 控件指定处理更新，由页面的 ScriptManager 控件实例处理的错误处理和自定义错误消息的支持。 这是通过公开一个事件， `AsyncPostBackError`，可以逐页然后提供自定义异常处理逻辑。

通过使用 AsyncPostBackError 事件，您可能指定`AsyncPostBackErrorMessage`属性，然后引发一个警告框以回调完成时引发。

客户端的自定义项，也可以而不是使用默认警告框;例如，你可能想要显示自定义`<div>`元素而不是默认浏览器模式对话框。 在这种情况下，你可以处理客户端脚本中的错误：

**列表 5:客户端脚本，以显示自定义错误**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

答案很简单，上述脚本注册的回调与客户端 AJAX 运行时为时已完成异步请求。 然后，它会检查是否报告了错误，以及如果是这样，处理的详细信息，最后，该值指示运行时在自定义脚本已处理该错误。

## <a name="globalization-and-localization-support"></a>全球化和本地化支持

ScriptManager 控件提供了广泛支持本地化的脚本字符串和用户界面组件;但是，该主题位于此白皮书的范围之外。 有关详细信息，请参阅白皮书，全球化支持 ASP.NET AJAX Extensions 中。

## <a name="the-updatepanel-control"></a>UpdatePanel 控件

## <a name="updatepanel-control-reference"></a>UpdatePanel 控件引用

已启用标记的属性：

| **属性名称** | **Type** | **说明** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | 指定子控件是否自动调用在回发时刷新。 |
| RenderMode | 枚举 （块、 内联） | 指定将直观地显示内容的方式。 |
| UpdateMode | 枚举 （始终、 条件） | 指定是否在部分呈现期间始终刷新 UpdatePanel 或如果它才会刷新时命中一个触发器。 |

仅限代码的属性：

| **属性名称** | **Type** | **说明** |
| --- | --- | --- |
| IsInPartialRendering | bool | 获取是否 UpdatePanel 当前请求的支持部分呈现。 |
| ContentTemplate | ITemplate | 获取更新请求的标记模板。 |
| ContentTemplateContainer | 控件 | 获取更新请求以编程方式的模板。 |
| Triggers | UpdatePanel- TriggerCollection | 获取与当前的 UpdatePanel 的触发器的列表。 |

公共代码的方法：

| **方法名称** | **Type** | **说明** |
| --- | --- | --- |
| Update （) | Void | 以编程方式更新指定的 UpdatePanel。 允许服务器请求来触发否则为存 UpdatePanel 部分呈现。 |

标记后代：

| **标记** | **说明** |
| --- | --- |
| &lt;ContentTemplate&gt; | 指定要用来呈现部分呈现结果的标记。 子级&lt;asp: UpdatePanel&gt;。 |
| &lt;触发器&gt; | 指定的集合*n*关联并将更新此 UpdatePanel 控件。 子级&lt;asp: UpdatePanel&gt;。 |
| &lt;asp:AsyncPostBackTrigger&gt; | 指定触发器调用有给定 UpdatePanel 部分页面呈现。 这可能会也可能不是作为相关 UpdatePanel 的后代控件。 精细的事件名称。子级&lt;触发器&gt;。 |
| &lt;asp:PostBackTrigger&gt; | 指定控件将导致整个页面刷新。 这可能会也可能不是作为相关 UpdatePanel 的后代控件。 精细对象。 子级&lt;触发器&gt;。 |

`UpdatePanel`控件是分隔的服务器端的内容，将会在 AJAX Extensions 的部分呈现功能的一部分的控件。 在页上，可以是 UpdatePanel 控件的数量没有限制，并且它们可以嵌套。 每个 UpdatePanel 是隔离的以便可以独立工作，每个 （可具有两个 Updatepanel 运行一次呈现页面的不同部分独立于页面的回发）。

UpdatePanel 控件主要交易与控件的触发器-默认情况下，UpdatePanel 中包含的任何控件`ContentTemplate`创建回发的 UpdatePanel 注册为触发器。 这意味着，UpdatePanel 可以处理使用用户控件的默认数据绑定控件 （如 GridView)，而且可以在脚本中对它们进行编程。

默认情况下，触发部分页面呈现后，将刷新页面上的所有 UpdatePanel 控件，UpdatePanel 控件定义此类操作的触发器。 例如，如果一个 UpdatePanel 定义了一个按钮控件，并单击该按钮控件，将默认情况下刷新该页面上的所有 UpdatePanel 控件。 这是因为，默认情况下`UpdateMode`UpdatePanel 属性设置为`Always`。 或者，可能会将 UpdateMode 属性设置为`Conditional`，这意味着如果达到特定触发器才会刷新 UpdatePanel。

## <a name="custom-control-notes"></a>自定义控件说明

UpdatePanel 可以添加到任何用户控件或自定义控件;但是，这些控件可的页还必须包括 ScriptManager 控件与将属性设置为 EnablePartialRendering **，则返回 true**。

一种方法在其中你可能会考虑到这一点时使用 Web 自定义控件是重写受保护`CreateChildControls()`方法的`CompositeControl`类。 这样，您可以注入 UpdatePanel 控件的子级和外界之间确定页支持部分呈现; 如果否则，只是到容器层的子控件`Control`实例。

## <a name="updatepanel-considerations"></a>UpdatePanel 注意事项

UpdatePanel 用作内容黑箱包装 JavaScript XMLHttpRequest 的上下文中的 ASP.NET 回发。 但是，有显著的性能注意事项需要牢记，两个方面的行为和速度。 若要了解 UpdatePanel 的工作原理，以便能够最好地确定何时适合使用，则应检查 AJAX exchange。 下面的示例使用了 Firebug 扩展 （Firebug 捕获 XMLHttpRequest 数据） 使用的现有站点和，Mozilla Firefox。

请考虑具有的窗体，除此之外，邮政编码文本框中，这应填充窗体或控件上的城市和州字段。 此窗体最终收集成员身份信息，包括用户的名称、 地址和联系信息。 有很多设计注意事项，以考虑在内，根据特定项目的要求。


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


在此应用程序的原始迭代，控件已生成合并整个用户注册数据，包括邮政编码、 城市和状态。 整个控件已换行 UpdatePanel 和拖放到 Web 窗体上。 当用户输入的邮政编码时，UpdatePanel 检测到事件 （后端中，通过指定触发器或通过使用 ChildrenAsTriggers 属性设置为 true 的相应 TextChanged 事件）。 AJAX 发布所有 UpdatePanel 中的字段由 FireBug 捕获 （请参阅右侧的关系图）。

如屏幕截图，传递从 UpdatePanel 中的每个控件的值 （在这种情况下，它们是全部为空），以及视图状态字段。 总之，超过 9 kb 的数据时，发送，实际上只有五个字节的数据所需要进行此特定请求。 响应是更多臃肿： 总共 57 kb 发送到客户端，只是为了更新文本字段和下拉字段。

它还可能感兴趣，若要查看 ASP.NET AJAX 更新演示文稿的方式。 在左侧，在 Firebug 控制台显示中显示的 UpdatePanel 的更新请求的响应部分它是专门表述竖线分隔字符串，然后重新组合的页上，由客户端脚本分解。 具体而言，设置 ASP.NET AJAX *innerHTML*表示在 UpdatePanel 的客户端上的 HTML 元素的属性。 在浏览器将重新生成 DOM，没有稍有延迟，具体取决于需要处理的信息量。

DOM 的重新生成会触发一系列其他问题：


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([单击此项可查看原尺寸图像](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- 如果 UpdatePanel 内具有焦点的 HTML 元素，则它将失去焦点。 因此，对于用户按下 Tab 键以退出邮政编码文本框中，其下一步目标就已市/县文本框。 但是，UpdatePanel 刷新显示后, 在窗体将不再具有焦点，并按 Tab 应开始突出显示的焦点元素 （如链接）。
- 如果在使用为任何类型的自定义客户端脚本访问 DOM 元素，引用保存的函数可能会变得不起作用后部分回发。

Updatepanel 并不是全方位的解决方案。 相反，它们提供快速解决方案对于某些情况下，包括原型制作，小型控制的更新，并向 ASP.NET 开发人员可能会熟悉的.NET 对象模型，但这样不太与 dom 一起提供熟悉的界面 有了一些可能会导致更好的性能，具体取决于应用程序方案的替代方法：

- 请考虑使用 PageMethods 和 JSON （JavaScript 对象表示法），开发人员可调用静态方法在页面上的，如同正在调用 web 服务调用。 方法是静态的因为没有状态是必需的;脚本调用方提供参数，并以异步方式返回结果。
- 请考虑使用 Web 服务和 JSON，如果单个控件需要跨应用程序在多个位置使用。 这再次需要特殊工作很少，并且以异步方式工作。

将通过 Web 服务或页面方法的功能纳入有也缺点。 首先，这一 ASP.NET 开发人员通常倾向于构建到用户控件 （.ascx 文件） 的功能的小组件。 页面方法不能托管在这些文件;它们必须托管在实际的.aspx 页类。 Web 服务，同样，必须承载.asmx 类中。 根据应用程序，此体系结构可能会违反单一责任原则，在于单个组件的功能现在分布在两个或多个物理组件可能具有很少或没有紧密结合的等同值。

最后，如果应用程序要求使用 Updatepanel，指导以下原则可帮助进行故障排除和维护。

- **嵌套 Updatepanel 降至最低，不仅在单位，而且还跨代码单元。** 例如，UpdatePanel 包装一个控件，而该控件还包含 UpdatePanel，其中包含 UpdatePanel 的另一个控件的页面上是跨单元嵌套。 这有助于清楚哪些元素应刷新，并对子级 Updatepanel 防止意外的刷新。
- **保持*ChildrenAsTriggers*属性设置为 false，并显式设置触发事件。** 利用`<Triggers>`集合是一种更清晰的方式来处理事件，并可能会阻止意外的行为，帮助维护任务，并强制开发人员参加的事件。
- **使用可能的最小单位来实现的功能。** 中所述的邮政编码服务，包装讨论仅的最低要求将时间减少到的服务器、 总处理和客户端-服务器交换的占用空间，从而增强的性能。

## <a name="the-updateprogress-control"></a>UpdateProgress 控件

## <a name="updateprogress-control-reference"></a>UpdateProgress 控件引用

已启用标记的属性：

| **属性名称** | **Type** | **说明** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | 指定此 UpdateProgress 应报告的 UpdatePanel 的 ID。 |
| DisplayAfter | Int | 异步请求开始后显示此控件之前，以毫秒为单位指定的超时。 |
| DynamicLayout | bool | 指定是否动态呈现进度。 |

标记后代：

| **标记** | **说明** |
| --- | --- |
| &lt;ProgressTemplate&gt; | 包含控件模板设置将与此控件显示的内容。 |

UpdateProgress 控件提供反馈，以执行必要的操作传输到服务器时保留用户的感兴趣的度量值。 这可以帮助用户知道你在进行一些即使它不太容易，尤其是因为大多数用户将使用到的页面，刷新并看到突出显示状态栏。

请注意，UpdateProgress 控件可以在页层次结构上任意位置出现。 但是，在其中部分回发从子 UpdatePanel （其中 UpdatePanel 嵌套在另一 UpdatePanel） 启动的情况下，在回发触发器 UpdatePanel 将导致 UpdateProgress 模板要显示的子级的子级UpdatePanel，以及父 UpdatePanel。 但是，如果触发器为父 UpdatePanel 的直接子级，则只有与父项关联的 UpdateProgress 模板将显示。

## <a name="summary"></a>总结

Microsoft ASP.NET AJAX 扩展是复杂的产品旨在以帮助使 web 内容更易于访问，从而提供更丰富的用户体验到 web 应用程序。 ASP.NET AJAX Extensions 部分页面呈现控件，包括 ScriptManager，UpdatePanel 和 UpdateProgress 控件将将的一些最明显的工具包的组件。

ScriptManager 组件集成，客户端 JavaScript 的预配扩展，以及实现要一起使用很少的开发投资的各种服务器和客户端组件。

UpdatePanel 控件是明显的神奇框-UpdatePanel 中的标记可以具有服务器端代码隐藏文件并不会触发页面刷新。 UpdatePanel 控件可以嵌套，并且可以依赖于其他 Updatepanel 中的控件。 默认情况下，Updatepanel 尽管此功能可以进行更细致优化，以声明方式或以编程方式处理由其后代控件调用任何回发。

当使用 UpdatePanel 控件，开发人员应注意可能时出现的性能影响。 潜在的替代方案包括 web 服务和页面方法，但应考虑应用程序的设计。

UpdateProgress 控件允许用户知道她或他不会被忽略，并且页面时究竟幕后请求是不采取任何措施来响应用户输入。 它还包括中止部分呈现结果的能力。

在一起，这些工具可帮助进行服务器工作对用户不太明显不中断工作流来创建丰富的无缝用户体验。

## <a name="bio"></a>个人简介

Scott Cate 自 1997 年以来一直致力于 Microsoft Web 技术和 myKB.com 总裁 ([www.myKB.com](http://www.myKB.com)) 专门负责编写 ASP.NET 基于侧重于知识库软件解决方案的应用程序。 可以通过电子邮件联系 Scott [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)或他的博客[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [下一页](understanding-asp-net-ajax-updatepanel-triggers.md)
