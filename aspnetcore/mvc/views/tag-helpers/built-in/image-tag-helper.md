---
title: ASP.NET Core resim etiketi Yardımcısı
author: pkellner
description: Resim etiketi Yardımcısı ile çalışmayı gösterir.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 964072ad276f7e3e411ee41cb03a2efb9d05c585
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663998"
---
# <a name="image-tag-helper-in-aspnet-core"></a>ASP.NET Core resim etiketi Yardımcısı

By [Peter Kellner](https://peterkellner.net)

Resim etiketi Yardımcısı, statik görüntü dosyaları için önbellek-busting davranışı sağlamak üzere `<img>` etiketini geliştirir.

Önbellek-busting dizesi, varlığın URL 'sine eklenen statik görüntü dosyasının karmasını temsil eden benzersiz bir değerdir. Benzersiz dize, istemcilerin (ve bazı proxy 'lerde), görüntüyü istemci önbelleğinden değil, ana bilgisayar Web sunucusundan yeniden yüklemesi için istemde bulunur.

Görüntü kaynağı (`src`), ana bilgisayar Web sunucusunda bir statik dosya ise:

* Benzersiz bir önbellek-busting dizesi, resim kaynağına bir sorgu parametresi olarak eklenir.
* Ana bilgisayar Web sunucusundaki dosya değişirse, güncelleştirilmiş istek parametresini içeren benzersiz bir istek URL 'SI oluşturulur.

Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

## <a name="image-tag-helper-attributes"></a>Resim etiketi Yardımcısı öznitelikleri

### <a name="src"></a>YN

Resim etiketi yardımcısını etkinleştirmek için, `<img>` öğesinde `src` özniteliği gereklidir.

Görüntü kaynağı (`src`), sunucudaki bir fiziksel statik dosyayı göstermelidir. `src` uzak bir URI ise, önbellek-busting sorgu dizesi parametresi oluşturulmaz.

### <a name="asp-append-version"></a>ASP-Append-sürüm

`asp-append-version`, bir `src` özniteliğiyle birlikte bir `true` değeriyle belirtildiğinde, resim etiketi Yardımcısı çağrılır.

Aşağıdaki örnek bir resim etiketi yardımcısını kullanır:

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

Statik dosya */Wwwroot/images/* dizininde varsa, oluşturulan HTML şuna benzerdir (karma farklı olur):

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

`v` parametreye atanan değer, diskteki *asplogo. png* dosyasının karma değeridir. Web sunucusu statik dosyaya okuma erişimi alamadığında, işlenmiş İşaretlemede `src` özniteliğine `v` parametresi eklenmez.

## <a name="hash-caching-behavior"></a>Karma önbelleğe alma davranışı

Resim etiketi Yardımcısı, belirli bir dosyanın hesaplanmış `Sha512` karmasını depolamak için yerel Web sunucusundaki önbellek sağlayıcısını kullanır. Dosya birden çok kez isteniyorsa, karma yeniden hesaplanmadı. Önbellek, dosyanın `Sha512` karması hesaplandığında dosyaya iliştirilmiş bir dosya İzleyicisi tarafından geçersiz kılınır. Dosya diskte değiştiğinde yeni bir karma hesaplanır ve önbelleğe alınır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
