---
uid: mvc/overview/getting-started/introduction/adding-validation
title: 添加验证 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/06/2019
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: 6831259ce19c3747c179d6fc1b7e2095051a603b
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099021"
---
<a name="adding-validation"></a>添加验证
====================
通过[Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

在本部分中，你将添加到的验证逻辑`Movie`模型，并确保每次用户尝试创建或编辑电影使用应用程序时，强制的验证规则。

## <a name="keeping-things-dry"></a>DRY 地保管东西

ASP.NET MVC 的核心设计原则之一是[DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;不要自我重复&quot;)。 ASP.NET MVC 支持你指定的功能或行为只有一次，然后让它应用到整个应用程序中。 这可以减少需要编写的代码量，并使您编写更少容易出错且更易于维护的代码。

由 ASP.NET MVC 和 Entity Framework Code First 提供的验证支持是 DRY 原则在操作中的一个极好示例。 在一个位置 （在模型类中） 以声明方式指定验证规则和规则所有位置强制执行应用程序中。

让我们看看电影应用程序中，您就可以充分利用此验证支持。

## <a name="adding-validation-rules-to-the-movie-model"></a>向电影模型添加验证规则

您将首先添加到某些验证逻辑`Movie`类。

打开 Movie.cs 文件。 请注意[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间不包含`System.Web`。 DataAnnotations 提供一组内置验证特性，可以以声明方式应用于任何类或属性。 (它还包含等格式特性[数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，有助于格式设置，但不提供任何验证。)

现在，更新`Movie`类，以充分利用内置[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)， [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)，和[`Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)验证特性。 替换为`Movie`以下类：

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=5,13-15,18-19,22-23)]

[ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)属性设置为字符串，最大长度和它在数据库上设置此限制，因此会更改数据库架构。 右键单击**电影**表中**服务器资源管理器**然后单击**打开表定义**:

![](adding-validation/_static/image1.png)

在上图中，您可以看到所有字符串字段设置为[NVARCHAR (MAX)](https://technet.microsoft.com/library/ms186939.aspx)。 我们将使用迁移来更新架构。 生成解决方案，然后打开**程序包管理器控制台**窗口并输入以下命令：

[!code-console[Main](adding-validation/samples/sample2.cmd)]

此命令完成后，Visual Studio 会打开定义新的类文件`DbMIgration`派生类指定的名称 (`DataAnnotations`)，然后在`Up`可以看到更新的架构约束的代码的方法：

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

`Genre`字段不再可以为 null （即，您必须输入一个值）。 `Rating`字段具有最大长度为 5 和`Title`60 的最大长度。 3 上的最小长度`Title`上的范围和`Price`未创建架构更改。

检查电影架构：

![](adding-validation/_static/image2.png)

字符串字段显示了新的长度限制和`Genre`不再检查为可以为 null。

验证特性指定要对应用这些特性的模型属性强制执行的行为。 `Required` 和 `MinimumLength` 特性表示属性必须有值；但用户可输入空格来满足此验证。 [正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)特性用于限制字符可以是输入。 在上述代码中，`Genre` 和 `Rating` 仅可使用字母（禁用空格、数字和特殊字符）。 [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)特性将限制到指定范围内的值。 `StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。 值类型 (如`decimal, int, float, DateTime`) 本质上需要而无须`Required`属性。

代码首先确保之前应用程序将更改保存在数据库中实施的 model 类指定的验证规则。 例如，下面的代码将引发[DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx)异常时`SaveChanges`调用方法，因为所需的几个`Movie`是丢失属性值：

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

上面的代码将引发以下异常：

*对一个或多个实体的验证失败。请参阅 EntityValidationErrors 属性的更多详细信息。*

具有自动强制执行由.NET Framework 的验证规则有助于使您的应用程序更可靠。 同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。

## <a name="validation-error-ui-in-aspnet-mvc"></a>验证错误在 ASP.NET MVC 中的 UI

运行应用程序并导航到 */Movies* URL。

单击**创建新**链接以添加新电影。 使用无效值填写表单。 当 jQuery 客户端验证检测到错误时，会显示一条错误消息。

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> 若要支持 jQuery 验证的非英语区域设置，请使用逗号 （"，"） 小数点，必须包含 NuGet 全球化如前面在本教程中所述。


请注意如何在窗体具有自动使用红色边框颜色突出显示的文本框包含无效的数据和已发出每个适当的验证错误消息。 客户端（使用 JavaScript 和 jQuery）和服务器端（若用户禁用 JavaScript）都必定会遇到这些错误。

一个实际优点是不需要更改任何一行中的代码`MoviesController`类中或在*Create.cshtml*来启用此验证 UI 的视图。 在本教程前面创建的控制器和视图会自动选取验证规则，这些规则是通过在 `Movie` 模型类的属性上使用验证特性所指定的。 使用 `Edit` 操作方法测试验证后，即已应用相同的验证。

存在客户端验证错误时，不会将表单数据发送到服务器。 您可以通过将中断点放在 HTTP Post 方法中，通过使用对此进行验证[fiddler 工具](http://fiddler2.com/fiddler2/)，或 IE [F12 开发人员工具](https://msdn.microsoft.com/ie/aa740478)。

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>如何在中进行验证创建查看和创建操作方法

你可能想知道在不对控制器或视图中的代码进行任何更新的情况下，验证 UI 是如何生成的。 下一步列表显示什么`Create`中的方法`MovieController`类看起来像。 它们是如何在本教程前面创建它们相比没有变化。

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

第一个 (HTTP GET) `Create` 操作方法显示初始的“创建”表单。 第二个 (`[HttpPost]`) 版本处理表单发布。 第二个`Create`方法 (`HttpPost`版本) 检查`ModelState.IsValid`若要查看电影是否有任何验证错误。 获取此属性的计算结果应用于对象的任何验证特性。 如果对象具有验证错误，`Create`方法重新显示该窗体。 如果没有错误，此方法则将新电影保存在数据库中。 在我们的电影示例中，**窗体不会发布到服务器时有验证错误在客户端; 上检测到第二个** `Create` **永远不会调用方法**。 如果在浏览器中禁用 JavaScript，客户端验证已禁用，，HTTP POST`Create`方法获取`ModelState.IsValid`检查电影是否有任何验证错误。

可以在 `HttpPost Create` 方法中设置断点，并验证方法从未被调用，客户端验证在检测到存在验证错误时不会提交表单数据。 如果在浏览器中禁用 JavaScript，然后提交错误的表单，将触发断点。 在没有 JavaScript 的情况下仍然可以进行完整的验证。 下图显示了如何禁用 Internet Explorer 中的 JavaScript。

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

以下图片显示如何在 FireFox 浏览器中禁用 JavaScript。

![](adding-validation/_static/image7.png)

以下图片显示如何在 Chrome 浏览器中禁用 JavaScript。

![](adding-validation/_static/image8.png)

下面是*Create.cshtml*基架在本教程前面部分中的视图模板。 以上所示的操作方法使用它来显示初始表单，并在发生错误时重新显示此表单。

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

请注意代码如何使用`Html.EditorFor`帮助器输出`<input>`每个元素`Movie`属性。 此帮助器旁边是对的调用`Html.ValidationMessageFor`帮助器方法。 这两个帮助器方法使用由控制器传递给视图的模型对象 (在这种情况下，`Movie`对象)。 它们会自动查找适当的模型和显示错误消息中指定的验证特性。

此方法真正好的一点是：无论是控制器还是 `Create` 视图模板都不知道强制实施的实际验证规则或显示的特定错误消息。 仅可在 `Movie` 类中指定验证规则和错误字符串。 这些相同的验证规则自动应用于 `Edit` 视图和可能创建用于编辑模型的任何其他视图模板。

如果你想要以后更改验证逻辑，就可以做到在同一个位置通过向模型添加验证特性 (在此示例中，`movie`类)。 无需担心对应用程序的不同部分所强制执行规则的方式不一致 - 所有验证逻辑都将定义在一个位置并用于整个应用程序。 这使代码非常简洁，并且更易于维护和改进。 它意味着，您完全遵守*DRY*原则。

## <a name="using-datatype-attributes"></a>使用 DataType 特性

打开 Movie.cs 文件并检查 `Movie` 类。 [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空间提供除了一组内置验证特性的格式设置属性。 我们已应用[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)发布日期和价格字段的枚举值。 下面的代码演示`ReleaseDate`并`Price`具有适当[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)属性。

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

[数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性仅提供提示帮助视图引擎设置数据的格式 (并提供特性，如`<a>`url 和`<a href="mailto:EmailAddress.com">`电子邮件。 可以使用[正则表达式](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)特性验证数据的格式。 [数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性用于指定比数据库内部类型更具体的数据类型，它们是***不***验证特性。 在此示例中，我们只想跟踪日期，而不是日期和时间。 [DataType 枚举](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)提供了多种数据类型，如*日期、 时间、 电话号码、 货币、 电子邮件地址*和的详细信息。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，`mailto:`可以为创建链接[DataType.EmailAddress](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)，和日期选择器可提供用于[DataType.Date](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)中支持的浏览器[HTML5](http://html5.org/). [数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性发出 HTML 5 [data-](http://ejohn.org/blog/html-5-data-attributes/) (读作*数据 dash*) 特性供 HTML 5 浏览器理解。 [数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，显示该数据字段根据基于服务器的默认格式[CultureInfo](https://msdn.microsoft.com/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx)。

`DisplayFormat` 特性用于显式指定日期格式：


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


`ApplyFormatInEditMode`设置指定，指定的格式设置也应该应用时的值显示在文本框中以进行编辑。 (您可能不想为某些字段 — 例如，对于货币值，您可能不希望在文本框中的货币符号以进行编辑。)

可以使用[DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)特性本身，但它通常是使用一个好办法[数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性。 `DataType`特性传达*语义*的数据作为相对于屏幕上的呈现方式，提供了以下具有前所未有的优势`DisplayFormat`:

- 在浏览器可启用 HTML5 功能 （例如显示日历控件、 区域设置适用的货币符号、 电子邮件链接等。）。
- 默认情况下，在浏览器将呈现数据采用正确的格式基于你[区域设置](https://msdn.microsoft.com/library/vstudio/wyzd2bce.aspx)。
- [数据类型](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx)特性使 MVC 能够选择正确的字段模板来呈现数据 ( [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)如果由自身使用字符串模板)。 有关详细信息，请参阅 Brad Wilson [ASP.NET MVC 2 模板](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html)。 （为 MVC 2 编写的但本文仍适用于 ASP.NET MVC 的当前版本。）

如果您使用`DataType`属性与日期字段，则必须指定`DisplayFormat`还以确保字段正确呈现在 Chrome 浏览器中的属性。 有关详细信息，请参阅[此 StackOverflow 线程](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie)。

> [!NOTE]
> jQuery 验证不适用于[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性和[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 例如，以下代码将始终显示客户端验证错误，即便日期在指定的范围内：
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> 将需要禁用 jQuery 日期验证才能使用[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)特性[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)。 它通常不是编译在模型中，因此，使用固定日期的好办法[范围](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)属性和[DateTime](https://msdn.microsoft.com/library/system.datetime.aspx)不建议这样做。


以下代码显示组合在一行上的特性：

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=4,6,10,12)]

在本系列的下一部分中，我们将回顾应用程序，并对自动生成的 `Details` 和 `Delete` 方法进行一些改进。

> [!div class="step-by-step"]
> [上一页](adding-a-new-field.md)
> [下一页](examining-the-details-and-delete-methods.md)
