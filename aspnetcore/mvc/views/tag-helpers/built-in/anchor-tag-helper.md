---
title: ASP.NET Core 'de bağlayıcı etiketi Yardımcısı
author: pkellner
description: ASP.NET Core tutturucu etiketi yardımcı özniteliklerini ve her bir özniteliğin, HTML bağlantısı etiketinin genişletme davranışında oynadığı rolü bulur.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/13/2019
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 6bfbad39115c7823b5677d3c52ca64cfb0683037
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664005"
---
# <a name="anchor-tag-helper-in-aspnet-core"></a>ASP.NET Core 'de bağlayıcı etiketi Yardımcısı

By [Peter Kellner](https://peterkellner.net) ve [Scott Ade](https://github.com/scottaddie)

[Tutturucu etiketi Yardımcısı](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper) , yeni öznitelikler ekleyerek standart HTML bağlayıcısı (`<a ... ></a>`) etiketini geliştirir. Kurala göre, öznitelik adlarına ön eki eklenmiş olarak `asp-`. İşlenmiş tutturucu öğenin `href` özniteliği değeri, `asp-` özniteliklerinin değerleriyle belirlenir.

Etiket Yardımcıları hakkında genel bilgi için bkz. <xref:mvc/views/tag-helpers/intro>.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/built-in/samples) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

*Speakercontroller* bu belgenin tamamında örneklerde kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?name=snippet_SpeakerController)]

## <a name="anchor-tag-helper-attributes"></a>Tutturucu etiketi yardımcı öznitelikleri

### <a name="asp-controller"></a>ASP-Controller

[ASP-Controller](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Controller*) ÖZNITELIĞI, URL oluşturmak için kullanılan denetleyiciyi atar. Aşağıdaki biçimlendirme tüm hoparlörleri listeler:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspController)]

Oluşturulan HTML:

```html
<a href="/Speaker">All Speakers</a>
```

`asp-controller` özniteliği belirtilirse ve `asp-action` değilse, varsayılan `asp-action` değeri şu anda yürütülmekte olan görünüm ile ilişkili denetleyici eylemi olur. Önceki işaretten `asp-action` atlanırsa ve *HomeController*'ın *Dizin* görünümünde ( */Home*) tutturucu etiketi Yardımcısı kullanılırsa, oluşturulan HTML:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>ASP-eylem

[ASP-Action](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Action*) öznitelik değeri, oluşturulan `href` özniteliğinde bulunan denetleyici eylemi adını temsil eder. Aşağıdaki biçimlendirme, üretilen `href` özniteliği değerini konuşmacı değerlendirmeleri sayfasına ayarlar:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAction)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

`asp-controller` öznitelik belirtilmemişse, geçerli görünümü yürüten görünümü çağıran varsayılan denetleyici kullanılır.

`asp-action` öznitelik değeri `Index`ise, URL 'ye hiçbir eylem eklenmez ve bu, varsayılan `Index` eyleminin çağrılması için önde gelen bir eylem değildir. Belirtilen eylem (veya varsayılan olarak) `asp-controller`içinde başvurulan denetleyicide bulunmalıdır.

### <a name="asp-route-value"></a>ASP-Route-{Value}

[ASP-Route-{Value}](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) özniteliği bir joker karakter yolu öneki sunar. `{value}` yer tutucusunu kullanan her türlü değer olası bir yol parametresi olarak yorumlanır. Varsayılan bir yol bulunmazsa, bu rota öneki oluşturulan `href` özniteliğine bir istek parametresi ve değeri olarak eklenir. Aksi takdirde, yol şablonunda değiştirilir.

Aşağıdaki denetleyici eylemini göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/BuiltInTagController.cs?name=snippet_AnchorTagHelperAction)]

Başlangıçta tanımlanmış varsayılan bir yol şablonuyla *. Yapılandır*:

[!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=8-10)]

MVC görünümü, aşağıdaki gibi eylem tarafından belirtilen modeli kullanır:

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

Varsayılan yolun `{id?}` yer tutucusu eşleşti. Oluşturulan HTML:

```html
<a href="/Speaker/Detail/12">SpeakerId: 12</a>
```

Aşağıdaki MVC görünümünde olduğu gibi, yol ön ekinin eşleşen yönlendirme şablonunun bir parçası olmadığını varsayın:

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

Eşleşen rotada `speakerid` bulunamadığı için aşağıdaki HTML oluşturuldu:

```html
<a href="/Speaker/Detail?speakerid=12">SpeakerId: 12</a>
```

`asp-controller` veya `asp-action` belirtilmemişse, aynı varsayılan işleme, `asp-route` özniteliğinde olduğu gibi olur.

### <a name="asp-route"></a>ASP-Route

[ASP-Route](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Route*) özniteliği, doğrudan adlandırılmış bir yola bağlanan bir URL oluşturmak için kullanılır. [Yönlendirme özniteliklerini](xref:mvc/controllers/routing#attribute-routing)kullanarak, bir yol `SpeakerController` gösterildiği gibi adlandırılabilir ve `Evaluations` eyleminde kullanılır:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=22-24)]

Aşağıdaki biçimlendirmede, `asp-route` özniteliği adlandırılmış yola başvurur:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspRoute)]

Tutturucu etiketi Yardımcısı, */Hoparlörker/değerlendirmeleri*URL 'sini kullanarak bu denetleyiciye doğrudan bir yol oluşturur. Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations">Speaker Evaluations</a>
```

`asp-route`ek olarak `asp-controller` veya `asp-action` belirtilirse, oluşturulan yol beklediğiniz şekilde olmayabilir. Yol çakışmasını önlemek için, `asp-controller` ve `asp-action` öznitelikleriyle kullanılmamalıdır `asp-route`.

### <a name="asp-all-route-data"></a>ASP-All-Route-Data

[ASP-All-Route-Data](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.RouteValues*) özniteliği, anahtar-değer çiftleri sözlüğü oluşturmayı destekler. Anahtar parametre adıdır ve değer parametre değeridir.

Aşağıdaki örnekte, bir sözlük başlatılmış ve Razor görünümüne geçirilir. Alternatif olarak, veriler modelinize iletilebilir.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAllRouteData)]

Yukarıdaki kod, aşağıdaki HTML 'yi oluşturur:

```html
<a href="/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true">Speaker Evaluations</a>
```

`asp-all-route-data` sözlüğü, aşırı yüklenmiş `Evaluations` eyleminin gereksinimlerini karşılayan bir QueryString oluşturmak üzere düzleştirilir:

[!code-csharp[](samples/TagHelpersBuiltIn/Controllers/SpeakerController.cs?range=26-30)]

Sözlükteki herhangi bir anahtar yol parametreleriyle eşleşiyorsa, bu değerler rotada uygun şekilde değiştirilir. Diğer eşleşme olmayan değerler istek parametreleri olarak oluşturulur.

### <a name="asp-fragment"></a>ASP-Fragment

[ASP-Fragment](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Fragment*) özniteliği URL 'ye eklenecek URL parçasını tanımlar. Tutturucu etiketi Yardımcısı, karma karakterini (#) ekler. Aşağıdaki biçimlendirmeyi göz önünde bulundurun:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspFragment)]

Oluşturulan HTML:

```html
<a href="/Speaker/Evaluations#SpeakerEvaluations">Speaker Evaluations</a>
```

Karma Etiketler, istemci tarafı uygulamalar oluşturulurken faydalıdır. JavaScript 'te kolay işaretlemek ve aramak için kullanılabilirler.

### <a name="asp-area"></a>ASP-alanı

[ASP-Area](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Area*) özniteliği, uygun yolu ayarlamak için kullanılan alan adını ayarlar. Aşağıdaki örneklerde `asp-area` özniteliğinin yolların yeniden eşleştirmesine neden olduğu gösterilmektedir.

#### <a name="usage-in-razor-pages"></a>Razor Pages kullanımı

Razor Pages alan ASP.NET Core 2,1 veya sonraki sürümlerde desteklenir.

Aşağıdaki dizin hiyerarşisini göz önünde bulundurun:

* **{Proje adı}**
  * **Wwwroot**
  * **Alanlar**
    * **Bağlanabilecek**
      * **Sayfalar**
        * *\_ViewStart. cshtml*
        * *Index. cshtml*
        * *Index.cshtml.cs*
  * **Sayfalar**

*Oturum* alanı *dizini* Razor sayfasına başvurmak için yapılan biçimlendirme şunlardır:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspAreaRazorPages)]

Oluşturulan HTML:

```html
<a href="/Sessions">View Sessions</a>
```

> [!TIP]
> Razor Pages uygulamasındaki bölgeleri desteklemek için `Startup.ConfigureServices`' de aşağıdakilerden birini yapın:
>
> * [Uyumluluk sürümünü](xref:mvc/compatibility-version) 2,1 veya üzeri olarak ayarlayın.
> * [RazorPagesOptions. AllowAreas](xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions.AllowAreas*) özelliğini `true`olarak ayarlayın:
>
>   [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_AllowAreas)]

#### <a name="usage-in-mvc"></a>MVC 'de kullanım

Aşağıdaki dizin hiyerarşisini göz önünde bulundurun:

* **{Proje adı}**
  * **Wwwroot**
  * **Alanlar**
    * **Bloglar**
      * **Denetleyiciler**
        * *HomeController.cs*
      * **Görünümler**
        * **Sayfa**
          * *AboutBlog. cshtml*
          * *Index. cshtml*
        * *\_ViewStart. cshtml*
  * **Denetleyiciler**

`asp-area` "blogları" olarak ayarlamak, Dizin *alanları/bloglarını* bu tutturucu etiketi için ilişkili denetleyicilerin ve görünümlerin yollarına ön ekler. *Aboutblog* görünümüne başvurmak için yapılan biçimlendirme şu şekilde yapılır:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspArea)]

Oluşturulan HTML:

```html
<a href="/Blogs/Home/AboutBlog">About Blog</a>
```

> [!TIP]
> MVC uygulamasındaki bölgeleri desteklemek için, yol şablonu varsa alana bir başvuru içermelidir. Bu şablon, başlangıçta `routes.MapRoute` yöntemi çağrısının ikinci parametresiyle temsil edilir *. configure*:
>
> [!code-csharp[](samples/TagHelpersBuiltIn/Startup.cs?name=snippet_UseMvc&highlight=5)]

### <a name="asp-protocol"></a>ASP-protokol

[ASP-Protocol](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Protocol*) ÖZNITELIĞI, URL 'nize bir protokol (örneğin, `https`) belirtmektir. Örnek:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspProtocol)]

Oluşturulan HTML:

```html
<a href="https://localhost/Home/About">About</a>
```

Örnekteki ana bilgisayar adı localhost 'tur. Tutturucu etiketi Yardımcısı, URL oluştururken Web sitesinin ortak etki alanını kullanır.

### <a name="asp-host"></a>ASP-ana bilgisayar

[ASP-Host](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Host*) ÖZNITELIĞI, URL 'niz için bir ana bilgisayar adı belirtmektir. Örnek:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspHost)]

Oluşturulan HTML:

```html
<a href="https://microsoft.com/Home/About">About</a>
```

### <a name="asp-page"></a>asp-sayfa

[ASP-Page](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.Page*) özniteliği Razor Pages ile kullanılır. Bir bağlayıcı etiketinin `href` öznitelik değerini belirli bir sayfaya ayarlamak için kullanın. Sayfa adının eğik çizgiyle ("/") önek olarak URL 'SI oluşturulur.

Aşağıdaki örnek, katılımcı Razor sayfasına işaret eder:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPage)]

Oluşturulan HTML:

```html
<a href="/Attendee">All Attendees</a>
```

`asp-page` özniteliği `asp-route`, `asp-controller`ve `asp-action` öznitelikleriyle birbirini dışlıyor. Ancak, aşağıdaki biçimlendirmede gösterildiği gibi `asp-page` yönlendirmeyi denetlemek için `asp-route-{value}` ile birlikte kullanılabilir:

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageAspRouteId)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=10">View Attendee</a>
```

### <a name="asp-page-handler"></a>ASP-Page-Handler

[ASP-Page-Handler](xref:Microsoft.AspNetCore.Mvc.TagHelpers.AnchorTagHelper.PageHandler*) özniteliği Razor Pages ile birlikte kullanılır. Belirli sayfa işleyicileriyle bağlantı için tasarlanmıştır.

Aşağıdaki sayfa işleyicisini göz önünde bulundurun:

[!code-csharp[](samples/TagHelpersBuiltIn/Pages/Attendee.cshtml.cs?name=snippet_OnGetProfileHandler)]

Sayfa modelinin ilişkili biçimlendirmesi `OnGetProfile` sayfa işleyicisine bağlanır. Burada, sayfa işleyicisi yöntem adının `On<Verb>` ön eki `asp-page-handler` öznitelik değerinde atlanacaktır. Yöntem zaman uyumsuz olduğunda, `Async` soneki de atlanır.

[!code-cshtml[](samples/TagHelpersBuiltIn/Views/Home/Index.cshtml?name=snippet_AspPageHandler)]

Oluşturulan HTML:

```html
<a href="/Attendee?attendeeid=12&handler=Profile">Attendee Profile</a>
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
