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
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core ana bilgisayar

.NET uygulamaları yapılandırmak ve başlatma bir *konak*. Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur. İki ana API'leri kullanılabilir:

* [Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.
* [Genel ana bilgisayar](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya sonrası) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalışan uygulamalar) barındırmak için uygundur. Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere her türlü barındırma için uygun olacaktır. Genel ana bilgisayar sonunda Web ana bilgisayarı yerini alır.

Şu anda geliştiriciler kullanmalıdır [Web ana bilgisayarı](xref:fundamentals/host/web-host) göre [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) ASP.NET Core uygulamaları barındırmak için.
