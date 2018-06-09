---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Eylemler ve OData v4 işlevleri kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: OData, eylemleri ve işlevleri kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bu öğreticide gösterilmiştir nasıl...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566775"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Eylemleri ve işlevleri OData v4 ASP.NET Web API 2.2 kullanma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> OData, eylemleri ve işlevleri kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bu öğretici eylemleri ve işlevleri Web API 2.2 kullanarak bir OData v4 uç noktaya nasıl ekleneceğini gösterir. Öğretici öğreticiyi derlemeler [OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> OData sürüm 3 için bkz: [ASP.NET Web API 2 OData Eylemler](../odata-v3/odata-actions.md).


Arasındaki farkı *Eylemler* ve *işlevleri* Eylemler yan etkileri olabilir ve işlevleri yapın. Eylemleri ve işlevleri veri döndürebilir. Bazı eylemler kullanımları şunları içerir:

- Karmaşık işlemleri.
- Aynı anda birden fazla varlıkların düzenleme.
- Bir varlığın belirli özellikleri için yalnızca güncelleştirmelere izin verme.
- Bir varlık olmayan veriler gönderme.

İşlevler, doğrudan bir varlık veya koleksiyonuna karşılık gelmediğinden bilgileri döndürmek için faydalıdır.

Bir eylem (veya işlevi) tek bir varlık veya bir koleksiyon hedefleyebilirsiniz. OData terminolojisinde budur *bağlama*. Ayrıca olabilir &quot;ilişkisiz&quot; hizmetteki statik işlemleri olarak adlandırılır Eylemler/İşlevler,.

## <a name="example-adding-an-action"></a>Örnek: bir eylem ekleme

Şimdi bir ürün derecelendirmek için bir eylem tanımlayın.

> [!NOTE]
> Bu öğretici öğreticiyi derlemeler [OData v4 uç nokta kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)


İlk olarak, ekleyin bir `ProductRating` derecelendirmeleri temsil etmek için model.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Ayrıca bir **DbSet** için `ProductsContext` sınıf böylece EF veritabanında bir derecelendirme tablo oluşturur.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Eylem için EDM ekleme

WebApiConfig.cs içinde aşağıdaki kodu ekleyin:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** yöntemi, varlık veri modeli (EDM) için bir eylem ekler. **Parametresi** yöntemi eylemi için belirtilmiş bir parametre belirtir.

Bu kod, ad alanı için EDM da ayarlar. Ad alanı URI eylem için eylem tam adı içerdiğinden önemlidir:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Tipik bir IIS yapılandırması bu URL'de nokta Hata 404 döndürmek üzere IIS neden olur. Bunu, aşağıdaki bölümü Web.Config dosyasına ekleyerek çözümleyebilirsiniz:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Eylem için bir denetleyici yöntemi ekleme

Etkinleştirmek için &quot;oranı&quot; eylem, aşağıdaki yöntemi ekleyin `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Yöntem adı eylem adı ile eşleşen dikkat edin. **[HttpPost]** özniteliği, yöntemdir bir HTTP POST yöntemini belirtir.

Eylemi çağırmak için istemci aşağıdaki gibi bir HTTP POST isteği gönderir:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Oranı&quot; eylem eylemi için URI URI varlığa eklenen tam eylem adı olacak şekilde ürün örneklerine bağlı. (Biz EDM ad alanı ayarlamak geri çağırma &quot;ProductService&quot;tam eylem adı gelir &quot;ProductService.Rate&quot;.)

İstek gövdesini bir JSON yükü olarak eylem parametrelerini içerir. Web API otomatik olarak dönüştürür JSON yükü bir **ODataActionParameters** yalnızca parametre değerleri sözlüğü nesne. Denetleyici yönteminin parametrelerinde erişmek için bu sözlük kullanın.

İstemcisi yanlış eylem parametrelerini gönderirse biçimi, değeri **ModelState.IsValid** false olur. Bu bayrak, denetleyici yönteminde denetleyin ve hata döndürür **IsValid** false olur.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Örnek: işlev ekleme

Şimdi en pahalı ürün döndüren bir OData işlevini ekleyelim. Önce ilk adım işlevi için EDM ekleme gibi. WebApiConfig.cs içinde aşağıdaki kodu ekleyin.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Bu durumda, işlevi ürünleri koleksiyon yerine tek tek ürün örnekleri bağlıdır. İstemciler, bir GET isteği göndererek işlevi çağırır:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Bu işlev için denetleyici yöntemi şöyledir:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Yöntem adı işlevi adıyla eşleşen dikkat edin. **[HttpGet]** özniteliği, yöntemdir bir HTTP GET yöntemi belirtir.

HTTP yanıtı şöyledir:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Örnek: ilişkisiz bir işlev ekleme

Önceki örnekte, bir koleksiyona bağlı bir işlev oluştu. Bu sonraki örnekte oluşturacağız bir *ilişkisiz* işlevi. İlişkisiz işlevleri hizmetteki statik işlemleri olarak adlandırılır. Bu örnekte işlevi verilen bir posta kodu için satış vergi döndürür.

WebApiConfig dosyasında EDM işlevi ekleyin:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Numaralı telefonu bildirimi **işlevi** doğrudan üzerinde **ODataModelBuilder**, varlık türü veya koleksiyon yerine. Bu model oluşturucu işlevi ilişkisiz olduğunu söyler.

İşlev uygulayan denetleyici yöntemi şöyledir:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Bu yöntemde yerleştirin hangi Web API denetleyicisi önemli değildir. Bunu içine `ProductsController`, veya ayrı bir denetleyici tanımlayın. **[ODataRoute]** özniteliği işlevi için URI şablonunu tanımlar.

Bir örnek istemci isteği şöyledir:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP yanıtı:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
