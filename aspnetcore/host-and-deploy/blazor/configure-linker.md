---
title: ASP.NET Core Blazor için bağlayıcı yapılandırma
author: guardrex
description: Blazor uygulaması oluştururken ara dil (IL) bağlayıcı denetimini nasıl denetleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 263b85a3213c1da233e4c96095faaf39d0a8e13f
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726767"
---
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a><span data-ttu-id="0588e-103">ASP.NET Core [!OP.NO-LOC(Blazor)] için bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0588e-103">Configure the Linker for ASP.NET Core [!OP.NO-LOC(Blazor)]</span></span>

<span data-ttu-id="0588e-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="0588e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!OP.NO-LOC(Blazor)]<span data-ttu-id="0588e-105">, uygulamanın çıkış derlemelerinden gereksiz Il 'yi kaldırmak için bir derleme sırasında [ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) bağlamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="0588e-105"> performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="0588e-106">Aşağıdaki yaklaşımlardan birini kullanarak derleme bağlamayı kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="0588e-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="0588e-107">[MSBuild özelliği](#disable-linking-with-a-msbuild-property)ile genel olarak bağlamayı devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="0588e-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="0588e-108">[Yapılandırma dosyası](#control-linking-with-a-configuration-file)ile derleme temelinde bağlama denetimi.</span><span class="sxs-lookup"><span data-stu-id="0588e-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="0588e-109">MSBuild özelliği ile bağlamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="0588e-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="0588e-110">Bir uygulama oluşturulduğunda, yayımlama de dahil olmak üzere varsayılan olarak bağlama etkindir.</span><span class="sxs-lookup"><span data-stu-id="0588e-110">Linking is enabled by default when an app is built, which includes publishing.</span></span> <span data-ttu-id="0588e-111">Tüm derlemeler için bağlamayı devre dışı bırakmak için `BlazorLinkOnBuild` MSBuild özelliğini proje dosyasında `false` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="0588e-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="0588e-112">Yapılandırma dosyası ile bağlamayı denetleme</span><span class="sxs-lookup"><span data-stu-id="0588e-112">Control linking with a configuration file</span></span>

<span data-ttu-id="0588e-113">Bir XML yapılandırma dosyası sağlayarak ve dosyayı proje dosyasında MSBuild öğesi olarak belirterek, derleme başına temelinde bağlamayı denetleyin:</span><span class="sxs-lookup"><span data-stu-id="0588e-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="0588e-114">*Bağlayıcı. xml*:</span><span class="sxs-lookup"><span data-stu-id="0588e-114">*Linker.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or [!OP.NO-LOC(Blazor)] packages must not be
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

<span data-ttu-id="0588e-115">Daha fazla bilgi için bkz. [Il Bağlayıcısı: XML tanımlayıcısının sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="0588e-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="0588e-116">Bağlayıcıyı uluslararası duruma getirme için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0588e-116">Configure the linker for internationalization</span></span>

<span data-ttu-id="0588e-117">Varsayılan olarak, [!OP.NO-LOC(Blazor)] WebAssembly uygulamaları için [!OP.NO-LOC(Blazor)]bağlayıcı yapılandırması, açıkça istenen yerel ayarlar dışında uluslararası duruma getirme bilgilerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="0588e-117">By default, [!OP.NO-LOC(Blazor)]'s linker configuration for [!OP.NO-LOC(Blazor)] WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="0588e-118">Bu derlemelerin kaldırılması uygulamanın boyutunu en aza indirir.</span><span class="sxs-lookup"><span data-stu-id="0588e-118">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="0588e-119">Hangi I18N derlemelerinin korunacağını denetlemek için, proje dosyasında `<MonoLinkerI18NAssemblies>` MSBuild özelliğini ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="0588e-119">To control which I18N assemblies are retained, set the `<MonoLinkerI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="0588e-120">Bölge değeri</span><span class="sxs-lookup"><span data-stu-id="0588e-120">Region Value</span></span>     | <span data-ttu-id="0588e-121">Mono bölgesi derlemesi</span><span class="sxs-lookup"><span data-stu-id="0588e-121">Mono region assembly</span></span>    |
| ---------------- | ----------------------- |
| `all`            | <span data-ttu-id="0588e-122">Tüm derlemeler dahil</span><span class="sxs-lookup"><span data-stu-id="0588e-122">All assemblies included</span></span> |
| `cjk`            | <span data-ttu-id="0588e-123">*I18N. CJK. dll*</span><span class="sxs-lookup"><span data-stu-id="0588e-123">*I18N.CJK.dll*</span></span>          |
| `mideast`        | <span data-ttu-id="0588e-124">*I18N. MIDEAST. dll*</span><span class="sxs-lookup"><span data-stu-id="0588e-124">*I18N.MidEast.dll*</span></span>      |
| <span data-ttu-id="0588e-125">`none` (varsayılan)</span><span class="sxs-lookup"><span data-stu-id="0588e-125">`none` (default)</span></span> | <span data-ttu-id="0588e-126">Yok.</span><span class="sxs-lookup"><span data-stu-id="0588e-126">None</span></span>                    |
| `other`          | <span data-ttu-id="0588e-127">*I18N. Diğer. dll*</span><span class="sxs-lookup"><span data-stu-id="0588e-127">*I18N.Other.dll*</span></span>        |
| `rare`           | <span data-ttu-id="0588e-128">*I18N. Nadir. dll*</span><span class="sxs-lookup"><span data-stu-id="0588e-128">*I18N.Rare.dll*</span></span>         |
| `west`           | <span data-ttu-id="0588e-129">*I18N. Batı. dll*</span><span class="sxs-lookup"><span data-stu-id="0588e-129">*I18N.West.dll*</span></span>         |

<span data-ttu-id="0588e-130">Birden çok değeri ayırmak için virgül kullanın (örneğin, `mideast,west`).</span><span class="sxs-lookup"><span data-stu-id="0588e-130">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="0588e-131">Daha fazla bilgi için bkz. [I18N: Pnetlib uluslararası duruma getirme çerçeve kitaplığı (Mono/Mono GitHub deposu)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span><span class="sxs-lookup"><span data-stu-id="0588e-131">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>
