---
uid: web-api/overview/error-handling/exception-handling
title: Özel durum işleme ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807667"
---
<a name="exception-handling-in-aspnet-web-api"></a>Özel durum işleme ASP.NET Web API
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu makalede, hata ve özel durum işleme ASP.NET Web API'de açıklanır.

- [HttpResponseException](#httpresponserexception)
- [Özel durum filtreleri](#exception_filters)
- [Özel durum filtreleri kaydediliyor](#registering_exception_filters)
- [HTTP hatası](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Bir Web API denetleyicisi yakalanmayan bir özel durum oluşturursa ne olur? Varsayılan olarak, çoğu özel durumların bir HTTP yanıtının durum kodu, 500 İç sunucu hatası ile çevrilir.

**HttpResponseException** bir özel durum türüdür. Bu durum, özel durum oluşturucuda belirttiğiniz herhangi bir HTTP durum kodu döndürür. Örneğin, aşağıdaki yöntem, 404, bulunamadı, döndürür *kimliği* parametresi geçerli değil.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Yanıt üzerinde daha fazla denetim için ayrıca tüm yanıt iletisi oluşturmak ve bunu ile dahil **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Özel durum filtreleri

Web API yazarak özel durumları nasıl işlediğini özelleştirebileceğiniz bir *özel durum filtresi*. Bir denetleyici yöntemi olan bir işlenmeyen özel durum oluşturduğunda, özel durum filtresi yürütülür *değil* bir **HttpResponseException** özel durum. **HttpResponseException** özellikle bir HTTP yanıtı döndüren için tasarlandığından türü olan bir özel durum.

Özel durum filtreleri uygulamak **System.Web.Http.Filters.IExceptionFilter** arabirimi. Türetilmesi için bir özel durum filtresi yazma en kolay yolu olan **System.Web.Http.Filters.ExceptionFilterAttribute** sınıf ve geçersiz kılma **OnException** yöntemi.

> [!NOTE]
> ASP.NET Web API'de özel durum filtreleri ASP.NET mvc'de benzerdir. Ancak, bunlar ayrı ad alanı ve işlevi ayrı olarak bildirilir. Özellikle, **HandleErrorAttribute** MVC'de kullanılan sınıf Web APİ'si denetleyicilerinin tarafından oluşturulan özel durumları işlemez.


İşte dönüştüren bir filtre **NotImplementedException** 501, uygulanmadı özel durumları HTTP durum kodu:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**Yanıt** özelliği **HttpActionExecutedContext** nesne istemciye gönderilecek HTTP yanıt iletisi içerir.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Özel durum filtreleri kaydediliyor

Bir Web API özel durum filtre kaydetmek için birkaç yol vardır:

- Eylem tarafından
- Denetleyici tarafından
- Genel olarak

Belirli bir eylem için filtre uygulamak için filtre eylemi için bir özniteliği olarak ekleyin:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Tüm bir denetleyici eylemleri için filtre uygulamak için bir özniteliği olarak filtre denetleyici sınıfına ekleyin:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Filtre için tüm Web APİ'si denetleyicilerinin genel olarak uygulamak için filtre bir örneğini ekleme **GlobalConfiguration.Configuration.Filters** koleksiyonu. Özel durum filtreleri bu koleksiyondaki herhangi bir Web API denetleyici eylemi için geçerlidir.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Projenizi oluşturmak için "ASP.NET MVC 4 Web uygulaması" proje şablonunu kullanıyorsanız, Web API configuration kodunuzda gezebilirsiniz put `WebApiConfig` uygulamada bulunan sınıf\_başlangıç klasörü:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HTTP hatası

**HTTP hatası** nesnesi, yanıt gövdesi içinde hata bilgilerini döndürmek için tutarlı bir yol sağlar. Aşağıdaki örnek, ile nasıl HTTP durum kodu 404 (bulunamadı) döndürüleceğini gösterir. bir **HTTP hatası** yanıt gövdesi içinde.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** içinde bir uzantı yönteminin tanımlandığı **System.Net.Http.HttpRequestMessageExtensions** sınıfı. Dahili olarak **CreateErrorResponse** oluşturur bir **HTTP hatası** örnek ve ardından oluşturan bir **HttpResponseMessage** içeren **HTTPhatası**.

Bu örnekte, yöntem başarılı olursa üründe bir HTTP yanıtı döndürür. Ancak istenen ürüne bulunamazsa, HTTP yanıtı içeren bir **HTTP hatası** istek gövdesindeki. Yanıt aşağıdaki gibi görünebilir:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Dikkat **HTTP hatası** Bu örnekte, JSON için seri duruma. Kullanmanın bir avantajı **HTTP hatası** aynı gittiği yerdir [içerik anlaşması](../formats-and-model-binding/content-negotiation.md) ve seri hale getirme işlem diğer kesin olarak belirlenmiş model.

### <a name="httperror-and-model-validation"></a>HTTP hatası ve Model doğrulama

Model doğrulama için model durumuna geçirebilirsiniz **CreateErrorResponse**, yanıtta doğrulama hataları dahil etmek için:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Bu örnekte şu yanıtı döndürebilir:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API'de Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>HTTP hatası HttpResponseException ile kullanma

Önceki örneklerde dönüş bir **HttpResponseMessage** denetleyici eylemi, ancak ileti de kullanabilir **HttpResponseException** döndürülecek bir **HTTP hatası**. Bu, kesin türü belirtilmiş bir model hala döndürürken normal başarı durumunda iade sağlar **HTTP hatası** bir hata varsa:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
