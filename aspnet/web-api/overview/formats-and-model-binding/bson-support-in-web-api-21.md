---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 BSON desteği | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 53ad705fad6d2225cecca4d73355bd6ebfcf56d5
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
ms.locfileid: "27984589"
---
<a name="bson-support-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 BSON desteği
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Web API 2.1 BSON için destek sunar. Bu konuda, Web API denetleyicisi (sunucu tarafı) ve .NET istemci uygulaması BSON kullanmayı gösterir.

## <a name="what-is-bson"></a>BSON nedir?

[BSON](http://bsonspec.org/) ikili seri hale getirme biçimi. "BSON" "İçin ikili JSON" anlamına gelir, ancak BSON ve JSON çok farklı serileştirilir. BSON olan "JSON benzeri", nesneleri ad-değer çiftleri, JSON olarak benzer olarak temsil edilir çünkü. JSON, sayısal veri türleri değil dizeleri bayt olarak depolanır

BSON basit, tarama kolay ve hızlı kodlamak ve çözmek için tasarlanmıştır.

- JSON olarak boyutunda BSON karşılaştırılabilir. Verilere bağlı olarak, daha küçük veya daha büyük bir JSON yükü BSON yükü olabilir. Base64 ile kodlanmış ikili veri olmadığı için bir görüntü dosyası gibi ikili verileri seri hale getirme için BSON JSON küçüktür.
- BSON belgeleri tarama bir Ayrıştırıcıyı kod çözme olmadan öğeleri atlayabilirsiniz öğeleri uzunluk alanı ile önek çünkü kolaydır.
- Sayısal veri türleri değil dizeleri sayı olarak depolandığından kodlama ve kod çözme, verimlidir.

.NET istemci uygulamaları gibi yerel istemcilerde JSON veya XML gibi metin tabanlı biçimler yerine BSON kullanarak yararlı olabilir. JavaScript doğrudan JSON yükü dönüştürebilirsiniz çünkü tarayıcı istemcileri için büyük olasılıkla JSON ile takılıyor istersiniz.

Neyse ki, Web API'sini kullanan [içerik anlaşması](content-negotiation.md), API'nizi hem biçimleri için destek ve seçin istemci olanak tanır.

## <a name="enabling-bson-on-the-server"></a>BSON sunucu üzerindeki etkinleştirme

Web API yapılandırmanızda eklemek **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonuna.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Şimdi istemci "uygulama/bson" isterse, Web API BSON biçimlendirici kullanın.

BSON diğer medya türleri ile ilişkilendirmek için bunları SupportedMediaTypes koleksiyonuna ekleyin. Aşağıdaki kod, desteklenen medya türleri için "application/vnd.contoso" ekler:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Örnek HTTP oturumu

Bu örnek için aşağıdaki model sınıfı artı basit bir Web API denetleyicisi kullanacağız:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Bir istemci aşağıdaki HTTP istek gönderebilir:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Yanıt şöyledir:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Burada t ikili verilerle değiştirilen &quot;.&quot; karakter. Aşağıdaki ekran Fiddler gösterir ham onaltılık değerler görüntüsü.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON HttpClient ile kullanma

.NET istemci uygulamaları ile BSON biçimlendirici kullanabilir **HttpClient**. Hakkında daha fazla bilgi için **HttpClient**, bkz: [bir Web API öğesinden bir .NET istemci çağırma](../advanced/calling-a-web-api-from-a-net-client.md).

Aşağıdaki kod BSON kabul edip yanıt BSON yükünde seri durumdan çıkarır GET isteği gönderir.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

BSON sunucudan istemek için "uygulama/bson" için Accept üstbilgisi ayarlayın:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Yanıt gövdesi seri durumdan çıkarılacak kullanmak **BsonMediaTypeFormatter**. Yanıt gövdesi okurken belirtmeniz gerekir böylece bu Biçimlendiricinin varsayılan biçimlendiricileri koleksiyonunda değil:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Sonraki örnek BSON içeren bir POST isteği göndermek nasıl gösterir.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Bu kodu çoğunu önceki örnekle aynıdır. Ancak **PostAsync** yöntemini belirtin **BsonMediaTypeFormatter** biçimlendirici olarak:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Üst düzey ilkel türler seri hale getirme

Anahtar/değer çiftlerinin listesini her BSON belgedir. BSON belirtimi tam sayı veya dize gibi tek bir ham değer serileştirmek için bir söz dizimi tanımlamıyor.

Bu sınırlamaya geçici bir çözüm için **BsonMediaTypeFormatter** ilkel türler özel bir durum olarak değerlendirir. Seri hale getirme önce bu değer bir anahtar/değer çifti "Value" anahtarla dönüştürür. Örneğin, API denetleyicisi bir tamsayı döndürür varsayın:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Seri hale getirme önce BSON biçimlendirici bu aşağıdaki anahtar/değer çifti dönüştürür:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Seri durumdan aktarıldığında, biçimlendirici verileri özgün değere dönüştürür. Ancak, web API ham değerler döndürürse farklı BSON ayrıştırıcı kullanan istemciler bu durumun üstesinden gerekir. Genel olarak, ham değerler yerine yapılandırılmış veri döndüren göz önünde bulundurmalısınız.

## <a name="additional-resources"></a>Ek Kaynaklar

[Web API BSON örneği](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Medya Biçimlendiricileri](media-formatters.md)
