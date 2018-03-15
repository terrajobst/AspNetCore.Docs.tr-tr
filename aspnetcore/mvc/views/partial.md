---
title: "ASP.NET Core kısmi görünümleri"
author: ardalis
description: "Kısmi görünüm nasıl olduğunu öğrenin başka bir görünüm içinde işlenir ve ne zaman bunlar kullanılmalıdır ASP.NET Core uygulamaları bir görünüm."
manager: wpickett
ms.author: riande
ms.date: 03/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/partial
ms.openlocfilehash: abe970b02a62ef58deb259241d7451de0185575c
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core kısmi görünümleri

Tarafından [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), ve [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core MVC web farklı görünümleri arasında paylaşmak istediğiniz sayfaları yeniden kullanılabilir parçalarını olduğunda faydalıdır kısmi görünümler destekler.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Kısmi görünümler nelerdir?

Kısmi görünüm içinde başka bir görünüm işlenen bir görünümüdür. Kısmi görünüm yürüterek oluşturulan HTML çıkışını çağırma (veya üst) görünüme işlenir. Görünümler gibi kısmi görünümleri kullanma *.cshtml* dosya uzantısı.

## <a name="when-should-i-use-partial-views"></a>Kısmi görünümler ne zaman kullanmalıyım?

Kısmi görünümler, büyük görünümleri ayırma küçük bileşenlere etkili bir yoldur. Bunlar içeriğini görüntüleme yinelenmesini azaltmak ve görünüm öğeleri yeniden izin verebilirsiniz. Ortak yerleşim öğeler belirtilmelidir [_Layout.cshtml](layout.md). Olmayan düzenini yeniden kullanılabilir içerik kısmi görünümlere kapsüllenmiş.

Çeşitli mantıksal parçalarını oluşan karmaşık bir sayfa varsa, her parça kendi kısmi görünüm olarak çalışmak yararlı olabilir. Her bir sayfanın parçası sayfasının geri kalanından yalıtım görüntülenebilir ve, yalnızca genel sayfa yapısı ve kısmi görünümleri işlemeye çağrıları içerdiğinden görünüm sayfası için daha kolay olur.

İpucu: İzleyin [yok yineleyin kendiniz ilkesine](http://deviq.com/don-t-repeat-yourself/) görünümlerinizi de.

## <a name="declaring-partial-views"></a>Kısmi görünümler bildirme

Kısmi görünümler gibi herhangi bir görünüm oluşturulur: oluşturduğunuz bir *.cshtml* içinde dosya *görünümleri* klasör. Kısmi Görünüm ve normal bir görünüm arasında anlamsal fark yoktur - bunlar yalnızca farklı çizilir. Doğrudan bir denetleyicinin döndürülen bir görünümü olabilir `ViewResult`, ve aynı görünüm bir kısmi görünüm olarak kullanılabilir. Kısmi görünümler çalıştırma bir görünüm ve kısmi görünüm nasıl işlendiğini arasındaki temel fark olan *_ViewStart.cshtml* (görünümler hakkında daha fazla bilgi sırada *_ViewStart.cshtml* içinde [düzeni ](layout.md)).

## <a name="referencing-a-partial-view"></a>Kısmi görünüm başvurma

Gelen bir görünüm sayfası içinde kısmi görünüm işlemek birkaç yolu vardır. Kullanılacak en kolayıdır `Html.Partial`, döndüren bir `IHtmlString` ve çağrısı ile ekleyerek başvurulabilir `@`:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

`PartialAsync` Yöntemi, (kod görünümlerde genellikle önerilmez rağmen) zaman uyumsuz kod içeren kısmi görünümleri için kullanılabilir:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

Kısmi bir görünümü ile işleyebilen `RenderPartial`. Bu yöntem, bir sonuç dönmez; doğrudan yanıta işlenen çıkış akışları. Bir sonuç döndürmediğinden, Razor kod bloğu içinde çağırılmalıdır (Ayrıca çağırabilirsiniz `RenderPartialAsync` gerekirse):

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

Sonuç doğrudan akışları çünkü `RenderPartial` ve `RenderPartialAsync` bazı senaryolarda daha iyi gerçekleştirebilir. Ancak, önerilir, çoğu durumda kullanmanız `Partial` ve `PartialAsync`.

> [!NOTE]
> Kendi görünümlerinizi kod yürütmek gerekiyorsa, önerilen desenini kullanmaktır bir [görünümü bileşen](view-components.md) yerine kısmi görünüm.

### <a name="partial-view-discovery"></a>Kısmi görünüm bulma

Kısmi görünüm başvururken çeşitli şekillerde konumuna başvurabilir:

```cshtml
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

Farklı bir görünüm klasörlerinde farklı kısmi görünümler aynı ada sahip olabilir. Görünümler (dosya uzantısı olmadan) adıyla başvururken her klasör görünümlerde bunları ile aynı klasörde kısmi görünüm kullanır. İçinde yerleştirme kullanmak için varsayılan kısmi görünüm belirtebilirsiniz *paylaşılan* klasör. Paylaşılan kısmi görünümü, kısmi görünümü kendi sürümüne sahip olmayan tüm görünümleri tarafından kullanılır. Varsayılan kısmi görünüm olabilir (içinde *paylaşılan*), üst görünüm olarak aynı klasörde aynı ada sahip bir kısmi görünüm tarafından geçersiz.

Kısmi görünümler olabilir *zincirleme*. Diğer bir deyişle, kısmi Görünüm (döngü oluşturmayın sürece) başka bir kısmi görünüm çağırabilirsiniz. Her görünüm veya kısmi görünümü içinde göreli her zaman bu görünüm veya değil, kök üst görünüme göre yollardır.

> [!NOTE]
> Bildirirseniz bir [Razor](razor.md) `section` kısmi bir görünümde, kendi üstlerinin için görünür olmaz; kısmi görünüme sınırlı olacaktır.

## <a name="accessing-data-from-partial-views"></a>Kısmi görünümleri verilerine erişme

Kısmi görünümün örneği oluşturulduğunda, üst görünümün bir kopyasını alır `ViewData` sözlük. Kısmi görünüm içindeki verilere yapılan güncelleştirmeler üst görünümde kalıcı değildir. `ViewData` Değiştirilen bir kısmi görünümü, kısmi görünüm döndürdüğünde kaybolur.

Örneği geçirebilirsiniz `ViewDataDictionary` kısmi görünüm için:

```cshtml
@Html.Partial("PartialName", customViewData)
```

Ayrıca, bir model kısmi görünüme geçirebilirsiniz. Bu sayfanın görünüm modeli, veya bir bölümünün veya özel bir nesne olabilir. Bir model geçirebilirsiniz `Partial`,`PartialAsync`, `RenderPartial`, veya `RenderPartialAsync`:

```cshtml
@Html.Partial("PartialName", viewModel)
```

Örneği geçirebilirsiniz `ViewDataDictionary` ve kısmi görünüm için Görünüm modeli:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

Biçimlendirme gösterir aşağıda *Views/Articles/Read.cshtml* iki kısmi görünümleri içeren görünümü. Bir model ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüme. Yeni geçirebilirsiniz `ViewData` varolan korurken sözlük `ViewData` Oluşturucusu aşırı yüklemesini kullanırsanız `ViewDataDictionary` aşağıda vurgulanan:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

*Görünümler/paylaşılan/AuthorPartial*:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

*ArticleSection* kısmi:

[!code-cshtml[](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

Çalışma zamanında kısmi işlendiğini üst görünüme kendisi işlenen paylaşılan içinde *_Layout.cshtml*

![Kısmi görünüm çıktı](partial/_static/output.png)
