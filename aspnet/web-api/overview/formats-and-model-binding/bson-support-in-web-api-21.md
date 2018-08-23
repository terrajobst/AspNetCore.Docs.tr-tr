---
uid: web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
title: ASP.NET Web API 2.1 sürümünde BSON desteği | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: ce11b017-0ca6-4376-aa9d-a7f3288101de
msc.legacyurl: /web-api/overview/formats-and-model-binding/bson-support-in-web-api-21
msc.type: authoredcontent
ms.openlocfilehash: 709fb0266c0725176358a1bd0d08b3e07fa6e2a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756611"
---
<a name="bson-support-in-aspnet-web-api-21"></a>ASP.NET Web API 2.1 sürümünde BSON desteği
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Web API 2.1 BSON desteği sunar. Bu konuda, Web API denetleyicinizde (sunucu tarafı) ve bir .NET istemci uygulamasında BSON kullanmayı gösterir.

## <a name="what-is-bson"></a>BSON nedir?

[BSON](http://bsonspec.org/) bir ikili serileştirme biçimidir. "BSON", "İçin ikili JSON" anlamına gelir, ancak BSON ve JSON çok farklı serileştirilir. BSON olan "JSON benzeri", çünkü nesneleri ad-değer çiftleri, JSON ile benzer olarak temsil edilir. JSON, sayısal veri türleri değil dizeleri bayt depolanır

BSON, basit, kolay taranabilir ve hızlı kodlama/kod çözme için tasarlanmıştır.

- BSON, JSON'a boyutu karşılaştırılabilir. Verilere bağlı olarak bir BSON yükü daha küçük veya büyük bir JSON yükü olabilir. İkili verileri base64 kodlu olmadığı için bir görüntü dosyası gibi ikili verileri seri hale getirme için BSON JSON küçüktür.
- Bir Ayrıştırıcı kod çözme olmadan öğeleri atlayabilirsiniz öğeleri bir uzunluğu alanıyla önünde taramak BSON belgeleri kolaydır.
- Sayısal veri türleri olmayan dizeler numaraları olarak depolandığından kodlama ve kodunu çözme ve verimlidir.

.NET istemci uygulamaları gibi yerel istemci JSON veya XML gibi metin tabanlı biçimler yerine BSON kullanımından yararlanabilir. JavaScript JSON yükü doğrudan dönüştürebilirsiniz çünkü tarayıcı istemcileri için büyük olasılıkla JSON ile takılıyor isteyebilirsiniz.

Neyse ki, Web API'si kullanan [içerik anlaşması](content-negotiation.md), API'nizi hem biçimleri için destek ve seçin istemci olanak tanır.

## <a name="enabling-bson-on-the-server"></a>BSON sunucu üzerindeki etkinleştirme

Web API yapılandırmanızda ekleme **BsonMediaTypeFormatter** biçimlendiricileri koleksiyonu.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample1.cs)]

Artık Web API'sini istemci "application/bson" isterse, BSON biçimlendirici kullanır.

BSON diğer medya türleri ile ilişkilendirilecek SupportedMediaTypes koleksiyona ekleyin. Aşağıdaki kod, desteklenen medya türleri için "application/vnd.contoso" ekler:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample2.cs)]

## <a name="example-http-session"></a>Örnek HTTP oturumu

Bu örnekte, aşağıdaki model sınıfı artı basit bir Web API denetleyicisi kullanacağız:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample3.cs)]

Bir istemci, aşağıdaki HTTP isteği gönderebilir:

[!code-console[Main](bson-support-in-web-api-21/samples/sample4.cmd)]

Yanıt şu şekildedir:

[!code-console[Main](bson-support-in-web-api-21/samples/sample5.cmd)]

Ben ikili verileri burada yerine &quot;.&quot; karakter. Aşağıdaki ekranda Fiddler programlarla ham onaltılık değerleri görüntüsü.

[![](bson-support-in-web-api-21/_static/image2.png)](bson-support-in-web-api-21/_static/image1.png)

## <a name="using-bson-with-httpclient"></a>BSON HttpClient kullanma

.NET istemci uygulamaları, BSON biçimlendirici ile kullanabileceğiniz **HttpClient**. Hakkında daha fazla bilgi için **HttpClient**, bkz: [bir Web API'si bir .NET istemcinin çağırma](../advanced/calling-a-web-api-from-a-net-client.md).

Aşağıdaki kod, BSON kabul eder ve yanıt BSON yükü seri durumdan çıkarır bir GET isteği gönderir.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample6.cs)]

BSON sunucudan istek için Accept üst bilgisi için "application/bson" ayarlayın:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample7.cs)]

Yanıt gövdesi seri durumdan çıkarılacak kullanın **BsonMediaTypeFormatter**. Yanıt gövdesi okunurken, bunu belirtmeniz gerekmez, bu Biçimlendiricinin varsayılan biçimlendiricileri koleksiyonunda değildir:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample8.cs)]

Sonraki örnek, BSON içeren bir POST isteği göndermek nasıl gösterir.

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample9.cs)]

Bu kod çoğunu önceki örnekle aynıdır. Ancak **PostAsync** yöntemini belirtin **BsonMediaTypeFormatter** biçimlendirici olarak:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample10.cs)]

## <a name="serializing-top-level-primitive-types"></a>Üst düzey ilkel türler seri hale getirme

Her BSON belge, anahtar/değer çifti listesidir. BSON belirtimi, bir tamsayı veya dize gibi tek bir ham değerini serileştirmeye yönelik bir söz dizimi tanımlamıyor.

Bu sınırlara yakın çalışmak için **BsonMediaTypeFormatter** ilkel türler, özel bir durum olarak değerlendirir. Seri hale getirme önce değeri bir anahtar/değer çifti "Value" anahtarı ile dönüştürür. Örneğin, bir API denetleyicisi bir tamsayı döndürür varsayalım:

[!code-csharp[Main](bson-support-in-web-api-21/samples/sample11.cs)]

Seri hale getirme önce BSON biçimlendirici bu aşağıdaki anahtar/değer çifti dönüştürür:

[!code-json[Main](bson-support-in-web-api-21/samples/sample12.json)]

Seri durumdan, biçimlendirici verileri özgün değerine dönüştürür. Ancak, web API'NİZİN ham değer döndürürse bu durumu işlemek farklı bir BSON Ayrıştırıcıyı kullanarak istemcileri gerekir. Genel olarak, ham değerler yerine, yapılandırılmış veriler döndüren düşünmelisiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Web API BSON örneği](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/BSONSample/)

[Medya Biçimlendiricileri](media-formatters.md)
