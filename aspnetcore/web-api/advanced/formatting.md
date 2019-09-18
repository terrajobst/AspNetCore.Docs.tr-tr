---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 8bee4efdae5341ddab5bd3aec278ecfef37f0c08
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082358"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme

[Steve Smith](https://ardalis.com/) tarafından

ASP.NET Core MVC, yanıt verilerini biçimlendirme, sabit biçimleri kullanma veya istemci belirtimlerine yanıt verme için yerleşik desteğe sahiptir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Formata özgü eylem sonuçları

Bazı eylem sonuç türleri, `JsonResult` ve `ContentResult`gibi belirli bir biçime özgüdür. Eylemler, her zaman belirli bir şekilde biçimlendirilen belirli sonuçları döndürebilir. Örneğin, döndürme `JsonResult` , istemci tercihlerinden bağımsız olarak JSON biçimli verileri döndürür. Benzer şekilde, döndürme `ContentResult` düz metin biçimli dize verileri döndürür (gibi, yalnızca bir dize döndürür).

> [!NOTE]
> Belirli bir türü döndürmek için bir eylem gerekli değildir; MVC tüm nesne dönüş değerlerini destekler. Bir eylem bir `IActionResult` uygulama döndürürse ve denetleyici öğesinden `Controller`devralırsa, geliştiricilerin birçok seçeneğe karşılık gelen birçok yardımcı yöntemi vardır. Tür olmayan `IActionResult` nesneleri döndüren eylemlerin sonuçları, uygun `IOutputFormatter` uygulama kullanılarak serileştirilir.

Verileri, `Controller` temel sınıftan devralan bir denetleyiciden belirli bir biçimde döndürmek için, JSON döndürmek ve `Content` düz metin için yerleşik yardımcı yöntemi `Json` kullanın. Eylem yönteminiz, belirli sonuç türünü (örneğin, `JsonResult`) ya `IActionResult`da döndürmelidir.

JSON biçimli veriler döndürülüyor:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Bu eylemden örnek yanıt:

![Microsoft Edge 'de, yanıtın Içerik türünü gösteren Geliştirici Araçları ağ sekmesi (uygulamanın/JSON)](formatting/_static/json-response.png)

Yanıtın `application/json`içerik türünün hem ağ istekleri listesinde hem de yanıt üstbilgileri bölümünde gösterildiğini unutmayın. Ayrıca, Istek üst bilgilerinde onay üst bilgisinde tarayıcı (Bu durumda, Microsoft Edge) tarafından sunulan seçenekler listesini de göz önünde bulundurun. Geçerli teknik bu üstbilgiyi yok sayıyor; Aşağıda ele alınmıştır.

Düz metin biçimli verileri döndürmek için `ContentResult` `Content` ve yardımcısını kullanın:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Bu eylemden bir yanıt:

![Microsoft Edge 'de Geliştirici Araçları yanıtın Içerik türünü gösteren ağ sekmesi metin/düz](formatting/_static/text-response.png)

Bu örnekte `Content-Type` `text/plain`döndürülen ' i aklınızda edin. Yalnızca bir dize yanıt türü kullanarak aynı davranışa de ulaşabilirsiniz:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Birden çok dönüş türü veya seçeneği olan önemsiz olmayan eylemler için (örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı http durum kodları), dönüş türü olarak tercih `IActionResult` edin.

## <a name="content-negotiation"></a>İçerik anlaşması

İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması (kısaca*conneg* ) oluşur. ASP.NET Core MVC tarafından kullanılan varsayılan biçim JSON 'dir. İçerik anlaşması tarafından `ObjectResult`uygulanır. Ayrıca, yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarının (tümü, temel `ObjectResult`alınan) içinde de yerleşiktir. Ayrıca bir model türü (veri aktarım türü olarak tanımladığınız bir sınıf) döndürebilir ve çerçeve sizin için otomatik olarak bir `ObjectResult` ile kaydırılır.

Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Başka bir biçim istenmediği ve sunucu istenen biçimi döndüremediğinde JSON biçimli bir yanıt döndürülür. Bir Accept üst bilgisi içeren bir istek oluşturmak ve başka bir biçim belirtmek için [Fiddler](https://www.telerik.com/fiddler) gibi bir araç kullanabilirsiniz. Bu durumda, sunucuda istenen biçimde yanıt üretemeyen bir *biçimlendirici* varsa, sonuç istemci tarafından tercih edilen biçimde döndürülür.

![Uygulama/XML Accept üst bilgisi değeri ile el ile oluşturulmuş bir GET isteği gösteren Fiddler konsolu](formatting/_static/fiddler-composer.png)

Yukarıdaki ekran görüntüsünde, bir istek oluşturmak için Fiddler Oluşturucu kullanılmıştır, belirterek `Accept: application/xml`. Varsayılan olarak, ASP.NET Core MVC yalnızca JSON 'ı destekler, bu nedenle başka bir biçim belirtildiğinde bile döndürülen sonuç hala JSON biçimli olur. Sonraki bölümde nasıl ek biçimlendirme ekleneceğini göreceksiniz.

Denetleyici eylemleri Pocos (düz eski CLR nesneleri) döndürebilir, bu durumda ASP.NET Core MVC nesneyi sarmalayan sizin `ObjectResult` için otomatik olarak oluşturur. İstemci, biçimlendirilen serileştirilmiş nesneyi alır (JSON biçimi varsayılandır; XML veya diğer biçimleri yapılandırabilirsiniz). Döndürülen nesne ise `null`, Framework bir `204 No Content` yanıt döndürür.

Nesne türü döndürülüyor:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Örnekte, geçerli bir yazar diğer adı için bir istek yazarın verileriyle 200 OK yanıtı alır. Geçersiz bir diğer ad isteği bir 204 Içerik yanıtı alacak. XML ve JSON biçimlerinde yanıtı gösteren ekran görüntüleri aşağıda gösterilmiştir.

### <a name="content-negotiation-process"></a>İçerik anlaşma Işlemi

İçerik *anlaşması* yalnızca, istekte bir `Accept` başlık görünürse gerçekleşir. Bir istek bir Accept üst bilgisi içerdiğinde, çerçeve kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır ve Accept üst bilgisi tarafından belirtilen biçimlerden birinde yanıt üreten bir biçimlendirici bulmaya çalışır. İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa, çerçeve bir yanıt üreten ilk biçimlendirici bulmayı dener (Geliştirici, bunun yerine 406 döndürmek `MvcOptions` için seçeneği yapılandırmamışsa). İstek XML belirtiyorsa, ancak XML biçimlendirici yapılandırılmamışsa, JSON biçimlendiricisi kullanılacaktır. Daha genel olarak, istenen biçimi sağlayabilecek bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır. Üst bilgi verilmezse, döndürülecek nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılacaktır. Bu durumda, hiçbir anlaşma gerçekleşmez; sunucu hangi biçimi kullanacağınızı belirliyor.

> [!NOTE]
> Accept üst bilgisi içeriyorsa `*/*`üstbilgi, true `MvcOptions`olarak ayarlanmadığı müddetçe `RespectBrowserAcceptHeader` yok sayılır.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve Içerik anlaşması

Tipik API istemcilerinin aksine, Web tarayıcıları joker karakterler de `Accept` dahil olmak üzere geniş bir biçim dizisi içeren Üstbilgiler sağlamayı eğilimindedir. Varsayılan olarak, çerçeve isteğin bir tarayıcıdan geldiğini algıladığında, `Accept` üstbilgiyi yoksayar ve bunun yerine uygulamanın yapılandırılmış varsayılan biçimindeki içeriği döndürür (aksi yapılandırılmamışsa JSON). Bu, API 'Leri kullanmak için farklı tarayıcılar kullanırken daha tutarlı bir deneyim sağlar.

Uygulamanızın tarayıcı için kabul etme üst `RespectBrowserAcceptHeader` bilgilerini benimseme seçeneğini tercih ediyorsanız, bunu *Startup.cs*içindeki `ConfigureServices` yönteminde olarak `true` ayarlayarak MVC 'nin yapılandırmasının bir parçası olarak yapılandırabilirsiniz.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Biçimleri yapılandırma

Uygulamanızın varsayılan JSON dışında ek biçimleri desteklemesi gerekiyorsa, NuGet paketleri ekleyebilir ve MVC 'yi destekleyecek şekilde yapılandırabilirsiniz. Giriş ve çıkış için ayrı biçimlendirme vardır. Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır; çıkış biçimleri, yanıtları biçimlendirmek için kullanılır. Ayrıca, [özel biçimleri](xref:web-api/advanced/custom-formatters)de yapılandırabilirsiniz.

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a>System. Text. JSON tabanlı formatlayıcıları yapılandırma

Tabanlı formatlayıcılar `System.Text.Json`için özellikler kullanılarak `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`yapılandırılabilir.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir. Örneğin:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Newtonsoft. JSON tabanlı JSON biçimi desteği ekleyin

ASP.NET Core 3,0 ' dan önce, MVC, `Newtonsoft.Json` paket kullanılarak uygulanan JSON formatlarını kullanmaya göre varsayılan. ASP.NET Core 3,0 veya sonraki bir sürümde, varsayılan JSON formatlayıcıları temel `System.Text.Json`alınır. [Microsoft. aspnetcore. Mvc. newtonsoftjson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve ' de `Startup.ConfigureServices`yapılandırarak, tabanlı biçimlendirme ve özellikler için `Newtonsoft.Json`destek bulabilirsiniz.

```csharp
services.AddControllers()
    .AddNewtonsoftJson();
```

Bazı özellikler, tabanlı formatlayıcılar ile `System.Text.Json`düzgün çalışmayabilir ve ASP.NET Core 3,0 sürümü için tabanlı formatlayıcılar `Newtonsoft.Json`için bir başvuru gerektirir. ASP.NET Core 3,0 veya `Newtonsoft.Json`sonraki bir sürüm uygulamanız varsa, tabanlı formatlayıcıları kullanmaya devam edin:

* Öznitelikleri `Newtonsoft.Json` kullanır (örneğin, `[JsonProperty]` veya `[JsonIgnore]`), `Newtonsoft.Json` serileştirme ayarlarını özelleştirir veya sağlayan özelliklere bağımlıdır.
* Yapılandırır `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. ASP.NET Core 3,0 ' dan önce `JsonResult.SerializerSettings` , öğesine `Newtonsoft.Json`özgü bir `JsonSerializerSettings` örneğini kabul eder.
* [Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.

Tabanlı formatlayıcılar `Newtonsoft.Json`için özellikler şu kullanılarak `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`yapılandırılabilir:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefautlContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

İşlem başına temelinde çıkış serileştirme seçenekleri kullanılarak `JsonResult`yapılandırılabilir. Örneğin:

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

### <a name="add-xml-format-support"></a>XML biçimi desteği ekle

::: moniker range="<= aspnetcore-2.2"

ASP.NET Core 2,2 veya önceki sürümlerde XML biçimlendirme desteği eklemek için [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini yüklemek için.

::: moniker-end

Kullanılarak `System.Xml.Serialization.XmlSerializer` uygulanan XML formatlayıcıları, ' de `Startup.ConfigureServices`çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> yapılandırılabilir:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatif olarak, kullanılarak `System.Runtime.Serialization.DataContractSerializer` uygulanan XML formatlayıcıları, ' de `Startup.ConfigureServices`çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> yapılandırılabilir:

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

XML biçimlendirme desteğini ekledikten sonra, bu Fiddler örneğinde gösterildiği gibi, denetleyici yöntemlerinizi isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir:

![Fiddler konsolu: İsteğin ham sekmesi, Accept üst bilgisini, application/xml değerini gösterir. Yanıtın ham sekmesi, uygulamanın/xml Içerik türü üstbilgi değerini gösterir.](formatting/_static/xml-response.png)

, Ham GET isteğinin bir `Accept: application/xml` başlık kümesiyle yapıldığını Inspectors sekmesinde görebilirsiniz. Yanıt bölmesi `Content-Type: application/xml` üstbilgiyi gösterir `Author` ve nesne XML olarak serileştirilir.

`Accept` Üst bilgide belirtmek `application/json` üzere isteği değiştirmek için besteci sekmesini kullanın. İsteği yürütün ve yanıt JSON olarak biçimlendirilir:

![Fiddler konsolu: İsteğin ham sekmesi, Accept üst bilgisi değerini bir Application/JSON olarak gösterir. Yanıtın ham sekmesi, uygulamanın/JSON öğesinin Içerik türü üstbilgi değerini gösterir.](formatting/_static/json-response-fiddler.png)

Bu ekran görüntüsünde, istek bir başlık `Accept: application/json` olduğunu ve yanıtın onunla `Content-Type`aynı olduğunu belirtir. `Author` Nesne, yanıtın gövdesinde, JSON biçiminde gösterilir.

### <a name="forcing-a-particular-format"></a>Belirli bir biçimi zorlama

Belirli bir eylem için yanıt biçimlerini kısıtlamak isterseniz, `[Produces]` filtre uygulayabilirsiniz. `[Produces]` Filtre, belirli bir eyleme (veya denetleyiciye) ait yanıt biçimlerini belirtir. Çoğu [filtre](xref:mvc/controllers/filters)gibi, bu işlem eylem, denetleyici veya genel kapsamda uygulanabilir.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Filtre, uygulama için başka formatlayıcılar yapılandırılmış `AuthorsController` olsa bile `Accept` ve istemci farklı bir başlık sağladıysa, içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlayacaktır. `[Produces]` formatını. Genel olarak filtrelerin nasıl uygulanacağı dahil olmak üzere daha fazla bilgi için [filtrelere](xref:mvc/controllers/filters) bakın.

### <a name="special-case-formatters"></a>Özel durum formatları

Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır. Varsayılan olarak, `string` dönüş türleri *metin/düz* olarak biçimlendirilir (üst bilgi ile `Accept` isteniyorsa*metin/html* ). Bu davranış, `TextOutputFormatter`' ı kaldırılarak kaldırılabilir. *Startup.cs* (aşağıda gösterildiği gibi) `Configure` yöntemindeki formatlayıcıları kaldırırsınız. Model nesnesi dönüş türüne sahip eylemler, döndürme `null`sırasında bir 204 içerik yanıtı döndürmez. Bu davranış, `HttpNoContentOutputFormatter`' ı kaldırılarak kaldırılabilir. Aşağıdaki kod `TextOutputFormatter` ve `HttpNoContentOutputFormatter`öğesini kaldırır.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

`TextOutputFormatter` Olmadan,dönüştürleri406dönüş`string` kabul edilemez, örneğin. Bir XML biçimlendiricisi varsa, kaldırılırsa dönüş türlerini `string` `TextOutputFormatter` biçimlendirir.

`HttpNoContentOutputFormatter`Olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin, JSON biçimlendiricisi yalnızca gövdesi `null`olan bir yanıt döndürür, XML biçimlendirici öznitelik `xsi:nil="true"` kümesi ile boş bir XML öğesi döndürür.

## <a name="response-format-url-mappings"></a>Yanıt biçimi URL eşlemeleri

İstemciler, URL 'nin bir parçası olarak, sorgu dizesinde veya yolun bir bölümünde veya. xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak belirli bir biçim talep edebilir. İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir. Örneğin:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Bu yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesine izin verir. `[FormatFilter]` Özniteliği ,`RouteData` içindeki biçim değerinin varlığını denetler ve yanıt oluşturulduğu zaman yanıt biçimini uygun biçimlendirici ile eşler.

|           Yolu            |             Biçimlendirici              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Varsayılan çıkış biçimlendiricisi    |
| `/products/GetById/5.json` | JSON biçimlendiricisi (yapılandırıldıysa) |
| `/products/GetById/5.xml`  | XML biçimlendiricisi (yapılandırıldıysa)  |
