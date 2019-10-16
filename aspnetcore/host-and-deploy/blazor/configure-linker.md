---
title: ASP.NET Core Blazor için bağlayıcı yapılandırma
author: guardrex
description: Blazor uygulaması oluştururken ara dil (IL) bağlayıcı denetimini nasıl denetleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: a7e59e63c163986c40155e230dc644028e78e5fd
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391458"
---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="dc1b7-103">ASP.NET Core Blazor için bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="dc1b7-103">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="dc1b7-104">[Luke Latham](https://github.com/guardrex) tarafından</span><span class="sxs-lookup"><span data-stu-id="dc1b7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="dc1b7-105">Blazor, uygulamanın çıkış derlemelerinden gereksiz Il 'yi kaldırmak için bir yayın derlemesi sırasında [ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) bağlamayı gerçekleştirir.</span><span class="sxs-lookup"><span data-stu-id="dc1b7-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a Release build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="dc1b7-106">Aşağıdaki yaklaşımlardan birini kullanarak derleme bağlamayı kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="dc1b7-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="dc1b7-107">[MSBuild özelliği](#disable-linking-with-a-msbuild-property)ile genel olarak bağlamayı devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="dc1b7-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="dc1b7-108">[Yapılandırma dosyası](#control-linking-with-a-configuration-file)ile derleme temelinde bağlama denetimi.</span><span class="sxs-lookup"><span data-stu-id="dc1b7-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="dc1b7-109">MSBuild özelliği ile bağlamayı devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="dc1b7-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="dc1b7-110">Dağıtım, yayınlama de dahil olmak üzere derleme modunda varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="dc1b7-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="dc1b7-111">Tüm derlemeler için bağlamayı devre dışı bırakmak için `BlazorLinkOnBuild` MSBuild özelliğini proje dosyasında `false` olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="dc1b7-111">To disable linking for all assemblies, set the `BlazorLinkOnBuild` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="dc1b7-112">Yapılandırma dosyası ile bağlamayı denetleme</span><span class="sxs-lookup"><span data-stu-id="dc1b7-112">Control linking with a configuration file</span></span>

<span data-ttu-id="dc1b7-113">Bir XML yapılandırma dosyası sağlayarak ve dosyayı proje dosyasında MSBuild öğesi olarak belirterek, derleme başına temelinde bağlamayı denetleyin:</span><span class="sxs-lookup"><span data-stu-id="dc1b7-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="dc1b7-114">*Bağlayıcı. xml*:</span><span class="sxs-lookup"><span data-stu-id="dc1b7-114">*Linker.xml*:</span></span>

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
      Fixes: https://github.com/aspnet/Blazor/issues/239
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

<span data-ttu-id="dc1b7-115">Daha fazla bilgi için bkz. [Il Bağlayıcısı: XML tanımlayıcısının sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="dc1b7-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
