---
title: 将从 2.1 到 2.2 或 3.0 Microsoft.Extensions.Logging 迁移
author: pakrym
description: 了解如何迁移使用 Microsoft.Extensions.Logging 从 2.1 到 2.2 或 3.0 的 ASP.NET Core 应用程序。
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099467"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>将从 2.1 到 2.2 或 3.0 Microsoft.Extensions.Logging 迁移

本文概述了用于迁移使用的非 ASP.NET Core 应用程序的常见步骤`Microsoft.Extensions.Logging`从 2.1 到 2.2 或 3.0。

## <a name="21-to-22"></a>2.1 到 2.2

手动创建`ServiceCollection`，并调用`AddLogging`。

2.1 示例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

2.2 示例：

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>2.1 到 3.0

在 3.0 中，使用`LoggingFactory.Create`。

2.1 示例：

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

3.0 的示例：

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>其他资源

<xref:fundamentals/logging/index>