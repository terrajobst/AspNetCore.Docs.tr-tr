---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Bir denetleyiciden modelinizin verilerine erişme | Microsoft Docs
author: Rick-Anderson
description: 'Not: Bu öğretici güncelleştirilmiş bir sürümünü burada ASP.NET MVC 5 ve Visual Studio 2013 kullanan kullanılabilir. Bu, daha güvenli ve izleyin ve tanıtım çok daha kolay...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2b5747f49a31a6f3559609bbae765025e43c650b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755319"
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyiciden modelinizin verilerine erişme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Bu öğreticide güncelleştirilmiş bir sürümü kullanılabilir [burada](../../getting-started/introduction/getting-started.md) ASP.NET MVC 5 ve Visual Studio 2013'ü kullanır. Bu, daha güvenli ve izlemek çok daha kolay ve daha fazla özelliklerini gösterir.


Bu bölümde, yeni bir oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak bir tarayıcıda görüntüleyen kod yazın.

**Uygulamayı derlemek** sonraki adıma geçmeden önce.

Sağ *denetleyicileri* klasörü ve yeni bir `MoviesController` denetleyicisi. Aşağıdaki seçenekler, uygulama oluşturmaya kadar görünmez. Aşağıdaki seçenekleri belirleyin:

- Denetleyici adı: **MoviesController**. (Bu varsayılan değerdir. )
- Şablonu: **okuma/yazma eylemleri ve görünümler ile MVC Entity Framework kullanarak denetleyicisi**.
- Model sınıfı: **film (MvcMovie.Models)**.
- Veri bağlamı sınıfı: **MovieDBContext (MvcMovie.Models)**.
- Görünümler: **Razor (CSHTML)**. (Varsayılan)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

**Ekle**'yi tıklatın. Visual Studio Express, aşağıdaki dosya ve klasörleri oluşturur:

- *Bir MoviesController.cs* proje dosyasında *denetleyicileri* klasör.
- A *filmler* proje klasöründe *görünümleri* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

ASP.NET MVC 4 otomatik olarak oluşturulan CRUD (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümler (CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir). Artık oluşturmak, listesinde, düzenlemek ve film girdileri Sil olanak sağlayan tam olarak işlevsel bir web uygulamanız var.

Uygulamayı çalıştırmak ve göz atın `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL. Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *Global.asax* dosyası), bir tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilir `Index` eylem yöntemi `Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü herhangi henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bazı film ayrıntılarını girin ve ardından **Oluştur** düğmesi.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Tıklayarak **Oluştur** düğmeyi formun film bilgileri veritabanında kaydedildiği sunucuya gönderilecek neden olur. Ardından için yönlendirilirsiniz */Movies* listesinde yeni oluşturulan film görebileceğiniz URL.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Birkaç daha fazla film girişi oluşturun. Deneyin **Düzenle**, **ayrıntıları**, ve **Sil** tüm işlevsel bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyiciyle bir kısmını `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Aşağıdaki satırı gelen `MoviesController` sınıf, daha önce açıklandığı gibi bir film veritabanı bağlamı oluşturur. Sorgulama, Düzenle ve Sil filmler film veritabanı bağlamı'nı kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Bir istek `Movies` denetleyicisi tüm girdileri döndürür `Movies` film veritabanı tablosunu ve ardından sonuçları geçirir `Index` görünümü.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Kesin olarak modelleri ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm şablonu geçirebilirsiniz gördüğünüz `ViewBag` nesne. `ViewBag` Görünümüne bilgi geçirmek için kullanışlı bir geç bağlanan yol sağlayan dinamik bir nesnedir.

ASP.NET MVC, veri ya da bir görünüm şablonu nesnelere kesin geçirme özelliği yazılan de sağlar. Bu yaklaşım etkinleştirir daha iyi kod ve Visual Studio düzenleyicisinde zengin IntelliSense denetleme zamanı kesin. Bu yaklaşım ile Visual Studio yapı iskelesi yönteminde kullanılan `MoviesController` metotları ve görünümleri oluştururken sınıfı ve görünüm şablonları.

İçinde *Controllers\MoviesController.cs* dosyasını inceleyin oluşturulan `Details` yöntemi. Film denetleyiciyle bir kısmını `Details` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Varsa bir `Movie` bulunduğunda, bir örneğini `Movie` model ayrıntıları görünümüne geçirilir. İçeriğini incelemek *Views\Movies\Details.cshtml* dosya.

Ekleyerek bir `@model` deyimi görünümü şablon dosyasının üst görünüm bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisi oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` en üstündeki deyimi *Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Details.cshtml* şablonu, kodu her film alanına geçirir `DisplayNameFor` ve [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne. Görüntüleme şablonları ve oluşturma ve düzenleme yöntemleri Ayrıca film model nesnesi geçirin.

İnceleme *Index.cshtml* şablonu görüntüleme ve `Index` yönteminde *MoviesController.cs* dosya. Kodun nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` denetleyicisinden liste görünümüne:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Film denetleyicisi oluşturduğunuzda, Visual Studio Express otomatik olarak aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Bu `@model` yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Index.cshtml* şablonu, kod döngüsü film gerçekleştirerek bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde, IntelliSense desteği tam anlamına gelir:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi işaret eden bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olduğunu doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız *Movies.mdf* dosyasına sağ tıklayıp **tüm dosyaları göster** düğmesine **Çözüm Gezgini** araç çubuğunda tıklatın **Yenile** düğmesini ve ardından *uygulama\_veri* klasör.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Çift *Movies.mdf* açmak için **veritabanı GEZGİNİ**, genişletin **tabloları** klasörü filmler tabloya bakın.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Veritabanı Gezgini görünmüyorsa, gelen **Araçları** menüsünde **veritabanına bağlan**, daha sonra iptal **veri kaynağı Seç** iletişim. Bu veritabanı Gezgini açık zorlar.


> [!NOTE]
> VWD veya Visual Studio 2010 kullanıyorsanız ve aşağıdaki aşağıdakilerden herhangi birini benzer bir hata iletisi varsa:
> 
> - Veritabanı ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' 706 sürümü olduğundan açılamıyor. Bu sunucu sürümü 655 ve önceki sürümleri destekler. İndirgeme yolu desteklenmez.
> - &quot;Invalidoperation özel durum, kullanıcı kodu tarafından işlenmemiş&quot; sağlanan SqlConnection bir ilk katalog belirtmiyor.
> 
> Yüklemeniz gereken [SQL Server veri Araçları](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) ve [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Doğrulama `MovieDBContext` önceki sayfada belirtilen bağlantı dizesi.


Sağ `Movies` tablosunu seçip **tablo verilerini Göster** oluşturduğunuz verileri görmek için.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Sağ `Movies` tablosunu seçip **açık tablo tanımı** için yapı, Entity Framework kod sizin için oluşturulan ilk tabloya bakın.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Bildirim nasıl şemasını `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şema için dayanarak, `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı sağ tıklayarak kapatın *MovieDBContext* seçerek **kapatmak bağlantı**. (Bağlantı kapatmayın, projeyi bir sonraki çalıştırmanızda hata alabilirsiniz).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Artık veritabanı ve bu içeriği görüntülemek için bir basit listesi sayfası vardır. Sonraki öğreticide, biz iskele kurulan kodu geri kalanını inceleyin ve ekleme bir `SearchIndex` yöntemi ve bir `SearchIndex` filmler bu veritabanındaki aramanıza olanak tanıyan bir görünüm.

> [!div class="step-by-step"]
> [Önceki](adding-a-model.md)
> [İleri](examining-the-edit-methods-and-edit-view.md)
