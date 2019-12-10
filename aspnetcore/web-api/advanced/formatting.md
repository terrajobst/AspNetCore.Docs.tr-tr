---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/05/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: cab383053751598b882f3716943d3d9392c56f4a
ms.sourcegitcommit: 29ace642ca0e1f0b48a18d66de266d8811df2b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74987964"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir. Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Formata özgü eylem sonuçları

Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult>gibi belirli bir biçime özgüdür. Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir. Örneğin, döndürme `JsonResult` JSON biçimli verileri döndürür. `ContentResult` veya dize döndürüldüğünde düz metin biçimli dize verileri döndürülür.

Belirli bir tür döndürmek için bir eylem gerekli değildir. ASP.NET Core, tüm nesne dönüş değerlerini destekler.  <xref:Microsoft.AspNetCore.Mvc.IActionResult> türler olmayan nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulamasını kullanarak serileştirilir. Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.

Yerleşik yardımcı yöntem <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON biçimli verileri döndürür: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]

Örnek indirme, yazarların listesini döndürür. Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:

* **Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.
* İstek üst bilgileri görüntülenir. Örneğin, `Accept` üst bilgisi. `Accept` üst bilgisi, önceki kod tarafından yok sayılır.

Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> yardımcısını kullanın:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

Yukarıdaki kodda, döndürülen `Content-Type` `text/plain`. Bir dize döndürmek `text/plain``Content-Type` sağlar:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

Birden çok dönüş türüne sahip eylemler için `IActionResult`döndürün. Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.

## <a name="content-negotiation"></a>İçerik anlaşması

İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur. ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir. İçerik anlaşması:

* <xref:Microsoft.AspNetCore.Mvc.ObjectResult>tarafından uygulandı.
* Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak. Eylem sonuçları yardımcı yöntemleri `ObjectResult`tabanlıdır.

Bir model türü döndürüldüğünde, dönüş türü `ObjectResult`.

Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

Varsayılan olarak, ASP.NET Core `application/json`, `text/json`ve `text/plain` medya türlerini destekler. [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, dönüş biçimini belirtmek için `Accept` istek üst bilgisini ayarlayabilir. `Accept` üst bilgisi sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür. Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.

Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir. POCO döndürüldüğünde, çalışma zamanı otomatik olarak nesneyi sarmalayan bir `ObjectResult` oluşturur. İstemci, biçimlendirilen serileştirilmiş nesneyi alır. Döndürülen nesne `null`, bir `204 No Content` yanıtı döndürülür.

Nesne türü döndürülüyor:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle `200 OK` bir yanıt döndürür. Geçersiz bir diğer ad isteği bir `204 No Content` yanıtı döndürüyor.

### <a name="the-accept-header"></a>Accept üstbilgisi

İstekte bir `Accept` üst bilgisi göründüğünde içerik *anlaşması* gerçekleşir. Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:

* Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.
* Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.

İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions> ayarlandıysa `406 Not Acceptable` döndürür veya-
* Yanıt üreten ilk biçimlendirici bulmayı dener.

İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır. İstekte hiçbir `Accept` üstbilgisi görünürse:

* Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.
* Hiçbir anlaşma gerçekleşmiyor. Sunucu hangi biçimin döneceğine karar verir.

Accept üst bilgisi `*/*`içeriyorsa, <xref:Microsoft.AspNetCore.Mvc.MvcOptions>`RespectBrowserAcceptHeader` true olarak ayarlanmadığı takdirde başlık yok sayılır.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve içerik anlaşması

Tipik API istemcilerinin aksine Web tarayıcıları `Accept` üst bilgileri sağlar. Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir. Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:

* `Accept` üst bilgisi yok sayılır.
* Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.

Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.

Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> `true`olarak ayarlayın:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a>Biçimleri yapılandırma

Ek biçimleri desteklemesi gereken uygulamalar uygun NuGet paketlerini ekleyebilir ve desteği yapılandırabilir. Giriş ve çıkış için ayrı biçimlendirme vardır. Giriş formatlayıcıları [model bağlama](xref:mvc/models/model-binding)tarafından kullanılır. Çıkış biçimleri, yanıtları biçimlendirmek için kullanılır. Özel bir biçimlendirici oluşturma hakkında daha fazla bilgi için bkz. [özel Formatlayıcılar](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a>XML biçimi desteği ekle

<xref:System.Xml.Serialization.XmlSerializer> kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>çağırarak yapılandırılır:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

Yukarıdaki kod, `XmlSerializer`kullanarak sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üst bilgisine göre uygun biçimi döndürür.

### <a name="configure-systemtextjson-based-formatters"></a>System. Text. JSON tabanlı formatlayıcıları yapılandırma

`System.Text.Json`tabanlı formatlayıcılar için özellikler, `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`kullanılarak yapılandırılabilir.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult`kullanılarak yapılandırılabilir. Örneğin:

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

ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paketi kullanılarak uygulanır. ASP.NET Core 3,0 veya üzeri sürümlerde, varsayılan JSON biçimleri `System.Text.Json`temel alır. `Newtonsoft.Json` tabanlı formatlayıcılar ve özellikler için destek, [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve `Startup.ConfigureServices`yapılandırılarak kullanılabilir.

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

Bazı özellikler `System.Text.Json`tabanlı formatlayıcılar ile iyi çalışmayabilir ve `Newtonsoft.Json`tabanlı Biçimlendiriciler için bir başvuru gerektirir. Uygulama şu durumlarda `Newtonsoft.Json`tabanlı formatlayıcıları kullanmaya devam edin:

* `Newtonsoft.Json` özniteliklerini kullanır. Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.
* Serileştirme ayarlarını özelleştirir.
* `Newtonsoft.Json` sağladığı özellikleri kullanır.
* `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`yapılandırır. ASP.NET Core 3,0 ' dan önce `JsonResult.SerializerSettings`, `Newtonsoft.Json`özgü bir `JsonSerializerSettings` örneğini kabul eder.
* [Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.

`Newtonsoft.Json`tabanlı formatlayıcılar için özellikler `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`kullanılarak yapılandırılabilir:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult`kullanılarak yapılandırılabilir. Örneğin:

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

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a>XML biçimi desteği ekle

XML biçimlendirme, [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini gerektirir.

<xref:System.Xml.Serialization.XmlSerializer> kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>çağırarak yapılandırılır:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

Yukarıdaki kod, `XmlSerializer`kullanarak sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üst bilgisine göre uygun biçimi döndürmelidir.

::: moniker-end

### <a name="specify-a-format"></a>Biçim belirtin

Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresini uygulayın. Çoğu [filtre](xref:mvc/controllers/filters)gibi `[Produces]` eylem, denetleyici veya genel kapsamda uygulanabilir:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresi:

* Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.
* Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.

Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Özel durum formatları

Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır. `string` dönüş türleri varsayılan olarak *metin/düz* (`Accept` üst bilgisi ile isteniyorsa*metin/html* ) olarak biçimlendirilir. Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>kaldırılarak silinebilir. Biçimlendiriciler `ConfigureServices` yönteminde kaldırılır. Model nesne dönüş türü olan eylemler `null`döndürürken `204 No Content` döndürür. Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>kaldırılarak silinebilir. Aşağıdaki kod `StringOutputFormatter` ve `HttpNoContentOutputFormatter`kaldırır.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

`StringOutputFormatter`olmadan, yerleşik JSON biçimlendirici `string` dönüş türlerini biçimlendirir. Yerleşik JSON biçimlendiricisi kaldırılırsa ve bir XML biçimlendirici varsa, XML biçimlendirici `string` dönüş türlerini biçimlendirir. Aksi takdirde, `string` dönüş türleri `406 Not Acceptable`döndürür.

`HttpNoContentOutputFormatter`olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin:

* JSON biçimlendiricisi `null`gövdesiyle bir yanıt döndürür.
* XML biçimlendiricisi, `xsi:nil="true"` ayarlanan özniteliğe sahip boş bir XML öğesi döndürür.

## <a name="response-format-url-mappings"></a>Yanıt biçimi URL eşlemeleri

İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:

* Sorgu dizesinde veya yolun bir bölümünde.
* . Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.

İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir. Örneğin:

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar. [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) özniteliği, `RouteData` biçim değerinin varlığını denetler ve yanıt oluşturulduğu zaman yanıt biçimini uygun biçimlendirici ile eşler.

|           Yol        |             Biçimlendirici              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    Varsayılan çıkış biçimlendiricisi    |
| `/api/products/5.json` | JSON biçimlendiricisi (yapılandırıldıysa) |
| `/api/products/5.xml`  | XML biçimlendiricisi (yapılandırıldıysa)  |
