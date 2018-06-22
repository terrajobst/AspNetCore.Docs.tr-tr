---
title: ASP.NET Core ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET çekirdek Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276623"
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core ana bilgisayar

.NET uygulamaları yapılandırmak ve başlatma bir *konak*. Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur. İki ana API'leri kullanılabilir:

* [Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.
* [Genel ana bilgisayar](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya sonrası) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalışan uygulamalar) barındırmak için uygundur. Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere her türlü barındırma için uygun olacaktır. Genel ana bilgisayar sonunda Web ana bilgisayarı yerini alır.

ASP.NET Core barındırmak için *web uygulamaları*, geliştiricilerin temel Web barındırma kullanması gereken [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Barındırma için *olmayan web uygulamaları*, geliştiricilerin genel göre ana kullanması gereken [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
