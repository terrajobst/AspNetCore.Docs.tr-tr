---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Modelinizin verilere bir denetleyicisinden (C#) | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 4218116eec6f177730087955b8fb69e5ecc0022f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872809"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Modelinizin verilere bir denetleyicisinden (C#)
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz Microsoft Visual Studio sürümünü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek. Başlamadan önce aşağıda listelenen önkoşulları kurduğunuzdan emin olun. Bunların tümünün aşağıdaki bağlantıyı tıklatarak yükleyin: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [C# sürümü](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürüm](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.

Bu bölümde, yeni oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak tarayıcıda görüntüleyen kod yazın. Devam etmeden önce uygulamanızı emin olun.

Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi. Aşağıdaki seçenekleri belirleyin:

- Denetleyici adı: **MoviesController**. (Varsayılan değer budur. )
- Şablonu: **denetleyici Entity Framework kullanarak okuma/yazma eylemleri ve görünümleri ile**.
- Model sınıfı: **film (MvcMovie.Models)**.
- Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.
- Görünümleri: **Razor (CSHTML)**. (Varsayılan)

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

**Ekle**'yi tıklatın. Visual Web Developer aşağıdaki dosyaları ve klasörleri oluşturur:

- *Bir MoviesController.cs* projenin dosyasında *denetleyicileri* klasör.
- A *filmler* projenin klasöründe *görünümleri* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 yapı iskelesi mekanizması otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümleri. Artık oluşturma listesinde, düzenleme ve film girişleri silmek olanak sağlayan bir tam olarak işlevsel bir web uygulaması sahipsiniz.

Uygulamayı çalıştırın ve Gözat `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye. Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilen `Index` eylem yöntemi `Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bir filmi bazı ayrıntılarını girin ve ardından **oluşturma** düğmesi.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Tıklatarak **oluşturma** düğmesi sunucuya film bilgileri veritabanında kaydedildiği postalama forma neden olur. Ardından için yönlendirilirsiniz */Movies* burada görebilirsiniz listesinde yeni oluşturulan film URL.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Daha fazla birkaç film girişleri oluşturur. Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyicisiyle bir kısmı `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Aşağıdaki satır `MoviesController` sınıfı, daha önce açıklandığı gibi bir filmi veritabanı bağlamını başlatır. Film veritabanı bağlamı, sorgu, düzenlemek ve filmler silmek için kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Bir istek `Movies` denetleyicisi döndürür tüm girişler `Movies` film veritabanı tablosunun ve sonuçları geçirir `Index` görünümü.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler görünümü kullanarak şablonu geçirebilirsiniz gördüğünüzü `ViewBag` nesnesi. `ViewBag` Bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesnedir.

ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesinlikle geçirme özelliği belirtilmiş de sağlar. Bu yaklaşım etkinleştirir daha iyi derleme kodu ve Visual Web Developer düzenleyicisinde daha zengin IntelliSense denetleme zamanı kesin türü belirtilmiş. Bu yaklaşımda kullanmakta olduğunuz `MoviesController` sınıfı ve *Index.cshtml* şablonu görüntüle.

Kodu nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` denetleyicisinden listesini görüntülemek için:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünüm şablonu görünümü bekliyor nesne türünü belirtebilirsiniz. Visual Web Developer film denetleyicisini oluşturduğunuzda, otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Index.cshtml* şablon kodu döngüler filmler yaparak bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde IntelliSense desteği tam anlamına gelir:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>SQL Server Compact ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi için işaret bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olmadığını doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız, *Movies.sdf* dosya, tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** araç, tıklatın **yenileme** düğmesine tıklayın ve ardından *uygulama\_veri* klasör.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Çift *Movies.sdf* açmak için **Sunucu Gezgini**. Ardından **tabloları** veritabanında oluşturulan tabloları görmek için klasör.

> [!NOTE]
> Çift tıkladığınızda bir hata alırsanız *Movies.sdf*, size yüklediğinizden emin olun [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları destekler). (Yazılım bağlantıları için Bu öğretici seri 1 bölümündeki önkoşul listesi bakın.) Yayın şimdi yüklerseniz, kapatmak ve Visual Web Developer yeniden açmak gerekir.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

İçin iki tablo `Movie` varlık kümesini ve ardından `EdmMetadata` tablo. `EdmMetadata` Tablo modeli ve veritabanı eşitlenmemiş olduğunda belirlemek için Entity Framework tarafından kullanılır.

Sağ `Movies` tablo ve seçin **Show Table Data** oluşturduğunuz verileri görmek için.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Sağ `Movies` tablo ve seçin **Düzenle tablo şemasını**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Bildirim nasıl şeması `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şemayı dayanarak için `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı kapatın. (Bağlantı kapatmayın, projenin bir sonraki çalıştırmanızda bir hata alabilirsiniz).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Artık bu içeriği görüntülemek için bir basit liste sayfası ve veritabanı vardır. Sonraki öğreticide biz kurulmuş kodu kalan inceleyin ve ekleme bir `SearchIndex` yöntemi ve `SearchIndex` bu veritabanında filmler arama yapmanıza olanak tanıyan görünümü.

> [!div class="step-by-step"]
> [Önceki](adding-a-model.md)
> [sonraki](examining-the-edit-methods-and-edit-view.md)
