---
ms.openlocfilehash: a3ac1ac389892681e8c122d569e6325353ed6e88
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341675"
---
# [ASP.NET Core 文档](/aspnet/#pivot=core)

# 概述
## [关于 ASP.NET Core](xref:index)
## [比较 ASP.NET Core 和 ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore)
## [比较 .NET Core 和 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server)

# [入门](xref:getting-started)

# 新增功能
## [2.2 中的新变化](xref:aspnetcore-2.2)
## [2.1 版中的新增功能](xref:aspnetcore-2.1)
## [2.0 版中的新增功能](xref:aspnetcore-2.0)
## [1.1 中的新增功能](xref:aspnetcore-1.1)

# 教程
## Web API 应用
### [创建 Web API](xref:tutorials/first-web-api)
### [使用 MongoDB 创建 Web API](xref:tutorials/first-mongo-app)
## Web 应用
### [Razor 页面](xref:tutorials/razor-pages/index)
### [MVC](xref:tutorials/first-mvc-app/index)

## 实时 web 应用
### [JavaScript 和 SignalR](xref:tutorials/signalr)
### [TypeScript 和 SignalR](xref:tutorials/signalr-typescript-webpack)
## [为本机移动应用创建后端服务](xref:mobile/native-mobile-backend)

## 数据访问
### [EF Core 和 Razor Pages](xref:data/ef-rp/index)
### [EF Core 和 MVC - 现有 DB](/ef/core/get-started/aspnetcore/existing-db)
### [EF Core 与 MVC - 新 DB](/ef/core/get-started/aspnetcore/new-db)
### [EF Core 和 MVC - 长篇教程](xref:data/ef-mvc/index)

# 基础知识
## [概述](xref:fundamentals/index)
## [应用启动](xref:fundamentals/startup)
## [依赖关系注入（服务）](xref:fundamentals/dependency-injection)
## [路由](xref:fundamentals/routing)
## [环境（开发、分阶段、生产）](xref:fundamentals/environments)
## [配置](xref:fundamentals/configuration/index)
## [选项](xref:fundamentals/configuration/options)
## [日志记录](xref:fundamentals/logging/index)
## [处理错误](xref:fundamentals/error-handling)
## 中间件
### [概述](xref:fundamentals/middleware/index)
### [基于工厂的中间件](xref:fundamentals/middleware/extensibility)
### [Factory-based middleware with third-party container（第三方容器中基于工厂的中间件）](xref:fundamentals/middleware/extensibility-third-party-container)
## Host
### [概述](xref:fundamentals/host/index)
### [Web 主机](xref:fundamentals/host/web-host)
### [通用主机](xref:fundamentals/host/generic-host)
## [服务器](xref:fundamentals/servers/index)
## [启动 HTTP 请求](xref:fundamentals/http-requests)

# Web 应用
## Razor 页面
### [概述](xref:razor-pages/index)
### [Razor Pages 教程](xref:tutorials/razor-pages/index)
#### [入门](xref:tutorials/razor-pages/razor-pages-start)
#### [添加模型](xref:tutorials/razor-pages/model)
#### [基架](xref:tutorials/razor-pages/page)
#### [使用 DB](xref:tutorials/razor-pages/sql)
#### [更新页面](xref:tutorials/razor-pages/da1)
#### [添加搜索](xref:tutorials/razor-pages/search)
#### [添加新字段](xref:tutorials/razor-pages/new-field)
#### [添加验证](xref:tutorials/razor-pages/validation)

## MVC
### [MVC 概述](xref:mvc/overview)
### [MVC 教程](xref:tutorials/first-mvc-app/index)
#### [入门](xref:tutorials/first-mvc-app/start-mvc)
#### [添加控制器](xref:tutorials/first-mvc-app/adding-controller)
#### [添加视图](xref:tutorials/first-mvc-app/adding-view)
#### [添加模型](xref:tutorials/first-mvc-app/adding-model)
#### [使用 DB](xref:tutorials/first-mvc-app/working-with-sql)
#### [控制器操作和视图](xref:tutorials/first-mvc-app/controller-methods-views)
#### [添加搜索](xref:tutorials/first-mvc-app/search)
#### [添加新字段](xref:tutorials/first-mvc-app/new-field)
#### [添加验证](xref:tutorials/first-mvc-app/validation)
#### [检查 Details 和 Delete 方法](xref:tutorials/first-mvc-app/details)

### [筛选器](xref:razor-pages/filter)
### [Razor 类库](xref:razor-pages/ui-class)
### [路由和应用约定](xref:razor-pages/razor-pages-conventions)
### [上载文件](xref:razor-pages/upload-files)
### [Razor SDK](xref:razor-pages/sdk)

### [视图](xref:mvc/views/overview)
### [部分视图](xref:mvc/views/partial)
### [控制器](xref:mvc/controllers/actions)
### [路由](xref:mvc/controllers/routing)
### [文件上传](xref:mvc/models/file-uploads)
### [依赖关系注入 - 控制器](xref:mvc/controllers/dependency-injection)
### [依赖关系注入 - 视图](xref:mvc/views/dependency-injection)
### [单元测试](xref:mvc/controllers/testing)
## [会话和应用状态](xref:fundamentals/app-state)
## 标记帮助程序
### [概述](xref:mvc/views/tag-helpers/intro)
### [创建标记帮助程序](xref:mvc/views/tag-helpers/authoring)
### [在窗体中使用标记帮助程序](xref:mvc/views/working-with-forms)
### [标记帮助程序组件](xref:mvc/views/tag-helpers/th-components)
### 内置标记帮助程序
#### [定位点](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)
#### [缓存](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
#### [分布式缓存](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
#### [环境](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)
#### [窗体](mvc/views/working-with-forms.md#the-form-tag-helper)
#### [Image](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)
#### [输入](mvc/views/working-with-forms.md#the-input-tag-helper)
#### [标签](mvc/views/working-with-forms.md#the-label-tag-helper)
#### [Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)
#### [选择](mvc/views/working-with-forms.md#the-select-tag-helper)
#### [Textarea](mvc/views/working-with-forms.md#the-textarea-tag-helper)
#### [验证消息](mvc/views/working-with-forms.md#the-validation-message-tag-helper)
#### [验证摘要](mvc/views/working-with-forms.md#the-validation-summary-tag-helper)
## [布局](xref:mvc/views/layout)
## [静态文件](xref:fundamentals/static-files)
## [模型绑定](xref:mvc/models/model-binding)
## [模型验证](xref:mvc/models/validation)
## [Razor 语法](xref:mvc/views/razor)
## 高级
### [视图组件](xref:mvc/views/view-components)
### [视图编译](xref:mvc/views/view-compilation)
### [应用模型](xref:mvc/controllers/application-model)
### [筛选器](xref:mvc/controllers/filters)
### [区域](xref:mvc/controllers/areas)
### [应用部件](xref:mvc/extensibility/app-parts)
### [自定义模型绑定](xref:mvc/advanced/custom-model-binding)
### [兼容性版本](xref:mvc/compatibility-version)

# Web API 应用
## [概述](xref:web-api/index)

## 教程
### [创建 Web API](xref:tutorials/first-web-api)
### [使用 MongoDB 创建 Web API](xref:tutorials/first-mongo-app)

## Swagger/OpenAPI
### [概述](xref:tutorials/web-api-help-pages-using-swagger)
### [Swashbuckle 入门](xref:tutorials/get-started-with-swashbuckle)
### [NSwag 入门](xref:tutorials/get-started-with-nswag)
## [操作返回类型](xref:web-api/action-return-types)
## [格式化响应数据](xref:web-api/advanced/formatting)
## [自定义格式化程序](xref:web-api/advanced/custom-formatters)

## [分析器](xref:web-api/advanced/analyzers)
## [约定](xref:web-api/advanced/conventions)

# 实时应用
## [SignalR 概述](xref:signalr/introduction)
## [支持的平台](xref:signalr/supported-platforms)
## 教程
### [JavaScript 和 SignalR](xref:tutorials/signalr)
### [TypeScript 和 SignalR](xref:tutorials/signalr-typescript-webpack)
## [示例](https://github.com/aspnet/SignalR-samples)
## 服务器概念
### [中心](xref:signalr/hubs)
### [HubContext](xref:signalr/hubcontext)
### [用户和组](xref:signalr/groups)
### [发布到 Azure](xref:signalr/publish-to-azure-web-app)
### [API 设计注意事项](xref:signalr/api-design)
## 客户端
### [.NET 客户端](xref:signalr/dotnet-client)
### [.NET API 参考](/dotnet/api/microsoft.aspnetcore.signalr.client)
### [Java 客户端](xref:signalr/java-client)
### [Java API 参考](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
### [JavaScript 客户端](xref:signalr/javascript-client)
### [JavaScript API 参考](/javascript/api/?view=signalr-js-latest)
## 托管和缩放
### [概述](xref:signalr/scale)
### [Azure SignalR 服务](/azure/azure-signalr/signalr-overview)
### [Redis 底板](xref:signalr/redis-backplane)
## [配置](xref:signalr/configuration)
## [身份验证和授权](xref:signalr/authn-and-authz)
## [安全注意事项](xref:signalr/security)
## [MessagePack 中心协议](xref:signalr/messagepackhubprotocol)
## [流式处理](xref:signalr/streaming)
## [比较 SignalR 和 SignalR Core](xref:signalr/version-differences)
## [不使用 SignalR 的 WebSockets](xref:fundamentals/websockets)

# 测试、调试和疑难解答
## [单元测试](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
## [Razor 页面单元测试](xref:test/razor-pages-tests)
## [测试控制器](xref:mvc/controllers/testing)
## [远程调试](/visualstudio/debugger/remote-debugging-azure)
## [快照调试](/azure/application-insights/app-insights-snapshot-debugger)
## [Visual Studio 中的快照调试](/visualstudio/debugger/debug-live-azure-applications)
## [集成测试](xref:test/integration-tests)
## [负载测试和压力测试](xref:test/loadtests)
## [疑难解答](xref:test/troubleshoot)
## [日志记录](xref:fundamentals/logging/index)

# 数据访问
## 教程
### EF Core 和 Razor Pages
#### [概述](xref:data/ef-rp/index)
#### [入门](xref:data/ef-rp/intro)
#### [创建、读取、更新和删除](xref:data/ef-rp/crud)
#### [排序、筛选器、页和组](xref:data/ef-rp/sort-filter-page)
#### [迁移](xref:data/ef-rp/migrations)
#### [创建复杂数据模型](xref:data/ef-rp/complex-data-model)
#### [读取相关数据](xref:data/ef-rp/read-related-data)
#### [更新相关数据](xref:data/ef-rp/update-related-data)
#### [处理并发冲突](xref:data/ef-rp/concurrency)
### [EF Core 与 MVC - 新 DB](/ef/core/get-started/aspnetcore/new-db)
### [EF Core 和 MVC - 现有 DB](/ef/core/get-started/aspnetcore/existing-db)
### EF Core 和 MVC - 长篇教程
#### [概述](xref:data/ef-mvc/index)
#### [入门](xref:data/ef-mvc/intro)
#### [创建、读取、更新和删除](xref:data/ef-mvc/crud)
#### [排序、筛选器、页和组](xref:data/ef-mvc/sort-filter-page)
#### [迁移](xref:data/ef-mvc/migrations)
#### [创建复杂数据模型](xref:data/ef-mvc/complex-data-model)
#### [读取相关数据](xref:data/ef-mvc/read-related-data)
#### [更新相关数据](xref:data/ef-mvc/update-related-data)
#### [处理并发冲突](xref:data/ef-mvc/concurrency)
#### [继承](xref:data/ef-mvc/inheritance)
#### [高级主题](xref:data/ef-mvc/advanced)
## [EF 6 和 ASP.NET Core](xref:data/entity-framework-6)
## 使用 Visual Studio 的 Azure 存储
### [连接的服务](/azure/vs-azure-tools-connected-services-storage)
### [Blob 存储](/azure/vs-storage-aspnet5-getting-started-blobs/)
### [队列存储](/azure/vs-storage-aspnet5-getting-started-queues/)
### [表存储](/azure/vs-storage-aspnet5-getting-started-tables/)

# 客户端开发
## [概述](xref:client-side/index)
## [Gulp](xref:client-side/using-gulp)
## [Grunt](xref:client-side/using-grunt)
## LibMan
### [概述](xref:client-side/libman/index)
### [CLI](xref:client-side/libman/libman-cli)
### [Visual Studio](xref:client-side/libman/libman-vs)
## [Bower](xref:client-side/bower)
## [LESS、Sass 和 Font Awesome](xref:client-side/less-sass-fa)
## [捆绑和缩小](xref:client-side/bundling-and-minification)
## [浏览器链接](xref:client-side/using-browserlink)
## 单页应用
### [概述](xref:spa/index)
### [Angular](xref:spa/angular)
### [React](xref:spa/react)
### [React 和 Redux](xref:spa/react-with-redux)
### [JavaScriptServices](xref:client-side/spa-services)

# 托管和部署
## [概述](xref:host-and-deploy/index)
## 在 Azure 应用服务上托管
### [概述](xref:host-and-deploy/azure-apps/index)
### [使用 Visual Studio 进行发布](xref:tutorials/publish-to-azure-webapp-using-vs)
### [使用 CLI 工具进行发布](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
### [使用 Visual Studio 和 Git 进行发布](xref:host-and-deploy/azure-apps/azure-continuous-deployment)
### [使用 Azure Pipelines 连续部署](/azure/devops/pipelines/get-started-yaml)
### [ASP.NET Core 模块](xref:host-and-deploy/aspnet-core-module)
### [疑难解答](xref:host-and-deploy/azure-apps/troubleshoot)
### [错误引用](xref:host-and-deploy/azure-iis-errors-reference)
## DevOps 
### [概述](xref:azure/devops/index)
### [工具和下载](xref:azure/devops/tools-and-downloads)
### [部署到应用服务](xref:azure/devops/deploy-to-app-service)
### [持续集成和部署](xref:azure/devops/cicd)
### [监视和故障排除](xref:azure/devops/monitor)
### [后续步骤](xref:azure/devops/next-steps)
## 使用 IIS 在 Windows 上进行托管
### [概述](xref:host-and-deploy/iis/index)
### [ASP.NET Core 模块](xref:host-and-deploy/aspnet-core-module)
### [Visual Studio 中的 IIS 支持](xref:host-and-deploy/iis/development-time-iis-support)
### [IIS 模块](xref:host-and-deploy/iis/modules)
### [疑难解答](xref:host-and-deploy/iis/troubleshoot)
### [错误引用](xref:host-and-deploy/azure-iis-errors-reference)
## [Kestrel](xref:fundamentals/servers/kestrel)
## [HTTP.sys](xref:fundamentals/servers/httpsys)
## [在 Windows 服务中进行托管](xref:host-and-deploy/windows-service)
## [在 Linux 上使用 Nginx 进行托管](xref:host-and-deploy/linux-nginx)
## [在 Linux 上使用 Apache 进行托管](xref:host-and-deploy/linux-apache)
## 在 Docker 中托管
### [概述](xref:host-and-deploy/docker/index)
### [生成 Docker 映像](/dotnet/articles/core/docker/building-net-docker-images)
### [Visual Studio Tools](xref:host-and-deploy/docker/visual-studio-tools-for-docker)
### [发布到 Docker 映像](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)
## [代理和负载均衡器配置](xref:host-and-deploy/proxy-load-balancer)
## [在 Web 场中托管](xref:host-and-deploy/web-farm)
## [Visual Studio 发布配置文件](xref:host-and-deploy/visual-studio-publish-profiles)
## [目录结构](xref:host-and-deploy/directory-structure)
## [运行状况检查](xref:host-and-deploy/health-checks)

# 安全和标识
## [概述](xref:security/index)
## 身份验证
### [标识简介](xref:security/authentication/identity)
### [基架标识](xref:security/authentication/scaffold-identity)
### [将自定义用户数据添加到标识](xref:security/authentication/add-user-data)
### [自定义标识](xref:security/authentication/customize_identity_model)
### [社区 OSS 身份验证选项](xref:security/authentication/community)
### [配置标识](xref:security/authentication/identity-configuration)
### [配置 Windows 身份验证](xref:security/authentication/windowsauth)
### [自动以标识的存储提供程序](xref:security/authentication/identity-custom-storage-providers)
### 外部提供程序
#### [概述](xref:security/authentication/social/index)
#### [Facebook 身份验证](xref:security/authentication/facebook-logins)
#### [Twitter 身份验证](xref:security/authentication/twitter-logins)
#### [Google 身份验证](xref:security/authentication/google-logins)
#### [Microsoft 身份验证](xref:security/authentication/microsoft-logins)
#### [外部身份验证提供程序](xref:security/authentication/otherlogins)
#### [其他声明](xref:security/authentication/social/additional-claims)
### [WS 联合身份验证](xref:security/authentication/ws-federation)
### [帐户确认和密码恢复](xref:security/authentication/accconfirm)
### [在标识中启用 QR 代码生成](xref:security/authentication/identity-enable-qrcodes)
### [使用 SMS 设置双因素身份验证](xref:security/authentication/2fa)
### [在没有标识的情况下使用 cookie 身份验证](xref:security/authentication/cookie)
### Azure Active Directory
#### [概述](xref:security/authentication/azure-active-directory/index)
#### [将 Azure AD 集成到 Web 应用中](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
#### [将 Azure AD B2C 集成到 Web 应用中](xref:security/authentication/azure-ad-b2c)
#### [将 Azure AD B2C 集成到 Web API 中](xref:security/authentication/azure-ad-b2c-webapi)
#### [从 WPF 调用 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
#### [使用 Azure AD 在 Web 应用中调用 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
### [使用 IdentityServer4 保护 ASP.NET Core 应用](https://identityserver4.readthedocs.io/)
### [使用 Azure App Service 身份验证保护 ASP.NET Core 应用（简易身份验证）](/azure/app-service/app-service-authentication-overview)
### [各个用户帐户](xref:security/authentication/individual)
## 授权
### [概述](xref:security/authorization/introduction)
### [通过身份验证创建 Web 应用](xref:security/authorization/secure-data)
### [Razor 页面授权约定](xref:security/authorization/razor-pages-authorization)
### [简单授权](xref:security/authorization/simple)
### [基于角色的授权](xref:security/authorization/roles)
### [基于声明的授权](xref:security/authorization/claims)
### [基于策略的授权](xref:security/authorization/policies)
### [授权策略提供程序](xref:security/authorization/iauthorizationpolicyprovider)
### [要求处理程序中的依赖关系注入](xref:security/authorization/dependencyinjection)
### [基于资源的授权](xref:security/authorization/resourcebased)
### [基于视图的授权](xref:security/authorization/views)
### [使用方案限制标识](xref:security/authorization/limitingidentitybyscheme)
## 数据保护
### [概述](xref:security/data-protection/introduction)
### [数据保护 API](xref:security/data-protection/using-data-protection)
### 使用者 API
#### [概述](xref:security/data-protection/consumer-apis/overview)
#### [目标字符串](xref:security/data-protection/consumer-apis/purpose-strings)
#### [目标层次结构和多租户](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
#### [哈希密码](xref:security/data-protection/consumer-apis/password-hashing)
#### [限制受保护负载的生存期](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
#### [取消保护已撤消其密钥的负载](xref:security/data-protection/consumer-apis/dangerous-unprotect)
### 配置
#### [概述](xref:security/data-protection/configuration/index)
#### [配置数据保护](xref:security/data-protection/configuration/overview)
#### [默认设置](xref:security/data-protection/configuration/default-settings)
#### [计算机范围的策略](xref:security/data-protection/configuration/machine-wide-policy)
#### [非 DI 感知方案](xref:security/data-protection/configuration/non-di-scenarios)
### 扩展性 API
#### [概述](xref:security/data-protection/extensibility/index)
#### [核心加密扩展性](xref:security/data-protection/extensibility/core-crypto)
#### [密钥管理扩展性](xref:security/data-protection/extensibility/key-management)
#### [其他 API](xref:security/data-protection/extensibility/misc-apis)
### 实现
#### [概述](xref:security/data-protection/implementation/index)
#### [已验证的加密详细信息](xref:security/data-protection/implementation/authenticated-encryption-details)
#### [子项派生和已验证的加密](xref:security/data-protection/implementation/subkeyderivation)
#### [上下文标头](xref:security/data-protection/implementation/context-headers)
#### [密钥管理](xref:security/data-protection/implementation/key-management)
#### [密钥存储提供程序](xref:security/data-protection/implementation/key-storage-providers)
#### [静态密钥加密](xref:security/data-protection/implementation/key-encryption-at-rest)
#### [密钥永久性和设置](xref:security/data-protection/implementation/key-immutability)
#### [密钥存储格式](xref:security/data-protection/implementation/key-storage-format)
#### [短数据保护提供程序](xref:security/data-protection/implementation/key-storage-ephemeral)
### 兼容性
#### [概述](xref:security/data-protection/compatibility/index)
#### [在 ASP.NET 中替换 <machineKey>](xref:security/data-protection/compatibility/replacing-machinekey)
## [在开发过程中保护机密](xref:security/app-secrets)
## [Enforce HTTPS](xref:security/enforcing-ssl)
## [欧盟一般数据保护条例 (GDPR) 支持](xref:security/gdpr)
## [Azure Key Vault 配置提供程序](xref:security/key-vault-configuration)
## [反请求伪造](xref:security/anti-request-forgery)
## [阻止打开重定向攻击](xref:security/preventing-open-redirects)
## [阻止跨站点脚本编写](xref:security/cross-site-scripting)
## [启用跨域请求 (CORS)](xref:security/cors)
## [在应用之间共享 Cookie](xref:security/cookie-sharing)
## [IP 安全列表](xref:security/ip-safelist)

# 性能
## [概述](xref:performance/performance-best-practices)
##  响应缓存
### [概述](xref:performance/caching/response)
### [内存中缓存](xref:performance/caching/memory)
### [分布式缓存](xref:performance/caching/distributed)
### [响应缓存中间件](xref:performance/caching/middleware)
## [响应压缩](xref:performance/response-compression)
## [诊断工具](xref:performance/diagnostic-tools)
## [负载测试和压力测试](xref:test/loadtests)

# 其他主题
## [全球化和本地化](xref:fundamentals/localization)
## [使用 Orchard Core 的可移植对象本地化](xref:fundamentals/portable-object-localization)
## [URL 重写](xref:fundamentals/url-rewriting)
## [文件提供程序](xref:fundamentals/file-providers)
## [请求功能](xref:fundamentals/request-features)
## [访问 HttpContext](xref:fundamentals/httpcontext)
## [更改令牌](xref:fundamentals/change-tokens)
## [.NET 的开放 Web 接口 (OWIN)](xref:fundamentals/owin)
## [使用托管服务的后台任务](xref:fundamentals/host/hosted-services)
## [承载启动程序集](xref:fundamentals/configuration/platform-specific-configuration)
## [Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)
## [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)
## [日志记录与 LoggerMessage](xref:fundamentals/logging/loggermessage)
## [使用文件观察程序](xref:tutorials/dotnet-watch)

# 迁移
## [2.2 至 3.0](xref:migration/22-to-30)
## [2.1 至 2.2](xref:migration/21-to-22)
## [2.0 至 2.1](xref:migration/20_21)
## 1.x 至 2.0
### [概述](xref:migration/1x-to-2x/index)
### [身份验证和标识](xref:migration/1x-to-2x/identity-2x)
## ASP.NET 到 ASP.NET Core
### [概述](xref:migration/proper-to-2x/index)
### [MVC](xref:migration/mvc)
### [Web API](xref:migration/webapi)
### [配置](xref:migration/configuration)
### [身份验证和标识](xref:migration/identity)
### [ClaimsPrincipal.Current](xref:migration/claimsprincipal-current)
### [共成员身份到标识](xref:migration/proper-to-2x/membership-to-core-identity)
### [HTTP 模块到中间件](xref:migration/http-modules)
## [日志记录（不是 ASP.NET Core）](xref:migration/logging-nonaspnetcore)

# [API 参考](/dotnet/api/?view=aspnetcore-2.1)

# [参与](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md)
