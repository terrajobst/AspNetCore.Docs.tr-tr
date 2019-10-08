---
title: ASP.NET Core Razor Pages giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 61b1c3a17b378524c8fea9004b615c2d3d480135
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007465"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages giriş

::: moniker range=">= aspnetcore-3.0"

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)

Razor Pages, kodlama sayfasına odaklanmış senaryolar denetleyicileri ve görünümleri kullanmaktan daha kolay ve daha üretken hale getirebilirsiniz.

Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).

Bu belge Razor Pages bir giriş sağlar. Adım adım öğretici değildir. Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start). ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>Razor Pages projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Komut satırından `dotnet new webapp` komutunu çalıştırın.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Komut satırından `dotnet new webapp` komutunu çalıştırın.

Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.

---

## <a name="razor-pages"></a>Razor Pages

Razor Pages, *Startup.cs*'de etkinleştirilmiştir:

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12)]

Temel bir sayfa düşünün:<a name="OnGet"></a>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar. Bunu farklı kılan [@page](xref:mvc/views/razor#page) yönergedir. `@page`, dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir. `@page` bir sayfada ilk Razor yönergesi olmalıdır. `@page`, diğer [Razor](xref:mvc/views/razor) yapıları davranışını etkiler. Razor Pages dosya adlarında *. cshtml* soneki vardır.

@No__t-0 sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir. *Pages/Index2. cshtml* dosyası:

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2. cshtml. cs* sayfa modeli:

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir. Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir. @No__t-0 sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.

URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir. Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:

| Dosya adı ve yolu               | eşleşen URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` veya `/Index` |
| */Pages/Contact.exe* | `/Contact` |
| */Pages/Store/Contact.exe* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` veya `/Store/Index` |

Notlar:

* Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.
* URL, bir sayfa içermiyorsa varsayılan sayfa olan `Index` ' dır.

## <a name="write-a-basic-form"></a>Temel form yazma

Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır. [Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* . @No__t-0 modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:

Bu belgedeki örneklerde, `DbContext` [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) dosyasında başlatılır.

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

Veri modeli:

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

DB bağlamı:

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

*Pages/Create. cshtml* görünüm dosyası:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

*Pages/Create. cshtml. cs* sayfa modeli:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.

@No__t-0 sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar. Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar. Bu ayrım şunları sağlar:

* [Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.
* [Birim testi](xref:test/razor-pages-tests)

Sayfada, @no__t 2 isteklerinde çalışan (bir Kullanıcı formu gönderdiğinde) `OnPostAsync` *işleyicisi yöntemi*vardır. Herhangi bir HTTP fiili için işleyici metotları eklenebilir. En yaygın işleyiciler şunlardır:

* sayfa için gereken durumu başlatmak için `OnGet`. Yukarıdaki kodda `OnGet` yöntemi *CreateModel. cshtml* Razor sayfasını görüntüler.
* form gönderilerini işlemek için `OnPost`.

@No__t-0 adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır. Yukarıdaki kod Razor Pages için tipik bir davranıştır.

Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:

* Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.
* [Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu denetleyiciler ve Razor Pages aynı şekilde çalışır. 

Önceki `OnPostAsync` yöntemi:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

@No__t temel akışı-0:

Doğrulama hatalarını kontrol edin.

* Hata yoksa, verileri kaydedin ve yeniden yönlendirin.
* Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin. Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.

*Pages/Create. cshtml* görünüm dosyası:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

Sayfalardan işlenmiş HTML */Create. cshtml*:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

Önceki kodda, formu deftere nakletme:

* Geçerli verilerle:

  * @No__t-0 işleyicisi yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> yardımcı yöntemini çağırır. `RedirectToPage` <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult> ' in bir örneğini döndürür. `RedirectToPage`:

    * Bir eylem sonucudur.
    * @No__t-0 veya `RedirectToRoute` ' e benzerdir (denetleyiciler ve görünümlerde kullanılır).
    * Sayfalar için özelleştirilir. Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir. `RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.

* Sunucuya geçirilen doğrulama hatalarıyla birlikte:

  * @No__t-0 işleyicisi yöntemi <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> yardımcı yöntemini çağırır. `Page` <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> ' in bir örneğini döndürür. @No__t-0 döndürme, denetleyicilerde bulunan eylemlerin `View` ' i döndürme biçimine benzer. `PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür. @No__t-0 döndüren bir işleyici yöntemi sayfayı işler.
  * Yukarıdaki örnekte, formun hiçbir değer olmadan nakledilmesi [ModelState ile sonuçlanır. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) yanlış döndürüyor. Bu örnekte, istemcide hiçbir doğrulama hatası gösterilmezler. Doğrulama hatası teslim etme bu belgenin ilerleyen bölümlerinde ele alınmıştır.

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* İstemci tarafı doğrulaması tarafından algılanan doğrulama hatalarıyla birlikte:

  * Veriler sunucuya **nakledilmedi.**
  * İstemci tarafı doğrulaması bu belgenin ilerleyen kısımlarında açıklanmıştır.

@No__t-0 özelliği, model bağlamasını kabul etmek için [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğini kullanır:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

`[BindProperty]`, istemci tarafından **değiştirilmemesi gereken özellikler** içeren modellerde kullanılmamalıdır. Daha fazla bilgi için bkz. fazla [nakil](xref:data/ef-rp/crud#overposting).

Razor Pages, varsayılan olarak, özellikleri yalnızca @no__t olmayan-0 olan fiiller ile bağlayın. Özelliklere bağlama, HTTP verilerini model türüne dönüştürmek için kod yazma ihtiyacını ortadan kaldırır. Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.

[!INCLUDE[](~/includes/bind-get.md)]

*Sayfalar/oluşturma. cshtml* görünüm dosyası gözden geçiriliyor:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* Yukarıdaki kodda, `<input asp-for="Customer.Name" />` [giriş etiketi Yardımcısı](xref:mvc/views/working-with-forms#the-input-tag-helper) , HTML `<input>` öğesini `Customer.Name` model ifadesine bağlar.
* [`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) etiket yardımcılarını kullanılabilir hale getirir.

### <a name="the-home-page"></a>Giriş sayfası

*Index. cshtml* giriş sayfasıdır:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

*Index. cshtml* dosyası aşağıdaki biçimlendirmeyi içerir:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

@No__t-0 [bağlantı etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı. Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir. Örneğin, `https://localhost:5001/Edit/1`. [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir.

*Index. cshtml* dosyası her müşteri için bir silme düğmesi oluşturmak için biçimlendirme içerir:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

İşlenmiş HTML:

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

Sil düğmesi HTML 'de işlendiğinde, bu nesnenin [biçimlendirme](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) parametreleri içerir:

* @No__t-0 özniteliğiyle belirtilen müşteri iletişim KIMLIĞI.
* @No__t-1 özniteliğiyle belirtilen `handler`.

Düğme seçildiğinde, sunucuya `POST` isteği gönderilir. Kural gereği, işleyici yönteminin adı, `OnPost[handler]Async` düzenine göre `handler` parametresinin değerine göre seçilir.

Bu örnekte `handler` `delete` olduğundan, `POST` isteğini işlemek için `OnPostDeleteAsync` işleyici yöntemi kullanılır. @No__t-0, `remove` gibi farklı bir değere ayarlanmışsa, `OnPostRemoveAsync` adlı bir işleyici yöntemi seçilidir.

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

`OnPostDeleteAsync` Yöntemi:

* Sorgu dizesinden `id` alır.
* @No__t-0 ile müşteri iletişim için veritabanını sorgular.
* Müşteri ilgili kişisi bulunursa, kaldırılır ve veritabanı güncelleştirilir.
* Kök dizin sayfasına yeniden yönlendirmek için <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> çağırır (`/Index`).

### <a name="the-editcshtml-file"></a>Edit. cshtml dosyası

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

İlk satır `@page "{id:int}"` yönergesini içerir. {No__t-0 Yönlendirme kısıtlaması, sayfaya `int` rota verileri içeren sayfaya istekleri kabul etmesini söyler. Sayfaya yapılan bir istek bir `int` ' a dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür. KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:

 ```cshtml
@page "{id:int?}"
```

*Edit.cshtml.cs* dosyası:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a>Doğrulama

Doğrulama kuralları:

* Model sınıfında bildirimli olarak belirtilir.
* Uygulamada her yerde zorlanır.

@No__t-0 ad alanı, bir sınıfa veya özelliğe bildirimli olarak uygulanan bir yerleşik doğrulama öznitelikleri kümesi sağlar. Veri açıklamaları, biçimlendirme ile yardım eden [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) gibi biçimlendirme özniteliklerini de içerir ve herhangi bir doğrulama sağlamaz.

@No__t-0 modelini göz önünde bulundurun:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Aşağıdaki *Create. cshtml* görünüm dosyasını kullanarak:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

Yukarıdaki kod:

* JQuery ve jQuery doğrulama betikleri içerir.
* Etkinleştirmek için `<div />` ve `<span />` [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) kullanır:

  * İstemci tarafı doğrulama.
  * Doğrulama hatası işleme.

* Aşağıdaki HTML 'yi oluşturur:

  [!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

Create formunu ad değeri olmadan göndermek "ad alanı gereklidir" hata iletisini görüntüler. formunda. İstemcide JavaScript etkinse tarayıcı, sunucuya göndermeden hatayı görüntüler.

@No__t-0 özniteliği işlenmiş HTML üzerinde `data-val-length-max="10"` oluşturur. `data-val-length-max`, tarayıcıların belirtilen uzunluk üst sınırından daha fazlasını girmesini engeller. Gönderiyi düzenlemek ve yeniden oynatmak için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanılıyorsa:

* , Adı 10 ' dan daha uzun.
* "Alan adı, en fazla 10 uzunluğunda bir dize olmalıdır" hata iletisi. döndürülür.

Aşağıdaki `Movie` modelini göz önünde bulundurun:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

Doğrulama öznitelikleri, uygulanan model özellikleri üzerinde zorlamak için davranışı belirtir:

* @No__t-0 ve `MinimumLength` öznitelikleri bir özelliğin bir değere sahip olması gerektiğini belirtir, ancak hiçbir şey, kullanıcının bu doğrulamayı karşılamak için boşluk girmesini engeller.
* @No__t-0 özniteliği, hangi karakterlerin girişi yapabileceğini sınırlamak için kullanılır. Yukarıdaki kodda, "tarz":

  * Yalnızca harfler kullanılmalıdır.
  * İlk harfin büyük harfle olması gerekir. Boşluk, sayı ve özel karakterlere izin verilmez.

* @No__t-0 "derecelendirmesi":

  * İlk karakterin büyük harf olmasını gerektirir.
  * Sonraki boşlukların içindeki özel karakter ve sayılara izin verir. "PG-13" bir derecelendirme için geçerlidir, ancak bir "tarz" için başarısız olur.

* @No__t-0 özniteliği, bir değeri belirtilen bir Aralık içinde kısıtlar.
* @No__t-0 özniteliği bir dize özelliğinin en büyük uzunluğunu ve isteğe bağlı olarak en düşük uzunluğunu ayarlar.
* Değer türleri (örneğin `decimal`, `int`, `float`, `DateTime`), doğal olarak gereklidir ve `[Required]` özniteliğine gerek kalmaz.

@No__t-0 modelinin Oluştur sayfası, geçersiz değerlere sahip hataları gösterir:

![Birden çok jQuery istemci tarafı doğrulama hatası içeren film görünümü formu](~/tutorials/razor-pages/validation/_static/val.png)

Daha fazla bilgi için bkz.

* [Film uygulamasına doğrulama ekleme](xref:tutorials/razor-pages/validation)
* [ASP.NET Core 'de model doğrulaması](xref:mvc/models/validation).

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri

`HEAD` istekleri belirli bir kaynağın üst bilgilerini almaya izin verir. @No__t-0 isteklerinin aksine, `HEAD` istekleri yanıt gövdesi döndürmez.

Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

Razor Pages, `OnHead` işleyicisi tanımlanmamışsa `OnGet` işleyicisini çağırmaya geri döner.

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF ve Razor Pages

Razor Pages, [Antiforgery doğrulaması](xref:security/anti-request-forgery)tarafından korunur. [Formtaghelper](xref:mvc/views/working-with-forms#the-form-tag-helper) , antiforgery belirteçlerini HTML form öğelerine çıkartır.

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma

Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır. Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*ve *_Viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.

Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.

*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

[Düzen](xref:mvc/views/layout):

* Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).
* JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.
* Razor sayfasının içerikleri `@RenderBody()` çağrıldığında işlenir.

Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout).

[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

Düzen *Sayfalar/paylaşılan* klasöründedir. Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar. *Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.

Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.

Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** . *Görünümler/paylaşılan* bir MVC görünümleri modelidir. Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.

Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir. MVC denetleyicileri ve geleneksel Razor görünümleriyle kullanılan düzenler, şablonlar ve partilar *yalnızca çalışır*.

*Pages/_Viewwimports. cshtml* dosyası ekleyin:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` daha sonra öğreticide açıklanmıştır. @No__t-0 yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.

<a name="namespace"></a>

Bir sayfada ayarlanan `@namespace` yönergesi:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

@No__t-0 yönergesi sayfanın ad alanını ayarlar. @No__t-0 yönergesinin ad alanını içermesi gerekmez.

@No__t-0 yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar. Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.

Örneğin, `PageModel` sınıf *sayfaları/müşteriler/Edit. cshtml. cs* açıkça ad alanını ayarlar:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.

`@namespace` *geleneksel Razor görünümleriyle de kullanılabilir.*

*Pages/Create. cshtml* görünüm dosyasını göz önünde bulundurun:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası *_Viewwimports. cshtml* ve önceki düzen dosyası:

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

Yukarıdaki kodda, *_Viewwimports. cshtml* ad alanını ve etiket yardımcıları içeri aktardı. Düzen dosyası JavaScript dosyalarını içeri aktardı.

[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.

Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Sayfalar için URL oluşturma

Daha önce gösterilen `Create` sayfası `RedirectToPage` kullanır:

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

Uygulama aşağıdaki dosya/klasör yapısına sahiptir:

* */Pages*

  * *Index.cshtml*
  * *Gizlilik. cshtml*
  * */Customers*

    * *. Cshtml oluştur*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *sayfaları/müşterileri/Index. cshtml* 'ye yeniden yönlendirir. @No__t-0 dizesi, önceki sayfaya erişmek için kullanılan göreli bir sayfa adıdır. *Pages/Customers/Index. cshtml* sayfasının URL 'leri oluşturmak için kullanılır. Örneğin:

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

@No__t-0 mutlak sayfa adı, *Sayfalar/Index. cshtml* sayfasına URL 'ler oluşturmak için kullanılır. Örneğin:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur. Önceki URL oluşturma örnekleri, bir URL 'YI sabit kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar. URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.

Sayfalar için URL oluşturma göreli adları destekler. Aşağıdaki tabloda, *sayfalarda/müşteriler/Create. cshtml*'de farklı `RedirectToPage` parametreleri kullanılarak hangi dizin sayfasının seçildiği gösterilmektedir.

| RedirectToPage (x)| Sayfasında |
| ----------------- | ------------ |
| RedirectToPage ("/Index") | *Sayfa/dizin* |
| RedirectToPage ("./Index"); | *Sayfalar/müşteriler/Dizin* |
| RedirectToPage (".. /İndex ") | *Sayfa/dizin* |
| RedirectToPage ("Dizin")  | *Sayfalar/müşteriler/Dizin* |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("../Index")` *göreli adlardır*. @No__t-0 parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .

Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur. Bir klasördeki sayfalar arasında bağlantı için göreli adlar kullanıldığında:

* Bir klasörü yeniden adlandırmak, göreli bağlantıları bozmaz.
* Klasör adını içermediği için bağlantılar kopuk değildir.

Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

Daha fazla bilgi için bkz. <xref:mvc/controllers/areas> ve <xref:razor-pages/razor-pages-conventions>.

## <a name="viewdata-attribute"></a>ViewData özniteliği

Veriler, <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute> ile bir sayfaya geçirilebilir. @No__t-0 özniteliğiyle birlikte bulunan özellikler, değerleri <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary> ' den depolanır ve yüklenir.

Aşağıdaki örnekte `AboutModel`, `Title` özelliğine `[ViewData]` özniteliğini uygular:

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:

```cshtml
<h1>@Model.Title</h1>
```

Mizanpajda, başlık ViewData sözlüğünden okundu:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a>TempData

ASP.NET Core, @no__t gösterir. Bu özellik, okunana kadar verileri depolar. @No__t-0 ve <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> yöntemleri, silme yapılmadan verileri incelemek için kullanılabilir. `TempData`, tek bir istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.

Aşağıdaki kod, `TempData` kullanarak `Message` değerini ayarlar:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `Message` değerini `TempData` kullanarak görüntüler.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.

```cs
[TempData]
public string Message { get; set; }
```

Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata).

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>Sayfa başına birden çok işleyici

Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır. @No__t-0 özniteliği, `asp-page` ' e yönelik bir yardımcı olur. `asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur. örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.

Sayfa modeli:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır. Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ' dan sonra ve `Async` ' den (varsa) önce, ad içindeki metin alınarak oluşturulur. Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir. *Onpost* ve *Async* kaldırılmış olarak, işleyici adları `JoinList` ve `JoinListUC` ' dir.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList` ' dir. @No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` ' dir.

## <a name="custom-routes"></a>Özel yollar

@No__t-0 yönergesini kullanarak şunları yapın:

* Sayfaya özel bir yol belirtin. Örneğin, hakkında sayfasının yolu `@page "/Some/Other/Path"` ile `/Some/Other/Path` olarak ayarlanabilir.
* Kesimleri bir sayfanın varsayılan yoluna ekleyin. Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"` ile eklenebilir.
* Bir sayfanın varsayılan yoluna parametreleri ekleyin. Örneğin, `id` olan bir ID parametresi, `@page "{id}"` içeren bir sayfa için gerekli olabilir.

Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir. Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"` ' dir.

URL 'deki `?handler=JoinList` sorgu dizesini, `@page "{handler?}"` yol şablonunu belirterek, `/JoinList` bir yol kesimine değiştirebilirsiniz.

URL 'de `?handler=JoinList` sorgu dizesini beğenmezseniz, bu yolu URL 'nin yol bölümüne işleyici adını koymak için değiştirebilirsiniz. @No__t-0 yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList` ' dir. @No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC` ' dir.

@No__t-0 `handler`, Route parametresinin isteğe bağlı olduğu anlamına gelir.

## <a name="advanced-configuration-and-settings"></a>Gelişmiş yapılandırma ve ayarlar

Aşağıdaki bölümlerdeki yapılandırma ve ayarlar çoğu uygulama için gerekli değildir.

Gelişmiş seçenekleri yapılandırmak için uzantı yöntemini kullanın <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

Sayfalar için kök dizini ayarlamak için <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> kullanın veya sayfalar için uygulama modeli kuralları ekleyin. Kurallar hakkında daha fazla bilgi için bkz. [Razor Pages yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization).

Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation).

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Razor Pages içerik kökünde olduğunu belirtin

Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır. Razor Pages uygulamanın (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) [içerik kökünde](xref:fundamentals/index#content-root) olduğunu belirtmek için <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> ekleyin:

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Razor Pages özel kök dizinde olduğunu belirtin

Razor Pages uygulamada bir özel kök dizinde olduğunu belirtmek için @no__t ekleyin-0 (göreli bir yol sağlayın):

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a>Ek kaynaklar

* Bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), bu giriş hakkında derleme
* [Örnek kodu indirme veya görüntüleme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan şimdi ak](https://github.com/rynowak)

Razor Pages, kod odaklı senaryoları daha kolay ve daha üretken hale getiren ASP.NET Core MVC 'nin yeni bir yönüdür.

Model-View-Controller yaklaşımını kullanan bir öğretici arıyorsanız, bkz. [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).

Bu belge Razor Pages bir giriş sağlar. Adım adım öğretici değildir. Bölümlerden bazılarını çok gelişmiş bir şekilde bulursanız, bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start). ASP.NET Core genel bir bakış için bkz. [ASP.NET Core giriş](xref:index).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a>Razor Pages projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Razor Pages projesi oluşturma hakkında ayrıntılı yönergeler için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) .

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Komut satırından `dotnet new webapp` komutunu çalıştırın.

Oluşturulan *. csproj* dosyasını Mac için Visual Studio açın.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Komut satırından `dotnet new webapp` komutunu çalıştırın.

---

## <a name="razor-pages"></a>Razor Pages

Razor Pages, *Startup.cs*'de etkinleştirilmiştir:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Temel bir sayfa düşünün:<a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Yukarıdaki kod, denetleyiciler ve görünümlerle ASP.NET Core bir uygulamada kullanılan [Razor görünüm dosyası](xref:tutorials/first-mvc-app/adding-view) gibi bir çok şey arar. Bu, farklı kılan `@page` yönergedir. `@page`, dosyayı bir MVC eylemine dönüştürür. Bu, bir denetleyiciden geçmeden istekleri doğrudan işlediği anlamına gelir. `@page` bir sayfada ilk Razor yönergesi olmalıdır. `@page`, diğer Razor yapıları davranışını etkiler.

@No__t-0 sınıfı kullanan benzer bir sayfa aşağıdaki iki dosyada gösterilmiştir. *Pages/Index2. cshtml* dosyası:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2. cshtml. cs* sayfa modeli:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Kurala göre `PageModel` sınıf dosyası, *. cs* eklenmiş Razor sayfası dosyasıyla aynı ada sahiptir. Örneğin, önceki Razor sayfası *Pages/Index2. cshtml*' dir. @No__t-0 sınıfını içeren dosya *sayfa/Index2. cshtml. cs*olarak adlandırılır.

URL yollarının sayfalara olan ilişkilendirmeleri, sayfanın dosya sistemindeki konumuna göre belirlenir. Aşağıdaki tabloda bir Razor sayfa yolu ve eşleşen URL gösterilmektedir:

| Dosya adı ve yolu               | eşleşen URL |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` veya `/Index` |
| */Pages/Contact.exe* | `/Contact` |
| */Pages/Store/Contact.exe* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` veya `/Store/Index` |

Notlar:

* Çalışma zamanı, *Sayfalar* klasöründeki Razor Pages dosyaları varsayılan olarak arar.
* URL, bir sayfa içermiyorsa varsayılan sayfa olan `Index` ' dır.

## <a name="write-a-basic-form"></a>Temel form yazma

Razor Pages, Web tarayıcıları ile kullanılan ortak desenleri bir uygulama oluştururken kolayca uygulanması için tasarlanmıştır. [Model bağlama](xref:mvc/models/model-binding), [ETIKET yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML Yardımcıları hepsi, Razor sayfası sınıfında tanımlanan özelliklerle *çalışır* . @No__t-0 modeli için temel bir "bize başvurun" formu uygulayan bir sayfa düşünün:

Bu belgedeki örneklerde, `DbContext` [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosyasında başlatılır.

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Veri modeli:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

DB bağlamı:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create. cshtml* görünüm dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create. cshtml. cs* sayfa modeli:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Kurala göre `PageModel` sınıfı `<PageName>Model` olarak adlandırılır ve sayfayla aynı ad alanında yer alan.

@No__t-0 sınıfı, bir sayfanın mantığının sunumuna ayrılmasını sağlar. Sayfaya gönderilen istekler için sayfa işleyicilerini ve sayfayı işlemek için kullanılan verileri tanımlar. Bu ayrım şunları sağlar:

* [Bağımlılık ekleme](xref:fundamentals/dependency-injection)yoluyla sayfa bağımlılıklarını yönetme.
* Sayfaların [birim testi](xref:test/razor-pages-tests) .

Sayfada, @no__t 2 isteklerinde çalışan (bir Kullanıcı formu gönderdiğinde) `OnPostAsync` *işleyicisi yöntemi*vardır. Herhangi bir HTTP fiili için işleyici yöntemleri ekleyebilirsiniz. En yaygın işleyiciler şunlardır:

* sayfa için gereken durumu başlatmak için `OnGet`. [OnGet](#OnGet) örneği.
* form gönderilerini işlemek için `OnPost`.

@No__t-0 adlandırma son eki isteğe bağlıdır, ancak genellikle zaman uyumsuz işlevler için kural tarafından kullanılır. Yukarıdaki kod Razor Pages için tipik bir davranıştır.

Denetleyicileri ve görünümleri kullanarak ASP.NET uygulamaları hakkında bilginiz varsa:

* Yukarıdaki örnekteki `OnPostAsync` kodu, tipik denetleyici koduna benzer şekilde görünür.
* [Model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), [doğrulama](xref:mvc/models/validation)ve eylem sonuçları gibi mvc temel elemanlarının çoğu paylaşılır.

Önceki `OnPostAsync` yöntemi:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

@No__t temel akışı-0:

Doğrulama hatalarını kontrol edin.

* Hata yoksa, verileri kaydedin ve yeniden yönlendirin.
* Hatalar varsa, doğrulama iletileriyle sayfayı yeniden görüntüleyin. İstemci tarafı doğrulaması geleneksel ASP.NET Core MVC uygulamalarıyla aynıdır. Çoğu durumda, istemci üzerinde doğrulama hataları algılanır ve sunucuya hiçbir zaman gönderilmez.

Veriler başarıyla girildiğinde, `OnPostAsync` işleyicisi yöntemi bir `RedirectToPageResult` örneğini döndürmek için `RedirectToPage` yardımcı yöntemini çağırır. `RedirectToPage`, `RedirectToAction` veya `RedirectToRoute` ' ye benzer ancak sayfalar için özelleştirilen yeni bir eylem sonucudur. Önceki örnekte, kök dizin sayfasına (`/Index`) yeniden yönlendirir. `RedirectToPage`, [Sayfalar Için URL oluşturma](#url_gen) bölümünde ayrıntılı olarak açıklanmıştır.

Gönderilen formda doğrulama hataları olduğunda (sunucuya geçilen), @ no__t-0 işleyici yöntemi `Page` yardımcı yöntemini çağırır. `Page` `PageResult` ' in bir örneğini döndürür. @No__t-0 döndürme, denetleyicilerde bulunan eylemlerin `View` ' i döndürme biçimine benzer. `PageResult`, bir işleyici yöntemi için varsayılan dönüş türüdür. @No__t-0 döndüren bir işleyici yöntemi sayfayı işler.

@No__t-0 özelliği, model bağlamasını kabul etmek için `[BindProperty]` özniteliğini kullanır.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Razor Pages, varsayılan olarak, özellikleri yalnızca @no__t olmayan-0 olan fiiller ile bağlayın. Özelliklere bağlama, yazmanız gerektiğini kodun miktarını azaltabilir. Bağlama, form alanlarını işlemek için aynı özelliği kullanarak kodu azaltır (`<input asp-for="Customer.Name">`) ve girişi kabul eder.

[!INCLUDE[](~/includes/bind-get.md)]

Giriş sayfası (*Index. cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index. cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak üzere aşağıdaki biçimlendirmeyi içerir:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

@No__t-0 [bağlantı etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) , düzenleme sayfasına bir bağlantı oluşturmak için `asp-route-{value}` özniteliğini kullandı. Bağlantı, iletişim KIMLIĞINE sahip rota verileri içerir. Örneğin, `https://localhost:5001/Edit/1`. [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro), Razor dosyalarında HTML öğelerinin oluşturulmasına ve işlenmesine sunucu tarafı kodun katılmasını etkinleştir. Etiket Yardımcıları @no__t tarafından etkinleştirilir-0

*Pages/Edit. cshtml* dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

İlk satır `@page "{id:int}"` yönergesini içerir. {No__t-0 Yönlendirme kısıtlaması, sayfaya `int` rota verileri içeren sayfaya istekleri kabul etmesini söyler. Sayfaya yapılan bir istek bir `int` ' a dönüştürülebileceği rota verileri içermiyorsa, çalışma zamanı bir HTTP 404 (bulunamadı) hatası döndürür. KIMLIĞI isteğe bağlı yapmak için, yol kısıtlamasına `?` ekleyin:

 ```cshtml
@page "{id:int?}"
```

*Pages/Edit. cshtml. cs* dosyası:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index. cshtml* dosyası, her müşteri kişisi için bir silme düğmesi oluşturmak için de biçimlendirme içerir:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Sil düğmesi HTML 'de işlendiğinde, `formaction` ' ın parametreleri şunları içerir:

* @No__t-0 özniteliği tarafından belirtilen müşteri iletişim KIMLIĞI.
* @No__t-1 özniteliği tarafından belirtilen `handler`.

Müşteri irtibat KIMLIĞI `1` olan işlenmiş silme düğmesine bir örnek aşağıda verilmiştir:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Düğme seçildiğinde, sunucuya `POST` isteği gönderilir. Kural gereği, işleyici yönteminin adı, `OnPost[handler]Async` düzenine göre `handler` parametresinin değerine göre seçilir.

Bu örnekte `handler` `delete` olduğundan, `POST` isteğini işlemek için `OnPostDeleteAsync` işleyici yöntemi kullanılır. @No__t-0, `remove` gibi farklı bir değere ayarlanmışsa, `OnPostRemoveAsync` adlı bir işleyici yöntemi seçilidir. Aşağıdaki kod `OnPostDeleteAsync` işleyicisini gösterir:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Yöntemi:

* Sorgu dizesinden `id` kabul eder. *Index. cshtml* sayfa yönergesi yönlendirme kısıtlaması içeriyorsa `"{id:int?}"`, `id` rota verilerinden gelir. @No__t-0 ' a ait rota verileri, URI 'de `https://localhost:5001/Customers/2` gibi belirtilir.
* @No__t-0 ile müşteri iletişim için veritabanını sorgular.
* Müşteri ilgili kişisi bulunursa, bunlar müşteri kişileri listesinden kaldırılır. Veritabanı güncelleştirildi.
* Kök dizin sayfasına yeniden yönlendirmek için `RedirectToPage` çağırır (`/Index`).

## <a name="mark-page-properties-as-required"></a>Sayfa özelliklerini gerektiği gibi işaretle

@No__t-0 ' a ait özellikler [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) öznitelikle birlikte kullanılabilir:

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Daha fazla bilgi için bkz. [model doğrulaması](xref:mvc/models/validation).

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a>OnGet işleyicisi geri dönüşü ile tanıtıcı HEAD istekleri

`HEAD` istekleri belirli bir kaynağın üst bilgilerini almanıza olanak sağlar. @No__t-0 isteklerinin aksine, `HEAD` istekleri yanıt gövdesi döndürmez.

Normalde, `HEAD` istekleri için `OnHead` işleyicisi oluşturulur ve çağırılır: 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

ASP.NET Core 2,1 veya sonraki bir sürümde, hiçbir `OnHead` işleyicisi tanımlanmamışsa Razor Pages `OnGet` işleyicisini çağırmaya geri döner. Bu davranış, `Startup.ConfigureServices` ' de [Setcompatibilityversion](xref:mvc/compatibility-version) çağrısıyla etkinleştirilir:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

Varsayılan Şablonlar, ASP.NET Core 2,1 ve 2,2 ' de `SetCompatibilityVersion` çağrısını üretir. `SetCompatibilityVersion` `AllowMappingHeadRequestsToGetHandler` Razor Pages seçeneğini `true` ' ye etkin şekilde ayarlar.

@No__t-0 ile tüm davranışlardan çıkmak yerine, açıkça *belirli* davranışları kabul edebilirsiniz. Aşağıdaki kod, `HEAD` isteklerinin `OnGet` işleyicisine eşlenmesine izin vermek için ' de kullanılır:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF ve Razor Pages

[Antiforgery doğrulaması](xref:security/anti-request-forgery)için herhangi bir kod yazmanız gerekmez. Antiforgery belirteci oluşturma ve doğrulama, Razor Pages otomatik olarak eklenir.

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Razor Pages ile düzenleri, partileri, şablonları ve etiket yardımcılarını kullanma

Sayfalar, Razor görünüm altyapısının tüm özellikleri ile çalışır. Düzenler, partıals, şablonlar, etiket yardımcıları, *_Viewstart. cshtml*, *_viewwimports. cshtml* geleneksel Razor görünümlerinde oldukları gibi çalışır.

Bu özelliklerden bazılarının avantajlarından yararlanarak bu sayfayı declutter edelim.

*Pages/Shared/_Layout. cshtml*öğesine bir [Düzen sayfası](xref:mvc/views/layout) ekleyin:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Düzen](xref:mvc/views/layout):

* Her sayfanın yerleşimini denetler (sayfa düzen dışında değilse).
* JavaScript ve stil sayfaları gibi HTML yapılarını içeri aktarır.

Daha fazla bilgi için bkz. [Düzen sayfası](xref:mvc/views/layout) .

[Layout](xref:mvc/views/layout#specifying-a-layout) özelliği *Pages/_viewstart. cshtml*içinde ayarlanır:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

Düzen *Sayfalar/paylaşılan* klasöründedir. Sayfalar, geçerli sayfayla aynı klasörden başlayarak diğer görünümleri (düzenler, şablonlar, parals) hiyerarşik olarak arar. *Sayfalar/paylaşılan* klasördeki bir düzen, *Sayfalar* klasörü altındaki herhangi bir Razor sayfasından kullanılabilir.

Düzen dosyası *Sayfalar/paylaşılan* klasörüne gitmelidir.

Düzen dosyasını *Görünümler/paylaşılan* klasöre **yerleştirmenizi öneririz** . *Görünümler/paylaşılan* bir MVC görünümleri modelidir. Razor Pages, yol kurallarını değil klasör hiyerarşisine güvenmektir.

Bir Razor sayfasından arama görüntüleme, *Sayfalar* klasörünü içerir. MVC denetleyicileri ve geleneksel Razor görünümleriyle kullandığınız düzenler, şablonlar ve parals işlemleri *yalnızca çalışır*.

*Pages/_Viewwimports. cshtml* dosyası ekleyin:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` daha sonra öğreticide açıklanmıştır. @No__t-0 yönergesi, [yerleşik etiket yardımcılarını](xref:mvc/views/tag-helpers/builtin-th/Index) *Sayfalar* klasöründeki tüm sayfalara getirir.

<a name="namespace"></a>

@No__t-0 yönergesi açıkça bir sayfada kullanıldığında:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Yönergesi sayfanın ad alanını ayarlar. @No__t-0 yönergesinin ad alanını içermesi gerekmez.

@No__t-0 yönergesi *_Viewwimports. cshtml*içinde yer aldığında, belirtilen ad alanı, `@namespace` yönergesini Içeri aktaran sayfada oluşturulan ad alanı için ön ek sağlar. Oluşturulan ad alanının geri kalanı (sonek bölümü), *_Viewwimports. cshtml* dosyasını ve sayfayı içeren klasörü içeren, noktayla ayrılmış göreli yoldur.

Örneğin, `PageModel` sınıf *sayfaları/müşteriler/Edit. cshtml. cs* açıkça ad alanını ayarlar:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_Viewwimports. cshtml* dosyası aşağıdaki ad alanını ayarlar:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

*Pages/Customers/Edit. cshtml* Razor sayfasının oluşturulan ad alanı `PageModel` sınıfıyla aynıdır.

`@namespace` *geleneksel Razor görünümleriyle de kullanılabilir.*

Özgün *Sayfalar/Create. cshtml* görünüm dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Güncelleştirilmiş *Sayfalar/Create. cshtml* görünüm dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Razor Pages Başlatıcı projesi](#rpvs17) , istemci tarafı doğrulamayı bağlayan *sayfaları/_ValidationScriptsPartial. cshtml*'yi içerir.

Kısmi görünümler hakkında daha fazla bilgi için bkz. <xref:mvc/views/partial>.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Sayfalar için URL oluşturma

Daha önce gösterilen `Create` sayfası `RedirectToPage` kullanır:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Uygulama aşağıdaki dosya/klasör yapısına sahiptir:

* */Pages*

  * *Index.cshtml*
  * */Customers*

    * *. Cshtml oluştur*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create. cshtml* ve *Pages/Customers/Edit. cshtml* sayfaları, başarılı olduktan sonra *Pages/Index. cshtml* dosyasına yönlendirilir. @No__t-0 dizesi, önceki sayfaya erişmek için URI 'nin bir parçasıdır. @No__t-0 dizesi, *Sayfalar/Index. cshtml* sayfasına URI 'ler oluşturmak için kullanılabilir. Örneğin:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Sayfa adı, kök */Pages* klasöründeki, önde gelen `/` (örneğin, `/Index`) içeren sayfanın yoludur. Önceki URL oluşturma örnekleri bir URL 'YI kodlamadan gelişmiş seçenekler ve işlevsel yetenekler sunar. URL oluşturma [yönlendirme](xref:mvc/controllers/routing) kullanır ve yolun hedef yolda nasıl tanımlandığınıza göre parametreleri oluşturabilir ve kodlayabilir.

Sayfalar için URL oluşturma göreli adları destekler. Aşağıdaki tabloda, *sayfa/müşteri/oluşturma. cshtml*'den farklı `RedirectToPage` parametreleriyle hangi dizin sayfasının seçildiği gösterilmektedir:

| RedirectToPage (x)| Sayfasında |
| ----------------- | ------------ |
| RedirectToPage ("/Index") | *Sayfa/dizin* |
| RedirectToPage ("./Index"); | *Sayfalar/müşteriler/Dizin* |
| RedirectToPage (".. /İndex ") | *Sayfa/dizin* |
| RedirectToPage ("Dizin")  | *Sayfalar/müşteriler/Dizin* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")` ve `RedirectToPage("../Index")` , *göreli adlardır*. @No__t-0 parametresi, hedef sayfanın adını hesaplamak için geçerli sayfanın yoluyla *birleştirilir* .  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Karmaşık bir yapıya sahip siteler oluştururken göreli ad bağlama yararlı olur. Bir klasördeki sayfalar arasında bağlantı sağlamak için göreli adlar kullanırsanız, bu klasörü yeniden adlandırabilirsiniz. Tüm bağlantılar hala çalışır (klasör adını içermediği için).

Farklı bir [alandaki](xref:mvc/controllers/areas)bir sayfaya yeniden yönlendirmek için alanını belirtin:

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

Daha fazla bilgi için bkz. <xref:mvc/controllers/areas>.

## <a name="viewdata-attribute"></a>ViewData özniteliği

Veri, [Viewdataattribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute)içeren bir sayfaya geçirilebilir. @No__t-0 ile donatılmış denetleyicilerde veya Razor sayfa modellerindeki özelliklerin değerleri, [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)'den depolanır ve yüklenir.

Aşağıdaki örnekte, `AboutModel` `[ViewData]` ile donatılmış bir `Title` özelliği içerir. @No__t-0 özelliği hakkında sayfasının başlığına ayarlanır:

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

Hakkında sayfasında, `Title` özelliğine model özelliği olarak erişin:

```cshtml
<h1>@Model.Title</h1>
```

Mizanpajda, başlık ViewData sözlüğünden okundu:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a>TempData

ASP.NET Core bir [denetleyicide](/dotnet/api/microsoft.aspnetcore.mvc.controller) [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliğini kullanıma sunar. Bu özellik, okunana kadar verileri depolar. @No__t-0 ve `Peek` yöntemleri, silme yapılmadan verileri incelemek için kullanılabilir. `TempData`, tek bir istekten daha fazla veri gerektiğinde yeniden yönlendirme için kullanışlıdır.

Aşağıdaki kod, `TempData` kullanarak `Message` değerini ayarlar:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

*Pages/Customers/Index. cshtml* dosyasında aşağıdaki biçimlendirme `Message` değerini `TempData` kullanarak görüntüler.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index. cshtml. cs* sayfa modeli, `[TempData]` özniteliğini `Message` özelliğine uygular.

```cs
[TempData]
public string Message { get; set; }
```

Daha fazla bilgi için bkz. [TempData](xref:fundamentals/app-state#tempdata) .

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a>Sayfa başına birden çok işleyici

Aşağıdaki sayfa `asp-page-handler` etiketi Yardımcısını kullanarak iki işleyici için biçimlendirme oluşturur:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Yukarıdaki örnekteki formda, her biri farklı bir URL 'ye göndermek için `FormActionTagHelper` kullanan iki gönderme düğmesi vardır. @No__t-0 özniteliği, `asp-page` ' e yönelik bir yardımcı olur. `asp-page-handler`, bir sayfa tarafından tanımlanan her bir işleyici yöntemini gönderen URL 'Ler oluşturur. örnek geçerli sayfaya bağlandığından `asp-page` belirtilmedi.

Sayfa modeli:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Yukarıdaki kod, *adlandırılmış işleyici yöntemlerini*kullanır. Adlandırılmış işleyici yöntemleri, `On<HTTP Verb>` ' dan sonra ve `Async` ' den (varsa) önce, ad içindeki metin alınarak oluşturulur. Yukarıdaki örnekte, Page metotları OnPost**Joinlist**Async ve onpost**Joinlıstuc**Async ' dir. *Onpost* ve *Async* kaldırılmış olarak, işleyici adları `JoinList` ve `JoinListUC` ' dir.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinList` ' dir. @No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` ' dir.

## <a name="custom-routes"></a>Özel yollar

@No__t-0 yönergesini kullanarak şunları yapın:

* Sayfaya özel bir yol belirtin. Örneğin, hakkında sayfasının yolu `@page "/Some/Other/Path"` ile `/Some/Other/Path` olarak ayarlanabilir.
* Kesimleri bir sayfanın varsayılan yoluna ekleyin. Örneğin, bir "öğe" segmenti sayfanın varsayılan yoluna `@page "item"` ile eklenebilir.
* Bir sayfanın varsayılan yoluna parametreleri ekleyin. Örneğin, `id` olan bir ID parametresi, `@page "{id}"` içeren bir sayfa için gerekli olabilir.

Yolun başındaki bir tilde (`~`) tarafından atanan kök göreli bir yol desteklenir. Örneğin, `@page "~/Some/Other/Path"` `@page "/Some/Other/Path"` ' dir.

URL 'deki `?handler=JoinList` sorgu dizesini, `@page "{handler?}"` yol şablonunu belirterek, `/JoinList` bir yol kesimine değiştirebilirsiniz.

URL 'de `?handler=JoinList` sorgu dizesini beğenmezseniz, bu yolu URL 'nin yol bölümüne işleyici adını koymak için değiştirebilirsiniz. @No__t-0 yönergesinden sonra çift tırnak içine alınmış bir rota şablonu ekleyerek yolu özelleştirebilirsiniz.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Yukarıdaki kodu kullanarak, `OnPostJoinListAsync` ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinList` ' dir. @No__t-0 ' a gönderen URL yolu `https://localhost:5001/Customers/CreateFATH/JoinListUC` ' dir.

@No__t-0 `handler`, Route parametresinin isteğe bağlı olduğu anlamına gelir.

## <a name="configuration-and-settings"></a>Yapılandırma ve ayarlar

Gelişmiş seçenekleri yapılandırmak için, MVC Oluşturucu 'da-0 @no__t genişletme yöntemini kullanın:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Şu anda, sayfalar için kök dizini ayarlamak üzere `RazorPagesOptions` ' ı kullanabilir veya sayfalar için uygulama modeli kuralları ekleyebilirsiniz. Gelecekte bu şekilde daha fazla genişletilebilirlik etkinleştireceğiz.

Görünümleri önceden derlemek için bkz. [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .

[Örnek kodu indirin veya görüntüleyin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).

Bu giriş hakkında bilgi için bkz. [Razor Pages kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start).

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Razor Pages içerik kökünde olduğunu belirtin

Varsayılan olarak, Razor Pages */Pages* dizininde kök olarak depolanır. Razor Pages, uygulamanın [içerik kökünde](xref:fundamentals/index#content-root) ([contentrootpath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) olduğunu belirtmek Için [addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) ekleyin:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Razor Pages özel kök dizinde olduğunu belirtin

Razor Pages uygulamadaki özel bir kök dizinde olduğunu belirtmek için [Addmvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 'ye [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) ekleyin (göreli bir yol sağlayın):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
