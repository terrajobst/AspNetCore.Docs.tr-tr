---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData sorgu ve veri kümeleri aracılığıyla CRUD işlemleri işlemek için Tekdüzen bir yol sağlar...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 48c1a78c96cb0ebfa0b053dfef84e76433112650
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795424"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

> Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData sorgu ve veri kümeleri aracılığıyla CRUD işlemleri işlemek için Tekdüzen bir yol sağlar (oluşturma, okuma, güncelleştirme ve silme).
>
> ASP.NET Web API, hem v3 hem de v4 protokolü destekler. Yan yana çalışan bir v4 uç noktası bile olabilir v3 uç noktası ile.
>
> Bu öğreticide, CRUD işlemleri destekleyen OData v4 uç noktası oluşturma işlemi gösterilmektedir.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
>
> - Web API 2.2
> - OData v4
> - Visual Studio 2013 (Visual Studio 2017'yi indirin [burada](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))
> - Entity Framework 6
> - .NET 4.5
>
> ## <a name="tutorial-versions"></a>Öğretici sürümleri
>
> OData sürüm 3 için bkz: [OData v3 uç noktası oluşturma](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'da gelen **dosya** menüsünde **yeni** &gt; **proje**.

Genişletin **yüklü** &gt; **şablonları** &gt; **Visual C#** &gt; **Web**, seçin **ASP.NET Web uygulaması** şablonu. Projeyi adlandırın &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

İçinde **yeni proje** iletişim kutusunda **boş** şablonu. Altında &quot;klasörleri ekleyin ve çekirdek başvuruları... &quot;, tıklayın **Web API**. **Tamam**'ı tıklatın.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>OData paketleri yükleme

Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde yazın:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Bu komut, en son OData NuGet paketlerini yükler.

## <a name="add-a-model-class"></a>Bir Model sınıfı ekleme

A *modeli* uygulamanızda bir veri varlığı temsil eden bir nesnedir.

Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** &gt; **sınıfı**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Kural olarak, model sınıfları modelleri klasörüne yerleştirilir ancak kendi projelerinizde bu kurala uymayan gerekmez.


Sınıf adını `Product`. Product.cs dosyasındaki ortak kod aşağıdakiyle değiştirin:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Özelliği Varlık anahtarı. İstemciler varlıkları anahtara göre sorgulayabilir. Örneğin, 5 Kimliğine sahip bir ürün almak için URI değil `/Products(5)`. `Id` Özelliği birincil anahtar arka uç veritabanı olarak da olacaktır.

## <a name="enable-entity-framework"></a>Entity Framework etkinleştir

Bu öğreticide, Entity Framework (EF) Code First arka uç veritabanı oluşturmak için kullanacağız.

> [!NOTE]
> Web API OData EF gerektirmez. Veritabanı varlıklarını modellerini çevirebilir herhangi bir veri erişim katmanı'nı kullanın.


İlk olarak EF için NuGet paketini yükleyin. Gelen **Araçları** menüsünde **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde yazın:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Web.config dosyasını açın ve içine aşağıdaki bölümü ekleyin **yapılandırma** öğesi, sonra **configSections** öğesi.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Bu ayar için bir LocalDB veritabanına bir bağlantı dizesi ekler. Bu veritabanı, uygulamayı yerel olarak çalıştırdığınızda kullanılır.

Ardından, adında bir sınıf ekleyin `ProductsContext` modeller klasörü için:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Oluşturucuya `"name=ProductsContext"` bağlantı dizesinin adını verir.

## <a name="configure-the-odata-endpoint"></a>OData uç noktasını yapılandırma

Uygulama dosyasını açın\_Start/WebApiConfig.cs. Aşağıdaki **kullanarak** ifadeleri:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Ardından aşağıdaki kodu ekleyin **kaydetme** yöntemi:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Bu kod, iki şey yapar:

- Bir varlık veri modeli (EDM) oluşturur.
- Bir rota ekler.

Bir EDM soyut bir veri modelidir. EDM hizmet meta verileri belgesi oluşturmak için kullanılır. **ODataConventionModelBuilder** sınıf varsayılan adlandırma kurallarını kullanarak bir EDM oluşturur. Bu yaklaşım, en az kod gerektirir. EDM üzerinde daha fazla denetim istiyorsanız kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmak için sınıf.

A *rota* Web API uç noktasına HTTP istekleri yönlendirme anlatır. Bir OData v4 rota oluşturmak için arama **MapODataServiceRoute** genişletme yöntemi.

Uygulamanızı birden çok OData uç noktaları varsa, her biri için ayrı bir yol oluşturun. Her yol, bir benzersiz rota adı ve ön eki ekleyin.

## <a name="add-the-odata-controller"></a>OData denetleyicisi Ekle

A *denetleyicisi* HTTP isteklerini işleyen sınıftır. OData hizmetinizi her varlık için bir ayrı denetleyicisinin oluşturduğunuz. Bu öğreticide, bir denetleyici için oluşturacağınız `Product` varlık.

Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın ve seçin **Ekle** &gt; **sınıfı**. Sınıf adını `ProductsController`.

> [!NOTE]
> Bu öğretici için OData v3 kullanan sürümünü **denetleyici Ekle** yapı iskelesi. Şu anda hiçbir yapı iskelesi OData v4 için yoktur.


ProductsController.cs Demirbaş kodu aşağıdakiyle değiştirin.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Denetleyici kullandığı `ProductsContext` EF kullanarak veritabanına erişmek için sınıf. Denetleyici geçersiz kılan bildirimi **Dispose** yöntemi elden çıkarmak **ProductsContext**.

Bu denetleyici için başlangıç noktasıdır. Ardından, tüm CRUD işlemleri için yöntemler ekleyeceğiz.

## <a name="querying-the-entity-set"></a>Varlık kümesi sorgulama

Aşağıdaki yöntemi ekleyin `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Parametresiz sürümünü `Get` yöntemi tüm ürünleri koleksiyonu döndürür. `Get` Yöntemi ile bir *anahtarı* parametresi anahtara göre bir ürün görünür (Bu durumda, `Id` özelliği).

**[EnableQuery]** özniteliği $filter $sort ve $page gibi sorgu Seçenekleri'ni kullanarak sorgu değiştirmek istemcileri etkinleştirir. Daha fazla bilgi için [OData sorgu seçeneklerini destekleme](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Varlık kümesine varlık ekleme

Yeni ürün eklemek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin. `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Bir varlığı güncelleştirmek

OData, düzeltme eki ve PUT bir varlığı güncelleştirmek için iki farklı semantikler destekler.

- Düzeltme eki kısmi bir güncelleştirmesini yapar. İstemci, yalnızca güncelleştirilecek özelliklerini belirtir.
- Tüm varlık PUT değiştirir.

PUT bir dezavantajı, istemci varlıkta değil değiştirme değerleri dahil olmak üzere tüm özellikler için değerleri göndermelisiniz olmasıdır. [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) düzeltme eki tercih edilen olduğunu belirtir.

Herhangi bir durumda, hem düzeltme eki hem de PUT yöntemleri için kod şu şekildedir:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Düzeltme eki söz konusu olduğunda denetleyicisi kullanan **Delta&lt;T&gt;**  değişiklikleri izlemek için türü.

## <a name="deleting-an-entity"></a>Bir varlığı silme

Bir ürün veritabanından silmek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin. `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
