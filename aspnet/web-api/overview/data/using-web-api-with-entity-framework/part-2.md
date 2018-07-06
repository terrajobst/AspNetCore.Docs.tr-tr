---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Model ve denetleyici ekleme | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 31e0fbf65c74a18677588cfa412dc34f7b25f78f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806364"
---
<a name="add-models-and-controllers"></a>Model ve denetleyici ekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](https://github.com/MikeWasson/BookService)

Bu bölümde, veritabanı varlıklar tanımlayan bir model sınıfları ekleyeceksiniz. Ardından bu varlıkların CRUD işlemleri Web APİ'si denetleyicilerinin ekleyeceksiniz.

## <a name="add-model-classes"></a>Model sınıfları ekleme

Bu öğreticide, veritabanı Entity Framework (EF) "Code First" yaklaşımı kullanarak oluşturacağız. Code First ile veritabanı tablolarında karşılık gelen C# sınıfları yazma ve veritabanı EF oluşturur. (Daha fazla bilgi için [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf).)

Biz, bizim etki alanı nesnelerini POCOs (düz eski CLR nesneler) tanımlayarak başlatın. Aşağıdaki POCOs oluşturacağız:

- Yazar
- Rehberi

Çözüm Gezgini'nde modeller klasörü sağ tıklayın. Seçin **Ekle**, ardından **sınıfı**. Sınıf adını `Author`.

![](part-2/_static/image1.png)

Tüm Author.cs Demirbaş kod, aşağıdaki kod ile değiştirin.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Adlı başka bir sınıf ekleyin `Book`, aşağıdaki kod ile.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework veritabanı tabloları oluşturmak için bu modelleri kullanır. Her model için `Id` özelliği, veritabanı tablosunun birincil anahtar sütunu olacak.

Kitap sınıfında `AuthorId` içine bir yabancı anahtar tanımlar `Author` tablo. (Kolaylık olması için miyim her kitabın tek bir yazar olduğunu varsayarak.) Kitap sınıfı da bir gezinti özelliğine ilgili içeren `Author`. Gezinme özelliğini ilgili erişmek için kullanabileceğiniz `Author` kod. Bölüm 4, gezinti özellikleri hakkında daha fazla söyleyin [varlık ilişkilerini işleme](part-4.md).

## <a name="add-web-api-controllers"></a>Web API denetleyicisi Ekle

Bu bölümde, CRUD işlemleri destekleyen Web APİ'si denetleyicilerinin ekleyeceğiz (oluşturma, okuma, güncelleştirme ve silme). Denetleyici, Entity Framework veritabanı katmanı ile iletişim kurmak için kullanır.

İlk olarak, dosya Controllers/ValuesController.cs silebilirsiniz. Bu dosya bir örnek Web API denetleyicisi içerir, ancak bu öğretici için gereksinim duymuyorsanız.

![](part-2/_static/image2.png)

Ardından, projeyi derleyin. Web APİ'si yapı iskelesini, derlenen bütünleştirilmiş gerekir model sınıfları bulmak için yansıtma kullanır.

Çözüm Gezgini'nde denetleyicileri klasörüne sağ tıklayın. Seçin **Ekle**, ardından **denetleyicisi**.

![](part-2/_static/image3.png)

İçinde **İskele Ekle** iletişim kutusunda "Web API 2 denetleyici Entity Framework kullanarak Eylemler ile". **Ekle**'yi tıklatın.

![](part-2/_static/image4.png)

İçinde **denetleyici Ekle** iletişim kutusunda, aşağıdakileri yapın:

1. İçinde **Model sınıfı** açılır menüsünde, select `Author` sınıfı. (Bu açılır listede görmüyorsanız, proje oluşturulan emin olun.)
2. "Zaman uyumsuz denetleyici eylemleri kullanın" denetleyin.
3. Denetleyici adı olarak bırakın &quot;AuthorsController&quot;.
4. Click artı (+) düğmesine yanındaki **veri bağlamı sınıfının**.

![](part-2/_static/image5.png)

İçinde **yeni veri bağlamı** iletişim kutusunda varsayılan adı bırakın ve tıklayın **Ekle**.

![](part-2/_static/image6.png)

Tıklayın **Ekle** tamamlanması **denetleyici Ekle** iletişim. İletişim kutusu, iki sınıf projenize ekler:

- `AuthorsController` bir Web API denetleyicisi tanımlar. İstemcilerin yazarlar listesinde CRUD işlemleri gerçekleştirmek için kullandığı REST API denetleyicisi uygular.
- `BookServiceContext` bir veritabanı, değişiklik izleme ve kalıcı veri verilerini veritabanına nesnelerle doldurmak içeren çalışma zamanı sırasında varlık nesnelerini yönetir. Devraldığı `DbContext`.

![](part-2/_static/image7.png)

Bu noktada, projeyi yeniden derleyin. Artık bir API denetleyicisi eklemek için aynı adımları inceleyin `Book` varlıklar. Bu kez, select `Book` model sınıfı ve mevcut seçim için `BookServiceContext` veri bağlamı sınıfı için sınıf. (Yeni bir veri bağlamı oluşturmayın.) Tıklayın **Ekle** denetleyicisi eklemek için.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Önceki](part-1.md)
> [İleri](part-3.md)
