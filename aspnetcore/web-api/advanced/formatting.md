---
title: ASP.NET Core Web API yanıt verileri biçimlendirme
author: ardalis
description: ASP.NET Core Web API içinde yanıt verinin nasıl biçimlendirileceğini öğrenin.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/formatting
ms.openlocfilehash: f25705bcd3a704de12ea24c3e196de1ba1afd492
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "32078644"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a>ASP.NET Core Web API yanıt verileri biçimlendirme

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC yanıt istemci belirtimlerine veya sabit biçimlerini kullanarak yanıt verileri biçimlendirme için yerleşik desteğe sahiptir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="format-specific-action-results"></a>Biçim özel eylem sonuçlarını

Bazı eylem sonucu türleri gibi belirli bir biçime belirli `JsonResult` ve `ContentResult`. Eylemler her zaman belirli bir şekilde biçimlendirilmiş belirli sonuçlar döndürebilir. Örneğin, döndüren bir `JsonResult` istemci Tercihler bakılmaksızın JSON biçimli verileri döndürür. Benzer şekilde, döndüren bir `ContentResult` düz metin biçimli dize verilerini (yalnızca döndüren bir dize olarak) döndürür.

> [!NOTE]
> Belirli bir türü döndürmek için bir eylem gerekli değildir; MVC hiçbir nesne dönüş değeri destekler. Bir eylem döndürürse bir `IActionResult` uygulama ve denetleyici devraldığı `Controller`, geliştiricilerin birçok seçenek karşılık gelen çok sayıda yardımcı yöntemler vardır. Nesneler döndürmeye Eylemler sonuçlarından olmayan `IActionResult` türleri serileştirilmiş uygun kullanarak `IOutputFormatter` uygulaması.

Belirli bir biçimde veri öğesinden devralınan bir denetleyici döndürmek için `Controller` temel sınıfı, yerleşik yardımcı yöntemi `Json` JSON dönün ve `Content` düz metin için. Eylem yönteminin belirli sonuç türü döndürmelidir (örneğin, `JsonResult`) veya `IActionResult`.

JSON biçimli veriyor:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Bu eylem yanıttan örnek:

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi application/json şeklindedir](formatting/_static/json-response.png)

Yanıtın içerik türü olduğuna dikkat edin `application/json`hem ağ isteklerin listesini ve yanıt üstbilgileri bölümünde gösterilen. Ayrıca, tarayıcıda (Bu durumda, Microsoft Edge) istek üstbilgileri bölümünde Accept üstbilgisindeki tarafından sunulan seçeneklerinin listesini unutmayın. Geçerli teknik bu başlığı sayıyor; Bu obeying aşağıda ele alınmıştır.

Düz metin olarak biçimlendirilmiş verileri döndürmek için kullanmak `ContentResult` ve `Content` Yardımcısı:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Bu eylem yanıttan:

![Yanıtın içerik türü gösteren Microsoft edge'de Geliştirici Araçları'nın Ağ sekmesi metin/düz değil](formatting/_static/text-response.png)

Bu durumda Not `Content-Type` döndürülen olduğu `text/plain`. Ayrıca, yalnızca bir dize yanıt türünü kullanarak bu aynı davranışı elde edebilirsiniz:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Birden çok önemsiz olmayan eylemler için dönüş türleri veya seçenekler (örneğin, farklı HTTP durum kodları sonucuna göre gerçekleştirilen işlemler), tercih ettiğiniz `IActionResult` dönüş türü.

## <a name="content-negotiation"></a>İçerik anlaşması

İçerik anlaşması (*conneg* kısaca) oluşur. istemcinin belirttiği bir [Accept üstbilgisi](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). ASP.NET Core MVC tarafından kullanılan varsayılan JSON biçimidir. İçerik anlaşması tarafından gerçekleştirilir `ObjectResult`. Ayrıca belirli eylem sonuçlarını Yardımcısı yöntemleri döndürülen durum kodu içinde yerleşik (hangi tüm temel `ObjectResult`). Model türü (veri aktarımı türü olarak tanımlanmış bir sınıf) geri dönebilirsiniz ve framework içinde otomatik olarak kaydırılır bir `ObjectResult` sizin için.

Aşağıdaki bir eylem yöntem `Ok` ve `NotFound` yardımcı yöntemler:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

JSON biçimli bir yanıt, başka bir biçime istendi ve sunucu istenen biçim döndürebilirsiniz sürece döndürülür. Gibi bir araç kullanabilirsiniz [Fiddler](http://www.telerik.com/fiddler) bir Accept üstbilgisi içeren bir isteği oluşturun ve başka bir biçim belirtin. Sunucusu varsa, bu durumda, bir *biçimlendirici* , istenen biçiminde bir yanıt oluşturabilirsiniz, sonuç istemci tercih edilen biçimde döndürülür.

![El ile oluşturulan bir gösteren fiddler konsol uygulama/xml bir Accept üstbilgi değeri ile isteği alma](formatting/_static/fiddler-composer.png)

Yukarıdaki ekran görüntüsünde, Fiddler Oluşturucusu bir isteği oluşturmak için kullanılmış belirtme `Accept: application/xml`. Varsayılan olarak, ASP.NET Core MVC yalnızca JSON, bu nedenle bile destekleyen başka bir biçime belirtildiğinde, hala JSON biçimli döndürülen sonuç. Sonraki bölümde ek biçimlendiricileri ekleme görürsünüz.

Denetleyici eylemleri durumda ASP.NET Core MVC otomatik olarak oluşturur (düz eski CLR nesneler), POCOs dönebilirsiniz bir `ObjectResult` , nesne sarmalar. İstemci biçimlendirilmiş serileştirilmiş nesne alır (JSON biçiminde varsayılan değerdir; XML veya diğer biçimlere yapılandırabilirsiniz). Nesne olan döndürülür, `null`, framework döndürecektir bir `204 No Content` yanıt.

Bir nesne türü döndüren:

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

Örnekte, geçerli Yazar diğer adı için bir istek yazarın verilerle 200 Tamam bir yanıt alır. Geçersiz bir diğer ad için bir istek 204 Hayır içerik yanıtı alır. XML ve JSON biçimlerde yanıtı gösteren ekran görüntüsü aşağıda verilmiştir.

### <a name="content-negotiation-process"></a>İçerik anlaşma işlemi

İçerik *anlaşma* yalnızca gerçekleşir, bir `Accept` üstbilgisi istekte görünür. Bir accept üstbilgisi bir istek içerdiğinde, framework tercih sırasına göre accept üstbilgisindeki medya türlerini numaralandırır ve kabul etme üstbilgisi tarafından belirtilen biçimlerden birinde bir yanıt oluşturmak üzere bir biçimlendirici bulmayı dener. İstemcinin isteği karşılamak biçimlendirici bulunamazsa durumda framework yanıt oluşturmak üzere ilk biçimlendirici bulmayı dener (Geliştirici üzerinde seçeneği yapılandırmamışsa `MvcOptions` 406 döndürmek için kabul edilebilir değil yerine). XML isteği belirtir, ancak XML biçimlendiricisi yapılandırılmazsa, JSON biçimlendirici kullanılır. Daha genel olarak, bir biçimlendirici yoksa yapılandırılmışsa, istenen biçim sağlayabilir ve ardından nesne biçimlendirebilirsiniz ilk biçimlendirici kullanılır. Üst bilgi verilirse, döndürülecek nesnesi işleyebilen ilk biçimlendirici yanıtı serileştirmek için kullanılır. Bu durumda, herhangi bir anlaşma alma yeri hiç - sunucu kullanacağı biçimi belirliyor.

> [!NOTE]
> Accept üst bilgisi içeriyorsa `*/*`, üstbilgi sürece göz ardı edilir `RespectBrowserAcceptHeader` ayarlandığında true `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Tarayıcılar ve içerik anlaşması

Tipik API istemcilerden farklı olarak, web tarayıcıları tedarik eğilimindedir `Accept` biçimlerinin joker karakterleri dahil olmak üzere çok çeşitli dahil üstbilgileri. Varsayılan olarak, framework isteğinin tarayıcıdan geldiğini algıladığında, göz ardı eder `Accept` üstbilgi ve bunun yerine dönüş uygulamadaki içerik varsayılan biçimi yapılandırılmış (JSON aksi belirtilmedikçe). Bu API'ları kullanmak için farklı tarayıcılar kullanırken, daha tutarlı bir deneyim sağlar.

Uygulama Uy tarayıcınızın kabul üstbilgileri tercih ederseniz, bu MVC'ın yapılandırmasının bir parçası ayarlayarak yapılandırabileceğiniz `RespectBrowserAcceptHeader` için `true` içinde `ConfigureServices` yönteminde *haline*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Biçimlendiricileri yapılandırma

Uygulamanızın JSON varsayılan ötesinde ek biçimleri desteklemesi gerekiyorsa, NuGet paketleri ekleyebilir ve onları desteklemek için MVC yapılandırın. Giriş ve çıkış için ayrı biçimlendiricileri vardır. Tarafından kullanılan giriş biçimlendiricileri [Model bağlama](xref:mvc/models/model-binding); çıktı biçimlendiricileri yanıtları biçimlendirmek için kullanılır. Ayrıca yapılandırabilirsiniz [özel Biçimlendiricileri](xref:web-api/advanced/custom-formatters).

### <a name="adding-xml-format-support"></a>XML biçim desteği ekleme

XML biçimlendirme için destek eklemek için yükleme `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet paketi.

MVC'ın yapılandırmasında XmlSerializerFormatters eklemek *haline*:

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternatif olarak, yalnızca çıktı biçimlendirici ekleyebilirsiniz:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Bu iki yaklaşım kullanarak sonuçları seri hale `System.Xml.Serialization.XmlSerializer`. İsterseniz, kullanabileceğiniz `System.Runtime.Serialization.DataContractSerializer` ilişkili biçimlendirici ekleyerek:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

XML biçimlendirme desteği ekledikten sonra denetleyici yöntemlerine isteğin üzerinde göre uygun biçimde döndürmelidir `Accept` üstbilgi örnek gösterir bu Fiddler olarak:

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/xml. Ham sekmesi yanıtı için uygulama/xml Content-Type üstbilgisi değeri gösterir.](formatting/_static/xml-response.png)

Ham GET isteği ile yapıldığı denetçiler sekmesinde görebilirsiniz bir `Accept: application/xml` üstbilgi kümesi. Yanıt bölmesi gösterir `Content-Type: application/xml` başlık ve `Author` nesnesi, XML serileştirilmiş.

Belirtmek için istek değiştirmek için oluşturucu sekmesini kullanın `application/json` içinde `Accept` üstbilgi. İsteğin yürütülmesi ve yanıt JSON biçiminde olmalıdır:

![Fiddler konsol: istek için ham sekmesi gösterir Accept üstbilgi değeri uygulama/json. Ham sekmesi yanıtı için uygulama/json Content-Type üstbilgisi değeri gösterir.](formatting/_static/json-response-fiddler.png)

Bu ekran görüntüsünde, istek bir üstbilgisinin ayarlar görebilirsiniz `Accept: application/json` ve yanıt aynı belirtir, `Content-Type`. `Author` Nesnesi, JSON biçiminde yanıt gövdesine gösterilir.

### <a name="forcing-a-particular-format"></a>Belirli bir biçimde zorlama

Belirli bir eylem için yanıt formatları kısıtlamak istiyorsanız şunları yapabilirsiniz, uygulayabileceğiniz `[Produces]` Filtresi. `[Produces]` Filtresi özel bir eylem (veya denetleyicisi) yanıt biçimi belirtir. Çoğu gibi [filtreleri](xref:mvc/controllers/filters), bu eylem, denetleyici veya genel kapsam uygulanabilir.

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` Filtre içinde tüm eylemler zorlayacak `AuthorsController` diğer biçimlendiricileri uygulama ve sağlanan istemci için yapılandırılmış olsa bile yanıtları, JSON biçimli döndürmek için bir `Accept` farklı, isteyen üstbilgi kullanılabilir biçimi. Bkz: [filtreleri](xref:mvc/controllers/filters) genel filtre uygulamak da dahil olmak üzere daha fazla bilgi için.

### <a name="special-case-formatters"></a>Özel büyük/küçük harf Biçimlendiricileri

Bazı özel durumlar, yerleşik biçimlendiricileri kullanılarak uygulanır. Varsayılan olarak, `string` dönüş türleri olarak biçimlendirilir *metin/düz* (*metin/html* aracılığıyla istediyseniz `Accept` üstbilgi). Bu davranış kaldırarak kaldırılabilir `TextOutputFormatter`. İçinde biçimlendiricileri kaldırmak `Configure` yönteminde *haline* (aşağıda gösterilen). Model nesnesi olan eylemler dönüş türü döndürecektir bir 204 İçerik yanıt döndürülürken `null`. Bu davranış kaldırarak kaldırılabilir `HttpNoContentOutputFormatter`. Aşağıdaki kod kaldırır `TextOutputFormatter` ve `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Olmadan `TextOutputFormatter`, `string` türleri 406 iade edilemez, örneğin. Bir XML biçimlendiricisi varsa, onu biçimlendirecek Not `string` , dönüş türü `TextOutputFormatter` kaldırılır.

Olmadan `HttpNoContentOutputFormatter`, null nesneleri yapılandırılmış biçimlendirici kullanılarak biçimlendirilir. Örneğin, JSON biçimlendirici gövdesine sahip bir yanıtta yalnızca döndürülecek `null`XML biçimlendiricisi özniteliğiyle boş bir XML öğesi döndürülecek yaparken `xsi:nil="true"` ayarlayın.

## <a name="response-format-url-mappings"></a>Yanıt biçimi URL eşlemeleri

İstemcileri belirli bir biçimde URL'SİNİN bir parçası gibi sorgu dizesi veya bölümü yolunun veya .xml veya .json gibi bir biçim özel dosya uzantısını kullanarak isteyebilir. İstek yolu eşlemesinden API kullanarak rotada belirtilmesi gerekir. Örneğin:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Bu yol, bir isteğe bağlı bir dosya uzantısı olarak belirtilmesi için istenen biçimde olanak tanır. `[FormatFilter]` Özniteliği biçimi değeri varlığını denetler `RouteData` ve yanıt oluşturulduğunda, yanıt biçimi için uygun bir biçimlendirici eşler.


|           Rota            |             Biçimlendirici              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    Varsayılan çıkış biçimlendirici    |
| `/products/GetById/5.json` | JSON biçimlendirici (yapılandırıldıysa) |
| `/products/GetById/5.xml`  | XML biçimlendirici (yapılandırıldıysa)  |

