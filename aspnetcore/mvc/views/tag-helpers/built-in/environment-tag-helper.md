---
title: ASP.NET Core ortam etiketi yok
author: pkellner
description: Tüm özellikleri dahil olmak üzere tanımlanan ASP.NET Core ortam etiketi Yardımcısı
ms.author: riande
ms.date: 07/14/2017
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 05c07b06a4fedac0b0ff39d168807f5e2e6996cf
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276922"
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
