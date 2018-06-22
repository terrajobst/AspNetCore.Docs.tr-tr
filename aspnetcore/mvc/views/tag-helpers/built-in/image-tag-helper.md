---
title: ASP.NET Core görüntü etiketi yok
author: pkellner
description: Görüntü etiketi Yardımcısı ile çalışmaya nasıl gösterir
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 7ed160354b25aa0183ac49db93307b1f1b4d0666
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276649"
---
# <a name="image-tag-helper-in-aspnet-core"></a>ASP.NET Core görüntü etiketi yok

Tarafından [Peter Kellner](http://peterkellner.net) 

Görüntü etiketi yardımcı geliştirir `img` (`<img>`) etiketi. Bunu gerektiren bir `src` etiketi yanı sıra `boolean` özniteliği `asp-append-version`.

Varsa görüntü kaynağı (`src`) statik bir dosya mı ana web sunucusunda dize busting benzersiz bir önbellek görüntü kaynağı için sorgu parametresi olarak eklenir. Bu, ana bilgisayar web sunucusunda dosyasını değişirse benzersiz istek URL'si güncelleştirilmiş istek parametresi içeren oluştuğunu sağlar. Dize busting önbellek statik görüntü dosyasının karma temsil eden benzersiz bir değerdir.

Varsa görüntü kaynağı (`src`) (örneğin bir uzak URL veya dosya yoksa sunucuda) statik bir dosya değil `<img>` etiketinin `src` özniteliği önbellek sorgu dizesi parametresi busting yok oluşturulur.

## <a name="image-tag-helper-attributes"></a>Görüntü etiketi yardımcı öznitelik


### <a name="asp-append-version"></a>ASP append sürümü

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
