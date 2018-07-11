---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 2d99a9c553ba79922cb0c5e852709ab97ea2e22b
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38211060"
---
<a name="adding-a-model"></a>Model ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz. Bu sınıflar olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması bir parçası.

Bir .NET Framework Veri erişim teknolojisi olarak bilinen kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/) tanımlayın ve bu model sınıfları ile çalışmak için. Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar. (Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnesi.&quot;) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir. İlk veritabanı oluşturmak için gerekli olduğunda, yine de MVC ve EF uygulama geliştirme hakkında bilgi edinmek için bu öğreticiyi takip edebilirsiniz. Ardından, Tom Fizmakens izleyebilirsiniz [ASP.NET iskeleti oluşturma](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) veritabanı ilk yaklaşım kapsayan bir öğretici.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

Girin *sınıfı* adı &quot;film&quot;.

Aşağıdaki beş özelliği Ekle `Movie` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf. Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.

Not: System.Data.Entity ve ilgili sınıf kullanmak için yüklemeniz gerekir [Entity Framework NuGet paketi](https://www.nuget.org/packages/EntityFramework/). Daha fazla yönerge için bağlantıyı izleyin.

Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri. `MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Bunu kullanarak el ile ekleyerek yapabilirsiniz deyimi veya kırmızı dalgalı çizgiler gelin, tıklayın `Show potential fixes` tıklayın `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Not: Birkaç kullanılmayan `using` deyimleri kaldırıldı. Visual Studio kullanılmayan bağımlılıkları gri renkte gösterilir. Gri bağımlılıkları gelerek kullanılmayan bağımlılıkları kaldırın, tıklayın `Show potential fixes` tıklatıp **kullanılmayan kullanımları kaldırma.**

![](adding-a-model/_static/image3.png)

Son olarak bir modeli (M mvc'de) ekledik. Sonraki bölümde veritabanı bağlantı dizesi ile çalışırsınız.

> [!div class="step-by-step"]
> [Önceki](adding-a-view.md)
> [İleri](creating-a-connection-string.md)
