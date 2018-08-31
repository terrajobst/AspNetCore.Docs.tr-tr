---
title: ASP.NET core'da alanları
author: rick-anderson
description: Alanlar (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba ilgili işlevleri düzenlemek için kullanılan bir ASP.NET MVC özelliği nasıl olduğunu öğrenin.
ms.author: riande
ms.date: 02/14/2017
uid: mvc/controllers/areas
ms.openlocfilehash: b78bb5146f1ab9039fa9ff015471654510718ed6
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312224"
---
# <a name="areas-in-aspnet-core"></a>ASP.NET core'da alanları

Tarafından [Dhananjay Kumar](https://twitter.com/debug_mode) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Alanlar, ilgili işlevleri (yönlendirme için) ayrı bir ad ve klasör yapısını (için görünümler) bir gruba düzenlemek için kullanılan bir ASP.NET MVC özelliğidir. Alanlara kullanarak başka bir rota parametresini ekleyerek yönlendirme amacıyla hiyerarşi oluşturur `area`, `controller` ve `action`.

Alanları büyük bir ASP.NET Core MVC Web uygulaması işlevsel gruplamalarda daha küçük bölümlere ayırmak için bir yol sağlar. Bir MVC yapı bir uygulama içinde etkili bir şekilde alanıdır. Bir MVC projesi mantıksal bileşenler modeli, denetleyici ve görünüm gibi farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır. Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir. Örneğin, bir e-ticaret uygulamayla kullanıma alma ve faturalandırma arama vb. gibi birden çok iş birimleri. Bu birimlerin her biri kendi mantıksal bileşen görünümleri, denetleyicilere ve modelleri sahip. Bu senaryoda, fiziksel olarak aynı projede iş bileşenleri bölümlemek için alanlar kullanabilirsiniz.

Bir alanı denetleyicileri, görünümler ve modelleri, kendi kümesi ile ASP.NET Core MVC projesinde daha küçük işlevsel birimi olarak tanımlanabilir.

Bir MVC alanlardaki ne zaman proje:

* Uygulamanız, mantıksal olarak ayrılması birden çok üst düzey işlevsel bileşenden

* Böylece her işlevsel alan üzerinde bağımsız olarak çalışılabilecek MVC projenize bölümlemek istediğiniz

Alan özellikleri:

* Bir ASP.NET Core MVC uygulaması herhangi bir sayıda alanları olabilir.

* Her alan kendi denetleyicileri, modelleri ve görünümleri sahiptir.

* Alanları üzerinde bağımsız olarak çalışması birden çok üst düzey bileşenlerine büyük MVC projeleri düzenlemenize olanak sağlar.

* Farklı sahip oldukları sürece aynı ada sahip birden çok denetleyicileri destek alanları *alanları*.

Alanlarını nasıl oluşturulduğunu ve kullanılan göstermek için örnek bir göz atalım. İki ayrı gruplandırmaları görünümleri ve denetleyicileri içeren bir mağaza uygulaması sahip düşünelim: Ürün ve Hizmetleri. Tipik bir klasör yapısı için MVC alanlara kullanarak aşağıda benzer olduğunu:

* Proje adı

  * Alanları

    * Ürünler

      * Denetleyiciler

        * HomeController.cs

        * ManageController.cs

      * Görünümler

        * Ana Sayfası

          * Index.cshtml

        * yönetme

          * Index.cshtml

    * Hizmetler

      * Denetleyiciler

        * HomeController.cs

      * Görünümler

        * Ana Sayfası

          * Index.cshtml

Varsayılan olarak bir alanda görünüm işlemek MVC çalıştığında, aşağıdaki konumlarda aramak çalışır:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Bunlar aracılığıyla değiştirilebilen varsayılan konumları `AreaViewLocationFormats` üzerinde `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Örneğin, 'Alanları olarak' klasör adı yerine aşağıdaki kodu, bu 'Kategorilere' değiştirildi.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Unutmayın olmasıdır yapısını *görünümleri* burada önemli olarak kabul edilen tek bir klasördür ve klasörleri geri kalanını içeriği gibi *denetleyicileri* ve *modelleri* mu **değil** önemi. Örneğin, sahip bir *denetleyicileri* ve *modelleri* hiç klasör. Bunun çalışmasının nedeni içeriğini *denetleyicileri* ve *modelleri* içeriği burada olarak derlenmiş bir .dll, yalnızca kod *görünümleri* , isteğine kadar değil Görünüm yapıldı.

Klasör hiyerarşisini tanımladınız sonra MVC denetleyicisi her bir alanı ile ilişkili olduğunu bildirmek gerekir. Denetleyici adı ile tasarlayarak bunu `[Area]` özniteliği.

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

Yeni oluşturulan alanlarınızı çalışır bir yönlendirme tanımı ayarlayın. [Denetleyici eylemleri için rota](routing.md) öznitelik rotaları karşı geleneksel yollar kullanarak da dahil olmak üzere, yönlendirme tanımları oluşturma hakkında daha fazla ayrıntı makale gider. Bu örnekte, geleneksel bir rota kullanacağız. Bunu yapmak için açık *Startup.cs* ekleyerek değiştirin ve dosya `areaRoute` rota tanımını aşağıdaki adlı.

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

Gözatmaya `http://<yourApp>/products`, `Index` eylem yöntemi `HomeController` içinde `Products` alan çağrılacak.

## <a name="link-generation"></a>Bağlantı oluşturma

* Bir alanda bir eylem bağlantıları oluşturmak aynı denetleyici içinde başka bir eylem denetleyiciye bağlı.

  Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`

  HtmlHelper sözdizimi: `@Html.ActionLink("Go to Product's Home Page", "Index")`

  TagHelper sözdizimi: `<a asp-action="Index">Go to Product's Home Page</a>`

  Unutmayın, biz 'alanı' ve 'controller' değerlerini belirtmeniz değil, zaten geçerli istek bağlamında kullanılabilir burada. Bu tür değerlere çağrılır `ambient` değerleri.

* Başka bir eylem farklı denetleyicisine denetleyiciye bağlı bir alanda bir eylem bağlantıları oluşturmak

  Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`

  HtmlHelper sözdizimi: `@Html.ActionLink("Go to Manage Products Home Page", "Index", "Manage")`

  TagHelper sözdizimi: `<a asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Burada ortam 'alanı' değeri kullanılmıştır, ancak 'controller' değeri açıkça yukarıda belirtilen unutmayın.

* Bir alanda bir eylem bağlantıları oluşturmak için başka bir eylem denetleyicisi farklı bir denetleyici ve farklı bir alana göre.

  Geçerli isteğin yolu gibi diyelim ki `/Products/Home/Create`

  HtmlHelper sözdizimi: `@Html.ActionLink("Go to Services Home Page", "Index", "Home", new { area = "Services" })`

  TagHelper sözdizimi: `<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services Home Page</a>`

  Unutmayın burada hiçbir ortam değerler kullanılır.

* İçinde bir temel alan denetleyici eylem bağlantıları farklı bir denetleyici üzerinde başka bir eylem oluşturmak ve **değil** bir alana.

  HtmlHelper sözdizimi: `@Html.ActionLink("Go to Manage Products  Home Page", "Index", "Home", new { area = "" })`

  TagHelper sözdizimi: `<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products Home Page</a>`

  Oluşturulacak istediğinden alanı olmayan bağlantılar denetleyici eylemi boş biz 'alanı' burada için ortam değerine göre.

## <a name="publishing-areas"></a>Yayımlama alanları

Tüm `*.cshtml` ve `wwwroot/**` dosyaları ne zaman çıkış yayımlanır `<Project Sdk="Microsoft.NET.Sdk.Web">` dahil *.csproj* dosya.
