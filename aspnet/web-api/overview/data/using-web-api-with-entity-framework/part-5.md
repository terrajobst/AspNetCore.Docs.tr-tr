---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Veri aktarımı nesnesi (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 35e01f959072b96204de3e2ce3d507635720e110
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="create-data-transfer-objects-dtos"></a>Veri aktarımı nesne (DTOs) oluşturma
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Sağ istemci veritabanı varlıklara artık, bizim web API kullanıma sunar. İstemci, doğrudan veritabanı tablolarında eşlemeleri veri alır. Bununla birlikte, her zaman iyi bir fikir değil. Bazen istemciye göndermek veri şeklini değiştirmek istersiniz. Örneğin, aşağıdakileri yapabilirsiniz:

- Dairesel başvurular (önceki bölüme bakın) kaldırın.
- İstemcileri görüntülemek için görmemesi belirli özelliklerini gizle.
- Yükü boyutunu azaltmak için bazı özellikler atlayın.
- Bunları istemciler için daha kullanışlı hale getirmek için iç içe nesneleri içeren nesne grafiklerinin düzleştirmek.
- "Güvenlik açıkları aşırı gönderim" kaçının. (Bkz [Model doğrulama](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) atlayarak nakil Tartışması için.)
- Veritabanı katmanı, hizmet katmanından ayırırsınız.

Bunu başarmak için tanımlayabilirsiniz bir *veri aktarım nesnesini* (DTO). Bir DTO nasıl verileri ağ üzerinden gönderilir tanımlayan bir nesnedir. Kitap varlıkla nasıl gerçekleştiğine görelim. Modeller klasörü iki DTO sınıfları ekleyin:

[!code-csharp[Main](part-5/samples/sample1.cs)]

`BookDetailDTO` Sınıfı kitap modelden hariç özelliklerin tümünü içeren `AuthorName` yazar adı tutacak bir dizedir. `BookDTO` Sınıfı içeren bir alt kümesini özelliklerinden `BookDetailDTO`.

Ardından, iki GET yöntemi Değiştir `BooksController` DTOs dönüş sürümleriyle sınıfı. LINQ kullanacağız **seçin** defteri varlıklardan DTOs dönüştürmek için deyimi.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Yeni tarafından oluşturulan SQL işte `GetBooks` yöntemi. EF LINQ çevirir görebilirsiniz **seçin** SQL SELECT deyimi içinde.

[!code-sql[Main](part-5/samples/sample3.sql)]

Son olarak, değişiklik `PostBook` bir DTO döndürülecek yöntemi.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Bu öğreticide, biz DTOs için el ile kodda dönüştürdüğünüz. Gibi bir kitaplık kullanmak üzere başka bir seçenektir [AutoMapper](http://automapper.org/) dönüştürme otomatik olarak yönetir.
> 
> [!div class="step-by-step"]
> [Önceki](part-4.md)
> [sonraki](part-6.md)
