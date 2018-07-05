---
uid: web-api/overview/formats-and-model-binding/content-negotiation
title: İçerik anlaşması ASP.NET Web API'de | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API HTTP İçerik anlaşması nasıl uyguladığını açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/20/2012
ms.topic: article
ms.assetid: 0dd51b30-bf5a-419f-a1b7-2817ccca3c7d
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: c4e7a0c2601ca60f081876e83757997a2e920298
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368935"
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API'de içerik anlaşması
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Bu makalede, ASP.NET Web API içerik anlaşması nasıl uyguladığını açıklanır.

HTTP belirtimini (RFC 2616) "kullanılabilir birden çok temsile olduğunda belirtilen bir yanıt için en iyi gösterimi seçme işleminin." olarak içerik anlaşmasını tanımlar. HTTP İçerik anlaşması için birincil mekanizma olan bu istek üst bilgileri:

- **Kabul et:** hangi medya türü "application/json" gibi bir yanıt için kabul edilebilir "application/xml" veya bir özel medya türü gibi &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** hangi karakter kümelerini UTF-8 veya ISO 8859-1 gibi kabul edilebilir.
- **-Encoding kabul:** gzip gibi hangi içerik kodlamaları edilebilir.
- **Kabul dil:** tercih edilen doğal dili gibi "en-us".

Sunucu HTTP isteği diğer kısımlarını de bakabilirsiniz. İsteğin bir X-Requested-With üst bilgisi içeriyorsa, Accept üst bilgisi yok ise örneğin, bir AJAX isteği belirten JSON için varsayılan sunucu.

Bu makalede, Web API'si kabul et ve Accept-Charset üstbilgileri nasıl kullandığını adresindeki inceleyeceğiz. (Şu anda yerleşik desteği yoktur Accept-Encoding veya Accept-Language.)

## <a name="serialization"></a>Serileştirme

Bir Web API denetleyicisi, bir kaynak olarak CLR türü döndürürse, işlem hattı dönüş değeri serileştirir ve HTTP yanıt gövdesi yazar.

Örneğin, aşağıdaki denetleyici eylemi göz önünde bulundurun:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Bir istemci, bu HTTP isteği gönderebilir:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Yanıt olarak, sunucunun gönderebilir:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

Bu örnekte, JSON, Javascript veya "şey" istemci istenen (\*/\*). Sunucu ile bir JSON temsili Hybrid `Product` nesne. Yanıttaki Content-Type üstbilgisi ayarlandığına dikkat edin &quot;application/json&quot;.

Bir denetleyici de döndürebilir bir **HttpResponseMessage** nesne. Yanıt gövdesi için bir CLR nesnesi belirtmek için çağrı **CreateResponse** genişletme yöntemi:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Bu seçenek hakkında daha fazla denetime yanıt ayrıntılarını sağlar. Durum kodu ayarlamak, HTTP üst bilgileri ekleyin ve VS.

Kaynak serileştiren nesnesi olarak adlandırılan bir *medya biçimlendiricisi*. Medya biçimlendiricileri türetilen **MediaTypeFormatter** sınıfı. Web API'si, XML ve JSON için medya biçimlendiricileri sağlar ve diğer medya türleri desteklemek için özel biçimlendiriciler oluşturabilirsiniz. Özel bir Biçimlendiricinin yazma hakkında daha fazla bilgi için bkz: [medya Biçimlendiricileri](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Nasıl içerik anlaşması çalışır

İlk olarak, işlem hattı alır **IContentNegotiator** hizmetinde **HttpConfiguration** nesne. Ayrıca medya biçimlendiricileri listesi alır **HttpConfiguration.Formatters** koleksiyonu.

Ardından, işlem hattını çağıran **IContentNegotiatior.Negotiate**, içinde geçen:

- Serileştirilecek nesnenin türü
- Medya biçimlendiricileri koleksiyonu
- HTTP isteği

**Anlaş** yöntemi iki bilgi parçasını döndürür:

- Kullanmak için hangi biçimlendirici
- Yanıt medya türü

Biçimlendirici bulunamazsa **anlaş** yöntemi döndürür **null**ve istemci recevies HTTP Hatası 406 (kabul edilemez).

Aşağıdaki kod nasıl bir denetleyici içerik anlaşması doğrudan çağırabilir gösterir:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Bu kod eşdeğerdir için işlem hattı otomatik olarak yapar.

## <a name="default-content-negotiator"></a>Varsayılan içerik Uzlaştırıcı

**DefaultContentNegotiator** sınıf varsayılan uygulamasını sağlar **IContentNegotiator**. Bir biçimlendirici seçmek için birden çok ölçüt kullanır.

İlk olarak, biçimlendirici türü olmalıdır. Bu çağrı yaparak doğrulanır **MediaTypeFormatter.CanWriteType**.

Ardından, içerik uzlaştırıcı her bir Biçimlendiricinin arar ve onu HTTP isteğiyle ne kadar iyi eşleştiğini değerlendirir. Eşleşme değerlendirilecek içerik uzlaştırıcı iki şeyi biçimlendirici üzerinde görünür:

- **SupportedMediaTypes** desteklenen medya türlerinin bir listesini içeren bir koleksiyon. İçerik uzlaştırıcı Accept üst bilgisi isteğe karşı bu listesi ile eşleşecek şekilde çalışır. Accept üst bilgisi aralıkları dahil edebileceğinizi unutmayın. Örneğin, "text/plain" metin için bir eşleşme olduğundan /\* veya \* / \*.
- **MediaTypeMappings** listesini içeren bir koleksiyon **MediaTypeMapping** nesneleri. **MediaTypeMapping** sınıfı HTTP isteklerini medya türleriyle eşleşmesi için genel bir yol sağlar. Örneğin, özel bir HTTP üstbilgisi belirli bir ortama eşleyebilirsiniz.

Varsa birden çok yüksek kalite faktörü WINS eşleşmeyi eşleşir. Örneğin:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

Bu örnekte, uygulama/json 1.0, bir örtük kalite faktörü bulunduğundan application/xml ' tercih edilir.

Herhangi bir eşleşme bulunursa, içerik uzlaştırıcı istek gövdesinin medya türü üzerinden varsa çalışır. Örneğin, istek JSON veri içeriyorsa, içerik uzlaştırıcı JSON biçimlendirici için görünür.

Yine de herhangi bir eşleşme varsa, içerik uzlaştırıcı yalnızca türü serileştirebilen ilk biçimlendiriciyi alır.

## <a name="selecting-a-character-encoding"></a>Bir karakter kodlaması seçme

Biçimlendiriciyi seçtikten sonra içerik uzlaştırıcı en iyi karakter bakarak kodlamasını seçer **SupportedEncodings** biçimlendirici ve bu istek üstbilgisinde Accept-Charset (varsa) eşleştirme özelliği.
