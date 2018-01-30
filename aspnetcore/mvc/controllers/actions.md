---
title: "ASP.NET Core MVC denetleyicileri istekleri işleme"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 07/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/actions
ms.openlocfilehash: 1c6bf5ad92a43274af351652d240e2fa8873a956
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>ASP.NET Core MVC denetleyicileri istekleri işleme

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://github.com/scottaddie)

Denetleyicileri, Eylemler ve eylem sonucu, nasıl ASP.NET Core MVC kullanarak uygulamaları geliştiriciler, temel bir parçasıdır.

## <a name="what-is-a-controller"></a>Bir denetleyici nedir?

Bir denetleyici tanımlamak ve eylemleri kümesini gruplamak için kullanılır. Bir eylem (veya *eylem yönteminin*) istekleri işleyen bir denetleyicisine yöntemidir. Denetleyicileri mantıksal olarak benzer eylemler gruplandırın. Bu toplama eylemlerin ortak yönlendirme, önbelleğe alma ve topluca uygulanacak yetkilendirme gibi kurallar kümesi sağlar. İstekleri eylemlerine eşlendi [yönlendirme](xref:mvc/controllers/routing).

Kurala göre denetleyici sınıfları:
* Projenin kök düzeyinde içinde bulunan *denetleyicileri* klasörü
* Devralınmalıdır`Microsoft.AspNetCore.Mvc.Controller`

Aşağıdaki koşulların en az biri doğru olduğu bir instantiable sınıfı denetleyicisidir:
* Sınıf adı "Controller" sonekine sahip
* Adı "Controller" sonekine sahip bir sınıf sınıfının devraldığı
* Sınıf ile donatılmış `[Controller]` özniteliği

Denetleyici sınıfını ilişkili bir olmamalıdır `[NonController]` özniteliği.

Denetleyicileri izlemelidir [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/). Bu ilkeyi uygulamak için birkaç yaklaşım vardır. Aynı hizmetin birden fazla denetleyici eylemleri ihtiyacınız varsa kullanmayı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) bu bağımlılıkların istemek için. Hizmetin yalnızca bir tek eylem yöntemi tarafından gerekirse kullanmayı [eylem ekleme](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) bağımlılık istemek için.

İçinde **M**odel -**V**görünümü -**C**ontroller deseni, bir denetleyici isteğin ilk işleme ve modeli örneklemesi için sorumlu. Genellikle, iş kararları içindeki model gerçekleştirilmesi gerekir.

Denetleyici (varsa) modelin işlem sonucunu alır ve doğru görünüm ve ilişkili görünüm verileri veya API çağrısının sonucunu döndürür. Hakkında daha fazla bilgi edinin [ASP.NET Core MVC genel bakış](xref:mvc/overview) ve [ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).

Denetleyicisidir bir *UI düzeyi* Özet. Sorumlulukları istek verileri geçerli olduğundan emin olun ve hangi görünüm (veya bir API için sonuç) döndürülmelidir seçmek için ' dir. İyi faktörlere göre uygulamalarda, doğrudan veri erişim veya iş mantığı içermez. Bunun yerine, denetleyici bu Sorumluluklar işleme Hizmetleri atar.

## <a name="defining-actions"></a>Eylemleri tanımlama

Genel yöntemler bir denetleyicisinde ile donatılmış hariç `[NonAction]` özniteliği, eylemlerdir. Eylemler parametrelere istek verilerini bağlı ve kullanılarak doğrulandı [model bağlama](xref:mvc/models/model-binding). Model doğrulama modeline bağlı olan her şeyi için oluşur. `ModelState.IsValid` Özellik değeri, model bağlama ve doğrulama başarılı olup olmadığını gösterir.

Eylem yöntemleri iş sorunu için bir istek eşleme mantığı içermelidir. İş sorunları genellikle temsil denetleyicisi aracılığıyla erişen Hizmetleri olarak [bağımlılık ekleme](xref:mvc/controllers/dependency-injection). Bir uygulama durumu sonra harita iş eylemin sonucu eylemleri.

Eylemler herhangi bir şeyi geri ancak sık örneği döndürür `IActionResult` (veya `Task<IActionResult>` zaman uyumsuz yöntemleri için) bir yanıt oluşturur. Eylem yöntemi seçme için sorumlu olduğu *ne tür bir yanıt*. Eylem sonucu *yanıt vermiyor*.

### <a name="controller-helper-methods"></a>Denetleyici yardımcı yöntemler

Denetleyicileri genellikle devral [denetleyicisi](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), ancak bu zorunlu değildir. Türetme `Controller` yardımcı yöntemlerinin üç kategoride erişim sağlar:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Bir boş yanıt gövdesinde kaynaklanan yöntemleri

Hayır `Content-Type` HTTP yanıt üstbilgisi eklenmiştir, yanıt gövdesi açıklamak için içerik eksik olduğundan.

Bu kategoride iki sonuç türü vardır: yeniden yönlendirme ve HTTP durum kodu.

* **HTTP durum kodu**

    Bu tür bir HTTP durum kodu döndürür. Bu tür birkaç yardımcı yöntemler `BadRequest`, `NotFound`, ve `Ok`. Örneğin, `return BadRequest();` çalıştırıldığında 400 durum kodu oluşturur. Zaman gibi yöntemler `BadRequest`, `NotFound`, ve `Ok` olan içerik anlaşması yer aldığı bu yana aşırı, bunlar artık HTTP durum kodu Yanıtlayıcı hakkını kullanmaya devam eder.

* **Redirect**

    Bu tür bir yeniden yönlendirme için bir eylem veya hedef döndürür (kullanarak `Redirect`, `LocalRedirect`, `RedirectToAction`, veya `RedirectToRoute`). Örneğin, `return RedirectToAction("Complete", new {id = 123});` yönlendirir `Complete`, anonim bir nesne geçirme.

    HTTP durum kodu türünde öncelikle ek yeniden yönlendirme sonuç türü farklı bir `Location` HTTP yanıt üst bilgisi.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Önceden tanımlanmış bir içerik türüyle boş yanıt gövdesi sonuçta yöntemleri

Bu kategorideki çoğu yardımcı yöntemler içerir bir `ContentType` ayarlamanıza olanak veren özelliği, `Content-Type` yanıt gövdesi açıklamak için yanıt üstbilgisi.

Bu kategoride iki sonuç türü vardır: [Görünüm](xref:mvc/views/overview) ve [biçimlendirilmiş yanıt](xref:mvc/models/formatting).

* **Görünümü**

    Bu tür HTML oluşturmak için bir model kullanan bir görünüm verir. Örneğin, `return View(customer);` görünüm veri bağlama için bir model geçirir.

* **Biçimlendirilmiş yanıt**

    Bu tür bir nesne belirli bir şekilde temsil etmek için JSON veya benzer bir veri değişimi biçimi döndürür. Örneğin, `return Json(customer);` sağlanan nesneyi JSON biçimine serileştiren.
    
    Bu tür ortak diğer yöntemleri kapsar `File`, `PhysicalFile`, ve `VirtualFile`. Örneğin, `return PhysicalFile(customerFilePath, "text/xml");` tarafından açıklanan bir XML dosyası döndüren bir `Content-Type` "text/xml" yanıt üstbilgisi değeri.

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. İstemciyle anlaşılan bir içerik türü boş yanıt gövdesi içinde kaynaklanan yöntemleri biçimlendirilmiş

Bu kategori daha iyi denir **içerik anlaşması**. [İçerik anlaşması](xref:mvc/models/formatting#content-negotiation) bir eylem döndürür her geçerli bir [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) türü veya dışında bir şey bir [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) uygulaması. Olmayan bir döndüren bir eylem`IActionResult` uygulaması (örneğin, `object`) de biçimlendirilmiş bir yanıt döndürür.

Bu türdeki bazı yardımcı yöntemler içerir `BadRequest`, `CreatedAtRoute`, ve `Ok`. Bu yöntemler örnekleridir `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, ve `return Ok(value);`sırasıyla. Unutmayın `BadRequest` ve `Ok` yalnızca bir değer geçirildiğinde içerik anlaşması gerçekleştirin; bir değer geçirilen olmadan, bunun yerine HTTP durum kodu sonuç türleri olarak verdikleri. `CreatedAtRoute` Yöntemi, diğer yandan, içerik anlaşmasını tüm gerektiren bir değere geçirilecek kendi aşırı itibaren her zaman gerçekleştirir.

### <a name="cross-cutting-concerns"></a>Çapraz kesme sorunları

Uygulamalar genellikle kendi iş akışının bölümlerini paylaşır. Alışveriş sepeti erişmek için kimlik doğrulaması gerektiren bir uygulama veya bazı sayfalarında verileri önbelleğe alarak bir uygulama örnek olarak verilebilir. Önce veya sonra bir eylem yönteminin mantığı gerçekleştirmek için bir *filtre*. Kullanarak [filtreleri](xref:mvc/controllers/filters) üzerinde çapraz kesme sorunları izlemek vermeden çoğaltma azaltabilir [yok yineleyin kendiniz (KURU) İlkesi](http://deviq.com/don-t-repeat-yourself/).

En filtre öznitelikleri gibi `[Authorize]`, ayrıntı düzeyi istenen düzeye bağlı olarak denetleyici veya eylem düzeyinde uygulanabilir.

Hata işleme ve yanıt önbelleğe alma genellikle arası kesme sorunları şunlardır:
   * [Hata işleme](xref:mvc/controllers/filters#exception-filters)
   * [Yanıtları Önbelleğe Alma](xref:performance/caching/response)

Filtre ya da özel kullanılarak birçok arası kesme sorunları işlenebilir [Ara](xref:fundamentals/middleware).
