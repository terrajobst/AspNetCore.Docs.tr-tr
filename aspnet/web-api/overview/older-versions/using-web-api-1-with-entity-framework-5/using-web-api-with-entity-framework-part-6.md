---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: "Bölüm 6: Ürün ve sipariş denetleyicileri oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 3b33543f02479b97112a63eb3879967ae31ccfb3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="part-6-creating-product-and-order-controllers"></a>Bölüm 6: Ürün oluşturma ve sipariş denetleyicileri
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Ürünler denetleyici ekleme

Yönetim Denetleyicisi yönetici ayrıcalıklarına sahip kullanıcılar için değil. Müşteriler, diğer yandan, ürünleri görüntüleyebilir ancak olamaz oluşturmak, güncelleştirmek veya silebilirsiniz.

Biz kolay erişim, Get yöntemleri açık bırakarak Post, Put ve silme yöntemleri kısıtlayabilirsiniz. Ancak, bir ürün döndürülen verileri bakın:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Özelliği müşterilerin görebildiği olmalıdır! Çözüm tanımlamaktır bir *veri aktarım nesnesini* (DTO) içeren bir alt kümesini müşterilere görünmesi gereken özellikleri. LINQ projeye kullanacağız `Product` için örnekleri `ProductDTO` örnekleri.

Adlı bir sınıf ekleyin `ProductDTO` modeller klasörü için.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Şimdi denetleyici ekleyin. Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**. İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;. Altında **şablonu**seçin **boş API denetleyicisi**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Kaynak dosyasında her şeyi aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Denetleyici hala kullanan `OrdersContext` veritabanını sorgulamak için. Ancak döndürme yerine `Product` örnekleri doğrudan diyoruz `MapProducts` açtığına projeye `ProductDTO` örnekleri:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Yöntemi döndürür bir **Iqueryable**, biz sonucu diğer sorgu parametreleri ile oluşturabilirsiniz. Bu konuda bkz `GetProduct` ekler yöntemi bir **burada** sorgu yan tümcesini:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Siparişleri denetleyici ekleyin

Ardından, oluşturma ve siparişleri görüntüleme kullanıcıların olanak sağlayan bir denetleyici ekleyin.

Başka bir DTO ile başlayacağız. Çözüm Gezgini'nde, modeller klasörü sağ tıklatın ve adlı bir sınıf ekleyin `OrderDTO` aşağıdaki uygulama kullanın:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Şimdi denetleyici ekleyin. Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**. İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdaki seçenekleri belirleyin:

- Altında **Denetleyici adı**, "OrdersController" girin.
- Altında **şablonu**seçin "Entity Framework kullanarak okuma/yazma eylemlerine sahip API denetleyicisi".
- Altında **Model sınıfı**seçin &quot;sipariş (ProductStore.Models)&quot;.
- Altında **veri bağlamı sınıfı**seçin &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

**Ekle**'yi tıklatın. Bu OrdersController.cs adlı bir dosya ekler. Ardından, biz denetleyicisi varsayılan uygulaması değiştirmeniz gerekir.

İlk olarak, Sil `PutOrder` ve `DeleteOrder` yöntemleri. Bu örnek, müşterilere değiştiremez veya varolan siparişleri Sil. Gerçek bir uygulamada bu gibi durumlarda arka uç mantığı çok sayıda gerekir. (Örneğin, sipariş zaten sevk edilen?)

Değişiklik `GetOrders` yöntemi yalnızca bir kullanıcıya ait siparişleri döndürmek için:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Değişiklik `GetOrder` yöntemini aşağıdaki şekilde:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Biz yönteme yapılan değişiklikler şunlardır:

- Dönüş değeri bir `OrderDTO` örneği yerine bir `Order`.
- Biz sipariş için veritabanı sorguladığınızda kullanırız [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) ilgili getirilemedi yöntemi `OrderDetail` ve `Product` varlıklar.
- Biz, sonuç bir yansıtma kullanarak düzleştirmek.

HTTP yanıtı miktarları ürünleriyle dizisi içerir:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Bu biçim, iç içe geçmiş varlıkları (sipariş, Ayrıntılar ve ürünleri) içeren özgün nesne grafiği, kullanmak istemcileri için kolaydır.

Bu dikkate alınması gereken son yöntemi `PostOrder`. Şu anda, bu yöntem alır bir `Order` örneği. Ancak ne olacağını düşünün bir istemci şöyle bir istek gövdesi gönderirse:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Bu iyi yapılandırılmış bir sipariş ve Entity Framework sonsuza dek bu veritabanına ekler. Ancak önceden var olmayan bir ürün varlık içeriyor. Yalnızca yeni bir ürün bizim veritabanında oluşturulan istemci! Koala bears için bir sıra gördüğünüzde Bu sipariş fullfilment departmanına beklenmedik biçimde olacaktır. Ahlaki, gerçekten bir POST veya PUT isteği kabul veriler hakkında dikkat edin.

Bu sorunu önlemek için değiştirme `PostOrder` yapılacak yöntemi bir `OrderDTO` örneği. Kullanım `OrderDTO` oluşturmak için `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Kullanırız bildirimi `ProductID` ve `Quantity` özellikleri ve biz yoksay ürün adı veya fiyat için istemciye gönderilen herhangi bir değeri. Ürün Kimliği geçerli değilse, veritabanındaki yabancı anahtar kısıtlamasını ihlal ve gerektiği gibi ekleme başarısız olur.

İşte tam `PostOrder` yöntemi:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Son olarak, ekleme **Authorize** özniteliği denetleyiciye:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Artık yalnızca kayıtlı kullanıcıların oluşturun veya siparişleri görüntüleyin.

>[!div class="step-by-step"]
[Önceki](using-web-api-with-entity-framework-part-5.md)
[sonraki](using-web-api-with-entity-framework-part-7.md)
