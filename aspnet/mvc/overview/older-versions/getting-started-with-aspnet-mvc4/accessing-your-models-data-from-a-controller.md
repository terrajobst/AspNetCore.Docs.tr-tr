---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Bir denetleyicisinden modelinizin verilere | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğreticide güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu daha güvenli, daha kolay izleyin ve gösteri...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyicisinden modelinizin verilerine erişme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğretici güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Daha güvenli, izlemek çok daha kolaydır ve daha fazla özelliklerini gösterir.


Bu bölümde, yeni oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak tarayıcıda görüntüleyen kod yazın.

**Uygulamayı derleme** sonraki adıma geçmeden önce.

Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi. Aşağıdaki seçenekler, uygulamanızı kadar görünmez. Aşağıdaki seçenekleri belirleyin:

- Denetleyici adı: **MoviesController**. (Varsayılan değer budur. )
- Şablonu: **MVC denetleyici Entity Framework kullanarak okuma/yazma eylemleri ve görünümleri ile**.
- Model sınıfı: **film (MvcMovie.Models)**.
- Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.
- Görünümleri: **Razor (CSHTML)**. (Varsayılan)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

**Ekle**'yi tıklatın. Visual Studio Express, aşağıdaki dosyaları ve klasörleri oluşturur:

- *Bir MoviesController.cs* projenin dosyasında *denetleyicileri* klasör.
- A *filmler* projenin klasöründe *görünümleri* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

ASP.NET MVC 4 otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümleri (CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir). Artık oluşturma listesinde, düzenleme ve film girişleri silmek olanak sağlayan bir tam olarak işlevsel bir web uygulaması sahipsiniz.

Uygulamayı çalıştırın ve Gözat `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye. Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilen `Index` eylem yöntemi `Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bir filmi bazı ayrıntılarını girin ve ardından **oluşturma** düğmesi.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Tıklatarak **oluşturma** düğmesi sunucuya film bilgileri veritabanında kaydedildiği postalama forma neden olur. Ardından için yönlendirilirsiniz */Movies* burada görebilirsiniz listesinde yeni oluşturulan film URL.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Daha fazla birkaç film girişleri oluşturur. Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyicisiyle bir kısmı `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Aşağıdaki satır `MoviesController` sınıfı, daha önce açıklandığı gibi bir filmi veritabanı bağlamını başlatır. Film veritabanı bağlamı, sorgu, düzenlemek ve filmler silmek için kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Bir istek `Movies` denetleyicisi döndürür tüm girişler `Movies` film veritabanı tablosunun ve sonuçları geçirir `Index` görünümü.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler görünümü kullanarak şablonu geçirebilirsiniz gördüğünüzü `ViewBag` nesnesi. `ViewBag` Bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesnedir.

ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesinlikle geçirme özelliği belirtilmiş de sağlar. Bu yaklaşım etkinleştirir daha iyi derleme kodu ve Visual Studio düzenleyicisinde daha zengin IntelliSense denetleme zamanı kesin türü belirtilmiş. Bu yaklaşımda Visual Studio yapı iskelesi yönteminde kullanılan `MoviesController` yöntemleri ve görünümler oluşturduğunuzda sınıfı ve görünüm şablonları.

İçinde *Controllers\MoviesController.cs* dosyasını inceleyin oluşturulan `Details` yöntemi. Film denetleyicisiyle bir kısmı `Details` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Varsa bir `Movie` bulunursa, bir örneğini `Movie` modeli Ayrıntılar görünümüne geçirilir. İçeriğini incelemek *Views\Movies\Details.cshtml* dosya.

Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünüm şablonu görünümü bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Details.cshtml* şablonu, kodu her film alanına geçirir `DisplayNameFor` ve [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Yardımcıları ile güçlü şekilde yazılan `Model` nesnesi. Ayrıca oluşturma ve düzenleme yöntemleri ve görünüm şablonları film model nesnesi geçirin.

İncelemek *Index.cshtml* şablonu görüntüleme ve `Index` yönteminde *MoviesController.cs* dosya. Kodu nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` denetleyicisinden listesini görüntülemek için:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Film denetleyicisini oluşturduğunuzda, Visual Studio Express otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Index.cshtml* şablon kodu döngüler filmler yaparak bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde IntelliSense desteği tam anlamına gelir:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server yerel veritabanı ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi için işaret bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olmadığını doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız, *Movies.mdf* dosya, tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** araç, tıklatın **yenileme** düğmesine tıklayın ve ardından *uygulama\_veri* klasör.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Çift tıklatın *Movies.mdf* açmak için **DATABASE EXPLORER**,'ni genişletin **tabloları** klasörü filmler tabloya bakın.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Veritabanı explorer görünmüyorsa, gelen **Araçları** menüsünde, select **veritabanına bağlan**, sonra iptal **veri kaynağı Seç** iletişim. Bu veritabanı explorer açık zorlar.


> [!NOTE]
> VWD veya Visual Studio 2010 kullanarak ve aşağıdaki aşağıdakilerden herhangi birini benzer bir hata iletisi varsa:
> 
> - Veritabanı ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' sürümü 706 olduğundan açılamıyor. Bu sunucunun sürümü 655 ve önceki sürümleri destekler. İndirgeme yolu desteklenmez.
> - &quot;InvalidOperation özel durum, kullanıcı kodu tarafından işlenmemiş&quot; sağlanan SqlConnection bir ilk katalog belirtmiyor.
> 
> Yüklemenize gerek [SQL Server veri Araçları](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) ve [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Doğrulama `MovieDBContext` önceki sayfada belirtilen bağlantı dizesi.


Sağ `Movies` tablo ve seçin **Show Table Data** oluşturduğunuz verileri görmek için.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Sağ `Movies` tablo ve seçin **açık tablo tanımı** bu Entity Framework kod sizin için oluşturduğu ilk yapısı tablosunu görmek için.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Bildirim nasıl şeması `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şemayı dayanarak için `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı sağ tıklayarak kapatın *MovieDBContext* ve seçerek **kapatmak bağlantı**. (Bağlantı kapatmayın, projenin bir sonraki çalıştırmanızda bir hata alabilirsiniz).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Artık bu içeriği görüntülemek için bir basit liste sayfası ve veritabanı vardır. Sonraki öğreticide biz kurulmuş kodu kalan inceleyin ve ekleme bir `SearchIndex` yöntemi ve `SearchIndex` bu veritabanında filmler arama yapmanıza olanak tanıyan görünümü.

> [!div class="step-by-step"]
> [Önceki](adding-a-model.md)
> [sonraki](examining-the-edit-methods-and-edit-view.md)
