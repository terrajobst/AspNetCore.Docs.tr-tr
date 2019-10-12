---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 0dd8b3b5ec58a199db086c4c0b0f057d26afd589
ms.sourcegitcommit: 7a2c820692f04bfba04398641b70f27fa15391f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72290638"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir. Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Formata özgü eylem sonuçları

Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult> gibi belirli bir biçime özgüdür. Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir. Örneğin, `JsonResult` ' ı döndürmek JSON biçimli verileri döndürür. @No__t-0 veya dize döndürmek düz metin biçimli dize verileri döndürür.

Belirli bir tür döndürmek için bir eylem gerekli değildir. ASP.NET Core, tüm nesne dönüş değerlerini destekler.  @No__t-0 türünde olmayan nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulamasını kullanarak serileştirilir. Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.

@No__t-0 yerleşik yardımcı yöntemi JSON biçimli verileri döndürüyor: [!code-csharp @ no__t-2

Örnek indirme, yazarların listesini döndürür. Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:

* **Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.
* İstek üst bilgileri görüntülenir. Örneğin, `Accept` üstbilgisi. @No__t-0 üstbilgisi, önceki kod tarafından yok sayılır.

Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> yardımcısını kullanın:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

Yukarıdaki kodda, döndürülen `Content-Type` `text/plain` ' dir. Dize döndürmek `Content-Type` ' dan `text/plain` ' i sunar:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

Birden çok dönüş türüne sahip eylemler için `IActionResult` döndürün. Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.

## <a name="content-negotiation"></a>İçerik anlaşması

İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur. ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir. İçerik anlaşması:

* @No__t-0 tarafından uygulandı.
* Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak. Eylem sonuçları yardımcı yöntemleri `ObjectResult` ' a dayalıdır.

Bir model türü döndürüldüğünde, dönüş türü `ObjectResult` ' dır.

Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

Varsayılan olarak, ASP.NET Core `application/json`, `text/json` ve `text/plain` medya türlerini destekler. [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, `Accept` istek üst bilgisini dönüş biçimini belirtecek şekilde ayarlayabilir. @No__t-0 üstbilgisi sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür. Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.

Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir. POCO döndürüldüğünde, çalışma zamanı nesneyi sarmalayan bir @no__t otomatik olarak oluşturur. İstemci, biçimlendirilen serileştirilmiş nesneyi alır. Döndürülen nesne `null` ise, bir `204 No Content` yanıtı döndürülür.

Nesne türü döndürülüyor:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle birlikte `200 OK` yanıtı döndürür. Geçersiz bir diğer ad isteği bir `204 No Content` yanıtı döndürüyor.

### <a name="the-accept-header"></a>Accept üst bilgisi

İstekte bir `Accept` üst bilgisi göründüğünde içerik *anlaşması* gerçekleşir. Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:

* Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.
* Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.

İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:

* @No__t-1 ayarlandıysa `406 Not Acceptable` döndürür veya-
* Yanıt üreten ilk biçimlendirici bulmayı dener.

İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır. İstekte `Accept` üstbilgisi görünürse:

* Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.
* Hiçbir anlaşma gerçekleşmiyor. Sunucu hangi biçimin döneceğine karar verir.

Accept üst bilgisi `*/*` içeriyorsa, `RespectBrowserAcceptHeader` <xref:Microsoft.AspNetCore.Mvc.MvcOptions> üzerinde true olarak ayarlanmadığı takdirde başlık yok sayılır.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve içerik anlaşması

Tipik API istemcilerinin aksine Web tarayıcıları `Accept` üst bilgilerini sağlar. Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir. Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:

* @No__t-0 üstbilgisi yoksayıldı.
* Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.

Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.

Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> ' ı `true` olarak ayarlayın:

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

@No__t-0 kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> çağırarak yapılandırılır:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

Yukarıdaki kod `XmlSerializer` kullanarak sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemlerinin isteğin `Accept` üstbilgisine göre uygun biçimi döndürmesi gerekir.

### <a name="configure-systemtextjson-based-formatters"></a>System. Text. JSON tabanlı formatlayıcıları yapılandırma

@No__t -0 tabanlı formatıcılar için özellikler `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` kullanılarak yapılandırılabilir.

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult` kullanılarak yapılandırılabilir. Örneğin:

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

ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paketi kullanılarak uygulanır. ASP.NET Core 3,0 veya üzeri sürümlerde, varsayılan JSON biçimleri `System.Text.Json` ' ı temel alır. @No__t-0 tabanlı formatcılar ve özellikler için destek, [Microsoft. AspNetCore. Mvc. NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketi yüklenerek ve bunu `Startup.ConfigureServices` ' de yapılandırarak kullanılabilir.

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

Bazı özellikler @no__t -0 tabanlı formatıcılar ile iyi çalışmayabilir ve @no__t -1 tabanlı formatlayıcılar için bir başvuru gerektirir. Uygulama şu durumlarda @no__t -0 tabanlı formatlayıcıları kullanmaya devam edin:

* @No__t-0 özniteliklerini kullanır. Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.
* Serileştirme ayarlarını özelleştirir.
* @No__t-0 ' ın sağladığı özellikleri kullanır.
* @No__t yapılandırır-0. ASP.NET Core 3,0 ' dan önce, `JsonResult.SerializerSettings` @no__t 2 ' ye özgü bir `JsonSerializerSettings` örneğini kabul eder.
* [Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri oluşturur.

@No__t -0 tabanlı formatıcılar için özellikler `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings` kullanılarak yapılandırılabilir:

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

Çıkış serileştirme seçenekleri, eylem başına temelinde, `JsonResult` kullanılarak yapılandırılabilir. Örneğin:

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

@No__t-0 kullanılarak uygulanan XML formatlayıcıları <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> çağırarak yapılandırılır:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

Yukarıdaki kod `XmlSerializer` kullanarak sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemlerinin isteğin `Accept` üstbilgisine göre uygun biçimi döndürmesi gerekir.

::: moniker-end

### <a name="specify-a-format"></a>Biçim belirtin

Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresini uygulayın. Çoğu [filtre](xref:mvc/controllers/filters)gibi `[Produces]` eylem, denetleyici veya genel kapsamda uygulanabilir:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtresi:

* Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.
* Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.

Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Özel durum formatları

Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır. Varsayılan olarak, `string` dönüş türleri *metin/düz* (`Accept` üst bilgisi ile isteniyorsa*metin/html* ) olarak biçimlendirilir. Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> kaldırılarak silinebilir. Biçimlendiriciler `Configure` yönteminde kaldırılır. Model nesne dönüş türü olan eylemler, `null` ' i döndürürken `204 No Content` döndürür. Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter> kaldırılarak silinebilir. Aşağıdaki kod `TextOutputFormatter` ve `HttpNoContentOutputFormatter` ' i kaldırır.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

@No__t-0 olmadan `string` dönüş türleri `406 Not Acceptable` ' i döndürür. Bir XML biçimlendiricisi varsa, `TextOutputFormatter` kaldırılırsa `string` dönüş türlerini biçimlendirir.

@No__t-0 olmadan null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin:

* JSON biçimlendiricisi `null` gövdesi olan bir yanıt döndürür.
* XML biçimlendiricisi `xsi:nil="true"` kümesi özniteliğine sahip boş bir XML öğesi döndürür.

## <a name="response-format-url-mappings"></a>Yanıt biçimi URL eşlemeleri

İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:

* Sorgu dizesinde veya yolun bir bölümünde.
* . Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.

İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir. Örneğin:

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar. [@No__t-1](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) özniteliği, `RouteData` ' deki biçim değerinin varlığını denetler ve yanıt oluşturulduğunda yanıt biçimini uygun biçimlendirici ile eşler.

|           Yolu        |             Biçimlendirici              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    Varsayılan çıkış biçimlendiricisi    |
| `/api/products/5.json` | JSON biçimlendiricisi (yapılandırıldıysa) |
| `/api/products/5.xml`  | XML biçimlendiricisi (yapılandırıldıysa)  |
