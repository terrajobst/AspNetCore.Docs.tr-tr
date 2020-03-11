---
title: ASP.NET Core 'de ortam etiketi Yardımcısı
author: pkellner
description: Tüm özellikler dahil ASP.NET Core ortam etiketi Yardımcısı tanımlandı
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 308e7db47104ebd4d6bb8d08c64f14bbd118898b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663991"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET Core 'de ortam etiketi Yardımcısı

, [Peter Kellner](https://peterkellner.net) ve [Hisham bin ateya](https://twitter.com/hishambinateya) tarafından

Ortam etiketi Yardımcısı, Şirket içindeki içeriğini koşullu olarak geçerli [barındırma ortamına](xref:fundamentals/environments)göre oluşturur. Ortam etiketi Yardımcısı 'nın tek özniteliği `names`, ortam adlarının virgülle ayrılmış listesidir. Belirtilen ortam adlarından herhangi biri geçerli ortamla eşleşiyorsa, eklenen içerik işlenir.

Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

## <a name="environment-tag-helper-attributes"></a>Ortam etiketi yardımcı öznitelikleri

### <a name="names"></a>adlar

`names`, tek bir barındırma ortamı adını veya ekteki içeriğin işlenmesini tetikleyen barındırma ortamı adlarının virgülle ayrılmış bir listesini kabul eder.

Ortam değerleri [ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)tarafından döndürülen geçerli değerle karşılaştırılır. Karşılaştırma büyük/küçük harf durumunu yoksayar.

Aşağıdaki örnek bir ortam etiketi Yardımcısı kullanır. İçerik, barındırma ortamı hazırlama veya üretim ise işlenir:

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a>öznitelikleri dahil etme ve hariç tutma

`include` & `exclude` öznitelik denetimi eklenen veya dışlanan barındırma ortamı adlarını temel alarak çevrelenmiş içeriği işleme.

### <a name="include"></a>include

`include` özelliği, `names` özniteliğine benzer davranışlar sergiler. `include` öznitelik değerinde listelenen bir ortam, `<environment>` etiketinin içeriğini işlemek için uygulamanın barındırma ortamıyla ([ıhostingenvironment. EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) eşleşmelidir.

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a>exclude

`include` özniteliğinin aksine, `<environment>` etiketinin içeriği, barındırma ortamı `exclude` öznitelik değerinde listelenen bir ortamla eşleşmediği zaman işlenir.

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/environments>
