---
title: ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage
author: Rick-Anderson
description: ASP.NET Core 2.1 ve üzeri Microsoft.AspNetCore.All metapackage önerilmez.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: 5d49213e6d694f121d8301c94ba71782b2dc45cf
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65086940"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="6e5a7-103">ASP.NET Core 2.0 için Microsoft.AspNetCore.All metapackage</span><span class="sxs-lookup"><span data-stu-id="6e5a7-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="6e5a7-104">ASP.NET Core 2.1 hedefleyen uygulamalar önerilir ve daha sonra [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) bu paket yerine.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="6e5a7-105">Bkz: [Microsoft.AspNetCore.App için Microsoft.AspNetCore.All geçirme](#migrate) bu makaledeki.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="6e5a7-106">Bu özellik, ASP.NET Core 2.x hedefleme .NET gerektirir 2.x çekirdek.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="6e5a7-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) için paylaşılan bir çerçeve başvuran bir metapackage olduğu.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-107">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) is a metapackage that refers to a shared framework.</span></span> <span data-ttu-id="6e5a7-108">A *paylaşılan çerçeve* derlemeleri kümesidir (*.dll* dosyaları) uygulamanın klasörlerinde olmayan.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-108">A *shared framework* is a set of assemblies (*.dll* files) that are not in the app's folders.</span></span> <span data-ttu-id="6e5a7-109">Paylaşılan framework uygulamasını çalıştırmak için makinede yüklü olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-109">The shared framework must be installed on the machine to run the app.</span></span> <span data-ttu-id="6e5a7-110">Daha fazla bilgi için [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-110">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="6e5a7-111">Paylaşılan çerçeve, `Microsoft.AspNetCore.All` içeren ifade eder:</span><span class="sxs-lookup"><span data-stu-id="6e5a7-111">The shared framework that `Microsoft.AspNetCore.All` refers to includes:</span></span>

* <span data-ttu-id="6e5a7-112">Tüm paketleri ASP.NET Core ekibi tarafından desteklenir.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-112">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="6e5a7-113">Tüm paketleri Entity Framework Core tarafından desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-113">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="6e5a7-114">ASP.NET Core ve Entity Framework Core tarafından kullanılan iç ve 3. taraf bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-114">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="6e5a7-115">ASP.NET Core özelliklerinin tümünü 2.x ve Entity Framework Core 2.x dahil edilecek `Microsoft.AspNetCore.All` paket.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-115">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="6e5a7-116">Bu paket hedefleyen ASP.NET Core 2.0 varsayılan proje şablonları kullanın.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-116">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="6e5a7-117">Sürüm numarasını `Microsoft.AspNetCore.All` sürümü Entity Framework Core ve ASP.NET Core ve üstünü metapackage temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-117">The version number of the `Microsoft.AspNetCore.All` metapackage represents the minimum ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="6e5a7-118">Aşağıdaki *.csproj* dosya başvuruları `Microsoft.AspNetCore.All` metapackage ASP.NET Core için:</span><span class="sxs-lookup"><span data-stu-id="6e5a7-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="6e5a7-119">Örtük sürüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="6e5a7-119">Implicit versioning</span></span>

<span data-ttu-id="6e5a7-120">ASP.NET Core 2.1 veya daha sonra belirtebilirsiniz `Microsoft.AspNetCore.All` paketini başvuru olmadan bir sürüm.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-120">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="6e5a7-121">Version belirtilmediğinde, SDK tarafından belirtilen örtük bir sürüm (`Microsoft.NET.Sdk.Web`).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-121">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="6e5a7-122">SDK'sı tarafından belirtilen sürüm numarası paket başvurusu üzerinde açıkça ayarlamak örtük sürümü güvenmek öneririz.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-122">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="6e5a7-123">Bu yaklaşım hakkında sorularınız varsa, GitHub yorum [Microsoft.AspNetCore.App örtük sürümü için tartışma](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-123">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/AspNetCore.Docs/issues/6430).</span></span>

<span data-ttu-id="6e5a7-124">Örtük sürüm kümesine `major.minor.0` taşınabilir uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-124">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="6e5a7-125">Paylaşılan çerçeve sarma mekanizması uygulamanın en yeni uyumlu sürümü yüklü paylaşılan çerçeveleri arasında çalışır.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-125">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="6e5a7-126">Aynı sürüm, geliştirme, test ve üretim kullanılan sağlamak için paylaşılan framework sürümüyle aynı sürümü, tüm ortamlara yüklenen emin olun.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-126">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="6e5a7-127">Bağımsız uygulamalar için örtük bir sürüm numarası ayarlanır `major.minor.patch` yüklü SDK'yı paketlenmiş paylaşılan framework'ün.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-127">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="6e5a7-128">Sürüm numarasını belirtme `Microsoft.AspNetCore.All` paket başvurusu mu **değil** paylaşılan sürümünün garanti framework seçilir.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-128">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="6e5a7-129">Örneğin, sürümü "2.1.1" belirtildi, ancak "2.1.3" yüklü olduğunu varsayalım.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-129">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="6e5a7-130">Bu durumda, uygulama "2.1.3" kullanır.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-130">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="6e5a7-131">Önerilmemesine rağmen ileri sarma (düzeltme eki ve/veya ikincil) devre dışı bırakabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-131">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="6e5a7-132">Dotnet konak sarma ve davranışını yapılandırma hakkında daha fazla bilgi için bkz. [dotnet konak ileri sarma](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-132">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="6e5a7-133">Projenin SDK ayarlanmalıdır `Microsoft.NET.Sdk.Web` örtük sürümünü kullanmak için proje dosyasında `Microsoft.AspNetCore.All`.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-133">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="6e5a7-134">Zaman `Microsoft.NET.Sdk` SDK belirtildi (`<Project Sdk="Microsoft.NET.Sdk">` proje dosyasının üst), aşağıdaki uyarısı oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6e5a7-134">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="6e5a7-135">*Uyarı NU1604: Proje bağımlılığı Microsoft.AspNetCore.All kapsamlı bir alt sınırı içermiyor. Alt sınır tutarlı geri yükleme sonuçları emin olmak için bağımlılık sürümünü içerir.*</span><span class="sxs-lookup"><span data-stu-id="6e5a7-135">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="6e5a7-136">Bu, .NET Core 2.1 SDK'sı ile bilinen bir sorundur ve .NET Core 2.2 SDK düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-136">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="6e5a7-137">Microsoft.AspNetCore.All Microsoft.AspNetCore.App için geçirme</span><span class="sxs-lookup"><span data-stu-id="6e5a7-137">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="6e5a7-138">Aşağıdaki paketler dahil `Microsoft.AspNetCore.All` ama `Microsoft.AspNetCore.App` paket.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-138">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

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

<span data-ttu-id="6e5a7-139">Taşıma için `Microsoft.AspNetCore.All` için `Microsoft.AspNetCore.App`yukarıdaki paketlerinden herhangi bir API uygulamanızın kullandığı veya paketleri getirilir, bu paketleri tarafından bu paketleri başvuruları projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-139">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="6e5a7-140">Aksi takdirde, bağımlılıkları olmayan yukarıdaki paketlerin herhangi bir bağımlılığın `Microsoft.AspNetCore.App` örtük olarak dahil edilmez.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-140">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="6e5a7-141">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="6e5a7-141">For example:</span></span>

* <span data-ttu-id="6e5a7-142">`StackExchange.Redis` bir bağımlılık olarak `Microsoft.Extensions.Caching.Redis`</span><span class="sxs-lookup"><span data-stu-id="6e5a7-142">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="6e5a7-143">`Microsoft.ApplicationInsights` bir bağımlılık olarak `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span><span class="sxs-lookup"><span data-stu-id="6e5a7-143">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="6e5a7-144">ASP.NET Core 2.1 güncelleştirmesi</span><span class="sxs-lookup"><span data-stu-id="6e5a7-144">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="6e5a7-145">Geçiş öneririz `Microsoft.AspNetCore.App` metapackage 2.1 ve üzeri.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-145">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="6e5a7-146">Kullanmaya devam etmek için `Microsoft.AspNetCore.All` metapackage ve en son düzeltme eki sürümü dağıtıldığı emin olun:</span><span class="sxs-lookup"><span data-stu-id="6e5a7-146">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="6e5a7-147">Makineleri geliştirme ve derleme sunucuları: Son yükleme [.NET Core SDK'sı](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-147">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="6e5a7-148">Dağıtım sunucularında: Son yükleme [.NET Core çalışma zamanı](https://www.microsoft.com/net/download).</span><span class="sxs-lookup"><span data-stu-id="6e5a7-148">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="6e5a7-149">Uygulamanız için en son yüklenen sürüm üzerinde uygulama yeniden başlatma İleri dökümünü yapar.</span><span class="sxs-lookup"><span data-stu-id="6e5a7-149">Your app will roll forward to the latest installed version on an application restart.</span></span>
