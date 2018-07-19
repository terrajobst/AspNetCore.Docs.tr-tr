---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: Microsoft.AspNetCore.All metapackage bağımlılıklarıyla desteklenen tüm ASP.NET Core ve Entity Framework Core paketleri içerir.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage
ms.openlocfilehash: fbc0f5465dc37a612b81c293f1a58b53ea4b2238
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123833"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="3cc3c-103">ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="3cc3c-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="3cc3c-104">ASP.NET Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) bu paket yerine.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="3cc3c-105">Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](#migrate) bu makaledeki.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="3cc3c-106">Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="3cc3c-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:</span><span class="sxs-lookup"><span data-stu-id="3cc3c-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="3cc3c-108">Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="3cc3c-109">Tüm paketleri Entity Framework Core tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="3cc3c-110">ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="3cc3c-111">ASP.NET Core özelliklerinin tümünü 2.x ve Entity Framework Core 2.x dahil edilecek `Microsoft.AspNetCore.All` paket.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="3cc3c-112">Bu paket hedefleyen ASP.NET Core 2.0 varsayılan proje şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="3cc3c-113">Sürüm numarasını `Microsoft.AspNetCore.All` metapackage sürümü Entity Framework Core ve ASP.NET Core sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="3cc3c-114">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET Core çalışma zamanı Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="3cc3c-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="3cc3c-115">Çalışma zamanı Store, ASP.NET Core 2.x uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="3cc3c-116">Kullanırken `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan bir ASP.NET Core NuGet paket varlıklarından uygulamayla dağıtılan &mdash; .NET Core çalışma zamanı Store bu varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="3cc3c-117">Uygulama başlatma süresini iyileştirmek için çalışma zamanı Store varlıkları önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="3cc3c-118">Paket kesme işlemi kullanmadığınız paketlerini kaldırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="3cc3c-119">Yayımlanmış uygulama çıktıda kırpılmış paketler bırakılır.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="3cc3c-120">Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="3cc3c-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=6)]

<a name="migrate"></a>
## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="3cc3c-121">Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme</span><span class="sxs-lookup"><span data-stu-id="3cc3c-121">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="3cc3c-122">Aşağıdaki paketler dahil `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-122">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span> 

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

<span data-ttu-id="3cc3c-123">Taşıma için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`yukarıdaki paketlerinden herhangi bir API uygulamanızın kullandığı veya paketleri getirilir, bu paketleri tarafından bu paketleri başvuruları projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-123">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="3cc3c-124">Aksi takdirde, bağımlılıkları olmayan yukarıdaki paketlerin herhangi bir bağımlılığın `Microsoft.AspNetCore.App` örtük olarak dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="3cc3c-124">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="3cc3c-125">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3cc3c-125">For example:</span></span>

* <span data-ttu-id="3cc3c-126">`StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="3cc3c-126">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="3cc3c-127">`Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="3cc3c-127">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>
