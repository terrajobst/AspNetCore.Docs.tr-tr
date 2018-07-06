---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: ASP.NET Web API ile OData v4 sürümünde açık türler | Microsoft Docs
author: microsoft
description: OData v4 sürümünde açık bir tür ek tür tanımında bildirilen herhangi bir özelliği olarak dinamik özellikleri içeren bir stuctured türüdür. Aç...
ms.author: aspnetcontent
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 560d47e0dc451847311eb9e2327190eed2209546
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832280"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>ASP.NET Web API ile OData v4 sürümünde türleri açın
====================
tarafından [Microsoft](https://github.com/microsoft)

> OData v4 sürümünde bir *açık tür* ek tür tanımında bildirilen herhangi bir özelliği olarak dinamik özellikleri içeren bir stuctured türüdür. Açık türler, veri modelleri için esneklik eklemenize olanak sağlar. Bu öğreticide, ASP.NET Web API OData açık türler kullanmayı gösterir.
> 
> Bu öğreticide, ASP.NET Web API OData uç noktası oluşturma bildiğiniz varsayılır. Aksi takdirde, okuyarak başlamanız [bir OData v4 uç noktası oluşturma](create-an-odata-v4-endpoint.md) ilk.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API OData 5.3
> - OData v4


İlk olarak, bazı OData terimler:

- Varlık türü: bir anahtar ile yapılandırılmış bir tür.
- Karmaşık tür: anahtar olmadan yapılandırılmış bir tür.
- Açık türü: dinamik özelliklere sahip bir tür. Hem varlık türlerinde ve karmaşık türler, açık olabilir.

Dinamik özelliğinin değeri, bir basit tür, karmaşık tür veya numaralandırma türü olabilir; veya bu türlerinden herhangi birinin bir koleksiyon. Açık türler hakkında daha fazla bilgi için bkz: [OData v4 belirtimi](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Web OData kitaplıklarını yükleme

NuGet paketi en son Web API OData kitaplıklarını yüklemek için Yöneticisi'ni kullanın. Paket Yöneticisi konsol penceresinden:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>CLR türlerini tanımlayın

EDM modeli CLR türleri olarak tanımlayarak başlatın.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Varlık veri modeli (EDM) oluşturulduğunda

- `Category` bir sabit listesi türüdür.
- `Address` karmaşık bir türdür. (Bir varlık türü, yani, bir anahtar yok.)
- `Customer` bir varlık türüdür. (Bir anahtarı yok.)
- `Press` bir açık karmaşık bir türdür.
- `Book` bir açık varlık türüdür.

Açık bir tür oluşturmak için CLR türü türünün bir özelliği olmalıdır `IDictionary<string, object>`, dinamik özelliklerini içerir.

## <a name="build-the-edm-model"></a>EDM modeli oluşturun

Kullanırsanız **ODataConventionModelBuilder** EDM oluşturmak için `Press` ve `Book` varlığını temel alarak açık türler olarak otomatik olarak eklenen bir `IDictionary<string, object>` özelliği.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

Ayrıca EDM açıkça kullanarak oluşturabilirsiniz **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Bir OData denetleyicisi Ekle

Ardından, bir OData denetleyicisi ekleyin. Bu öğretici için yalnızca destekler alma POST istekleri ve varlıkları depolamak için bir bellek içi listesini kullanır, Basitleştirilmiş bir denetleyici kullanacağız.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Dikkat ilk `Book` örneğinin dinamik özellik vardır. İkinci `Book` örneği aşağıdaki dinamik özelliklere sahiptir:

- "Yayımlanmış": temel tür
- "Yazar": ilkel türler koleksiyonu
- "OtherCategories": numaralandırma türleri koleksiyonu.

Ayrıca, `Press` , söz konusu özellik `Book` örneği aşağıdaki dinamik özelliklere sahiptir:

- "Blog": temel tür
- "Address": karmaşık tür

## <a name="query-the-metadata"></a>Meta verileri Sorgulama

OData meta veri belgesi almak için GET isteğini göndermek `~/$metadata`. Yanıt gövdesi, şuna benzer görünmelidir:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Meta veri belgeden görebilirsiniz:

- İçin `Book` ve `Press` türleri, değerini `OpenType` öznitelik değeri true. `Customer` Ve `Address` türleri bu özniteliği yok.
- `Book` Varlık türünde üç bildirilmiş özellikleri: ISBN, başlık ve tuşuna basın. OData meta veri içermemesi `Book.Properties` CLR sınıftaki özellik.
- Benzer şekilde, `Press` karmaşık tür yalnızca iki bildirilen özelliklere sahiptir: adı ve kategori. Meta veriler içermemesi `Press.DynamicProperties` CLR sınıftaki özellik.

## <a name="query-an-entity"></a>Sorgu bir varlık

ISBN defteriyle "978-0-7356-7942-9" eşit almak için GET isteğini göndermek `~/Books('978-0-7356-7942-9')`. Yanıt gövdesi, aşağıdakine benzer görünmelidir. (Daha okunabilir yapmak için girintili.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Dinamik özellikler satır içi bildirilen özelliklere sahip olduğuna dikkat edin.

## <a name="post-an-entity"></a>Bir varlık gönderin

Bir kitap varlığı eklemek için bir POST isteği gönderin `~/Books`. İstemci istek yükünde dinamik özellikleri ayarlayabilirsiniz.

İşte bir örnek istek. "Price" ve "Yayımlanmış" özellikleri unutmayın.

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Denetleyici yöntemde bir kesme noktası ayarlarsanız, Web API'si için bu özellikleri eklendi görebilirsiniz `Properties` sözlüğü.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Ek Kaynaklar

[OData açık tür örneği](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
