---
title: ASP.NET Core ortam etiketi yok
author: pkellner
description: "Tüm özellikleri dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 07/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 2639e4d7494e752462a1a2cb0648042a2d2d06ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="environment-tag-helper-in-aspnet-core"></a>ASP.NET Core ortam etiketi yok

Tarafından [Peter Kellner](http://peterkellner.net) ve [Hisham Bin Ateya](https://twitter.com/hishambinateya)

Ortam etiketi yardımcı koşullu geçerli barındırma ortamına bağlı kapalı içeriği işler. Tek öznitelik `names` ortamı virgülle ayrılmış listesi olduğu ad, herhangi bir geçerli ortama eşleşen, işlenmek üzere kapalı içerik tetikler.

## <a name="environment-tag-helper-attributes"></a>Ortam etiketi yardımcı öznitelik

### <a name="names"></a>adlar

Tek bir barındırma ortamı ad veya kapalı içerik çizmeye tetiklemek ortam adları barındırma virgülle ayrılmış listesini kabul eder.

Bu değer ASP.NET Core statik özelliğinden döndürülen geçerli değer karşılaştırılır `HostingEnvironment.EnvironmentName`.  Bu değer şunlardan biri değil: **hazırlama**; **Geliştirme** veya **üretim**. Karşılaştırma durumu yok sayar.

Geçerli bir örneği `environment` etiketi yardımcı:

```cshtml
<environment names="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="include-and-exclude-attributes"></a>eklemek veya çıkartmak öznitelikleri

ASP.NET Core 2.x ekler `include`  &  `exclude` öznitelikleri. Kapsanan veya dışlanan barındırma ortamı adlarına göre kapalı içeriğini işlemek bu öznitelikler denetim.

### <a name="include-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 ve üzeri dahil

`include` Özelliğine benzer bir davranışını `names` ASP.NET Core 1.0 özniteliği.

```cshtml
<environment include="Staging,Production">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude-aspnet-core-20-and-later"></a>ASP.NET Core 2.0 ve üzeri Dışla

Buna karşılık, `exclude` özelliği sağlar `EnvironmentTagHelper` belirttiğiniz one(s) dışındaki tüm barındırma ortamı adları için kapalı içerik oluşturması.

```cshtml
<environment exclude="Development">
  <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/environments>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
