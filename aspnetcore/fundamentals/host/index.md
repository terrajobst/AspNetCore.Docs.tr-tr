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
# <a name="host-in-aspnet-core"></a><span data-ttu-id="0b1f3-103">ASP.NET Core ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="0b1f3-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="0b1f3-104">.NET uygulamaları yapılandırmak ve başlatma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="0b1f3-105">Ana bilgisayar için uygulama başlatma ve ömrü Yönetimi sorumludur.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="0b1f3-106">İki ana API'leri kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="0b1f3-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="0b1f3-107">[Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="0b1f3-108">[Genel ana bilgisayar](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya sonrası) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalışan uygulamalar) barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="0b1f3-109">Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere her türlü barındırma için uygun olacaktır.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="0b1f3-110">Genel ana bilgisayar sonunda Web ana bilgisayarı yerini alır.</span><span class="sxs-lookup"><span data-stu-id="0b1f3-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="0b1f3-111">ASP.NET Core barındırmak için *web uygulamaları*, geliştiricilerin temel Web barındırma kullanması gereken [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0b1f3-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="0b1f3-112">Barındırma için *olmayan web uygulamaları*, geliştiricilerin genel göre ana kullanması gereken [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="0b1f3-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
