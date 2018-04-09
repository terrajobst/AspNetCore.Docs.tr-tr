---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Bir denetleyicisinden modelinizin verilere | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyicisinden modelinizin verilerine erişme
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, yeni oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak tarayıcıda görüntüleyen kod yazın.

**Uygulamayı derleme** sonraki adıma geçmeden önce. Uygulamayı yapılandırdıysanız olmayan bir denetleyici eklenirken bir hata alırsınız.

Çözüm Gezgini'nde sağ *denetleyicileri* klasörünü ve ardından **Ekle**, ardından **denetleyicisi**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

İçinde **İskele Ekle** iletişim kutusu, tıklatın **Entity Framework kullanarak MVC 5 denetleyici, görünümleri olan**ve ardından **Ekle**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Seçin **film (MvcMovie.Models)** Model sınıfı için.
- Seçin **MovieDBContext (MvcMovie.Models)** veri bağlamı sınıfı için.
- Denetleyici adı **MoviesController**.

  Aşağıdaki görüntü, tamamlanan iletişim kutusunu gösterir.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

**Ekle**'yi tıklatın. (Bir hata alırsanız, büyük olasılıkla denetleyicisi ekleme başlatmadan önce uygulamayı oluşturabilirsiniz alamadık.) Visual Studio, aşağıdaki dosyaları ve klasörleri oluşturur:

- *Bir MoviesController.cs* dosyasını *denetleyicileri* klasör.
- A *Views\Movies* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

Visual Studio otomatik olarak oluşturulan [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümleri (CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir). Artık oluşturma listesinde, düzenleme ve film girişleri silmek olanak sağlayan bir tam olarak işlevsel bir web uygulaması sahipsiniz.

Uygulamayı çalıştırın ve tıklayın **MVC film** bağlantısı (veya Gözat `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye). Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *uygulama\_Start\RouteConfig.cs* dosyası), tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilen `Index` Eylemyöntemi`Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bir filmi bazı ayrıntılarını girin ve ardından **oluşturma** düğmesi.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Ondalık ayırıcıların veya virgül Fiyat alanına girmeniz mümkün olmayabilir. bir virgül İngilizce dışındaki yerel ayarlar için jQuery doğrulamasına desteklemek için (&quot;,&quot;) ondalık ve ABD İngilizcesi dışındaki tarih biçimleri için içermelidir *globalize.js* ve özel  *cultures/globalize.cultures.js* dosyası (gelen [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. Sonraki öğreticide bunu nasıl göster. Şimdilik, yalnızca tam sayılar 10 gibi girin.


Tıklatarak **oluşturma** düğmesi sunucuya film bilgileri veritabanında kaydedildiği postalama forma neden olur. Ardından için yönlendirilirsiniz */Movies* burada görebilirsiniz listesinde yeni oluşturulan film URL.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Daha fazla birkaç film girişleri oluşturur. Deneyin **Düzenle**, **ayrıntıları**, ve **silmek** tüm işlev bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyicisiyle bir kısmı `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Bir istek `Movies` denetleyicisi döndürür tüm girişler `Movies` tablo ve sonuçları geçirir `Index` görünümü. Aşağıdaki satır `MoviesController` sınıfı, daha önce açıklandığı gibi bir filmi veritabanı bağlamını başlatır. Film veritabanı bağlamı, sorgu, düzenlemek ve filmler silmek için kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelleri'kesin türü belirtilmiş ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneler görünümü kullanarak şablonu geçirebilirsiniz gördüğünüzü `ViewBag` nesnesi. `ViewBag` Bir görünüme bilgi geçirmek için kullanışlı bir geç bağlama yol sağlayan dinamik bir nesnedir.

MVC geçirmek olanağı da sağlar *kesinlikle* belirlenmiş nesnelerin şablonu görüntüleme için. Kesin türü belirtilmiş bu yaklaşım daha iyi derleme zamanı kodunuzun denetleme ve daha zengin sağlar [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio düzenleyicisinde. Bu yaklaşım Visual Studio yapı iskelesi yönteminde kullanılan (diğer bir deyişle, geçirme bir *kesinlikle* yazılı modeli) ile `MoviesController` yöntemleri ve görünümler oluşturduğunuzda sınıfı ve görünüm şablonları.

İçinde *Controllers\MoviesController.cs* dosyasını inceleyin oluşturulan `Details` yöntemi. `Details` Yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametre genellikle geçirilir rota verileri, örneğin `http://localhost:1234/movies/details/1` denetleyicisi film denetleyici için eylem ayarlar `details` ve `id` 1. Ayrıca, bir sorgu dizesi ile kimliği gibi geçirebilirdiniz:

`http://localhost:1234/movies/details?id=1`

Varsa bir `Movie` bulunursa, bir örneğini `Movie` modeli iletilir `Details` görünümü:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

İçeriğini incelemek *Views\Movies\Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Ekleyerek bir `@model` deyimini dosyanın üst kısmındaki görünüm şablonu görünümü bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Details.cshtml* şablonu, kodu her film alanına geçirir `DisplayNameFor` ve [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Yardımcıları ile güçlü şekilde yazılan `Model` nesnesi. `Create` Ve `Edit` yöntemleri ve görünüm şablonları da film model nesnesi geçirin.

İncelemek *Index.cshtml* şablonu görüntüleme ve `Index` yönteminde *MoviesController.cs* dosya. Kodu nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` dan listesinde `Index` eylem yöntemi görüntülemek için:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Film denetleyicisini oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` deyimi en üstündeki *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen filmler listesi erişmenize olanak sağlayan bir `Model` kesin türü belirtilmiş nesnesi. Örneğin, *Index.cshtml* şablon kodu döngüler filmler yaparak bir `foreach` kesin türü belirtilmiş üzerinden deyimi `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Çünkü `Model` nesne kesin türü belirtilmiş (olarak bir `IEnumerable<Movie>` nesnesi), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde IntelliSense desteği tam anlamına gelir:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server yerel veritabanı ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi için işaret bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olmadığını doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız, *Movies.mdf* dosya, tıklatın **tüm dosyaları göster** düğmesini **Çözüm Gezgini** araç, tıklatın **yenileme** düğmesine tıklayın ve ardından *uygulama\_veri* klasör.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Çift tıklatın *Movies.mdf* açmak için **sunucu GEZGİNİ**, ardından genişletin **tabloları** klasörü filmler tabloya bakın. Kimliği yanındaki anahtar simgesine unutmayın Varsayılan olarak, birincil anahtar kimliği adlı bir özellik EF hale getirir. EF ve MVC hakkında daha fazla bilgi için zel Dykstra'nın mükemmel bkz [MVC ve EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Sağ `Movies` tablo ve seçin **Show Table Data** oluşturduğunuz verileri görmek için.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Sağ `Movies` tablo ve seçin **açık tablo tanımı** bu Entity Framework kod sizin için oluşturduğu ilk yapısı tablosunu görmek için.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Bildirim nasıl şeması `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şemayı dayanarak için `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı sağ tıklayarak kapatın *MovieDBContext* ve seçerek **kapatmak bağlantı**. (Bağlantı kapatmayın, projenin bir sonraki çalıştırmanızda bir hata alabilirsiniz).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Bir veritabanı ve görüntüleme, düzenleme, güncelleştirme ve verileri silmek için sayfaları artık sahipsiniz. Sonraki öğreticide biz kurulmuş kodu kalan inceleyin ve ekleme bir `SearchIndex` yöntemi ve `SearchIndex` bu veritabanında filmler arama yapmanıza olanak tanıyan görünümü. Entity Framework MVC ile kullanma hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC uygulaması için Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Önceki](creating-a-connection-string.md)
> [sonraki](examining-the-edit-methods-and-edit-view.md)
