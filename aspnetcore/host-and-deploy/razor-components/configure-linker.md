---
title: Blazor için bağlayıcı yapılandırma
author: guardrex
description: Ara dil (IL) bağlayıcı Blazor uygulaması derlerken, denetlemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: host-and-deploy/razor-components/configure-linker
ms.openlocfilehash: c3c38ec2509344cc02f3895d5d0c2d35059d1d8e
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668175"
---
# <a name="configure-the-linker-for-blazor"></a><span data-ttu-id="696c4-103">Blazor için bağlayıcı yapılandırma</span><span class="sxs-lookup"><span data-stu-id="696c4-103">Configure the Linker for Blazor</span></span>

<span data-ttu-id="696c4-104">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="696c4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

<span data-ttu-id="696c4-105">Blazor gerçekleştirir [Ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) gereksiz IL çıkış derlemeleri kaldırmak için her sürüm modu yapı sırasında bağlama.</span><span class="sxs-lookup"><span data-stu-id="696c4-105">Blazor performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during each Release mode build to remove unnecessary IL from the output assemblies.</span></span>

<span data-ttu-id="696c4-106">Derleme bağlama aşağıdaki yaklaşımlardan birini kullanarak denetleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="696c4-106">You can control assembly linking with either of the following approaches:</span></span>

* <span data-ttu-id="696c4-107">Bir MSBuild özelliği ile küresel olarak bağlama devre dışı bırakın.</span><span class="sxs-lookup"><span data-stu-id="696c4-107">Disable linking globally with an MSBuild property.</span></span>
* <span data-ttu-id="696c4-108">Derleme başına temelinde bir yapılandırma dosyası bağlama denetimi.</span><span class="sxs-lookup"><span data-stu-id="696c4-108">Control linking on a per-assembly basis with a configuration file.</span></span>

## <a name="disable-linking-with-an-msbuild-property"></a><span data-ttu-id="696c4-109">Bir MSBuild özelliği ile bağlama devre dışı bırak</span><span class="sxs-lookup"><span data-stu-id="696c4-109">Disable linking with an MSBuild property</span></span>

<span data-ttu-id="696c4-110">Bir uygulama, yayımlama içeren oluşturulduğunda bağlama yayın modunda varsayılan olarak etkindir.</span><span class="sxs-lookup"><span data-stu-id="696c4-110">Linking is enabled by default in Release mode when an app is built, which includes publishing.</span></span> <span data-ttu-id="696c4-111">Tüm derlemeler için bağlama devre dışı bırakmak için ayarlanmış `<BlazorLinkOnBuild>` MSBuild özelliğini `false` proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="696c4-111">To disable linking for all assemblies, set the `<BlazorLinkOnBuild>` MSBuild property to `false` in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="696c4-112">Bir yapılandırma dosyası bağlama denetimi</span><span class="sxs-lookup"><span data-stu-id="696c4-112">Control linking with a configuration file</span></span>

<span data-ttu-id="696c4-113">Bağlama derlemesi başına temelinde bir XML yapılandırma dosyasını sağlayarak ve bir MSBuild öğesi olarak proje dosyasının dosya belirtme denetlenebilir.</span><span class="sxs-lookup"><span data-stu-id="696c4-113">Linking can be controlled on a per-assembly basis by providing an XML configuration file and specifying the file as an MSBuild item in the project file.</span></span>

<span data-ttu-id="696c4-114">Örnek bir yapılandırma dosyası verilmiştir (*Linker.xml*):</span><span class="sxs-lookup"><span data-stu-id="696c4-114">The following is an example configuration file (*Linker.xml*):</span></span>

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

<span data-ttu-id="696c4-115">Yapılandırma dosyası için dosya biçimi hakkında daha fazla bilgi için bkz: [IL bağlayıcı: Xml tanımlayıcı sözdizimi](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span><span class="sxs-lookup"><span data-stu-id="696c4-115">To learn more about the file format for the configuration file, see [IL Linker: Syntax of xml descriptor](https://github.com/mono/linker/blob/master/linker/README.md#syntax-of-xml-descriptor).</span></span>

<span data-ttu-id="696c4-116">Yapılandırma dosyasını proje dosyasında belirtmek `BlazorLinkerDescriptor` öğesi:</span><span class="sxs-lookup"><span data-stu-id="696c4-116">Specify the configuration file in the project file with the `BlazorLinkerDescriptor` item:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```
