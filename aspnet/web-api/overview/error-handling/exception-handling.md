---
uid: web-api/overview/error-handling/exception-handling
title: "Özel durum işleme ASP.NET Web API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>Özel durum ASP.NET Web API işleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu makalede, hata ve özel durum işleme ASP.NET Web API'de açıklanmaktadır.

- [HttpResponseException](#httpresponserexception)
- [Özel durum filtreleri](#exception_filters)
- [Özel durum filtreleri kaydetme](#registering_exception_filters)
- [HTTP hatası](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Web API denetleyicisi yakalanmayan bir özel durum oluşturursa ne olur? Varsayılan olarak, bir HTTP yanıtı durum kodu 500, iç sunucu hatası ile içine çoğu özel durumlar çevrilir.

**HttpResponseException** türü olan bir özel durum. Bu özel durum oluşturucuda belirttiğiniz tüm HTTP durum kodu döndürür. Örneğin, aşağıdaki yöntemi 404, bulunamadı, döndürür *kimliği* parametresi geçerli değil.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Yanıt üzerinde daha fazla denetim için ayrıca tüm yanıt iletisi oluşturmak ve onunla dahil **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Özel durum filtreleri

Web API yazarak özel durumları nasıl işler özelleştirebilirsiniz bir *özel durum filtresi*. Denetleyici yöntemi herhangi işlenmeyen özel durum oluşturduğunda bir özel durum filtresi yürütülür *değil* bir **HttpResponseException** özel durum. **HttpResponseException** türü olan bir özel durum çünkü özel olarak bir HTTP yanıtının döndürmek için tasarlanmıştır.

Özel durum filtreleri uygulamak **System.Web.Http.Filters.IExceptionFilter** arabirimi. Öğesinden türetilen için bir özel durum filtresi yazma en basit yolu olan **System.Web.Http.Filters.ExceptionFilterAttribute** sınıfı ve geçersiz kılma **OnException** yöntemi.

> [!NOTE]
> ASP.NET Web API özel durum filtrelerini ASP.NET mvc'de benzerdir. Ancak, bunlar bir ayrı ad alanı ve işlev ayrı olarak bildirilir. Özellikle, **HandleErrorAttribute** MVC uygulamasında kullanılan sınıf Web API denetleyicisi tarafından oluşturulan özel durumları işleme değil.


Dönüştüren bir filtre işte **NotImplementedException** özel durumlar içine HTTP durum kodu 501, uygulanmadı:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Yanıt** özelliği **HttpActionExecutedContext** nesne istemciye gönderilen HTTP yanıt iletisi içerir.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Özel durum filtreleri kaydetme

Bir Web API özel durum filtresi kaydetmek için birkaç yolu vardır:

- Eylem tarafından
- Denetleyici tarafından
- Genel

Belirli bir eylem filtresi uygulamak için filtre eylemi için bir özniteliği olarak ekleyin:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Tüm bir denetleyici eylemleri filtre uygulamak için filtre öznitelik olarak denetleyici sınıfına ekleyin:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Tüm Web API denetleyicilerinin genel filtre uygulamak için filtre örneğini ekleme **GlobalConfiguration.Configuration.Filters** koleksiyonu. Bu koleksiyonda exeption filtreleri için herhangi bir Web API denetleyici eylemi uygular.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonu kullanırsanız, Web API yapılandırması kodunuzu içinde put `WebApiConfig` uygulamada bulunan sınıfı\_başlangıç klasörü:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HTTP hatası

**HTTP hatası** nesnesi, yanıt gövdesinde hata bilgilerini döndürmek için tutarlı bir yol sağlar. Aşağıdaki örnek, ile nasıl HTTP durum kodu 404 (bulunamadı) döndürüleceğini gösterir. bir **HTTP hatası** yanıt gövdesi içinde.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** bir genişletme yöntemi tanımlanan **System.Net.Http.HttpRequestMessageExtensions** sınıfı. Dahili olarak, **CreateErrorResponse** oluşturur bir **HTTP hatası** örneği ve ardından oluşturan bir **httpresponsemessage öğesini** içeren **HTTP hatası**.

Bu örnekte, yöntem başarılı olursa HTTP yanıt ürün döndürür. Ancak istenen ürün bulunmazsa, HTTP yanıtı içeren bir **HTTP hatası** istek gövdesindeki. Yanıt aşağıdakine benzeyebilir:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Dikkat **HTTP hatası** JSON olarak bu örnekte serileştirilmiş. Kullanmanın bir avantajı **HTTP hatası** aynı gider olan [içerik anlaşması](../formats-and-model-binding/content-negotiation.md) ve seri hale getirme işlemek gibi diğer kesin türü belirtilmiş modeli.

### <a name="httperror-and-model-validation"></a>HTTP hatası ve Model doğrulama

Model doğrulama için model durumuna geçirebilirsiniz **CreateErrorResponse**, doğrulama hataları yanıta dahil etmek için:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Bu örnekte aşağıdaki yanıtı döndürebilir:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>HTTP hatası HttpResponseException ile kullanma

Önceki örneklerde dönüş bir **httpresponsemessage öğesini** denetleyici eylemi, ancak ileti de kullanabilir **HttpResponseException** döndürmek için bir **HTTP hatası**. Bu, hala döndürürken normal başarılı durumda kesin türü belirtilmiş bir model dönüş sağlar **HTTP hatası** bir hata varsa:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
