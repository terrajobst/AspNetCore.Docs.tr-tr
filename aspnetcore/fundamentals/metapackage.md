---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: ASP.NET Core 2.1 ve üzeri Microsoft.AspNetCore.All metapackage önerilmez.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148856"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="20abc-103">ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="20abc-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="20abc-104">ASP.NET Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) bu paket yerine.</span><span class="sxs-lookup"><span data-stu-id="20abc-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="20abc-105">Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](#migrate) bu makaledeki.</span><span class="sxs-lookup"><span data-stu-id="20abc-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="20abc-106">Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.</span><span class="sxs-lookup"><span data-stu-id="20abc-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="20abc-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage ASP.NET Core içerir:</span><span class="sxs-lookup"><span data-stu-id="20abc-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="20abc-108">Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="20abc-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="20abc-109">Tüm paketleri Entity Framework Core tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="20abc-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="20abc-110">ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="20abc-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="20abc-111">ASP.NET Core özelliklerinin tümünü 2.x ve Entity Framework Core 2.x dahil edilecek `Microsoft.AspNetCore.All` paket.</span><span class="sxs-lookup"><span data-stu-id="20abc-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="20abc-112">Bu paket hedefleyen ASP.NET Core 2.0 varsayılan proje şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="20abc-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="20abc-113">Sürüm numarasını `Microsoft.AspNetCore.All` metapackage sürümü Entity Framework Core ve ASP.NET Core sürümünü temsil eder.</span><span class="sxs-lookup"><span data-stu-id="20abc-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="20abc-114">Kullanan uygulamalar `Microsoft.AspNetCore.All` metapackage otomatik olarak avantajından [.NET Core çalışma zamanı Store](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="20abc-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="20abc-115">Çalışma zamanı Store, ASP.NET Core 2.x uygulamaları çalıştırmak için gerekli olan tüm çalışma zamanı varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="20abc-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="20abc-116">Kullanırken `Microsoft.AspNetCore.All` metapackage, **hiçbir** başvurulan bir ASP.NET Core NuGet paket varlıklarından uygulamayla dağıtılan &mdash; .NET Core çalışma zamanı Store bu varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="20abc-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="20abc-117">Uygulama başlatma süresini iyileştirmek için çalışma zamanı Store varlıkları önceden derlenmiş.</span><span class="sxs-lookup"><span data-stu-id="20abc-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="20abc-118">Paket kesme işlemi kullanmadığınız paketlerini kaldırmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20abc-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="20abc-119">Yayımlanmış uygulama çıktıda kırpılmış paketler bırakılır.</span><span class="sxs-lookup"><span data-stu-id="20abc-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="20abc-120">Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="20abc-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="20abc-121">Örtük sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="20abc-121">Implicit versioning</span></span>

<span data-ttu-id="20abc-122">ASP.NET Core 2.1 veya daha sonra belirtebilirsiniz `Microsoft.AspNetCore.All` paketini başvuru olmadan bir sürüm.</span><span class="sxs-lookup"><span data-stu-id="20abc-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="20abc-123">Version belirtilmediğinde, SDK tarafından belirtilen örtük bir sürüm (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="20abc-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="20abc-124">SDK'sı tarafından belirtilen sürüm numarası paket başvurusu üzerinde açıkça ayarlamak örtük sürümü güvenmek öneririz.</span><span class="sxs-lookup"><span data-stu-id="20abc-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="20abc-125">Bu yaklaşım hakkında sorularınız varsa, GitHub yorum [Microsoft.AspNetCore.App örtük sürümü için tartışma](https://github.com/aspnet/Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="20abc-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="20abc-126">Örtük sürüm kümesine `major.minor.0` taşınabilir uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="20abc-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="20abc-127">Paylaşılan çerçeve sarma mekanizması uygulamanın en yeni uyumlu sürümü yüklü paylaşılan çerçeveleri arasında çalışır.</span><span class="sxs-lookup"><span data-stu-id="20abc-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="20abc-128">Aynı sürüm, geliştirme, test ve üretim kullanılan sağlamak için paylaşılan framework sürümüyle aynı sürümü, tüm ortamlara yüklenen emin olun.</span><span class="sxs-lookup"><span data-stu-id="20abc-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="20abc-129">Bağımsız uygulamalar için örtük bir sürüm numarası ayarlanır `major.minor.patch` yüklü SDK'yı paketlenmiş paylaşılan framework'ün.</span><span class="sxs-lookup"><span data-stu-id="20abc-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="20abc-130">Sürüm numarasını belirtme `Microsoft.AspNetCore.All` paket başvurusu mu **değil** paylaşılan sürümünün garanti framework seçilir.</span><span class="sxs-lookup"><span data-stu-id="20abc-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="20abc-131">Örneğin, sürümü "2.1.1" belirtildi, ancak "2.1.3" yüklü olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="20abc-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="20abc-132">Bu durumda, uygulama "2.1.3" kullanır.</span><span class="sxs-lookup"><span data-stu-id="20abc-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="20abc-133">Önerilmemesine rağmen ileri sarma (düzeltme eki ve/veya ikincil) devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="20abc-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="20abc-134">Dotnet konak sarma ve davranışını yapılandırma hakkında daha fazla bilgi için bkz. [dotnet konak ileri sarma](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="20abc-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="20abc-135">Projenin SDK ayarlanmalıdır `Microsoft.NET.Sdk.Web` örtük sürümünü kullanmak için proje dosyasında `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="20abc-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="20abc-136">Zaman `Microsoft.NET.Sdk` SDK belirtildi (`<Project Sdk="Microsoft.NET.Sdk">` proje dosyasının üst), aşağıdaki uyarısı oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="20abc-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="20abc-137">*Uyarı NU1604: Proje bağımlılığı Microsoft.AspNetCore.All kapsamlı bir alt sınırı içermiyor. Alt sınır tutarlı geri yükleme sonuçları emin olmak için bağımlılık sürümünü içerir.*</span><span class="sxs-lookup"><span data-stu-id="20abc-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="20abc-138">Bu, .NET Core 2.1 SDK'sı ile bilinen bir sorundur ve .NET Core 2.2 SDK düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="20abc-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="20abc-139">Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme</span><span class="sxs-lookup"><span data-stu-id="20abc-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="20abc-140">Aşağıdaki paketler dahil `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket.</span><span class="sxs-lookup"><span data-stu-id="20abc-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="20abc-141">Taşıma için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`yukarıdaki paketlerinden herhangi bir API uygulamanızın kullandığı veya paketleri getirilir, bu paketleri tarafından bu paketleri başvuruları projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="20abc-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="20abc-142">Aksi takdirde, bağımlılıkları olmayan yukarıdaki paketlerin herhangi bir bağımlılığın `Microsoft.AspNetCore.App` örtük olarak dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="20abc-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="20abc-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="20abc-143">For example:</span></span>

* <span data-ttu-id="20abc-144">`StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="20abc-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="20abc-145">`Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="20abc-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="20abc-146">ASP.NET Core 2.1 güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="20abc-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="20abc-147">Geçiş öneririz `Microsoft.AspNetCore.App` metapackage 2.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="20abc-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="20abc-148">Kullanmaya devam etmek için `Microsoft.AspNetCore.All` metapackage ve en son düzeltme eki sürümü dağıtıldığı emin olun:</span><span class="sxs-lookup"><span data-stu-id="20abc-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="20abc-149">Geliştirme makineler ve yapı sunucularını: en yeni [.NET Core SDK'sı](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="20abc-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="20abc-150">Dağıtım sunucularında: en yeni [.NET Core çalışma zamanı](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="20abc-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="20abc-151">Uygulamanız için en son yüklenen sürüm üzerinde uygulama yeniden başlatma İleri dökümünü yapar.</span><span class="sxs-lookup"><span data-stu-id="20abc-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
