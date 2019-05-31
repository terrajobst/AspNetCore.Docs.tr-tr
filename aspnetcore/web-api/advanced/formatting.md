---
title: ASP.NET Core Web API'si yanıtı verileri biçimlendirme
author: ardalis
description: ASP.NET Core Web API'si yanıtı verilerinde biçimlendirmeyi öğrenin.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 7628565d8646c0a057e28aa54dc9ce9198750c15
ms.sourcegitcommit: 9ae1fd11f39b0a72b2ae42f0b450345e6e306bc0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66415684"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API'si yanıtı verileri biçimlendirme

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC, sabit biçimlerini kullanarak, yanıt verilerini biçimlendirme veya yanıt istemci belirtimleri için yerleşik destek sunmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Biçim özel eylem sonuçları

Bazı eylem sonucu türleri gibi belirli bir biçimde belirli `JsonResult` ve `ContentResult`. Eylemler, her zaman belirli bir şekilde biçimlendirilmiş belirli sonuçlar döndürebilir. Örneğin, döndüren bir `JsonResult` istemci tercihleri bağımsız olarak, JSON biçimli verileri döndürür. Benzer şekilde, döndüren bir `ContentResult` (yalnızca bir dize döndüren gibi) düz metin biçimli dize verileri döndürür.

> [!NOTE]
> Herhangi bir türü için bir eylem gerekli değildir; MVC herhangi bir nesne dönüş değeri destekler. Bir eylem döndürürse bir `IActionResult` uygulama ve denetleyici devraldığı `Controller`, geliştiricilere seçimleri çoğuna karşılık gelen çok sayıda yardımcı yöntemler vardır. İlgili nesneler dönüş eylemleri sonuçlardan olmayan `IActionResult` türleri seri hale getirilemiyor uygun kullanarak `IOutputFormatter` uygulaması.

Devralınan denetleyicisinden belirli bir biçimde veri döndürülecek `Controller` temel sınıfı, yerleşik yardımcı yöntemi `Json` JSON döndürülecek ve `Content` düz metin. Eylem yöntemi, belirli bir sonuç türü döndürmesi gerekir (örneğin, `JsonResult`) veya `IActionResult`.

JSON biçimli veriler döndürme:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Bu eylem örnek yanıttan:

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi application/json şeklindedir.](formatting/_static/json-response.png)

Yanıtın içerik türü olduğuna dikkat edin `application/json`hem de ağ isteklerinin listesini yanıt üst kısmında gösterilen. Ayrıca istek üstbilgileri bölümünde Accept üst bilgisi tarayıcıda (Bu durumda, Microsoft Edge) tarafından sunulan seçeneklerin listesini unutmayın. Geçerli teknik bu üst bilgi yok sayıyor; Bunu obeying aşağıda ele alınmıştır.

Düz metin olarak biçimlendirilmiş veri döndürmek için `ContentResult` ve `Content` yardımcı:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Bu eylem için bir yanıt:

![Metin/düz içerik yanıtının türünü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi olduğu](formatting/_static/text-response.png)

Bu durumda Not `Content-Type` döndürülen olan `text/plain`. Ayrıca, yalnızca dize yanıt türünü kullanarak bu aynı davranışı elde edebilirsiniz:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Birden çok önemsiz olmayan eylemler için dönüş türü veya seçenekler (örneğin, gerçekleştirilen işlemleri sonucuna göre farklı HTTP durum kodları), tercih ettiğiniz `IActionResult` dönüş türü.

## <a name="content-negotiation"></a>İçerik anlaşması

İçerik anlaşması (*conneg* kısaca) istemci oluşur bir [Accept üst bilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). ASP.NET Core MVC tarafından kullanılan varsayılan JSON biçimidir. İçerik anlaşması tarafından gerçekleştirilen `ObjectResult`. Ayrıca belirli bir eylem sonuçlarını yardımcı yöntemlerinden döndürülen durum kodu ile oluşturulur (tüm alan üzerinde `ObjectResult`). Ayrıca, bir model türü (veri aktarımı türünüz olarak tanımlanmış bir sınıf) döndürebilir ve framework içinde otomatik olarak kaydırılır bir `ObjectResult` sizin için.

Aşağıdaki eylem yöntemini kullanan `Ok` ve `NotFound` yardımcı yöntemler:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Başka bir biçime istendi ve sunucu istenen biçimi döndürebilir sürece JSON biçimli bir yanıt döndürülür. Gibi bir araç kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) bir Accept üst bilgisi içeren bir istek oluşturun ve başka bir biçim belirtin. Sunucusu varsa, bu durumda, bir *biçimlendirici* , istenen biçimde yanıt üretebilir, sonuç istemci tercih edilmeyen biçiminde döndürülür.

![El ile oluşturulan bir gösteren fiddler konsol GET isteği bir Accept üstbilgi değeri uygulama/XML ile](formatting/_static/fiddler-composer.png)

Bir isteği oluşturmak için yukarıdaki ekran görüntüsünde, Fiddler Oluşturucu kullanıldı belirtme `Accept: application/xml`. Varsayılan olarak, ASP.NET Core MVC yalnızca JSON, böylece destekleyen başka bir biçime belirtildiğinde, JSON biçimli hala döndürülen sonuç. Sonraki bölümde ek biçimlendiricileri ekleme görürsünüz.

Denetleyici eylemleri, bu durumda ASP.NET Core MVC otomatik olarak oluşturur POCOs (düz eski CLR nesneleri) döndürebilir bir `ObjectResult` nesneyi sarmalayan sizin için. İstemci biçimlendirilmiş serileştirilmiş nesne alırsınız (JSON biçiminde varsayılan değerdir; XML veya diğer biçimlere yapılandırabileceğiniz). Döndürülen nesne olması durumunda `null`, framework döndürecektir bir `204 No Content` yanıt.

Bir nesne türü döndürüyor:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Aşağıdaki örnekte, geçerli Yazar diğer adı için bir istek yazarın verilerle 200 Tamam yanıtı alırsınız. Geçersiz diğer ad isteği 204 İçerik yok yanıt alırsınız. XML ve JSON biçimlerde yanıtı gösteren ekran görüntüsü aşağıda gösterilmiştir.

### <a name="content-negotiation-process"></a>İçerik anlaşma işlemi

İçerik *anlaşma* yalnızca gerçekleşir, bir `Accept` istekte üstbilgi görünür. İstek bir accept üst bilgisi içeriyorsa, framework accept üst bilgisi tercih sırasına göre medya türlerini numaralandırır ve accept üst bilgisi tarafından belirtilen biçimlerden birinde bir yanıt oluşturan bir biçimlendirici bulmayı dener. Biçimlendirici istemcinin isteği karşılamak bulunan durumda framework bir yanıt oluşturan ilk biçimlendiriciyi bulmayı dener (Geliştirici üzerinde seçeneği yapılandırmamışsa `MvcOptions` 406 döndürmek için kabul edilebilir değil yerine). İstek XML belirtiyor, ancak XML biçimlendiricisi yapılandırılmamış, JSON biçimlendirici kullanılır. Daha genel olarak, biçimlendirici yapılandırıldıysa, istenen biçimi sağlayın, sonra nesneyi biçimlendirebilirsiniz ilk biçimlendiriciyi kullanılır. Üst bilgi belirtilmezse, döndürülecek nesnesi işleyebilen ilk biçimlendiriciyi yanıtı serileştirmek için kullanılır. Bu durumda, herhangi bir anlaşma alma yerde yok - sunucu kullanacağı hangi biçimde belirliyor.

> [!NOTE]
> Accept üst bilgisi içeriyorsa `*/*`, üstbilgi sürece yok sayılacak `RespectBrowserAcceptHeader` ayarlandığında true `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve içerik anlaşması

Tipik API istemcilerden farklı olarak, web tarayıcıları tedarik eğilimindedir `Accept` biçimleri, joker karakterler dahil olmak üzere geniş bir yelpazede içeren üst bilgiler. Varsayılan olarak, çerçeve isteğinin bir tarayıcıdan geldiğini algıladığında yok `Accept` üstbilgi ve bunun yerine dönüş uygulamadaki içerik varsayılan biçimi yapılandırılmış (JSON aksi şekilde yapılandırılmadıkça). Bu, farklı tarayıcılar API'lerini tüketmesi kullanırken daha tutarlı bir deneyim sağlar.

Uygulama Uy tarayıcınızın kabul üst bilgileri tercih ederseniz, bu MVC'nin yapılandırmasının bir parçası olarak ayarlayarak yapılandırabilirsiniz `RespectBrowserAcceptHeader` için `true` içinde `ConfigureServices` yönteminde *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Biçimlendiricileri yapılandırma

Uygulamanız varsayılan değer olan JSON ek biçimleri için destek gerekiyorsa, NuGet paketlerini ekleyip, bunları desteklemek için MVC yapılandırabilirsiniz. Girdi ve çıktı ayrı biçimlendiricileri vardır. Tarafından kullanılan giriş biçimlendiricileri [Model bağlama](xref:mvc/models/model-binding); çıkış biçimlendiricileri yanıtları biçimlendirmek için kullanılır. Ayrıca [özel Biçimlendiricileri](xref:web-api/advanced/custom-formatters).

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a>Biçimlendiricileri System.Text.Json tabanlı yapılandırma

Özellikleri `System.Text.Json`-tabanlı biçimlendiricileri kullanarak yapılandırılabilir `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a>Newtonsoft.Json tabanlı JSON biçimi desteği eklendi

ASP.NET Core 3.0 önce MVC kullanarak JSON biçimlendiricileri kullanılarak uygulanan varsayılan `Newtonsoft.Json` paket. ASP.NET Core 3.0 veya sonraki sürümlerde, varsayılan JSON biçimlendiricileri dayalı `System.Text.Json`. Destek `Newtonsoft.Json`-tabanlı biçimlendiricileri ve özellikler kullanılabilir yükleyerek [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet paketi ve onu yapılandırma `Startup.ConfigureServices`.

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

İle bazı özellikler çalışmayabilir `System.Text.Json`-biçimlendiricileri temel alır ve bir başvuru gerektirir `Newtonsoft.Json`-tabanlı ASP.NET Core 3.0 sürümü için biçimlendiricileri. Kullanmaya devam `Newtonsoft.Json`-biçimlendiricileri, temel ASP.NET Core 3.0 veya üzeri uygulama:

* Kullanan `Newtonsoft.Json` öznitelikleri (örneğin, `[JsonProperty]` veya `[JsonIgnore]`), serileştirme ayarları özelleştirdikten veya kullanır üzerinde özelliklerinin `Newtonsoft.Json` sağlar.
* Yapılandırır `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`. ASP.NET Core 3.0 önce `JsonResult.SerializerSettings` kabul eden bir örneğini `JsonSerializerSettings` özgü olan `Newtonsoft.Json`.
* Oluşturur [Openapı](<xref:tutorials/web-api-help-pages-using-swagger>) belgeleri.

::: moniker-end

### <a name="add-xml-format-support"></a>XML biçimi desteği eklendi

::: moniker range="<= aspnetcore-2.2"

XML biçimlendirme desteği ASP.NET Core 2.2 veya önceki eklemek için yükleme [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet paketi.

::: moniker-end

XML biçimlendiricileri kullanılarak uygulanan `System.Xml.Serialization.XmlSerializer` çağırarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> içinde `Startup.ConfigureServices`:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatif olarak, XML biçimlendiricileri kullanılarak uygulanan `System.Runtime.Serialization.DataContractSerializer` çağırarak yapılandırılabilir <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> içinde `Startup.ConfigureServices`:

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

XML biçimlendirme desteği ekledikten sonra denetleyici yöntemlerinizi isteğin üzerinde göre uygun biçimde döndürmelidir `Accept` üst bilgisi olarak bu Fiddler örnek gösterir:

![Fiddler konsolu: Ham sekmesini istek için Accept üstbilgi değeri uygulama/xml gösterilir. Uygulama/xml Content-Type üstbilgisi değeri ham yanıt sekmesini gösterir.](formatting/_static/xml-response.png)

Ham GET isteği ile yapılan denetçiler sekmesinde gördüğünüz bir `Accept: application/xml` başlık kümesi. Yanıt bölmesi gösterir `Content-Type: application/xml` başlık ve `Author` nesne, XML'e seri.

Belirtmek için istek değiştirmek için oluşturucu sekmesini kullanın `application/json` içinde `Accept` başlığı. İsteği yürütün ve yanıtı JSON olarak biçimlendirilir:

![Fiddler konsolu: İstek için ham sekmesi, uygulama/json Accept üstbilgi değeri gösterir. Uygulama/json Content-Type üstbilgisi değeri ham yanıt sekmesini gösterir.](formatting/_static/json-response-fiddler.png)

Bu ekran görüntüsünde, istek üst bilgisine ayarlar görebilirsiniz `Accept: application/json` ve yanıt aynı belirtir, `Content-Type`. `Author` Nesnesi, JSON biçiminde yanıt gövdesine gösterilir.

### <a name="forcing-a-particular-format"></a>Belirli bir biçimde zorlama

Belirli bir eylemi yanıt biçimi kısıtlamak istiyorsanız, aşağıdakileri yapabilirsiniz, uygulayabileceğiniz `[Produces]` filtre. `[Produces]` Filtre belirli bir eylem (veya denetleyicisi) yanıt biçimi belirtir. İster en [filtreleri](xref:mvc/controllers/filters), bu eylem, denetleyici veya genel kapsamlı uygulanabilir.

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` Filtre, içindeki tüm eylemleri zorlayacak `AuthorsController` diğer biçimlendiricileri uygulama ve sağlanan istemci için yapılandırılmış olsa bile, JSON biçimli yanıt döndürülecek bir `Accept` farklı, isteyen başlığı kullanılabilir biçimi. Bkz: [filtreleri](xref:mvc/controllers/filters) nasıl genel filtre uygulamak da dahil olmak üzere daha fazla bilgi için.

### <a name="special-case-formatters"></a>Özel durum Biçimlendiricileri

Bazı özel durumlar, yerleşik biçimlendiricileri kullanılarak uygulanır. Varsayılan olarak, `string` dönüş türleri olarak biçimlendirilir *metin/düz* (*metin/html* aracılığıyla istenirse `Accept` üst bilgisi). Bu davranış, kaldırarak kaldırılabilir `TextOutputFormatter`. İçinde biçimlendiricileri Kaldır `Configure` yönteminde *Startup.cs* (aşağıda gösterilmiştir). Bir model nesnesi olan eylemler dönüş türü döndürür bir 204 İçerik yanıt döndürülürken `null`. Bu davranış, kaldırarak kaldırılabilir `HttpNoContentOutputFormatter`. Aşağıdaki kod kaldırır `TextOutputFormatter` ve `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Olmadan `TextOutputFormatter`, `string` döndürür dönüş türleri 406 Kabul edilemez, örneğin. XML biçimlendirici varsa, bunu biçimlendirecek Not `string` , dönüş türü `TextOutputFormatter` kaldırılır.

Olmadan `HttpNoContentOutputFormatter`, null nesnelerine yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin, JSON biçimlendirici bir yanıt gövdesi ile yalnızca döndürür `null`, XML biçimlendiricisi özniteliğine sahip boş bir XML öğesi döndürür ancak `xsi:nil="true"` ayarlayın.

## <a name="response-format-url-mappings"></a>Yanıt biçim URL eşlemeleri

İstemcilerin belirli bir biçimde URL'nin bir parçası gibi sorgu dizesi ya da yolun veya .xml veya .json gibi bir biçim özel dosya uzantısını kullanarak talep edebilir. İstek yolu eşlemesinden API'sini kullanarak bir yolun belirtilmesi gerekir. Örneğin:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Bu yol, bir isteğe bağlı dosya uzantısı olarak belirtilmesi istenen biçimi izin verir. `[FormatFilter]` Öznitelik biçimi değeri varlığını denetler `RouteData` ve yanıt oluşturulduğunda yanıt biçimi için uygun bir biçimlendirici eşler.

|           yol            |             Biçimlendirici              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Varsayılan çıkış biçimlendirici    |
| `/products/GetById/5.json` | JSON biçimlendirici (yapılandırılmışsa) |
| `/products/GetById/5.xml`  | XML biçimlendirici (yapılandırılmışsa)  |
