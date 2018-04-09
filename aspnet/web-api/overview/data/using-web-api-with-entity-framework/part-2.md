---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Modelleri ve denetleyicileri ekleyin | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 015bb9698d81387d03ea8f9629316fb53232e708
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="add-models-and-controllers"></a>Modelleri ve denetleyicileri ekleyin
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](https://github.com/MikeWasson/BookService)

Bu bölümde, veritabanı varlıklarını tanımlama modeli sınıfları ekleyeceksiniz. Ardından bu varlıkların CRUD işlemleri Web API denetleyicilerinin ekleyeceksiniz.

## <a name="add-model-classes"></a>Model sınıfları ekleme

Bu öğreticide, veritabanı "Code First" yaklaşımı Entity Framework (EF) kullanarak oluşturacağız. Code First ile veritabanı tablolarında karşılık gelen C# sınıfları yazma ve veritabanı EF oluşturur. (Daha fazla bilgi için bkz: [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatın. Aşağıdaki POCOs oluşturacağız:

- Yazar
- Rehberi

Çözüm Gezgini'nde modeller klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **sınıfı**. Sınıf adını `Author`.

![](part-2/_static/image1.png)

Tüm Author.cs Demirbaş kodu aşağıdaki kodla değiştirin.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Adlı başka bir sınıf ekleyin `Book`, aşağıdaki kod ile.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework veritabanı tabloları oluşturmak için bu modeller kullanır. Her model için `Id` özelliği, veritabanı tablosunun birincil anahtar sütunu hale gelir.

Kitap sınıfında `AuthorId` içine yabancı anahtar tanımlayan `Author` tablo. (Kolaylık sağlamak için t her kitap tek Yazar olduğunu varsayarak.) Ayrıca Gezinti özelliğine ilgili kitap sınıf içerir `Author`. Gezinti özelliği ilgili erişmek için kullanabileceğiniz `Author` kod. Bölüm 4, gezinti özellikleri hakkında daha fazla söyleyin [işleme varlık ilişkileriyle](part-4.md).

## <a name="add-web-api-controllers"></a>Web API denetleyicilerinin ekleme

Bu bölümde, CRUD işlemleri destekleyen Web API denetleyicilerinin ekleyeceğiz (oluşturma, okuma, güncelleştirme ve silme). Denetleyicileri Entity Framework veritabanı katmanı ile iletişim kurmak için kullanır.

İlk olarak, dosya Controllers/ValuesController.cs silebilirsiniz. Bu dosya bir örnek Web API denetleyicisi içeriyor, ancak bu öğretici için gerekmez.

![](part-2/_static/image2.png)

Ardından, projeyi oluşturun. Web API yapı iskelesi yansıma derlenmiş derleme gereken şekilde modeli sınıfları bulmak için kullanır.

Çözüm Gezgini'nde denetleyicileri klasörü sağ tıklatın. Seçin **Ekle**seçeneğini belirleyip **denetleyicisi**.

![](part-2/_static/image3.png)

İçinde **İskele Ekle** iletişim kutusunda "Web API 2 denetleyici Entity Framework kullanarak Eylemler ile". **Ekle**'yi tıklatın.

![](part-2/_static/image4.png)

İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdakileri yapın:

1. İçinde **Model sınıfı** açılan listesinde, select `Author` sınıfı. (Bunu açılır listede görmüyorsanız, proje yerleşik emin olun.)
2. "Kullanım zaman uyumsuz denetleyici eylemlerini" denetleyin.
3. Denetleyici adı olarak bırakın &quot;AuthorsController&quot;.
4. Tıklatın artı (+) düğmesini yanına **veri bağlamı sınıfı**.

![](part-2/_static/image5.png)

İçinde **yeni veri bağlamı** iletişim kutusunda, varsayılan adı bırakın ve tıklayın **Ekle**.

![](part-2/_static/image6.png)

Tıklatın **Ekle** tamamlamak için **denetleyici Ekle** iletişim. İletişim kutusu iki sınıf projenize ekler:

- `AuthorsController` Web API denetleyicisi tanımlar. İstemcilerin yazarlar listesinde CRUD işlemleri gerçekleştirmek için kullandığı REST API denetleyicisi uygular.
- `BookServiceContext` Veritabanı, değişiklik izleme ve kalıcı veri verilerini veritabanına nesnelerle doldurmak içeren çalışma zamanı sırasında varlık nesneleri yönetir. Öğesinden devralınan `DbContext`.

![](part-2/_static/image7.png)

Bu noktada, projeyi yeniden oluşturun. Şimdi bir API denetleyicisi için eklemek için aynı adımlarını `Book` varlıklar. Bu süre, select `Book` model sınıfı ve varolan seçin `BookServiceContext` veri bağlamı sınıfı için sınıf. (Yeni bir veri bağlamı oluşturmayın.) Tıklatın **Ekle** denetleyicisi eklemek için.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Önceki](part-1.md)
> [sonraki](part-3.md)
