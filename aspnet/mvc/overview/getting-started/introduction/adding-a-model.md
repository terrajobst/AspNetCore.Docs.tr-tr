---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: b3ef871c4d7627a03c8f0fd8cce9d3e97fc1a4ba
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model"></a>Model ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz. Bu sınıfların olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması parçası.

Olarak bilinen bir .NET Framework veri erişimi teknoloji kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/) tanımlamak ve bu modeli sınıfları ile çalışmak için. Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar. (Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnelerini.&quot;) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir. Veritabanını oluşturmak için önce gerekli olduğunda hala MVC ve EF uygulama geliştirme hakkında bilgi edinmek için bu öğreticiyi izleyebilirsiniz. Ardından zel Fizmakens izleyin [ASP.NET yapı İskelesi](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) veritabanı ilk yaklaşımı kapsayan öğretici.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

Girin *sınıfı* adı &quot;film&quot;.

Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf. Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.

Not: System.Data.Entity ve ilgili sınıfını kullanmak için yüklemeniz gerekir [Entity Framework NuGet paketi](https://www.nuget.org/packages/EntityFramework/). Daha fazla yönerge için bağlantıyı izleyin.

Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği. `MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Bunu kullanarak el ile ekleyerek yapabilirsiniz deyimi veya kırmızı dalgalı çizgiler getirin, tıklatın `Show potential fixes` ve'ı tıklatın `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Not: Birkaç kullanılmayan `using` deyimleri kaldırıldı. Visual Studio kullanılmayan bağımlılıklarını gri renkte gösterilir. Gri bağımlılıkları gelerek kullanılmayan bağımlılıkları kaldırın, tıklatın `Show potential fixes` tıklatıp **kullanılmayan kullanımları kaldırma.**

![](adding-a-model/_static/image3.png)

Son olarak bir modeli (MVC M) ekledik. Sonraki bölümde veritabanı bağlantı dizesi ile çalışması.

> [!div class="step-by-step"]
> [Önceki](adding-a-view.md)
> [sonraki](creating-a-connection-string.md)
