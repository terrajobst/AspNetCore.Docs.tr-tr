---
title: ASP.NET Core, kısmi görünümleri
author: ardalis
description: Kısmi görünümler büyük işaretleme dosyaları bölün ve ASP.NET Core uygulamaları, web sayfaları arasında ortak biçimlendirme çoğaltma azaltmak için nasıl kullanılacağını keşfedin.
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601762"
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core, kısmi görünümleri

Tarafından [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Scott Sauber](https://twitter.com/scottsauber)

Kısmi bir görünümü bir [Razor](xref:mvc/views/razor) işaretleme dosyasının (*.cshtml*) HTML çıkışı işleyen *içinde* başka bir işaretleme dosyasının çıkış işlenen.

::: moniker range=">= aspnetcore-2.1"

Terim *kısmi Görünüm* burada biçimlendirme dosyaları olarak da adlandırılır ya da bir MVC uygulaması geliştirilirken kullanılır *görünümleri*, veya biçimlendirme dosyalarında biçimlendirmeyi çağrılır burada bir Razor sayfaları uygulama *sayfaları*. Bu konuda genel MVC görünümleri ve Razor sayfaları sayfaları olarak başvurduğu *biçimlendirme dosyalarında biçimlendirmeyi*.

::: moniker-end

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-partial-views"></a>Kısmi görünümler kullanma zamanı

Kısmi görünümler için etkili bir yoludur:

* Daha küçük bileşenlere büyük işaretleme dosyaları bölün.

  Büyük, karmaşık işaretleme dosyasının mantıksal parçalarını oluşur, her parça bir kısmi görünüme yalıtılmış çalışma bir avantaj sağlamaz. Kodu biçimlendirme dosyasında yönetilebilir çünkü biçimlendirme yalnızca genel sayfa yapısı ve kısmi görünümler için başvurular içerir.
* Ortak biçimlendirme içeriğin biçimlendirme dosyalardaki azaltmak.

  Biçimlendirme dosyalardaki aynı biçimlendirme öğeleri kullanıldığında biçimlendirme içerik çoğaltılması bir kısmi görünüm dosyasına kısmi görünüm kaldırır. Kısmi görünüm biçimlendirme değiştirildiğinde, kısmi görünümü kullanmak biçimlendirme dosyaların işlenen çıkışı güncelleştirir.

Kısmi görünümler, ortak yerleşim öğelerinin korumak için kullanılmamalıdır. Ortak yerleşim öğeleri tanımlanmamalıdır [_Layout.cshtml](xref:mvc/views/layout) dosyaları.

Kısmi görünüm karmaşık işleme mantığı ya da kod yürütme biçimlendirmesi oluşturmak için gerekli olduğu kullanmayın. Kısmi bir görünümü yerine kullanmak bir [görünümü bileşen](xref:mvc/views/view-components).

## <a name="declare-partial-views"></a>Kısmi görünümler bildirme

::: moniker range=">= aspnetcore-2.1"

Kısmi bir görünümü bir *.cshtml* işaretleme dosyasının tutulan içinde *görünümleri* klasörü (MVC) veya *sayfaları* klasörü (Razor sayfaları).

ASP.NET Core MVC, denetleyici 's, <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir görünüm veya kısmi görünüm döndürme özelliğine sahiptir. ASP.NET Core 2.2 Razor sayfaları için benzer bir yetenek planlanmaktadır. Razor sayfaları içinde bir <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> döndürebilir bir <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>. Başvuru ve kısmi görünümleri işlemeye açıklanan [kısmi görünüm başvuru](#reference-a-partial-view) bölümü.

MVC görünümü veya sayfa işleme aksine, kısmi görünüm çalıştırmaz *_ViewStart.cshtml*. Daha fazla bilgi için *_ViewStart.cshtml*, bkz: <xref:mvc/views/layout>.

Kısmi görünüm dosya adları, genellikle bir alt çizgiyle başlayan (`_`). Bu adlandırma kuralı gerekmez, ancak kısmi görünümler görünümlere ve sayfalara görsel olarak ayırt etmenize yardımcı olur. Dosya adı alt çizgi ile başlar, dosyanın biçimlendirme içerdiğinde bile Razor sayfaları işaretleme dosyasının bir Razor sayfaları sayfa olarak işlemiyor `@page` yönergesi.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Kısmi bir görünümü bir *.cshtml* işaretleme dosyasının tutulan içinde *görünümleri* klasör.

Bir denetleyicinin <xref:Microsoft.AspNetCore.Mvc.ViewResult> bir görünüm veya kısmi görünüm döndürme özelliğine sahiptir.

MVC görünümü işleme aksine, kısmi görünüm çalıştırmaz *_ViewStart.cshtml*. Daha fazla bilgi için *_ViewStart.cshtml*, bkz: <xref:mvc/views/layout>.

Kısmi görünüm dosya adları, genellikle bir alt çizgiyle başlayan (`_`). Bu adlandırma kuralı gerekmez, ancak kısmi görünümler görünümleri görsel olarak ayırt etmesine yardımcı olur.

::: moniker-end

## <a name="reference-a-partial-view"></a>Kısmi görünüm başvurusu

::: moniker range=">= aspnetcore-2.1"

Bir işaretleme dosyasının içinde kısmi görünüm başvurmak için birkaç yolu vardır. Uygulamaları aşağıdaki zaman uyumsuz işleme yaklaşımlardan birini kullanmanızı öneririz:

* [Kısmi Etiket Yardımcısı](#partial-tag-helper)
* [Zaman uyumsuz HTML Yardımcısı](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Bir işaretleme dosyasının içinde kısmi görünüm başvurmak için iki yolu vardır:

* [Zaman uyumsuz HTML Yardımcısı](#asynchronous-html-helper)
* [Zaman uyumlu HTML Yardımcısı](#synchronous-html-helper)

Uygulamaları kullanmanızı öneririz [zaman uyumsuz HTML Yardımcısı](#asynchronous-html-helper).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Kısmi etiket Yardımcısı

[Kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) ASP.NET Core 2.1 veya üzerini gerektirir.

Kısmi etiket Yardımcısı içeriği zaman uyumsuz olarak işler ve HTML benzeri bir sözdizimi kullanır:

```cshtml
<partial name="_PartialName" />
```

Bir dosya uzantısı varsa, etiket Yardımcısı kısmi görünüm çağırma biçimlendirme dosyasıyla aynı klasörde olması gereken bir kısmi görünüm başvuruyor:

```cshtml
<partial name="_PartialName.cshtml" />
```

Aşağıdaki örnek, uygulama kök kısmi görünüm başvuruyor. Bir tilde-eğik çizgi ile başlayan yollar (`~/`) veya eğik çizgi (`/`) uygulama kök dizinine bakın:

**Razor Sayfaları**

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

**MVC**

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

Aşağıdaki örnek, göreli bir yol ile kısmi görünüm başvuruyor:

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Zaman uyumsuz HTML Yardımcısı

Bir HTML Yardımcısı kullanırken en iyi kullanmaktır <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>. `PartialAsync` döndürür bir <xref:Microsoft.AspNetCore.Html.IHtmlContent> türü içinde kaydırılır bir <xref:System.Threading.Tasks.Task`1>. Yöntem ile bekletilen çağrısı koyarak başvurulan bir `@` karakter:

```cshtml
@await Html.PartialAsync("_PartialName")
```

Dosya uzantısı varsa, kısmi görünüm çağırma biçimlendirme dosyasıyla aynı klasörde olmalıdır kısmi bir görünümü HTML Yardımcısı başvuruyor:

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

Aşağıdaki örnek, uygulama kök kısmi görünüm başvuruyor. Bir tilde-eğik çizgi ile başlayan yollar (`~/`) veya eğik çizgi (`/`) uygulama kök dizinine bakın:

::: moniker range=">= aspnetcore-2.1"

**Razor Sayfaları**

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

**MVC**

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

Aşağıdaki örnek, göreli bir yol ile kısmi görünüm başvuruyor:

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Alternatif olarak, kısmi bir görünümü ile oluşturulabilen <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>. Bu yöntem döndürmüyor bir <xref:Microsoft.AspNetCore.Html.IHtmlContent>. Bu yanıt doğrudan işlenmiş çıktı akışları. Yöntem bir sonuç döndürmediğinden Razor kodu bloğu çağrılmalıdır:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Bu yana `RenderPartialAsync` akışları işlenmiş içeriği, bazı senaryolarda daha iyi performans sağlar. Performans açısından kritik durumlarda, her iki yaklaşım kullanarak sayfayı Kıyaslama ve daha hızlı bir yanıt oluşturan bir yaklaşım kullanın.

### <a name="synchronous-html-helper"></a>Zaman uyumlu HTML Yardımcısı

<xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> ve <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> zaman uyumlu eşdeğerleri olan `PartialAsync` ve `RenderPartialAsync`sırasıyla. Hangi kilitlenme senaryolar olduğundan, zaman uyumlu eşdeğerleri önerilmez. Gelecek sürümlerde kaldırılması için zaman uyumlu metotları hedefler.

> [!IMPORTANT]
> Kodu çalıştırmak ihtiyacınız varsa, bir [görünümü bileşen](xref:mvc/views/view-components) yerine kısmi görünüm.

::: moniker range=">= aspnetcore-2.1"

Çağırma `Partial` veya `RenderPartial` sonuçları bir Visual Studio analyzer uyarı. Örneğin, varlığını `Partial` aşağıdaki uyarı iletisini verir:

> Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir. Kullanmayı &lt;kısmi&gt; etiketi Yardımcısı veya IHtmlHelper.PartialAsync.

Çağrıları değiştirin `@Html.Partial` ile `@await Html.PartialAsync` veya [kısmi etiket Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Kısmi etiket Yardımcısı geçiş hakkında daha fazla bilgi için bkz. [HTML Yardımcısı'ten geçiş](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Kısmi görünüm bulma

Kısmi Görünüm adı olmayan bir dosya uzantısı tarafından başvurulduğunda belirtilen sırayla aşağıdaki konumlardan aranır:

::: moniker range=">= aspnetcore-2.1"

**Razor Sayfaları**

1. Sayfanın klasörü şu anda yürütülüyor
1. Dizin graph sayfanın klasörün üstünde
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

**MVC**

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

Kısmi görünüm bulma için aşağıdaki kurallar geçerlidir:

* Aynı dosya adına sahip farklı kısmi görünümler, kısmi görünümleri farklı klasörlerde bulunan olduğunda izin verilir.
* Kısmi Görünüm adı olmayan bir dosya uzantısı ile kısmi görünüm tarafından başvuru olduğunda hem arayanın klasörde mevcut ve *paylaşılan* kısmi görünüm klasörü, arayanın klasöründeki kısmi görünüm sağlar. Kısmi görünüm arayanın klasörde mevcut değilse, kısmi görünüm gelen sağlanır *paylaşılan* klasör. Kısmi görünümler içinde *paylaşılan* klasör çağrılır *paylaşılan kısmi görünümler* veya *varsayılan kısmi görünümler*.
* Kısmi görünümler olabilir *zincirleme*&mdash;döngüsel bir başvuru çağrıları'na göre biçimlendirilmiş değil, kısmi görünüm başka bir kısmi görünüm çağırabilirsiniz. Göreli yolları her zaman kök veya dosyanın üst geçerli dosyanın göreli olur.

> [!NOTE]
> A [Razor](xref:mvc/views/razor) `section` tanımlanan bir kısmi görünüm üst işaretleme dosyaları için görünmez. `section` Yalnızca tanımlanmış kısmi görünüm için görünür durumdadır.

## <a name="access-data-from-partial-views"></a>Kısmi görünümler verilere erişmek

Kısmi görünümün örneği oluşturulduğunda aldığı bir *kopyalama* üst öğenin'ın `ViewData` sözlüğü. Kısmi görünüm içindeki verilerde yapılan güncelleştirmeler üst görünümde kalıcı değildir. `ViewData` Kısmi görünüm döndürdüğünde kısmi görünüm değişiklikler kaybolur.

Aşağıdaki örnek bir örneğini geçirin gösterilmektedir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüm için:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Kısmi görünüme, bir model geçirebilirsiniz. Modeli, özel bir nesne olabilir. Bir model ile geçirdiğiniz `PartialAsync` (içerik bloğu çağırana işler) veya `RenderPartialAsync` (çıkış içeriği akışları):

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

**Razor Sayfaları**

Örnek uygulama aşağıdaki biçimlendirmede dandır *Pages/ArticlesRP/ReadRP.cshtml* sayfası. İki kısmi görünüm sayfası içerir. Bir modeldeki ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüm için. `ViewDataDictionary` Oluşturucu aşırı yüklemesi, yeni bir geçirmek için kullanılır `ViewData` varolan korurken sözlük `ViewData` sözlüğü.

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

*Pages/Shared/_AuthorPartialRP.cshtml* tarafından başvurulan ilk kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

*Pages/ArticlesRP/_ArticleSectionRP.cshtml* tarafından başvurulan ikinci kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

**MVC**

::: moniker-end

Aşağıdaki örnek uygulamanın gösterildiği biçimlendirmede *Views/Articles/Read.cshtml* görünümü. İki kısmi görünümler görünümün içerir. Bir modeldeki ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüm için. `ViewDataDictionary` Oluşturucu aşırı yüklemesi, yeni bir geçirmek için kullanılır `ViewData` varolan korurken sözlük `ViewData` sözlüğü.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

*Views/Shared/_AuthorPartial.cshtml* tarafından başvurulan ilk kısmi Görünüm *ReadRP.cshtml* işaretleme dosyasının:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*Views/Articles/_ArticleSection.cshtml* tarafından başvurulan ikinci kısmi Görünüm *Read.cshtml* işaretleme dosyasının:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Kısmi çalışma zamanında işlendiğini işlenmiş çıkış üst işaretleme dosyasının kendisi işlenen paylaşılan içinde *_Layout.cshtml*. Bir makalede yazarın adı ve yayın tarihi ilk kısmi görünümü işler:

> Abraham Lincoln
>
> Kısmi bu görünümden &lt;paylaşılan kısmi görünüm dosya yolu&gt;.
> 19/11/1863 12:00:00: 00

Makalenin bölümleri ikinci kısmi görünümü işler:

> Bir dizin bölümünde: 0
>
> Dört puanı ve yedi yıl önce...
>
> İki bölüm dizini: 1
>
> Biz de harika bir inşaat war katılan artık test...
>
> Üç bölüm dizini: 2
>
> Ancak, daha büyük bir anlamda, biz değil ayırabilirsiniz...

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
