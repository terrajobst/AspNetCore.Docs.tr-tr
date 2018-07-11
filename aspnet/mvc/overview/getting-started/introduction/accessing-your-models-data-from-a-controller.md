---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Bir denetleyiciden modelinizin verilerine erişme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: a3f3f4a030650ff65b070528c5efa1605be764a0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817557"
---
<a name="accessing-your-models-data-from-a-controller"></a>Bir denetleyiciden modelinizin verilerine erişme
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Bu bölümde, yeni bir oluşturacağınız `MoviesController` sınıfı ve film verileri alır ve bir görünüm şablonu kullanarak bir tarayıcıda görüntüleyen kod yazın.

**Uygulamayı derlemek** sonraki adıma geçmeden önce. Uygulama oluşturuyorsanız yoksa, bir denetleyici eklenirken bir hata alırsınız.

Çözüm Gezgini'nde sağ *denetleyicileri* klasörünü ve ardından **Ekle**, ardından **denetleyicisi**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

İçinde **İskele Ekle** iletişim kutusu, tıklayın **MVC 5 denetleyici Entity Framework kullanarak görünümler ile**ve ardından **Ekle**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Seçin **film (MvcMovie.Models)** Model sınıfı için.
- Seçin **MovieDBContext (MvcMovie.Models)** veri bağlamı sınıfı.
- Denetleyici adını girin **MoviesController**.

  Aşağıdaki resimde tamamlanmış iletişim kutusunu gösterir.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

**Ekle**'yi tıklatın. (Bir hata alırsanız, büyük olasılıkla denetleyicisi ekleme başlatmadan önce uygulamayı oluşturabilirsiniz kaydetmedi.) Visual Studio, aşağıdaki dosya ve klasörleri oluşturur:

- *Bir MoviesController.cs* dosyası *denetleyicileri* klasör.
- A *Views\Movies* klasör.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, ve *Index.cshtml* yeni *Views\Movies* klasör.

Visual Studio otomatik olarak oluşturulan [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (oluşturma, okuma, güncelleştirme ve silme) eylem yöntemleri ve sizin için görünümler (CRUD eylem yöntemleri ve görünümler otomatik olarak oluşturulmasını yapı iskelesi olarak bilinir). Artık oluşturmak, listesinde, düzenlemek ve film girdileri Sil olanak sağlayan tam olarak işlevsel bir web uygulamanız var.

Uygulamayı çalıştırmak ve tıklayın **MVC film** bağlantısına (veya göz atın `Movies` ekleyerek denetleyicisi */Movies* tarayıcınızın adres çubuğundaki URL'ye). Varsayılan yönlendirme uygulama bağlı olduğundan (tanımlanan *uygulama\_Start\RouteConfig.cs* dosyası), bir tarayıcı isteğini `http://localhost:xxxxx/Movies` varsayılan yönlendirilir `Index` Eylemyöntemi`Movies` denetleyicisi. Diğer bir deyişle, tarayıcı isteğini `http://localhost:xxxxx/Movies` tarayıcı isteğini etkili bir şekilde aynıdır `http://localhost:xxxxx/Movies/Index`. Sonuç boş bir liste film, çünkü herhangi henüz eklemediniz.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Bir filmi oluşturma

Seçin **Yeni Oluştur** bağlantı. Bazı film ayrıntılarını girin ve ardından **Oluştur** düğmesi.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Ondalık nokta veya virgül Fiyat alanına girmeniz mümkün olmayabilir. JQuery doğrulama virgül İngilizce olmayan yerel ayara yönelik desteği için (&quot;,&quot;) ondalık ve ABD İngilizce olmayan tarih biçimleri için içermelidir *globalize.js* ve size özgü  *cultures/globalize.cultures.js* dosyası (gelen [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) ve kullanmak için JavaScript'i `Globalize.parseFloat`. Ben sonraki öğreticide bunun nasıl yapılacağını göstereceğiz. Şimdilik yalnızca 10 gibi tam sayı girin.


Tıklayarak **Oluştur** düğmeyi formun film bilgileri veritabanında kaydedildiği sunucuya gönderilecek neden olur. Ardından için yönlendirilirsiniz */Movies* listesinde yeni oluşturulan film görebileceğiniz URL.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Birkaç daha fazla film girişi oluşturun. Deneyin **Düzenle**, **ayrıntıları**, ve **Sil** tüm işlevsel bağlantıları.

## <a name="examining-the-generated-code"></a>Oluşturulan kod İnceleme

Açık *Controllers\MoviesController.cs* dosya ve oluşturulan inceleyin `Index` yöntemi. Film denetleyiciyle bir kısmını `Index` yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Bir istek `Movies` denetleyicisi tüm girdileri döndürür `Movies` tablosunu ve ardından sonuçları geçirir `Index` görünümü. Aşağıdaki satırı gelen `MoviesController` sınıf, daha önce açıklandığı gibi bir film veritabanı bağlamı oluşturur. Sorgulama, Düzenle ve Sil filmler film veritabanı bağlamı'nı kullanabilirsiniz.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Kesin olarak modelleri ve @model anahtar sözcüğü

Bu öğreticide daha önce nasıl bir denetleyici veri veya nesneleri kullanarak bir görünüm şablonu geçirebilirsiniz gördüğünüz `ViewBag` nesne. `ViewBag` Görünümüne bilgi geçirmek için kullanışlı bir geç bağlanan yol sağlayan dinamik bir nesnedir.

MVC geçirmek olanağı da sağlar *kesin* yazılan nesneler için bir şablonu görüntüleme. Bu türü kesin belirlenmiş bir yaklaşım sağlayan daha iyi derleme zamanı denetimi kodunuzun ve daha zengin [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) Visual Studio düzenleyicisinde. Bu yaklaşım Visual Studio yapı iskelesi yönteminde kullanılan (diğer bir deyişle, geçirerek bir *kesin* belirlenmiş model) ile `MoviesController` metotları ve görünümleri oluştururken sınıfı ve görünüm şablonları.

İçinde *Controllers\MoviesController.cs* dosyasını inceleyin oluşturulan `Details` yöntemi. `Details` Yöntemi aşağıda gösterilmektedir.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

`id` Parametresi genellikle geçirilen rota verileri, örneğin `http://localhost:1234/movies/details/1` denetleyici film denetleyici, eylem için ayarlar `details` ve `id` 1. Ayrıca, bir sorgu dizesi ile kimliği şu şekilde çağrılsaydı:

`http://localhost:1234/movies/details?id=1`

Varsa bir `Movie` bulunduğunda, bir örneğini `Movie` modeline geçirilir `Details` görüntüle:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

İçeriğini incelemek *Views\Movies\Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Ekleyerek bir `@model` deyimi görünümü şablon dosyasının üst görünüm bekliyor nesne türünü belirtebilirsiniz. Film denetleyicisi oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` en üstündeki deyimi *Details.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Bu `@model` yönergesi kullanarak denetleyici görünüm tarafından geçirilen film erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Details.cshtml* şablonu, kodu her film alanına geçirir `DisplayNameFor` ve [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Yardımcıları ile kesin olarak belirlenmiş `Model` nesne. `Create` Ve `Edit` yöntemleri ve görünüm şablonları da film model nesnesi geçirin.

İnceleme *Index.cshtml* şablonu görüntüleme ve `Index` yönteminde *MoviesController.cs* dosya. Kodun nasıl oluşturduğunu fark bir [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) nesne çağırdığında `View` yardımcı yönteminin `Index` eylem yöntemi. Kodu daha sonra bu geçirir `Movies` gelen listesinde `Index` eylem yöntemine görünümü:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Film denetleyicisi oluşturduğunuzda, Visual Studio otomatik olarak aşağıdaki dahil `@model` en üstündeki deyimi *Index.cshtml* dosyası:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Bu `@model` yönergesi kullanarak görünüm tarafından geçirilen denetleyici filmler listesini erişmenize olanak sağlayan bir `Model` türü kesin belirlenmiş bir nesne. Örneğin, *Index.cshtml* şablonu, kod döngüsü film gerçekleştirerek bir `foreach` deyimi kesin olarak belirlenmiş üzerinden `Model` nesnesi:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Çünkü `Model` nesne türü kesin belirlenmiş (olarak bir `IEnumerable<Movie>` nesne), her `item` döngüsünde nesne türü olarak `Movie`. Diğer avantajlar arasında bu kod derleme zamanı denetimi Al ve kod düzenleyicisinde, IntelliSense desteği tam anlamına gelir:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>SQL Server LocalDB ile çalışma

Entity Framework Code First algılanan sağlanan veritabanı bağlantı dizesi işaret eden bir `Movies` Code First veritabanı otomatik olarak oluşturulan, henüz yoksa veritabanı. Bakarak oluşturulduktan olduğunu doğrulayabilirsiniz *uygulama\_veri* klasör. Görmüyorsanız *Movies.mdf* dosyasına sağ tıklayıp **tüm dosyaları göster** düğmesine **Çözüm Gezgini** araç çubuğunda tıklatın **Yenile** düğmesini ve ardından *uygulama\_veri* klasör.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Çift *Movies.mdf* açmak için **sunucu GEZGİNİ**, genişletin **tabloları** klasörü filmler tabloya bakın. Not kimliği yanında anahtar simgesi Varsayılan olarak EF kimliği birincil anahtarını adlı bir özellik hale getirir. EF ve MVC hakkında daha fazla bilgi için Tom Dykstra'nın mükemmel öğreticiye bakın [MVC ve EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Sağ `Movies` tablosunu seçip **tablo verilerini Göster** oluşturduğunuz verileri görmek için.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Sağ `Movies` tablosunu seçip **açık tablo tanımı** için yapı, Entity Framework kod sizin için oluşturulan ilk tabloya bakın.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Bildirim nasıl şemasını `Movies` tablo eşlenir `Movie` daha önce oluşturduğunuz sınıfı. Entity Framework Code First otomatik olarak oluşturulan bu şema için dayanarak, `Movie` sınıfı.

İşlemi tamamladığınızda, bağlantıyı sağ tıklayarak kapatın *MovieDBContext* seçerek **kapatmak bağlantı**. (Bağlantı kapatmayın, projeyi bir sonraki çalıştırmanızda hata alabilirsiniz).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Artık bir veritabanı ve görüntülemek, düzenlemek, güncelleştirme ve verileri silmek için sayfa vardır. Sonraki öğreticide, biz iskele kurulan kodu geri kalanını inceleyin ve ekleme bir `SearchIndex` yöntemi ve bir `SearchIndex` filmler bu veritabanındaki aramanıza olanak tanıyan bir görünüm. MVC ile Entity Framework kullanma ile ilgili daha fazla bilgi için bkz: [bir ASP.NET MVC uygulaması için bir Entity Framework veri modeli oluşturma](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Önceki](creating-a-connection-string.md)
> [İleri](examining-the-edit-methods-and-edit-view.md)
