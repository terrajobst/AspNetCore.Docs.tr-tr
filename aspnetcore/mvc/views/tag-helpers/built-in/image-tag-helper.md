---
title: "ASP.NET Core görüntü etiketi yok"
author: pkellner
description: "Görüntü etiketi Yardımcısı ile çalışmaya nasıl gösterir"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

Tarafından [Peter Kellner](http://peterkellner.net) 

Görüntü etiketi yardımcı geliştirir `img` (`<img>`) etiketi. Bunu gerektiren bir `src` etiketi yanı sıra `boolean` özniteliği `asp-append-version`.

Varsa görüntü kaynağı (`src`) statik bir dosya mı ana web sunucusunda dize busting benzersiz bir önbellek görüntü kaynağı için sorgu parametresi olarak eklenir. Bu, ana bilgisayar web sunucusunda dosyasını değişirse benzersiz istek URL'si güncelleştirilmiş istek parametresi içeren oluştuğunu sağlar. Dize busting önbellek statik görüntü dosyasının karma temsil eden benzersiz bir değerdir.

Varsa görüntü kaynağı (`src`) (örneğin bir uzak URL veya dosya yoksa sunucuda) statik bir dosya değil `<img>` etiketinin `src` özniteliği önbellek sorgu dizesi parametresi busting yok oluşturulur.

## <a name="image-tag-helper-attributes"></a>Görüntü etiketi yardımcı öznitelik


### <a name="asp-append-version"></a>asp-append-version

İle birlikte belirtildiğinde bir `src` , resim etiketi yardımcı öznitelik çağrılır.

Geçerli bir örneği `img` etiketi yardımcı:

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

Statik dosya dizininde bulunuyorsa *... wwwroot/images/asplogo.PNG* oluşturulan html (karma farklı olacaktır) aşağıdakine benzer:

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

Parametresine atanan değer `v` diskteki dosya karma değeri. Web sunucusu için okuma erişimi alamadı ise statik dosya başvurulan, Hayır `v` parametreleri eklenir `src` özniteliği.

- - -

### <a name="src"></a>src

Görüntü etiketi yardımcı etkinleştirmek için src özniteliği gereklidir `<img>` öğesi. 

> [!NOTE]
> Görüntü etiketi yardımcı kullanan `Cache` hesaplanan depolamak için sağlayıcı yerel web sunucusunda `Sha512` , belirli bir dosya. Dosyayı yeniden isteniyorsa `Sha512` hesaplanacak gerekmez. Önbellek dosyaya iliştirilmiş bir dosya İzleyicisi tarafından geçersiz kılınan olduğunda dosyanın `Sha512` hesaplanır.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:performance/caching/memory>
