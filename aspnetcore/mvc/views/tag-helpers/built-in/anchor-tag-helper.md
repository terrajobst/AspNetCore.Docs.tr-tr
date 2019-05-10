---
title: ASP.NET core'da yer işareti etiketi Yardımcısı
author: pkellner
description: ASP.NET Core yer işareti etiketi Yardımcısı öznitelikleri ve her bir öznitelik HTML yer işareti etiketi davranışını genişletmek oynadığı rolü keşfedin.
ms.author: scaddie
ms.custom: mvc
ms.date: 4/18/2019
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: de45c99194d4825c1e404aa193b0f076ba659748
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64898577"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>ASP.NET core'da yer işareti etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) ve [Scott Addie](https://github.com/scottaddie)

[Yer işareti etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) standart HTML tutturucusu geliştirir (`<a ... ></a>`) yeni özellikler ekleyerek etiketi. Kural gereği, öznitelik adları ile ön ekli `asp-`. İşlenen bağlantı öğenin `href` öznitelik değeri, değerleri tarafından belirlenir `asp-` öznitelikleri.

Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

*SpeakerController* örnekleri bu belge boyunca kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

## <a name="anchor-tag-helper-attributes"></a>Yer işareti etiketi Yardımcısı öznitelikleri

### <a name="asp-controller"></a>ASP denetleyicisi

[Asp denetleyicisi](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) öznitelik, URL'yi oluşturmak için kullanılan denetleyici atar. Aşağıdaki biçimlendirmede tüm konuşmacılarını listeler:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Oluşturulan HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Varsa `asp-controller` özniteliği belirtilirse ve `asp-action` değil, varsayılan `asp-action` yürütülmekte görünüm ile ilişkilendirilen denetleyici eylemi bir değerdir. Varsa `asp-action` önceki biçimlendirmeden atlanır ve yer işareti etiketi Yardımcısı kullanılır *HomeController*'s *dizin* görünümü (*/Home*), oluşturulan HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>ASP eylemi

[Asp eylem](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) öznitelik değeri temsil eden oluşturulmuş dahil denetleyici eylem adı `href` özniteliği. Aşağıdaki biçimlendirmede oluşturulan ayarlar `href` Konuşmacı değerlendirmeleri sayfasına öznitelik değeri:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Hayır ise `asp-controller` özniteliği belirtilmezse, geçerli görünüm yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.

Varsa `asp-action` öznitelik değeri `Index`, hiçbir eylem için varsayılan çağırma giden URL, eklenecek sonra `Index` eylem. Eylem belirtilen (veya varsayılan), başvurulan denetleyicisindeki bulunmalıdır `asp-controller`.

### <a name="asp-route-value"></a>ASP - route-{value}

[Asp - route-{value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) öznitelik joker karakter rota öneki sağlar. Herhangi bir değer kaplayan `{value}` yer tutucusu, olası bir rota parametresini yorumlanır. Varsayılan bir yol bulunmazsa, bu rota öneki eklenir oluşturulan `href` bir istek parametresi ve değeri olarak özniteliği. Aksi takdirde, bu rota şablonu konur.

Şu denetleyici eylemi göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

İçinde tanımlanan varsayılan rota şablonuyla *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

MVC görünümü gibi bir eylem tarafından sağlanan bir model kullanır:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail" 
       asp-route-id="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
</body>
</html>
```

Varsayılan yolun `{id?}` yer tutucu eşleşti. Oluşturulan HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Rota öneki eşleştirme yönlendirme şablonunun parçası olmadığından aşağıdaki MVC görünümü olarak kabul edin:

```cshtml
@model Speaker
<!DOCTYPE html>
<html>
<body>
    <a asp-controller="Speaker"
       asp-action="Detail"
       asp-route-speakerid="@Model.SpeakerId">SpeakerId: @Model.SpeakerId</a>
<body>
</html>
```

Aşağıdaki HTML'yi çünkü oluşturulan `speakerid` eşleşen yolda bulunamadı:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Ya da `asp-controller` veya `asp-action` belirtilmemişse, sonra aynı varsayılan işlem olduğundan ardından `asp-route` özniteliği.

### <a name="asp-route"></a>ASP yol

[Asp rota](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) öznitelik, URL'yi doğrudan adlandırılmış bir rotayı bağlama oluşturmak için kullanılır. Kullanarak [yönlendirme öznitelikleri](xref:mvc/controllers/routing#attribute-routing), gösterildiği gibi bir yol adlandırılabilir `SpeakerController` ve kullanılan kendi `Evaluations` eylem:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Aşağıdaki biçimlendirmede `asp-route` öznitelik adlandırılmış rota başvuruyor:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Yer işareti etiketi Yardımcısı doğrudan URL kullanılarak bu denetleyici eylemi bir yol oluşturur */Konuşmacı/değerlendirmeleri*. Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir. Bir rota çakışmayı önlemek için `asp-route` ile kullanılmaması `asp-controller` ve `asp-action` öznitelikleri.

## <a name="asp-all-route-data"></a>ASP tüm rota veri

[Tüm rota veri asp](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) özniteliği bir anahtar-değer çiftlerinin dictionary'si oluşturulmasını destekler. Anahtarı parametre adı ve değerin parametre değeri olduğu.

Aşağıdaki örnekte, bir sözlük başlatılır ve bir Razor görünüme geçirildi. Alternatif olarak, veri modelinizi oturum geçirilebilir.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Yukarıdaki kod, aşağıdaki HTML'yi oluşturur:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Sözlük Aşırı yüklenen gereksinimlerini karşılayan bir sorgu dizesi üretmek için düzleştirilir `Evaluations` eylem:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Sözlükteki tüm anahtarları rota parametrelerinin eşleşiyorsa, bu değerleri uygun şekilde rotadaki yerine kullanılır. Eşleşmeyen diğer değerleri, istek parametreleri oluşturulur.

### <a name="asp-fragment"></a>ASP parçası

[Asp parça](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) özniteliği URL'ye için URL parçası belirtmesini tanımlar. Yer işareti etiketi Yardımcısı karma karakteri ekler (#). Aşağıdaki biçimlendirmede göz önünde bulundurun:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Karma etiketleri, istemci tarafı uygulamalar oluştururken yararlı olur. Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.

### <a name="asp-area"></a>ASP alanı

[Asp alan](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) özniteliği uygun bir yol ayarlamak için kullanılan alan adını ayarlar. Aşağıdaki örnekler tarif nasıl `asp-area` özniteliği, yeniden eşleme yollarını neden olur.

### <a name="usage-in-razor-pages"></a>Razor sayfaları kullanımı

Razor sayfaları alanları, ASP.NET Core 2.1 veya sonraki sürümlerde desteklenir.

Aşağıdaki dizin hiyerarşi göz önünde bulundurun:

* **{} Proje adı**
  * **wwwroot**
  * **Alanlar**
    * **Oturumları**
      * **Sayfalar**
        * *\_ViewStart.cshtml*
        * *Index.cshtml*
        * *Index.cshtml.cs*
  * **Sayfalar**

Başvurmak için biçimlendirmeyi *oturumları* alan *dizin* Razor sayfası:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

Oluşturulan HTML:

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> Razor sayfaları uygulamada alanlarını desteklemek için aşağıdakilerden birini yapın `Startup.ConfigureServices`:
>
> * Ayarlama [uyumluluk sürümü](xref:mvc/compatibility-version) 2.1 veya üzeri.
> * Ayarlama [RazorPagesOptions.AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) özelliğini `true`:
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

### <a name="usage-in-mvc"></a>MVC kullanımı

Aşağıdaki dizin hiyerarşi göz önünde bulundurun:

* **{} Proje adı**
  * **wwwroot**
  * **Alanlar**
    * **Bloglar**
      * **Denetleyiciler**
        * *HomeController.cs*
      * **Görünümler**
        * **Giriş**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *\_ViewStart.cshtml*
  * **Denetleyiciler**

Ayarı `asp-area` "Bloglarda" dizin ön ekleri *alanlar/Blog'lar* ilişkili denetleyicileri ve görünümlerinin bu yer işareti etiketi için yollar. Başvurmak için biçimlendirmeyi *AboutBlog* görünümü:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Oluşturulan HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Bir MVC uygulamasında alanlarını desteklemek için rota şablonu varsa, alan başvuru içermelidir. Bu şablon ikinci parametre tarafından temsil edilen `routes.MapRoute` yöntem çağrısı *Startup.Configure*:
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

### <a name="asp-protocol"></a>ASP Protokolü

[Asp Protokolü](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) özniteliği, bir protokol belirtmek için (gibi `https`), URL. Örneğin:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Oluşturulan HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Ana bilgisayar adı örnekteki localhost'tur. Yer işareti etiketi Yardımcısı Web sitesinin genel etki alanı için URL oluşturulurken kullanır.

### <a name="asp-host"></a>ASP konak

[Asp konak](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) özniteliği, bir ana bilgisayar adı, URL belirtmek için. Örneğin:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Oluşturulan HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

### <a name="asp-page"></a>ASP sayfası

[Asp sayfasının](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) özniteliği, Razor sayfaları ile kullanılır. Bir yer işareti etiketin ayarlamak için kullanın `href` belirli bir sayfaya öznitelik değeri. Sayfanın adını bir eğik çizgi ("/"), URL oluşturur.

Aşağıdaki örnek, katılımcı Razor sayfası noktaları:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Oluşturulan HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` Özniteliktir birbirini dışlayan `asp-route`, `asp-controller`, ve `asp-action` öznitelikleri. Ancak, `asp-page` kullanılabilir `asp-route-{value}` yönlendirme, aşağıdaki biçimlendirme gösterildiği gibi denetlemek için:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

### <a name="asp-page-handler"></a>ASP sayfası işleyicisi

[Asp sayfasını işleyici](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) özniteliği, Razor sayfaları ile kullanılır. Belirli bir sayfaya işleyicilerine bağlamak için tasarlanmıştır.

Aşağıdaki sayfayı işleyici göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Sayfa modeli biçimlendirme bağlantılar ilişkili `OnGetProfile` sayfası işleyicisi. Not `On<Verb>` sayfa işleyicisi yöntem adı ön eki atlanırsa `asp-page-handler` öznitelik değeri. Yöntemi zaman uyumsuz olduğunda `Async` soneki atlanırsa, çok.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
