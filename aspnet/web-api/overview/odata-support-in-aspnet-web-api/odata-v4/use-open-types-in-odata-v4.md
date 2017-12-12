---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: "ASP.NET Web API ile OData v4 türleri Aç | Microsoft Docs"
author: microsoft
description: "OData v4 içinde bir açık tür tanımında bildirilen herhangi bir özellik yanı sıra Dinamik Özellikler içeren bir stuctured türü türüdür. Aç..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: c2d7454534ff0e9e0a80365793800ab7c45d3b6e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API ile OData v4 türleri Aç
====================
tarafından [Microsoft](https://github.com/microsoft)

> OData v4 içinde bir *açmak türü* tür tanımında bildirilen herhangi bir özellik yanı sıra Dinamik Özellikler içeren bir stuctured türüdür. Açık türleri, veri modelleri için esneklik eklemenize olanak sağlar. Bu öğretici ASP.NET Web API OData açık türleri kullanmayı gösterir.
> 
> Bu öğreticide, ASP.NET Web API'de OData uç noktası oluşturmak nasıl zaten bildiğinizi varsayar. Aksi takdirde, okuyarak başlamanız [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md) ilk.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API OData 5.3
> - OData v4


İlk olarak, bazı OData terminolojisi:

- Varlık türü: anahtar ile yapılandırılmış bir tür.
- Karmaşık tür: bir anahtar olmadan yapılandırılmış bir tür.
- Açık tür: dinamik özelliklere sahip bir tür. Varlık türleri ve karmaşık türler açık olabilir.

Dinamik özelliğinin değeri bir ilkel tür, karmaşık tür veya numaralandırma türü olabilir; veya bu türlerinden herhangi birini koleksiyonu. Açık türleri hakkında daha fazla bilgi için bkz: [OData v4 belirtimi](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Web OData kitaplıkları yükleme

NuGet paketi en son Web API OData kitaplıkları Yükleme Yöneticisi'ni kullanın. Paket Yöneticisi konsolu penceresinden:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>CLR türlerini tanımlayın

EDM modelleri CLR türleri olarak tanımlayarak başlatın.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Varlık veri modeli (EDM) oluşturulduğunda

- `Category`bir numaralandırma türü değil.
- `Address`karmaşık bir tür değil. (Dolayısıyla bir varlık türü, bir anahtar yok.)
- `Customer`bir varlık türüdür. (Bir anahtara sahip.)
- `Press`açık bir karmaşık türü değil.
- `Book`bir açık varlık türüdür.

Açık bir tür oluşturmak için CLR türü türünde bir özellik olması gerekir `IDictionary<string, object>`, dinamik özellikleri içerir.

## <a name="build-the-edm-model"></a>EDM modeli oluşturma

Kullanırsanız **ODataConventionModelBuilder** EDM oluşturmak için `Press` ve `Book` olmadığına göre açık türü olarak otomatik olarak eklenen bir `IDictionary<string, object>` özelliği.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Bir OData denetleyicisi ekleme

Ardından, bir OData denetleyicisi ekleyin. Bu öğretici için yalnızca destekler GET ve POST istekleri ve bir bellek içi listesi varlıkları depolamak için kullandığını basitleştirilmiş bir denetleyici kullanacağız.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Dikkat ilk `Book` örneğinin dinamik özellik vardır. İkinci `Book` örneği aşağıdaki dinamik özelliklere sahiptir:

- "Yayımlanan": basit türü
- "Yazar": ilkel türler koleksiyonu
- "OtherCategories": numaralandırma türleri koleksiyonu.

Ayrıca, `Press` özelliği, `Book` örneği aşağıdaki dinamik özelliklere sahiptir:

- "Blog": basit türü
- "Adres": karmaşık türü

## <a name="query-the-metadata"></a>Sorgu meta veriler

OData meta veri belgesi almak için bir GET isteği Gönder `~/$metadata`. Yanıt gövdesi şuna benzer görünmelidir:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Meta veri belgeden görebilirsiniz:

- İçin `Book` ve `Press` türleri, değeri `OpenType` özniteliği doğrudur. `Customer` Ve `Address` türleri, bu öznitelik zorunda kalmaz.
- `Book` Varlık türüne sahip üç bildirilmiş özellikleri: ISBN, başlık ve tuşuna basın. OData meta veri içermemesi `Book.Properties` CLR sınıfından özelliği.
- Benzer şekilde, `Press` karmaşık türü var. yalnızca iki bildirilmiş özellikleri: adı ve kategori. Meta veri değil içermemesi `Press.DynamicProperties` CLR sınıfından özelliği.

## <a name="query-an-entity"></a>Sorgu bir varlık

ISBN defteriyle "978-0-7356-7942-9 için" eşit almak için göndermek için bir GET isteği `~/Books('978-0-7356-7942-9')`. Yanıt gövdesi aşağıdakine benzer görünmelidir. (Daha okunabilir yapmak için girintili.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Dinamik özellikler dahil satır içi bildirilen özelliklere sahip olduğuna dikkat edin.

## <a name="post-an-entity"></a>Bir varlık sonrası

Kitap varlık eklemek için bir POST isteği Gönder `~/Books`. İstemci istek yükünde dinamik özellikleri ayarlayabilirsiniz.

Burada, örnek istek verilmiştir. "Price" ve "Yayımlanan" özellikleri unutmayın.

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Denetleyici yönteminde kesme noktası ayarlarsanız, Web API için bu özellikleri eklenen görebilirsiniz `Properties` sözlük.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Ek Kaynaklar

[OData açık türü örneği](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
