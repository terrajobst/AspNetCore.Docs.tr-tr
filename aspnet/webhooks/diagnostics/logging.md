---
uid: webhooks/diagnostics/logging
title: ASP.NET oturum Web kancaları | Microsoft Docs
author: rick-anderson
description: ASP.NET Web Kancalarıyla günlüğü nasıl.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.openlocfilehash: 2e86d519c24da102075b4da0a32787c90deb0f6b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753348"
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET oturum Web kancaları

Microsoft ASP.NET WebHooks günlüğe kaydetme sorunları raporlama bir yolu olarak kullanır. Varsayılan olarak günlükleri kullanarak yazılır [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) burada manged olabilirler kullanarak [izleme dinleyicilerine](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) herhangi bir günlük akışı ister.

Web uygulamanızı bir Azure Web App olarak dağıtırken, günlükleri otomatik olarak seçilir ve diğer birlikte yönetilebilir [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) günlüğe kaydetme. Ayrıntılar için lütfen bkz [Azure App Service'te web apps için tanılama günlüğünü etkinleştirme](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Ayrıca, günlükleri düz örneğinden alınabilen açıklandığı gibi Visual Studio içinde [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Günlükleri yeniden yönlendirme

Günlükleri yazmak yerine [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), doğrudan bir günlük yöneticisi gibi oturum diğer günlük kaydı uygulaması sağlamak mümkün [Log4Net](http://logging.apache.org/log4net/) ve [NLog ](http://nlog-project.org/). Yalnızca bir uygulamasını sağlamak [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) kendi seçtiğiniz bir bağımlılık ekleme altyapısıyla kaydetmek ve, Microsoft ASP.NET WebHooks tarafından toplanmış. Lütfen [ASP.NET Web API 2'de bağımlılık ekleme](https://www.asp.net/web-api/overview/advanced/dependency-injection) Ayrıntılar için.
