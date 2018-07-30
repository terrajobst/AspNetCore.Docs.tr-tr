---
title: ASP.NET core'da ortam etiketi Yardımcısı
author: pkellner
description: Tüm özellikler dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 4a283a3a03aa6cac228ec6effd02e3f1095be260
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342230"
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET core'da ortam etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)

Ortam etiketi Yardımcısı koşullu olarak geçerli barındırma ortamına bağlı kapalı içeriğini işler. Bunun tek öznitelik `names` ortam virgülle ayrılmış listesidir ad, tüm geçerli ortama eşleşen, işlenecek ekteki içeriğin tetikler.

## <a name="environment-tag-helper-attributes"></a>Ortam etiketi Yardımcısı öznitelikleri

### <a name="names"></a>adlar

Tek bir barındırma ortamı adı veya işleme ekteki içeriğin tetikleyen ortam adları barındırma virgülle ayrılmış listesini kabul eder.

Bu değerler ASP.NET Core statik özelliğinden döndürülen değerle karşılaştırılır `HostingEnvironment.EnvironmentName`.  Bu değer aşağıdakilerden biridir: **hazırlama**; **Geliştirme** veya **üretim**. Karşılaştırma çalışması yoksayar.

Geçerli bir örneği `environment` etiket Yardımcısı:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>dahil etme ve dışlama öznitelikleri

ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri. Bu öznitelikler dahil veya hariç tutulan barındırma ortamı adlarını temel alarak ekteki içeriğin oluşturma denetimi.

### <a name="include-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 ve üzeri dahil

`include` Özelliğine sahiptir, benzer bir davranış `names` ASP.NET Core 1.0 özniteliği.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 ve sonraki sürümleri hariç tut

Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için ekteki içeriğin işleme.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/environments>
