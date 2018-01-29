---
title: "ASP.NET Core MVC görünümlerde"
author: ardalis
description: "Görünümler, uygulamanın veri sunumu ve ASP.NET Core MVC kullanıcı etkileşimini nasıl işleneceğini öğrenin."
ms.author: riande
manager: wpickett
ms.date: 12/12/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/overview
ms.openlocfilehash: b88c3ea05e37a82c7bcbde953c10a04ce1943dc5
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2018
---
# <a name="views-in-aspnet-core-mvc"></a>ASP.NET Core MVC görünümlerde

Tarafından [Steve Smith](https://ardalis.com/) ve [Luke Latham](https://github.com/guardrex)

Bu belgede ASP.NET Core MVC uygulamalarında kullanılan görünümler açıklanmaktadır. Razor sayfalarında daha fazla bilgi için bkz: [Razor sayfalarının giriş](xref:mvc/razor-pages/index).

İçinde **M**odel -**V**görünümü -**C**ontroller (MVC) deseni *Görünüm* uygulamanın veri sunu ve kullanıcı etkileşimini işler. Bir HTML şablonu görünümdür ile katıştırılmış [Razor biçimlendirme](xref:mvc/views/razor). Razor biçimlendirme istemciye gönderilen bir Web sayfası oluşturmak için HTML biçimlendirmesi etkileşimde kodudur.

ASP.NET Core MVC'de görünümlerdir *.cshtml* dosyaları kullanan [C# programlama dili](/dotnet/csharp/) Razor biçimlendirme. Genellikle, her uygulamanın adlı klasörlere dosyaları görüntüle gruplandırılmış [denetleyicileri](xref:mvc/controllers/actions). Klasörleri depolanmış bir *görünümleri* uygulama kökünde klasör:

![Visual Studio Çözüm Gezgini görünümler klasöründe giriş klasörü About.cshtml, Contact.cshtml ve Index.cshtml dosyaları göstermek için açık açık](overview/_static/views_solution_explorer.png)

*Giriş* denetleyicisi ile temsil edilir bir *giriş* klasöre *görünümleri* klasör. *Giriş* görünümleri içeren klasör *hakkında*, *kişi*, ve *dizin* (giriş sayfası) Web sayfaları. Bir kullanıcı isteğinde bulunduğunda bu üç Web sayfaları, denetleyici eylemleri birini *giriş* denetleyicisi belirlemek, üç görünüm oluşturmak ve bir Web sayfası kullanıcıya döndürmek için kullanılır.

Kullanım [düzenleri](xref:mvc/views/layout) tutarlı Web sayfası bölümler sağlar ve kod yineleme azaltmak için. Düzenleri genellikle üstbilgi, gezinme ve menü öğeleri ve alt bilgi içerir. Üstbilgi ve altbilgi genellikle Demirbaş biçimlendirme çok sayıda meta veri öğeleri ve komut dosyası ve stil varlıklar bağlantılar içerir. Düzenleri görünümlerinizi bu Demirbaş biçimlendirme önlemenize yardımcı.

[Kısmi görünümler](xref:mvc/views/partial) görünümleri yeniden kullanılabilir bölümlerini yöneterek kod yinelemesinden azaltın. Örneğin, kısmi görünüm, birkaç görünümlerde bir blog Web sitesinde Yazar biyografisi yararlıdır. Yazar biyografisi sıradan görünüm içeriğin ve Web sayfasının içeriği üretmek için yürütmek için kodu gerektirmez. Bu içerik türü için kısmi görünüm kullanılması idealdir şekilde yazar biyografisi içerik görünüm tek başına, model bağlama tarafından kullanılabilir.

[Görüntüleme bileşenleri](xref:mvc/views/view-components) yineleyen kod azaltmanızı izin vermesi benzeyen kısmi görünümler, ancak bu Web sayfası oluşturmak için sunucuda çalıştırmak için kod gerektiren görünümü içerik için uygun. İşlenmiş içeriği gerektirdiğinde alışveriş sepeti bir Web sitesi için veritabanı etkileşimini gibi görünüm bileşenleri yararlı olur. Görünüm bileşenleri Web sayfası bir çıktı oluşturmak için bağlama modellemek için sınırlı değildir.

## <a name="benefits-of-using-views"></a>Görünümleri kullanmanın yararları

Görünümleri Yardım kurmak için bir [ **S**eparation **o**f **C**oncerns (SoC) tasarım](http://deviq.com/separation-of-concerns/) kullanıcı arabirimi biçimlendirmeden ayıran bir MVC uygulaması içindeki uygulamanın diğer bölümleri. SoC tasarım aşağıdaki uygulamanızı birkaç avantaj sağlayan modüler yapar:

* Uygulama, daha iyi organize çünkü korumak kolaydır. Görünümleri genellikle uygulama özelliği tarafından gruplandırılır. Bu, bir özellik çalışırken ilgili görünümleri bulmayı kolaylaştırır.
* Uygulama bölümlerini gevşek. Derleme ve uygulamanın görünümleri ayrı ayrı iş mantığı ve verileri erişim bileşenleri güncelleştirebilirsiniz. Uygulamanın diğer bölümleri güncelleştirmek mutlaka zorunda kalmadan uygulama görünümlerini değiştirebilirsiniz.
* Uygulama kullanıcı arabirimi bölümlerini görünümleri ayrı birimi olduğundan test daha kolaydır.
* Daha iyi düzenleme nedeniyle kullanıcı arabiriminin yanlışlıkla yineleme bölümleri gerekir daha az olasıdır.

## <a name="creating-a-view"></a>Bir görünümü oluşturma

Bir denetleyiciye özgü görünümler oluşturulan *görünümler / [controllername öğesi]* klasör. Denetleyicileri arasında paylaşılan görünümleri yerleştirilir *görünümler/paylaşılan* klasör. Bir görünüm oluşturmak için yeni bir dosya ekleyin ve onun ilişkili denetleyici eylemi ile aynı adı vermek *.cshtml* dosya uzantısı. Karşılık gelen bir görünüm oluşturmak için *hakkında* eylemde *giriş* denetleyicisi oluşturma bir *About.cshtml* dosyasını *görünümler/giriş*klasörü:

[!code-cshtml[Main](../../common/samples/WebApplication1/Views/Home/About.cshtml)]

*Razor* biçimlendirme ile başlayan `@` simgesi. C# yerleştirerek çalıştırma C# deyimleri kod içinde [Razor kod blokları](xref:mvc/views/razor#razor-code-blocks) küme ayraçları ayarlayın (`{ ... }`). Örneğin, "Hakkında" atanması için bkz: `ViewData["Title"]` yukarıda gösterilen. HTML içindeki değerleri değeriyle başvurarak görüntüleyebilirsiniz `@` simgesi. İçeriğini görmek `<h2>` ve `<h3>` yukarıdaki öğeler.

Yukarıda gösterilen görünüm içeriği kullanıcı için işlenen tüm Web sayfası yalnızca bir parçası olur. Sayfanın düzenini geri kalanı ve diğer genel görünümü yönlerini diğer görünüm dosyalarında belirtilmiş. Daha fazla bilgi için bkz: [düzeni konu](xref:mvc/views/layout).

## <a name="how-controllers-specify-views"></a>Denetleyicileri görünümleri nasıl belirtin

Görünümleri eylemlerine genellikle döndürülür bir [ViewResult](/aspnet/core/api/microsoft.aspnetcore.mvc.viewresult), bir tür olduğu [ActionResult](/aspnet/core/api/microsoft.aspnetcore.mvc.actionresult). Eylem yöntemi oluşturun ve dönüş bir `ViewResult` doğrudan, ancak değil, yaygın olarak yapılır. Çoğu denetleyicileri devralınmalıdır beri [denetleyicisi](/aspnet/core/api/microsoft.aspnetcore.mvc.controller), yalnızca kullandığınız `View` dönmek için yardımcı yöntem `ViewResult`:

*HomeController.cs*

[!code-csharp[Main](../../common/samples/WebApplication1/Controllers/HomeController.cs?highlight=5&range=16-21)]

Bu eylem geri döndüğünde, *About.cshtml* son bölümünde gösterilen görünüm aşağıdaki Web sayfası işlenen:

![Edge tarayıcısında çizilir sayfası hakkında](overview/_static/about-page.png)

`View` Yardımcı yöntem birçok aşırı yüklemeye sahip. İsteğe bağlı olarak belirtebilirsiniz:

* Döndürülecek açık bir görünümü:

  ```csharp
  return View("Orders");
  ```
* A [modeli](xref:mvc/models/model-binding) görünüme iletmek için:

  ```csharp
  return View(Orders);
  ```
* Bir görünüm ve model için:

  ```csharp
  return View("Orders", Orders);
  ```

### <a name="view-discovery"></a>Görünüm bulma

Bir eylem bir görünüm döndürdüğünde adlı bir işlem *görünüm bulma* gerçekleşmesi. Bu işlem, hangi görünüm dosyası görünüm adını temel alarak kullanıldığını belirler. 

Varsayılan davranışını `View` yöntemi (`return View();`) içinden çağırıldığında eylem yöntemi aynı ada sahip bir görünüm getirmektir. Örneğin, *hakkında* `ActionResult` denetleyicinin yöntemi adı adlı bir görünüm dosyayı aramak için kullanılan *About.cshtml*. İlk olarak, çalışma zamanı görünür *görünümler / [controllername öğesi]* görünümü için klasör. Eşleşen bir görünüm vardır bulamazsa arama *paylaşılan* görünümü için klasör.

Örtük olarak döndürürse önemli değildir `ViewResult` ile `return View();` veya Görünüm adı açıkça geçirmek `View` yöntemiyle `return View("<ViewName>");`. Her iki durumda da, bu sırada eşleşen bir görünüm dosyası görünüm bulma arar:

   1. *Görünümler /\[controllername öğesi]\[Görünümadı] .cshtml*
   1. *Görünümler/paylaşılan/\[Görünümadı] .cshtml*

Bir görünüm adı yerine bir görünüm dosya yolu sağlanabilir. Uygulama kök dizininde başlayan mutlak bir yol kullanıyorsanız (isteğe bağlı olarak başlayarak "/" veya "~ /"), *.cshtml* uzantısı belirtilmesi gerekir:

```csharp
return View("Views/Home/About.cshtml");
```

Görünümler farklı dizinlerde belirtmek için göreli bir yol kullanabilirsiniz *.cshtml* uzantısı. İçinde `HomeController`, geri dönebilirsiniz *dizin* görünümünü, *Yönet* göreli bir yol görünümlerle:

```csharp
return View("../Manage/Index");
```

Benzer şekilde, geçerli denetleyici özgü dizinle belirtebilir ". /" öneki:

```csharp
return View("./About");
```

[Kısmi görünümler](xref:mvc/views/partial) ve [görüntülemek bileşenleri](xref:mvc/views/view-components) benzer (ancak aynı) bulma mekanizmasını kullanın.

Nasıl görünümleri özel kullanarak uygulama içinde bulunur ve varsayılan kuralını özelleştirebilirsiniz [IViewLocationExpander](/aspnet/core/api/microsoft.aspnetcore.mvc.razor.iviewlocationexpander).

Görünüm bulma görünümü dosyaları dosya adına göre bulma kullanır. Temeldeki dosya sistemi büyük küçük harfe duyarlı ise, görünüm adları büyük olasılıkla büyük küçük harf duyarlıdır. İşletim sistemleri arasında uyumluluk için denetleyici ve eylem adları ve ilişkili görünüm klasörleri ve dosya adları arasında büyük küçük harf duyarlı. Bulunamayan bir görünüm dosyası büyük küçük harfe duyarlı dosya sistemi ile çalışırken bir hatayla karşılaşırsanız, büyük küçük harf istenen görünüm dosyası ve gerçek görünümü dosya adı arasında eşleştiğini doğrulayın.

Denetleyicileri, işlemler ve Bakım ve daha anlaşılır olması için görünümler arasındaki ilişkiyi yansıtmak kendi görünümlerinizi dosya yapısı düzenleme en iyi uygulama olarak izleyin.

## <a name="passing-data-to-views"></a>Veri görünümleri geçirme

Çeşitli yaklaşımlar kullanarak görünümleri için veri geçirebilirsiniz. En güçlü belirtmek için bir yaklaşımdır bir [modeli](xref:mvc/models/model-binding) görünümünde türü. Bu model, genellikle olarak adlandırılır bir *viewmodel*. Viewmodel türünün bir örneği eylemden görünümüne geçin.

Bir görünüme veri iletmek için bir viewmodel kullanarak sağlar yararlanmak için görünümü *güçlü* türü denetleniyor. *Güçlü yazarak* (veya *kesin türü belirtilmiş*) her değişken ve sabiti açıkça tanımlanmış bir türü olduğu anlamına gelir (örneğin, `string`, `int`, veya `DateTime`). Bir görünümde kullanılan türleri geçerliliğini derleme zamanında denetlenir.

[Visual Studio](https://www.visualstudio.com/vs/) ve [Visual Studio Code](https://code.visualstudio.com/) denilen bir özelliği kullanarak türü kesin belirlenmiş sınıf üyeleri liste [IntelliSense](/visualstudio/ide/using-intellisense). Bir viewmodel özelliklerini görmek istediğinizde, ardından bir noktayla viewmodel değişken adını yazın (`.`). Bu kodu daha az hatalarla daha hızlı yazmanıza yardımcı olur.

Kullanarak bir model belirtin `@model` yönergesi. Model ile kullanmak `@Model`:

```cshtml
@model WebApplication1.ViewModels.Address

<h2>Contact</h2>
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Modeli görünüme sağlamak için denetleyici, bir parametre olarak geçirir:

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

Bir görünüme sağlayabilir modeli türlerinde bir kısıtlama yoktur. Kullanmanızı öneririz **P**larak **O**ld **C**LR **O**nesne (POCO) viewmodels tanımlanan çok az kayıpla veya hiç davranışları (yöntemleri) sahip. Genellikle, viewmodel sınıflar ya da depolanmış *modelleri* klasör veya ayrı bir *ViewModels* uygulama kökünde klasör. *Adresi* yukarıdaki örnekte kullanılan viewmodel olan adındaki bir dosyada depolanan bir POCO viewmodel *Address.cs*:

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

> [!NOTE]
> Hiçbir şey viewmodel türleri ve iş modeli türleri için aynı sınıfları kullanmalarını engeller. Ancak, ayrı modelleri kullanarak iş mantığı ve verileri, uygulamanızın erişim bölümleri bağımsız olarak değiştirmek kendi görünümlerinizi sağlar. Modelleri ve viewmodels ayrılması sunduğu güvenlik avantajlarından modelleri kullandığınızda [model bağlama](xref:mvc/models/model-binding) ve [doğrulama](xref:mvc/models/validation) uygulama için kullanıcı tarafından gönderilen veriler için.


<a name="VD_VB"></a>

### <a name="weakly-typed-data-viewdata-and-viewbag"></a>Zayıf yazılmış verileri (ViewData ve Görünüm Paketi)

Not: `ViewBag` Razor sayfalarında kullanılamaz.

Kesin türü belirtilmiş görünümleri yanı sıra, görünümleri erişimi bir *zayıf yazılmış* (olarak da bilinir *geniş yazılmış*) veri koleksiyonu. Güçlü türlerinin aksine *zayıf türleri* (veya *kaybetmiş türleri*) kullanmakta olduğunuz veri türünü açıkça bildirmeyin anlamına gelir. Zayıf yazılmış veri koleksiyonunu küçük miktarda denetleyicileri ve görünümler ve dışındaki veri geçirmek için kullanabilirsiniz.

| Arasında veri geçirme bir...                        | Örnek                                                                        |
| ------------------------------------------------- | ------------------------------------------------------------------------------ |
| Denetleyici ve bir görünüm                             | Bir açılır liste verilerle doldurma.                                          |
| Görünüm ve [görünümü](xref:mvc/views/layout)   | Ayarı  **\<title >** bir görünüm dosyasından düzeni görünümünde öğe içeriği.  |
| [Kısmi Görünüm](xref:mvc/views/partial) ve bir görünüm | Kullanıcının istenen Web sayfasındaki göre verileri görüntüleyen bir pencere öğesi.      |

Bu koleksiyon yoluyla başvurulabilir `ViewData` veya `ViewBag` denetleyicileri ve görünümlere özellikleri. `ViewData` Zayıf yazılmış nesneleri sözlüğü bir özelliktir. `ViewBag` Özelliktir çevresinde bir sarmalayıcı `ViewData` arka plandaki için dinamik özellikler sağlayan `ViewData` koleksiyonu.

`ViewData`ve `ViewBag` çalışma zamanında dinamik olarak çözümlendiği. Derleme zamanı tür denetimi sunmuyoruz olduğundan, her ikisi de genellikle daha hata-bir viewmodel kullanmaktan daha fazladır. Bu nedenle, bazı geliştiriciler en düşük düzeyde ya da hiç kullanmayı tercih ederseniz `ViewData` ve `ViewBag`.


<a name="VD"></a>

**ViewData**

`ViewData`olan bir [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) üzerinden erişilen nesne `string` anahtarları. Dize verilerini depolanır ve bir cast gerek kalmadan doğrudan kullanılır, ancak diğer atamalısınız `ViewData` bunları ayıkladığınızda belirli türleri için değer nesnesi. Kullanabileceğiniz `ViewData` görünümlere ve görünümler dahil olmak üzere içinde denetleyicilerinden veri iletmek için [kısmi görünümler](xref:mvc/views/partial) ve [düzenleri](xref:mvc/views/layout).

Aşağıdaki tebrik kartı ve bir adresi kullanarak ilişkin değerleri ayarlayan bir örnektir `ViewData` bir eylem:

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

Bir görünümdeki verilerle çalışma:

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

**ViewBag**

Not: `ViewBag` Razor sayfalarında kullanılamaz.

`ViewBag`olan bir [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata) depolanan nesnelere dinamik erişim sağlayan nesne `ViewData`. `ViewBag`atama gerektirmez, çalışmaya daha kullanışlı olabilir. Aşağıdaki örnekte nasıl kullanılacağını gösterir `ViewBag` kullanarak aynı sonucu ile `ViewData` yukarıda:

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

**ViewData ve Görünüm Paketi aynı anda kullanma**

Not: `ViewBag` Razor sayfalarında kullanılamaz.

Bu yana `ViewData` ve `ViewBag` aynı temel bakın `ViewData` koleksiyonu, her ikisi de kullanabilirsiniz `ViewData` ve `ViewBag` karıştırmak ve aralarındaki okurken ve yazarken değerleri aynı.

Başlık kullanılarak ayarlanan `ViewBag` ve açıklaması kullanarak `ViewData` en üstündeki bir *About.cshtml* görünümü:

```cshtml
@{
    Layout = "/Views/Shared/_Layout.cshtml";
    ViewBag.Title = "About Contoso";
    ViewData["Description"] = "Let us tell you about Contoso's philosophy and mission.";
}
```

Özellikleri okunmaya ancak kullanımını ters `ViewData` ve `ViewBag`. İçinde *_Layout.cshtml* dosya, başlık kullanarak elde `ViewData` ve açıklaması kullanarak elde `ViewBag`:

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"]</title>
    <meta name="description" content="@ViewBag.Description">
    ...
```

Dizeler için bir cast gerektirmeyen unutmayın `ViewData`. Kullanabileceğiniz `@ViewData["Title"]` atama olmadan.

Her ikisini de kullanarak `ViewData` ve `ViewBag` adresindeki karıştırma ve okuma ve yazma özellikleri eşleştirme yoksa olarak aynı zaman çalışır. Aşağıdaki biçimlendirmede oluşturulur:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>About Contoso</title>
    <meta name="description" content="Let us tell you about Contoso's philosophy and mission.">
    ...
```

**Görünüm Paketi ViewData arasındaki farkları özeti**

 `ViewBag`Razor sayfalarında kullanılamaz.

* `ViewData`
  * Türetilen [ViewDataDictionary](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)gibi yararlı olabilecek sözlük özelliklere sahip nedenle `ContainsKey`, `Add`, `Remove`, ve `Clear`.
  * Boşluk izin sözlükteki anahtarları, dizelerdir. Örnek:`ViewData["Some Key With Whitespace"]`
  * Herhangi türdeki dışındaki bir `string` kullanmak için görünümünde dönüştürülmelidir `ViewData`.
* `ViewBag`
  * Türetilen [DynamicViewData](/aspnet/core/api/microsoft.aspnetcore.mvc.viewfeatures.internal.dynamicviewdata), böylece noktalı gösterim kullanılarak dinamik özellikleri oluşturulmasını sağlar (`@ViewBag.SomeKey = <value or object>`), ve hiçbir atama gereklidir. Söz dizimi `ViewBag` denetleyicileri ve görünümleri eklemek daha hızlı hale getirir.
  * Null değerler için denetlemek daha basit. Örnek:`@ViewBag.Person?.Name`

**Zaman ViewData veya Görünüm paketini kullanmak için**

Her ikisi de `ViewData` ve `ViewBag` eşit olan küçük miktarda denetleyicileri ve görünüm arasında veri geçirme için geçerli yaklaşımlar. Hangisinin kullanılacağını Seçimi tercihine göre dayanır. Karışık ve eşleşen `ViewData` ve `ViewBag` nesneleri, Bununla birlikte, kod okuyun ve sürekli olarak kullanılan bir yaklaşım ile korumak daha kolay. Her iki yaklaşımın çalışma zamanında dinamik olarak çözümlenmiş ve çalışma zamanı hataları neden dolayısıyla saldırıya. Bazı geliştirme ekiplerinin bunları kaçının.

### <a name="dynamic-views"></a>Dinamik Görünüm

Bir model bildirmeyin görünümleri yazın kullanarak `@model` ancak onlara geçirilen bir model örneği sahip (örneğin, `return View(Address);`) örneğin özellikleri dinamik olarak başvurabilir:

```cshtml
<address>
    @Model.Street<br>
    @Model.City, @Model.State @Model.PostalCode<br>
    <abbr title="Phone">P:</abbr> 425.555.0100
</address>
```

Bu özellik esneklik sunar ancak derleme koruma veya IntelliSense sunmaz. Web sayfası oluşturma özelliği yoksa, çalışma zamanında başarısız olur.

## <a name="more-view-features"></a>Daha fazla görünümü özellikleri

[Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) varolan HTML etiketleri için sunucu tarafı davranışı eklemek kolaylaştırır. Etiket Yardımcıları kullanarak özel kod veya kendi görünümlerinizi içinde Yardımcıları yazmak için gereken önler. Etiket Yardımcıları olarak öznitelikleri HTML öğelerine uygulanır ve bunları işleyemiyor düzenleyiciler tarafından göz ardı edilir. Bu, çeşitli araçları görünüm biçimlendirmesinde oluşturmak ve düzenlemek sağlar.

Özel HTML biçimlendirme oluşturmak çok sayıda yerleşik HTML Yardımcıları ile gerçekleştirilebilir. Daha karmaşık kullanıcı arabirimi mantığı tarafından işlenebilir [görünüm bileşenleri](xref:mvc/views/view-components). Görünüm bileşenleri aynı SoC o denetleyicileri sağlar ve görünümler sunar. Bunlar, eylemleri ve ortak kullanıcı arabirimi öğeleri tarafından kullanılan verileri uğraşmanız görünümleri gereksinimini ortadan kaldırabilirsiniz.

Birçok diğer yönlerini ASP.NET Core gibi görünümleri Destek [bağımlılık ekleme](xref:fundamentals/dependency-injection), olmasını hizmetlerine izin [görünümlere eklenen](xref:mvc/views/dependency-injection).
