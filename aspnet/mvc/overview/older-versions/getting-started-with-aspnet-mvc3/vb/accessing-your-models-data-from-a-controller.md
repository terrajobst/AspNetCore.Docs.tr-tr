---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Modelinizin verilere denetleyicisinden (VB) | Microsoft Docs
author: Rick-Anderson
description: "Bu öğretici Microsoft Visual Web Developer 2010 Express Service Pack olan 1, kullanarak bir ASP.NET MVC Web uygulaması oluşturmanın temellerini öğretmek..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d0c6976519f4f4bae10fabf4cbf85401de4f58e7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Bir denetleyici (VB) modelinizin verilerine erişme
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
> VB.NET kaynak kodu ile Visual Web Developer projesi bu konuya eşlik etmek kullanılabilir. [VB.NET Eki](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). C# tercih ederseniz, geçiş [C# sürümü](../cs/accessing-your-models-data-from-a-controller.md) Bu öğreticinin.


Bu bölümde, yeni oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak tarayıcıda görüntüleyen kod yazın. Devam etmeden önce uygulamanızı emin olun.

Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi. Aşağıdaki seçenekleri belirleyin:

- Denetleyici adı: **MoviesController**. (Varsayılan değer budur.)
- Şablonu: **denetleyici Entity Framework kullanarak okuma/yazma eylemleri ve görünümleri ile**.
- Model sınıfı: **film (MvcMovie.Models)**.
- Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.
- Görünümleri: **Razor (CSHTML)**. (Varsayılan)

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

**Ekle**'yi tıklatın. Visual Web Developer aşağıdaki dosyaları ve klasörleri oluşturur:

- *Bir MoviesController.vb* projenin dosyasında *denetleyicileri* klasör.
- A *filmler* projenin klasöründe *görünümleri* klasör.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, ve *Index.vbhtml* yeni *Views\Movies* klasör.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

ASP.NET MVC 3 yapı iskelesi mekanizması otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümleri. Artık oluşturma listesinde, düzenleme ve film girişleri silmek olanak sağlayan bir tam olarak işlevsel bir web uygulaması sahipsiniz.

Uygulamayı çalıştırın ve Gözat `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye. Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilen `Index` eylem yöntemi `Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bir filmi bazı ayrıntılarını girin ve ardından **oluşturma** düğmesi.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Tıklatarak **oluşturma** düğmesi sunucuya film bilgileri veritabanında kaydedildiği postalama forma neden olur. Ardından için yönlendirilirsiniz */Movies* burada görebilirsiniz listesinde yeni oluşturulan film URL.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Daha fazla birkaç film girişleri oluşturur. Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.vb* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyicisiyle bir kısmı `Index` yöntemi aşağıda gösterilmektedir.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

Aşağıdaki satır `MoviesController` sınıfı, daha önce açıklandığı gibi bir filmi veritabanı bağlamını başlatır. Film veritabanı bağlamı, sorgu, düzenlemek ve filmler silmek için kullanabilirsiniz.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Bir istek `Movies` denetleyicisi döndürür tüm girişler `Movies` film veritabanı tablosunun ve sonuçları geçirir `Index` görünümü.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler görünümü kullanarak şablonu geçirebilirsiniz gördüğünüzü `ViewBag` nesnesi. `ViewBag` Bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesnedir.

ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesinlikle geçirme özelliği belirtilmiş de sağlar. Bu yaklaşım etkinleştirir daha iyi derleme kodu ve Visual Web Developer düzenleyicisinde daha zengin IntelliSense denetleme zamanı kesin türü belirtilmiş. Bu yaklaşımda kullanmakta olduğunuz `MoviesController` sınıfı ve *Index.vbhtml* şablonu görüntüle.

Kodu nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` denetleyicisinden listesini görüntülemek için:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Ekleyerek bir `@ModelType` deyimini dosyanın üst kısmındaki görünüm şablonu görünümü bekliyor nesne türünü belirtebilirsiniz. Visual Web Developer film denetleyicisini oluşturduğunuzda, otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Index.vbhtml* dosyası:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Bu `@ModelType` yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Index.vbhtml* şablon kodu döngüler filmler yaparak bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde IntelliSense desteği tam anlamına gelir:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>SQL Server Compact ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi için işaret bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olmadığını doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız, *Movies.sdf* dosya, tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** araç, tıklatın **yenileme** düğmesine tıklayın ve ardından *uygulama\_veri* klasör.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Çift *Movies.sdf* açmak için **Sunucu Gezgini**. Ardından **tabloları** veritabanında oluşturulan tabloları görmek için klasör.

> [!NOTE]
> Çift tıkladığınızda bir hata alırsanız *Movies.sdf*, size yüklediğinizden emin olun **SQL Server Compact 4.0 için Visual Studio 2010 SP1 Araçları**. (Yazılım bağlantıları için Bu öğretici seri 1 bölümündeki önkoşul listesi bakın.) Yayın şimdi yüklerseniz, kapatmak ve Visual Web Developer yeniden açmak gerekir.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

İçin iki tablo `Movie` varlık kümesini ve ardından `EdmMetadata` tablo. `EdmMetadata` Tablo modeli ve veritabanı eşitlenmemiş olduğunda belirlemek için Entity Framework tarafından kullanılır.

Sağ `Movies` tablo ve seçin **Show Table Data** oluşturduğunuz verileri görmek için.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Sağ `Movies` tablo ve seçin **Düzenle tablo şemasını**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Bildirim nasıl şeması `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şemayı dayanarak için `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı kapatın. (Bağlantı kapatmayın, projenin bir sonraki çalıştırmanızda bir hata alabilirsiniz).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Artık bu içeriği görüntülemek için bir basit liste sayfası ve veritabanı vardır. Sonraki öğreticide biz kurulmuş kodu kalan inceleyin ve ekleme bir `SearchIndex` yöntemi ve `SearchIndex` bu veritabanında filmler arama yapmanıza olanak tanıyan görünümü.

>[!div class="step-by-step"]
[Önceki](adding-a-model.md)
[sonraki](examining-the-edit-methods-and-edit-view.md)
