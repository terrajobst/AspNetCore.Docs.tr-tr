---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Model (VB) ekleme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 0c846d31fb6b24ad964d69d0f81858c87305d68c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756355"
---
<a name="adding-a-model-vb"></a>Model (VB) ekleme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Bu konuya eşlik etmek üzere bir Visual Web Developer proje VB.NET kaynak koduyla birlikte kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/adding-a-model.md) Bu öğreticinin.


## <a name="adding-a-model"></a>Model ekleme

Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz. Bu sınıf, ASP.NET MVC uygulamasını "modeli" parçası olacak.

Entity Framework bilinen bir .NET Framework Veri erişim teknolojisi tanımlayın ve bu model sınıfları ile çalışmak için kullanacaksınız. Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*. Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar. (Bunlar da POCO sınıflardan "düz eski CLR nesnesi." verilir) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir.

## <a name="adding-model-classes"></a>Model sınıfları ekleme

İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.

![](adding-a-model/_static/image1.png)

"Film" sınıfı adı.

Aşağıdaki beş özelliği Ekle `Movie` sınıfı:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf. Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.

Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri. `MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı. Hakkında daha fazla bilgi için `DbContext` ve `DbSet`, bkz: [Entity Framework için üretkenlik geliştirmeleri](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `imports` deyimini dosyanın üst:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Tam *Movie.vb* dosya aşağıda gösterilmektedir.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Bağlantı dizesi oluşturma ve SQL Server Compact ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri. Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir. Bu bağlantı bilgilerini ekleyerek gerçekleştirirsiniz *Web.config* uygulamanın dosya.

Uygulama kökü açın *Web.config* dosya. (Değil *Web.config* dosyası *görünümleri* klasör.) Aşağıdaki görüntüde göstermek hem de *Web.config* dosyaları; açık *Web.config* dosya kırmızı daire içinde.

![](adding-a-model/_static/image2.png)

## 

Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Bu küçük kod ve XML temsil eder ve film verileri bir veritabanında saklamak için yazmanız gereken her şeyi miktarıdır.

Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.

> [!div class="step-by-step"]
> [Önceki](adding-a-view.md)
> [İleri](accessing-your-models-data-from-a-controller.md)
