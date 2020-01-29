---
title: ASP.NET Core Web SDK 'Sı
author: Rick-Anderson
description: Microsoft. NET. SDK. Web 'e genel bakış.
ms.author: riande
ms.date: 01/25/2020
no-loc:
- Blazor
uid: razor-pages/web-sdk
ms.openlocfilehash: 49e21e1a432149409a01550452cedf4009dcfba7
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76830676"
---
# <a name="aspnet-core-web-sdk"></a><span data-ttu-id="96569-103">ASP.NET Core Web SDK 'Sı</span><span class="sxs-lookup"><span data-stu-id="96569-103">ASP.NET Core Web SDK</span></span>

### <a name="overview"></a><span data-ttu-id="96569-104">Genel bakış</span><span class="sxs-lookup"><span data-stu-id="96569-104">Overview</span></span>

<span data-ttu-id="96569-105">`Microsoft.NET.Sdk.Web`, ASP.NET Core uygulamalar oluşturmaya yönelik bir [MSBuild proje SDK 'sına](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) sahiptir.</span><span class="sxs-lookup"><span data-stu-id="96569-105">`Microsoft.NET.Sdk.Web` is a [MSBuild project SDK](https://docs.microsoft.com/visualstudio/msbuild/how-to-use-project-sdk) for building ASP.NET Core apps.</span></span> <span data-ttu-id="96569-106">Bu SDK olmadan bir ASP.NET Core uygulaması oluşturmak mümkündür, ancak Web SDK 'Sı şu şekilde olur:</span><span class="sxs-lookup"><span data-stu-id="96569-106">It's possible to build an ASP.NET Core app without this SDK, however, the Web SDK is:</span></span>

* <span data-ttu-id="96569-107">Birinci sınıf bir deneyim sağlamaya yönelik olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="96569-107">Tailored towards providing a first-class experience.</span></span>
* <span data-ttu-id="96569-108">Çoğu kullanıcı için önerilen hedef.</span><span class="sxs-lookup"><span data-stu-id="96569-108">The recommended target for most users.</span></span>

<span data-ttu-id="96569-109">Bir projede Web. SDK 'Yı kullanın:</span><span class="sxs-lookup"><span data-stu-id="96569-109">Use the Web.SDK in a project:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
  </Project>
  ```

<span data-ttu-id="96569-110">Web SDK kullanılarak etkinleştirilen özellikler:</span><span class="sxs-lookup"><span data-stu-id="96569-110">Features enabled by using the Web SDK:</span></span>

* <span data-ttu-id="96569-111">.NET Core 3,0 veya sonraki bir sürümü hedefleyen projeler:</span><span class="sxs-lookup"><span data-stu-id="96569-111">Projects targeting .NET Core 3.0 or later implicitly reference:</span></span>

  * <span data-ttu-id="96569-112">[ASP.NET Core paylaşılan çerçeve](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="96569-112">The [ASP.NET Core shared framework](xref:fundamentals/metapackage-app).</span></span>
  * <span data-ttu-id="96569-113">ASP.NET Core uygulamalar oluşturmak için tasarlanan [çözümleyiciler](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) .</span><span class="sxs-lookup"><span data-stu-id="96569-113">[Analyzers](/visualstudio/extensibility/getting-started-with-roslyn-analyzers) designed for building ASP.NET Core apps.</span></span>
* <span data-ttu-id="96569-114">WebSDK, yayımlama profillerinin kullanımını ve WebDeploy kullanılarak yayımlamayı sağlayan MSBuild hedeflerini mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="96569-114">The WebSDK enables MSBuild targets that enables the use of publish profiles, and publishing using WebDeploy.</span></span>

### <a name="properties"></a><span data-ttu-id="96569-115">Özellikler</span><span class="sxs-lookup"><span data-stu-id="96569-115">Properties</span></span>

| <span data-ttu-id="96569-116">Özellik</span><span class="sxs-lookup"><span data-stu-id="96569-116">Property</span></span> | <span data-ttu-id="96569-117">Açıklama</span><span class="sxs-lookup"><span data-stu-id="96569-117">Description</span></span> |
| -------- | ----------- |
| `DisableImplicitFrameworkReferences` | <span data-ttu-id="96569-118">`Microsoft.AspNetCore.App` paylaşılan çerçeveye örtük başvuruyu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="96569-118">Disables implicit reference to the `Microsoft.AspNetCore.App` shared framework.</span></span> |
| `DisableImplicitAspNetCoreAnalyzers` | <span data-ttu-id="96569-119">ASP.NET Core Çözümleyicileri için örtülü başvuruyu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="96569-119">Disables implicit reference to ASP.NET Core analyzers.</span></span> |
| `DisableImplicitComponentsAnalyzers` | <span data-ttu-id="96569-120">Blazor (sunucu) uygulamaları oluştururken, Razor bileşenleri çözümleyicilerine örtülü başvuruyu devre dışı bırakır.</span><span class="sxs-lookup"><span data-stu-id="96569-120">Disables implicit reference to Razor Components analyzers when building Blazor (server) applications.</span></span> |
