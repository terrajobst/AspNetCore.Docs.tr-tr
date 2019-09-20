---
title: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme
author: ardalis
description: ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 8/22/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 7b19bf54ea9f8a87c26ba7567a7348fcbea997c8
ms.sourcegitcommit: e7dc89620fa02c2ff80bee1e3f77297f97616968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71151187"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API 'sindeki yanıt verilerini biçimlendirme

By [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC, yanıt verilerini biçimlendirme desteğine sahiptir. Yanıt verileri, belirli biçimler kullanılarak veya istemci tarafından istenen biçime yanıt olarak biçimlendirilebilir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Formata özgü eylem sonuçları

Bazı eylem sonuç türleri, <xref:Microsoft.AspNetCore.Mvc.JsonResult> ve <xref:Microsoft.AspNetCore.Mvc.ContentResult>gibi belirli bir biçime özgüdür. Eylemler, istemci tercihlerinden bağımsız olarak belirli bir biçimde biçimlendirilen sonuçları döndürebilir. Örneğin, döndürme `JsonResult` JSON biçimli verileri döndürür. Döndürme `ContentResult` veya dize, düz metin biçimli dize verileri döndürür.

Belirli bir tür döndürmek için bir eylem gerekli değildir. ASP.NET Core, tüm nesne dönüş değerlerini destekler.  Tür olmayan <xref:Microsoft.AspNetCore.Mvc.IActionResult> nesneleri döndüren eylemlerin sonuçları, uygun <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> uygulama kullanılarak serileştirilir. Daha fazla bilgi için bkz. <xref:web-api/action-return-types>.

Yerleşik yardımcı yöntemi <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON biçimli verileri döndürür:[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]

Örnek indirme, yazarların listesini döndürür. Önceki kodla F12 tarayıcı geliştirici araçları veya [Postman](https://www.getpostman.com/tools) kullanma:

* **Content-Type:** `application/json; charset=utf-8` içeren yanıt üst bilgisi görüntülenir.
* İstek üst bilgileri görüntülenir. Örneğin, `Accept` üst bilgi. `Accept` Üst bilgi, önceki kod tarafından yok sayılır.

Düz metin biçimli verileri döndürmek için <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> ve yardımcısını kullanın:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

Yukarıdaki kodda, `Content-Type` `text/plain`döndürülen. Şu şekilde bir dize `Content-Type` `text/plain`döndürür:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

Birden çok dönüş türüne sahip eylemler için, `IActionResult`döndürün. Örneğin, gerçekleştirilen işlemlerin sonucuna göre farklı HTTP durum kodları döndürülüyor.

## <a name="content-negotiation"></a>İçerik anlaşması

İstemci bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)belirttiğinde içerik anlaşması oluşur. ASP.NET Core tarafından kullanılan varsayılan biçim [JSON](https://json.org/)'dir. İçerik anlaşması:

* <xref:Microsoft.AspNetCore.Mvc.ObjectResult>Uygulayan.
* Yardımcı metotlarından döndürülen durum koduna özgü eylem sonuçlarına yerleşik olarak. Eylem sonuçları yardımcı yöntemleri temel `ObjectResult`alınır.

Bir model türü döndürüldüğünde, dönüş türü olur `ObjectResult`.

Aşağıdaki eylem yöntemi `Ok` ve `NotFound` yardımcı yöntemlerini kullanır:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

Başka bir biçim istenmediği ve sunucu istenen biçimi döndüremediğinde JSON biçimli bir yanıt döndürülür. [Fiddler](https://www.telerik.com/fiddler) veya [Postman](https://www.getpostman.com/tools) gibi araçlar, `Accept` dönüş biçimini belirtmek için üst bilgiyi ayarlayabilir. , `Accept` Sunucunun desteklediği bir tür içerdiğinde, bu tür döndürülür.

Varsayılan olarak, ASP.NET Core yalnızca JSON 'ı destekler. Varsayılan, JSON biçimli yanıtları değiştirmemiş uygulamalar için, istemci isteğinden bağımsız olarak her zaman döndürülür. Sonraki bölümde, ek Biçimlendiriciler ekleme gösterilmektedir.

Denetleyici eylemleri POCOs (düz eski CLR nesneleri) döndürebilir. Poco döndürüldüğünde, çalışma zamanı nesneyi sarmalayan bir `ObjectResult` otomatik olarak oluşturur. İstemci, biçimlendirilen serileştirilmiş nesneyi alır. Döndürülen nesne ise `null` `204 No Content` yanıt döndürülür.

Nesne türü döndürülüyor:

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

Önceki kodda, geçerli bir yazar diğer adı için bir istek yazarın verileriyle bir `200 OK` yanıt döndürür. Geçersiz bir diğer ad isteği bir `204 No Content` yanıt döndürüyor.

### <a name="the-accept-header"></a>Accept üst bilgisi

İstekte bir `Accept` üst bilgi göründüğünde içerik anlaşması gerçekleşir. Bir istek bir Accept üst bilgisi içerdiğinde ASP.NET Core:

* Kabul üst bilgisindeki medya türlerini tercih sırasına göre numaralandırır.
* Belirtilen biçimlerden birinde yanıt üretemeyen bir biçimlendirici bulmaya çalışır.

İstemcinin isteğini karşılayabilen bir biçimlendirici bulunmazsa ASP.NET Core:

* Ayarlanmışsa döndürür `406 Not Acceptable`veya- <xref:Microsoft.AspNetCore.Mvc.MvcOptions>
* Yanıt üreten ilk biçimlendirici bulmayı dener.

İstenen biçim için bir biçimlendirici yapılandırılmamışsa, nesneyi biçimlendirebileceğini ilk biçimlendirici kullanılır. İstekte hiçbir `Accept` başlık görünürse:

* Nesneyi işleyebilen ilk biçimlendirici, yanıtı seri hale getirmek için kullanılır.
* Hiçbir anlaşma gerçekleşmiyor. Sunucu hangi biçimin döneceğine karar verir.

Accept üst bilgisi içeriyorsa `*/*`üstbilgi, true <xref:Microsoft.AspNetCore.Mvc.MvcOptions>olarak ayarlanmadığı müddetçe `RespectBrowserAcceptHeader` yok sayılır.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve içerik anlaşması

Tipik API istemcilerinin aksine, Web tarayıcıları üst `Accept` bilgileri sağlar. Web tarayıcısı, joker karakterler dahil olmak üzere birçok biçim belirtir. Varsayılan olarak, Framework isteğin bir tarayıcıdan geldiğini algıladığında:

* `Accept` Üst bilgi yok sayılır.
* Aksi yapılandırılmadığı takdirde içerik JSON içinde döndürülür.

Bu, API 'Leri tükettiren tarayıcılarda daha tutarlı bir deneyim sağlar.

Bir uygulamayı tarayıcı onay üstbilgilerini kabul edecek şekilde yapılandırmak için, <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> şu `true`şekilde ayarlayın:

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

Kullanılarak <xref:System.Xml.Serialization.XmlSerializer> uygulanan XML formatlayıcıları, çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>yapılandırılır:

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

Yukarıdaki kod, kullanılarak `XmlSerializer`sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir.

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

ASP.NET Core 3,0 ' dan önce, varsayılan olarak kullanılan JSON formatlayıcıları `Newtonsoft.Json` paket kullanılarak uygulanır. ASP.NET Core 3,0 veya sonraki bir sürümde, varsayılan JSON formatlayıcıları temel `System.Text.Json`alınır. Tabanlı formatlayıcılar ve özellikler için `Newtonsoft.Json` destek, [Microsoft. aspnetcore. Mvc. newtonsoftjson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketini yükleyerek ve içinde `Startup.ConfigureServices`yapılandırılarak kullanılabilir.

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

Bazı özellikler, `Newtonsoft.Json`tabanlı formatlayıcılar ile `System.Text.Json`düzgün çalışmayabilir ve tabanlı formatlara bir başvuru gerektirir. Uygulama şu durumlarda `Newtonsoft.Json`tabanlı formatlayıcıları kullanmaya devam edin:

* Öznitelikleri `Newtonsoft.Json` kullanır. Örneğin, `[JsonProperty]` veya `[JsonIgnore]`.
* Serileştirme ayarlarını özelleştirir.
* , Tarafından `Newtonsoft.Json` sağlanan özellikleri kullanır.
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

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a>XML biçimi desteği ekle

XML biçimlendirme, [Microsoft. AspNetCore. Mvc. Formatters. xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketini gerektirir.

Kullanılarak <xref:System.Xml.Serialization.XmlSerializer> uygulanan XML formatlayıcıları, çağırarak <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>yapılandırılır:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

Yukarıdaki kod, kullanılarak `XmlSerializer`sonuçları seri hale getirir.

Önceki kodu kullanırken, denetleyici yöntemleri isteğin `Accept` üstbilgisine göre uygun biçimi döndürmelidir.

::: moniker-end

### <a name="specify-a-format"></a>Biçim belirtin

Yanıt biçimlerini kısıtlamak için [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtreyi uygulayın. Çoğu [filtre](xref:mvc/controllers/filters) `[Produces]` gibi eylem, denetleyici veya genel kapsamda de uygulanabilir:

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

Önceki [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filtre:

* Denetleyici içindeki tüm eylemleri JSON biçimli yanıtları döndürecek şekilde zorlar.
* Diğer formatlayıcılar yapılandırıldıysa ve istemci farklı bir biçim belirtiyorsa JSON döndürülür.

Daha fazla bilgi için bkz. [Filtreler](xref:mvc/controllers/filters).

### <a name="special-case-formatters"></a>Özel durum formatları

Bazı özel durumlar, yerleşik formatlayıcılar kullanılarak uygulanır. Varsayılan olarak, `string` dönüş türleri *metin/düz* olarak biçimlendirilir ( `Accept` üst bilgi ile isteniyorsa*metin/html* ). Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter>' ı kaldırılarak silinebilir. Biçimlendiriciler `Configure` yönteminde kaldırılır. Bir model nesne dönüş türü döndürme `204 No Content` `null`sırasında döndürülen eylemler. Bu davranış, <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>' ı kaldırılarak silinebilir. Aşağıdaki kod `TextOutputFormatter` ve `HttpNoContentOutputFormatter`öğesini kaldırır.

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupTextOutputFormatter.cs?name=snippet)]
::: moniker-end

`TextOutputFormatter`Olmadan, `string` dönüş türleri döndürülür `406 Not Acceptable`. Bir XML biçimlendiricisi varsa, bu, kaldırılırsa `string` dönüş türlerini `TextOutputFormatter` biçimlendirir.

`HttpNoContentOutputFormatter`Olmadan, null nesneler yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin:

* JSON biçimlendiricisi, gövdesi `null`olan bir yanıt döndürür.
* XML biçimlendiricisi özniteliği `xsi:nil="true"` ayarlanmış bir boş XML öğesi döndürüyor.

## <a name="response-format-url-mappings"></a>Yanıt biçimi URL eşlemeleri

İstemciler URL 'nin bir parçası olarak belirli bir biçim talep edebilir, örneğin:

* Sorgu dizesinde veya yolun bir bölümünde.
* . Xml veya. JSON gibi formata özgü bir dosya uzantısı kullanarak.

İstek yolundan eşleme, API 'nin kullandığı rotada belirtilmelidir. Örneğin:

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

Önceki yol, istenen biçimin isteğe bağlı bir dosya uzantısı olarak belirtilmesini sağlar. [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) Özniteliği ,`RouteData` içindeki biçim değerinin varlığını denetler ve yanıt oluşturulduğunda yanıt biçimini uygun biçimlendirici ile eşler.

|           Yolu            |             Biçimlendirici              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Varsayılan çıkış biçimlendiricisi    |
| `/products/GetById/5.json` | JSON biçimlendiricisi (yapılandırıldıysa) |
| `/products/GetById/5.xml`  | XML biçimlendiricisi (yapılandırıldıysa)  |
