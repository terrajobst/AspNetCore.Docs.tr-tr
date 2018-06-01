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
ms.openlocfilehash: 37c527718433410eede8321dd7813f0ffd6473e5
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687450"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="e8f86-103">ASP.NET Core ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="e8f86-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="e8f86-104">.NET uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="e8f86-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="e8f86-105">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="e8f86-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="e8f86-106">İki ana API'leri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="e8f86-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="e8f86-107">[Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="e8f86-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="e8f86-108">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya sonrası) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalışan uygulamalar) barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="e8f86-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="e8f86-109">Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere her türlü barındırma için uygun olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e8f86-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="e8f86-110">Genel ana bilgisayar sonunda Web ana bilgisayarı yerini alır.</span><span class="sxs-lookup"><span data-stu-id="e8f86-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="e8f86-111">Şu anda geliştiriciler kullanmalıdır [Web ana bilgisayarı](xref:fundamentals/host/web-host) göre [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) ASP.NET Core uygulamaları barındırmak için.</span><span class="sxs-lookup"><span data-stu-id="e8f86-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) for hosting ASP.NET Core apps.</span></span>
