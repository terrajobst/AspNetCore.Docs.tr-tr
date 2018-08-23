---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: '6. Bölüm: Ürün ve sipariş denetleyicileri oluşturma | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752964"
---
<a name="part-6-creating-product-and-order-controllers"></a>6. Bölüm: Ürün oluşturma ve sipariş denetleyicileri
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Ürünleri denetleyici ekleme

Yönetici, yönetici ayrıcalıklarına sahip kullanıcılar için denetleyicisidir. Müşteriler, diğer taraftan, ürünleri görüntülemek ancak oluşturulamıyor, güncelleştirme veya silebilirsiniz.

Biz kolayca erişim, Get yöntemleri açık bırakarak Post, Put ve Delete yöntemler ile kısıtlayabilirsiniz. Ancak, bir ürün için döndürülen veri bakın:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

`ActualCost` Özelliği müşterilere görünebilir olmamalıdır! Çözüm tanımlamaktır bir *veri aktarımı nesnesi* (DTO) bir alt kümesini müşterilere görünür olması gereken özellikleri içeren. LINQ projenize kullanacağız `Product` için örnekler `ProductDTO` örnekleri.

Adlı bir sınıf ekleyin `ProductDTO` modeller klasörü için.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Şimdi denetleyici ekleyin. Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **Ekle**, ardından **denetleyicisi**. İçinde **denetleyici Ekle** iletişim kutusunda, denetleyici adı &quot;ProductsController&quot;. Altında **şablon**seçin **boş API denetleyicisi**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Kaynak dosyadaki her şeyi, aşağıdaki kodla değiştirin:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Denetleyici hala kullanıyor `OrdersContext` veritabanını sorgulamak için. Ancak döndürmek yerine `Product` örnekleri doğrudan diyoruz `MapProducts` açtığına projeye `ProductDTO` örnekleri:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

`MapProducts` Yöntemi döndürür bir **Iqueryable**, biz sonucu diğer sorgu parametreleri ile oluşturabilirsiniz. Bu konuda bkz `GetProduct` ekleyen yöntemi bir **burada** sorgu yan tümcesinin:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Orders denetleyicisinin Ekle

Ardından, kullanıcıların siparişlerini görüntülemek ve oluşturmalarına olanak tanıyan bir denetleyici ekleyeceksiniz.

Başka bir DTO ile başlayacağız. Çözüm Gezgini'nde, modeller klasörü sağ tıklatın ve adlı bir sınıf ekleyin `OrderDTO` aşağıdaki uygulama kullanın:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Şimdi denetleyici ekleyin. Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **Ekle**, ardından **denetleyicisi**. İçinde **denetleyici Ekle** iletişim kutusunda aşağıdaki seçenekleri belirleyin:

- Altında **Denetleyici adı**, "OrdersController" girin.
- Altında **şablon**seçin "Entity Framework kullanarak okuma/yazma eylemleri olan API denetleyicisi".
- Altında **Model sınıfı**seçin &quot;sırası (ProductStore.Models)&quot;.
- Altında **veri bağlamı sınıfının**seçin &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

**Ekle**'yi tıklatın. Bu OrdersController.cs adlı bir dosya ekler. Ardından, varsayılan uygulama denetleyicisinin değiştirileceğini ihtiyacımız var.

İlk olarak, Sil `PutOrder` ve `DeleteOrder` yöntemleri. Bu örnek için müşterilerin değiştiremez veya mevcut siparişlerin silemezsiniz. Gerçek bir uygulamada çok sayıda arka uç mantığının bu durumları idare etmek gerekir. (Örneğin, sipariş zaten sevk?)

Değişiklik `GetOrders` yöntemi yalnızca bir kullanıcıya ait siparişlerini döndürmek için:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Değişiklik `GetOrder` yöntemini aşağıdaki şekilde:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Biz yönteme yapılan değişiklikler şunlardır:

- Dönüş değeri bir `OrderDTO` örneği yerine bir `Order`.
- Biz siparişi için veritabanını sorgulama, kullandığımız [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) ilgili getirilecek yöntemi `OrderDetail` ve `Product` varlıklar.
- Biz, sonucu bir yansıtma kullanarak düzleştirin.

HTTP yanıt bir dizi miktarlar ürünleriyle içerir:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Bu biçim, iç içe geçmiş varlıkları (sipariş, ayrıntıları ve ürünleri) içeren özgün nesne grafiği, kullanmak istemcileri için kolaydır.

Bunu göz önüne almanız gereken son yöntemi `PostOrder`. Şu anda, bu yöntem bir `Order` örneği. Ne olacağını düşünün, ancak bir istemci bir istek gövdesi böyle gönderirse:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Bu iyi yapılandırılmış bir siparişi ve Entity Framework sonsuza dek bunun veritabanına ekler. Ancak, önceden var olmayan bir ürün varlığı içerir. İstemci, yalnızca veritabanımızda yer yeni ürün oluşturuldu! Bunlar koala ayılarının için sipariş gördüğünüzde Bu siparişi fullfilment departmanı şaşkınlık olacaktır. Ahlaki, bir POST veya PUT isteği kabul veriler hakkında çok dikkatli olun.

Bu sorunu önlemek için değiştirme `PostOrder` gerçekleştirilecek yöntemi bir `OrderDTO` örneği. Kullanım `OrderDTO` oluşturmak için `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Kullandığımız bildirimi `ProductID` ve `Quantity` özellikleri ve ürün adı veya fiyat için istemciye gönderilen herhangi bir değeri yoksayın. Ürün Kimliği geçerli değilse, veritabanında yabancı anahtar kısıtlaması ihlal ve gerektiği gibi ekleme başarısız olur.

İşte tam `PostOrder` yöntemi:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Son olarak, ekleme **Authorize** özniteliği denetleyiciye:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Artık yalnızca kayıtlı kullanıcıların oluşturabilir veya siparişleri görüntüleyin.

> [!div class="step-by-step"]
> [Önceki](using-web-api-with-entity-framework-part-5.md)
> [İleri](using-web-api-with-entity-framework-part-7.md)
