---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: 添加新字段 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 950ae17ebd6b0f15520c2a4e9372703f5374dfbe
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098683"
---
<a name="adding-a-new-field"></a>添加新字段
====================
通过[Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本部分中将使用 Entity Framework Code First 迁移来迁移到模型类的一些更改，因此更改被应用到数据库。

默认情况下，当您使用 Entity Framework Code First 自动创建数据库，就像前面在本教程中，代码优先将表添加到数据库来帮助跟踪数据库的架构是否与从其中生成它的模型类同步。 如果它们不同步，实体框架将引发错误。 这使得更轻松地在开发时，你可能会否则仅发现 （通过奇怪的错误） 在运行时跟踪问题。

## <a name="setting-up-code-first-migrations-for-model-changes"></a>为模型更改设置 Code First 迁移

导航到解决方案资源管理器。 右键单击*Movies.mdf*文件，然后选择**删除**删除电影数据库。 如果没有看到*Movies.mdf*文件中，单击**显示所有文件**图标中的红色框如下所示。

![](adding-a-new-field/_static/image1.png)

生成应用程序以确保没有任何错误。

从**工具**菜单上，单击**NuGet 包管理器**，然后**程序包管理器控制台**。

![添加包 Man](adding-a-new-field/_static/image2.png)

在中**程序包管理器控制台**处窗口`PM>`提示输入

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

**Enable-migrations** （如上所示） 的命令将创建*Configuration.cs*文件中的新*迁移*文件夹。

![](adding-a-new-field/_static/image4.png)

Visual Studio 将打开*Configuration.cs*文件。 替换`Seed`中的方法*Configuration.cs*文件使用以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

将鼠标悬停在红色的波浪线`Movie`然后单击`Show Potential Fixes`，然后单击**使用** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

执行此操作将添加以下 using 语句：

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> 代码优先迁移调用`Seed`每个迁移后的方法 (即，调用**更新数据库**程序包管理器控制台)，，此方法更新行，具有已插入，或将其插入，如果它们尚不存在。
> 
> [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)下面的代码中的方法执行的"upsert"操作：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> 因为[种子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法运行与每个迁移，您不能只是插入数据，因为你尝试添加的行已存在后将创建数据库的初始迁移。 "[Upsert](http://en.wikipedia.org/wiki/Upsert)"操作可以防止当尝试插入的行已存在，将会发生的错误，但它将替代对测试应用程序时可能已做的数据的任何更改。 使用测试数据的某些表中您可能不希望发生这种情况： 在某些情况下测试时更改数据时所需数据库更新后保留所做的更改。 在这种情况下想要执行条件插入操作： 插入行，仅当它尚不存在。   
> 
> 第一个参数传递给[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法指定要用于检查行是否已存在的属性。 提供的，测试电影数据`Title`属性可用于此目的，因为每个标题在列表中的是唯一的：
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> 此代码假定标题是唯一的。 如果你手动添加重复的标题，您将得到以下异常执行迁移的下一个时间。   
> 
>  *序列包含多个元素*  
> 
> 有关详细信息[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，请参阅[负责使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...


**按 CTRL-SHIFT-B 以生成项目。**（以下步骤将失败，如果此时不生成。）

下一步是创建`DbMigration`类适用于初始迁移。 此迁移过程将创建一个新的数据库，这就是为什么你删除*movie.mdf*上一步中的文件。

在中**程序包管理器控制台**窗口中，输入命令`add-migration Initial`创建初始迁移。 "初始"的名称是任意参数并用于命名创建的迁移文件。

![](adding-a-new-field/_static/image6.png)

Code First 迁移创建另一个类文件中的*迁移*文件夹 (具有名称 *{日期戳}\_Initial.cs* )，并且此类包含创建数据库架构的代码。 预先使用时间戳修复迁移文件名以帮助进行排序。 检查 *{日期戳}\_Initial.cs*文件，它包含的说明创建`Movies`电影数据库的表。 更新以下，这说明中的数据库时 *{日期戳}\_Initial.cs*文件将运行并创建数据库架构。 然后**种子**方法将运行以填充具有测试数据的数据库。

在中**程序包管理器控制台**，输入命令`update-database`若要创建数据库并运行`Seed`方法。

![](adding-a-new-field/_static/image7.png)

如果收到一个错误，指示表已存在，无法创建，则可能是因为删除了该数据库后，您在执行前运行应用程序`update-database`。 在这种情况下，删除*Movies.mdf*再次文件，然后重试`update-database`命令。 如果仍遇到错误，删除的 migrations 文件夹和内容，然后开始在此页顶部的说明进行操作 (这是 delete *Movies.mdf*文件，然后转到 Enable-migrations)。 如果仍遇到错误，打开 SQL Server 对象资源管理器，并从列表中删除数据库。

运行应用程序并导航到 */Movies* URL。 将显示种子数据。

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>向电影模型添加分级属性

首先，通过添加一个新`Rating`属性设置为现有`Movie`类。 打开*Models\Movie.cs*文件，并添加`Rating`如下属性：

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

完整`Movie`类现在看起来如以下代码：

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

生成应用程序 (Ctrl + Shift + B)。

因为你已将新字段添加到`Movie`类，你还需要更新绑定允许列表以便将包含此新属性。 更新`bind`特性`Create`并`Edit`操作方法，以包含`Rating`属性：

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

还需要更新视图模板，以便在浏览器视图中显示、创建和编辑新的 `Rating` 属性。

打开 *\Views\Movies\Index.cshtml* 文件，并添加`<th>Rating</th>`列标题之后**价格**列。 然后添加`<td>`快要结束的模板来呈现列`@item.Rating`值。 下面是哪些更新 *Index.cshtml* 视图模板如下所示：

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

接下来，打开 *\Views\Movies\Create.cshtml* 文件，并添加`Rating`字段使用以下 highlighed 标记。 这会使文本框中，以便创建新电影时，可以指定一个级别。

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

现已更新以支持新的应用程序代码`Rating`属性。

运行应用程序并导航到 */Movies* URL。 执行此操作，不过，将看到以下错误之一：

![](adding-a-new-field/_static/image9.png)  
  
创建数据库后，支持 MovieDBContext 上下文的模型已更改。 请考虑使用 Code First 迁移来更新数据库 (https://go.microsoft.com/fwlink/?LinkId=238269)。

![](adding-a-new-field/_static/image10.png)

之所以看到此错误，因为已更新`Movie`应用程序中的 model 类现在与不同的架构`Movie`现有数据库表。 （数据库表中没有 `Rating` 列。）


可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 在测试数据库上进行开发时，此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。 缺点，不过，是会丢失数据库中的现有数据，因此您 *不* 需要生产数据库上使用此方法 ！ 使用初始值设定项，以使用测试数据自动设定数据库种子，这通常是开发应用程序的有效方式。 有关实体框架数据库初始值设定项的详细信息，请参阅[ASP.NET MVC/实体框架教程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。
3. 使用 Code First 迁移更新数据库架构。


本教程使用 Code First 迁移。

更新 Seed 方法，使它向新列提供值。 打开 Migrations\Configuration.cs 文件并将评级字段添加到每个电影对象。

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

生成解决方案，然后打开**程序包管理器控制台**窗口并输入以下命令：

`add-migration Rating`

`add-migration`命令会通知迁移框架以检查当前电影模型与当前的电影数据库架构并创建必要的代码以将数据库迁移到新模型。 名称*评级*是任意参数并用于命名迁移文件。 最好使用迁移步骤有意义的名称。

此命令完成后，Visual Studio 会打开定义新的类文件`DbMigration`派生的类，然后在`Up`方法您可以看到创建的新列的代码。

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

生成解决方案，并输入`update-database`命令，在**程序包管理器控制台**窗口。

下图显示了中的输出**程序包管理器控制台**窗口 (日期戳追加*评级*会有所不同。)

![](adding-a-new-field/_static/image11.png)

重新运行该应用程序并导航到 /Movies URL。 您可以看到新的评级字段。

![](adding-a-new-field/_static/image12.png)

单击**创建新**链接以添加新电影。 请注意，您可以添加一个级别。

![7_CreateRioII](adding-a-new-field/_static/image13.png)

单击 **“创建”**。 新电影，包括分级，现在显示在列表的电影：

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

现在，项目使用迁移，不需要将新字段添加或更新架构，否则时删除数据库。 在下一步部分中，我们将进行更多架构更改，并使用迁移来更新数据库。

您还应添加`Rating`字段编辑、 详细信息和删除视图模板。

可以输入中的"更新数据库"命令**程序包管理器控制台**再次窗口和没有迁移代码会运行，因为该架构与模型相匹配。 但是，运行"更新数据库"将运行`Seed`方法同样，如果您更改了任何种子数据，所做的更改都将丢失，因为`Seed`方法更新插入数据。 可以阅读更多有关`Seed`方法在 Tom Dykstra 的常用[ASP.NET MVC/实体框架教程](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

在本部分中您将看到如何修改模型对象并使数据库所做的更改同步。 你还了解了一种方法来填充新创建的数据库使用示例数据，这样您可以试用方案。 这是只需向 Code First 简介，请参阅[为 ASP.NET MVC 应用程序创建实体框架数据模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)有关主题的更完整教程。 接下来，让我们看看如何将更丰富的验证逻辑添加到模型类并启用某些业务规则以强制实施。

> [!div class="step-by-step"]
> [上一页](adding-search.md)
> [下一页](adding-validation.md)
