---
title: ASP.NET core'da yer işareti etiketi Yardımcısı
author: pkellner
description: ASP.NET Core yer işareti etiketi Yardımcısı öznitelikleri ve her bir öznitelik HTML yer işareti etiketi davranışını genişletmek oynadığı rolü keşfedin.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 13508729c1e3b64a8b0e6965da57880738ab85c3
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325555"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>ASP.NET core'da yer işareti etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) ve [Scott Addie](https://github.com/scottaddie)

[Yer işareti etiketi Yardımcısı](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) standart HTML tutturucusu geliştirir (`<a ... ></a>`) yeni özellikler ekleyerek etiketi. Kural gereği, öznitelik adları ile ön ekli `asp-`. İşlenen bağlantı öğenin `href` öznitelik değeri, değerleri tarafından belirlenir `asp-` öznitelikleri.

Etiket Yardımcıları genel bakış için bkz. <xref:mvc/views/tag-helpers/intro>.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

*SpeakerController* örnekleri bu belge boyunca kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Bir envanterini `asp-` aşağıdaki öznitelikleri.

## <a name="asp-controller"></a>ASP denetleyicisi

[Asp denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) öznitelik, URL'yi oluşturmak için kullanılan denetleyici atar. Aşağıdaki biçimlendirmede tüm konuşmacılarını listeler:

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

[Asp eylem](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) öznitelik değeri temsil eden oluşturulmuş dahil denetleyici eylem adı `href` özniteliği. Aşağıdaki biçimlendirmede oluşturulan ayarlar `href` Konuşmacı değerlendirmeleri sayfasına öznitelik değeri:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Hayır ise `asp-controller` özniteliği belirtilmezse, geçerli görünüm yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.

Varsa `asp-action` öznitelik değeri `Index`, hiçbir eylem için varsayılan çağırma giden URL, eklenecek sonra `Index` eylem. Eylem belirtilen (veya varsayılan), başvurulan denetleyicisindeki bulunmalıdır `asp-controller`.

## <a name="asp-route-value"></a>ASP - route-{value}

[Asp - route-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) öznitelik joker karakter rota öneki sağlar. Herhangi bir değer kaplayan `{value}` yer tutucusu, olası bir rota parametresini yorumlanır. Varsayılan bir yol bulunmazsa, bu rota öneki eklenir oluşturulan `href` bir istek parametresi ve değeri olarak özniteliği. Aksi takdirde, bu rota şablonu konur.

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

## <a name="asp-route"></a>ASP yol

[Asp rota](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) öznitelik, URL'yi doğrudan adlandırılmış bir rotayı bağlama oluşturmak için kullanılır. Kullanarak [yönlendirme öznitelikleri](xref:mvc/controllers/routing#attribute-routing), gösterildiği gibi bir yol adlandırılabilir `SpeakerController` ve kullanılan kendi `Evaluations` eylem:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Aşağıdaki biçimlendirmede `asp-route` öznitelik adlandırılmış rota başvuruyor:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Yer işareti etiketi Yardımcısı doğrudan URL kullanılarak bu denetleyici eylemi bir yol oluşturur */Konuşmacı/değerlendirmeleri*. Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir. Bir rota çakışmayı önlemek için `asp-route` ile kullanılmaması `asp-controller` ve `asp-action` öznitelikleri.

## <a name="asp-all-route-data"></a>ASP tüm rota veri

[Tüm rota veri asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) özniteliği bir anahtar-değer çiftlerinin dictionary'si oluşturulmasını destekler. Anahtarı parametre adı ve değerin parametre değeri olduğu.

Aşağıdaki örnekte, bir sözlük başlatılır ve bir Razor görünüme geçirildi. Alternatif olarak, veri modelinizi oturum geçirilebilir.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Yukarıdaki kod, aşağıdaki HTML'yi oluşturur:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Sözlük Aşırı yüklenen gereksinimlerini karşılayan bir sorgu dizesi üretmek için düzleştirilir `Evaluations` eylem:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Sözlükteki tüm anahtarları rota parametrelerinin eşleşiyorsa, bu değerleri uygun şekilde rotadaki yerine kullanılır. Eşleşmeyen diğer değerleri, istek parametreleri oluşturulur.

## <a name="asp-fragment"></a>ASP parçası

[Asp parça](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) özniteliği URL'ye için URL parçası belirtmesini tanımlar. Yer işareti etiketi Yardımcısı karma karakteri ekler (#). Aşağıdaki biçimlendirmede göz önünde bulundurun:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Karma etiketleri, istemci tarafı uygulamalar oluştururken yararlı olur. Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.

## <a name="asp-area"></a>ASP alanı

[Asp alan](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) özniteliği uygun bir yol ayarlamak için kullanılan alan adını ayarlar. Aşağıdaki örnek nasıl yeniden eşleme yollarını alan özniteliği neden gösterilmektedir. Ayarı `asp-area` "Bloglarda" dizin ön ekleri *alanlar/Blog'lar* ilişkili denetleyicileri ve görünümlerinin bu yer işareti etiketi için yollar.

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

Yukarıdaki dizin hiyerarşisinin başvurmak için biçimlendirmeyi verilen *AboutBlog.cshtml* dosyasıdır:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Oluşturulan HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Varsa bir MVC uygulamasında çalışma alanları için rota şablonu alanına bir başvuru içermelidir. Bu şablon ikinci parametre tarafından temsil edilen `routes.MapRoute` yöntem çağrısı *Startup.Configure*:
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>ASP Protokolü

[Asp Protokolü](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) özniteliği, bir protokol belirtmek için (gibi `https`), URL. Örneğin:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Oluşturulan HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Ana bilgisayar adı örnekteki localhost'tur, ancak URL oluşturulurken yer işareti etiketi Yardımcısı Web sitesinin genel etki alanı kullanır.

## <a name="asp-host"></a>ASP konak

[Asp konak](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) özniteliği, bir ana bilgisayar adı, URL belirtmek için. Örneğin:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Oluşturulan HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>ASP sayfası

[Asp sayfasının](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) özniteliği, Razor sayfaları ile kullanılır. Bir yer işareti etiketin ayarlamak için kullanın `href` belirli bir sayfaya öznitelik değeri. Sayfanın adını bir eğik çizgi ("/"), URL oluşturur.

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

## <a name="asp-page-handler"></a>ASP sayfası işleyicisi

[Asp sayfasını işleyici](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) özniteliği, Razor sayfaları ile kullanılır. Belirli bir sayfaya işleyicilerine bağlamak için tasarlanmıştır.

Aşağıdaki sayfayı işleyici göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Sayfa modeli biçimlendirme bağlantılar ilişkili `OnGetProfile` sayfası işleyicisi. Unutmayın `On<Verb>` sayfa işleyicisi yöntem adı ön eki atlanırsa `asp-page-handler` öznitelik değeri. Bu zaman uyumsuz bir yöntem olsaydı `Async` soneki etmeyebilirsiniz çok.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
