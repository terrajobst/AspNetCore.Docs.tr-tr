---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: LINQ-SQL (C#) ile modeli sınıfları oluşturma | Microsoft Docs
author: microsoft
description: Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model c oluşturmayı öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a56ceb9eab5774906ecc89ce9da570d4f691a82
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2018
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>LINQ-SQL (C#) ile modeli sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, modeli sınıfları oluşturmak ve veritabanı erişimi Microsoft LINQ SQL yararlanarak gerçekleştirmeyi öğrenin.


Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, modeli sınıfları oluşturmak ve veritabanı erişimi Microsoft LINQ SQL yararlanarak gerçekleştirmek öğrenin

Bu öğreticide, temel bir filmi veritabanı uygulaması oluşturun. Film veritabanı uygulama hızlı ve kolay şekilde olası oluşturarak başlayın. Biz bizim veri erişimi tümünün doğrudan sunduğumuz denetleyicisi Eylemler gerçekleştirin.

Ardından, havuz deseni kullanmayı öğrenin. Depo düzeni kullanarak biraz daha fazla iş gerektirir. Ancak, bu deseni benimsenmesi avantajı için uyarlanabilir uygulamalar oluşturmanıza olanak tanır olduğu değiştirin ve kolayca sınanabilir.

## <a name="what-is-a-model-class"></a>Bir Model sınıfı nedir?

Bir MVC model tüm MVC görünümü ya da MVC denetleyicisi bulunmayan uygulama mantığını içerir. Özellikle, bir MVC model tüm uygulama iş ve veri erişim mantığı içerir.

Veri erişim mantığı uygulamak için çeşitli farklı teknolojiler kullanabilirsiniz. Örneğin, Microsoft Entity Framework, NHibernate, Subsonic veya ADO.NET sınıflarını kullanarak, veri erişim sınıfları oluşturabilirsiniz.

Bu öğreticide, ı sorgulamak ve veritabanını güncelleştirmek için LINQ-SQL kullanın. LINQ-SQL, bir Microsoft SQL Server veritabanıyla etkileşim çok çok kolay bir yöntemini sağlar. Ancak, ASP.NET MVC çerçevesi LINQ-SQL için herhangi bir şekilde bağlı değildir olduğunu anlamak önemlidir. ASP.NET MVC, tüm veri erişim teknolojisi ile uyumludur.

## <a name="create-a-movie-database"></a>Film veritabanı oluşturma

Bu öğreticide--modeli sınıfları--nasıl oluşturulacağına göstermek için biz basit bir filmi veritabanı uygulaması oluşturun. İlk adım, yeni bir veritabanı oluşturmaktır. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinin veri klasörü **Ekle, yeni öğe**. Seçin **SQL Server veritabanı** şablonu, MoviesDB.mdf adını verin ve tıklatın **Ekle** (bkz: Şekil 1) düğmesine tıklayın.


[![Yeni bir SQL Server veritabanı ekleme](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Şekil 01**: yeni bir SQL Server veritabanı ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Yeni veritabanı oluşturduktan sonra uygulama MoviesDB.mdf dosyasını çift tıklatarak veritabanını açabilirsiniz\_veri klasörü. MoviesDB.mdf dosyasını çift tıklayarak Sunucu Gezgini penceresini açar (bkz: Şekil 2).

Sunucu Gezgini penceresini Visual Web Developer kullanırken veritabanı Gezgini penceresi adı verilir.


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Şekil 02**: Sunucu Gezgini penceresini kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Bir tablo bizim filmler temsil eden bizim veritabanına eklemek gerekir. Tables klasörünü sağ tıklatın ve menü seçeneğini seçmek **Yeni Tablo Ekle**. Bu menü seçeneği Tablo Tasarımcısı açar (bkz: Şekil 3).


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Şekil 03**: Tablo Tasarımcısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Aşağıdaki sütunlar bizim veritabanı tablosuna eklemek ihtiyacımız var:

| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Director | nvarchar(50) | False |

İki özel ID sütunu için yapmanız gerekir. İlk olarak, Tablo Tasarımcısı'nda sütunun seçilmesi ve bir anahtar simgesine tıklayarak ID sütunu birincil anahtar sütun olarak işaretlemek gerekir. LINQ-SQL gerçekleştirme ekler veya veritabanına karşı güncelleştirdiğinde, birincil anahtar sütunlarının belirtmenizi gerektirir.

Ardından, değeri Evet atayarak ID sütunu bir kimlik sütunu işaretlemek ihtiyacınız **Is Identity** özelliği (bkz: Şekil 3). Bir kimlik sütunu bir tablo için yeni bir veri satırı eklediğinizde, yeni bir sayı otomatik olarak atanan bir sütundur.

## <a name="create-linq-to-sql-classes"></a>LINQ-SQL sınıfları oluşturma

MVC modelimizi LINQ tblMovie veritabanı tablosu temsil eden SQL'e sınıflarını içerir. Bu LINQ'ten SQL'e sınıflarını oluşturmak için en kolay yoludur modeller klasörü sağ tıklatın, seçin **Ekle, yeni öğe**, LINQ to SQL sınıfları şablonu seçin, sınıfları Movie.dbml adını verin ve tıklatın **Ekle**(bkz: Şekil 4) düğmesine tıklayın.


[![LINQ SQL sınıfları için oluşturma](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Şekil 04**: SQL sınıfları için oluşturma LINQ ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Hemen film LINQ-SQL sınıfları oluşturduktan sonra Nesne İlişkisel Tasarımcısı görüntülenir. LINQ-SQL belirli veritabanı tablolarını temsil eden sınıfları oluşturmak için Nesne İlişkisel Tasarımcısı Sunucu Gezgini penceresinden veritabanı tablolarını sürükleyin. Nesne İlişkisel Tasarımcısı üzerine tblMovie veritabanı tablosuna eklemek ihtiyacımız (bkz. Şekil 5).


[![Nesne İlişkisel Tasarımcısı'nı kullanarak](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Şekil 05**: Nesne İlişkisel Tasarımcısı'nı kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Varsayılan olarak, Nesne İlişkisel Tasarımcısı tasarımcısına sürükleyin veritabanı tablosunun çok aynı ada sahip bir sınıf oluşturur. Ancak, biz çağrı bizim sınıfı istemediğiniz `tblMovie`. Bu nedenle, Sınıf Tasarımcısı'nda adına tıklayın ve film sınıfın adını değiştirin.

Son olarak, tıklatın unutmayın **kaydetmek** SQL'e sınıflarını LINQ kaydetmek için (disket resim) düğmesine tıklayın. Aksi takdirde, LINQ to SQL sınıfları Nesne İlişkisel Tasarımcısı tarafından oluşturulan olmaz.

## <a name="using-linq-to-sql-in-a-controller-action"></a>LINQ-SQL kullanarak bir denetleyici eylemi

Bizim LINQ'ten SQL'e sınıflarını sahibiz, biz bu sınıfların veritabanından veri almak için kullanabilirsiniz. Bu bölümde, LINQ için bir denetleyici eylemi içinde doğrudan SQL sınıfların nasıl kullanılacağını öğrenin. Biz tblMovies veritabanı tablosundan filmler listesi MVC görünümünde görüntülersiniz.

İlk olarak, biz HomeController sınıfı değiştirmeniz gerekir. Bu sınıf, uygulamanızın denetleyicileri klasöründe bulunabilir. Sınıfı, listeleme 1 sınıfında benzer şekilde değiştirin.

**Kod 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Listeleme 1 eylemini kullanan bir LINQ to SQL DataContext sınıfı ( `MovieDataContext`) göstermek için `MoviesDB` veritabanı. `MoveDataContext` Sınıfı, Visual Studio nesne ilişkisel tasarımcısı tarafından üretilmiştir.

LINQ Sorgu tüm film almak için DataContext karşı yapılır `tblMovies` veritabanı tablosu. Film listesi adlı yerel bir değişkene atanır `movies`. Son olarak, filmler listesi görünüm verilerine görünümüne geçirilir.

Film gösterebilmeniz biz sonraki dizin görünümünün değiştirmeniz gerekir. Dizin görünümünde bulabilirsiniz `Views\Home\` klasör. Dizin görünümünün listeleme 2 görünümünde benzer şekilde güncelleştirin.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Değiştirilen dizin görünümü içerir bildirimi bir `<%@ import namespace %>` görünümü üstündeki yönergesi. Bu yönerge alır `MvcApplication1.Models namespace`. Bu ad ile çalışması için ihtiyacımız `model` sınıfları – özellikle, `Movie` sınıfında--görünümü.

Listeleme 2 görünümünde içeren bir `foreach` tarafından temsil edilen öğelerin tümünü dolaşır döngü `ViewData.Model` özelliği. Değeri `Title` özelliği her biri için görüntülenir `movie`.

Dikkat değerini `ViewData.Model` özelliği atandığında bir `IEnumerable`. İçeriği döngü için bu gereklidir `ViewData.Model`. Kesin türü belirtilmiş bir oluşturmak için burada başka bir seçenek olan `view`. Kesin türü belirtilmiş bir oluşturduğunuzda `view`, verdiğiniz `ViewData.Model` bir görünümün arka plan kodu sınıfında belirli bir tür özelliği.

Değiştirme sonra uygulama çalıştırırsanız `HomeController` sınıfı ve dizini görüntülemek boş bir sayfa alırsınız. Hiç film kayıt olduğundan boş bir sayfa elde edersiniz `tblMovies` veritabanı tablosu.

Kayıtları eklemek için `tblMovies` veritabanı tablosu, sağ `tblMovies` Veritabanı Sunucu Gezgini penceresinde (Visual Web Developer veritabanı Gezgini penceresinde) tablosu ve tablo verileri göster menü seçeneğini belirleyin. Ekleyebileceğiniz `movie` (bkz. Şekil 6) görünür kılavuz kullanarak kaydeder.


[![Film ekleme](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Şekil 06**: filmler ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Bazı veritabanı kayıtlarını ekledikten sonra `tblMovies` tablo ve uygulamayı çalıştırır, Şekil 7'deki sayfası görürsünüz. Film veritabanı kayıtların tümünü madde işaretli listede görüntülenir.


[![Film dizin görünümünün ile görüntüleme](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Şekil 07**: dizin görünümüyle filmler görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Depo düzeni kullanma

Önceki bölümde SQL sınıflara doğrudan bir denetleyici eylemi içinde LINQ kullanılır. Kullandık `MovieDataContext` doğrudan sınıf `Index()` denetleyici eylemi. Basit bir uygulama durumunda bunu ile yanlış bir şey yoktur. Ancak, daha karmaşık bir uygulama oluşturmak ihtiyacınız olduğunda LINQ-SQL ile doğrudan çalışmak denetleyicisi sınıfında sorunları oluşturur.

LINQ-SQL içinde denetleyici sınıfını kullanarak veri erişim teknolojileri gelecekte geçiş yapmak zorlaştırır. Örneğin, veri erişim teknoloji Microsoft Entity Framework kullanarak Microsoft LINQ-SQL kullanarak geçiş karar verebilirsiniz. Bu durumda, veritabanı, uygulamanızın içinde erişen her bir denetleyicisi yeniden gerekir.

LINQ-SQL içinde denetleyici sınıfını kullanarak da, uygulamanız için birim testleri oluşturmak zorlaştırır. Normalde, birim testleri gerçekleştirirken bir veritabanıyla etkileşim kurmak istemezsiniz. Birim testleri test uygulamanızın mantığı ve olmayan veritabanı sunucunuz için kullanmak istediğiniz.

Bir MVC uygulaması oluşturmak için gelecekteki daha uyarlanabilir değişiklik ve daha kolay sınanabilir, havuz deseni kullanmayı düşünmeniz gerekir. Depo düzeni kullandığınızda, tüm veritabanı erişim mantığı içeren bir ayrı deposu sınıf oluşturun.

Depo sınıfını oluşturduğunuzda, tüm depo sınıfı tarafından kullanılan yöntemleri temsil eden bir arabirim oluşturun. Denetleyicilerinizi içinde kodunuzu depo yerine arabirimi karşı yazın. Bu şekilde, gelecekte farklı veri erişim teknolojileri kullanarak depoyu uygulayabilirsiniz.

Listeleme 3 arabiriminde adlı `IMovieRepository` ve adlı tek bir yöntemi gösterir `ListAll()`.

**Kod 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Depo sınıfını listeleme 4 uygulayan `IMovieRepository` arabirimi. Adlı bir yöntem içerdiğine dikkat edin `ListAll()` karşılık gelen gerektirdiği yöntemini `IMovieRepository` arabirimi.

**4 listeleme – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Son olarak, `MoviesController` listeleme 5 sınıfında havuz deseni kullanır. Artık LINQ SQL'e sınıflarını doğrudan kullanır.

**5 listeleme – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Dikkat `MoviesController` listeleme 5 sınıfında iki Oluşturucusu vardır. Uygulamanızı çalıştırırken parametresiz bir kurucusu ilk Oluşturucusu çağrılır. Bu oluşturucuya bir örneğini oluşturur `MovieRepository` sınıfı ve ikinci oluşturucuya geçirir.

İkinci oluşturucu tek bir parametreye sahiptir: bir `IMovieRepository` parametresi. Bu oluşturucu, parametresinin değeri yalnızca adlı bir sınıf düzeyi alanına atar. `_repository`.

`MoviesController` Sınıfı bağımlılık ekleme düzeni adlı bir yazılım tasarım modelinin avantajı, sürüyor. Özellikle, bir şey Oluşturucusu bağımlılık ekleme adlı kullanıyor. Daha fazla bilgiyi aşağıdaki makaleyi Martin Fowler tarafından okuyarak bu desen hakkında:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Dikkat tüm kod `MoviesController` sınıfı (hariç ilk Oluşturucusu) etkileşime `IMovieRepository` arabirimi gerçek yerine `MovieRepository` sınıfı. Kod arabirimin somut bir uygulama yerine soyut bir arabirim ile etkileşim kurar.

Uygulama tarafından kullanılan veri erişim teknolojisi değiştirmek istediğiniz sonra yalnızca uygulayabileceğiniz `IMovieRepository` alternatif veritabanı erişim teknolojisini kullanan bir sınıfı arabirimi. Örneğin, oluşturabilirsiniz bir `EntityFrameworkMovieRepository` sınıfı veya `SubSonicMovieRepository` sınıfı. Denetleyici sınıfı karşı arabirimi programlanmış olduğundan, yeni bir uyarlamasını geçirebilirsiniz `IMovieRepository` denetleyiciye ve sınıf çalışmaya devam eder.

Ayrıca, test etmek isterseniz `MoviesController` sınıfı bir sahte film depo sınıfını geçirebilirsiniz sonra `HomeController`. Uygulayabileceğiniz `IMovieRepository` yok gerçekte access veritabanı ancak tüm gerekli yöntemlerinden birini içeren bir sınıf sınıfıyla `IMovieRepository` arabirimi. Böylece, birim testi için `MoviesController` gerçekten gerçek bir veritabanına erişirken olmadan sınıfı.

## <a name="summary"></a>Özet

Microsoft LINQ SQL yararlanarak MVC model sınıfları nasıl oluşturabileceğinizi göstermek için bu öğreticinin amacı oluştu. Bir ASP.NET MVC uygulamasında veritabanı veri görüntüleme iki stratejileri bileşen başvuru incelenir. İlk olarak, biz LINQ SQL'e sınıflarını oluşturulur ve doğrudan bir denetleyici eylemi içinde yer alan sınıfları kullanılır. SQL sınıfları için bir denetleyici için LINQ kullanarak hızlı bir şekilde sağlar ve kolayca bir MVC uygulamasında veritabanı veri görüntüleme.

Ardından, veritabanı verilerini görüntülemek için biraz daha zor ancak kesinlikle daha virtuous bir yolu incelediniz. Biz depo modelinin avantajı, sürdü ve tüm veritabanı erişimi mantığımızı ayrı deposu sınıfında yerleştirilir. Bizim denetleyicisi tüm kodumuza arabirimin somut bir sınıf yerine karşı yazdığımız. Depo modelinin avantajı, bize kolayca veritabanı erişim teknolojileri gelecekte değiştirmek etkinleştirir ve bize kolayca bizim denetleyicisi sınıfları test etmek yararlanmanızı sağlamasıdır.

> [!div class="step-by-step"]
> [Önceki](creating-model-classes-with-the-entity-framework-cs.md)
> [sonraki](displaying-a-table-of-database-data-cs.md)
