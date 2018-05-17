---
title: Razor görünüm derleme ve ASP.NET Core içinde ön derlemesi
author: rick-anderson
description: MVC Razor görünüm derleme ve ASP.NET Core uygulamalarda ön derlemesi etkinleştirmeyi öğrenin.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 013ca0d149c6415b5e6825aa5a48e93ae48f6728
ms.sourcegitcommit: 0063338c2e130409081bb60fcffa0c3f190cd46a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="razor-file-cshtml-compilation-in-aspnet-core"></a><span data-ttu-id="ca390-103">Razor dosya (.cshtml) ASP.NET Core derlemede</span><span class="sxs-lookup"><span data-stu-id="ca390-103">Razor file (.cshtml) compilation in ASP.NET Core</span></span>

<span data-ttu-id="ca390-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca390-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca390-105">Görünüm çağrıldığında razor görünümleri çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="ca390-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="ca390-106">ASP.NET Core 2.1.0 veya üzeri derleme görünümlerde oluşturmak ve zaman kullanarak yayımlamak [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="ca390-106">ASP.NET Core 2.1.0 or later compile views at build and publish time using [Razor Sdk](/aspnetcore/mvc/razor-pages/sdk).</span></span> <span data-ttu-id="ca390-107">ASP.NET Core 1.1 ve ASP.NET Core 2.0, görünümleri isteğe bağlı olarak Yayımla derlenmiş ve değiştirebilirsiniz uygulamayla dağıtılan &mdash; ön derleme aracını kullanarak.</span><span class="sxs-lookup"><span data-stu-id="ca390-107">In ASP.NET Core 1.1, and ASP.NET Core 2.0, views can optionally be compiled at publish and deployed with the app &mdash; using the precompilation tool.</span></span> 



<span data-ttu-id="ca390-108">Ön derleme dikkate alınacak noktalar:</span><span class="sxs-lookup"><span data-stu-id="ca390-108">Precompilation considerations:</span></span>

* <span data-ttu-id="ca390-109">Görünümleri önceden derleme, bir küçük yayımlanan paket ve daha hızlı başlangıç saati ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="ca390-109">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="ca390-110">Görünümleri ön derleme yap sonra Razor dosyaları düzenleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="ca390-110">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="ca390-111">Düzenlenen görünümleri yayımlanan pakete mevcut olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="ca390-111">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="ca390-112">Önceden derlenmiş görünümleri dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="ca390-112">To deploy precompiled views:</span></span>

# <a name="aspnet-core-21tabaspnetcore21"></a>[<span data-ttu-id="ca390-113">ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ca390-113">ASP.NET Core 2.1</span></span>](#tab/aspnetcore21/)
<span data-ttu-id="ca390-114">Derleme ve Razor dosyaları derlenmesini Razor SDK'sı tarafından varsayılan olarak etkindir zaman yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="ca390-114">Build and publish time compilation of Razor files is enabled by default by the Razor Sdk.</span></span> <span data-ttu-id="ca390-115">Güncelleştirmeden sonra Razor dosyaları düzenleme derleme zamanında desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ca390-115">Editing Razor files after they are updated is supported at build time.</span></span> <span data-ttu-id="ca390-116">Varsayılan olarak, yalnızca derlenmiş *Views.dll* ve hiçbir cshtml dosyası uygulamanızla birlikte dağıtılır.</span><span class="sxs-lookup"><span data-stu-id="ca390-116">By default, only the compiled *Views.dll* and no cshtml files are deployed with your application.</span></span> 
    
> [!IMPORTANT]
> <span data-ttu-id="ca390-117">Razor SDK'sı etkili için hiçbir ön derleme belirli özellikler, proje dosyasında ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="ca390-117">The Razor Sdk is only effective when no precompilation specific properties are set in your project file.</span></span> <span data-ttu-id="ca390-118">Örneğin, ayarı `MvcRazorCompileOnPublish` içinde *.csproj* dosya Razor SDK'yı devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="ca390-118">For instance, setting `MvcRazorCompileOnPublish` in your *.csproj* file will disable the Razor Sdk.</span></span>

# <a name="aspnet-core-20tabaspnetcore20"></a>[<span data-ttu-id="ca390-119">ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="ca390-119">ASP.NET Core 2.0</span></span>](#tab/aspnetcore20/)

<span data-ttu-id="ca390-120">Projeniz .NET Framework hedefliyorsa, bir paket başvuru eklemek [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="ca390-120">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="ca390-121">Projeniz .NET Core hedefliyorsa, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ca390-121">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="ca390-122">ASP.NET Core 2.x proje şablonları örtük olarak ayarlar `MvcRazorCompileOnPublish` için `true` varsayılan olarak, yani bu düğüm güvenle kaldırılabilir gelen *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="ca390-122">The ASP.NET Core 2.x project templates implicitly sets `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span>
    
> [!IMPORTANT]
> <span data-ttu-id="ca390-123">Razor görünüm ön derlemesi kullanılamıyor gerektiğinde gerçekleştirme bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="ca390-123">Razor view precompilation in not available when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> 

<span data-ttu-id="ca390-124">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) ile [.NET Core CLI yayımlama komutu](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="ca390-124">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="ca390-125">Örneğin, proje kökündeki aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="ca390-125">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="ca390-126">A *< project_name >. PrecompiledViews.dll* derlenmiş Razor görünümleri içeren dosyası ön derlemesi başarılı olduğunda oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="ca390-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="ca390-127">Örneğin, aşağıdaki ekran görüntüsünde içeriğini gösterir *Index.cshtml* içine *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="ca390-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ca390-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ca390-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ca390-130">Ayarlama `MvcRazorCompileOnPublish` için `true` ve bir paket başvuru eklemek `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="ca390-130">Set `MvcRazorCompileOnPublish` to `true` and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="ca390-131">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="ca390-131">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

