---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 使用 Visual Studio 的 ASP.NET Web 部署： 故障排除 |Microsoft Docs
author: tdykstra
description: 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure 应用服务 Web 应用或第三方托管提供商，通过使用...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 65cd5cd9f7d1f9c5fdaea9b0d16bdfd84259efdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824887"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>使用 Visual Studio 的 ASP.NET Web 部署： 故障排除
====================
通过[Tom Dykstra](https://github.com/tdykstra)

[下载初学者项目](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教程演示如何部署 （发布） ASP.NET web 应用程序到 Azure 应用服务 Web 应用或第三方托管提供商，通过使用 Visual Studio 2012 或 Visual Studio 2010。 有关序列的信息，请参阅[系列中的第一个教程](introduction.md)。


此页介绍了使用 Visual Studio 部署 ASP.NET web 应用程序时可能出现的一些常见问题。 为每个提供了一个或多个可能的原因和相应的解决方案。

所示的方案适用于 Azure 和第三方主机托管提供商。 有关 Azure 应用服务中的 web 应用进行故障排除的详细信息，请参阅以下资源：

- [使用 Visual Studio 对 Azure 应用服务中的 Web 应用进行故障排除](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [在 Azure 应用服务中监视 Web 应用](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [宣布推出 Windows Azure SDK 2.0 for.NET 的版本](http:// https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) （ScottGu 的博客，演示了如何在 Visual Studio 中获取诊断日志）

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>服务器错误 '/' 应用程序-在当前的自定义错误设置防止错误的详细信息查看远程

### <a name="scenario"></a>方案

将站点部署到远程主机后, 您将得到错误消息，指出 Web.config 文件中的 customErrors 设置但并不指示实际错误的原因是：

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，ASP.NET 仅当 web 应用程序运行在本地计算机上时显示详细的错误信息。 通常您不想要在 web 应用程序是 internet 上公开可用，因为黑客可能能够使用此信息来查找该应用程序中的漏洞时显示详细的错误信息。 但是，当要将站点或更新部署到站点，有时都会出现错误，您需要先获取实际的错误消息。

若要启用要在远程主机上运行时显示详细的错误消息的应用程序，请编辑 Web.config 文件以将 customErrors 模式设置为 off，重新部署应用程序，并再次运行应用程序：

1. 如果应用程序 Web.config 文件具有 acustomErrors 元素 thesystem.web 元素中，更改 themode 属性为"关闭"。 否则 acustomErrors 元素中添加 thesystem.web 元素 themode 属性设置为"off"，如下面的示例中所示： 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. 部署应用程序。
3. 运行应用程序并重复任何内容，请像前面导致错误发生。 现在可以看到实际的错误消息是什么。
4. 解决了错误，恢复原始 customErrors 设置并重新部署应用程序。

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>无法创建/卷影副本 ContosoUniversity 该文件已存在时。

### <a name="scenario"></a>方案

当您尝试在 Visual Studio 中运行项目时，获取一条消息，如下例所示的错误页：

'/' 应用程序中的服务器错误。 无法创建/卷影副本 ContosoUniversity 该文件已存在时。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

稍等片刻并刷新浏览器，或重新编译站点并尝试再次运行。

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>访问被拒绝在网页中使用 SQL Server Compact

### <a name="scenario"></a>方案

当部署使用 SQL Server Compact 的站点，并且在访问数据库所部署站点中运行一个页面时，您将看到以下错误消息：

拒绝访问。 (异常来自 HRESULT: 0x80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

在服务器上的网络服务帐户必须是可以读取 SQL Compact 服务中的本机二进制文件*bin\amd64*或*bin\x86*文件夹中，但它没有读取这些文件夹的权限。 读取在网络服务的权限集*bin*文件夹，并确保将子文件夹的权限扩展。

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>无法读取配置文件，由于没有足够的权限

### <a name="scenario"></a>方案

当您单击 Visual Studio 发布按钮以部署到 IIS 应用程序在本地计算机上的发布将失败并**输出**窗口会显示一条错误消息类似于此：

读取 IIS 配置文件 ' MACHINE/重定向时出错。 执行此操作的标识是...错误： 无法读取配置文件，由于没有足够的权限。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

若要使用一键式发布到 IIS 在本地计算机上，您必须具有管理员权限运行 Visual Studio。 关闭 Visual Studio，使用管理员权限重新启动它。

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>无法连接到目标计算机...使用指定的进程

### <a name="scenario"></a>方案

当您单击 Visual Studio 发布按钮部署应用程序，发布将失败并**输出**窗口会显示一条错误消息类似于此：

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

代理服务器与目标服务器的通信中断。 在 Windows 控制面板或 Internet 资源管理器中，选择**Internet 选项**，然后选择**连接**选项卡。在中**互联**对话框中，单击**LAN 设置**。 在中**所在的局域网 (LAN) 设置**对话框中，清除**自动检测设置**复选框。 然后再次单击发布按钮。

如果问题仍然存在，请联系系统管理员联系，以确定与代理或防火墙设置可以做什么。 因为 Web Deploy Web 管理服务部署 (8172); 使用非标准端口仍然出现该问题对于其他连接，Web 部署使用端口 80。 当您要部署到第三方托管提供商时，您通常使用 Web 管理服务。

## <a name="default-net-40-application-pool-does-not-exist"></a>默认.NET 4.0 应用程序池不存在

### <a name="scenario"></a>方案

在部署的应用程序需要.NET Framework 4 时，您将看到以下错误消息：

默认.NET 4.0 应用程序池不存在或无法添加该应用程序。 请验证此计算机上安装了 ASP.NET 4.0。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

ASP.NET 4 未安装在 IIS 中。 如果要部署到的服务器是在开发计算机上安装 Visual Studio 2010，ASP.NET 4 的计算机上安装，但可能未安装在 IIS 中。 要部署到服务器上, 打开提升的命令提示符并安装在 IIS 中的 ASP.NET 4 中，通过运行以下命令：

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

您可能还需要手动设置默认应用程序池的.NET Framework 版本。 详细信息，请参阅本系列中在测试环境教程部署到 IIS。

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>初始化字符串的格式不符合从索引 0 处开始的规范。

### <a name="scenario"></a>方案

部署应用程序使用一次单击后发布，当你运行的页访问该数据库获得以下错误消息：

初始化字符串的格式不符合从索引 0 处开始的规范。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

打开*Web.config*文件中的已部署的站点和检查以查看连接字符串值是否以 $ 开头 (ReplacableToken\_，如下面的示例：

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

如果连接字符串看起来像此示例中，编辑项目文件，并将以下属性添加到用于所有生成配置的 PropertyGroup 元素：

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

然后重新部署应用程序。

## <a name="http-500-internal-server-error"></a>HTTP 500 内部服务器错误

### <a name="scenario"></a>方案

运行已部署的站点时，您将看到以下错误消息，而无需指示错误的原因的特定信息：

HTTP 错误 500-内部服务器错误。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

有许多原因的 500 错误，但如果您按照这些教程中的一个可能原因是将 XML 元素放在一个 Web.config 转换文件中错误的位置。 例如，您会遇到此错误，如果将插入的转换&lt;位置&gt;元素下的&lt;system.web&gt;而不是正下方&lt;配置&gt;。 Web.config 转换预览功能可用于验证转换按预期方式工作。 如果找到一个转换，该编码不正确的解决方案是更正转换文件，并重新部署。 如果错误并不明显，请尝试注释掉转换并重新部署，若要查看哪一个导致 500 错误。

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 内部服务器错误

### <a name="scenario"></a>方案

当运行已部署的站点时，您将看到以下错误消息：

HTTP 错误 500.21-内部服务器错误。 处理程序"PageHandlerFactory 集成"具有错误的模块"ManagedPipelineHandler"在其模块列表中。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

该站点已部署 ASP.NET 4 中，但 ASP.NET 4 未注册在 IIS 中的服务器的目标。 在服务器上打开提升的命令提示符并运行以下命令来注册 ASP.NET 4:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

您可能还需要手动设置默认应用程序池的.NET Framework 版本。 详细信息，请参阅本系列中在测试环境教程部署到 IIS。

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>登录失败的应用中的打开 SQL Server Express 数据库\_数据

### <a name="scenario"></a>方案

您提供最新*Web.config*文件连接字符串以指向 SQL Server Express 数据库作为 *.mdf*文件中您*应用\_数据*文件夹中，并且第一个运行应用程序，请参阅以下的错误消息的时间：

System.Data.SqlClient.SqlException： 无法打开数据库"数据库名称"请求的登录名。 登录失败。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

名称 *.mdf*文件不能匹配任何曾经存在您的计算机的 SQL Server Express 数据库的名称，即使你删除 *.mdf*以前存在的数据库文件。 更改的名称 *.mdf*永远不会被用作数据库名称和更改的名称的文件*Web.config*文件以使用新名称。 作为替代方法，你可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)删除先前存在的 SQL Server Express 数据库。

## <a name="model-compatibility-cannot-be-checked"></a>不能模型兼容性检查

### <a name="scenario"></a>方案

您提供最新*Web.config*文件连接字符串以指向新的 SQL Server Express 数据库，并首次运行该应用程序，请参阅以下的错误消息：

无法检查模型的兼容性，因为数据库不包含模型元数据。 请确保 IncludeMetadataConvention 验证已添加到 DbModelBuilder 约定。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

如果数据库名称将放在 Web.config 文件中，使用过之前在计算机上，数据库可能已存在的某些表在其使用。 选择尚未使用之前的计算机，然后更改的新名称*Web.config*文件以使其指向使用此新的数据库名称。 作为替代方法，你可以使用[SQL Server Express 实用工具](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)若要删除现有数据库。

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>当脚本尝试创建用户或角色时 SQL 错误

### <a name="scenario"></a>方案

将数据库部署上配置**打包/发布 SQL**选项卡上，在部署期间运行的 SQL 脚本包括创建用户或创建角色命令和脚本执行失败时执行这些命令。 您可能会看到更多详细消息，如下所示：

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

如果已配置中的数据库部署时会发生此错误**发布 Web**向导而不是**打包/发布 SQL**选项卡上，创建中的线程[配置和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)论坛和解决方案将添加到此故障排除页。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

用来执行部署的用户帐户没有创建用户或角色的权限。 例如，托管公司可能会分配 db\_datareader，db\_datawriter，和 db\_ddladmin 角色到为您设置的用户帐户。 这些是足够用于创建大多数数据库对象，但不是用于创建用户或角色。 若要避免错误的一种方法是通过从数据库部署中排除用户和角色。 可以通过编辑数据库的自动生成的脚本的 PreSource 元素，以便它包含以下属性来执行此操作：

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

有关如何编辑项目文件中的 PreSource 元素的信息，请参阅[如何： 编辑项目文件中的部署设置](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。 如果用户或开发数据库中的角色必须位于目标数据库，联系你托管提供商获取帮助。

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server 超时错误时在部署期间运行自定义脚本

### <a name="scenario"></a>方案

您指定了自定义 SQL 脚本来运行部署过程中，并且当 Web 部署运行它们时，它们的超时时间。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

运行具有不同的事务模式的多个脚本可能会导致超时错误。 默认情况下，自动生成的脚本运行在事务中，但自定义脚本不这样做。 如果选择**抽取数据和/或从现有数据库架构**选项卡上**打包/发布 SQL**选项卡上，如果添加自定义 SQL 脚本，必须更改的事务设置一些脚本，以便所有脚本都使用相同的事务设置。 有关详细信息，请参阅[如何： 部署数据库与 Web 应用程序项目](https://msdn.microsoft.com/library/dd465343.aspx)。

如果你已配置事务设置，以便所有都相同，但仍然收到此错误，可能的解决方法是单独运行这些脚本。 中**数据库脚本**中的网格**打包/发布**SQL 选项卡，清除**Include**会导致超时错误，该脚本的复选框，然后发布项目。 然后转回**数据库脚本**网格中，选择该脚本**Include**复选框，然后清除**Include**其他脚本对应的复选框。 然后再次发布该项目。 这一次发布时，仅在选择的自定义脚本运行。

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>站点清单的 Stream 数据尚不可用

### <a name="scenario"></a>方案

当要安装包使用*deploy.cmd*文件使用 t （测试） 选项，请参阅以下的错误消息：

错误： 流数据的 sitemanifest/dbFullSql [@path= C:\TEMP\AdventureWorksGrant.sql']/sqlScript 尚不可用。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

错误消息表示该命令不能生成测试报告。 但是，如果您使用 y （实际安装） 选项，可能会运行该命令。 该消息仅指示在测试模式下运行该命令的问题。

## <a name="this-application-requires-managedruntimeversion-v40"></a>此应用程序需要 ManagedRuntimeVersion v4.0

### <a name="scenario"></a>方案

当你尝试部署时，您将看到以下错误消息：

想要使用的应用程序池已将 managedRuntimeVersion 属性设置为 v2.0。 此应用程序需要 v4.0。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

ASP.NET 4 未安装在 IIS 中。 如果要部署到的服务器是在开发计算机上安装 Visual Studio 2010，ASP.NET 4 的计算机上安装，但可能未安装在 IIS 中。 要部署到服务器上, 打开提升的命令提示符并安装在 IIS 中的 ASP.NET 4 中，通过运行以下命令：

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>无法转换 Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>方案

在部署包时，您将看到以下错误消息：

无法将类型 Microsoft.Web.Deployment.DeploymentProviderOptions' 到 'Microsoft.Web.Deployment.DeploymentProviderOptions 的对象强制转换。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

想要部署从 IIS 管理器中使用 Web 部署 1.1 UI 到已安装的 Web Deploy 2.0 的服务器。 如果使用 IIS 的远程管理工具来部署通过导入包，检查**新功能可用**时建立的连接对话框。 （此对话框中可能才会显示一次第一次建立连接时。 若要清除连接并重新开始，关闭 IIS 管理器和它再次启动通过输入 inetmgr/重置在命令提示符下。）如果列出的功能之一是**Web 部署 UI**，和它有一个版本号低于 8，您要部署到服务器可能具有 1.1 和 2.0 版本的 Web 部署安装。 若要从具有 2.0 安装的客户端部署，服务器必须具有仅 Web Deploy 2.0 安装。 你将需要联系托管提供商，若要解决此问题。

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>无法加载 SQL Server compact 的本机组件

### <a name="scenario"></a>方案

当运行已部署的站点时，您将看到以下错误消息：

无法加载 SQL Server compact 与版本 8482 的 ADO.NET 提供程序相对应的本机组件。 安装 SQL Server Compact 的正确版本。 请参阅 KB 文章 974247 的更多详细信息。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

已部署的站点并没有*amd64*并*x86*具有本机中这些程序集的应用程序下的子文件夹*bin*文件夹。 在上具有 SQL Server Compact 安装的计算机，本机程序集位于*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。 到正确的文件夹中的 Visual Studio 项目中获取正确的文件的最佳方法是安装 NuGet SqlServerCompact 包。 包安装添加后期生成脚本将复制到本机程序集*amd64*并*x86*。 为了使这些部署，但是，必须手动将其包含在项目中。 有关详细信息，请参阅[部署 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教程。

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First 应用程序部署之后，"路径不是有效的"错误

### <a name="scenario"></a>方案

部署应用程序，如 SQL Server Compact 的应用程序中的文件中存储其数据库中使用 Entity Framework Code First 迁移和 DBMS\_数据文件夹。 您必须配置为在第一次部署后创建的数据库的 Code First 迁移。 运行应用程序将获得一条错误消息，如下例所示：

路径不是有效的。 检查数据库的目录。 [路径 = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

代码首先尝试创建数据库，但应用\_数据文件夹不存在。 或者您没有任何文件*应用程序\_数据*时，部署，或所选的文件夹**排除应用\_数据**上**打包/发布 Web**选项卡的项目属性窗口。 如果在要复制到服务器的文件夹中没有任何文件，部署过程不会在服务器上创建一个文件夹。 如果已有在站点中设置的数据库，在部署过程将删除的文件和*应用程序\_数据*本身如果您选择的文件夹**删除目标处的其他文件**中发布配置文件。 为了解决此问题，将占位符文件，如.txt 文件中*应用程序\_数据*文件夹，请确保你没有**排除应用\_数据**选择，并重新部署。

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"不能使用已离开其基础的 RCW 的 COM 对象"。

### <a name="scenario"></a>方案

你已成功使用一键式发布来部署应用程序，然后你会开始收到此错误：

Web 部署任务失败。 (无法完成对远程代理 URL 的请求<https://serverurl.com/msdeploy.axd?site=sitename>。)  
 无法完成对远程代理 URL 的请求<https://url/msdeploy.axd?site=sitename>。  
请求已中止： 请求已取消。  
不能使用已离开其基础的 RCW 的 COM 对象。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

关闭并重新启动 Visual Studio 通常是所有需要来解决此错误。

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>部署失败由于用户的凭据用于发布没有 setACL 颁发机构

### <a name="scenario"></a>方案

发布失败并出现错误，指示您没有设置文件夹权限 （所使用的用户帐户没有 setACL 颁发机构） 的授权。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，Visual Studio 设置网站的根文件夹的权限上读写权限应用\_数据文件夹。 如果您知道站点文件夹上的默认权限是否正确，并且不需要设置，则禁用此行为通过添加 **&lt;IncludeSetACLProviderOn 目标&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 到发布配置文件 （若要影响单个配置文件） 或 wpp.targets 文件 （用于会影响所有配置文件）。 有关如何编辑这些文件的信息，请参阅[如何： 编辑配置文件 (.pubxml) 文件中的部署设置](https://msdn.microsoft.com/library/ff398069.aspx)。

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>当应用程序尝试写入应用程序文件夹时，访问被拒绝错误

### <a name="scenario"></a>方案

应用程序错误时它会尝试创建或编辑的文件中的一个应用程序文件夹中，因为它不具有该文件夹的写入权限。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

默认情况下，Visual Studio 设置网站的根文件夹的权限上读写权限应用\_数据文件夹。 如果你的应用程序需要到子文件夹的写访问权限，可以将该文件夹的权限设置中所示设置文件夹权限并部署到此系列中的生产环境教程。 如果你的应用程序需要到该站点的根文件夹的写访问权限，您必须以防止它在根文件夹上设置只读访问权限，通过添加 **&lt;IncludeSetACLProviderOn 目标&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 到发布配置文件 （若要影响单个配置文件） 或 wpp.targets 文件 （用于会影响所有配置文件）。 有关如何编辑这些文件的信息，请参阅[如何： 编辑配置文件 (.pubxml) 文件中的部署设置](https://msdn.microsoft.com/library/ff398069.aspx)。

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>配置错误的 targetFramework 特性引用的版本晚于安装的.NET Framework 版本

### <a name="scenario"></a>方案

已成功发布的 web 项目面向 ASP.NET 4.5 中，但是 （与将 customErrors 模式设置为"关闭"Web.config 文件中） 运行该应用程序时收到以下错误：

TargetFramework 属性中&lt;编译&gt;仅针对目标版本 4.0 和更高版本的.NET Framework，则使用 Web.config 文件的元素 (例如，&lt;编译 targetFramework ="4.0"&gt;'). TargetFramework 属性当前引用晚于安装的.NET Framework 版本的版本。 指定有效的目标版本的.NET Framework 中，或安装.NET Framework 所需的版本。

错误页的源错误框中为错误的原因，突出显示了 Web.config 中的以下行：

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

服务器不支持 ASP.NET 4.5。 请联系托管提供商来确定何时及如果可以添加对 ASP.NET 4.5 支持。 如果升级服务器不是一个选项，则必须部署一个面向 ASP.NET 4 或更早版本的 web 项目改为。

如果将 ASP.NET 4 或更早的 web 项目部署到同一目标中，选择**删除目标处的其他文件**上的复选框**设置**选项卡**发布 Web**向导。 如果没有选择**删除目标处的其他文件**，仍将显示配置错误页。

项目**属性**windows 包含目标框架下拉列表，但您不能解决此问题，只需更改，从 **.NET Framework 4.5**到 **.NET Framework 4**. 如果目标框架更改为较早的 framework 版本，此项目仍将对更高版本的 framework 版本的程序集的引用，并将不会运行。 您必须手动更改这些引用或创建新的项目面向.NET Framework 4 或更早版本。 有关详细信息，请参阅[网站的.NET Framework 目标](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。

## <a name="medium-trust-errors"></a>中等信任错误

### <a name="scenario"></a>方案

在生产环境中运行你的应用程序时，它将获取与中等信任相关的错误。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

许多第三方托管提供商在中等信任，这意味着它不允许执行的操作的一些事项中运行你的网站。 例如，应用程序代码不能访问 Windows 注册表和无法读取或写入应用程序的文件夹层次结构之外的文件。 默认情况下你的应用程序运行*完全信任*上本地计算机，这意味着应用程序可能能够执行操作时将其部署到生产环境会失败。

可以配置要在本地 IIS 环境中的中等信任环境中运行以进行故障排除的应用程序。 为此，请打开应用程序*Web.config*文件，并添加**信任**中的元素**system.web**元素，在此示例中所示。

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

甚至在本地计算机上，现在在 IIS 中的中等信任应用程序将运行。

不这样做部署到 Azure 应用服务，因为 Azure 不需要中等信任。 在 2012 年 2 月撰写本教程时使用此方法让你的应用程序运行在中等信任中将导致错误在 Azure 中。

如果您使用 Entity Framework Code First 迁移，并且要部署到的宿主提供程序在中等信任中运行你的应用程序，请确保具有版本 5.0 或更高版本安装。 在实体框架版本 4.3，迁移以更新数据库架构需要完全信任。

## <a name="http-40417-not-found-error"></a>找不到 HTTP 404.17 错误

### <a name="scenario"></a>方案

在 IIS 中在开发计算机上运行已部署的站点时，您将看到以下错误消息报告服务器无法处理 Default.aspx:

HTTP 错误 404.17-找不到

请求的内容显示为脚本，并且不会提供静态文件处理程序。

### <a name="possible-cause-and-solution"></a>可能的原因和解决方案

可能未在计算机上安装 ASP.NET 4.5。 本系列中说明了如何安装 ASP.NET 4.5 在测试环境教程，请参阅中部署到 IIS 的步骤。

> [!div class="step-by-step"]
> [上一篇](deploying-extra-files.md)
