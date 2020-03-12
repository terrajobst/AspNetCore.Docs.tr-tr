---
title: ASP.NET Core Blazor için bağlayıcı yapılandırma
author: guardrex
description: Blazor uygulaması oluştururken ara dil (IL) bağlayıcı denetimini nasıl denetleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/10/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: b08ec26fb8d139223c57774600bc3cb19a56ac49
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083294"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="677fb-103">ASP.NET Core Blazor için bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="677fb-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="677fb-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="677fb-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="677fb-105">Blazor WebAssembly, uygulamanın çıkış derlemelerinden gereksiz Il 'yi kırpmak için bir derleme sırasında [ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) bağlamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="677fb-105">Blazor WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="677fb-106">Hata ayıklama yapılandırmasında oluşturulurken bağlayıcı devre dışı bırakıldı.</span><span class="sxs-lookup"><span data-stu-id="677fb-106">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="677fb-107">Bağlayıcı etkinleştirmek için uygulamaların yayın yapılandırmasında derlenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="677fb-107">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="677fb-108">Blazor WebAssembly uygulamalarınızı dağıttığınızda yayında derleme yapmanız önerilir.</span><span class="sxs-lookup"><span data-stu-id="677fb-108">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="677fb-109">Uygulama bağlama boyutu için en iyi duruma getirir, ancak bu etkilere sebep olabilir.</span><span class="sxs-lookup"><span data-stu-id="677fb-109">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="677fb-110">Bağlayıcı bu dinamik davranışı öğrenmediği ve çalışma zamanında yansıma için hangi türlerin gerekli olduğunu belirleyemediği için yansıma veya ilgili dinamik özellikleri kullanan uygulamalar kırpılmayabilir.</span><span class="sxs-lookup"><span data-stu-id="677fb-110">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="677fb-111">Bu tür uygulamaları kırpmak için bağlayıcı, koddaki yansıma tarafından gerek duyulan herhangi bir tür ve uygulamanın bağımlı olduğu paketler veya çerçeveler hakkında bilgilendirmelidir.</span><span class="sxs-lookup"><span data-stu-id="677fb-111">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="677fb-112">Kırpılan uygulamanın dağıtıldıktan sonra düzgün çalıştığından emin olmak için, geliştirme sırasında uygulamanın yayın derlemelerini test etmek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="677fb-112">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="677fb-113">Blazor uygulamaları için bağlama, bu MSBuild özellikleri kullanılarak yapılandırılabilir:</span><span class="sxs-lookup"><span data-stu-id="677fb-113">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="677fb-114">Bir [MSBuild özelliği](#control-linking-with-an-msbuild-property)ile genel olarak bağlamayı yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="677fb-114">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="677fb-115">[Yapılandırma dosyası](#control-linking-with-a-configuration-file)ile derleme temelinde bağlama denetimi.</span><span class="sxs-lookup"><span data-stu-id="677fb-115">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="677fb-116">MSBuild özelliği ile bağlamayı denetleme</span><span class="sxs-lookup"><span data-stu-id="677fb-116">Control linking with an MSBuild property</span></span>

<span data-ttu-id="677fb-117">Bir uygulama `Release` yapılandırma sırasında oluşturulduğunda bağlama etkinleştirilir.</span><span class="sxs-lookup"><span data-stu-id="677fb-117">Linking is enabled when an app is built in `Release` configuation.</span></span> <span data-ttu-id="677fb-118">Bunu değiştirmek için, proje dosyasında `BlazorWebAssemblyEnableLinking` MSBuild özelliğini yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="677fb-118">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="677fb-119">Yapılandırma dosyası ile bağlamayı denetleme</span><span class="sxs-lookup"><span data-stu-id="677fb-119">Control linking with a configuration file</span></span>

<span data-ttu-id="677fb-120">Bir XML yapılandırma dosyası sağlayarak ve dosyayı proje dosyasında MSBuild öğesi olarak belirterek, derleme başına temelinde bağlamayı denetleyin:</span><span class="sxs-lookup"><span data-stu-id="677fb-120">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="677fb-121">*Bağlayıcı. xml*:</span><span class="sxs-lookup"><span data-stu-id="677fb-121">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="677fb-122">Daha fazla bilgi için bkz. [Il Bağlayıcısı: XML tanımlayıcısının sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="677fb-122">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="677fb-123">Bağlayıcıyı uluslararası duruma getirme için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="677fb-123">Configure the linker for internationalization</span></span>

<span data-ttu-id="677fb-124">Varsayılan olarak, Blazor 'in Blazor WebAssembly uygulamaları için bağlayıcı yapılandırması, açıkça istenen yerel ayarlar dışında uluslararası duruma getirme bilgilerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="677fb-124">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="677fb-125">Bu derlemelerin kaldırılması uygulamanın boyutunu en aza indirir.</span><span class="sxs-lookup"><span data-stu-id="677fb-125">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="677fb-126">Hangi I18N derlemelerinin korunacağını denetlemek için, proje dosyasında `<MonoLinkerI18NAssemblies>` MSBuild özelliğini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="677fb-126">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="677fb-127">Bölge değeri</span><span class="sxs-lookup"><span data-stu-id="677fb-127">Region Value</span></span>     | <span data-ttu-id="677fb-128">Mono bölgesi derlemesi</span><span class="sxs-lookup"><span data-stu-id="677fb-128">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="677fb-129">Tüm derlemeler dahil</span><span class="sxs-lookup"><span data-stu-id="677fb-129">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="677fb-130">*I18N. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="677fb-130">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="677fb-131">*I18N. MIDEAST. dll*</span><span class="sxs-lookup"><span data-stu-id="677fb-131">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="677fb-132">`none` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="677fb-132">`none` (default)</span></span> | <span data-ttu-id="677fb-133">Yok.</span><span class="sxs-lookup"><span data-stu-id="677fb-133">None</span></span>                    |
| `other`          | <span data-ttu-id="677fb-134">*I18N. Diğer. dll*</span><span class="sxs-lookup"><span data-stu-id="677fb-134">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="677fb-135">*I18N. Nadir. dll*</span><span class="sxs-lookup"><span data-stu-id="677fb-135">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="677fb-136">*I18N. Batı. dll*</span><span class="sxs-lookup"><span data-stu-id="677fb-136">*I18N.West.dll*</span></span>         |

<span data-ttu-id="677fb-137">Birden çok değeri ayırmak için virgül kullanın (örneğin, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="677fb-137">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="677fb-138">Daha fazla bilgi için bkz. [I18N: Pnetlib uluslararası duruma getirme çerçeve kitaplığı (Mono/Mono GitHub deposu)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="677fb-138">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
