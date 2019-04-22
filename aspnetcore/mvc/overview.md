---
title: ASP.NET Core MVC’ye Genel Bakış
author: ardalis
description: ASP.NET Core MVC web uygulamaları oluşturmaya yönelik zengin bir altyapı nasıl olduğunu öğrenin ve API'leri kullanarak Model-View-Controller tasarım deseni.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: eb636616ab952f506993ae5f4692e5e966bb20f4
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59516306"
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC’ye Genel Bakış

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC web uygulamaları oluşturmaya yönelik zengin bir altyapı olan ve API'leri kullanarak Model-View-Controller tasarım deseni.

## <a name="what-is-the-mvc-pattern"></a>MVC düzeni nedir?

Model-View-Controller (MVC) tasarım örüntüsü, uygulamanın üç ana bileşenleri gruplar halinde ayırır: Modelleri, görünümleri ve denetleyicileri. Bu düzen, elde yardımcı olur [görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns). Bu düzeni kullanmak, kullanıcı isteklerini, kullanıcı eylemlerini gerçekleştirmek ve/veya sorguların sonuçlarını almak için modeli ile çalışmak için sorumlu denetleyicisi yönlendirilir. Denetleyici kullanıcıya görüntülenecek görünüm seçer ve gerektirdiği modeli verilerle sağlar.

Aşağıdaki diyagramda üç ana bileşenleri gösterilmektedir ve hangilerinin diğer başvuru:

![MVC düzeni](overview/_static/mvc.png)

Bu kabul edilebilir açıklıkta sorumlulukları karmaşıklık açısından uygulama kodu, hata ayıklama ve sorun (model, görünüm veya denetleyicisi) tek bir iş olan test etmek daha kolay ölçeklendirme yardımcı olur. Bu güncelleştirme, test ve iki veya daha fazlası üç bu alanlar arasında yayılabilir bağımlılıkları olan hata ayıklama kodu daha zordur. Örneğin, kullanıcı arabirimi mantığı, iş mantığı daha sık değiştirmek için eğilimi gösterir. Sunu kod ve iş mantığı tek bir nesnede birleştirildiğinde, kullanıcı arabirimi her değiştiğinde iş mantığı içeren bir nesne değiştirilmesi gerekir. Bu genellikle hataları sunar ve iş mantığı her en düşük kullanıcı arabirimi değişiklikten sonra çözülüp gerektirir.

> [!NOTE]
> Hem görünümü hem de denetleyicisi modeline bağlıdır. Ancak, model, görünüm ne denetleyici bağlıdır. Bu, ayrımı başlıca avantajlarından biridir. Bu ayrım modeli oluşturulan test edilmiş ve görsel sunumunu bağımsız sağlar.

### <a name="model-responsibilities"></a>Model sorumlulukları

Bir MVC uygulamasında Model, uygulama ve iş mantığı veya tarafından yapılması gereken işlemlerin durumunu temsil eder. İş mantığı modelde, uygulamanın durumunu kalıcı hale getirmeniz için herhangi bir uygulama mantığı ile birlikte kapsüllenmiş. Kesin türü belirtilmiş görünümler, verileri içerecek şekilde tasarlanmış ViewModel türleri genellikle bu görünümde göstermek için kullanın. Denetleyici oluşturur ve bu ViewModel örnekler modelinden doldurur.

### <a name="view-responsibilities"></a>Görünümünü Sorumluluklar

Görünümler kullanıcı arabirimi aracılığıyla içerik sunmak için sorumlu olursunuz. Kullandıkları [Razor görünüm altyapısı](#razor-view-engine) HTML biçimlendirmesini ekleme .NET kodu için. Görünümler içinde en az bir mantıksal olmalıdır ve bunlara herhangi bir mantık içerik sunmak için sebeple ilişkili olmalıdır. Harika bir fırsat mantığı görünümde gerçekleştirmek için gereken dosyaları bir karmaşık modeli verileri görüntülemek için fark ederseniz, kullanmayı bir [görünümü bileşen](views/view-components.md), ViewModel, ya da görünümü basitleştirmek için şablonu görüntüle.

### <a name="controller-responsibilities"></a>Denetleyici sorumlulukları

Denetleyicileri, kullanıcı etkileşimi işlemek, modelle çalışan ve sonuç olarak işlemek için bir görünüm seçin bileşenlerdir. Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir. MVC örüntüsü, denetleyici ilk giriş noktasıdır ve hangi modelle çalışmak için türleri ve işlemek için hangi görünümün seçmek için sorumludur (Bu nedenle adını - denetimleri uygulama için belirtilen bir isteğin nasıl yanıt vereceğini).

> [!NOTE]
> Denetleyicileri, çok fazla sorumluluklara göre aşırı karmaşık olmamalıdır. Fazla karmaşık hale gelmesini denetleyicisi mantıksal tutmak için iş mantığı denetleyicisi dışında ve etki alanı modeline gönderin.

>[!TIP]
> Denetleyici eylemleri sık aynı tür eylemler gerçekleştirmek bulursanız, bu ortak Eylemler içine Taşı [filtreleri](#filters).

## <a name="what-is-aspnet-core-mvc"></a>ASP.NET Core MVC nedir

ASP.NET Core MVC, bir basit, açık kaynaklı bir ASP.NET Core ile kullanılmak için iyileştirilmiş yüksek düzeyde sınanabilir bir sunu çerçevesidir çerçevedir.

ASP.NET Core MVC, ilgilenilecek alanların temiz bir biçimde ayrılmasını sağlayan dinamik Web siteleri oluşturmak için desen tabanlı bir yöntem sağlar. İşaretleme üzerinde tam denetim verir, TDD kullanımı kolay geliştirme destekler ve en son web standartlarını kullanır.

## <a name="features"></a>Özellikler

ASP.NET Core MVC aşağıdakileri içerir:

* [Yönlendirme](#routing)
* [Model bağlama](#model-binding)
* [Model doğrulaması](#model-validation)
* [Bağımlılık ekleme](../fundamentals/dependency-injection.md)
* [Filtreler](#filters)
* [Alanlar](#areas)
* [Web API'leri](#web-apis)
* [Test Edilebilirlik](#testability)
* [Razor görünüm altyapısı](#razor-view-engine)
* [Kesin türü belirtilmiş görünümler](#strongly-typed-views)
* [Etiket Yardımcıları](#tag-helpers)
* [Görünüm bileşenleri](#view-components)

### <a name="routing"></a>Yönlendirme

ASP.NET Core MVC üzerine kurulmuştur [ASP.NET Core'nın Yönlendirme](../fundamentals/routing.md), olanak tanıyan güçlü bir URL eşlemesi bileşeni, anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturun. Bu, web sunucunuzdaki dosyaları nasıl düzenlendiği için bakmadan bağlantı oluşturma ve arama motoru iyileştirmesi (SEO) için iyi iş uygulamanızın URL adlandırma modelleri tanımlamanıza olanak sağlar. Rota değeri kısıtlamaları, Varsayılanları ve isteğe bağlı değerler destekleyen bir kullanışlı bir yol şablonu söz dizimini kullanarak yollarınızı tanımlayabilirsiniz.

*Kural tabanlı yönlendirme* genel URL tanımlamanızı biçimleri sağlar, uygulamanızın kabul eder ve her birini nasıl biçimlendirir belirli bir eylem yöntemine üzerinde denetleyici eşler. Gelen bir istek alındığında, yönlendirme altyapısını URL ayrıştırır ve tanımlanmış URL biçimlerinden biri kendisine eşleşir ve ardından ilişkili denetleyicinin eylem yöntemini çağırır.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Öznitelik yönlendirme* denetleyici ve Eylemler, uygulamanızın yollar tanımlayan öznitelikleri ile tasarlayarak yönlendirme bilgilerini belirtmenize olanak sağlar. Başka bir deyişle, denetleyici ve eylem ile ilişkili oldukları yanındaki rota tanımlarınızı yerleştirilir.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Model bağlama

ASP.NET Core MVC [model bağlama](models/model-binding.md) istemci isteği verilerini (form değerleri, rota verileri, sorgu dizesi parametreleri, HTTP üst bilgileri) denetleyicisi işleyebileceği nesnelerine dönüştürür. Sonuç olarak, denetleyici mantığınızı gelen istek verileri başarınızda yapması gerekmez; yalnızca kendi eylem yöntemlerine parametre olarak veri yok.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Model doğrulaması

ASP.NET Core MVC destekler [doğrulama](models/validation.md) modeli nesnenizle veri ek açıklama doğrulama özniteliklerinin dekorasyon olarak. Doğrulama özniteliklerinin değerleri sunucuya gönderilen önce istemci tarafında denetlenir yanı denetleyici eylemini önce sunucu üzerinde çağrılır.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Bir denetleyici eylemi:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

Çerçeve doğrulama isteği verileri istemci ve sunucu işler. Belirtilen model türleri üzerinde doğrulama mantığı işlenmiş görünüm örtük ek açıklamaları olarak eklenir ve tarayıcı ile zorlanır [jQuery doğrulaması](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Bağımlılık ekleme

ASP.NET Core için yerleşik desteği vardır [bağımlılık ekleme (dı)](../fundamentals/dependency-injection.md). ASP.NET Core MVC, [denetleyicileri](controllers/dependency-injection.md) istek gerekli hizmetleri aracılığıyla bunları izlemek izin verme oluşturucuları, [açık bağımlılıkları ilkesine](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

Uygulamanızı kullanabilirsiniz [görünümünde bağımlılık ekleme dosyaları](views/dependency-injection.md)kullanarak `@inject` yönergesi:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>FilTReleri

[Filtreler](controllers/filters.md) özel durum işleme veya yetkilendirme gibi çapraz kesme konuları kapsülleyen geliştiricilerin yardımcı. Filtreleri eylem yöntemleri için çalışan özel öncesi ve sonrası işleme mantığı etkinleştirmek ve belirli bir istek için yürütme işlem hattı içindeki bazı noktalarda çalıştırmak için yapılandırılabilir. Filtreler denetleyicileri veya öznitelikleri eylemleri uygulanabilir (veya genel olarak çalıştırılabilir). Birkaç filtreleri (gibi `Authorize`) Framework'e dahil edilen. `[Authorize]` MVC yetkilendirme filtrelerini oluşturmak için kullanılan özniteliğidir.

```csharp
[Authorize]
public class AccountController : Controller
```

### <a name="areas"></a>Alanlar

[Alanları](controllers/areas.md) büyük bir ASP.NET Core MVC Web uygulaması işlevsel gruplamalarda daha küçük bölümlere ayırmak için bir yol sağlar. Bir alan, bir uygulama içinde bir MVC yapısıdır. Bir MVC projesi mantıksal bileşenler modeli, denetleyici ve görünüm gibi farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır. Büyük bir uygulama için ayrı yüksek düzey alanlarına işlev uygulamasını bölümleme için yararlı olabilir. Örneğin, bir e-ticaret uygulamayla kullanıma alma ve faturalandırma arama vb. gibi birden çok iş birimleri. Bu birimlerin her biri kendi mantıksal bileşen görünümleri, denetleyicilere ve modelleri sahip.

### <a name="web-apis"></a>Web API'leri

ASP.NET Core MVC, web siteleri oluşturmak için harika bir platform olmaya ek olarak, Web API'leri oluşturmaya yönelik harika desteğe sahiptir. Bir çeşit tarayıcılar ve mobil cihazlar dahil olmak üzere istemciye ulaşan hizmetler oluşturabilirsiniz.

HTTP İçerik anlaşma desteği için yerleşik destekle framework içerir [veri biçimlendirme](xref:web-api/advanced/formatting) JSON veya XML. Yazma [özel biçimlendiriciler](xref:web-api/advanced/custom-formatters) kendi biçimleri için destek eklemek için.

Bağlantı oluşturma, Hiper medyayı desteğini etkinleştirmek için kullanın. Kolayca desteğini etkinleştirme [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) böylece Web API'leri, birden çok Web uygulaması arasında paylaşılabilir.

### <a name="testability"></a>Test Edilebilirlik

Framework'ün kullanımını arabirimleri ve bağımlılık ekleme kılmaktadır için birim testi ve framework, Özellikler (örneğin, Entity Framework için TestHost ve Inmemory sağlayıcısı) içerir [tümleştirme testleri](xref:test/integration-tests) hızlı ve kolay de. Daha fazla bilgi edinin [denetleyicisi mantıksal test etme](controllers/testing.md).

### <a name="razor-view-engine"></a>Razor görünüm altyapısı

[ASP.NET Core MVC görünümleri](views/overview.md) kullanın [Razor görünüm altyapısı](views/razor.md) görünüm işlemek için. Razor, katıştırılmış C# kodunu kullanarak görünümlerini tanımlamak için bir compact, ifadesel ve akıcı şablon biçimlendirme dilidir. Razor, web içeriği sunucusundaki dinamik olarak oluşturmak için kullanılır. Sunucu kodu, istemci tarafı içeriği ve kod ile düzgün bir şekilde karıştırabilirsiniz.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Tanımlayabilirsiniz Razor görünüm altyapısı kullanılarak [düzenleri](views/layout.md), [kısmi görünümler](views/partial.md) ve bölümleri değiştirilebilir.

### <a name="strongly-typed-views"></a>Kesin türü belirtilmiş görünümler

MVC Razor görünümleri, modelinize göre türü kesin olarak. Denetleyicileri tür denetlemesi ve IntelliSense desteği sağlamak kendi görünümlerinizi etkinleştirme görünüme kesin türü belirtilmiş bir model geçirebilirsiniz.

Örneğin, bir türü modelin aşağıdaki görünümü işleyen `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Etiket Yardımcıları

[Etiket Yardımcıları](views/tag-helpers/intro.md) sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğeleri işleme katılmak etkinleştirin. Etiket Yardımcıları özel etiketler tanımlamak için kullanabilirsiniz (örneğin, `<environment>`) veya mevcut olan etiketlerin davranışını değiştirmek için (örneğin, `<label>`). Etiket Yardımcıları öğe adı ve öznitelikleri temel alan belirli öğeleri bağlayın. Bunlar, bir HTML düzenleme deneyimi hala korurken, sunucu tarafı işleme avantajlarını sağlar.

Birçok yerleşik etiket Yardımcıları için formlar, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi ortak görevler - vardır. Etiket Yardımcıları C# dilinde yazılmış ve öğe adı, öznitelik adı veya üst etiketi dayalı HTML öğeleri hedeflenir. Örneğin, LinkTagHelper bağlantısı oluşturmak için kullanılabilir yerleşik `Login` eylemi `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` Farklı betikler görünümleriniz (örneğin, ham veya küçültülmüş) çalışma zamanı ortamı, geliştirme, hazırlama veya üretim gibi temel dahil etmek için kullanılabilir:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Etiket Yardımcıları, bir HTML kullanımı kolay geliştirme deneyimi ve HTML ve Razor biçimlendirme oluşturmak için zengin IntelliSense ortamı sağlar. Yerleşik etiket Yardımcıları çoğu mevcut HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.

### <a name="view-components"></a>Görünüm bileşenleri

[Görüntüleme bileşenleri](views/view-components.md) işleme mantığının paketleyin ve uygulamanın tamamında yeniden olanak sağlar. Benzer şekilde oldukları [kısmi görünümler](views/partial.md), ancak ilişkili mantığı.

## <a name="compatibility-version"></a>Uyumluluk sürümü

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar.

Daha fazla bilgi için bkz. <xref:mvc/compatibility-version>.
