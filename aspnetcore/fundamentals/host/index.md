---
title: ASP.NET Core 中的 Web 主机和通用主机
author: guardrex
description: 了解有关 ASP.NET Core Web 主机和 .NET 通用主机的信息，它们负责应用启动和生存期管理。
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 43a68f67368ce37a1fb4032579c6c4e4c05d2719
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284573"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>ASP.NET Core 中的 Web 主机和通用主机

.NET 应用配置和启动主机。 主机负责应用程序启动和生存期管理。 两个主机 API 可供使用：

* [Web 主机](xref:fundamentals/host/web-host) &ndash; 适用于托管 Web 应用。
* [通用主机](xref:fundamentals/host/generic-host)（ASP.NET Core 2.1 或更高版本）&ndash; 适用于托管非 Web 应用（例如，运行后台任务的应用）。 在未来的版本中，通用主机将适用于托管任何类型的应用，包括 Web 应用。 通用主机最终将取代 Web 主机。

为托管 ASP.NET Core Web 应用，开发人员应使用基于 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 的 Web 主机。 为托管非 Web 应用，开发人员应使用基于 <xref:Microsoft.Extensions.Hosting.HostBuilder> 的通用主机。

<xref:fundamentals/host/hosted-services>  
了解如何在 ASP.NET Core 中使用托管服务实现后台任务。

<xref:fundamentals/configuration/platform-specific-configuration>  
了解如何使用 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 实现从引用或未引用程序集增强 ASP.NET Core 应用。
