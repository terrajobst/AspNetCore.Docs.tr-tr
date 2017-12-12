---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Bir Model (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: efc18dd71e29d12dacc6cf84a1d3c7f7e92f520d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model-vb"></a>Bir Model (VB) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-model.md) Bu öğreticinin.


## <a name="adding-a-model"></a>Model ekleme

Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz. Bu sınıfların "modeli" bölümü ASP.NET MVC uygulamasının olacaktır.

Entity Framework bilinen bir .NET Framework veri erişimi teknoloji tanımlayabilir ve bu modeli sınıfları ile çalışmak için kullanacağız. Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar. (Bu da POCO sınıflardan "düz eski CLR nesneler." verilir) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

"Film" sınıf adı.

Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf. Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.

Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği. `MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı. Hakkında daha fazla bilgi için `DbContext` ve `DbSet`, bkz: [Entity Framework için üretkenlik iyileştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `imports` deyimini dosyanın üst:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Tam *Movie.vb* dosya aşağıda gösterilmektedir.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Bağlantı dizesi oluşturma ve SQL Server Compact ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere. Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir. Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.

Uygulama kök açmak *Web.config* dosya. (Değil *Web.config* dosyasını *görünümleri* klasörü.) Aşağıdaki görüntü Göster her ikisi de *Web.config* ; açık dosyalar *Web.config* dosya kırmızıyla yuvarlak içine alınmıştır.

![](adding-a-model/_static/image2.png)

## 

Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Bu küçük kod ve XML temsil eder ve bir veritabanında film verileri depolamak için yazma için gereken her şeyi miktarıdır.

Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.

>[!div class="step-by-step"]
[Önceki](adding-a-view.md)
[sonraki](accessing-your-models-data-from-a-controller.md)
