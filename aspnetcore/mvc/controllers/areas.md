---
title: "Alanları"
author: rick-anderson
description: "Alanları ile çalışmaya nasıl gösterir."
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/areas
ms.openlocfilehash: 1ade49de3f6c58edc4ea7b06bc593b3db797081c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="areas"></a>Alanları

Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Alanlar, ilgili işlevleri (Yönlendirme) ayrı ad alanını ve klasör yapısı (için görünümler) olarak bir grup düzenlemek için kullanılan bir ASP.NET MVC özelliğidir. Alanları kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla bir hiyerarşi oluşturur `area`, `controller` ve `action`.

Alanları büyük bir ASP.NET Core MVC Web uygulaması işlevsel gruplamalarda daha küçük bölümlere ayırmak için bir yol sağlar. Etkin bir uygulama içinde bir MVC yapısı alanıdır. MVC projesinde, Model, denetleyici ve görünüm gibi mantıksal bileşenlerin farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır. Büyük bir uygulama için uygulama ayrı yüksek düzey alanlarına işlevlerin bölümlemek için yararlı olabilir. Örneğin, bir e-ticaret uygulamayla checkout, faturalama ve arama vb. gibi birden çok iş birimleri. Her bu birimleri kendi mantıksal bileşen görünümleri, denetleyicileri ve modeli vardır. Bu senaryoda, fiziksel olarak iş bileşenleri aynı projede bölümlemek için alanları kullanabilirsiniz.

Bir alanı denetleyicileri, görünümler ve modelleri kendi kümesiyle ASP.NET Core MVC projesinde daha küçük işlevsel birimleri olarak tanımlanabilir.

Bir MVC alanlarda kullanmayı ne zaman proje:

* Uygulamanız, mantıksal olarak ayrılmalıdır birden çok üst düzey işlev bileşenlerden

* Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek MVC projenizi bölüm istiyor

Alan özellikleri:

* Bir ASP.NET Core MVC uygulama herhangi bir sayıda alanları olabilir

* Kendi denetleyicileri, modelleri ve görünümler her bir alan vardır

* Büyük MVC projeler üzerinde bağımsız olarak çalışılabilecek birden çok üst düzey bileşenlerine düzenlemenizi sağlar

* Farklı olduğu sürece birden çok denetleyicisi aynı adda - destekleyen *alanları*

Alanları nasıl oluşturulduğunu ve kullanılan göstermek için örnek bir göz atalım. İki ayrı gruplandırmaları denetleyicilerinin ve görünümlerin sahip bir mağaza uygulaması sahip varsayalım: ürünler ve hizmetler. Tipik bir klasör yapısı için MVC alanları kullanarak aşağıda benzer olduğunu:

* Proje adı

  * Alanları

    * Ürünler

      * Denetleyiciler

        * HomeController.cs

        * ManageController.cs

      * Görünümler

        * Ana Sayfası

          * Index.cshtml

        * Yönetme

          * Index.cshtml

    * Hizmetler

      * Denetleyiciler

        * HomeController.cs

      * Görünümler

        * Ana Sayfası

          * Index.cshtml

Varsayılan olarak bir alandaki bir görünümü işlemek MVC çalıştığında, aşağıdaki konumlarda aramak çalışır:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Aracılığıyla değiştirilebilen varsayılan konumları bunlar `AreaViewLocationFormats` üzerinde `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Örneğin, klasör adı 'Alanlarını' olarak sahip olmak yerine kod 'Kategorilere' değiştirildi.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Not etmek için bir şey yapan, yapısını *görünümleri* klasör burada önemli olarak kabul edilen tek ve klasörleri geri kalanı içeriğini ister *denetleyicileri* ve *modelleri* mu **değil** önemli. Örneğin, yüklü bir *denetleyicileri* ve *modelleri* hiç klasör. Bu çalışır çünkü içeriğini *denetleyicileri* ve *modelleri* bir .dll burada içeriğini olarak derlenmiş yalnızca kodu *görünümleri* , bir istek kadar değil Görünüm yapıldı.

Klasör hiyerarşisi tanımladığınız sonra MVC her denetleyici bir alanı ile ilişkili olduğunu söylemek gerekir. Denetleyici adı ile tasarlayarak bunu `[Area]` özniteliği.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Yeni oluşturulan alanlarınızı ile çalışan bir rota tanımı ayarlayın. [Denetleyici eylemleri için yönlendirme](routing.md) makale gider içine öznitelik rotaları karşı geleneksel yolları kullanma dahil olmak üzere yönlendirme tanımları oluşturma hakkında ayrıntılı bilgi. Bu örnekte, geleneksel bir rota kullanacağız. Bunu yapmak için açık *haline* dosya ve ekleyerek değiştirmeye `areaRoute` route tanımını aşağıdaki adlı.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

İçin dizin taramayı `http://<yourApp>/products`, `Index` eylem yöntemi `HomeController` içinde `Products` alanı çağrılabilir.

## <a name="link-generation"></a>Bağlantı oluşturma

* Bir alanda bir eylemden bağlantıları oluşturmak aynı denetleyicisi içinde başka bir eylem denetleyiciye bağlı.

  Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`

  HtmlHelper sözdizimi:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper sözdizimi:`<a asp-action="Index">Go to Product's Home Page</a>`

  Biz 'alanı' ve 'controller' değerleri sağlamanızı olmayan Not zaten geçerli istek bağlamında kullanılabilir burada. Bu tür bir değerleri çağrılır `ambient` değerleri.

* Farklı bir denetleyicideki başka bir eylem denetleyiciye bağlı bir alanda bir eylemden bağlantıları oluşturmak

  Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`

  HtmlHelper sözdizimi:`@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper sözdizimi:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Burada bir 'alanı' ortam değeri kullanılır, ancak 'controller' değeri açıkça yukarıda belirtilen unutmayın.

* Bir alanda bir eylemden bağlantıları oluşturmak için başka bir eylem denetleyicisi farklı bir denetleyici ve farklı bir alan göre.

  Geçerli isteğin yolu benzer düşünelim`/Products/Home/Create`

  HtmlHelper sözdizimi:`@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper sözdizimi:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Unutmayın burada yok ortam değerler kullanılır.

* Bir temel alan denetleyicisi içinde bir eylem bağlantıları farklı bir denetleyicideki başka bir eylem oluşturmak ve **değil** bir bölgede.

  HtmlHelper sözdizimi:`@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper sözdizimi:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Biz oluşturmak istediğinizde bu yana alanına olmayan bağlantılar 'alanı' burada ortam değeri boş biz denetleyici eylemi temel.

## <a name="publishing-areas"></a>Yayımlama alanları

Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.
