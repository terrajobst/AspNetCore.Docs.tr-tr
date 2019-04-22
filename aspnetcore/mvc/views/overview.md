---
title: ASP.NET Core MVC görünümleri
author: ardalis
description: Uygulamanın verilerini sunumu ve kullanıcı etkileşimine karşılık olarak ASP.NET Core MVC görünümleri nasıl işleneceğini öğrenin.
ms.author: riande
ms.date: 04/03/2019
uid: mvc/views/overview
ms.openlocfilehash: 766996645bc6ef2b6be42d729baf5d57f55b6ddd
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012805"
---
# <a name="views-in-aspnet-core-mvc"></a>ASP.NET Core MVC görünümleri

Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)

Bu belgede, ASP.NET Core MVC uygulamalarında kullanılan görünümler açıklanmaktadır. Razor sayfaları hakkında daha fazla bilgi için bkz: [Razor sayfaları giriş](xref:razor-pages/index).

Model-View-Controller (MVC) düzeninde *görünümü* uygulamanın verilerini sunumu ve kullanıcı etkileşimi işler. Bir görünümü HTML şablonudur ile katıştırılmış [Razor işaretlemesi](xref:mvc/views/razor). Razor işaretlemesi istemciye gönderilen bir Web sayfası oluşturmak için bir HTML biçimlendirmesi ile etkileşime giren kodudur.

ASP.NET Core MVC görünümleri olan *.cshtml* dosyaları kullanan [C# programlama dilinin](/dotnet/csharp/) Razor biçimlendirme. Genellikle, her uygulama için klasörleri dosyalarını görüntüle gruplanmıştır [denetleyicileri](xref:mvc/controllers/actions). Klasörleri depolanan bir *görünümleri* uygulamanın kök klasörü:

![Visual Studio Çözüm Gezgini görünümlerini klasörü giriş klasörünün About.cshtml Contact.cshtml ve Index.cshtml dosyaları göstermek için açık açıktır](overview/_static/views_solution_explorer.png)

*Giriş* denetleyici tarafından temsil edilen bir *giriş* klasörün içine *görünümleri* klasör. *Giriş* klasörde görünümleri *hakkında*, *kişi*, ve *dizin* (giriş) Web sayfaları. Bir kullanıcı istediğinde bu üç Web sayfaları, denetleyici eylemleri birini *giriş* denetleyicisini belirleyin, üç görünüm oluşturun ve kullanıcıya bir Web sayfasını döndürmek için kullanılır.

Kullanım [düzenleri](xref:mvc/views/layout) tutarlı Web Bölümleri sağlamak ve kod tekrarından azaltın. Düzenleri, genellikle üst bilgi, gezinti ve menü öğeleri ve alt bilgi içerir. Genellikle üstbilgi ve altbilgi Demirbaş biçimlendirme çok sayıda meta veri öğeleri ve betik, stil ve varlıklar bağlantılar içerir. Düzenleri, bu ortak biçimlendirme, görünümlerde önlemenize yardımcı.

[Kısmi görünümler](xref:mvc/views/partial) görünümleri yeniden kullanılabilir bölümlerini yöneterek kod yinelemesinden azaltın. Örneğin, kısmi görünüm, birden fazla görünümlerde bir blog Web sitesinde bir yazar biyografisi yararlıdır. Yazar biyografisi normal görünüm içeriği ve Web sayfasının içeriğini üretmek için yürütmek için kod gerekmez. Yazar biyografisi içeriği görünüm tarafından tek başına, model bağlama için kullanılabilir olduğundan kısmi bir görünümü kullanarak bu içerik türü için idealdir.

[Görüntüleme bileşenleri](xref:mvc/views/view-components) tekrarlanan kodları azaltabilir size izin vermesi için kısmi benzer görünümleridir, ancak bunlar Web sayfasını işlemek için sunucuda çalıştırmak için kod gerektiren görünümü içeriğini uygun. Görünüm bileşenleri işlenmiş içeriği gerektiren alışveriş sepeti bir Web sitesi için veritabanı etkileşimi gibi olduğunda yararlıdır. Görünüm bileşenleri Web sayfası çıktı oluşturmak için model bağlama için sınırlı değildir.

## <a name="benefits-of-using-views"></a>Görünümleri kullanmanın avantajları

Görünümleri yardımcı kurmak için [görev ayrımı nettir](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) uygulamanın diğer bölümlerden kullanıcı arabirimi biçimlendirme ayırarak bir MVC uygulaması içinde. SoC tasarım aşağıdaki uygulamanızı çeşitli avantajlar sağlayan modüler, yapar:

* Uygulama, daha iyi organize çünkü korumak daha kolay olur. Görünümler, genel olarak app özelliği tarafından gruplandırılır. Bu, bir özellik üzerinde çalışırken ilgili görünümleri bulmayı kolaylaştırır.
* Uygulama bölümleri birbirine sıkı şekilde bağlı. Derleme ve iş mantığı ve verileri erişim bileşenleri ayrı olarak uygulamanın görünümleri güncelleştirin. Uygulama görünümlerini mutlaka uygulamanın diğer bölümlerini güncelleştirmek zorunda kalmadan değiştirebilirsiniz.
* Uygulama kullanıcı arabirimi bölümlerini görünümleri ayrı birim olduğundan test etmek daha kolaydır.
* Daha iyi düzenleme nedeniyle yanlışlıkla kullanıcı arabirimi bölümlerini tekrarlayacaksınız daha az olasıdır.

## <a name="creating-a-view"></a>Bir görünümü oluşturma

Bir denetleyiciye özgü görünümler oluşturulur *görünümler / [ControllerName]* klasör. Görünümleri ve denetleyicileri arasında paylaşılan yerleştirildiğinde *görünümler/paylaşılan* klasör. Bir görünüm oluşturmak için yeni bir dosya ekleyin ve ilişkili denetleyici eylemi ile aynı adı verin *.cshtml* dosya uzantısı. Karşılık gelen bir görünüm oluşturmak için *hakkında* eylemde *giriş* denetleyicisi oluşturma bir *About.cshtml* dosyası *görünümler/giriş*klasörü:

[!code-cshtml[](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* biçimlendirme ile başlayan `@` simgesi. C# yerleştirerek çalıştırma C# deyimleri kod içinde [Razor kod blokları](xref:mvc/views/razor#razor-code-blocks) küme ayraçları ayarlayın (`{ ... }`). Örneğin, "Hakkında" ataması için bkz: `ViewData["Title"]` yukarıda gösterilen. Değeriyle başvurarak HTML'de değerleri gösterebilen `@` simgesi. İçeriğini görmek `<h2>` ve `<h3>` yukarıdaki öğeler.

Yukarıda gösterilen içeriği görüntüleme, kullanıcı tarafından işlenen tüm Web sayfasının yalnızca bir parçası olur. Sayfa düzeninin geri kalan ve görünüm ortak diğer yönleri diğer görünüm dosyalarında belirtilir. Daha fazla bilgi için bkz. [Düzen konu](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Denetleyicileri görünümlerin nasıl belirtin

Görünüm eylemlerine genellikle döndürülür bir [ViewResult](/dotnet/api/microsoft.aspnetcore.mvc.viewresult), bir tür olduğu [actionresult öğesini](/dotnet/api/microsoft.aspnetcore.mvc.actionresult). Eylem yönteminizi oluşturabilir ve dönüş bir `ViewResult` doğrudan, ancak değil, yaygın olarak yapılır. Çoğu denetleyicileri devralınacak beri [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller), yalnızca kullandığınız `View` döndürmek için yardımcı yöntem `ViewResult`:

*HomeController.cs*

[!code-csharp[](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Bu eylem geri döndüğünde, *About.cshtml* son bölümde gösterilen görünümü, aşağıdaki Web sayfası işlenir:

![Edge tarayıcısında işlenen sayfası hakkında](overview/_static/about-page.png)

`View` Yardımcı yöntem olan çeşitli aşırı yükler. İsteğe bağlı olarak belirtebilirsiniz:

* Döndürülecek açık bir görünümü:

  ```csharp
  return View("Orders");
  ```

* A [modeli](xref:mvc/models/model-binding) görünüme iletmek için:

  ```csharp
  return View(Orders);
  ```

* Bir görünümü ve bir model için:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Görünüm bulma

Görünüm bir eylemi geri döndüğünde, adlandırılan bir işlem *görünümü bulma* gerçekleşir. Bu işlem, hangi görünüm dosyası görünüm adını temel alarak kullanıldığını belirler. 

Varsayılan davranışını `View` yöntemi (`return View();`) içinden çağırıldığında eylem yöntemi olarak aynı ada sahip bir görünüm getirmektir. Örneğin, *hakkında* `ActionResult` denetleyicinin yöntemi adı adlı bir görünüm dosyayı aramak için kullanılan *About.cshtml*. İlk olarak, çalışma zamanı arar *görünümler / [ControllerName]* klasör görünümü. Eşleşen görünüm yok bulamazsa, aradığı *paylaşılan* klasör görünümü.

Dolaylı olarak dönüş yaparsa farketmez `ViewResult` ile `return View();` veya Görünüm adına açıkça geçirebilirsiniz `View` yöntemiyle `return View("<ViewName>");`. Her iki durumda da Görünüm bulma eşleşen bir görünüm dosyası şu sırayla arar:

   1. *Görünümler /\[ControllerName] /\[Görünümadı] .cshtml*
   1. *Görünümler/paylaşılan/\[Görünümadı] .cshtml*

Bir görünümü dosya yolu yerine bir görünüm adı sağlanabilir. Uygulama kökü başlatılıyor mutlak yol kullanıyorsanız (isteğe bağlı olarak başlayarak "/" veya "~ /"), *.cshtml* uzantı belirtilmelidir:

```csharp
return View("Views/Home/About.cshtml");
```

Görünümler farklı dizinlerde belirtmek için göreli bir yol kullanabilirsiniz *.cshtml* uzantısı. İçinde `HomeController`, geri dönebilirsiniz *dizin* görünümünü, *Yönet* göreli yolu içeren görünümler:

```csharp
return View("../Manage/Index");
```

Benzer şekilde, geçerli denetleyici özgü dizinle belirtebilir ". /" ön eki:

```csharp
return View("./About");
```

[Kısmi görünümler](xref:mvc/views/partial) ve [görüntülemek bileşenleri](xref:mvc/views/view-components) benzer (ancak aynı değildir) bulma mekanizmasını kullanın.

Nasıl görünümleri kullanarak özel bir uygulama içinde bulunan varsayılan kural özelleştirebilirsiniz [IViewLocationExpander](/dotnet/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Görünüm bulma görünümü dosyaları dosya adına göre bulma üzerinde kullanır. Temeldeki dosya sistemi büyük/küçük harfe duyarlı ise, görünüm adları büyük olasılıkla büyük/küçük harfe duyarlıdır. İşletim sistemleri arasında uyumluluk için denetleyici ve eylem adları ve ilişkili görünüme klasörleri ve dosya adları arasında harf. Bulunamayan bir görünüm dosyası büyük küçük harfe duyarlı dosya sistemi ile çalışırken bir hatayla karşılaşırsanız, büyük/küçük harf gerçek görünümü dosya adı ve istenen görünüm dosyası arasında eşleştiğini doğrulayın.

Denetleyicileri, işlemler ve Bakım ve açıklık için görünümler arasındaki ilişkiyi yansıtmak kendi görünümlerinizi dosya yapısını düzenleme en iyi izleyin.

## <a name="passing-data-to-views"></a>Görünüm veri geçirme

Veri çeşitli yaklaşımlar kullanılarak geçer:

* Veri türü kesin: viewmodel
* Zayıf sahipli türü belirtilmiş veri
  * `ViewData` (`ViewDataAttribute`)
  * `ViewBag`

### <a name="strongly-typed-data-viewmodel"></a>Kesin türü belirtilmiş veri (viewmodel)

Belirtmek için en güçlü yaklaşımdır bir [modeli](xref:mvc/models/model-binding) görünümünde türü. Bu model, genellikle olarak adlandırılır bir *viewmodel*. Eylem görünümü viewmodel türün bir örneğini geçirin.

Bir görünüme veri iletmek için bir viewmodel kullanarak yararlanmak görünüm sağlayan *güçlü* tür denetimi. *Güçlü yazım, yazım* (veya *kesin tür belirtilmiş*) her değişken ve sabit açıkça tanımlanmış bir tür olduğu anlamına gelir (örneğin, `string`, `int`, veya `DateTime`). Bir görünümde kullanılan türler geçerliliğini derleme sırasında denetlenir.

[Visual Studio](https://www.visualstudio.com/vs/) ve [Visual Studio Code](https://code.visualstudio.com/) denilen bir özelliği kullanarak türü kesin belirlenmiş sınıf üyelerini listeleyin [IntelliSense](/visualstudio/ide/using-intellisense). Bir viewmodel özelliklerini görmek istediğinizde, bir nokta viewmodel değişken adını yazın (`.`). Bu kod, daha az hatayla daha hızlı yazmanıza yardımcı olur.

Kullanarak bir model belirtin `@model` yönergesi. Modelinizle `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Modeli görünüme sağlamak için denetleyici bu parametre olarak geçirir:

```csharp
public IActionResult Contact()
{
    ViewData["Message"] = "Your contact page.";

    var viewModel = new Address()
    {
        Name = "Microsoft",
        Street = "One Microsoft Way",
        City = "Redmond",
        State = "WA",
        PostalCode = "98052-6399"
    };

    return View(viewModel);
}
```

Bir görünüm sağlayabilir model türleri ilgili bir kısıtlama yoktur. Düz eski CLR nesnesi (POCO) viewmodel'lar tanımlanan çok az kayıpla veya hiç davranışı ile (yöntem) kullanmanızı öneririz. Genellikle, viewmodel sınıfları ya da depolanan *modelleri* klasör veya ayrı bir *Viewmodel'lar* uygulamanın kök klasör. *Adresi* yukarıdaki örnekte kullanılan viewmodel olan adındaki bir dosyada depolanan bir POCO viewmodel *Address.cs*:

```csharp
namespace WebApplication1.ViewModels
{
    public class Address
    {
        public string Name { get; set; }
        public string Street { get; set; }
        public string City { get; set; }
        public string State { get; set; }
        public string PostalCode { get; set; }
    }
}
```

Hiçbir şey viewmodel türleri ve iş modeli türleriniz için aynı sınıf kullanmasını önler. Ancak, ayrı modelleri kullanarak görünümlerinizde uygulamanızın bölümlerini erişim iş mantığı ve verileri bağımsız olarak değiştirmenizi sağlar. Modelleri ve viewmodel'lar ayrımı sunduğu güvenlik açısından faydalı modelleri kullandığınızda [model bağlama](xref:mvc/models/model-binding) ve [doğrulama](xref:mvc/models/validation) uygulamaya kullanıcı tarafından gönderilen veriler için.

<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-viewdata-attribute-and-viewbag"></a>Veri'zayıf yazılmış (ViewData, ViewData özniteliği ve ViewBag)

`ViewBag` *Razor sayfaları içinde kullanılamaz.*

Kesin türü belirtilmiş görünümler yanı sıra görünümleri erişimi bir *zayıf yazılmış* (olarak da adlandırılan *gevşek yazılmış*) veri koleksiyonu. Tanımlayıcı türlerinin aksine *zayıf türlerine* (veya *kaybetmiş türleri*) kullandığınız veri türünü açıkça bildirmeyin anlamına gelir. Küçük miktarlarda veri denetleyicilerine ve görünümleri içine ve dışına geçirmek için zayıf sahipli türü belirtilmiş veri koleksiyonunu kullanabilirsiniz.

| Arasında veri geçirme bir...                        | Örnek                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Denetleyici ve bir görünüm                             | Bir açılan listedeki verilerle dolduruluyor.                                          |
| Görünüm ve [görünümü](xref:mvc/views/layout)   | Ayarı  **\<başlığı >** düzeni görünümünde bir görünüm dosyasından öğe içeriği.  |
| [Kısmi Görünüm](xref:mvc/views/partial) ve bir görünüm | Kullanıcının istenen Web sayfasına göre verileri görüntüleyen bir pencere öğesi.      |

Bu koleksiyonu aracılığıyla başvurulabilir `ViewData` veya `ViewBag` denetleyicileri ve görünümleri özellikleri. `ViewData` Özelliktir zayıf yazılan nesnelerin bir sözlük. `ViewBag` Özelliği çevresinde bir sarmalayıcı `ViewData` Dinamik özellikler için temel sağlayan `ViewData` koleksiyonu. Not: Anahtar aramaları için hem de büyük küçük harf duyarsız `ViewData` ve `ViewBag`.

`ViewData` ve `ViewBag` çalışma zamanında dinamik olarak çözümlenir. Derleme zamanı tür denetimini sundukları yoksa, her ikisi de genellikle daha bir viewmodel kullanmaktan hataya olduğundan. Bu nedenle, en düşük düzeyde ya da hiç kullanmak bazı geliştiriciler tercih `ViewData` ve `ViewBag`.

<a name="VD"></a>

**ViewData**

`ViewData` olan bir [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) aracılığıyla erişilen nesne `string` anahtarları. Dize verileri depolanır ve bir atama gerek kalmadan doğrudan kullanılır, ancak diğer dönüştürmelisiniz `ViewData` bunları ayıkladığınızda değerleri belirli türler için nesne. Kullanabileceğiniz `ViewData` denetleyicilerinden, görünümlere ve görünümler içinde veri iletmek için [kısmi görünümler](xref:mvc/views/partial) ve [düzenleri](xref:mvc/views/layout).

Bir karşılama ve kullanarak bir adres ilişkin değerleri ayarlayan bir örnek verilmiştir `ViewData` doğan:

```csharp
public IActionResult SomeAction()
{
    ViewData["Greeting"] = "Hello";
    ViewData["Address"]  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

Bir görünümdeki veriler ile çalışır:

```cshtml
@{
    // Since Address isn't a string, it requires a cast.
    var address = ViewData["Address"] as Address;
}

@ViewData["Greeting"] World!

<address>
    @address.Name<br>
    @address.Street<br>
    @address.City, @address.State @address.PostalCode
</address>
```

::: moniker range=">= aspnetcore-2.1"

**ViewData özniteliği**

Kullanan başka bir yaklaşım [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) olduğu [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute). Denetleyicilerde veya modellerde Razor sayfası özelliklerini düzenlenmiş ile `[ViewData]` sözlükten yüklendi ve depolanan değerlerine sahip.

Aşağıdaki örnekte giriş denetleyicisini içeren bir `Title` özelliği düzenlenmiş ile `[ViewData]`. `About` Yöntemi hakkında görünümü başlığını ayarlar:

```csharp
public class HomeController : Controller
{
    [ViewData]
    public string Title { get; set; }

    public IActionResult About()
    {
        Title = "About Us";
        ViewData["Message"] = "Your application description page.";

        return View();
    }
}
```

Hakkında Görünümü'nde erişim `Title` özelliği model özelliği olarak:

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

**Görünüm Paketi**

`ViewBag` *Razor sayfaları içinde kullanılamaz.*

`ViewBag` olan bir [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) depolanan nesnelere dinamik erişim sağlayan nesne `ViewData`. `ViewBag` atama gerektirmeyen bu yana birlikte çalışmak daha kullanışlı olabilir. Aşağıdaki örnek nasıl kullanılacağını gösterir `ViewBag` kullanarak aynı sonucu ile `ViewData` yukarıda:

```csharp
public IActionResult SomeAction()
{
    ViewBag.Greeting = "Hello";
    ViewBag.Address  = new Address()
    {
        Name = "Steve",
        Street = "123 Main St",
        City = "Hudson",
        State = "OH",
        PostalCode = "44236"
    };

    return View();
}
```

```cshtml
@ViewBag.Greeting World!

<address>
    @ViewBag.Address.Name<br>
    @ViewBag.Address.Street<br>
    @ViewBag.Address.City, @ViewBag.Address.State @ViewBag.Address.PostalCode
</address>
```

**ViewData ve ViewBag aynı anda kullanma**

`ViewBag` *Razor sayfaları içinde kullanılamaz.*

Bu yana `ViewData` ve `ViewBag` aynı temel alınan bakın `ViewData` koleksiyonu, her ikisi de kullanabilirsiniz `ViewData` ve `ViewBag` karışımı ve bunlar arasında okurken ve yazarken değerleri aynı.

Başlığı kullanılarak ayarlanan `ViewBag` ve açıklaması kullanarak `ViewData` üst kısmında bir *About.cshtml* görüntüle:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Özelliklerini okumak istiyorum, ancak kullanımını ters `ViewData` ve `ViewBag`. İçinde *_Layout.cshtml* dosya, başlığı kullanılarak elde `ViewData` ve açıklaması kullanarak elde `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Dizeleri için bir tür dönüştürme gerektirmeyen unutmayın `ViewData`. Kullanabileceğiniz `@ViewData["Title"]` olmadan.

Her ikisini de kullanarak `ViewData` ve `ViewBag` adresindeki karıştırma ve okuma ve yazma özellikleri eşleştirme yoksa olarak aynı zaman çalışır. Aşağıdaki biçimlendirmede oluşturulur:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**ViewData ViewBag arasındaki farkları özeti**

 `ViewBag` Razor sayfaları kullanılamaz.

* `ViewData`
  * Öğesinden türetilen [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary), gibi yararlı olabileceği sözlük özelliklerine sahip `ContainsKey`, `Add`, `Remove`, ve `Clear`.
  * Sözlükteki anahtarların dizeleri olduğundan boşluğa izin. Örnek: `ViewData["Some Key With Whitespace"]`
  * Dışında herhangi türdeki bir `string` kullanmak için görünümünde dönüştürülmelidir `ViewData`.
* `ViewBag`
  * Öğesinden türetilen [DynamicViewData](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), böylece noktalı gösterim kullanılarak dinamik özellikler oluşturulmasını sağlar (`@ViewBag.SomeKey = <value or object>`), ve hiçbir atama gereklidir. Söz dizimi `ViewBag` denetleyicileri ve görünümleri eklemek daha hızlı hale getirir.
  * Null değerler için basit. Örnek: `@ViewBag.Person?.Name`

**ViewData veya ViewBag ne zaman kullanılacağını**

Her ikisi de `ViewData` ve `ViewBag` eşit olarak küçük miktarlarda veri denetleyicilerine ve görünümleri arasında geçirmek için geçerli bir yaklaşım olan. Hangisinin kullanılacağını Seçimi tercihine göre hesaplanmıştır. Karıştırın ve eşleşen `ViewData` ve `ViewBag` nesneleri, ancak kodu, okunması ve düzenlenmesi tutarlı bir şekilde kullanılan bir yaklaşım ile daha kolay. Her iki yaklaşım, çalışma zamanında dinamik olarak çözümlenen ve çalışma zamanı hatalarına neden dolayısıyla saldırıya. Bazı geliştirme ekipleri, bunların kaçının.

### <a name="dynamic-views"></a>Dinamik görünümler

Bir model bildirmeyin görünümleri türü kullanarak `@model` ancak geçirilen bir model örneği varsa (örneğin, `return View(Address);`) örneğin özellikleri dinamik olarak başvurabilirsiniz:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Bu özellik üst düzeyde esneklik sunar, ancak derleme koruma veya IntelliSense sunmaz. Web sayfası oluşturma özelliği yoksa, çalışma zamanında başarısız olur.

## <a name="more-view-features"></a>Daha fazla özelliklerini görüntüleyin

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) sunucu tarafı davranışı mevcut HTML etiket eklemek kolaylaştırır. Etiket Yardımcıları kullanarak özel kod veya kendi görünümlerinizi içinde Yardımcıları yazmak için gereksinimini ortadan kaldırır. Etiket Yardımcıları öznitelik olarak HTML öğeleri için uygulanır ve bunları işleyemiyor düzenleyiciler tarafından göz ardı edilir. Bu, düzenlemek ve Araçlar çeşitli görünümü biçimlendirmesi oluşturmak sağlar.

Özel HTML biçimlendirme oluşturmak çok sayıda yerleşik HTML Yardımcıları ile gerçekleştirilebilir. Daha karmaşık kullanıcı arabirimi mantığı tarafından işlenebilir [görünüm bileşenleri](xref:mvc/views/view-components). Görünüm bileşenleri aynı SoC bu denetleyicileri sağlayın ve görünümler sunar. Bunlar, Eylemler ve ortak kullanıcı arabirim öğeleri tarafından kullanılan veri uğraşmanız görünümleri gereğini ortadan kaldırabilir.

Çok sayıda diğer yönleri ASP.NET Core gibi görünümleri desteği [bağımlılık ekleme](xref:fundamentals/dependency-injection), hizmetler sağlayan [görünümlere eklenmiş](xref:mvc/views/dependency-injection).
