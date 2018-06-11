---
title: ASP.NET Core ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür boyu yönetimi için sorumlu ASP.NET çekirdek Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252015"
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core ana bilgisayar

.NET uygulamaları yapılandırmak ve başlatma bir *konak*. Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur. İki ana API'leri kullanılabilir:

* [Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.
* [Genel ana bilgisayar](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya sonrası) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalışan uygulamalar) barındırmak için uygundur. Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere her türlü barındırma için uygun olacaktır. Genel ana bilgisayar sonunda Web ana bilgisayarı yerini alır.

ASP.NET Core barındırmak için *web uygulamaları*, geliştiricilerin temel Web barındırma kullanması gereken [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder). Barındırma için *olmayan web uygulamaları*, geliştiricilerin genel göre ana kullanması gereken [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).
