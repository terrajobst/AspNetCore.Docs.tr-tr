---
title: ASP.NET Core Web API'sinde özel biçimlendiricileri
author: rick-anderson
description: Oluşturmayı ve web ASP.NET Core API'leri için özel biçimlendiricileri kullanmayı öğrenin.
manager: wpickett
ms.author: tdykstra
ms.date: 02/08/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ec38a988a73278481de6535c627b2479a9805387
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2018
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>ASP.NET Core Web API'sinde özel biçimlendiricileri

Tarafından [zel Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC web API'leri JSON, XML veya düz metin biçimlerini kullanarak veri değişimi için yerleşik destek sahip olur. Bu makalede, özel biçimlendiricileri oluşturarak ek biçimleri için destek eklemek gösterilmiştir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Ne zaman özel biçimlendiricileri kullanılır

İstediğiniz zaman özel bir biçimlendirici kullanmak [içerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) yerleşik biçimlendiricileri (JSON, XML ve düz metin) tarafından desteklenmeyen bir içerik türü desteklemek için işlem.

Örneğin, bazı web API için istemci işleyebiliyorsa [Protobuf](https://github.com/google/protobuf) biçimi, daha etkili olduğundan Protobuf istemcilerle kullanmak isteyebilirsiniz. Veya kişi adları ve adresleri göndermek için web API isteyebilirsiniz [vCard](https://wikipedia.org/wiki/VCard) biçimi, kişi veri değişimi için sık kullanılan biçim. Bu makalede sağlanan örnek uygulamayı basit vCard biçimlendirici uygular.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Özel bir biçimlendirici kullanma genel bakış

Özel bir biçimlendirici oluşturmak için adımlar şunlardır:

* İstemciye gönderilecek verilerini seri hale getirmek istiyorsanız, bir çıktı biçimlendirici sınıfı oluşturun.
* İstemciden alınan verileri seri durumdan istiyorsanız, bir giriş biçimlendirici sınıfı oluşturun.
* Örnek, biçimlendiricilerin eklemek `InputFormatters` ve `OutputFormatters` koleksiyonlarda [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

Aşağıdaki bölümler bu adımların her biri için yönergeler ve kod örnekleri sağlar.

## <a name="how-to-create-a-custom-formatter-class"></a>Özel biçimlendirici sınıfı oluşturma

Bir biçimlendirici oluşturmak için:

* Sınıf uygun temel sınıfından türetilir.
* Geçerli bir medya türlerini ve Kodlamalar oluşturucuda belirtin.
* Geçersiz kılma `CanReadType` / `CanWriteType` yöntemleri
* Geçersiz kılma `ReadRequestBodyAsync` / `WriteResponseBodyAsync` yöntemleri
  
### <a name="derive-from-the-appropriate-base-class"></a>Uygun temel sınıfından türetilir

Metin medya türleri için (örneğin, vCard) türetilen [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) veya [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) temel sınıfı.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

İkili türleri için türetilen [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) veya [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) temel sınıfı.

### <a name="specify-valid-media-types-and-encodings"></a>Geçerli bir medya türlerini ve Kodlamalar belirtin

Ekleyerek oluşturucuda geçerli bir medya türleri ve Kodlamalar belirtin `SupportedMediaTypes` ve `SupportedEncodings` koleksiyonları.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]
> Bir biçimlendirici sınıf oluşturucu bağımlılık ekleme yapamazsınız. Örneğin, oluşturucuya bir Günlükçü parametresini ekleyerek bir Günlükçü alınamıyor. Hizmetlere erişmek için yönteme geçirilen context nesnesi kullanmak zorunda. Kod örneği [aşağıda](#read-write) bunun nasıl yapılacağı gösterilmektedir.

### <a name="override-canreadtypecanwritetype"></a>CanReadType/CanWriteType geçersiz kıl

İçine seri durumdan ya da geçersiz kılarak öğesinden seri türünü belirtmek `CanReadType` veya `CanWriteType` yöntemleri. Örneğin, yalnızca vCard metinden oluşturmak mümkün olabilir bir `Contact` türü tersi.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>CanWriteResult yöntemi

Bazı senaryolarda geçersiz kılmak zorunda `CanWriteResult` yerine `CanWriteType`. Kullanım `CanWriteResult` aşağıdaki koşullar doğruysa:

* Eylem yöntemi bir model sınıfı döndürür.
* Çalışma zamanında döndürülen türetilmiş sınıfları vardır.
* Sınıfı eylem tarafından döndürülen türetilmiş çalışma zamanında bilmeniz gerekir.

Örneğin, eylem yöntemi imzası döndürür varsayalım bir `Person` türü, ancak döndürebilir bir `Student` veya `Instructor` türetilen tür `Person`. Yalnızca işlemek için biçimlendirici istiyorsanız `Student` nesneleri denetleyin türünü [nesne](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) için sağlanan context nesnesi içinde `CanWriteResult` yöntemi. Kullanmak için gerekli olmadığını göz önünde bulundurun `CanWriteResult` eylem yönteminin döndüğünde `IActionResult`; bu durumda, `CanWriteType` yöntemi çalışma zamanı türü alır.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>ReadRequestBodyAsync/WriteResponseBodyAsync geçersiz kıl

Seri durumdan veya içinde seri hale getirme gerçek yapması `ReadRequestBodyAsync` veya `WriteResponseBodyAsync`. Aşağıdaki örnekte vurgulanmış satırlar (bunları Oluşturucusu parametrelerinden alınamıyor) bağımlılık ekleme kapsayıcısını Hizmetleri verimli şekilde nasıl gösterir.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>MVC özel bir biçimlendirici kullanacak şekilde yapılandırma

Özel bir biçimlendirici kullanmak için biçimlendirici sınıfının bir örneğini ekleme `InputFormatters` veya `OutputFormatters` koleksiyonu.

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Biçimlendiricileri ekledikten sırayla değerlendirilir. Birinci öncelik kazanır.

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [örnek uygulama](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample), uygulayan basit vCard giriş ve çıkış biçimlendiricileri. Uygulama okur ve aşağıdaki gibi görünen vCard Yazar:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

VCard çıktı, uygulamayı çalıştırın ve başlığı "metin/vcard" Get isteğini kabul et ile göndermek için görmek için `http://localhost:63313/api/contacts/` (Visual Studio'dan çalışırken) veya `http://localhost:5000/api/contacts/` (komut satırından çalışırken).

VCard kişiler bellek içi koleksiyona eklemek için yukarıdaki örnekteki gibi biçimlendirilmiş gövdesinde vCard metin ile Content-Type üstbilgisi "metin/vcard" ile aynı URL bir Post isteği gönderin.
