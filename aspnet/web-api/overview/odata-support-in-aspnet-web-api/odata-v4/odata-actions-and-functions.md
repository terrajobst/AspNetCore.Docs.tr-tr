---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Eylemler ve OData v4 sürümünde işlevleri kullanarak ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: OData, eylemleri ve işlevleri varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yoludur. Bu öğreticide gösterilmiştir nasıl yapılır...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 45b84ec4ee76e83ece99bf6841c28e13c3ab7728
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795273"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Eylemler ve OData v4 sürümünde işlevleri kullanarak ASP.NET Web API 2.2
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> OData, eylemleri ve işlevleri varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yoludur. Bu öğreticide, Web API 2.2 kullanan bir OData v4 uç noktasına, eylemleri ve işlevleri eklemek gösterilir. Öğretici öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
> - Web API 2.2
> - OData v4
> - Visual Studio 2013 (Visual Studio 2017'yi indirin [burada](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Öğretici sürümleri
>
> OData sürüm 3 için bkz: [ASP.NET Web API 2'de OData eylemleri](../odata-v3/odata-actions.md).

Arasındaki fark *eylemleri* ve *işlevleri* Eylemler, yan etkileri olabilir ve İşlevler yapın. Hem Eylemler hem de işlevleri veri döndürebilir. Bazı eylemler kullanımları şunlardır:

- Karmaşık işlemleri.
- Çeşitli varlıklar aynı anda işleniyor.
- Güncelleştirmeleri yalnızca belirli özellikleri bir varlığın izin verme.
- Bir varlık değil veri gönderiliyor.

İşlevler, bir varlık veya koleksiyon için gelmediğinden bilgilerini döndürmek için yararlıdır.

Bir eylem (ya da işlevin) tek bir varlık veya koleksiyon hedefleyebilirsiniz. Bu OData terminolojisinde, *bağlama*. Bulundurabilirsiniz &quot;ilişkisiz&quot; hizmetteki işlemleri statik olarak adlandırılan Eylemler/İşlevler.

## <a name="example-adding-an-action"></a>Örnek: bir eylem ekleme

Şimdi bir ürün derecelendirmek için bir eylem tanımlayın.

> [!NOTE]
> Bu öğreticide öğreticiyi genişletip yapılar [OData v4 uç noktası kullanarak ASP.NET Web API 2 oluşturma](create-an-odata-v4-endpoint.md)


İlk olarak, ekleme bir `ProductRating` derecelendirmeleri temsil etmek için model.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Ayrıca bir **olan DB** için `ProductsContext` EF veritabanında bir derecelendirme tablo oluşturacaksınız böylece sınıf.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>EDM için eylem ekleme

WebApiConfig.cs içinde aşağıdaki kodu ekleyin:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

**EntityTypeConfiguration.Action** yöntemi, varlık veri modeli (EDM) için bir eylem ekler. **Parametre** yöntemi eylemi için belirtilmiş bir parametre belirtir.

Bu kod, ad alanı için EDM de ayarlar. Ad alanı URI eylem için eylem tam olarak nitelenmiş adını içerdiğinden önemlidir:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> Tipik bir IIS yapılandırması bu URL'de nokta 404 hatası döndürmek IIS neden olur. Aşağıdaki bölümü Web.Config dosyasına ekleyerek bunu çözebilirsiniz:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Bir denetleyici yöntemi için eylem ekleme

Etkinleştirmek için &quot;oranı&quot; eylemi, aşağıdaki yöntemi ekleyin `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Yöntem adı eylem adı eşleştiğine dikkat edin. **[HttpPost]** özniteliği belirtir yöntemi bir HTTP POST yöntemidir.

Bir eylemi çağırmak için istemci aşağıdaki gibi bir HTTP POST isteği gönderir:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

&quot;Oranı&quot; eylem URI eylem için varlık URI eklenmiş tam eylem adı, bu nedenle ürün örneklerine bağlı. (Biz EDM ad alanı ayarlama geri çağırma &quot;ProductService&quot;, tam olarak nitelenmiş eylem adı, bu nedenle &quot;ProductService.Rate&quot;.)

İstek gövdesi JSON yükü olarak eylem parametrelerini içerir. Web API'si, JSON yükü için otomatik olarak dönüştürür bir **ODataActionParameters** yalnızca parametre değerleri sözlüğü nesne. Bu sözlük, parametreleri, denetleyici yöntemi erişmek için kullanın.

İstemci yanlış eylem parametrelerini gönderirse biçimi, değerini **ModelState.IsValid** false'tur. Bu bayrak denetleyicisi yönteminizde denetleyin ve hata döndürür **IsValid** false'tur.

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Örnek: bir işlev ekleme

Şimdi en pahalı ürün döndüren bir OData işlevini ekleyelim. Önce ilk adımı için EDM işlevi ekleme olduğu gibi. WebApiConfig.cs içinde aşağıdaki kodu ekleyin.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

Bu durumda, işlev ürünleri koleksiyonu yerine tek tek ürün örneklerine bağlıdır. İstemciler bir GET isteği göndererek bu işlevi çağırın:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Bu işlev için denetleyici yöntemi aşağıda verilmiştir:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Yöntem adı işlev adı eşleştiğine dikkat edin. **[HttpGet]** özniteliği, yöntemdir bir HTTP GET yöntemini belirtir.

HTTP yanıtı aşağıda verilmiştir:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Örnek: ilişkisiz bir işlev ekleme

Önceki örnekte, bir koleksiyona bağlı bir işlev oluştu. Bu sonraki örnekte oluşturacağız bir *ilişkisiz* işlevi. İlişkisiz işlevleri statik hizmetteki işlemleri olarak adlandırılır. Bu örnekte işlevi verilen bir posta kodu için vergi döndürür.

WebApiConfig dosyasında için EDM işlevi ekleyin:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Çağrı yaptığınız dikkat edin **işlevi** doğrudan üzerinde **ODataModelBuilder**, varlık türünün veya koleksiyon yerine. Bu, model oluşturucu işlevi ilişkisiz olduğunu bildirir.

İşlev uygular denetleyici yöntemi aşağıda verilmiştir:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Bu yöntemde yerleştirdiğiniz hangi Web API denetleyicisi önemli değildir. İçine girdiğiniz `ProductsController`, veya ayrı denetleyicisinin tanımlayın. **[ODataRoute]** özniteliği, işlev için URI şablonu tanımlar.

Bir örnek istemci isteği şu şekildedir:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

HTTP yanıtı:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
