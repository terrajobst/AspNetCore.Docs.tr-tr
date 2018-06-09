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
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/content-negotiation
msc.type: authoredcontent
ms.openlocfilehash: ca373af6754e82889dc100b63f73b76aaa4e4f27
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566412"
---
<a name="content-negotiation-in-aspnet-web-api"></a>ASP.NET Web API'de içerik anlaşması
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu makalede, ASP.NET Web API içerik anlaşması nasıl uyguladığını açıklanmaktadır.

"Kullanılabilir birden çok Beyanları olduğunda belirtilen yanıt için en iyi gösterimi seçme işlemi." olarak içerik anlaşması HTTP belirtimi (RFC 2616) tanımlar HTTP İçerik anlaşması için birincil mekanizma olan bu istek üst bilgileri:

- **Kabul et:** hangi medya türü "application/json" gibi yanıtı için kabul edilebilir "application/xml" ya da özel ortam türü gibi &quot;application/vnd.example+xml&quot;
- **Accept-Charset:** hangi karakter kümesi UTF-8 veya ISO 8859-1 gibi kabul edilir.
- **-Encoding kabul:** gzip gibi hangi içerik kodlamaları edilebilir.
- **Kabul dil:** tercih edilen doğal dil gibi "en-us".

Sunucu HTTP isteği diğer kısımlarını de bakabilirsiniz. İsteği X-Requested-With üst bilgisi içeriyorsa, Accept üstbilgisi ise örneğin, bir AJAX isteği belirten JSON için varsayılan sunucu.

Bu makalede, Web API kabul ediyor ve Accept-Charset üstbilgileri nasıl kullandığını adresindeki göreceğiz. (Şu anda yerleşik desteği yoktur Accept-Encoding veya Accept-Language.)

## <a name="serialization"></a>Serileştirme

Web API denetleyicisi bir kaynak olarak CLR türü döndürürse, ardışık düzen dönüş değeri olarak serileştirir ve HTTP yanıt gövdesi yazar.

Örneğin, aşağıdaki denetleyici eylemi göz önünde bulundurun:

[!code-csharp[Main](content-negotiation/samples/sample1.cs)]

Bir istemci bu HTTP istek gönderebilir:

[!code-console[Main](content-negotiation/samples/sample2.cmd)]

Yanıt olarak, sunucunun gönderebilir:

[!code-console[Main](content-negotiation/samples/sample3.cmd)]

Bu örnekte, istemci istenen JSON, Javascript veya "hiçbir şey" (\*/\*). Sunucunun JSON temsili yanıtlanan `Product` nesnesi. Content-Type üstbilgisi yanıt kümesine bildirimi &quot;uygulama/json&quot;.

Bir denetleyici ayrıca dönebilirsiniz bir **httpresponsemessage öğesini** nesnesi. Yanıt gövdesi için bir CLR nesnesi belirtmek için arama **CreateResponse** genişletme yöntemi:

[!code-csharp[Main](content-negotiation/samples/sample4.cs)]

Bu seçenek yanıt ayrıntılarını üzerinde daha fazla denetim sağlar. Durum kodu ayarlamanız, HTTP üstbilgileri ekleme ve benzeri.

Kaynak serileştiren nesnesi olarak adlandırılan bir *medya biçimlendiricisi*. Medya biçimlendiricileri türetilen **MediaTypeFormatter** sınıfı. XML ve JSON için medya biçimlendiricileri Web API'si sağlar ve diğer medya türlerini desteklemek için özel biçimlendiricileri oluşturabilirsiniz. Özel bir biçimlendirici yazma hakkında daha fazla bilgi için bkz: [medya Biçimlendiricileri](media-formatters.md).

## <a name="how-content-negotiation-works"></a>Nasıl içerik anlaşması çalışır

İlk olarak, ardışık düzen alır **IContentNegotiator** gelen hizmet **HttpConfiguration** nesnesi. Da medya biçimlendiricileri listesini alır **HttpConfiguration.Formatters** koleksiyonu.

Ardından, ardışık düzen çağırır **IContentNegotiatior.Negotiate**, geçen içinde:

- Serileştirilecek nesnenin türü
- Medya biçimlendiricileri koleksiyonu
- HTTP isteği

**Anlaş** yöntemi iki parça bilgi verir:

- Kullanmak için hangi biçimlendirici
- Yanıt medya türü

Biçimlendirici bulunamazsa, **anlaş** yöntemi döndürür **null**ve istemci recevies HTTP Hatası 406 (kabul edilemez).

Aşağıdaki kod, nasıl bir denetleyici içerik anlaşması doğrudan çağırabileceği gösterir:

[!code-csharp[Main](content-negotiation/samples/sample5.cs)]

Bu kod eşdeğerdir ardışık düzen otomatik olarak ne yapar.

## <a name="default-content-negotiator"></a>Varsayılan içerik Uzlaştırıcı

**DefaultContentNegotiator** SAX varsayılan uygulaması **IContentNegotiator**. Bir biçimlendirici seçmek için çeşitli ölçütler kullanır.

İlk olarak, biçimlendirici türü seri hale getiremiyor olması gerekir. Bu çağırarak doğrulanır **MediaTypeFormatter.CanWriteType**.

Ardından, içerik uzlaştırıcı her bir Biçimlendiricinin arar ve ne kadar iyi HTTP isteği eşleştiğini değerlendirir. Eşleşme değerlendirmek için içerik uzlaştırıcı biçimlendirici üzerinde ikisinden görünür:

- **SupportedMediaTypes** desteklenen medya türlerinin bir listesini içeren koleksiyon. İçerik uzlaştırıcı bu listeyi Accept üstbilgisi isteği karşı eşleştirmeyi dener. Accept üstbilgisi aralıkları dahil edebileceğinizi unutmayın. Örneğin, "metin/düz" bir metin için eşleşmedir /\* veya \* / \*.
- **MediaTypeMappings** bir listesini içeren koleksiyon **MediaTypeMapping** nesneleri. **MediaTypeMapping** sınıfı HTTP isteklerini medya türleriyle eşleştirmek için genel bir yol sağlar. Örneğin, belirli bir medya türünün için özel bir HTTP üstbilgisi eşlemek.

Varsa birden çok eşleşme yüksek kalite faktörü WINS ile eşleşir. Örneğin:

[!code-console[Main](content-negotiation/samples/sample6.cmd)]

Bu örnekte, uygulama/json 1.0, bir kapsanan kalite faktörü sahiptir, böylece uygulama/xml tercih edilir.

Herhangi bir eşleşme bulunmazsa, içerik uzlaştırıcı istek gövdesi medya türünü eşleştirmek varsa çalışır. Örneğin, istek JSON veri içeriyorsa, içerik uzlaştırıcı JSON biçimlendirici için arar.

Hala herhangi bir eşleşme varsa, içerik uzlaştırıcı yalnızca türü serileştirebilen ilk biçimlendiriciyi alır.

## <a name="selecting-a-character-encoding"></a>Karakter kodlamasını seçme

Bir biçimlendirici seçtikten sonra içerik uzlaştırıcı en iyi karakter bakarak kodlamasını seçer **SupportedEncodings** biçimlendirici ve bu isteği Accept-Charset üstbilgisinde (varsa) eşleştirme özelliği.
