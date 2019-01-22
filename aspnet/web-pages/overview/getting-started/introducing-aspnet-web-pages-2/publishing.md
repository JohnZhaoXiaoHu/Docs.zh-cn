---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Introducing ASP.NET 网页-通过使用 WebMatrix 发布站点 |Microsoft Docs
author: Rick-Anderson
description: 本教程是在引入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教程系列中最后一期。 介绍了如何发布站点 t...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: bd6611a03ee4940f5d4176ce23464f313b9ec884
ms.sourcegitcommit: eef99d14d96dc8c3c1bb0e2c4cb14da152f8a952
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "53022500"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>ASP.NET 网页简介-通过使用 WebMatrix 发布站点
====================
通过[Tom FitzMacken](https://github.com/tfitzmac)

> 本教程是在引入了 ASP.NET Web Pages 和 Microsoft WebMatrix 的教程系列中最后一期。 它讨论了如何将您的网站发布到 Internet，以便其他人可以使用它。 它假定你已完成通过时序[为 ASP.NET Web Pages 站点创建一致的查看](https://go.microsoft.com/fwlink/?LinkId=251585)。
> 
> 您将学习如何发布站点使用：
> 
> - Microsoft Azure
> - Web 托管公司


## <a name="about-publishing-your-site"></a>有关发布站点

到目前为止，已完成您的本地计算机上，包括测试您的页面的所有工作。 若要运行你<em>.cshtml</em>页中，你已使用内置到 WebMatrix 中，即 IIS Express web 服务器。 但当然没有人可以看到已创建除您的站点。 若要让其他人使用你的站点，必须将其发布到 Internet。

除非已有权访问公共 web 服务器，否则发布意味着，必须有一个帐户*云平台*或*托管提供商*。 Microsoft Azure 等云平台，为应用程序提供按需基础结构。 宿主提供程序是一家公司拥有可公开访问的 web 服务器和，将通过您租借空间为您的网站。 托管计划运行从每个月的几个美元 （或甚至免费） 的小型站点添加到由数以百计的一个月，提供大量商业网站几美元。

> [!NOTE]
> 可能有权通过 internet 服务提供商 (ISP) 用来获取在家里 internet 服务的公共 web 服务器。 但是，托管提供商必须支持 ASP.NET Web Pages。 很多 Isp 不这样做，但最好始终检查。


在本教程中，我们将提供如何将发布的概述。 因为过程稍微不同的每个宿主提供程序，它并不可行的所有内容，提供有关确切详细信息。 但您仍然可以清楚地了解该过程的工作原理。

本教程包含四个部分：

1. [设置默认页](#defaultpage)
2. 发布 （选择以下值之一）  
 a. [你的站点发布到 Microsoft Azure](#azure)  
 b. [将站点发布到 Web 托管公司](#host)
3. [更新实时站点： 重新发布](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>设置默认页

当用户导航到你的网站的基址时，向用户显示你的站点的默认页。 例如，当*Default.htm*设置为在站点的默认页面`www.contoso.com`，然后导航到`www.contoso.com`导航到相同`www.contoso.com/Default.htm`。

目前，您的网站使用**Default.cshtml**与默认页面。 此页是相当不错的默认页面，但在本教程中未添加任何内容到该页面以便它将显示空白页。 打开 Default.cshtml 并将内容替换为以下代码。

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

现在你的站点是准备好进行发布。 首先，您将看到如何将站点部署到 Azure，以及如何将其部署到 web 托管公司。 任一选项适用于您的网站，并且您只需遵循的部署选项之一。

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>你的站点发布到 Microsoft Azure

本教程将首先演示如何将您的网站部署到 Microsoft Azure。 通过使用 Microsoft 帐户登录，可以在 Azure 上创建最多 10 个免费网站。 这些免费站点提供了方便地测试您的站点。 您始终可以删除此示例站点更高版本以避免使用所有免费站点。 只需几分钟，可以创建一个免费试用帐户。 有关详细信息，请参阅[Azure 免费试用版](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

在 WebMatrix 功能区中，单击**发布**按钮。

![WebMatrix 功能区中的发布按钮](publishing/_static/image1.png)

**发布你的站点**显示对话框。 如果你有未登录到你的 Microsoft 帐户，将包含对话框**开始使用 Azure**链接。 单击此链接。

![发布站点](publishing/_static/image2.png)

如果你不具有登录 Microsoft 帐户，您再次提供机会进行登录。 您必须登录到 Microsoft 帐户来发布 azure 网站。

![登录](publishing/_static/image3.png)

登录到你的 Microsoft 帐户后, 对话框中包含用于在 Azure 上创建新的站点或连接到一个现有的站点在 Azure 上的链接。

![创建新的站点](publishing/_static/image4.png)

选择**创建新的站点**。

如果名为你的项目**WebPagesMovies**，将为你的站点的默认名称**webpagesmovies.azurewebsites.net**。 此默认名称是最有可能不可用，红色感叹号标记所示。

![默认 Web 站点名称](publishing/_static/image5.png)

将该站点的名称更改为可不可用，并选择靠近所在位置的位置。

![已更改的站点名称](publishing/_static/image6.png)

单击 **“确定”**。

WebMatrix performss 测试，确定服务器是否与你的站点兼容。

![兼容性测试](publishing/_static/image7.png)

选择**继续**。

显示兼容性测试的结果。

![兼容性结果](publishing/_static/image8.png)

选择**继续**。

WebMatrix 显示文件和将发布到网站的数据库。 由于这是要将站点发布第一次，会列出所有文件。 你可以取消选中未准备好要发布的文件。 在后续的发布，将显示已更改的文件。 请参阅[更新实时站点： 重新发布](#update)。

![发布预览](publishing/_static/image9.png)

选择**继续**。

站点部署到 Azure 后，将显示一条消息指示已完成部署。

![发布完成](publishing/_static/image10.png)

站点和数据库已发布到 Azure，并且现在可供公众。 单击消息，指示发布完成后，现在，您将看到你已部署的站点中的链接。 你或 Internet 访问权限的任何人都可以添加或修改数据库中的记录。

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>将站点发布到 Web 托管公司

如果您决定不将发布到 Azure，你可以改为将你的站点发布到 web 托管公司。

单击**查找 web 托管**链接。

![在发布设置对话框中的查找 web 托管按钮](publishing/_static/image12.png)

列出了支持 ASP.NET 的托管提供商的 Microsoft 站点上进入一个页面。

![列出了宿主提供程序的 Microsoft 网站上的页](publishing/_static/image13.png)

显然，可能很难知道现在完全从长远来说可能需要哪些托管功能。 下面是一些要考虑的事项：

- 对于 WebPagesMovies 站点，您无需具有单独的外接程序对于 SQL Server，通常的额外费用。 在网站中，使用 SQL Server Compact Edition，这自包含。 但是，您可能需要访问 SQL Server 为你做一些将来的网站工作。 如果您认为您可能，请确保可以稍后添加 SQL Server 功能。
- 检查宿主提供程序是否支持 Web 部署发布协议。 可以使用 FTP 协议发布但更方便地使用 Web 部署。

某些站点提供免费试用期。 免费试用版是尝试进行发布的好办法，承载时仍与 WebMatrix 和 ASP.NET Web Pages 正在试验。

选择您喜欢的一个。 对于本教程中，我们选择 DiscountASP.NET，因为虽然我们已创建本教程，该公司必须让人们托管站点的几个月免费升级。

> [!NOTE]
> 我们选择的本教程中的托管提供程序不应通过任何其他解释为代表该公司的认可。 但我们需要选择其中一个是为了进行说明，和 DiscountASP.NET 是许多支持 ASP.NET Web Pages 和 Web Deploy 协议用于发布的公司之一。


通常情况下，您已经使用宿主提供程序注册后，公司向你发送一封电子邮件，其中包含用户名和密码，web 服务器和等等的 URL。 如果托管的公司支持 Web Deploy 协议，它们可能会发送包含的文件发布设置，也可以让您下载一个。 发布设置文件为您简化过程。

已注册且已准备好发布，请单击**发布**WebMatrix 功能区中的按钮。 **发布设置**显示对话框。

如果托管提供商向你发送了发布设置文件，请单击**导入发布设置**链接，然后导入文件。 如果你没有发布设置文件，填写字段通过使用托管公司向你发送电子邮件中的值。 下面是什么**发布设置**对话框可能如下所示完成后：

![在发布设置对话框中填写发布设置](publishing/_static/image14.png)

单击**验证连接**。 如果所有内容是确定，对话框的报告**已成功连接**，这意味着它可以与宿主提供程序的服务器进行通信。

![成功消息，如果将发布设置正确无误](publishing/_static/image15.png)

如果没有问题，WebMatrix 会尽力告诉您的问题是：

![如果没有问题的错误消息将发布设置](publishing/_static/image16.png)

单击**保存**以保存设置。 WebMatrix 提供了执行测试以确保，它可以正确地与宿主站点通信：

![产品/服务来执行发布过程的测试消息](publishing/_static/image17.png)

单击 **“是”**。 WebMatrix 将一些示例文件上传到宿主提供程序。 完成操作后兼容性测试，WebMatrix 将报告结果：

![发布测试结果](publishing/_static/image18.png)

如果您准备好，请继续并单击**继续**真正开始发布过程。 WebMatrix 将计算出什么文件位于你的站点和 （稍后再试，无） 的主机服务器上已以及您提供的发布过程预览：

![哪些发布过程将上传的文件的预览](publishing/_static/image19.png)

要发布的文件列表包括已创建与类似的网页*Movies.cshtml*。 列表还包括帮助程序的已安装的数据库，运行 SQL Server Compact Edition 等的文件的文件。 因此，在初始发布过程可能很高。

单击 **“继续”**。 WebMatrix 将你的文件复制到托管提供商的服务器。 完成后，状态栏中报告结果：

![状态条消息时已成功完成发布过程](publishing/_static/image20.png)

若要查看实时站点，请单击状态栏中的链接。 添加*电影*到的 URL，您将看到*Movies.cshtml*你创建的文件：

![显示电影页面在实时站点](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>更新实时站点： 重新发布

一旦您已将站点发布 （到 Azure 或 web 托管公司），有两份&mdash;上您的计算机和服务提供程序上的版本的版本。 您可能需要在继续开发站点 (如果没有其他下, 一步的教程系列的一部分)。 执行操作时，您必须重新发布才能将更改从您的计算机复制到服务提供商的站点。 在 WebMatrix 中的发布过程可以确定您的站点上，文件已更改内容和发布的那些文件。

若要查看重新发布的工作原理，请打开*Movies.cshtml*站点、 进行一些小更改，并保存该文件。 例如，将标题更改为`Movies - Updated`。

单击**发布**中功能区按钮。 WebMatrix 确定哪些更改并将发布的文件的位置处显示。

![发布对话框中显示已更改文件可供重新发布](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> 默认情况下，WebMatrix 发布你的数据库 (*.sdf*文件) 仅在发布站点的第一次。 一旦发布你的站点，并与网站交互的人员，实时站点上的数据库通常具有站点的真实数据。 一定要非常小心，不要覆盖实时数据库与 *.sdf*是您通常只包含测试数据的计算机的文件。 这就是为什么您会看到警告**发布将覆盖任何远程数据库**，以及为什么的复选框*WebPagesMovies.sdf*默认情况下清除。


单击 **“继续”**。 WebMatrix 发布已更改的文件，并显示一条成功消息，像第一次发布。

转到实时网站 （如果仍显示可单击成功消息中的链接），并验证所做的更改已发布。

> [!TIP] 
> 
> **远程编辑文件**
> 
> 为更改你的站点，然后重新发布的替代方法，可以编辑直接在 WebMatrix 中的远程文件。 在此方案中，打开位于服务提供商，文件和 WebMatrix 下载它为您要编辑的副本。 每次保存该文件，WebMatrix 会将所做的更改发送到站点。
> 
> 远程编辑是轻松地对实时站点进行更改。 但是，进行这种方式的更改不会与你的本地站点中的文件同步。 若要同步的本地文件与远程站点，可以下载远程文件。 此过程工作方式非常类似发布，除非按相反的顺序。
> 
> 我们不会详细介绍的远程编辑和远程下载功能的 WebMatrix 此处。 它们非常有用，如果多个用户必须在不同的计算机上的同一站点上处理。 有关详细信息，请参阅[发布和编辑远程站点与 WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591)。


## <a name="additional-resources"></a>其他资源

- [ASP.NET WebMatrix ASP.NET Web Pages 论坛](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages)，一个很好的发布问题和获取答案。

> [!div class="step-by-step"]
> [上一篇](layouts.md)
