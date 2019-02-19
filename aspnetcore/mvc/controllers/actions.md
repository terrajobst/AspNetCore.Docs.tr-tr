---
title: ASP.NET Core MVC denetleyicileri tanıtıcı istekleri
author: ardalis
description: ''
ms.author: riande
ms.date: 07/03/2017
uid: mvc/controllers/actions
ms.openlocfilehash: 8289424b3cd3678bea18a25c7850e409795d1577
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410449"
---
# <a name="handle-requests-with-controllers-in-aspnet-core-mvc"></a>ASP.NET Core MVC denetleyicileri tanıtıcı istekleri

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://github.com/scottaddie)

Denetleyicileri, eylemleri ve eylem sonuçlarını nasıl geliştiriciler ASP.NET Core MVC kullanarak uygulamalar oluşturun, temel bir parçasıdır.

## <a name="what-is-a-controller"></a>Bir denetleyici nedir?

Bir denetleyici tanımlayın ve bir dizi eylemi gruplandırmak için kullanılır. Bir eylem (veya *eylem yöntemine*) isteklerini yürüten bir denetleyicisinde bir yöntemdir. Denetleyicileri mantıksal olarak benzer işlemler gruplandırın. Bu toplama eylemlerin gibi yönlendirme, önbelleğe alma ve topluca uygulanacak yetkilendirme kurallarının genel ayarlar sağlar. İstekleri Eylemler ile eşlendiğinden [yönlendirme](xref:mvc/controllers/routing).

Kural gereği, denetleyici sınıflarına:
* Projenin kök düzey içinde bulunan *denetleyicileri* klasörü
* Devralma `Microsoft.AspNetCore.Mvc.Controller`

Bir denetleyici, en az biri aşağıdaki koşulların true olduğu bir instantiable sınıfı şöyledir:
* Sınıf adı, "Controller" soneki
* Adlarında "Controller" sonekine sahip bir sınıftan sınıfından devralan
* Sınıf ile donatılmış `[Controller]` özniteliği

Denetleyici sınıfı bir ilişkili olmamalıdır `[NonController]` özniteliği.

Denetleyicileri izlemelidir [açık bağımlılıkları ilkesine](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies). Bu ilkeyi uygulamak için yaklaşımların birkaç vardır. Aynı hizmetin birden fazla denetleyici eylemleri ihtiyacınız varsa kullanmayı [Oluşturucu ekleme](xref:mvc/controllers/dependency-injection#constructor-injection) bu bağımlılıkların istemek için. Hizmet yalnızca bir tek bir eylem yöntemi tarafından gerekirse kullanmayı [eylem ekleme](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) bağımlılık istemek için.

İçinde **M**odel -**V**görüntüle -**C**ontroller desen, bir denetleyici isteğin ilk işleme ve modeli örneğinin sorumludur. Genellikle, iş kararlarını model içinde gerçekleştirilmelidir.

Denetleyici modelinin (varsa) işlem sonucunu alır ve doğru görünüm ve, ilişkili görünüm verileri veya API çağrısının sonucunu döndürür. Daha fazla bilgi [ASP.NET Core MVC genel bakış](xref:mvc/overview) ve [ASP.NET Core MVC ve Visual Studio ile çalışmaya başlama](xref:tutorials/first-mvc-app/start-mvc).

Denetleyici bir *UI düzeyi* Özet. Sorumlulukları şunlardır: İstek verileri geçerli olduğundan emin olmak için ve hangi görünümü (veya bir API için sonuç) döndürülmesi gerektiğini seçmek için. Katsayıları iyi belirlenmiş uygulamalarda, doğrudan veri erişim veya iş mantığı içermez. Bunun yerine, denetleyici bu sorumlulukları işleme Hizmetleri atar.

## <a name="defining-actions"></a>Eylemleri tanımlama

Bir denetleyici genel yöntemleri ile donatılmış hariç `[NonAction]` özniteliği, eylemlerdir. Eylemlerdeki parametreleri istek verilerini bağlı ve kullanılarak doğrulandı [model bağlama](xref:mvc/models/model-binding). Model doğrulama modeline bağlı olan her şey için gerçekleşir. `ModelState.IsValid` Özellik değeri, model bağlama ve doğrulama başarılı olup olmadığını gösterir.

Eylem yöntemleri için iş önemli bir talep eşleme için mantıksal içermelidir. İşle ilgili genellikle temsil edilemiyor denetleyicisi aracılığıyla eriştiği Hizmetleri olarak [bağımlılık ekleme](xref:mvc/controllers/dependency-injection). Eylemler, ardından iş eylem sonucu bir uygulama durumuna eşlenir.

Eylemler herhangi bir şey getirmesi ama sık örneğini döner `IActionResult` (veya `Task<IActionResult>` zaman uyumsuz yöntemler için), bir yanıt oluşturur. Eylem yöntemi, tercih ettiğiniz için sorumlu *ne tür bir yanıt*. Eylem sonucu *yanıt vermiyor*.

### <a name="controller-helper-methods"></a>Denetleyici yardımcı yöntemler

Denetleyicileri genellikle devralınan [denetleyicisi](/dotnet/api/microsoft.aspnetcore.mvc.controller), ancak bu zorunlu değildir. Öğesinden türetme `Controller` yardımcı yöntemler üç kategorisi erişim sağlar:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Bunun sonucunda bir boş yanıt gövdesi içinde yöntemleri

Hayır `Content-Type` yanıt gövdesi açıklamak için içerik eksik olduğundan HTTP yanıt üst bilgisi dahil.

Bu kategorideki iki sonuç türleri şunlardır: Yeniden yönlendirme ve HTTP durum kodu.

* **HTTP durum kodu**

    Bu tür bir HTTP durum kodu döndürür. Bu tür yardımcı yöntemler birkaç olan `BadRequest`, `NotFound`, ve `Ok`. Örneğin, `return BadRequest();` yürütüldüğünde bir 400 durum kodu üretir. Zaman gibi yöntemler `BadRequest`, `NotFound`, ve `Ok` olan içerik anlaşması gerçekleşen beri aşırı, bunlar artık HTTP durum kodu Yanıtlayıcı niteleyin.

* **yeniden yönlendirme**

    Bu tür bir eylem veya hedef bir yeniden yönlendirme döndürür (kullanarak `Redirect`, `LocalRedirect`, `RedirectToAction`, veya `RedirectToRoute`). Örneğin, `return RedirectToAction("Complete", new {id = 123});` yönlendirir `Complete`, anonim bir nesne geçirme.

    Yeniden yönlendirme sonuç türü birincil ek olarak HTTP durum kodu türünden farklı bir `Location` HTTP yanıt üst bilgisi.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Önceden tanımlanmış bir içerik türüyle boş yanıt gövdesi içinde elde edilen yöntemleri

Bu kategorideki çoğu yardımcı yöntemler içerir. bir `ContentType` ayarlamanızı izin verme özelliği `Content-Type` yanıt gövdesi açıklamak için yanıt üst bilgisi.

Bu kategorideki iki sonuç türleri şunlardır: [Görünüm](xref:mvc/views/overview) ve [yanıt biçimlendirilmiş](xref:web-api/advanced/formatting).

* **Görünümü**

    Bu tür HTML oluşturmak için bir modeli kullanan bir görünüm verir. Örneğin, `return View(customer);` görünüm veri bağlama için bir model geçirir.

* **Biçimlendirilmiş yanıt**

    Bu tür bir nesne belirli bir şekilde temsil etmek için JSON veya benzer bir veri değişimi biçimi döndürür. Örneğin, `return Json(customer);` belirtilen nesneyi JSON biçimine serileştiren.
    
    Bu tür genel diğer yöntemleri kapsar `File` ve `PhysicalFile`. Örneğin, `return PhysicalFile(customerFilePath, "text/xml");` döndürür [PhysicalFileResult](/dotnet/api/microsoft.aspnetcore.mvc.physicalfileresult).

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. İstemciyle anlaşılan bir içerik türü boş yanıt gövdesi içinde elde edilen yöntemleri biçimlendirilmiş

Bu kategoriye daha iyi denir **içerik anlaşması**. [İçerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) bir eylem döndürür. her geçerli bir [ObjectResult](/dotnet/api/microsoft.aspnetcore.mvc.objectresult) türü veya dışında bir şey bir [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) uygulaması. Olmayan bir döndüren bir eylem`IActionResult` uygulaması (örneğin, `object`) da biçimlendirilmiş bir yanıt döndürür.

Bu tür bazı yardımcı yöntemler içerir `BadRequest`, `CreatedAtRoute`, ve `Ok`. Bu yöntemlerin örnekler `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, ve `return Ok(value);`sırasıyla. Unutmayın `BadRequest` ve `Ok` yalnızca bir değer geçirildiğinde içerik anlaşması gerçekleştirir; bir değer olmadan geçirilen, bunun yerine HTTP durum kodu sonuç türleri verdikleri. `CreatedAtRoute` Yöntemi, diğer taraftan, içerik anlaşmasını tüm gerektiren bir değere geçirilecek bunun aşırı yüklerinden beri her zaman gerçekleştirir.

### <a name="cross-cutting-concerns"></a>Geniş kapsamlı kritik konular

Uygulamalar genellikle kendi iş akışının bölümlerini paylaşın. Alışveriş sepetini erişmek için kimlik doğrulaması gerektiren bir uygulama veya bazı sayfalar verileri önbelleğe alan bir uygulama verilebilir. Önce veya sonra bir eylem yöntemi mantığını gerçekleştirmek için bir *filtre*. Kullanarak [filtreleri](xref:mvc/controllers/filters) geniş kapsamlı kritik konular üzerinde çoğaltma azaltabilir.

En filtre öznitelikleri gibi `[Authorize]`, istediğiniz ayrıntı düzeyi düzeye bağlı olarak denetleyici veya eylem düzeyinde uygulanabilir.

Hata işleme ve yanıt önbelleğe alma genellikle geniş kapsamlı kritik konular şunlardır:
   * [Hataları işleme](xref:mvc/controllers/filters#exception-filters)
   * [Yanıtları Önbelleğe Alma](xref:performance/caching/response)

Çok geniş kapsamlı kritik konular, filtreler veya özel kullanılarak işlenebilir [ara yazılım](xref:fundamentals/middleware/index).
