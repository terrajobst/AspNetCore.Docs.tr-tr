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
# <a name="aspnet-webhooks-logging"></a>ASP.NET Web Kancalarını günlüğe kaydetme

Microsoft ASP.NET WebHooks günlüğe kaydetme sorunları raporlama bir yolu olarak kullanır. Varsayılan olarak günlükleri kullanılarak yazılan [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) burada manged olabilirler kullanarak [izleme dinleyicileri](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) herhangi bir günlük akışı ister.

Web uygulamanızı Azure Web uygulaması olarak dağıtırken günlükleri otomatik olarak toplanmış ve diğer birlikte yönetilebilir [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) günlüğü. Ayrıntılar için lütfen bkz [Azure App Service'te web uygulamalarını için tanılama günlüğünü etkinleştirme](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)

Ayrıca, günlükleri düz öğesinden elde edilebilir açıklandığı gibi Visual Studio içinde [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Günlükleri yeniden yönlendirme

Günlükleri yazmak yerine [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), doğrudan bir günlük yöneticisi gibi oturum alternatif günlük kaydı uygulaması sağlamak mümkün [Log4Net](http://logging.apache.org/log4net/) ve [NLog ](http://nlog-project.org/). Uygulaması belirtmeniz yeterlidir [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) ve tercih ettiğiniz bir bağımlılık ekleme altyapısı ile kaydeder ve onu Microsoft ASP.NET WebHooks tarafından toplanma. Lütfen bakın [bağımlılık ekleme ASP.NET Web API 2'deki](https://www.asp.net/web-api/overview/advanced/dependency-injection) Ayrıntılar için.
