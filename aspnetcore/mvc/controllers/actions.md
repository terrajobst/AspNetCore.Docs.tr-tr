---
title: ASP.NET Core MVC 'de denetleyicilerle istekleri işleme
author: ardalis
description: ''
ms.author: riande
ms.date: 12/05/2019
uid: mvc/controllers/actions
ms.openlocfilehash: 715a73863513870d1cbd522e75013d41830da1e7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662794"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>ASP.NET Core MVC 'de denetleyicilerle istekleri işleme

, [Steve Smith](https://ardalis.com/) ve [Scott Ade](https://github.com/scottaddie) tarafından

Denetleyiciler, Eylemler ve eylem sonuçları, geliştiricilerin ASP.NET Core MVC kullanarak uygulama oluşturma konusunda temel bir parçasıdır.

## <a name="what-is-a-controller"></a>Denetleyici nedir?

Bir denetleyici, bir dizi eylemi tanımlamak ve gruplandırmak için kullanılır. Bir eylem (veya *eylem yöntemi*), bir denetleyicide istekleri işleyen bir yöntemdir. Denetleyiciler benzer eylemleri birlikte mantıksal olarak gruplayın. Bu eylemlerin toplamı, yönlendirme, önbelleğe alma ve yetkilendirme gibi ortak kural kümelerinin toplu olarak uygulanmasını sağlar. İstekler, [yönlendirme](xref:mvc/controllers/routing)aracılığıyla eylemlerle eşleştirilir.

Kurala göre, denetleyici sınıfları:

* Projenin kök düzeyi *denetleyiciler* klasöründe bulunur.
* `Microsoft.AspNetCore.Mvc.Controller`'den devralma.

Denetleyici, aşağıdaki koşullardan en az birinin doğru olduğu bir instantiable sınıfıdır:

* Sınıf adı `Controller`ile Sonya düzeltildi.
* Sınıfı, adı `Controller`sonındaki bir sınıftan devralır.
* `[Controller]` özniteliği sınıfa uygulanır.

Denetleyici sınıfı ilişkili bir `[NonController]` özniteliğine sahip olmamalıdır.

Denetleyiciler [Açık bağımlılıklar ilkesini](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)izlemelidir. Bu ilkeyi uygulamak için birkaç yaklaşım vardır. Birden çok denetleyici eylemi aynı hizmeti gerektiriyorsa, bu bağımlılıkları istemek için [Oluşturucu Ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) kullanmayı düşünün. Hizmet yalnızca tek bir eylem yöntemiyle gerekliyse, bağımlılığı istemek için [eylem ekleme işlemini](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) kullanmayı düşünün.

**M**odel-**V**IEW-**C**ontroller düzeninde bir denetleyici, modelin istek ve örneklemesinin ilk işlemeden sorumludur. Genellikle, iş kararları model içinde gerçekleştirilmelidir.

Denetleyici, modelin işleme sonucunu alır (varsa) ve uygun görünümü ve ilgili görünüm verilerini ya da API çağrısının sonucunu döndürür. [ASP.NET Core MVC 'ye genel bakış](xref:mvc/overview) ve [ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc)hakkında daha fazla bilgi edinin.

Denetleyici bir *UI düzeyi* soyutlamadır. Sorumlulukları, istek verilerinin geçerli olduğundan ve hangi görünümün (ya da bir API 'nin sonucunun) döndürüldüğünden emin sağlamaktır. İyi şekilde uyumlu olmayan uygulamalarda, doğrudan veri erişimi veya iş mantığı dahil değildir. Bunun yerine, denetleyici bu sorumlulukları işleyen hizmetlere temsilci seçer.

## <a name="defining-actions"></a>Eylemleri tanımlama

Bir denetleyicide, `[NonAction]` özniteliği olanlar hariç genel yöntemler eylemlerdir. Eylemlerdeki Parametreler istek verilerine bağlıdır ve [model bağlama](xref:mvc/models/model-binding)kullanılarak onaylanır. Model-bağlantılı her şey için model doğrulaması oluşur. `ModelState.IsValid` özelliği değeri, model bağlamanın ve doğrulamanın başarılı olup olmadığını gösterir.

Eylem yöntemleri bir sorunu iş açısından eşlemek için mantık içermelidir. İş kaygıları genellikle denetleyicinin [bağımlılık ekleme](xref:mvc/controllers/dependency-injection)yoluyla eriştiği hizmetler olarak temsil edilmelidir. Eylemler daha sonra iş eyleminin sonucunu bir uygulama durumuna eşler.

Eylemler her şeyi döndürebilir, ancak bir yanıt üreten `IActionResult` (veya zaman uyumsuz metotlar için `Task<IActionResult>`) bir örneğini döndürür. Eylem yöntemi, *ne tür bir yanıt*seçmekten sorumludur. Eylem sonucu *Yanıt*verir.

### <a name="controller-helper-methods"></a>Denetleyici Yardımcısı yöntemleri

Denetleyiciler genellikle [denetleyiciden](/dotnet/api/microsoft.aspnetcore.mvc.controller)devralınır, ancak bu gerekli değildir. `Controller` türetmek, üç yardımcı yöntem kategorisine erişim sağlar:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Yöntemler boş bir yanıt gövdesine yol açar

Yanıt gövdesinde betimleyen içerik olmadığından `Content-Type` HTTP yanıt üst bilgisi dahil değildir.

Bu kategori içinde iki sonuç türü vardır: Redirect ve HTTP durum kodu.

* **HTTP durum kodu**

    Bu tür bir HTTP durum kodu döndürür. Bu türden birkaç yardımcı yöntem `BadRequest`, `NotFound`ve `Ok`. Örneğin, `return BadRequest();` yürütüldüğünde 400 durum kodu üretir. `BadRequest`, `NotFound`ve `Ok` gibi yöntemler aşırı yüklendiğinde, içerik anlaşması gerçekleşdiğinden artık HTTP durum kodu Yanıtlayıcıları olarak niteleyemez.

* **Meniz**

    Bu tür bir eyleme veya hedefe yeniden yönlendirme döndürür (`Redirect`, `LocalRedirect`, `RedirectToAction`veya `RedirectToRoute`kullanarak). Örneğin, `Complete`bir anonim nesne geçirerek `return RedirectToAction("Complete", new {id = 123});` yeniden yönlendirir.

    Yeniden yönlendirme sonuç türü, birincil olarak `Location` HTTP yanıt üst bilgisi ekleme içindeki HTTP durum kodu türünden farklıdır.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Yöntemler, önceden tanımlanmış bir içerik türüyle boş olmayan bir yanıt gövdesine yol açar

Bu kategorideki birçok yardımcı yöntem bir `ContentType` özelliği içerir ve bu da yanıt gövdesini tanımlayacak `Content-Type` yanıt üst bilgisini ayarlamanıza olanak sağlar.

Bu kategori içinde iki sonuç türü vardır: [görüntüleme](xref:mvc/views/overview) ve [biçimli yanıt](xref:web-api/advanced/formatting).

* **Görünümü**

    Bu tür, HTML işlemek için bir model kullanan bir görünüm döndürür. Örneğin `return View(customer);`, veri bağlama için bir modeli bir modele geçirir.

* **Biçimlendirilen yanıt**

    Bu tür, bir nesneyi belirli bir şekilde göstermek için JSON veya benzer bir veri değişimi biçimi döndürür. Örneğin, `return Json(customer);`, belirtilen nesneyi JSON biçimine dizleştirir.
    
    Bu türün diğer yaygın yöntemleri `File` ve `PhysicalFile`içerir. Örneğin, `return PhysicalFile(customerFilePath, "text/xml");` [Physicalfileresult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult)döndürür.

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Yöntemler, istemci ile anlaşan bir içerik türünde biçimlendirilen boş olmayan bir yanıt gövdesinin oluşmasına neden olur

Bu kategori, **Içerik anlaşması**olarak daha iyi bilinir. [İçerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) , bir eylem bir [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) türü ya da [ıactionresult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) uygulaması dışında bir şey döndürdüğünde geçerlidir. `IActionResult` olmayan bir uygulama döndüren bir eylem (örneğin, `object`), aynı zamanda biçimli bir yanıt döndürür.

Bu türden bazı yardımcı yöntemler `BadRequest`, `CreatedAtRoute`ve `Ok`içerir. Bu yöntemlere örnek olarak sırasıyla `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`ve `return Ok(value);`verilebilir. `BadRequest` ve `Ok` yalnızca bir değer geçirildiğinde içerik anlaşması gerçekleştirdiğine unutmayın; bir değer geçirilmeden, bunun yerine HTTP durum kodu sonuç türleri olarak işlev görür. Diğer taraftan `CreatedAtRoute` yöntemi her zaman içerik anlaşması gerçekleştirir, çünkü aşırı yüklemelerinin hepsi bir değer geçirilmesini gerektirir.

### <a name="cross-cutting-concerns"></a>Çapraz kesme konuları

Uygulamalar genellikle iş akışının parçalarını paylaşır. Örnek olarak, alışveriş sepetine erişmek için kimlik doğrulaması gerektiren bir uygulama veya bazı sayfalarda verileri önbelleğe alan bir uygulama verilebilir. Bir eylem yönteminden önce veya sonra mantık gerçekleştirmek için bir *filtre*kullanın. Çapraz kesme sorunları üzerinde [filtrelerin](xref:mvc/controllers/filters) kullanılması, yinelemeyi azaltabilir.

`[Authorize]`gibi çoğu filtre özniteliği, istenen ayrıntı düzeyi düzeyine bağlı olarak denetleyiciye veya eylem düzeyine uygulanabilir.

Hata işleme ve yanıt önbelleklemesi genellikle çapraz kesme kaygılardır:
* [Hataları işleme](xref:mvc/controllers/filters#exception-filters)
* [Yanıtları Önbelleğe Alma](xref:performance/caching/response)

Birçok çapraz kesme konusu, filtreler veya özel [Ara yazılım](xref:fundamentals/middleware/index)kullanılarak işlenebilir.
