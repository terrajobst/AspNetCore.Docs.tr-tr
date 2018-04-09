---
title: ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x ve sonraki sürümler
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage bağımlılıklarını yanı sıra tüm desteklenen ASP.NET Core ve Entity Framework Çekirdek paketleri içerir.
manager: wpickett
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 69a1683feb66f9da5fe61378c4a4ed55e08d39ac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="9696c-103">ASP.NET Core Microsoft.AspNetCore.All metapackage 2.x</span><span class="sxs-lookup"><span data-stu-id="9696c-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="9696c-104">Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.</span><span class="sxs-lookup"><span data-stu-id="9696c-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="9696c-105">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:</span><span class="sxs-lookup"><span data-stu-id="9696c-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="9696c-106">Tüm paketler ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9696c-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="9696c-107">Tüm paketleri Entity Framework Çekirdek tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="9696c-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="9696c-108">ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="9696c-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="9696c-109">ASP.NET Core tüm özelliklerini 2.x ve Entity Framework Çekirdek 2.x dahil edilmiştir `Microsoft.AspNetCore.All` paket.</span><span class="sxs-lookup"><span data-stu-id="9696c-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="9696c-110">Bu paket varsayılan proje şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="9696c-110">The default project templates use this package.</span></span>

<span data-ttu-id="9696c-111">Sürüm numarasını `Microsoft.AspNetCore.All` metapackage ASP.NET Core sürüm ve Entity Framework Çekirdek sürüm (.NET Core sürümüyle hizalı) temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9696c-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="9696c-112">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET çekirdeği çalışma zamanı deposu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="9696c-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="9696c-113">Çalışma zamanı deposu ASP.NET Core 2.x uygulamaları çalıştırmak için gereken tüm çalışma zamanı varlıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="9696c-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="9696c-114">Kullandığınızda `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan ASP.NET Core NuGet paketlerini varlıklarından uygulama ile dağıtılan &mdash; .NET çekirdeği çalışma zamanı deposu bu varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="9696c-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="9696c-115">Çalışma zamanı deposundaki varlıklar uygulama başlangıç zamanını geliştirmek için önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="9696c-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="9696c-116">Paket kesme işlemi kullanmadığınız paketleri kaldırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9696c-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="9696c-117">Yayımlanan uygulama çıktıda bölünen paketleri bırakılır.</span><span class="sxs-lookup"><span data-stu-id="9696c-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="9696c-118">Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="9696c-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
