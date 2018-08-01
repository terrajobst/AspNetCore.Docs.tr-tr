---
title: ASP.NET Core, kısmi görünümleri
author: ardalis
description: Kısmi görünüm nasıl olduğunu öğrenin başka bir görünümü içinde işlenir ve ne zaman bunların kullanılması gerekir ASP.NET Core uygulamalarında bir görünüm.
ms.author: riande
ms.custom: mvc
ms.date: 07/06/2018
uid: mvc/views/partial
ms.openlocfilehash: 7cb20fc30609adad83cb40e91316da115817f035
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39378689"
---
# <a name="partial-views-in-aspnet-core"></a>ASP.NET Core, kısmi görünümleri

Tarafından [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), ve [Scott Sauber](https://twitter.com/scottsauber)

ASP.NET Core, kısmi görünümleri destekler. Kısmi görünümler, web sayfaları yeniden kullanılabilir parça farklı görünümleri arasında paylaşmak için kullanılır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-are-partial-views"></a>Kısmi görünümler nelerdir

Kısmi görünüm başka bir görünüm işlenen bir görünümüdür. Kısmi görünüm yürüterek oluşturulan HTML çıktı çağırma (veya üst) görünüm oluşturulur. Gibi görünümler, kısmi görünümleri kullanma *.cshtml* dosya uzantısı.

Örneğin, ASP.NET Core 2.1 **Web uygulaması** proje şablonu içeren bir *_CookieConsentPartial.cshtml* kısmi görünüm. Kısmi görünüm içinden yüklenen *_Layout.cshtml*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_Layout.cshtml?name=snippet_CookieConsentPartial)]

## <a name="when-to-use-partial-views"></a>Kısmi görünümler kullanma zamanı

Kısmi görünümler, daha küçük bileşenlere büyük görünümleri ayırma etkili bir yoludur. Görünüm içeriğinin azaltmak ve görünüm öğelerinin yeniden kullanılabilmeleri. Ortak yerleşim öğeleri tanımlanmamalıdır [_Layout.cshtml](xref:mvc/views/layout). Yeniden kullanılabilir içerik olmayan Düzen kısmi görünümlere kapsüllenmiş.

Mantıksal parçalarını oluşan karmaşık bir sayfasında, her bir parçanın kendi kısmi görünüm olarak çalışmak yararlıdır. Her sayfanın parçası sayfanın geri kalanını yalıtımdan görüntülenebilir. Sayfa görünümü, yalnızca kısmi görünüm işlemek için çağrıları ve genel sayfa yapısı içerdiğinden kolaylaşır.

ASP.NET Core MVC denetleyicileri sahip bir [PartialView](/dotnet/api/microsoft.aspnetcore.mvc.controller.partialview#Microsoft_AspNetCore_Mvc_Controller_PartialView) eylem yönteminden çağrılan yöntem. Razor sayfaları eşdeğeri olan `PartialView` yöntemi.

## <a name="declare-partial-views"></a>Kısmi görünümler bildirme

Kısmi görünümler, normal bir görünüm gibi oluşturulur&mdash;oluşturarak bir *.cshtml* içinde dosya *görünümleri* klasör. Kısmi Görünüm ve normal görünüm arasında anlamsal fark yoktur; Ancak, bunlar farklı işlenen. Doğrudan bir denetleyicinin döndürülen bir görünüm olabilir [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), ve bir kısmi görünüm olarak aynı görünümde kullanılabilir. Kısmi görünümler çalıştırma görünümü ve kısmi görünümün nasıl oluşturulacağını arasındaki ana fark olduğu *_ViewStart.cshtml*. Normal Görünüm çalıştırma *_ViewStart.cshtml*. Daha fazla bilgi edinin *_ViewStart.cshtml* içinde [Düzen](xref:mvc/views/layout)).

Bir kural, kısmi görünüm dosya adları genellikle ile başlayan `_`. Bu adlandırma kuralını bir gereksinim değildir, ancak kısmi görünümler normal görüntülerden görsel olarak ayırt etmesine yardımcı olur.

## <a name="reference-a-partial-view"></a>Kısmi görünüm başvurusu

Bir görünüm sayfası içinde kısmi görünüm işlemek için çeşitli yollar vardır. Zaman uyumsuz işleme kullanmak için en iyi yöntem olacaktır.

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a>Kısmi etiket Yardımcısı

Kısmi etiket Yardımcısı, ASP.NET Core 2.1 veya üzerini gerektirir. Zaman uyumsuz olarak işler ve HTML benzeri bir sözdizimi kullanır:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialTagHelper)]

Daha fazla bilgi için bkz. <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.

::: moniker-end

### <a name="asynchronous-html-helper"></a>Zaman uyumsuz HTML Yardımcısı

Bir HTML Yardımcısı kullanırken en iyi kullanmaktır [PartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partialasync#Microsoft_AspNetCore_Mvc_Rendering_HtmlHelperPartialExtensions_PartialAsync_Microsoft_AspNetCore_Mvc_Rendering_IHtmlHelper_System_String_). Döndürür bir [IHtmlContent](/dotnet/api/microsoft.aspnetcore.html.ihtmlcontent) türü içinde kaydırılır bir `Task`. Yöntem çağrısı ile ekleyerek başvurulan `@`:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_PartialAsync)]

Alternatif olarak, kısmi bir görünümü ile oluşturulabilen [RenderPartialAsync](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartialasync). Bu yöntem, bir sonuç döndürmez. Bu yanıt doğrudan işlenmiş çıktı akışları. Yöntem bir sonuç döndürmediğinden Razor kodu bloğu çağrılmalıdır:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

Sonuç doğrudan akışları beri `RenderPartialAsync` bazı senaryolarda daha iyi gerçekleştirebilir. Ancak, kullanmanız önerilir `PartialAsync`.

### <a name="synchronous-html-helper"></a>Zaman uyumlu HTML Yardımcısı

[Kısmi](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.partial) ve [RenderPartial](/dotnet/api/microsoft.aspnetcore.mvc.rendering.htmlhelperpartialextensions.renderpartial) zaman uyumlu eşdeğerleri olan `PartialAsync` ve `RenderPartialAsync`sırasıyla. Senaryoları, kilitlenme olduğundan, zaman uyumlu eşdeğerleri kullanımı önerilmez. Gelecek sürümlerde, zaman uyumlu metotları içermez.

> [!IMPORTANT]
> Kod yürütmek kendi görünümlerinizi ihtiyacınız varsa, bir [görünümü bileşen](xref:mvc/views/view-components) yerine kısmi görünüm.

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 veya daha sonra çağırma `Partial` veya `RenderPartial` sonuçları bir çözümleyici uyarı. Örneğin, kullanımını `Partial` aşağıdaki uyarı iletisini verir:

> Uygulama kilitlenmeleri IHtmlHelper.Partial kullanımına neden olabilir. Kullanmayı `<partial>` etiketi Yardımcısı veya `IHtmlHelper.PartialAsync`.

Çağrıları değiştirin `@Html.Partial` ile `@await Html.PartialAsync` veya kısmi etiket Yardımcısı. Kısmi etiket Yardımcısı geçiş hakkında daha fazla bilgi için bkz. [HTML Yardımcısı'ten geçiş](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).

::: moniker-end

## <a name="partial-view-discovery"></a>Kısmi görünüm bulma

Kısmi görünüm başvururken çeşitli şekillerde konumuna başvurabilir. Örneğin:

::: moniker range=">= aspnetcore-2.1"

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
<partial name="_ViewName" />

// A view with this name must be in the same folder
<partial name="_ViewName.cshtml" />

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
<partial name="~/Views/Folder/_ViewName.cshtml" />
<partial name="/Views/Folder/_ViewName.cshtml" />

// Locate the view using a relative path
<partial name="../Account/_LoginPartial.cshtml" />
```

Yukarıdaki örnekte, ASP.NET Core 2.1 veya üzeri gerektiren kısmi etiket Yardımcısı, kullanır. Aşağıdaki örnek aynı görevi başarmak için zaman uyumsuz HTML Yardımcıları kullanır.

::: moniker-end

```cshtml
// Uses a view in current folder with this name.
// If none is found, searches the Shared folder.
@await Html.PartialAsync("_ViewName")

// A view with this name must be in the same folder
@await Html.PartialAsync("_ViewName.cshtml")

// Locate the view based on the app root.
// Paths that start with "/" or "~/" refer to the app root.
@await Html.PartialAsync("~/Views/Folder/_ViewName.cshtml")
@await Html.PartialAsync("/Views/Folder/_ViewName.cshtml")

// Locate the view using a relative path
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

Farklı bir görünüm klasörlerinde farklı kısmi görünümler ile aynı dosya adına sahip olabilir. Görünümleri (olmadan, bir dosya uzantısı) adıyla başvururken görünümleri her klasördeki kısmi görünüm bunları ile aynı klasörde kullanın. İçine yerleştirerek kullanmak üzere varsayılan bir kısmi görünüm belirtebilirsiniz *paylaşılan* klasör. Paylaşılan kısmi görünümü, kısmi görünümü kendi sürümüne sahip değilseniz herhangi görünümler tarafından kullanılır. Varsayılan kısmi görünüm olabilir (içinde *paylaşılan*), üst görünüm olarak aynı klasörde aynı adda bir kısmi görünüm tarafından geçersiz.

Kısmi görünümler olabilir *zincirleme*&mdash;(döngü oluşturmayın sürece) kısmi görünüm başka bir kısmi görünüm çağırabilirsiniz. Her görünüm veya kısmi görünümü içinde göreli yolları her zaman bu görünümü, kök uygulanmaz veya üst görünüm göreli olur.

> [!NOTE]
> A [Razor](xref:mvc/views/razor) `section` tanımlanan bir kısmi görünüm ana görünümlerine görünmez. `section` Yalnızca tanımlanmış kısmi görünüm için görünür durumdadır.

## <a name="access-data-from-partial-views"></a>Kısmi görünümler verilere erişmek

Kısmi görünümün örneği oluşturulduğunda, üst görünümün bir kopyasını alır. `ViewData` sözlüğü. Kısmi görünüm içindeki verilerde yapılan güncelleştirmeler üst görünümde kalıcı değildir. `ViewData` Kısmi görünüm döndürdüğünde kısmi görünüm değişiklikler kaybolur.

Örneği geçirebilirsiniz [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) kısmi görünüm için:

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

Kısmi görünüme, bir model geçirebilirsiniz. Model, sayfanın görünüm modeli veya özel bir nesne olabilir. Bir modele geçirebilir `PartialAsync` veya `RenderPartialAsync`:

```cshtml
@await Html.PartialAsync("_PartialName", viewModel)
```

Örneği geçirebilirsiniz `ViewDataDictionary` kısmi görünüm için bir görünüm modeli:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_PartialAsync)]

Aşağıdaki biçimlendirme gösterildiği *Views/Articles/Read.cshtml* görünümü, iki kısmi görünümler içerir. Bir modeldeki ikinci kısmi görünüm geçirir ve `ViewData` kısmi görünüm için. Vurgulanan kullanın `ViewDataDictionary` oluşturucu aşırı yüklemesi, yeni bir geçirilecek `ViewData` varolan korurken sözlük `ViewData` sözlüğü.

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=17-20)]

*Görünümler/paylaşılan/_AuthorPartial*:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

*_ArticleSection* kısmi:

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

Kısmi çalışma zamanında işlendiğini üst görünüme kendisi işlenen paylaşılan içinde *_Layout.cshtml*.

![Kısmi görünüm çıkış](partial/_static/output.png)

## <a name="additional-resources"></a>Ek kaynaklar

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>

::: moniker-end
