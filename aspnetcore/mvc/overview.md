---
title: ASP.NET Core MVC’ye Genel Bakış
author: ardalis
description: ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve nasıl olduğunu öğrenin ve Model-View-Controller kullanarak API'leri düzeni tasarlayın.
ms.author: riande
ms.date: 01/08/2018
uid: mvc/overview
ms.openlocfilehash: aca34f91e8c7efaa34263ddf830b1662a2518969
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272598"
---
# <a name="overview-of-aspnet-core-mvc"></a>ASP.NET Core MVC’ye Genel Bakış

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC web uygulamaları oluşturmak için zengin bir çerçeve olan ve Model-View-Controller kullanarak API'leri düzeni tasarlayın.

## <a name="what-is-the-mvc-pattern"></a>MVC modeli nedir?

Model-View-Controller (MVC) tasarım örüntüsü bir uygulama bileşenlerinin üç ana gruplara ayırır: modeller, görünümler ve denetleyiciler. Elde etmek için bu deseni yardımcı [sorunları ayrılması](http://deviq.com/separation-of-concerns/). Bu deseni kullanarak, kullanıcı isteklerini kullanıcı eylemleri gerçekleştirmek ve/veya sorgu sonuçları almak için modeli ile çalışmak için sorumlu denetleyicisi yönlendirilir. Denetleyici kullanıcıya gösterilmesi için görüntüyü seçer ve gerektirdiği herhangi bir Model veri ile sağlar.

Aşağıdaki diyagramda üç ana bileşeni gösterir ve hangilerinin diğer başvuru:

![MVC örüntüsü](overview/_static/mvc.png)

Bu kabul edilebilir açıklıkta sorumlulukları karmaşıklık açısından uygulama kodu, hata ayıklama ve bir şey (model, görünüm veya denetleyicisi) tek bir işin test etmeyi daha kolay olduğundan ölçek yardımcı olur (ve izleyen [tek sorumluluk İlkesi ](http://deviq.com/single-responsibility-principle/)). Bu güncelleştirme, test ve iki veya daha fazla üç bu alanlar arasında yayılan bağımlılıkları olan hata ayıklama kodu daha zordur. Örneğin, kullanıcı arabirimi mantığı, iş mantığı daha sık değiştirmek için eğilimi gösterir. Sunu kodu ve iş mantığı tek bir nesnede birleştirildiğinde, kullanıcı arabirimi her değiştiğinde iş mantığı içeren bir nesne değiştirilmesi gerekir. Bu genellikle hataları tanıtır ve iş mantığını her en az kullanıcı arabirimi değişiklikten sonra retesting gerektirir.

> [!NOTE]
> Görünüm ve denetleyici modeline bağlı olarak değişir. Ancak, model, görünüm ne denetleyicisi bağlıdır. Bu, avantajları ayırma biridir. Bu ayrım oluşturulur ve test modeli görsel sunumu bağımsız olanak sağlar.

### <a name="model-responsibilities"></a>Model sorumlulukları

Modelin MVC uygulamasındaki uygulama ve iş mantığı veya tarafından gerçekleştirilmesi gereken işlemleri durumunu temsil eder. İş mantığı modelde uygulama durumunu sürdürmek için tüm uygulama mantığı ile birlikte kapsüllenmiş. Kesin türü belirtilmiş görünümleri, verileri içerecek şekilde tasarlanmış ViewModel türleri genellikle bu görünümde göstermek için kullanılır. Denetleyici oluşturur ve bu ViewModel örnekler modelden doldurur.

> [!NOTE]
> MVC tasarım örüntüsü kullanan bir uygulama modelinde düzenlemek için birçok yolu vardır. Bazı hakkında daha fazla bilgi [model türlerinin farklı türde](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Görünüm sorumlulukları

Görünümler, kullanıcı arabirimi aracılığıyla içerik sunmak için sorumludur. Kullandıkları [Razor görüntüleme altyapısı](#razor-view-engine) .NET kodu HTML biçimlendirmesini katıştırmak için. Görünümler içinde en az mantığının olmalıdır ve bunlara herhangi bir mantık içerik sunmak için ilişkili olmalıdır. Büyük bir bölümünü mantığı görünümde gerçekleştirmek için gereken dosyaları bir karmaşık modeli verileri görüntülemek için bulursanız, kullanmayı bir [görünümü bileşen](views/view-components.md), ViewModel, ya da görünümü basitleştirmek için şablonu görüntüle.

### <a name="controller-responsibilities"></a>Denetleyici sorumlulukları

Denetleyicileri kullanıcı etkileşimini işleyen, modelle çalışan ve sonuç olarak işlemek için bir görünüm seçin bileşenleridir. Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir. MVC örüntüsü denetleyicisi ilk giriş noktası ve hangi modelle çalışmak için türleri ve işlemek için hangi görünüm seçmek için sorumludur (Bu nedenle, ad - denetimleri uygulama için belirtilen bir isteğin nasıl yanıt vereceğini).

> [!NOTE]
> Tarafından çok sayıda sorumlulukları denetleyicileri aşırı karmaşık döndürmemelidir. Denetleyici mantığında fazla karmaşık hale gelmesini tutmak için kullanın [tek sorumluluk ilkesine](http://deviq.com/single-responsibility-principle/) İtme iş mantığı denetleyicisi dışında ve etki alanı modeline.

>[!TIP]
> Denetleyici eylemleri sık aynı tür Eylemler gerçekleştirir bulursanız, takip edebilirsiniz [kendiniz ilkesine yineleme](http://deviq.com/don-t-repeat-yourself/) bu ortak eylemlere taşıyarak [filtreleri](#filters).

## <a name="what-is-aspnet-core-mvc"></a>ASP.NET Core MVC nedir

ASP.NET Core MVC çerçevesi bir hafif, açık kaynaklı, ASP.NET Core ile kullanmak için en iyi duruma getirilmiş yüksek düzeyde sınanabilir bir sunu çerçevesidir ' dir.

ASP.NET Core MVC sorunları temiz ayrılması sağlayan dinamik Web siteleri oluşturmak için desenleri dayanan bir yol sağlar. Biçimlendirme üzerinde tam denetim verir, TDD kolay geliştirme destekler ve en son web standartlarını kullanır.

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
* [Razor görüntüleme altyapısı](#razor-view-engine)
* [Kesin türü belirtilmiş görünümleri](#strongly-typed-views)
* [Etiket Yardımcıları](#tag-helpers)
* [Görünüm bileşenleri](#view-components)

### <a name="routing"></a>Yönlendirme

ASP.NET Core MVC üstünde oluşturulan [ASP.NET Core'nın Yönlendirme](../fundamentals/routing.md), olanak sağlayan güçlü bir URL eşlemesi bileşeni anlaşılabilir ve aranabilir URL'lere sahip uygulamalar oluşturun. Bu, web sunucusundaki dosyaları nasıl düzenlendiği için bakmadan bağlantı oluşturma ve arama motoru iyileştirme (SEO) için iyi iş uygulamanızın URL adlandırma modelleri tanımlamanızı sağlar. Rota değeri kısıtlamaları, Varsayılanları ve isteğe bağlı değerler destekler uygun rota şablonu sözdizimini kullanarak yollarınızı tanımlayabilirsiniz.

*Kurala dayalı yönlendirme* URL genel tanımlamanızı biçimleri etkinleştirir, uygulamanızın kabul eder ve nasıl görüntülerin her birini biçimleri bir özel eylem yöntemine üzerinde denetleyicisi eşler. Gelen bir istek alındığında yönlendirme altyapısını URL ayrıştırır ve tanımlanan URL biçimlerinden birini için eşleşen ve ilişkili denetleyicinin eylem yöntemini çağırır.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Öznitelik yönlendirme* denetleyicileri ve eylemleri uygulamanızın yolları tanımlayın özniteliklere sahip tasarlayarak yönlendirme bilgilerini belirtmenizi sağlar. Başka bir deyişle, denetleyici ve eylem ile ilişkili oldukları yanındaki route tanımlarını yerleştirilir.

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

ASP.NET Core MVC [model bağlama](models/model-binding.md) denetleyicisi işleyebileceği nesnelerini istemci isteği verilerini (form değerleri, rota verileri, sorgu dizesi parametreleri, HTTP üstbilgileri) dönüştürür. Sonuç olarak, denetleyici mantığınızı gelen istek verileri getirme işini yapmak zorunda değildir; yalnızca kendi eylem yöntemleri için parametre olarak veri yok.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>Model doğrulama

ASP.NET Core MVC destekleyen [doğrulama](models/validation.md) , model nesnesi veri ek açıklaması doğrulama öznitelikleri ile dekorasyon tarafından. Doğrulama özniteliklerinin değerleri sunucuya gönderilen önce istemci tarafında denetlenir yanı denetleyici eylemini önce sunucuda olarak adlandırılır.

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

Framework doğrulama isteği verileri istemci ve sunucu işler. Model türlerinde belirtilen doğrulama mantığını oluşturulmuş görünümler örtük ek açıklamaları olarak eklenir ve tarayıcı ile zorlanır [jQuery doğrulama](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Bağımlılık ekleme

ASP.NET Core sahip için yerleşik destek [bağımlılık ekleme (dı)](../fundamentals/dependency-injection.md). ASP.NET Core mvc'de [denetleyicileri](controllers/dependency-injection.md) istek gerekli hizmetleri izlemek vermeden kendi oluşturucular aracılığıyla [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/).

Uygulamanızı de kullanabilirsiniz [bağımlılık ekleme görünümünde dosyaları](views/dependency-injection.md)kullanarak `@inject` yönergesi:

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

[Filtreler](controllers/filters.md) geliştiricilerin, özel durum işleme veya yetkilendirme gibi çapraz kesme sorunlarını yalıtma. Filtreleri eylem yöntemleri için çalışmakta olan özel öncesi ve sonrası işleme mantığı etkinleştirin ve belirli bir istek için yürütme ardışık düzeni içinde belirli zamanlarda çalışacak şekilde yapılandırılmış. Filtreler denetleyicileri veya öznitelik olarak Eylemler uygulanabilir (veya genel olarak çalıştırılabilir). Birkaç filtreleri (gibi `Authorize`) framework dahil edilir.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Alanları

[Alanları](controllers/areas.md) daha küçük işlevsel gruplamalarda büyük bir ASP.NET Core MVC Web uygulaması bölüm için bir yol sağlar. Bir uygulama içinde MVC yapısının bir alandır. MVC projesinde, Model, denetleyici ve görünüm gibi mantıksal bileşenlerin farklı klasörlerde tutulur ve MVC bu bileşenler arasındaki ilişki oluşturmak için adlandırma kuralları kullanır. Büyük bir uygulama için uygulama ayrı yüksek düzey alanlarına işlevlerin bölümlemek için yararlı olabilir. Örneğin, bir e-ticaret uygulamayla checkout, faturalama ve arama vb. gibi birden çok iş birimleri. Her bu birimleri kendi mantıksal bileşen görünümleri, denetleyicileri ve modeli vardır.

### <a name="web-apis"></a>Web API'leri

Web siteleri oluşturmak için harika bir platform olmaya ek olarak, ASP.NET Core MVC Web API'ları oluşturmak için harika desteğe sahiptir. Tarayıcılar ve mobil cihazlar dahil olmak üzere istemcileri geniş bir yelpazedeki ulaşmak hizmetler oluşturabilirsiniz.

HTTP İçerik anlaşma desteği için yerleşik destekle framework içeren [biçimlendirmek veri](xref:web-api/advanced/formatting) JSON veya XML olarak. Yazma [özel biçimlendiricileri](xref:web-api/advanced/custom-formatters) kendi biçimleri için destek eklemek için.

Bağlantı oluşturma iletilir desteğini etkinleştirmek için kullanın. Kolayca desteğini etkinleştirme [çıkış noktaları arası kaynak paylaşımı (CORS)](http://www.w3.org/TR/cors/) böylece Web API'leri birden çok Web uygulamaları arasında paylaşılabilir.

### <a name="testability"></a>Test Edilebilirlik

Framework'ün kullanımını arabirimleri ve bağımlılık ekleme kılmaktadır birim testi ve framework (gibi bir TestHost ve Inmemory sağlayıcısı için Entity Framework) olun özellikler içerir [tümleştirme testleri](xref:test/integration-tests) hızlı ve kolay de. Daha fazla bilgi edinmek [denetleyici mantığında test etme](controllers/testing.md).

### <a name="razor-view-engine"></a>Razor görüntüleme altyapısı

[ASP.NET Core MVC görünümleri](views/overview.md) kullanmak [Razor görüntüleme altyapısı](views/razor.md) görünümlerini işlemek için. Razor, katıştırılmış C# kod kullanarak görünümleri tanımlamak için bir compact, açıklayıcı ve sıvı şablon biçimlendirme dilidir. Razor web içeriği sunucusundaki dinamik olarak oluşturmak için kullanılır. Sunucu kodu düzgün bir şekilde, istemci tarafı içerik ve kod da karıştırabilirsiniz.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Tanımlayabilirsiniz Razor görünüm altyapısını kullanarak [düzenleri](views/layout.md), [kısmi görünümler](views/partial.md) ve değiştirebilen bölümler.

### <a name="strongly-typed-views"></a>Kesin türü belirtilmiş görünümleri

MVC Razor görünümleri kesinlikle modelinizi göre yazılabilir. Denetleyicileri kesin türü belirtilmiş bir model türü denetleme ve IntelliSense desteği sağlamak kendi görünümlerinizi etkinleştirme görünümlerine geçirebilirsiniz.

Örneğin, bir model türü aşağıdaki görünümü işleyen `IEnumerable<Product>`:

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

[Etiket Yardımcıları](views/tag-helpers/intro.md) sunucu tarafı kodu oluşturma ve Razor dosyalarında HTML öğelerin işlenmesi katılmayı etkinleştir. Özel etiketler tanımlamak için etiket Yardımcıları kullanabilirsiniz (örneğin, `<environment>`) veya varolan etiketleri davranışını değiştirmek için (örneğin, `<label>`). Öğe adı ve özniteliklerini temel alınarak belirli öğelere etiket Yardımcıları bağlayın. Düzenleme deneyimi HTML hala korurken sunucu tarafı işleme yararları sağlarlar.

Formları, bağlantılar, yükleme varlıklar ve ortak GitHub depoları ve NuGet olarak daha kullanılabilir ve daha fazla - paketleri oluşturma gibi genel görevleri - birçok yerleşik etiket Yardımcılarında vardır. Etiket Yardımcıları C# dilinde yazılan ve öğe adı, öznitelik adı veya üst etiket göre HTML öğeleri hedefleyin. Örneğin, LinkTagHelper, bir bağlantı oluşturmak için kullanılabilir yerleşik `Login` eylemi `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

`EnvironmentTagHelper` Farklı betikleri geliştirme, hazırlama ve üretim gibi çalışma zamanı ortamı göre kendi görünümlerinizi (örneğin, ham veya küçültülmüş) dahil etmek için kullanılabilir:

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

Etiket Yardımcıları bir HTML kolay geliştirme deneyimi ve HTML ve Razor biçimlendirmesi oluşturmak için zengin bir IntelliSense ortam sağlar. Yerleşik etiket Yardımcıları çoğu olan HTML öğeleri hedef ve öğe için sunucu tarafı öznitelikler sağlar.

### <a name="view-components"></a>Görünüm bileşenleri

[Görüntüleme bileşenleri](views/view-components.md) işleme mantığı paketini ve uygulama genelinde yeniden olanak sağlar. Benzer şekilde [kısmi görünümler](views/partial.md), ancak ilişkili mantığı ile.
