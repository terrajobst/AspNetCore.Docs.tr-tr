---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
title: Bir Model (C#) ekleme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 42355b95-5f1f-413e-8d16-14cdfaaefcd8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7f9206ddd43b4a3b4f96240ee48b9414450da22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879354"
---
<a name="adding-a-model-c"></a>Bir Model (C#) ekleme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/adding-a-model.md) Bu öğreticinin.


## <a name="adding-a-model"></a>Model ekleme

Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz. Bu sınıfların "modeli" bölümü ASP.NET MVC uygulamasının olacaktır.

Entity Framework bilinen bir .NET Framework veri erişimi teknoloji tanımlayabilir ve bu modeli sınıfları ile çalışmak için kullanacağız. Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar. (Bu da POCO sınıflardan "düz eski CLR nesneler." verilir) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

Ad *sınıfı* "Film".

[![CreateMovieClass](adding-a-model/_static/image3.png)](adding-a-model/_static/image2.png)

Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf. Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.

Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği. `MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı. Hakkında daha fazla bilgi için `DbContext` ve `DbSet`, bkz: [Entity Framework için üretkenlik iyileştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Tam *Movie.cs* dosya aşağıda gösterilmektedir.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Bağlantı dizesi oluşturma ve SQL Server Compact ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere. Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir. Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.

Uygulama kök açmak *Web.config* dosya. (Değil *Web.config* dosyasını *görünümleri* klasörü.) Aşağıdaki görüntü Göster her ikisi de *Web.config* ; açık dosyalar *Web.config* dosya kırmızıyla yuvarlak içine alınmıştır.

![](adding-a-model/_static/image4.png)

### 

Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Bu küçük kod ve XML temsil eder ve bir veritabanında film verileri depolamak için yazma için gereken her şeyi miktarıdır.

Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.

> [!div class="step-by-step"]
> [Önceki](adding-a-view.md)
> [sonraki](accessing-your-models-data-from-a-controller.md)
