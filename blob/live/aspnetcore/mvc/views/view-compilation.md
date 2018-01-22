---
title: "Razor görünüm derleme ve ön derlemesi"
author: rick-anderson
description: "MVC Razor görünüm derleme ve ASP.NET Core uygulamalarında ön derlemesi etkinleştirme açıklayan bir başvuru belgesi."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="44042-103">Razor görünüm derleme ve ASP.NET Core içinde ön derlemesi</span><span class="sxs-lookup"><span data-stu-id="44042-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="44042-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44042-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44042-105">Görünüm çağrıldığında razor görünümleri çalışma zamanında derlenir.</span><span class="sxs-lookup"><span data-stu-id="44042-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="44042-106">ASP.NET Core 1.1.0 ve daha yüksek isteğe bağlı olarak Razor görünümleri derlemek ve uygulamayla dağıtmadan&mdash;ön derlemesi da bilinen bir işlem.</span><span class="sxs-lookup"><span data-stu-id="44042-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="44042-107">ASP.NET Core 2.x proje şablonları ön derlemesi varsayılan olarak etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="44042-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="44042-108">Razor görünüm ön derlemesi şu anda kullanılamıyor gerçekleştirirken bir [müstakil dağıtım (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="44042-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="44042-109">2.1 bıraktığında özellik SCDs için kullanılabilir olacak.</span><span class="sxs-lookup"><span data-stu-id="44042-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="44042-110">Daha fazla bilgi için bkz: [görünüm derleme başarısız cross-Windows Linux için derlerken](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="44042-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="44042-111">Ön derleme dikkate alınacak noktalar:</span><span class="sxs-lookup"><span data-stu-id="44042-111">Precompilation considerations:</span></span>

* <span data-ttu-id="44042-112">Görünümleri önceden derleme, bir küçük yayımlanan paket ve daha hızlı başlangıç saati ile sonuçlanır.</span><span class="sxs-lookup"><span data-stu-id="44042-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="44042-113">Görünümleri ön derleme yap sonra Razor dosyaları düzenleyemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="44042-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="44042-114">Düzenlenen görünümleri yayımlanan pakete mevcut olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="44042-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="44042-115">Önceden derlenmiş görünümleri dağıtmak için:</span><span class="sxs-lookup"><span data-stu-id="44042-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="44042-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="44042-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="44042-117">Projeniz .NET Framework hedefliyorsa, bir paket başvuru eklemek [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="44042-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="44042-118">Projeniz .NET Core hedefliyorsa, hiçbir değişiklik gereklidir.</span><span class="sxs-lookup"><span data-stu-id="44042-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="44042-119">ASP.NET Core 2.x proje şablonları örtük olarak ayarlamak `MvcRazorCompileOnPublish` için `true` varsayılan olarak, yani bu düğüm güvenle kaldırılabilir gelen *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="44042-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="44042-120">Açık olmasını isterseniz, ayarı hiçbir zarar yoktur `MvcRazorCompileOnPublish` özelliğine `true`.</span><span class="sxs-lookup"><span data-stu-id="44042-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="44042-121">Aşağıdaki *.csproj* örnek bu ayar vurgular:</span><span class="sxs-lookup"><span data-stu-id="44042-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="44042-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="44042-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="44042-123">Ayarlama `MvcRazorCompileOnPublish` için `true`ve bir paket başvuru eklemek `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="44042-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="44042-124">Aşağıdaki *.csproj* örnek bu ayarları vurgular:</span><span class="sxs-lookup"><span data-stu-id="44042-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="44042-125">Uygulama için hazırlama bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-deployments-fdd) proje kökündeki aşağıdaki gibi bir komutu çalıştırarak:</span><span class="sxs-lookup"><span data-stu-id="44042-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="44042-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span><span class="sxs-lookup"><span data-stu-id="44042-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="44042-127">Örneğin, aşağıdaki ekran görüntüsünde içeriğini gösterir *Index.cshtml* içine *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="44042-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL içindeki Razor görünümleri](view-compilation/_static/razor-views-in-dll.png)
