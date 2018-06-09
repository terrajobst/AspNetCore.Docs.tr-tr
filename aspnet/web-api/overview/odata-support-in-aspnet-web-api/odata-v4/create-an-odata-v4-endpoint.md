---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma | Microsoft Docs
author: MikeWasson
description: Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData sorgu ve veri kümelerini CRUD işlemleri üzerinden işlemek için Tekdüzen bir yol sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566721"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>ASP.NET Web API 2.2 kullanarak bir OData v4 uç noktası oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

> Açık Veri Protokolü (OData), web için veri erişim protokolüdür. OData sorgu ve veri kümelerini CRUD işlemleri üzerinden işlemek için Tekdüzen bir yöntem sunar (oluşturma, okuma, güncelleştirme ve silme).
> 
> ASP.NET Web API hem v3 hem de v4 protokolü destekler. Yan yana çalışır bir v4 uç noktası bile olabilir v3 uç noktası ile.
> 
> Bu öğretici CRUD işlemleri destekleyen bir OData v4 uç nokta oluşturulacağını gösterir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2.2
> - OData v4
> - [Visual Studio 2013 Güncelleştirme 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Eğitmen sürümleri
> 
> OData sürüm 3 için bkz: [bir OData v3 uç noktası oluşturulurken](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Visual Studio projesi oluşturma

Visual Studio'da gelen **dosya** menüsünde, select **yeni** &gt; **proje**.

Genişletme **yüklü** &gt; **şablonları** &gt; **Visual C#** &gt; **Web**, seçin **ASP.NET Web uygulaması** şablonu. Proje adı &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

İçinde **yeni proje** iletişim kutusunda **boş** şablonu. Altında &quot;klasörler ekleme ve çekirdek başvuruları... &quot;, tıklatın **Web API**. **Tamam**'ı tıklatın.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>OData paket yüklemek için

Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde yazın:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Bu komut, en son OData NuGet paketlerini yükler.

## <a name="add-a-model-class"></a>Bir Model sınıfı ekleme

A *modeli* uygulamanızda veri varlığı temsil eden bir nesnedir.

Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Bağlam menüsünden seçin **Ekle** &gt; **sınıfı**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Kuralı, modeli sınıfları modelleri klasörüne yerleştirilir; ancak bu kural, kendi projelerine izleyin gerekmez.


Sınıf adını `Product`. Product.cs dosyasındaki Demirbaş kod aşağıdakiyle değiştirin:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Varlık anahtarı bir özelliktir. İstemciler tarafından anahtar varlıklar sorgulayabilir. Örneğin, 5 kimlikli ürün almak için URI. `/Products(5)`. `Id` Özelliği de arka uç veritabanı birincil anahtarı olmalıdır.

## <a name="enable-entity-framework"></a>Entity Framework etkinleştir

Bu öğretici için Entity Framework (EF) Code First arka uç veritabanı oluşturmak için kullanacağız.

> [!NOTE]
> Web API OData EF gerektirmez. Veritabanı varlıklar modellerini çevirebilir tüm veri erişim katmanı kullanın.


İlk olarak, EF için NuGet paketini yükleyin. Gelen **Araçları** menüsünde, select **NuGet Paket Yöneticisi** &gt; **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde yazın:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Web.config dosyasını açın ve aşağıdaki bölümün içine ekleyin **yapılandırma** öğesi, sonra **configSections** öğesi.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Bu ayar bir LocalDB veritabanına bir bağlantı dizesi ekler. Uygulamayı yerel olarak çalıştırdığınızda bu veritabanı kullanılır.

Ardından, adlı bir sınıf ekleyin `ProductsContext` modeller klasörü için:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

Oluşturucu, `"name=ProductsContext"` bağlantı dizesinin adını verir.

## <a name="configure-the-odata-endpoint"></a>OData uç noktasını yapılandırma

Uygulama dosyasını açın\_Start/WebApiConfig.cs. Aşağıdakileri ekleyin **kullanarak** deyimleri:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Ardından aşağıdaki kodu ekleyin **kaydetmek** yöntemi:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Bu kod iki işlemi yapar:

- Bir varlık veri modeli (EDM) oluşturur.
- Bir yol ekler.

Bir EDM verilerin soyut bir modelidir. EDM hizmeti meta veri belgesi oluşturmak için kullanılır. **ODataConventionModelBuilder** sınıfı, varsayılan adlandırma kurallarını kullanarak bir EDM oluşturur. Bu yaklaşım, en az kod gerektirir. EDM üzerinde daha fazla denetim isterseniz, kullanabileceğiniz **ODataModelBuilder** açıkça özellikleri, anahtarları ve gezinti özellikleri ekleyerek EDM oluşturmasını sağlar.

A *rota* Web API uç noktasına HTTP istekleri yönlendirmek anlatır. Bir OData v4 rota oluşturmak için arama **MapODataServiceRoute** genişletme yöntemi.

Uygulamanız birden çok OData uç noktaları varsa, her biri için ayrı bir yol oluşturun. Her yol bir benzersiz rota adı ve önek verin.

## <a name="add-the-odata-controller"></a>OData denetleyicisi ekleme

A *denetleyicisi* HTTP isteklerini işleyen sınıftır. OData hizmetinizi her varlık için ayrı bir denetleyicisi oluşturun. Bu öğreticide, bir denetleyici için oluşturacağınız `Product` varlık.

Çözüm Gezgini'nde denetleyicileri klasörünü sağ tıklatın ve seçin **Ekle** &gt; **sınıfı**. Sınıf adını `ProductsController`.

> [!NOTE]
> Bu öğretici için OData v3 kullanan sürümü **denetleyici Ekle** yapı iskelesi. Şu anda, OData v4 için hiçbir askılamayı yoktur.


ProductsController.cs Demirbaş kod aşağıdakiyle değiştirin.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Denetleyici kullandığı `ProductsContext` EF kullanan veritabanına erişmek için sınıf. Denetleyici geçersiz kılmaları bildirimi **Dispose** elden yöntemi **ProductsContext**.

Bu denetleyici için başlangıç noktasıdır. Ardından, tüm CRUD işlemleri için yöntemleri ekleyeceğiz.

## <a name="querying-the-entity-set"></a>Varlık kümesini sorgulama

Aşağıdaki yöntemi ekleyin `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Parametresiz sürümü `Get` yöntemi tüm ürünleri koleksiyon döndürür. `Get` Yöntemi ile bir *anahtar* parametresi anahtara göre bir ürün görünüyor (Bu durumda, `Id` özelliği).

**[EnableQuery]** özniteliği $filter, $sort ve $page gibi sorgu seçeneklerini kullanarak sorguyu değiştirmek istemcileri etkinleştirir. Daha fazla bilgi için bkz: [destekleyen OData sorgu seçeneklerini](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Varlık kümesine varlık ekleme

Yeni bir ürün veritabanına eklemek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Bir varlık güncelleştiriliyor

OData düzeltme eki ve PUT bir varlığı güncelleştirmek için iki farklı semantiği destekler.

- Düzeltme eki kısmi güncelleştirme gerçekleştirir. İstemci güncelleştirmek için yalnızca özellikleri belirtir.
- PUT tüm varlığı değiştirir.

PUT dezavantajı, istemci varlıkta değil değiştirecekseniz değerler dahil olmak üzere, tüm özellikler için değerleri göndermelidir olmasıdır. [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) düzeltme eki tercih edilen olduğunu belirtir.

Herhangi bir durumda, düzeltme eki ve PUT yöntemleri için kod aşağıdadır:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

Düzeltme eki söz konusu olduğunda, denetleyici kullanan **Delta&lt;T&gt;**  değişiklikleri izlemek için türü.

## <a name="deleting-an-entity"></a>Bir varlığı silme

Bir ürün veritabanından silmek istemcileri etkinleştirmek için aşağıdaki yöntemi ekleyin `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
