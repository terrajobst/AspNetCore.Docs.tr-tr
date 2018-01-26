---
uid: webhooks/diagnostics/logging
title: "ASP.NET Web Kancalarını günlüğü | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını içinde günlüğü nasıl."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="ec7d6-103">ASP.NET Web Kancalarını günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="ec7d6-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="ec7d6-104">Microsoft ASP.NET WebHooks günlüğe kaydetme sorunları raporlama bir yolu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="ec7d6-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="ec7d6-105">Varsayılan olarak günlükleri kullanılarak yazılan [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) burada manged olabilirler kullanarak [izleme dinleyicileri](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) herhangi bir günlük akışı ister.</span><span class="sxs-lookup"><span data-stu-id="ec7d6-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="ec7d6-106">Web uygulamanızı Azure Web uygulaması olarak dağıtırken günlükleri otomatik olarak toplanmış ve diğer birlikte yönetilebilir [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) günlüğü.</span><span class="sxs-lookup"><span data-stu-id="ec7d6-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="ec7d6-107">Ayrıntılar için lütfen bkz [Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="ec7d6-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="ec7d6-108">Ayrıca, günlükleri düz öğesinden elde edilebilir açıklandığı gibi Visual Studio içinde [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="ec7d6-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="ec7d6-109">Günlükleri yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="ec7d6-109">Redirecting Logs</span></span>

<span data-ttu-id="ec7d6-110">Günlükleri yazmak yerine [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), doğrudan bir günlük yöneticisi gibi oturum alternatif günlük kaydı uygulaması sağlamak mümkün [Log4Net](http://logging.apache.org/log4net/) ve [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="ec7d6-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="ec7d6-111">Uygulaması belirtmeniz yeterlidir [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) ve tercih ettiğiniz bir bağımlılık ekleme altyapısı ile kaydeder ve onu Microsoft ASP.NET WebHooks tarafından toplanma.</span><span class="sxs-lookup"><span data-stu-id="ec7d6-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="ec7d6-112">Lütfen bakın [bağımlılık ekleme ASP.NET Web API 2'deki](https://www.asp.net/web-api/overview/advanced/dependency-injection) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="ec7d6-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
