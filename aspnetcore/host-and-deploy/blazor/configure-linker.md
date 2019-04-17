---
title: Blazor için bağlayıcı yapılandırma
author: guardrex
description: Ara dil (IL) bağlayıcı Blazor uygulaması derlerken, denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/configure-linker
ms.openlocfilehash: 01e18498a16e86392755b02b92ffda929669cb7d
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614840"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="2e61f-103">Blazor için bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2e61f-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="2e61f-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2e61f-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="2e61f-105">Blazor gerçekleştirir [Ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) uygulamadan gereksiz IL kaldırmak için her sürüm modu yapı sırasında bağlama derlemeleri çıkış.</span><span class="sxs-lookup"><span data-stu-id="2e61f-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the app's output assemblies.</span></span>

<span data-ttu-id="2e61f-106">Denetim derleme: aşağıdaki yaklaşımlardan birini kullanarak bağlama</span><span class="sxs-lookup"><span data-stu-id="2e61f-106">Control assembly linking using either of the following approaches:</span></span>

* <span data-ttu-id="2e61f-107">İle küresel olarak bağlama devre dışı bir [MSBuild özelliği](#disable-linking-with-a-msbuild-property).</span><span class="sxs-lookup"><span data-stu-id="2e61f-107">Disable linking globally with a [MSBuild property](#disable-linking-with-a-msbuild-property).</span></span>
* <span data-ttu-id="2e61f-108">Denetim ile derleme başına temelinde bağlama bir [yapılandırma dosyası](#control-linking-with-a-configuration-file).</span><span class="sxs-lookup"><span data-stu-id="2e61f-108">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="disable-linking-with-a-msbuild-property"></a><span data-ttu-id="2e61f-109">Bir MSBuild özellik bağlama devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="2e61f-109">Disable linking with a MSBuild property</span></span>

<span data-ttu-id="2e61f-110">Bir uygulama, yayımlama içeren oluşturulduğunda bağlama yayın modunda varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="2e61f-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="2e61f-111">Tüm derlemeler için bağlama devre dışı bırakmak için ayarlanmış `<BlazorLinkOnBuild>` MSBuild özelliğini `false` proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="2e61f-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="2e61f-112">Bir yapılandırma dosyası bağlama denetimi</span><span class="sxs-lookup"><span data-stu-id="2e61f-112">Control linking with a configuration file</span></span>

<span data-ttu-id="2e61f-113">Derleme başına temelinde bir XML yapılandırma dosyasını sağlayarak ve bir MSBuild öğesi olarak proje dosyasının dosya belirtme bağlama denetimi:</span><span class="sxs-lookup"><span data-stu-id="2e61f-113">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

<span data-ttu-id="2e61f-114">*Linker.xml*:</span><span class="sxs-lookup"><span data-stu-id="2e61f-114">*Linker.xml*:</span></span>

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

<span data-ttu-id="2e61f-115">Daha fazla bilgi için [IL bağlayıcı: Xml tanımlayıcı sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="2e61f-115">For more information, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).</span></span>
