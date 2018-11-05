---
title: ASP.NET Core Web API'si, özel biçimlendiriciler
author: rick-anderson
description: Oluşturma ve web ASP.NET Core API'leri için özel biçimlendiriciler kullanma hakkında bilgi edinin.
ms.author: tdykstra
ms.date: 02/08/2017
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: ee6f166ced41c41506f2a17a7d362399c165b718
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020656"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>ASP.NET Core Web API'si, özel biçimlendiriciler

tarafından [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC, JSON, XML veya düz metin biçimleri kullanarak, web API'leri veri değişimi için yerleşik destek sunmaktadır. Bu makalede, özel biçimlendiriciler oluşturarak ek biçimleri için destek eklenecek şekilde gösterilmektedir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="when-to-use-custom-formatters"></a>Özel biçimlendiriciler kullanıldığı durumlar

İstediğiniz zaman bir özel biçimlendiricisini kullanmayı [içerik anlaşması](xref:web-api/advanced/formatting#content-negotiation) yerleşik biçimlendiricileri (JSON, XML ve düz metin) tarafından desteklenmeyen bir içerik türünü desteklemek üzere işlem.

Örneğin, web API'niz için olan istemcilerin bazılarını işleyebilir [Protobuf](https://github.com/google/protobuf) biçimi daha etkilidir çünkü Protobuf istemcilerle kullanmak isteyebilirsiniz. Veya kişi adlarını ve adreslerini göndermek için web API'nizi isteyebileceğiniz [vCard](https://wikipedia.org/wiki/VCard) biçimi, kişi veri değişimi için yaygın olarak kullanılan bir biçimi. Bu makalede sağlanan örnek uygulama basit vCard biçimlendirici uygular.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Özel bir biçimlendirici kullanmayı genel bakış

Özel bir biçimlendirici oluşturup kullanacağınızı adımlar şunlardır:

* İstemciye gönderilecek verileri seri hale getirmek istiyorsanız, bir çıkış biçimlendirici sınıfı oluşturun.
* İstemciden alınan verileri seri durumdan istiyorsanız bir giriş biçimlendirici sınıfı oluşturun.
* Eklemek için biçimlendiricileri örneklerini `InputFormatters` ve `OutputFormatters` koleksiyonlarında [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions).

Aşağıdaki bölümler bu adımların her biri için rehberlik ve kod örnekleri sağlar.

## <a name="how-to-create-a-custom-formatter-class"></a>Özel biçimlendirici sınıfı oluşturma

Bir biçimlendirici oluşturmak için:

* Sınıfı, uygun bir temel sınıfından türetilir.
* Geçerli bir medya türleri ve kodlamaları oluşturucuda belirtin.
* Geçersiz kılma `CanReadType` / `CanWriteType` yöntemleri
* Geçersiz kılma `ReadRequestBodyAsync` / `WriteResponseBodyAsync` yöntemleri
  
### <a name="derive-from-the-appropriate-base-class"></a>Uygun temel sınıfından türetilir

Metin medya türleri için (örneğin, vCard) öğesinden türetilen [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) veya [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) temel sınıfı.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Giriş biçimlendiricisi örneği için bkz. [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).

İkili türleri için türetilen [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) veya [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) temel sınıfı.

### <a name="specify-valid-media-types-and-encodings"></a>Geçerli bir medya türleri ve kodlamayı belirtin

Ekleyerek oluşturucusunun içinde geçerli bir medya türleri ve kodlamayı belirtin `SupportedMediaTypes` ve `SupportedEncodings` koleksiyonları.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

Giriş biçimlendiricisi örneği için bkz. [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).

> [!NOTE]
> Bir biçimlendirici sınıftaki Oluşturucu bağımlılık ekleme yapamazsınız. Örneğin, oluşturucuya bir Günlükçü parametre ekleyerek bir Günlükçü alınamıyor. Hizmetlere erişmek için yönteme geçirilen bağlam nesnesi kullanmak zorunda. Bir kod örneği [aşağıda](#read-write) bunun nasıl yapılacağı gösterilmektedir.

### <a name="override-canreadtypecanwritetype"></a>CanReadType/CanWriteType geçersiz kıl

İçine seri durumdan veya öğesinden serileştirme geçersiz kılarak türü belirtmek `CanReadType` veya `CanWriteType` yöntemleri. Örneğin, yalnızca vCard metinden oluşturmak mümkün olabilir bir `Contact` türü geçme veya tam tersi.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

Giriş biçimlendiricisi örneği için bkz. [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).

#### <a name="the-canwriteresult-method"></a>CanWriteResult yöntemi

Geçersiz kılmak sahip bazı senaryolarda `CanWriteResult` yerine `CanWriteType`. Kullanım `CanWriteResult` aşağıdaki koşullar geçerli olduğunda:

* Bir model sınıfı, eylem yöntemi döndürür.
* Çalışma zamanında döndürülen türetilmiş sınıfları vardır.
* Sınıfı eylem tarafından döndürülen türetilmiş çalışma zamanında bilmeniz gerekir.

Örneğin, eylem yöntemi imzanızı döndürür varsayalım. bir `Person` türü, ancak döndürebilir bir `Student` veya `Instructor` , türetilen tür `Person`. Yalnızca işlemek için biçimlendirici istiyorsanız `Student` nesneleri kontrol türünü [nesne](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) sağlanan context nesnesi içinde `CanWriteResult` yöntemi. Kullanmak için gerekli olmadığını göz önünde bulundurun `CanWriteResult` eylem yöntemi döndüğünde `IActionResult`; bu durumda, `CanWriteType` yöntem çalışma zamanı türü alır.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>ReadRequestBodyAsync/WriteResponseBodyAsync geçersiz kıl

Seri durumdan çıkarılırken veya seri hale getirme gerçek kitaplıklarımızı `ReadRequestBodyAsync` veya `WriteResponseBodyAsync`. Vurgulanan satırları aşağıdaki örnekte, Hizmetleri (bunları Oluşturucu parametreler alınamıyor) bağımlılık ekleme kapsayıcısını alma gösterilmektedir.

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

Giriş biçimlendiricisi örneği için bkz. [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample).

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>MVC özel bir biçimlendirici kullanmak için yapılandırma

Özel bir biçimlendirici kullanılacak biçimlendirici sınıfı bir örneğini ekleme `InputFormatters` veya `OutputFormatters` koleksiyonu.

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

VCard çıktı, uygulamayı çalıştırmak ve kabul etme ile bir Get isteği üst bilgisi "metin/vcard" gönderme için görmek için `http://localhost:63313/api/contacts/` (Visual Studio çalışırken) veya `http://localhost:5000/api/contacts/` (komut satırından çalışırken).

VCard kişiler bellek içi koleksiyonuna eklemek için aynı URL'yi vCard metin yukarıdaki örnekte olduğu gibi biçimlendirilmiş gövdesinde ve Content-Type üstbilgisi "metin/vcard" ile bir Post isteği gönderin.
