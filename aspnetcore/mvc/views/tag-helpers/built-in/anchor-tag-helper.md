---
title: "Bağlantı etiketi Yardımcısı | Microsoft Docs"
author: pkellner
description: "Yer işareti etiketi Yardımcısı ile çalışmaya nasıl gösterir"
keywords: "ASP.NET Core, etiket Yardımcısı"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 86756a1d09e6e55ca79aed6e5b718088b82b782c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="anchor-tag-helper"></a>Yer işareti etiketi Yardımcısı

Tarafından [Peter Kellner](http://peterkellner.net) 

Yer işareti etiketi yardımcı HTML bağlantı geliştirir (`<a ... ></a>`) yeni öznitelikler ekleyerek etiketi. Oluşturulan bağlantı (üzerinde `href` etiketi) yeni öznitelikler kullanılarak oluşturulur. Bu URL, https gibi isteğe bağlı bir protokolü içerebilir.

Aşağıdaki Konuşmacı denetleyicisi örnekleri bu belgedeki kullanılır.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Yer işareti etiketi yardımcı öznitelik

### <a name="asp-controller"></a>ASP denetleyicisi

`asp-controller`hangi denetleyicisi URL'yi oluşturmak için kullanılan ilişkilendirmek için kullanılır. Belirtilen denetleyicileri geçerli projede mevcut olması gerekir. Aşağıdaki kod tüm konuşmacılar listeler: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

Oluşturulan biçimlendirme olacaktır:

```html
<a href="/Speaker">All Speakers</a>
```

Varsa `asp-controller` belirtilir ve `asp-action` varsayılan olarak etkin değildir, `asp-action` şu anda yürütülen görünümünün varsayılan denetleyici yöntemi olacaktır. Olduğunu, yukarıdaki örnekte ise `asp-action` çıkışı, sol ve bu bağlantı etiketi yardımcı oluşturulur *HomeController*'s `Index` Görünüm (**/ev**), oluşturulan biçimlendirme olacaktır:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>ASP eylemi

`asp-action`Eklenecek denetleyicideki eylem yöntemi adını oluşturulan içinde `href`. Örneğin, aşağıdaki kodu oluşturulan ayarlayın `href` Konuşmacı Ayrıntı Sayfası'na işaret etmek için:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

Oluşturulan biçimlendirme olacaktır:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Öyle değilse `asp-controller` özniteliği belirtilmezse, geçerli görünümü yürütme görünümü çağırma varsayılan denetleyicisi kullanılır.  
 
Öznitelik `asp-action` olan `Index`, hiçbir eylem varsayılan önde gelen URL, eklenecek sonra `Index` çağrılan yöntem. Eylem belirtilen (veya varsayılan), başvurulan denetleyicisi bulunmalıdır `asp-controller`.

### <a name="asp-page"></a>ASP sayfasının

Kullanım `asp-page` belirli bir sayfaya işaret edecek şekilde URL'sini ayarlamak için bir yer işareti etiketi özniteliği. Sayfa adı eğik çizgiyle önek "/" URL oluşturur. Aşağıdaki örnek URL'de geçerli dizin "Konuşmacı" sayfasında işaret eder.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

`asp-page` Öznitelik önceki kod örneğinde aşağıdaki kod parçacığını benzer görünümünde olan HTML çıktısı oluşturur:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

`asp-page` Özniteliği ile birbirini dışlayan `asp-route`, `asp-controller`, ve `asp-action` öznitelikleri. Ancak, `asp-page` ile kullanılan `asp-route-id` yönlendirme, aşağıdaki kod örneği gösterilmektedir olarak denetlemek için:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

`asp-route-id` Şu çıkışı üretir:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Kullanılacak `asp-page` Razor sayfalarında, URL'ler özniteliği bir göreli yol olmalıdır, örneğin `"./Speaker"`. Göreli yolda `asp-page` özniteliği MVC görünümlerde kullanılabilir değildir. "/" Sözdizimi için MVC görünümleri kullanın.

### <a name="asp-route-value"></a>ASP - rota-{value}

`asp-route-`joker karakter rota öneki ' dir. Sonda Tire olası bir rota parametresi olarak yorumlanacak sonra yerleştirdiğiniz herhangi bir değer. Varsayılan bir yol bulunmazsa, bu rota öneki oluşturulan href istek parametresi ve değeri olarak eklenir. Aksi takdirde rota şablonunda değiştirilecektir.

Varsayılmıştır denetleyici yöntemi gibi tanımladınız:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

Ve içinde tanımlanan varsayılan rota şablonuna sahip, *haline* gibi:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

**Cshtml** kullanmak için gerekli bağlantı etiket Yardımcısı içeren dosya **Konuşmacı** denetleyicisinden görünüme iletilen model parametresi aşağıdaki gibidir:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Oluşturulan HTML sonra şu şekilde olacaktır, çünkü **kimliği** varsayılan rotada bulundu.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Rota öneki bulunan yönlendirme şablonunun parçası değilse, olduğu aşağıdaki durumuyla **cshtml** dosyası:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

Oluşturulan HTML sonra şu şekilde olacaktır, çünkü **speakerid** eşleşen yol bulunamadı:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Her iki `asp-controller` veya `asp-action` de olduğu gibi aynı varsayılan işleme ardından sonra belirtilmeyen `asp-route` özniteliği.

### <a name="asp-route"></a>ASP yol

`asp-route`adlandırılmış bir rotayı bağlanan doğrudan bir URL oluşturmak için bir yol sağlar. Yönlendirme özniteliklerini kullanarak, bir rota gösterildiği şekilde adlandırılabilir `SpeakerController` ve kullanılan kendi `Evaluations` yöntemi.

`Name = "speakerevals"`bir rota URL'yi kullanarak doğrudan bu yönteme denetleyicisi oluşturmak için yer işareti etiketi yardımcı söyler `/Speaker/Evaluations`. Varsa `asp-controller` veya `asp-action` ek olarak belirtilen `asp-route`, oluşturulan rota beklediğiniz olmayabilir. `asp-route`öznitelikleri birini kullanarak kullanılmamalıdır `asp-controller` veya `asp-action` rota çakışmayı önlemek için.

### <a name="asp-all-route-data"></a>ASP tüm rota veri

`asp-all-route-data`Burada anahtar parametre adı ve değeri bu anahtarla ilişkili değeri anahtar değer çifti sözlüğü oluşturma sağlar.

Örnekteki aşağıda gösterildiği gibi bir satır içi sözlük oluşturulur ve veriler razor görünüme iletilir. Alternatif olarak, veri modeliniz oturum gönderilebilir.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

Yukarıdaki kod aşağıdaki URL'yi oluşturur: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Ne zaman bağlantısı tıklatıldığında, denetleyici yönteminin `EvaluationsCurrent` olarak adlandırılır. Bu denetleyici ne gelen oluşturuldu eşleşen iki dize parametresi olduğundan adlı `asp-all-route-data` sözlük.

Herhangi bir anahtarı sözlük eşleşme parametreleri yol varsa, bu değerleri uygun şekilde rotadaki değiştirilecektir ve diğer eşleşmeyen değerleri İstek parametreleri oluşturulur.

### <a name="asp-fragment"></a>ASP parçası

`asp-fragment`URL'ye eklemek için URL parçası tanımlar. Yer işareti etiketi yardımcı karma karakteri ekler (#). Bir etiket oluşturmak istiyorsanız:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

Oluşturulan URL olacaktır: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Karma etiketleri, istemci tarafı uygulamaları oluştururken yararlıdır. Bunlar, kolay işaretleme ve JavaScript'te, örneğin arama için kullanılabilir.

### <a name="asp-area"></a>ASP alanı

`asp-area`uygun yolu için ASP.NET Core kullanır alan adını ayarlar. Yeniden eşleme yolların alanı özniteliği nasıl neden örnekleri aşağıda verilmiştir. Ayarı `asp-area` Bloglara dizin önekleri `Areas/Blogs` ilişkili denetleyicilerinin ve görünümlerin bu yer işareti etiketi için yollar.

* Proje adı
  * wwwroot
  * Alanları
    * Bloglar
      * Denetleyiciler
        * HomeController.cs
      * Görünümler
        * Ana Sayfası
          * Index.cshtml
          * AboutBlog.cshtml
  * Denetleyiciler

Gibi geçerli bir alan etiketi belirtme ```area="Blogs"``` başvururken ```AboutBlog.cshtml``` dosya, aşağıdaki gibi görünür yer işareti etiketi Yardımcısını kullanarak.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

Oluşturulan HTML alanları segmenti içerir ve şu şekilde olacaktır:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Varsa MVC alanları bir web uygulamasında çalışmak rota şablonu alanı için bir başvuru içermelidir. İkinci parametre Bu şablon, `routes.MapRoute` yöntem çağrısı olarak görünür:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>ASP Protokolü

`asp-protocol` Bir protokolü belirtmek için (gibi `https`) URL'nizde. Bir örnek protokolünü içeren bir yer işareti etiketi yardımcı şu şekilde görünür:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

ve HTML gibi oluşturur:

```<a href="https://localhost/Home/About">About</a>```

Örnekteki etki alanı, localhost olmakla birlikte bağlantı etiket Yardımcısı Web sitesinin ortak etki alanı için URL oluşturulurken kullanır.

## <a name="additional-resources"></a>Ek kaynaklar

* [Alanlar](xref:mvc/controllers/areas)
