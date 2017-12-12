---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: "Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>Model ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz. Bu sınıfların olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması parçası.

Olarak bilinen bir .NET Framework veri erişimi teknoloji kullanacağınız [Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) tanımlamak ve bu modeli sınıfları ile çalışmak için. Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar. (Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnelerini.&quot;) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

Girin *sınıfı* adı &quot;film&quot;.

Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf. Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.

Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği. `MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı.

Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Tam *Movie.cs* dosya aşağıda gösterilmektedir. (Birkaç olmayan deyimleri kullanarak kaldırıldı.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere. Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir. Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.

Uygulama kök açmak *Web.config* dosya. (Değil *Web.config* dosyasını *görünümleri* klasörü.) Açık *Web.config* kırmızı renkle dosya.

![](adding-a-model/_static/image2.png)

Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Bu küçük kod ve XML temsil eder ve bir veritabanında film verileri depolamak için yazma için gereken her şeyi miktarıdır.

Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.

>[!div class="step-by-step"]
[Önceki](adding-a-view.md)
[sonraki](accessing-your-models-data-from-a-controller.md)
