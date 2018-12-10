---
title: Web ana bilgisayarı ve ASP.NET Core genel ana bilgisayar
author: guardrex
description: Uygulama başlatma ve ömür yönetimi için sorumlu ASP.NET Core Web ana bilgisayarı ve .NET genel ana bilgisayar hakkında bilgi edinin.
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121524"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a><span data-ttu-id="03058-103">Web ana bilgisayarı ve ASP.NET Core genel ana bilgisayar</span><span class="sxs-lookup"><span data-stu-id="03058-103">Web Host and Generic Host in ASP.NET Core</span></span>

<span data-ttu-id="03058-104">.NET uygulamaları ve başlatma yapılandırma bir *konak*.</span><span class="sxs-lookup"><span data-stu-id="03058-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="03058-105">Uygulama başlatma ve ömür yönetimi için konak sorumludur.</span><span class="sxs-lookup"><span data-stu-id="03058-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="03058-106">İki ana API'leri kullanılabilir duruma gelir:</span><span class="sxs-lookup"><span data-stu-id="03058-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="03058-107">[Web ana bilgisayar](xref:fundamentals/host/web-host) &ndash; web uygulamalarını barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="03058-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="03058-108">[Genel konak](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 veya üzeri) &ndash; olmayan web uygulamaları (örneğin, arka plan görevleri çalıştırma uygulamaları) barındırmak için uygundur.</span><span class="sxs-lookup"><span data-stu-id="03058-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="03058-109">Gelecekteki bir sürümde genel ana bilgisayar uygulaması, web uygulamaları dahil olmak üzere herhangi bir türden barındırma için uygun olacaktır.</span><span class="sxs-lookup"><span data-stu-id="03058-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="03058-110">Genel konak Web ana bilgisayarı sonunda yerini alır.</span><span class="sxs-lookup"><span data-stu-id="03058-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="03058-111">ASP.NET Core barındırma *web uygulamaları*, geliştiriciler, temel Web barındırma kullanmalıdır <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="03058-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>.</span></span> <span data-ttu-id="03058-112">Barındırma için *olmayan web apps*, geliştiricilere genel bir temel konak kullanması gereken <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="03058-112">For hosting *non-web apps*, developers should use the Generic Host based on <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span>

<xref:fundamentals/host/hosted-services>  
<span data-ttu-id="03058-113">ASP.NET Core barındırılan hizmetler ile arka plan görevleri uygulamak öğrenin.</span><span class="sxs-lookup"><span data-stu-id="03058-113">Learn how to implement background tasks with hosted services in ASP.NET Core.</span></span>

<xref:fundamentals/configuration/platform-specific-configuration>  
<span data-ttu-id="03058-114">ASP.NET Core uygulaması kullanarak başvurulan veya başvurulmayan bir bütünleştirilmiş koddan geliştirmek nasıl bir <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> uygulaması.</span><span class="sxs-lookup"><span data-stu-id="03058-114">Discover how to enhance an ASP.NET Core app from a referenced or unreferenced assembly using an <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation.</span></span>
