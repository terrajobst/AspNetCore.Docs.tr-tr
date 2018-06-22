---
title: Microsoft.AspNetCore.All metapackage ASP.NET Core 2.0 ve üzeri
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage bağımlılıklarını yanı sıra tüm desteklenen ASP.NET Core ve Entity Framework Çekirdek paketleri içerir.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: 2fddc59d74dce4b114b5b4ed0646f773eb66ffb9
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277831"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="3d508-103">ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="3d508-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="3d508-104">ASP.NET Core 2.1 hedefleme uygulamaları önerilir ve daha sonra kullanmak [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) yerine bu paketi.</span><span class="sxs-lookup"><span data-stu-id="3d508-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="3d508-105">Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçiş](#migrate) bu makalede.</span><span class="sxs-lookup"><span data-stu-id="3d508-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="3d508-106">Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.</span><span class="sxs-lookup"><span data-stu-id="3d508-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="3d508-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) ASP.NET Core metapackage içerir:</span><span class="sxs-lookup"><span data-stu-id="3d508-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="3d508-108">Tüm paketler ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3d508-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="3d508-109">Tüm paketleri Entity Framework Çekirdek tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3d508-109">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="3d508-110">ASP.NET Core ve Entity Framework Çekirdek tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="3d508-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="3d508-111">ASP.NET Core tüm özelliklerini 2.x ve Entity Framework Çekirdek 2.x dahil edilmiştir `Microsoft.AspNetCore.All` paket.</span><span class="sxs-lookup"><span data-stu-id="3d508-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="3d508-112">Bu paket ASP.NET Core 2.0 hedefleme varsayılan proje şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="3d508-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="3d508-113">Sürüm numarasını `Microsoft.AspNetCore.All` ASP.NET Core sürüm ve Entity Framework Çekirdek sürüm metapackage temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3d508-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="3d508-114">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET çekirdeği çalışma zamanı deposu](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="3d508-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="3d508-115">Çalışma zamanı deposu ASP.NET Core 2.x uygulamaları çalıştırmak için gereken tüm çalışma zamanı varlıklarını içerir.</span><span class="sxs-lookup"><span data-stu-id="3d508-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="3d508-116">Kullandığınızda `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan ASP.NET Core NuGet paketlerini varlıklarından uygulama ile dağıtılan &mdash; .NET çekirdeği çalışma zamanı deposu bu varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3d508-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="3d508-117">Çalışma zamanı deposundaki varlıklar uygulama başlangıç zamanını geliştirmek için önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="3d508-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="3d508-118">Paket kesme işlemi kullanmadığınız paketleri kaldırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d508-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="3d508-119">Yayımlanan uygulama çıktıda bölünen paketleri bırakılır.</span><span class="sxs-lookup"><span data-stu-id="3d508-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="3d508-120">Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="3d508-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="3d508-121">Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme</span><span class="sxs-lookup"><span data-stu-id="3d508-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="3d508-122">Aşağıdaki paketler içinde yer alan `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket.</span><span class="sxs-lookup"><span data-stu-id="3d508-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="3d508-123">Sunucudan taşımak için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`, yukarıdaki paketlerinden herhangi bir API uygulamanızı kullanıyorsa veya paketler getirildi bu paketleri tarafından bu Paketlerine yönelik başvuruları projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3d508-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="3d508-124">Aksi takdirde bağımlılıkları olmayan önceki paket bağımlılıkları `Microsoft.AspNetCore.App` örtük olarak dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="3d508-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="3d508-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3d508-125">For example:</span></span>

* <span data-ttu-id="3d508-126">`StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="3d508-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="3d508-127">`Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="3d508-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
