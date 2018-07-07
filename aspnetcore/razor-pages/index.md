---
title: ASP.NET Core Razor sayfalar giriş
author: Rick-Anderson
description: Nasıl ASP.NET Core Razor sayfalar kodlama sayfa odaklı senaryolar daha kolay ve MVC kullanmaktan daha üretken hale getirdiğini öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/12/2018
uid: razor-pages/index
ms.openlocfilehash: 49bed6cc150a74ff8b72848f276c55c2490b6fa5
ms.sourcegitcommit: a09820f91e71a7d98b7347bf93210abb9e995e22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889148"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor sayfalar giriş

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Ryan Nowak](https://github.com/rynowak)

Razor sayfaları, ASP.NET Core MVC sayfası odaklı senaryolar daha kolay ve daha üretken kodlama sağlayan yeni bir yönü olur.

Model-View-Controller yaklaşım kullanan bir öğretici için arıyorsanız bkz [ASP.NET Core MVC ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).

Bu belge, Razor sayfaları için bir giriş sağlar. Bir adım adım öğretici değil. Bazı bölümlerinin çok Gelişmiş bulma olmadığını [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start). ASP.NET Core genel bakış için bkz. [ASP.NET Core'a giriş](xref:index).

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Razor sayfaları proje oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start) Visual Studio kullanarak Razor sayfaları oluşturma hakkında ayrıntılı yönergeler için proje.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.1"

Çalıştırma `dotnet new webapp` komut satırından.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Çalıştırma `dotnet new razor` komut satırından.

::: moniker-end

Oluşturulan açın *.csproj* Mac için Visual Studio'dan dosyası

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

::: moniker range=">= aspnetcore-2.1"

Çalıştırma `dotnet new webapp` komut satırından.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Çalıştırma `dotnet new razor` komut satırından.

::: moniker-end

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

::: moniker range=">= aspnetcore-2.1"

Çalıştırma `dotnet new webapp` komut satırından.

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Çalıştırma `dotnet new razor` komut satırından.

::: moniker-end

---

## <a name="razor-pages"></a>Razor sayfaları

Razor sayfaları etkin *Startup.cs*:

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Temel sayfa göz önünde bulundurun: <a name="OnGet"></a>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Yukarıdaki kod, çok Razor görünüm dosyası gibi görünüyor. Neler farklı kılan unsurdur `@page` yönergesi. `@page` Dosya, istekleri doğrudan bir denetleyici geçmeden işleme anlamına gelir ve MVC eyleme - yapar. `@page` ilk Razor yönergesi bir sayfa üzerinde olmalıdır. `@page` diğer Razor yapıları davranışını etkiler.

Benzer bir sayfa, kullanarak bir `PageModel` sınıfında, aşağıdaki iki dosyada gösterilir. *Pages/Index2.cshtml* dosyası:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

*Pages/Index2.cshtml.cs* sayfa modeli:

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Kural olarak, `PageModel` sınıf dosyası Razor sayfası dosyası ile aynı ada sahip *.cs* eklenir. Örneğin, önceki Razor sayfa *Pages/Index2.cshtml*. Dosyayı içeren `PageModel` sınıf adlı *Pages/Index2.cshtml.cs*.

URL yolu sayfalara ilişkilendirmelerini dosya sisteminde sayfanın konuma göre belirlenir. Aşağıdaki tabloda bir Razor sayfası ile eşleşen bir URL gösterilmektedir:

| Dosya adı ve yolu               | URL eşleştirme |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` veya `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` veya `/Store/Index` |

Notlar:

* Razor sayfaları dosyalarında çalışma zamanı arar *sayfaları* varsayılan klasör.
* `Index` bir URL bir sayfa içermediğinde varsayılan sayfasıdır.

## <a name="writing-a-basic-form"></a>Temel bir form yazma

Razor sayfaları web tarayıcıları ile kolay bir uygulama oluştururken uygulamak için kullanılan ortak desenler hale getirmek için tasarlanmıştır. [Model bağlama](xref:mvc/models/model-binding), [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro)ve HTML yardımcılarını tüm *yalnızca iş* bir Razor sayfası sınıfta tanımlanan özelliklere sahip. "Bize başvurun" oluşturmak için temel bir uygulayan bir sayfa göz önünde bulundurun `Contact` modeli:

Bu belgedeki örnekler için `DbContext` içinde başlatılan [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) dosya.

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Veri modeli:

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

Db bağlamı:

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

*Pages/Create.cshtml* dosyasını görüntüle:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

*Pages/Create.cshtml.cs* sayfa modeli:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Kural olarak, `PageModel` sınıfı çağrıldığında `<PageName>Model` ve aynı ad alanında sayfası.

`PageModel` Sınıf kendi sunudan sayfasının mantığının ayrımı sağlar. Bu sayfa işleyicileri sayfasına gönderilen istekleri ve sayfayı oluşturmak için kullanılan verileri tanımlar. Bu ayırma sayfasında bağımlılıkları üzerinden yönetmenizi sağlar [bağımlılık ekleme](xref:fundamentals/dependency-injection) ve [birim testi](xref:test/razor-pages-tests) sayfaları.

Sayfanın bir `OnPostAsync` *işleyicisi yöntemi*, üzerinde çalıştığı `POST` ister (kullanıcı formu gönderdiğinde). Herhangi bir HTTP fiil için işleyici yöntemleri ekleyebilirsiniz. En yaygın işleyicileri şunlardır:

* `OnGet` sayfa için gerekli durumu başlatılamadı. [OnGet](#OnGet) örnek.
* `OnPost` Form gönderilerini görselleştirip işlemek için.

`Async` Adlandırma soneki isteğe bağlıdır, ancak genellikle kurala göre zaman uyumsuz işlevleri için kullanılır. `OnPostAsync` Kod önceki örnekte ne, normalde bir denetleyicisi yazmalısınız için benzer görünür. Yukarıdaki kod, Razor sayfaları için tipik bir durumdur. MVC temelleri çoğu ister [model bağlama](xref:mvc/models/model-binding), [doğrulama](xref:mvc/models/validation), ve eylem sonuçlarını paylaşılır.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Önceki `OnPostAsync` yöntemi:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Temel akışı `OnPostAsync`:

Doğrulama hataları denetleyin.

*  Herhangi bir hata varsa, verileri kaydetmek ve yönlendirin.
*  Hatalar varsa sayfası yeniden doğrulama iletileri göster. İstemci tarafı doğrulama geleneksel ASP.NET Core MVC uygulamaları için aynıdır. Çoğu durumda, doğrulama hataları istemcide algılandı ve hiçbir zaman sunucuya teslim.

Verileri başarıyla girildiğinde `OnPostAsync` işleyicisi yöntem çağrılarını `RedirectToPage` örneği döndürmek için yardımcı yöntem `RedirectToPageResult`. `RedirectToPage` Yeni eylem sonucu, benzer olan `RedirectToAction` veya `RedirectToRoute`, ancak özelleştirilmiş sayfalar için. İsteğe bağlı olarak önceki örnekte, kök dizin sayfasına yönlendirir (`/Index`). `RedirectToPage` içinde ayrıntılı [sayfaları için URL üretimi](#url_gen) bölümü.

Gönderilen bir formu, (yani sunucuya geçirilir) doğrulama hataları olduğunda`OnPostAsync` işleyicisi yöntem çağrılarını `Page` yardımcı yöntemi. `Page` örneği döndürür `PageResult`. Döndüren `Page` denetleyicileri eylemleri nasıl döndürmek için benzer `View`. `PageResult` Varsayılan değer <!-- Review  --> dönüş türü için bir işleyici yöntemi. Döndürür bir işleyici yönteminin `void` sayfasını işler.

`Customer` Özelliği kullanan `[BindProperty]` model bağlama için katılım için özniteliği.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Razor sayfaları varsayılan olarak, GET olmayan fiilleri yalnızca özelliklerle bağlayın. Özellikleri bağlama yazmanız gereken kod miktarını azaltır. Form alanlarını işlemek için aynı özellik kullanarak kod azaltır bağlama (`<input asp-for="Customer.Name" />`) ve giriş kabul edin.

> [!NOTE]
> Güvenlik nedenleriyle, sayfa modeli özellikleri için GET isteği veri bağlaması kabul gerekir. Özellikleri için eşleme önce kullanıcı girişi doğrulayın. Bu davranış için seçim olan sorgu dizesi veya rota değerlerini senaryoları belirtirken yararlıdır.
>
> Bir özellik üzerinde GET istekleri bağlamak için ayarlanmış `[BindProperty]` özniteliğin `SupportsGet` özelliğini `true`: `[BindProperty(SupportsGet = true)]`

Giriş sayfası (*Index.cshtml*):

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

İlişkili `PageModel` sınıfı (*Index.cshtml.cs*):

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

*Index.cshtml* dosyası her kişi için bir düzenleme bağlantısı oluşturmak için aşağıdaki biçimlendirme içerir:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Yer işareti etiketi Yardımcısı](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) kullanılan `asp-route-{value}` düzenleme sayfasına giden bir bağlantı oluşturmak için öznitelik. Rota verilerini kişiyle bağlantısını içeren kimliği Örneğin, `http://localhost:5000/Edit/1`.

*Pages/Edit.cshtml* dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

İlk satırında `@page "{id:int}"` yönergesi. Yönlendirme kısıtlaması`"{id:int}"` sayfasına içeren isteklerini kabul etmek için sayfayı söyler `int` rota verileri. Sayfasına bir istek dönüştürülebilir rota verilerini içermiyorsa, bir `int`, çalışma zamanı HTTP 404 (bulunamadı) hatası döndürür. Kimliği isteğe bağlı yapmak için URL'ye `?` rota kısıtlaması için:

 ```cshtml
@page "{id:int?}"
```

*Pages/Edit.cshtml.cs* dosyası:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

*Index.cshtml* dosyası ayrıca Sil düğmesini her müşteri iletişim bilgileri için oluşturulacak işaretleme içerir:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Sil düğmesini HTML işlendiğinde, `formaction` parametreleri içerir:

* Müşteri Kimliği tarafından belirtilen iletişime `asp-route-id` özniteliği.
* `handler` Tarafından belirtilen `asp-page-handler` özniteliği.

İşte bir müşteri bir işlenen Sil düğmesini bir örnek kimliği başvurun `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Düğme seçildiğinde, bir form `POST` isteği sunucuya gönderilir. Kural gereği, işleyici yönteminin adı seçili değerine göre `handler` parametre düzeni göre `OnPost[handler]Async`.

Çünkü `handler` olduğu `delete` Bu örnekte, `OnPostDeleteAsync` işleyicisi yöntemi kullanılır işleme `POST` isteği. Varsa `asp-page-handler` gibi farklı bir değere ayarlanmış `remove`, ada sahip bir sayfa işleyicisi yöntemi `OnPostRemoveAsync` seçilir.

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

`OnPostDeleteAsync` Yöntemi:

* Kabul `id` Sorgu dizesinden.
* Veritabanı ile müşteri iletişim bilgileri için sorgular `FindAsync`.
* Müşteri irtibat bulunursa, müşteri kişiler listesinden kaldırıldı. Veritabanı güncelleştirilir.
* Çağrıları `RedirectToPage` kök dizin sayfasına yeniden yönlendirmek için (`/Index`).

::: moniker range=">= aspnetcore-2.1"

## <a name="mark-page-properties-required"></a>Gerekli işareti sayfa özellikleri

Özellikleri bir `PageModel` ile donatılmış [gerekli](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) özniteliği:

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

Bkz: [Model doğrulama](xref:mvc/models/validation) daha fazla bilgi için.

## <a name="manage-head-requests-with-the-onget-handler"></a>HEAD isteklerini OnGet işleyici ile yönetme

Normalde, bir baş işleyici oluşturulur ve HEAD isteklerini çağrılır:

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

Hiçbir baş işleyici (`OnHead`) olan tanımlanan, Razor sayfaları geri alma sayfası işleyicisi çağırma için döner (`OnGet`) ASP.NET Core 2.1 veya üzeri. Bu davranışı ile kabul etmek [SetCompatibilityVersion yöntemi](xref:fundamentals/startup#setcompatibilityversion-for-aspnet-core-mvc) içinde `Startup.Configure` 2.x için ASP.NET Core 2.1 için:

```csharp
services.AddMvc()
    .SetCompatibilityVersion(Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1);
```

`SetCompatibilityVersion` Razor sayfaları seçeneği etkili bir şekilde ayarlar `AllowMappingHeadRequestsToGetHandler` için `true`.

Tüm 2.1 davranışlarla içine edilmesiyle yerine `SetCompatibilityVersion`, siz açıkça özel davranışları için katılımı. Aşağıdaki kod içine GET işleyicisine eşleme HEAD isteklerini kabul eder.


```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```
::: moniker-end

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF ve Razor sayfaları

Herhangi bir kod yazmanıza gerek kalmaz [antiforgery doğrulama](xref:security/anti-request-forgery). Otomatik olarak antiforgery belirteç oluşturma ve doğrulama Razor sayfaları dahil edilir.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Razor sayfaları kullanmaya düzenleri, kısmi, şablonları ve etiket yardımcılarını kullanma

Sayfaları Razor görüntüleme motorunu tüm özellikleriyle çalışma. Düzenleri, kısmi, şablonlar, etiket Yardımcıları *_ViewStart.cshtml*, *_viewımports.cshtml* geleneksel Razor görünümleri yaptıkları aynı şekilde çalışır.

Şimdi bu sayfa, bu özelliklerden bazılarını avantajlarından yararlanarak declutter.

::: moniker range=">= aspnetcore-2.1"

Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/Shared/_Layout.cshtml*:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Ekleme bir [düzen sayfası](xref:mvc/views/layout) için *Pages/_Layout.cshtml*:

::: moniker-end

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

[Düzen](xref:mvc/views/layout):

* (Sayfa düzeni dışında bölgedeyse sürece) her sayfasının düzenini denetler.
* JavaScript ve stil sayfalarını gibi HTML yapıları içeri aktarır.

Bkz: [düzen sayfası](xref:mvc/views/layout) daha fazla bilgi için.

[Düzen](xref:mvc/views/layout#specifying-a-layout) özelliği ayarlandığında *Pages/_ViewStart.cshtml*:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

::: moniker range=">= aspnetcore-2.1"

Düzen bulunduğu *sayfaları/paylaşılan* klasör. Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın. Bir düzende *sayfaları/paylaşılan* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.

Düzen dosyası gitmesi gereken *sayfaları/paylaşılan* klasör.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Düzen bulunduğu *sayfaları* klasör. Geçerli sayfa ile aynı klasörde başlangıç sayfaları başka görünümlerini (düzenleri, şablonlar, kısmi) hiyerarşik olarak arayın. Bir düzende *sayfaları* klasörü altında herhangi bir Razor sayfadan kullanılabilir *sayfaları* klasör.

::: moniker-end

Öneririz **değil** Düzen dosyası koymak *görünümler/paylaşılan* klasör. *Görünümler/paylaşılan* bir MVC görünümleri modelidir. Razor sayfaları klasör hiyerarşisi, yol kuralları yararlanmayı yöneliktir.

Bir Razor sayfası görünümü aramadan içerir *sayfaları* klasör. Düzenleri, şablonları ve geleneksel Razor görünümleri ile MVC denetleyicileri ile kullanmakta olduğunuz kısmi *yalnızca iş*.

Ekleme bir *Pages/_ViewImports.cshtml* dosyası:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` öğreticinin ilerleyen bölümlerinde açıklanmıştır. `@addTagHelper` Yönergesi getirdiği [yerleşik etiket Yardımcıları](xref:mvc/views/tag-helpers/builtin-th/Index) tüm sayfalara *sayfaları* klasör.

<a name="namespace"></a>

Zaman `@namespace` yönergesi bir sayfada açıkça kullanılır:

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

Yönergesi, ad alanı sayfası için ayarlar. `@model` Yönergesi ad alanını katmak gerekmez.

Zaman `@namespace` yönergesi kapsanıyorsa *_viewımports.cshtml*, belirtilen ad alanı içe aktaran sayfasında oluşturulan ad alanı öneki sağlayan `@namespace` yönergesi. Oluşturulan ad alanı (sonek kısmı) geri kalanı içeren klasörü arasında noktayla ayrılmış göreli yoludur *_viewımports.cshtml* ve sayfayı içeren klasör.

Örneğin, `PageModel` sınıfı *Pages/Customers/Edit.cshtml.cs* ad alanını açıkça ayarlar:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

*Pages/_ViewImports.cshtml* dosyasını ayarlar şu ad alanı:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Oluşturulan ad alanı için *Pages/Customers/Edit.cshtml* Razor sayfası aynıdır `PageModel` sınıfı.

`@namespace` *Geleneksel Razor görünümleri ile de çalışır.*

Özgün *Pages/Create.cshtml* dosyasını görüntüle:

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Güncelleştirilmiş *Pages/Create.cshtml* dosyasını görüntüle:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

[Razor sayfaları başlangıç projesini](#rpvs17) içeren *Pages/_ValidationScriptsPartial.cshtml*, istemci tarafı doğrulama kancaları.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Sayfaları için URL üretimi

`Create` Sayfasında, daha önce kullandığı gösterilen `RedirectToPage`:

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

Uygulama, aşağıdaki dosya/klasör yapısına sahiptir:

* */ Sayfaları*

  * *Index.cshtml*
  * */ Müşteriler*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

*Pages/Customers/Create.cshtml* ve *Pages/Customers/Edit.cshtml* yeniden yönlendirme sayfası *Pages/Index.cshtml* başarılı olduktan sonra. Dize `/Index` önceki sayfaya erişmek için URI'ın bir parçasıdır. Dize `/Index` için URI oluşturmak için kullanılan *Pages/Index.cshtml* sayfası. Örneğin:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Sayfa adı sayfasına kökünden yoludur */sayfaları* önde gelen dahil olmak üzere klasörü `/` (örneğin, `/Index`). Önceki URL oluşturma örnekleri, runbook'a kod bir URL Gelişmiş Seçenekler ve işlevsel yetenekleri sunar. URL üretimi kullanan [yönlendirme](xref:mvc/controllers/routing) ve oluşturmak ve parametreleri hedef yolda bir rota nasıl tanımlandığını göre kodlayın.

URL üretimi sayfaları için göreli adlarını destekler. Aşağıdaki tabloda, hangi dizin sayfası ile farklı seçili olduğunu gösterir `RedirectToPage` parametrelerinden *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Sayfa |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Sayfalar/dizin* |
| RedirectToPage("./Index"); | *Müşteriler/sayfalar/dizin* |
| RedirectToPage(".. / Dizin") | *Sayfalar/dizin* |
| RedirectToPage("Index")  | *Müşteriler/sayfalar/dizin* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, ve `RedirectToPage("../Index")` olan <em>göreli adlar</em>. `RedirectToPage` Parametresi <em>birleştirilmiş</em> ile hedef sayfanın adını işlem için geçerli sayfasının yolu.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

Göreli adı bağlama siteleri karmaşık bir yapısı ile derleme yaparken yararlıdır. Bir klasördeki sayfalar arasında bağlamak için göreli adları kullanıyorsa, bu klasörü yeniden adlandırabilirsiniz. (Bunlar klasör adı olmadığından) tüm bağlantıların hala çalıştığından.

::: moniker range=">= aspnetcore-2.1"
## <a name="viewdata-attribute"></a>ViewData özniteliği

Veri içeren bir sayfa geçilebilir [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Denetleyicilerde veya modellerde Razor sayfası özelliklerini düzenlenmiş ile `[ViewData]` depolanır ve gelen yüklenen değerlerine sahip [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).

Aşağıdaki örnekte, `AboutModel` içeren bir `Title` özelliği düzenlenmiş ile `[ViewData]`. `Title` Özelliği hakkında sayfasının başlığı ayarlayın:

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

Hakkında sayfasında erişim `Title` özelliği model özelliği olarak:

```cshtml
<h1>@Model.Title</h1>
```

Düzende dışında ViewData sözlükten başlığını okuyun:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```
::: moniker-end

## <a name="tempdata"></a>TempData

ASP.NET Core sunan [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) özelliği bir [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller). Bu özellik, okunur kadar verileri depolar. `Keep` Ve `Peek` yöntemleri silme olmadan verileri incelemek için kullanılabilir. `TempData` için yeniden yönlendirme, birden çok tek bir istek verileri gerektiğinde yararlıdır.

`[TempData]` Özniteliği ASP.NET Core 2.0 sürümünde yenidir ve denetleyicileri ve sayfaları desteklenmektedir.

Aşağıdaki kodu değerini ayarlar `Message` kullanarak `TempData`:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Aşağıdaki biçimlendirmede *Pages/Customers/Index.cshtml* dosya değerini görüntülediğine `Message` kullanarak `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

*Pages/Customers/Index.cshtml.cs* sayfa modeli uygular `[TempData]` özniteliğini `Message` özelliği.

```cs
[TempData]
public string Message { get; set; }
```

Bkz: [TempData](xref:fundamentals/app-state#tempdata) daha fazla bilgi için.

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Sayfa başına birden çok işleyicileri

İki sayfa kullanarak işleyicileri için şu sayfaya biçimlendirmeleri oluşturur `asp-page-handler` etiketi Yardımcısı:

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

Önceki örnekte iki düğmeleri kullanarak her gönderme formundadır `FormActionTagHelper` için farklı bir URL göndermek için. `asp-page-handler` Özniteliktir na yardımcı olarak `asp-page`. `asp-page-handler` bir sayfa tarafından tanımlanan işleyici yöntemlerin her biri için gönderme URL oluşturur. `asp-page` örnek, geçerli sayfa için bağlama için belirtilmemiş.

Sayfa modeli:

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Önceki kod *işleyici yöntemleri adlı*. Adlandırılmış işleyici yöntemleri adında sonra metin alarak oluşturulur `On<HTTP Verb>` ve önce `Async` (varsa). Önceki örnekte OnPost sayfa yöntemlerdir**JoinList**zaman uyumsuz ve OnPost**JoinListUC**zaman uyumsuz. İle *OnPost* ve *zaman uyumsuz* kaldırıldıysa, işleyici adları olan `JoinList` ve `JoinListUC`.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="custom-routes"></a>Özel yollar

Kullanım `@page` yönergesini:

* Bir sayfa için özel bir yol belirtin. Rota hakkında sayfasına gibi ayarlanabilir `/Some/Other/Path` ile `@page "/Some/Other/Path"`.
* Bir sayfanın varsayılan yol kesimleri ekleyin. Örneğin, bir "Item" segment ile bir sayfanın varsayılan rota eklenebilir `@page "item"`.
* Bir sayfanın varsayılan rota için parametreleri ekleyin. Örneğin, bir kimliği parametre `id`, içeren bir sayfa için gerekli olabilir `@page "{id}"`.

Bir tilde işareti tarafından atanan bir köküne göreli yol (`~`) yolunun başında desteklenir. Örneğin, `@page "~/Some/Other/Path"` aynı `@page "/Some/Other/Path"`.

Sorgu dizesi değiştirebilirsiniz `?handler=JoinList` yol kesimi URL'yi `/JoinList` rota şablonu belirterek `@page "{handler?}"`.

Sorgu dizesi beğenmezseniz `?handler=JoinList` URL'de, yol işleyicisi adı URL'nin yol kısmı put değiştirebilirsiniz. Rota sonra çift tırnak içine alınmış bir rota şablonu ekleyerek özelleştirebilirsiniz `@page` yönergesi.

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

Kod önceki kullanarak, gönderildiği URL yolu `OnPostJoinListAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinList`. Gönderildiği URL yolu `OnPostJoinListUCAsync` olduğu `http://localhost:5000/Customers/CreateFATH/JoinListUC`.

`?` Aşağıdaki `handler` rota parametresinin isteğe bağlı olduğu anlamına gelir.

## <a name="configuration-and-settings"></a>Yapılandırma ve ayarlar

Gelişmiş seçeneklerini yapılandırmak için genişletme yöntemini kullanmak `AddRazorPagesOptions` MVC oluşturucu üzerinde:

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Şu anda kullanabileceğiniz `RazorPagesOptions` sayfa için kök dizine ayarlayın veya uygulama sayfaları için model kuralları ekleyin. Daha fazla genişletilebilirlik bu şekilde gelecekte etkinleştiririz.

Görünümlerinizi önceden derleme için bkz: [Razor görünüm derlemesi](xref:mvc/views/view-compilation) .

[İndirme veya görüntüleme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/index/sample).

Bkz: [Razor sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start), ilgili bu girişi oluşturur.

### <a name="specify-that-razor-pages-are-at-the-content-root"></a>Razor sayfaları içerik kök dizininde olduğunu belirtin

Varsayılan olarak, Razor sayfaları, köklü olmayan */sayfaları* dizin. Ekleme [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları içerik kök dizininde olduğunu belirtmek için ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) uygulama:

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a>Razor sayfaları özel kök dizininde olduğunu belirtin

Ekleme [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) için [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , Razor sayfaları bir uygulamadaki özel kök dizininde olduğunu belirtmek için (göreli bir yol belirtin):

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a>Ayrıca bkz.

* [ASP.NET Core'a giriş](xref:index)
* [Razor söz dizimi](xref:mvc/views/razor)
* [Razor Sayfaları kullanmaya başlama](xref:tutorials/razor-pages/razor-pages-start)
* [Razor sayfaları yetkilendirme kuralları](xref:security/authorization/razor-pages-authorization)
* [Razor sayfaları özel rota ve sayfa modeli sağlayıcılar](xref:razor-pages/razor-pages-conventions)
* [Razor Sayfaları birim testleri](xref:test/razor-pages-tests)
