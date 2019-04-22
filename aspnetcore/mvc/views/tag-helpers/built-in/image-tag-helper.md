---
title: ASP.NET core'da görüntü etiketi Yardımcısı
author: pkellner
description: Görüntü etiketi Yardımcısı ile çalışma işlemi gösterilmektedir.
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 916a68c187cbf516a59d3c5d7578cdb6ada01b86
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705516"
---
# <a name="image-tag-helper-in-aspnet-core"></a>ASP.NET core'da görüntü etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net)

Görüntü etiketi Yardımcısı geliştirir `<img>` statik resim dosyaları için önbellek busting davranışı sağlamak için etiket.

Bir önbellek busting dize varlığın URL'ye eklenen statik görüntü dosyasının bir karmasını temsil eden benzersiz bir değerdir. Benzersiz bir dize istemciler (ve bazı Ara sunucular) konak web sunucusu ve istemci önbelleğinden görüntü yeniden yüklemek ister.

Varsa resim kaynağını (`src`) konak web sunucusundaki bir statik dosya:

* Önbellek busting benzersiz bir dize, görüntü kaynağı için bir sorgu parametresi olarak eklenir.
* Konak web sunucusunda dosyayı değişirse, güncelleştirilmiş istek parametresi içeren bir benzersiz istek URL'si oluşturulur.

Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.

## <a name="image-tag-helper-attributes"></a>Görüntü etiketi Yardımcısı öznitelikleri

### <a name="src"></a>src

Görüntü etiketi Yardımcısı, etkinleştirme `src` öznitelik gerekli `<img>` öğesi.

Resim kaynağını (`src`) statik fiziksel bir dosya sunucusuna işaret etmelidir. Varsa `src` uzak bir URI önbelleği busting sorgu dizesi parametresi oluşturulmadığından.

### <a name="asp-append-version"></a>ASP ekleme sürümü

Zaman `asp-append-version` ile belirtilen bir `true` değer ile birlikte bir `src` özniteliği, görüntü etiketi Yardımcısı çağrılır.

Aşağıdaki örnek, bir görüntü etiketi Yardımcısı kullanır:

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

Statik dosya dizininde bulunuyorsa */wwwroot/resimler/*, oluşturulan HTML (karma farklı olacaktır) aşağıdakine benzer:

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

Parametresine atanan değer `v` karma değeri *asplogo.png* diskteki dosya. Web sunucusu statik dosya için okuma erişimi alamadı ise hiçbir `v` parametresi eklenir `src` biçimlendirmenin özniteliği.

## <a name="hash-caching-behavior"></a>Karma önbelleğe alma davranışı

Görüntü etiketi Yardımcısı önbelleği sağlayıcısı hesaplanan depolamak için yerel web sunucusu üzerinde kullanır `Sha512` belirli bir dosya karması. Dosyanın birden çok kez istenirse, karma yeniden hesaplanması değil. Önbellek dosyaya eklenmiş bir dosya İzleyicisi tarafından geçersiz olduğunda dosyanın `Sha512` karma hesaplanır. Dosyanın diskte değiştiğinde yeni bir karma hesaplanır ve önbelleğe alınmış.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
