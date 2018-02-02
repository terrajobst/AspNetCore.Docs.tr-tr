---
title: "Yer işareti etiketi Yardımcısı"
author: pkellner
description: "ASP.NET Core yer işareti etiketi yardımcı öznitelik ve her öznitelik HTML yer işareti etiketi davranışını genişletme oynadığı rolü bulur."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 01/31/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: f3b704174c3287edda12725b7973a2464e485bac
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="anchor-tag-helper"></a>Yer işareti etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) ve [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tag-helpers/built-in/samples/TagHelpersBuiltInAspNetCore) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

[Yer işareti etiketi yardımcı](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper) standart HTML bağlantı geliştirir (`<a ... ></a>`) yeni öznitelikler ekleyerek etiketi. Kurala göre öznitelik adları ile önek `asp-`. İşlenen bağlantı öğenin `href` öznitelik değeri değerleri tarafından belirlenir `asp-` öznitelikleri.

*SpeakerController* örnekleri bu belge boyunca kullanılır:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

Bir envanterini `asp-` aşağıdaki öznitelikleri.

## <a name="asp-controller"></a>ASP denetleyicisi

[Asp denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.controller) özniteliği URL oluşturmak için kullanılan denetleyici atar. Aşağıdaki biçimlendirmede tüm konuşmacılar listeler:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspController)]

Oluşturulan HTML:

```html
<a href="/Speaker">All Speakers</a>
```

Varsa `asp-controller` özniteliği belirtilmediyse ve `asp-action` değil, varsayılan `asp-action` şu anda yürütülen görünüm ile ilişkilendirilen denetleyici eylemi bir değerdir. Varsa `asp-action` önceki biçimlendirmeden atlanır ve yer işareti etiketi yardımcı kullanılan *HomeController*'s *dizin* Görünüm (*/ev*), oluşturulan HTML:

```html
<a href="/Home">All Speakers</a>
```

## <a name="asp-action"></a>ASP eylemi

[Asp eylem](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.action) öznitelik değerini temsil eder oluşturulmuş dahil denetleyici eylem adı `href` özniteliği. Aşağıdaki biçimlendirmede oluşturulan ayarlar `href` Konuşmacı değerlendirmeleri sayfasına öznitelik değeri:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAction)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Öyle değilse `asp-controller` özniteliği belirtilmezse, geçerli görünümü yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.

Varsa `asp-action` öznitelik değeri `Index`, hiçbir eylem varsayılan çağrılması için önde gelen URL, eklenecek sonra `Index` eylem. Eylem belirtilen (veya varsayılan), başvurulan denetleyicisi bulunmalıdır `asp-controller`.

## <a name="asp-route-value"></a>asp-route-{value}

[Asp - rota-{value}](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) öznitelik joker karakter rota öneki sağlar. Herhangi bir değer kaplayan `{value}` yer tutucu, olası bir rota parametresi olarak yorumlanır. Varsayılan bir yol bulunmazsa, bu rota öneki eklenir oluşturulan `href` istek parametresi ve değeri olarak özniteliği. Aksi takdirde, rota şablonu konur.

Aşağıdaki denetleyici eylemi göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Tanımlanan varsayılan rota şablonuyla *Startup.Configure*:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

MVC görünüm gibi eylem tarafından sağlanan modeli kullanır:

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

Varsayılan rotanın `{id?}` yer tutucu eşleşti. Oluşturulan HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Rota öneki eşleşen yönlendirme şablonunun parçası değil olarak aşağıdaki MVC görünümü ile varsayın:

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

Aşağıdaki HTML çünkü oluşturulan `speakerid` eşleşen rotada bulunamadı:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

Her iki `asp-controller` veya `asp-action` de olduğu gibi aynı varsayılan işleme ardından belirtilen olmayan `asp-route` özniteliği.

## <a name="asp-route"></a>ASP yol

[Asp rota](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.route) özniteliği, bir URL bir adlandırılmış rota doğrudan bağlama oluşturmak için kullanılır. Kullanarak [yönlendirme öznitelikleri](xref:mvc/controllers/routing#attribute-routing), bir rota gösterildiği gibi adlı `SpeakerController` ve kullanılan kendi `Evaluations` eylem:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=22-24)]

Aşağıdaki biçimlendirmede `asp-route` özniteliği adlandırılmış rota başvuruyor:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Yer işareti etiketi yardımcı doğrudan URL'yi kullanarak bu denetleyici eylemi için bir yol oluşturur */Konuşmacı/değerlendirmeleri*. Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir. Bir rota çakışmayı önlemek için `asp-route` ile kullanılmaması `asp-controller` ve `asp-action` öznitelikleri.

## <a name="asp-all-route-data"></a>asp-all-route-data

[Tüm rota veri asp](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.routevalues) özniteliği, anahtar-değer çiftleri sözlüğü oluşturulmasını destekler. Parametre adı anahtarıdır ve değerin parametre değeridir.

Aşağıdaki örnekte, bir sözlük başlatıldı ve Razor görünümüne geçirildi. Alternatif olarak, veri modeliniz oturum geçirilen.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Yukarıdaki kod aşağıdaki HTML oluşturur:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` Aşırı yüklenmiş gereksinimlerini karşılayan bir sorgu dizesi üretmek için Sözlük düzleştirilmiş `Evaluations` eylem:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs?range=26-30)]

Sözlükteki tüm anahtarları rota parametrelerinin eşleşiyorsa, bu değerleri uygun şekilde rotadaki yerine kullanılır. Diğer eşleşmeyen değerleri İstek parametreleri üretilir.

## <a name="asp-fragment"></a>ASP parçası

[Asp parça](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.fragment) özniteliği URL'si eklemek için URL parçası tanımlar. Yer işareti etiketi yardımcı karma karakteri ekler (#). Aşağıdaki biçimlendirmede göz önünde bulundurun:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Karma etiketleri, istemci-tarafı uygulamaları oluştururken yararlıdır. Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.

## <a name="asp-area"></a>asp-area

[Asp alanı](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.area) öznitelik ayarlar uygun yol ayarlamak için kullanılan alan adı. Aşağıdaki örnek, alan özniteliği yeniden eşleme yolların nasıl neden gösterilmektedir. Ayarı `asp-area` "Bloglara" dizin önekleri *alanları/blog* ilişkili denetleyicilerinin ve görünümlerin bu yer işareti etiketi için yollar.

* **< proje adı\>**
  * **wwwroot**
  * **Alanlar**
    * **Bloglar**
      * **Denetleyiciler**
        * *HomeController.cs*
      * **Görünümler**
        * **Giriş**
          * *AboutBlog.cshtml*
          * *Index.cshtml*
        * *_ViewStart.cshtml*
  * **Denetleyiciler**

Yukarıdaki dizin hiyerarşisinin başvurmak için biçimlendirme verilen *AboutBlog.cshtml* dosyasıdır:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspArea)]

Oluşturulan HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> Varsa bir MVC uygulamasında çalışmaya alanları için rota şablonu alanı için bir başvuru içermelidir. Bu şablon ikinci parametre tarafından temsil edilen `routes.MapRoute` yöntem çağrısı *Startup.Configure*:[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Startup.cs?name=snippet_UseMvc&highlight=5)]

## <a name="asp-protocol"></a>ASP Protokolü

[Asp Protokolü](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.protocol) özniteliği olan bir protokolü belirtmek için (gibi `https`) URL'nizde. Örneğin:

[!code-cshtml[samples/TagHelpersBuiltInAspNetCore/Views/Index.cshtml?name=snippet_AspProtocol]]

Oluşturulan HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Localhost ana bilgisayar adıdır örnekteki ancak yer işareti etiketi yardımcı Web sitesinin ortak etki alanı için URL oluşturulurken kullanır.

## <a name="asp-host"></a>ASP ana bilgisayar

[Asp konak](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.host) özniteliktir, URL'de bir ana bilgisayar adını belirtmek için. Örneğin:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspHost)]

Oluşturulan HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

## <a name="asp-page"></a>ASP sayfasının

[Asp sayfasının](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.page) özniteliği Razor sayfalarıyla kullanılır. Bir bağlantı etiketinin ayarlamak için kullanın `href` belirli bir sayfaya öznitelik değeri. Sayfa adı bir eğik çizgi ("/") ile önek URL oluşturur.

Aşağıdaki örnek, katılımcı Razor sayfasını noktaları:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPage)]

Oluşturulan HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` Özniteliği ile birbirini dışlayan `asp-route`, `asp-controller`, ve `asp-action` öznitelikleri. Ancak, `asp-page` ile kullanılan `asp-route-{value}` yönlendirme, aşağıdaki biçimlendirme gösterdiği gibi denetlemek için:

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

## <a name="asp-page-handler"></a>ASP sayfası işleyicisi

[Asp sayfası işleyici](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.anchortaghelper.pagehandler) özniteliği Razor sayfalarıyla kullanılır. Belirli bir sayfaya işleyicilerine bağlama için tasarlanmıştır.

Aşağıdaki sayfayı işleyicisini göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltInAspNetCore/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Sayfa modeli biçimlendirme bağlantılar ilişkili `OnGetProfile` sayfası işleyicisi. Unutmayın `On<Verb>` sayfa işleyici yöntemi adı öneki atlanırsa `asp-page-handler` öznitelik değeri. Bu zaman uyumsuz bir yöntem olsaydı `Async` soneki etmeyebilirsiniz çok.

[!code-cshtml[](samples/TagHelpersBuiltInAspNetCore/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Alanlar](xref:mvc/controllers/areas)
* [Razor sayfalarının giriş](xref:mvc/razor-pages/index)
