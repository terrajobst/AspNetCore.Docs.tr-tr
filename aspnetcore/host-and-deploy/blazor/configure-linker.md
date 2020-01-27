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
# <a name="configure-the-linker-for-aspnet-core-opno-locblazor"></a>ASP.NET Core Blazor için bağlayıcı yapılandırma

Tarafından [Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

Blazor, uygulamanın çıkış derlemelerinden gereksiz Il 'yi kaldırmak için bir derleme sırasında [ara dil (IL)](/dotnet/standard/managed-code#intermediate-language--execution) bağlamayı gerçekleştirir.

Aşağıdaki yaklaşımlardan birini kullanarak derleme bağlamayı kontrol edin:

* [MSBuild özelliği](#disable-linking-with-a-msbuild-property)ile genel olarak bağlamayı devre dışı bırakın.
* [Yapılandırma dosyası](#control-linking-with-a-configuration-file)ile derleme temelinde bağlama denetimi.

## <a name="disable-linking-with-a-msbuild-property"></a>MSBuild özelliği ile bağlamayı devre dışı bırak

Bir uygulama oluşturulduğunda, yayımlama de dahil olmak üzere varsayılan olarak bağlama etkindir. Tüm derlemeler için bağlamayı devre dışı bırakmak için `BlazorLinkOnBuild` MSBuild özelliğini proje dosyasında `false` olarak ayarlayın:

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

Daha fazla bilgi için bkz. [Il Bağlayıcısı: XML tanımlayıcısının sözdizimi](https://github.com/mono/linker/blob/master/src/linker/README.md#syntax-of-xml-descriptor).

### <a name="configure-the-linker-for-internationalization"></a>Bağlayıcıyı uluslararası duruma getirme için yapılandırma

Varsayılan olarak, Blazor WebAssembly uygulamaları için Blazorbağlayıcı yapılandırması, açıkça istenen yerel ayarlar dışında uluslararası duruma getirme bilgilerini kaldırır. Bu derlemelerin kaldırılması uygulamanın boyutunu en aza indirir.

Hangi I18N derlemelerinin korunacağını denetlemek için, proje dosyasında `<MonoLinkerI18NAssemblies>` MSBuild özelliğini ayarlayın:

```xml
<PropertyGroup>
  <MonoLinkerI18NAssemblies>{all|none|REGION1,REGION2,...}</MonoLinkerI18NAssemblies>
</PropertyGroup>
```

| Bölge değeri     | Mono bölgesi derlemesi    |
| ---------------- | ----------------------- |
| `all`            | Tüm derlemeler dahil |
| `cjk`            | *I18N. CJK. dll*          |
| `mideast`        | *I18N. MIDEAST. dll*      |
| `none` (varsayılan) | Yok.                    |
| `other`          | *I18N. Diğer. dll*        |
| `rare`           | *I18N. Nadir. dll*         |
| `west`           | *I18N. Batı. dll*         |

Birden çok değeri ayırmak için virgül kullanın (örneğin, `mideast,west`).

Daha fazla bilgi için bkz. [I18N: Pnetlib uluslararası duruma getirme çerçeve kitaplığı (Mono/Mono GitHub deposu)](https://github.com/mono/mono/tree/master/mcs/class/I18N).
