---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
title: Bir denetleyiciden (C#) modelinizin verilerine erişme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack, 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 002ada5c-f114-47ab-a441-57dbdb728ea0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 6e517538f6ebb2c42f7c1d66efed7831c2c877fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757020"
---
<a name="accessing-your-models-data-from-a-controller-c"></a>Bir denetleyiciden (C#) modelinizin verilerine erişme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.
> 
> 
> Bu öğreticide, Microsoft Visual Web Developer 2010 Express Service Pack ücretsiz bir Microsoft Visual Studio sürümü olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Başlamadan önce aşağıda listelenen ön yüklediğiniz emin olun. Aşağıdaki bağlantıya tıklayarak bunların tümünü yükleyebilirsiniz: [Web Platformu yükleyicisi](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternatif olarak, aşağıdaki bağlantıları kullanarak önkoşulları ayrı ayrı yükleyebilirsiniz:
> 
> - [Visual Studio Web Developer Express SP1 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 araçları güncelleştirme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği)
> 
> Visual Web Developer 2010 yerine Visual Studio 2010 kullanıyorsanız, aşağıdaki bağlantıyı tıklatarak önkoşulları yükleyin: [Visual Studio 2010 önkoşulları](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> C# kaynak kodu içeren bir Visual Web Developer proje, bu konuya eşlik etmek üzere kullanılabilir. [C# sürümü indirme](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Visual Basic tercih ederseniz, geçiş [Visual Basic sürümü](../vb/intro-to-aspnet-mvc-3.md) Bu öğreticinin.

Bu bölümde, yeni bir oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak bir tarayıcıda görüntüleyen kod yazın. Devam etmeden önce uygulamanızı emin olun.

Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi. Aşağıdaki seçenekleri belirleyin:

- Denetleyici adı: **MoviesController**. (Bu varsayılan değerdir. )
- Şablonu: **Entity Framework kullanarak okuma/yazma eylemleri ve görünümler ile denetleyicisi**.
- Model sınıfı: **film (MvcMovie.Models)**.
- Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.
- Görünümler: **Razor (CSHTML)**. (Varsayılan)

[![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image2.png "AddScaffoldedMovieController")](accessing-your-models-data-from-a-controller/_static/image1.png)

**Ekle**'yi tıklatın. Visual Web Developer, aşağıdaki dosya ve klasörleri oluşturur:

- *Bir MoviesController.cs* proje dosyasında *denetleyicileri* klasör.
- A *filmler* proje klasöründe *görünümleri* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

[![NewMovieControllerScreenShot](accessing-your-models-data-from-a-controller/_static/image4.png "NewMovieControllerScreenShot")](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 yapı iskelesi mekanizması otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem metotları ve görünümleri sizin için. Artık oluşturmak, listesinde, düzenlemek ve film girdileri Sil olanak sağlayan tam olarak işlevsel bir web uygulamanız var.

Uygulamayı çalıştırmak ve göz atın `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL. Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), bir tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilir `Index` eylem yöntemi `Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü herhangi henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bazı film ayrıntılarını girin ve ardından **Oluştur** düğmesi.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Tıklayarak **Oluştur** düğmeyi formun film bilgileri veritabanında kaydedildiği sunucuya gönderilecek neden olur. Ardından için yönlendirilirsiniz */Movies* listesinde yeni oluşturulan film görebileceğiniz URL.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png "IndexWhenHarryMet")](accessing-your-models-data-from-a-controller/_static/image7.png)

Birkaç daha fazla film girişi oluşturun. Deneyin **Düzenle**, **ayrıntıları**, ve **Sil** tüm işlevsel bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyiciyle bir kısmını `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Aşağıdaki satırı gelen `MoviesController` sınıf, daha önce açıklandığı gibi bir film veritabanı bağlamı oluşturur. Sorgulama, Düzenle ve Sil filmler film veritabanı bağlamı'nı kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Bir istek `Movies` denetleyicisi tüm girdileri döndürür `Movies` film veritabanı tablosunu ve ardından sonuçları geçirir `Index` görünümü.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Kesin olarak modelleri ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm şablonu geçirebilirsiniz gördüğünüz `ViewBag` nesne. `ViewBag` Görünümüne bilgi geçirmek için kullanışlı bir geç bağlanan yol sağlayan dinamik bir nesnedir.

ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesin geçirme özelliği yazılan de sağlar. Bu yaklaşım etkinleştirir daha iyi kod ve Visual Web Developer düzenleyicisinde zengin IntelliSense denetleme zamanı kesin. Bu yaklaşımı kullanıyoruz `MoviesController` sınıfı ve *Index.cshtml* şablonu görüntüle.

Kodun nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` denetleyicisinden liste görünümüne:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

Ekleyerek bir `@model` deyimi görünümü şablon dosyasının üst görünüm bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisi oluşturduğunuzda otomatik olarak dahil edilen aşağıdaki Visual Web Developer `@model` en üstündeki deyimi *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Bu `@model` yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Index.cshtml* şablonu, kod döngüsü film gerçekleştirerek bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml)]

Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde, IntelliSense desteği tam anlamına gelir:

[![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image10.png "ModelIntellisene")](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>SQL Server Compact ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi işaret eden bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olduğunu doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız *Movies.sdf* dosyasına sağ tıklayıp **tüm dosyaları göster** düğmesine **Çözüm Gezgini** araç çubuğunda tıklatın **Yenile** düğmesini ve ardından *uygulama\_veri* klasör.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png "SDF_in_SolnExp")](accessing-your-models-data-from-a-controller/_static/image11.png)

Çift *Movies.sdf* açmak için **Sunucu Gezgini**. Ardından **tabloları** veritabanında oluşturduğunuz tabloları görmek için klasör.

> [!NOTE]
> Bir hata alırsanız, çift tıkladığınızda *Movies.sdf*, size yüklediğinizden emin olun [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(çalışma zamanı + araçları desteği). (Yazılım bağlantıları için Bu öğretici serisinin 1 bölümünde Önkoşullar listesine bakın.) Sürüm artık yüklerseniz, kapatın ve Visual Web Developer yeniden açmanız gerekir.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png "DB_explorer")](accessing-your-models-data-from-a-controller/_static/image13.png)

İçin iki tablo `Movie` varlık kümesini ve ardından `EdmMetadata` tablo. `EdmMetadata` Tablo model ve veritabanı eşitlenmedi olduğunda belirlemek için Entity Framework tarafından kullanılır.

Sağ `Movies` tablosunu seçip **tablo verilerini Göster** oluşturduğunuz verileri görmek için.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png "MoviesTable")](accessing-your-models-data-from-a-controller/_static/image15.png)

Sağ `Movies` tablosunu seçip **tablo şemasını düzenleme**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png "EditTableSchema")](accessing-your-models-data-from-a-controller/_static/image17.png)

[![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image20.png "TableSchemaSM")](accessing-your-models-data-from-a-controller/_static/image19.png)

Bildirim nasıl şemasını `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şema için dayanarak, `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı kapatın. (Bağlantı kapatmayın, projeyi bir sonraki çalıştırmanızda hata alabilirsiniz).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image22.png "CloseConnection")](accessing-your-models-data-from-a-controller/_static/image21.png)

Artık veritabanı ve bu içeriği görüntülemek için bir basit listesi sayfası vardır. Sonraki öğreticide, biz iskele kurulan kodu geri kalanını inceleyin ve ekleme bir `SearchIndex` yöntemi ve bir `SearchIndex` filmler bu veritabanındaki aramanıza olanak tanıyan bir görünüm.

> [!div class="step-by-step"]
> [Önceki](adding-a-model.md)
> [İleri](examining-the-edit-methods-and-edit-view.md)
