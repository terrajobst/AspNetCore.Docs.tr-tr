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
# <a name="configure-the-linker-for-aspnet-core-blazor"></a>ASP.NET Core Blazor için bağlayıcı yapılandırma

[Luke Latham](https://github.com/guardrex) tarafından

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, uygulamanın çıkış derlemelerinden gereksiz Il 'yi kaldırmak için bir yayın derlemesi sırasında [ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) bağlamayı gerçekleştirir.

Aşağıdaki yaklaşımlardan birini kullanarak derleme bağlamayı kontrol edin:

* [MSBuild özelliği](#disable-linking-with-a-msbuild-property)ile genel olarak bağlamayı devre dışı bırakın.
* [Yapılandırma dosyası](#control-linking-with-a-configuration-file)ile derleme temelinde bağlama denetimi.

## <a name="disable-linking-with-a-msbuild-property"></a>MSBuild özelliği ile bağlamayı devre dışı bırak

Dağıtım, yayınlama de dahil olmak üzere derleme modunda varsayılan olarak etkindir. Tüm derlemeler için bağlamayı devre dışı bırakmak için `BlazorLinkOnBuild` MSBuild özelliğini proje dosyasında `false` olarak ayarlayın:

```xml
<PropertyGroup>
  <BlazorLinkOnBuild>false</BlazorLinkOnBuild>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a>Yapılandırma dosyası ile bağlamayı denetleme

Bir XML yapılandırma dosyası sağlayarak ve dosyayı proje dosyasında MSBuild öğesi olarak belirterek, derleme başına temelinde bağlamayı denetleyin:

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="Linker.xml" />
</ItemGroup>
```

*Bağlayıcı. xml*:

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

Daha fazla bilgi için bkz. [Il Bağlayıcısı: XML tanımlayıcısının sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).
