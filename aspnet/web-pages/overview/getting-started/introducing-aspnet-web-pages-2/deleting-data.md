---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introducing ASP.NET 网页-正在删除数据库数据 |Microsoft Docs
author: Rick-Anderson
description: 本教程演示如何删除单独的数据库条目。 它假定你已完成通过在 ASP.NET Web 的 pa。 中更新数据库数据系列...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020437"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET 网页简介-正在删除数据库数据
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程演示如何删除单独的数据库条目。 它假定你已完成通过时序[更新数据库数据在 ASP.NET Web Pages](updating-data.md)。
> 
> 你将学习：
> 
> - 如何从记录列表中选择的单个记录。
> - 如何从数据库中删除一条记录。
> - 如何检查特定按钮被单击窗体中。
>   
> 
> 功能/技术讨论：
> 
> - `WebGrid`帮助器。
> - SQL`Delete`命令。
> - `Database.Execute`方法来运行 SQL`Delete`命令。


## <a name="what-youll-build"></a>你将生成

在前面的教程，您学习了如何更新现有的数据库记录。 本教程是类似，只不过而不是更新记录，则会将其删除。 进程是大致相同，只不过删除是更简单，因此本教程将会比较短。

在*电影*页上，你将更新`WebGrid`帮助程序，以便它显示**删除**旁边伴随每个电影链接**编辑**前面添加的链接。

![电影页面，其中显示每个电影的删除链接](deleting-data/_static/image1.png)

使用编辑，单击**删除**链接，它将您带入不同的页的电影信息已在窗体：

![删除具有显示电影的电影页](deleting-data/_static/image2.png)

然后可以单击按钮后，若要永久删除的记录。

## <a name="adding-a-delete-link-to-the-movie-listing"></a>将删除链接添加到电影列表

将首先添加**删除**链接到`WebGrid`帮助器。 此链接是类似于**编辑**添加上一教程中的链接。

打开*Movies.cshtml*文件。

更改`WebGrid`通过添加列页的正文中的标记。 下面是修改后的标记：

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

新列是这样一个：

[!code-html[Main](deleting-data/samples/sample2.html)]

在网格的配置的方式**编辑**列是在网格中最左侧并**删除**列是最右侧。 (没有逗号之后`Year`列现在，如果您没有注意到的。)没有什么特别之处这些链接列在哪里，并可以轻松将其彼此。 在这种情况下，它们是单独以使其难以搞乱套了。

![电影页面包含一个编辑和详细信息的链接标记以显示它们彼此不是](deleting-data/_static/image3.png)

新列将显示一个链接 (`<a>`元素) 的文本显示的是"删除"。 链接的目标 (其`href`属性) 是具有最终解析为此 URL，类似的代码`id`每部电影的不同值：

[!code-css[Main](deleting-data/samples/sample3.css)]

此链接将调用一个名为页*DeleteMovie*并将其传递所选的电影 ID。

本教程不会详细介绍如何构造此链接，因为它几乎等同于**编辑**以前一教程的链接 ([更新数据库数据在 ASP.NET Web Pages](updating-data.md))。

## <a name="creating-the-delete-page"></a>创建删除页

现在，你可以创建将用于目标的页面**删除**网格中的链接。

> [!NOTE] 
> 
> **重要**首先选择要删除的记录，然后使用一个单独的页面和按钮以确认该过程的方法是非常重要的安全。 如您所读在前面的教程，使*任何*更改对你的网站的排序应*始终*来完成使用窗体&mdash;使用 HTTP POST 操作。 如果您成为可能只需通过单击的链接 （即使用 GET 操作） 更改的站点，人员无法向你的站点发出简单请求和删除你的数据。 甚至一个搜索引擎爬网程序，索引您的网站可能无意中删除数据，只需通过以下链接。
> 
> 当你的应用让他人更改记录时，必须编辑仍要向用户显示该记录。 但您可能想要跳过此步骤中的删除一条记录。 不但是跳过该步骤。 （该技术还有助于用户可以看到记录，并确认他们要删除它们的记录。）
> 
> 在后续教程集中，您将了解如何添加登录功能，因此用户需要登录，然后再删除记录。


创建一个名为页*DeleteMovie.cshtml*和替换为以下标记文件中：

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

此标记就像*EditMovie*页中的，不同之处在于而不是使用文本框 (`<input type="text">`)，标记包括`<span>`元素。 此处没有任何编辑。 只需是显示电影详细信息，以便用户可以确保它们要删除右电影。

标记已包含允许用户返回到影片列表页的链接。

在中作为*EditMovie*页上，将所选电影 ID 存储在隐藏字段中。 （它传递到页中第一个位置中作为查询字符串值。）没有`Html.ValidationSummary`将显示验证错误的调用。 在这种情况下，该错误可能是任何电影 ID 已传递到页或电影 ID 无效。 如果有人运行而无需首先选择电影中的此页，可能会发生这种情况下*电影*页。

该按钮标题**删除电影**，并且其名称属性设置为`buttonDelete`。 `name`属性将使用在代码中，用于标识提交窗体的按钮。

您必须编写代码以 1） 当第一次显示页面时读取电影详细信息，以及 2） 实际删除此电影，当用户单击按钮。

## <a name="adding-code-to-read-a-single-movie"></a>添加代码来读取单个电影

在顶部*DeleteMovie.cshtml*页上，添加以下代码块：

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

此标记是中的相应代码相同*EditMovie*页。 获取查询字符串外的电影 ID，并使用 ID 来从数据库读取一条记录。 该代码包括验证测试 (`IsInt()`和`row != null`) 以确保传递到页电影 ID 是否有效。

请记住此代码应仅运行该页面运行第一次。 不想要重新读取电影记录数据库中的，当用户单击**删除电影**按钮。 因此，代码读取电影是内部测试表明`if(!IsPost)` &mdash; ，即*如果请求不是 post 操作 （表单提交）*。

## <a name="adding-code-to-delete-the-selected-movie"></a>添加代码以删除所选的电影

若要删除电影，当用户单击按钮时，添加以下代码置于右大括号的`@`块：

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

此代码是类似的代码更新现有记录，但更简单。 基本上，代码将运行 SQL`Delete`语句。

 在中作为*EditMovie*页上，代码都位于`if(IsPost)`块。 这一次，`if()`条件是稍微复杂一些： 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

有以下两个条件。 第一种是，正在提交页面，如您所见之前&mdash; `if(IsPost)`。

第二个条件是`!Request["buttonDelete"].IsEmpty()`，这意味着该请求具有一个名为`buttonDelete`。 不可否认，它是测试哪个按钮提交窗体的间接方法。 如果窗体包含多个提交按钮，只有被单击的按钮的名称将显示在请求中。 因此，在逻辑上，如果特定按钮的名称将出现在请求&mdash;如所述在代码中，如果该按钮不为空或&mdash;是提交窗体的按钮。

`&&`运算符表示"和"（逻辑与）。 因此整个`if`条件是...

*此请求为 post （而不是首次请求）*  
  
 AND  
  
`buttonDelete` *按钮已提交窗体的按钮。*

（事实上，此页） 此窗体包含只有一个按钮，因此其他测试`buttonDelete`从技术上讲不是必需的。 尽管如此，您要执行的操作将永久删除数据。 因此，你想要为确保尽可能仅当用户已显式请求时要执行该操作。 例如，假设您稍后扩展此页，并向其添加其他按钮。 仅当删除电影的代码将运行即使如此，`buttonDelete`所单击的按钮。

在中作为*EditMovie*页上，从隐藏字段获取 ID，然后运行 SQL 命令。 语法`Delete`语句是：

`DELETE FROM table WHERE ID = value`

务必包括`WHERE`子句和 id。 如果省略 WHERE 子句中，*表中的所有记录将被都删除*。 如您所见，您的 ID 值传递给 SQL 命令使用占位符。

## <a name="testing-the-movie-delete-process"></a>测试电影删除过程

现在，可以测试。 运行*电影*页，然后单击**删除**旁边电影。 当*DeleteMovie*页面出现后，单击**删除电影**。

![删除突出显示的按钮删除电影的电影页](deleting-data/_static/image4.png)

当单击按钮时，代码将删除电影，并返回到影片列表。 可以在曲终搜索已删除的电影，并确认它已被删除。

## <a name="coming-up-next"></a>即将推出下一步

下一步的教程演示了如何授予您的站点上的所有页面，常见的外观和布局。

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>电影页面 （使用删除链接，更新） 的完整列表

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>DeleteMovie 页的完整列表

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>其他资源

- [使用 Razor 语法的 ASP.NET Web 编程简介](../introducing-razor-syntax-c.md)
- [SQL DELETE 语句](http://www.w3schools.com/sql/sql_delete.asp)W3Schools 站点上

> [!div class="step-by-step"]
> [上一页](updating-data.md)
> [下一页](layouts.md)
