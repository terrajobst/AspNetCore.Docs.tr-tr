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
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="04903-103">ASP.NET Web Kancalarını günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="04903-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="04903-104">Microsoft ASP.NET WebHooks günlüğe kaydetme sorunları raporlama bir yolu olarak kullanır.</span><span class="sxs-lookup"><span data-stu-id="04903-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="04903-105">Varsayılan olarak günlükleri kullanılarak yazılan [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) burada manged olabilirler kullanarak [izleme dinleyicileri](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) herhangi bir günlük akışı ister.</span><span class="sxs-lookup"><span data-stu-id="04903-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="04903-106">Web uygulamanızı Azure Web uygulaması olarak dağıtırken günlükleri otomatik olarak toplanmış ve diğer birlikte yönetilebilir [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) günlüğü.</span><span class="sxs-lookup"><span data-stu-id="04903-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="04903-107">Ayrıntılar için lütfen bkz [Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="04903-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="04903-108">Ayrıca, günlükleri düz öğesinden elde edilebilir açıklandığı gibi Visual Studio içinde [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="04903-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="04903-109">Günlükleri yeniden yönlendirme</span><span class="sxs-lookup"><span data-stu-id="04903-109">Redirecting Logs</span></span>

<span data-ttu-id="04903-110">Günlükleri yazmak yerine [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), doğrudan bir günlük yöneticisi gibi oturum alternatif günlük kaydı uygulaması sağlamak mümkün [Log4Net](http://logging.apache.org/log4net/) ve [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="04903-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="04903-111">Uygulaması belirtmeniz yeterlidir [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) ve tercih ettiğiniz bir bağımlılık ekleme altyapısı ile kaydeder ve onu Microsoft ASP.NET WebHooks tarafından toplanma.</span><span class="sxs-lookup"><span data-stu-id="04903-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="04903-112">Lütfen bakın [bağımlılık ekleme ASP.NET Web API 2'deki](https://www.asp.net/web-api/overview/advanced/dependency-injection) Ayrıntılar için.</span><span class="sxs-lookup"><span data-stu-id="04903-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
