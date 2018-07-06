---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: LINQ to SQL (VB) ile model sınıfları oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model c oluşturmayı öğrenin...
ms.author: aspnetcontent
ms.date: 10/07/2008
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 2073ef716763f746f315a2131c4aa049bbdeec22
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841360"
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>LINQ to SQL (VB) ile model sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model sınıfları oluşturma ve Microsoft LINQ SQL yararlanarak veritabanı erişimi gerçekleştirme konusunda bilgi edinin.


Bu öğreticide bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model sınıfları oluşturma ve Microsoft LINQ SQL yararlanarak veritabanı erişimi gerçekleştirme konusunda bilgi edinin.

Bu öğreticide, temel bir film veritabanı uygulaması ekleriz. Biz film veritabanı uygulaması en hızlı ve kolay bir şekilde olası oluşturarak başlayın. Tüm müşterilerimizin veri erişimi bizim denetleyici eylemlerine doğrudan gerçekleştiririz.

Ardından, depo deseni kullanmayı öğrenin. Depo deseni kullanılarak biraz daha fazla iş gerektirir. Ancak, bu Düzen'nu benimsemenin avantajı için uyarlanabilir uygulamalar oluşturmanıza imkan sağlayan, değiştirme ve kolayca sınanabilir.

## <a name="what-is-a-model-class"></a>Bir Model sınıfı nedir?

Bir MVC model tüm bir MVC görünümü veya MVC denetleyicisi bulunmayan bir uygulama mantığı içerir. Özellikle, bir MVC modeli, tüm uygulama iş ve veri erişim mantığı içerir.

Veri erişim mantığı kullanmak, çeşitli farklı teknolojileri kullanabilirsiniz. Örneğin, Microsoft Entity Framework, NHibernate, Subsonic veya ADO.NET sınıflarını kullanarak, veri erişim sınıfları oluşturabilirsiniz.

Bu öğreticide, LINQ to SQL sorgulama ve veritabanını güncelleştirmek için kullanıyorum. LINQ to SQL ile bir Microsoft SQL Server veritabanı ile etkileşim kurmaya çok çok kolay bir yöntemini sağlar. Ancak, ASP.NET MVC çerçevesi LINQ to SQL için herhangi bir şekilde bağlı değil olduğunu anlamak önemlidir. ASP.NET MVC, tüm veri erişim teknolojisi ile uyumludur.

## <a name="create-a-movie-database"></a>Bir film veritabanı oluşturma

Bu öğreticide--model sınıfları--nasıl oluşturabileceğinizi göstermek için basit bir film veritabanı uygulaması ekleriz. İlk adım, yeni bir veritabanı oluşturmaktır. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinde veri klasörü **Ekle, yeni öğe**. SQL Server veritabanı şablonu seçin, MoviesDB.mdf ad verin ve tıklayın **Ekle** (bkz. Şekil 1) düğmesi.


[![Yeni bir SQL Server veritabanı ekleme](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Şekil 01**: yeni bir SQL Server veritabanı ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Yeni veritabanı oluşturduktan sonra veritabanı uygulamasında MoviesDB.mdf dosyasına çift tıklayarak açabilirsiniz\_veri klasörü. MoviesDB.mdf dosyasına çift tıklayarak Sunucu Gezgini penceresini açar (bkz: Şekil 2).


|   | Sunucu Gezgini penceresi Visual Web Developer kullanarak veritabanı Gezgini penceresi çağrılır. |
|---|----------------------------------------------------------------------------------------------------|
|   |                                                                                                    |

[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Şekil 02**: Sunucu Gezgini penceresini kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Bizim filmler temsil eden bir tablo eklemek ihtiyacımız var. Tabloları klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**. Bu menü seçeneği seçildiğinde, Tablo Tasarımcısı açılır (bkz: Şekil 3).


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Şekil 03**: Tablo Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Veritabanı tablomuza aşağıdaki sütunları ekleyin gerekir:

| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Direktörü | nvarchar(50) | False |

Kimlik sütunu için iki özel işlem gerçekleştirmesi gerekir. Öncelikle, Tablo Tasarımcısı'nda sütununu seçip, anahtar simgesine tıklayarak kimlik sütunu birincil anahtar sütunu işaretlemek gerekir. LINQ to SQL gerçekleştirme ekler ya da veritabanında güncelleştirir, birincil anahtar sütunları belirtmenizi gerektirir.

Ardından, değeri Evet atayarak kimlik sütunu bir kimlik sütunu işaretlemek ihtiyacınız **olan kimlik** özelliği (bkz: Şekil 3). Bir kimlik sütunu tabloya yeni bir satır veri eklediğinizde, otomatik olarak yeni bir sayı atanan bir sütundur.

Bu değişiklikleri yaptıktan sonra tabloyu adı tblMovie ile kaydedin. Kaydet düğmesine tıklayarak tabloyu kaydedebilirsiniz.

## <a name="create-linq-to-sql-classes"></a>LINQ to SQL sınıfları oluşturma

MVC modelimizi tblMovie veritabanı tablosunu temsil eden SQL sınıflarına LINQ içerir. Bu LINQ to SQL sınıfları oluşturmak için en kolay yolu olan modeller klasörü sağ tıklayın, için **Ekle, yeni öğe**, LINQ to SQL sınıfları şablonu seçin, sınıfları Movie.dbml ad verin ve tıklayın **Ekle**(bkz: Şekil 4) düğmesi.


[![SQL sınıflarına LINQ oluşturma](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Şekil 04**: LINQ to SQL sınıfları oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Hemen film LINQ to SQL sınıfları oluşturduktan sonra Nesne İlişkisel Tasarımcısı görüntülenir. LINQ to SQL belirli veritabanı tablolarını temsil eden sınıfları oluşturmak için Nesne İlişkisel Tasarımcısı Sunucu Gezgini pencereden veritabanı tabloları sürükleyebilirsiniz. Nesne İlişkisel Tasarımcısı tblMovie veritabanı tablosunu eklemek ihtiyacımız (bkz: Şekil 4).


[![Nesne İlişkisel Tasarımcısı kullanma](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Şekil 05**: Nesne İlişkisel Tasarımcısı'nı kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Varsayılan olarak, Nesne İlişkisel Tasarımcısı tasarımcının üzerine sürükleyerek veritabanı tablosu çok aynı ada sahip bir sınıf oluşturur. Ancak biz bizim sınıfı tblMovie çağrılacak istemezsiniz. Bu nedenle, Sınıf Tasarımcısı'nda adına tıklayın ve film sınıfın adını değiştirin.

Son olarak, e tıklamayı unutmayın **Kaydet** SQL sınıflarına LINQ kaydetmek için düğme (disket resim). Aksi takdirde, LINQ to SQL sınıfları Nesne İlişkisel Tasarımcısı tarafından oluşturulan olmaz.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Bir denetleyici eylemi LINQ to SQL kullanma

Size sunduğumuz LINQ to SQL sınıfları sahip olduğunuza göre bu sınıflar veritabanından veri almak için kullanabiliriz. Bu bölümde, doğrudan bir denetleyici eylemi içinde SQL sınıflarına LINQ kullanmayı öğrenin. Bir MVC Görünümü'nde film tblMovies veritabanı tablosundan listenin görüntüleyeceğiz.

İlk olarak biz HomeController sınıfı değiştirmeniz gerekir. Bu sınıf, uygulamanızın denetleyicileri klasöründe bulunabilir. Sınıf listesi 1 sınıfında benzer şekilde değiştirin.

**Kod 1 – `Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

1 listeleme İNDİS() eylemi MoviesDB veritabanı temsil etmek için bir LINQ to SQL Datacontext'e sınıfı (MovieDataContext) kullanır. Visual Studio nesne ilişkisel tasarımcısı tarafından MoveDataContext sınıf oluşturuldu.

LINQ sorgusu, tüm film tblMovies veritabanı tablosundan almak için DataContext karşı yapılır. Filmler listesini filmler adlı yerel bir değişkene atanır. Son olarak, filmler listesi görünüm verilerine görünüme iletilir.

Filmler göstermek için sonraki Index görünümünü değiştirmek gerekiyor. Dizin görünümünün Views\Home\ klasöründe bulabilirsiniz. Dizin görünümünün güncelleştirin, böylece 2 liste görünümünde gibi görünüyor.

**Kod 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Değiştirilen dizin görünümünün içeren bildirimi bir &lt;alma ad alanı % @ %&gt; üst görünümün yönerge. Bu yönerge MvcApplication1 ad alanını içeri aktarır. Film sınıfı--görünümünde model sınıfları ile – özellikle çalışması için bu ad alanı ihtiyacımız var.

2 liste görünümünde bir For içeren her bir döngü tüm ViewData.Model özelliği tarafından temsil edilen öğeleri aracılığıyla yinelenir. Başlık özelliğinin değeri, her filmin için görüntülenir.

ViewData.Model özelliğinin değeri için bir IEnumerable dönüştürüldüğüne dikkat edin. ViewData.Model içeriğini döngü için bu gereklidir. Burada başka bir seçenek, kesin türü belirtilmiş görünüm oluşturmaktır. Kesin türü belirtilmiş Görünüm oluşturduğunuzda, bir görünümün arka plan kod sınıfında ViewData.Model özelliği belirli bir türe dönüştürme.

Ardından HomeController sınıfı ve Index görünümünü değiştirme sonra uygulamayı çalıştırmak, boş bir sayfa alırsınız. TblMovies veritabanı tablosu, film kayıt olduğundan boş bir sayfa elde edersiniz.

Kayıtları tblMovies veritabanı tablosuna eklemek için Sunucu Gezgini penceresini (Visual Web Developer veritabanı Gezgini penceresinde) tblMovies veritabanı tablosuna sağ tıklayın ve menü seçeneğini **tablo verilerini Göster**. Film kayıtları (bkz: Şekil 5) görünen kılavuz kullanarak ekleyebilirsiniz.


[![Filmler ekleme](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Şekil 06**: filmler ekleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Sonra tblMovies tabloya bazı veritabanı kayıtlarını ekleyin ve uygulamayı çalıştırın, Şekil 7'deki sayfası görürsünüz. Tüm film veritabanı kayıtlarını madde işaretli listede görüntülenir.


[![Dizin görünümünün filmlerle görüntüleme](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Şekil 07**: dizin görünümünün filmlerle görüntüleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Depo düzenini kullanma

Önceki bölümde, LINQ to SQL sınıfları doğrudan bir denetleyici eylemi içinde kullandık. MovieDataContext sınıftan doğrudan İNDİS() denetleyici eylemi kullandık. Basit bir uygulaması olması durumunda bunu ile yanlış bir şey yoktur. Ancak, daha karmaşık bir uygulama oluşturmak ihtiyacınız olduğunda LINQ to SQL ile doğrudan bir denetleyici sınıfı çalışma sorunları oluşturur.

Bir denetleyici sınıfı içinde LINQ to SQL kullanarak veri erişim teknolojileri ileride geçiş yapmak zorlaştırır. Örneğin, veri erişim teknolojisi olarak Microsoft Entity Framework kullanarak Microsoft LINQ-SQL kullanarak geçiş karar verebilirsiniz. Bu durumda, uygulamanızda veritabanına erişen her denetleyici yeniden gerekecektir.

Bir denetleyici sınıfı içinde LINQ to SQL kullanarak de uygulamanız için birim testleri oluşturmak zorlaştırır. Normalde, birim testleri gerçekleştirirken bir veritabanıyla etkileşime girmek istemezsiniz. Birim testleriniz uygulama mantığınızın ve, veritabanı sunucunuza test etmek için kullanmak istediğiniz.

Bir MVC uygulaması oluşturmak için gelecekteki daha uyarlanabilir değişiklik ve daha bir kolayca sınanabilir, depo deseni kullanmayı düşünmeniz gerekir. Depo düzeni kullandığınızda, tüm veritabanı erişim mantığınızı içeren bir ayrı bir depo sınıfına oluşturun.

Depo sınıfı oluşturduğunuzda, tüm depo sınıfı tarafından kullanılan yöntemleri temsil eden bir arabirim oluşturun. Denetleyicilerinizi içinde arabirim depo yerine kodunuzu yazın. Böylece, gelecekte farklı veri erişim teknolojileri kullanarak depoyu uygulayabilir.

Listeleme 3 arabiriminde IMovieRepository olarak adlandırılır ve ListAll() adlı tek bir yöntemi gösterir.

**Kod 3 – `Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Depo sınıfını listeleme 4'te IMovieRepository arabirimini uygular. IMovieRepository arabirim tarafından gereken yöntemini karşılık gelen ListAll() adlı bir yöntemi içerdiğine dikkat edin.

**4 listeleme – `Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Son olarak, 5 listeleme MoviesController sınıfında havuz deseni kullanır. Artık LINQ SQL sınıflarına doğrudan kullanır.

**5 listeleme – `Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Listeleme 5 MoviesController sınıfında iki Oluşturucu olduğuna dikkat edin. Uygulamanız çalışırken ilk Oluşturucu, parametresiz bir oluşturucu çağrılır. Bu oluşturucu MovieRepository sınıfının bir örneğini oluşturur ve ikinci oluşturucusuna iletir.

İkinci oluşturucu tek bir parametreye sahiptir: IMovieRepository parametresi. Bu oluşturucu yalnızca adlı bir sınıf seviyesi alanını parametresinin değeri atar \_depo.

MoviesController sınıfı, bağımlılık ekleme desenini adlı bir yazılım tasarım deseni avantajlarından sürüyor. Özellikle, bir oluşturucu bağımlılık ekleme adlı kullanıyor. Daha fazla bilgi edinebilirsiniz Martin Fowler tarafından şu makaleyi okuyarak bu desen hakkında:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Tüm kod MoviesController (ilk Oluşturucu dışında) sınıf bildirimi, gerçek MovieRepository sınıfı yerine IMovieRepository arabirimi ile etkileşim kurar. Kod, soyut bir arabirim yerine arabirimin somut bir uygulama ile etkileşim kurar.

Uygulama tarafından kullanılan veri erişim teknolojisi değiştirmek istiyorsanız alternatif veritabanı erişimi teknolojisi kullanan bir sınıf ile IMovieRepository arabirimi yalnızca uygulayabilirsiniz. Örneğin, bir EntityFrameworkMovieRepository sınıfı veya SubSonicMovieRepository sınıfı oluşturabilirsiniz. Denetleyici sınıfı karşı arabirimi programlanmıştır denetleyici sınıfına IMovieRepository yeni bir uygulamasını geçirebilirsiniz ve sınıf çalışmaya devam eder.

MoviesController sınıfı test etmek isterseniz, ayrıca, ardından sahte film depo sınıfı için MoviesController geçirebilirsiniz. Aslında veritabanına erişim değil ancak tüm gerekli IMovieRepository arabirimin yöntemlerini içeren bir sınıf ile IMovieRepository sınıfı uygulayabilir. Bu şekilde, gerçek bir veritabanı erişmeden birim testi MoviesController sınıfı olabilir.

## <a name="summary"></a>Özet

Bu öğreticinin amacı, Microsoft LINQ SQL yararlanarak MVC model sınıfları nasıl oluşturacağınızı göstermek için oluştu. Biz, bir ASP.NET MVC uygulamasındaki veritabanı verilerini görüntülemek için iki stratejiler incelenir. İlk olarak, biz SQL sınıflarına LINQ oluşturulan ve doğrudan bir denetleyici eylemi içindeki sınıflarda kullanılır. LINQ to SQL sınıfları bir denetleyici içinde kullanarak hızlı bir şekilde sağlar ve kolayca veritabanı bir MVC uygulamasında verileri görüntülemek.

Ardından, veritabanı verilerini görüntülemek için biraz daha zor, ancak daha kesin bulduklarında yolunu inceledik. Biz, depo deseni avantajlarından sürdü ve tüm müşterilerimize veritabanı erişim mantığı ayrı depo sınıfında yerleştirilir. Denetleyicimizin, Kodumuzun bir arabirim yerine bir somut sınıf karşı tüm yazdığımız. Depo düzeni avantajlarından bize bir kolayca veritabanı erişim teknolojileri gelecekte değiştirmek etkinleştirir ve kolayca test bizim denetleyici sınıflarına olanak sağlayan ' dir.

> [!div class="step-by-step"]
> [Önceki](creating-model-classes-with-the-entity-framework-vb.md)
> [İleri](displaying-a-table-of-database-data-vb.md)
