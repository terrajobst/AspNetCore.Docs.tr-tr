---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: LINQ to SQL (C#) ile model sınıfları oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model c oluşturmayı öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: 01a8443326807ff784999288761de85b6ea976b9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395366"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>LINQ to SQL (C#) ile model sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model sınıfları oluşturma ve Microsoft LINQ SQL yararlanarak veritabanı erişimi gerçekleştirme konusunda bilgi edinin.


Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model sınıfları oluşturma ve Microsoft LINQ SQL yararlanarak veritabanı erişimi gerçekleştirme hakkında bilgi edinin

Bu öğreticide, temel bir film veritabanı uygulaması ekleriz. Biz film veritabanı uygulaması en hızlı ve kolay bir şekilde olası oluşturarak başlayın. Tüm müşterilerimizin veri erişimi bizim denetleyici eylemlerine doğrudan gerçekleştiririz.

Ardından, depo deseni kullanmayı öğrenin. Depo deseni kullanılarak biraz daha fazla iş gerektirir. Ancak, bu Düzen'nu benimsemenin avantajı için uyarlanabilir uygulamalar oluşturmanıza imkan sağlayan, değiştirme ve kolayca sınanabilir.

## <a name="what-is-a-model-class"></a>Bir Model sınıfı nedir?

Bir MVC model tüm bir MVC görünümü veya MVC denetleyicisi bulunmayan bir uygulama mantığı içerir. Özellikle, bir MVC modeli, tüm uygulama iş ve veri erişim mantığı içerir.

Veri erişim mantığı kullanmak, çeşitli farklı teknolojileri kullanabilirsiniz. Örneğin, Microsoft Entity Framework, NHibernate, Subsonic veya ADO.NET sınıflarını kullanarak, veri erişim sınıfları oluşturabilirsiniz.

Bu öğreticide, LINQ to SQL sorgulama ve veritabanını güncelleştirmek için kullanıyorum. LINQ to SQL ile bir Microsoft SQL Server veritabanı ile etkileşim kurmaya çok çok kolay bir yöntemini sağlar. Ancak, ASP.NET MVC çerçevesi LINQ to SQL için herhangi bir şekilde bağlı değil olduğunu anlamak önemlidir. ASP.NET MVC, tüm veri erişim teknolojisi ile uyumludur.

## <a name="create-a-movie-database"></a>Bir film veritabanı oluşturma

Bu öğreticide--model sınıfları--nasıl oluşturabileceğinizi göstermek için basit bir film veritabanı uygulaması ekleriz. İlk adım, yeni bir veritabanı oluşturmaktır. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinde veri klasörü **Ekle, yeni öğe**. Seçin **SQL Server veritabanı** şablon MoviesDB.mdf ad verin ve tıklayın **Ekle** (bkz. Şekil 1) düğmesi.


[![Yeni bir SQL Server veritabanı ekleme](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Şekil 01**: yeni bir SQL Server veritabanı ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Yeni veritabanı oluşturduktan sonra veritabanı uygulamasında MoviesDB.mdf dosyasına çift tıklayarak açabilirsiniz\_veri klasörü. MoviesDB.mdf dosyasına çift tıklayarak Sunucu Gezgini penceresini açar (bkz: Şekil 2).

Sunucu Gezgini penceresi Visual Web Developer kullanarak veritabanı Gezgini penceresi çağrılır.


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Şekil 02**: Sunucu Gezgini penceresini kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Bizim filmler temsil eden bir tablo eklemek ihtiyacımız var. Tabloları klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**. Bu menü seçeneği seçildiğinde, Tablo Tasarımcısı açılır (bkz: Şekil 3).


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Şekil 03**: Tablo Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Veritabanı tablomuza aşağıdaki sütunları ekleyin gerekir:

| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Direktörü | nvarchar(50) | False |

Kimlik sütunu için iki özel işlem gerçekleştirmesi gerekir. Öncelikle, Tablo Tasarımcısı'nda sütununu seçip, anahtar simgesine tıklayarak kimlik sütunu birincil anahtar sütunu işaretlemek gerekir. LINQ to SQL gerçekleştirme ekler ya da veritabanında güncelleştirir, birincil anahtar sütunları belirtmenizi gerektirir.

Ardından, değeri Evet atayarak kimlik sütunu bir kimlik sütunu işaretlemek ihtiyacınız **olan kimlik** özelliği (bkz: Şekil 3). Bir kimlik sütunu tabloya yeni bir satır veri eklediğinizde, otomatik olarak yeni bir sayı atanan bir sütundur.

## <a name="create-linq-to-sql-classes"></a>LINQ to SQL sınıfları oluşturma

MVC modelimizi tblMovie veritabanı tablosunu temsil eden SQL sınıflarına LINQ içerir. Bu LINQ to SQL sınıfları oluşturmak için en kolay yolu olan modeller klasörü sağ tıklayın, için **Ekle, yeni öğe**, LINQ to SQL sınıfları şablonu seçin, sınıfları Movie.dbml ad verin ve tıklayın **Ekle**(bkz: Şekil 4) düğmesi.


[![SQL sınıflarına LINQ oluşturma](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Şekil 04**: LINQ to SQL sınıfları oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Hemen film LINQ to SQL sınıfları oluşturduktan sonra Nesne İlişkisel Tasarımcısı görüntülenir. LINQ to SQL belirli veritabanı tablolarını temsil eden sınıfları oluşturmak için Nesne İlişkisel Tasarımcısı Sunucu Gezgini pencereden veritabanı tabloları sürükleyebilirsiniz. Nesne İlişkisel Tasarımcısı tblMovie veritabanı tablosunu eklemek ihtiyacımız (bkz: Şekil 5).


[![Nesne İlişkisel Tasarımcısı kullanma](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Şekil 05**: Nesne İlişkisel Tasarımcısı'nı kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


Varsayılan olarak, Nesne İlişkisel Tasarımcısı tasarımcının üzerine sürükleyerek veritabanı tablosu çok aynı ada sahip bir sınıf oluşturur. Ancak biz çağrı bizim sınıfı istemiyorsanız `tblMovie`. Bu nedenle, Sınıf Tasarımcısı'nda adına tıklayın ve film sınıfın adını değiştirin.

Son olarak, e tıklamayı unutmayın **Kaydet** SQL sınıflarına LINQ kaydetmek için düğme (disket resim). Aksi takdirde, LINQ to SQL sınıfları Nesne İlişkisel Tasarımcısı tarafından oluşturulan olmaz.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Bir denetleyici eylemi LINQ to SQL kullanma

Size sunduğumuz LINQ to SQL sınıfları sahip olduğunuza göre bu sınıflar veritabanından veri almak için kullanabiliriz. Bu bölümde, doğrudan bir denetleyici eylemi içinde SQL sınıflarına LINQ kullanmayı öğrenin. Bir MVC Görünümü'nde film tblMovies veritabanı tablosundan listenin görüntüleyeceğiz.

İlk olarak biz HomeController sınıfı değiştirmeniz gerekir. Bu sınıf, uygulamanızın denetleyicileri klasöründe bulunabilir. Sınıf listesi 1 sınıfında benzer şekilde değiştirin.

**Kod 1 – `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

`Index()` Listeleme 1 eylemini kullanan bir LINQ to SQL Datacontext'e sınıfı ( `MovieDataContext`) temsil etmek için `MoviesDB` veritabanı. `MoveDataContext` Sınıfı, Visual Studio nesne ilişkisel tasarımcısı tarafından oluşturuldu.

LINQ sorgusu tüm film almak için DataContext karşı gerçekleştirilen `tblMovies` veritabanı tablosu. Filmler listesini adlı yerel bir değişkene atanır `movies`. Son olarak, filmler listesi görünüm verilerine görünüme iletilir.

Filmler göstermek için sonraki Index görünümünü değiştirmek gerekiyor. Dizin görünümü'nde bulabilirsiniz `Views\Home\` klasör. Dizin görünümünün güncelleştirin, böylece 2 liste görünümünde gibi görünüyor.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Değiştirilen dizin görünümünün içeren bildirimi bir `<%@ import namespace %>` üst görünümün yönergesi. Bu yönerge aktarır `MvcApplication1.Models namespace`. Bu ad alanı ile çalışmak için ihtiyacımız `model` sınıfları – özellikle `Movie` --görünümünde sınıfı.

2 liste görünümünde içeren bir `foreach` tüm tarafından temsil edilen öğeleri aracılığıyla yinelenir döngü `ViewData.Model` özelliği. Değerini `Title` özelliği her biri için görüntülenir `movie`.

Dikkat değerini `ViewData.Model` özelliği atandığında bir `IEnumerable`. İçeriği döngü için bu gereklidir `ViewData.Model`. Türü kesin belirlenmiş bir oluşturmak için buraya başka bir seçenek olan `view`. Türü kesin belirlenmiş bir oluşturduğunuzda `view`, verdiğiniz `ViewData.Model` özelliğini de bir görünümün arka plan kod sınıfı belirli bir tür.

Değiştirme sonra uygulamayı çalıştırırsanız `HomeController` sınıfı ve Dizin boş bir sayfa olacağı görüntüleyin. Film kayıt yok olduğundan boş bir sayfa elde edecekleriniz `tblMovies` veritabanı tablosu.

Kayıtları eklemek için `tblMovies` veritabanı tablosu, sağ `tblMovies` tablo (veritabanı Gezgini penceresi Visual Web Developer) Sunucu Gezgini penceresinde, veritabanı ve tablo verilerini Göster ' menü seçeneği seçin. Ekleyebileceğiniz `movie` görünür (bkz. Şekil 6) kılavuz kullanarak kaydeder.


[![Filmler ekleme](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Şekil 06**: filmler ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Bazı veritabanı kayıtlarına ekledikten sonra `tblMovies` tablo ve uygulamayı çalıştırır, Şekil 7'deki sayfası görürsünüz. Tüm film veritabanı kayıtlarını madde işaretli listede görüntülenir.


[![Dizin görünümünün filmlerle görüntüleme](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Şekil 07**: dizin görünümünün filmlerle görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Depo düzenini kullanma

Önceki bölümde, LINQ to SQL sınıfları doğrudan bir denetleyici eylemi içinde kullandık. Kullandık `MovieDataContext` doğrudan sınıf `Index()` denetleyici eylemi. Basit bir uygulaması olması durumunda bunu ile yanlış bir şey yoktur. Ancak, daha karmaşık bir uygulama oluşturmak ihtiyacınız olduğunda LINQ to SQL ile doğrudan bir denetleyici sınıfı çalışma sorunları oluşturur.

Bir denetleyici sınıfı içinde LINQ to SQL kullanarak veri erişim teknolojileri ileride geçiş yapmak zorlaştırır. Örneğin, veri erişim teknolojisi olarak Microsoft Entity Framework kullanarak Microsoft LINQ-SQL kullanarak geçiş karar verebilirsiniz. Bu durumda, uygulamanızda veritabanına erişen her denetleyici yeniden gerekecektir.

Bir denetleyici sınıfı içinde LINQ to SQL kullanarak de uygulamanız için birim testleri oluşturmak zorlaştırır. Normalde, birim testleri gerçekleştirirken bir veritabanıyla etkileşime girmek istemezsiniz. Birim testleriniz uygulama mantığınızın ve, veritabanı sunucunuza test etmek için kullanmak istediğiniz.

Bir MVC uygulaması oluşturmak için gelecekteki daha uyarlanabilir değişiklik ve daha bir kolayca sınanabilir, depo deseni kullanmayı düşünmeniz gerekir. Depo düzeni kullandığınızda, tüm veritabanı erişim mantığınızı içeren bir ayrı bir depo sınıfına oluşturun.

Depo sınıfı oluşturduğunuzda, tüm depo sınıfı tarafından kullanılan yöntemleri temsil eden bir arabirim oluşturun. Denetleyicilerinizi içinde arabirim depo yerine kodunuzu yazın. Böylece, gelecekte farklı veri erişim teknolojileri kullanarak depoyu uygulayabilir.

Arabirim listeleme 3'te adlı `IMovieRepository` adlı tek bir yöntemi temsil eder `ListAll()`.

**Kod 3 – `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

Depo sınıfını listeleme 4'te uygulayan `IMovieRepository` arabirimi. Adlı bir yöntemi içerdiğine dikkat edin `ListAll()` karşılık gelen gereken yöntemini `IMovieRepository` arabirimi.

**4 listeleme – `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Son olarak, `MoviesController` listeleme 5 sınıfında kullanır depo deseni. Artık LINQ SQL sınıflarına doğrudan kullanır.

**5 listeleme – `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Dikkat `MoviesController` listeleme 5 sınıfında iki Oluşturucu vardır. Uygulamanız çalışırken ilk Oluşturucu, parametresiz bir oluşturucu çağrılır. Bu oluşturucu örneği oluşturur `MovieRepository` sınıfı ve ikinci oluşturucuya geçirir.

İkinci oluşturucu tek bir parametreye sahiptir: bir `IMovieRepository` parametresi. Bu oluşturucu yalnızca adlı bir sınıf seviyesi alanını parametresinin değeri atar `_repository`.

`MoviesController` Sınıfı, bağımlılık ekleme desenini adlı bir yazılım tasarım deseni avantajlarından sürüyor. Özellikle, bir oluşturucu bağımlılık ekleme adlı kullanıyor. Daha fazla bilgi edinebilirsiniz Martin Fowler tarafından şu makaleyi okuyarak bu desen hakkında:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Dikkat tüm kodun `MoviesController` sınıfı (hariç, ilk Oluşturucu) etkileşim `IMovieRepository` arabirimi yerine gerçek `MovieRepository` sınıfı. Kod, soyut bir arabirim yerine arabirimin somut bir uygulama ile etkileşim kurar.

Uygulama tarafından kullanılan veri erişim teknolojisi değiştirmek istediğiniz sonra basitçe uygulayabileceğiniz `IMovieRepository` alternatif veritabanı erişimi teknolojisi kullanan bir sınıf arabirimi. Örneğin, aşağıdakileri oluşturabilirsiniz bir `EntityFrameworkMovieRepository` sınıfı veya `SubSonicMovieRepository` sınıfı. Denetleyici sınıfı karşı arabirimi programlanmıştır olduğundan, yeni bir uygulamasını geçirebilirsiniz `IMovieRepository` denetleyiciye sınıfı ve sınıfı çalışmaya devam eder.

Ayrıca, test etmek isterseniz `MoviesController` sahte film depo sınıfına geçirebilirsiniz sonra sınıf `HomeController`. Uygulayabileceğiniz `IMovieRepository` sınıfı yok gerçekten access veritabanı ancak tüm gerekli yöntemlerinden birini içeren bir sınıf ile `IMovieRepository` arabirimi. Böylece, birim testi için `MoviesController` gerçekten gerçek veritabanına erişim olmadan sınıfı.

## <a name="summary"></a>Özet

Bu öğreticinin amacı, Microsoft LINQ SQL yararlanarak MVC model sınıfları nasıl oluşturacağınızı göstermek için oluştu. Biz, bir ASP.NET MVC uygulamasındaki veritabanı verilerini görüntülemek için iki stratejiler incelenir. İlk olarak, biz SQL sınıflarına LINQ oluşturulan ve doğrudan bir denetleyici eylemi içindeki sınıflarda kullanılır. LINQ to SQL sınıfları bir denetleyici içinde kullanarak hızlı bir şekilde sağlar ve kolayca veritabanı bir MVC uygulamasında verileri görüntülemek.

Ardından, veritabanı verilerini görüntülemek için biraz daha zor, ancak daha kesin bulduklarında yolunu inceledik. Biz, depo deseni avantajlarından sürdü ve tüm müşterilerimize veritabanı erişim mantığı ayrı depo sınıfında yerleştirilir. Denetleyicimizin, Kodumuzun bir arabirim yerine bir somut sınıf karşı tüm yazdığımız. Depo düzeni avantajlarından bize bir kolayca veritabanı erişim teknolojileri gelecekte değiştirmek etkinleştirir ve kolayca test bizim denetleyici sınıflarına olanak sağlayan ' dir.

> [!div class="step-by-step"]
> [Önceki](creating-model-classes-with-the-entity-framework-cs.md)
> [İleri](displaying-a-table-of-database-data-cs.md)
