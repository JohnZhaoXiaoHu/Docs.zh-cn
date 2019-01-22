---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter 使用 ASP.NET Web 页面的帮助程序 |Microsoft Docs
author: Rick-Anderson
description: 此主题和应用程序演示如何将 Twitter 帮助程序添加到 WebMatrix 3 项目。 它包含的 Twitter 帮助程序代码，并演示如何调用帮助器...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: fabe9f2b84d278a766dc8d8b7bfc00e1eb967127
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299425"
---
<a name="twitter-helper-with-aspnet-web-pages"></a>使用 ASP.NET 网页的 twitter 帮助程序
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Twitter 帮助程序已过时。 关于 Twitter 的最新 engagement 工具的网站，请参阅[Twitter 网站概述](https://developer.twitter.com/en/docs/twitter-for-websites/overview)。

> 此主题和应用程序演示如何将 Twitter 帮助程序添加到 WebMatrix 3 项目。 它包含的 Twitter 帮助器代码，并演示了如何调用帮助器方法。
> 
> 这段代码用于 Twitter.cshtml 文件由开发**天平移**Microsoft。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教程中使用的软件版本
> 
> 
> - ASP.NET 网页 (Razor) 3
>   
> 
> 本教程还适用于 ASP.NET Web Pages 2。


## <a name="introduction"></a>介绍

本主题演示如何将 Twitter 帮助程序添加到你的应用程序并使用 Razor 语法来调用帮助器方法。 Twitter 帮助程序轻松合并 Twitter 按钮和应用程序中的小组件。 若要使用 Twitter 小组件，如用户的时间线或井号标签的搜索结果必须先创建[Twitter 上的小组件](https://twitter.com/settings/widgets)。 创建小组件后, 会收到一个小组件 id。显示小组件的帮助器方法在调用时作为参数传递此小组件 id。

本主题已针对 Twitter API 的 1.1 版编写。 通过直接将 Twitter 帮助程序代码添加到你的项目，可以更新的帮助器代码，如果 Twitter API 发生更改。

有关 WebMatrix 的安装信息，请参阅[引入了 ASP.NET Web Pages 2-Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md)。

## <a name="add-twitter-helper-to-your-project"></a>将 Twitter 帮助程序添加到你的项目

若要添加 Twitter 帮助程序，首先，添加名为的文件夹**应用程序\_代码**到你的项目。 然后，创建名为的文件**Twitter.cshtml**。

![App_Code 文件夹](twitter-helper/_static/image1.png)

Twitter.cshtml 中的默认代码替换为以下代码。

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>从您的 web 页面调用 Twitter 方法

下面的示例演示如何在项目中使用从一个页面的 Twitter 帮助器方法。 在项目中，你将想要的参数值替换为与你的需求相关的值。 可以使用提供的小组件 id 来探索方法的工作方式，但想要生成你自己的小组件为你的项目。

需要不是所有的参数如下所示。 可选参数用于自定义按钮或小组件的显示方式。 例如，执行按钮只需要用户名称，若要执行，但该示例演示如何包含数量的关注者，并指定大小的按钮和语言的方式。

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>查看结果

上面的代码生成以下按钮和小组件。 这些按钮和小组件是功能完整的不是屏幕截图。 请按照按钮显示在西班牙语中，因为语言参数设置为**es**。

### <a name="follow-button"></a>请按照按钮

[请按照@aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>推文按钮

[推文](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="user-timeline-profile"></a>用户时间线 （配置文件）

[通过推文 @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>收藏夹

[收藏的推文作者 @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>列表

[从推文@Microsoft/MS\_使用者\_带区](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>搜索

[有关推文&quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
