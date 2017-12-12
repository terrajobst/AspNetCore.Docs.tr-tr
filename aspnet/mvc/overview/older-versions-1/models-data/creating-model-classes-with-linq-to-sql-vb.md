---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
title: "LINQ-SQL (VB) ile modeli sınıfları oluşturma | Microsoft Docs"
author: microsoft
description: "Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, model c oluşturmayı öğrenin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: a4a25a75-d71f-4509-98b4-df72e748985a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-vb
msc.type: authoredcontent
ms.openlocfilehash: 972d5b11049825e84e070ef1c4b2b90116654397
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-linq-to-sql-vb"></a>LINQ-SQL (VB) ile modeli sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_VB.pdf)

> Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, modeli sınıfları oluşturmak ve veritabanı erişimi Microsoft LINQ SQL yararlanarak gerçekleştirmeyi öğrenin.


Bu öğretici bir ASP.NET MVC uygulaması için model sınıfları oluşturma bir yöntem açıklamak için hedefidir. Bu öğreticide, modeli sınıfları oluşturmak ve veritabanı erişimi Microsoft LINQ SQL yararlanarak gerçekleştirmeyi öğrenin.

Bu öğreticide, temel bir filmi veritabanı uygulaması oluşturun. Film veritabanı uygulama hızlı ve kolay şekilde olası oluşturarak başlayın. Biz bizim veri erişimi tümünün doğrudan sunduğumuz denetleyicisi Eylemler gerçekleştirin.

Ardından, havuz deseni kullanmayı öğrenin. Depo düzeni kullanarak biraz daha fazla iş gerektirir. Ancak, bu deseni benimsenmesi avantajı için uyarlanabilir uygulamalar oluşturmanıza olanak tanır olduğu değiştirin ve kolayca sınanabilir.

## <a name="what-is-a-model-class"></a>Bir Model sınıfı nedir?

Bir MVC model tüm MVC görünümü ya da MVC denetleyicisi bulunmayan uygulama mantığını içerir. Özellikle, bir MVC model tüm uygulama iş ve veri erişim mantığı içerir.

Veri erişim mantığı uygulamak için çeşitli farklı teknolojiler kullanabilirsiniz. Örneğin, Microsoft Entity Framework, NHibernate, Subsonic veya ADO.NET sınıflarını kullanarak, veri erişim sınıfları oluşturabilirsiniz.

Bu öğreticide, ı sorgulamak ve veritabanını güncelleştirmek için LINQ-SQL kullanın. LINQ-SQL, bir Microsoft SQL Server veritabanıyla etkileşim çok çok kolay bir yöntemini sağlar. Ancak, ASP.NET MVC çerçevesi LINQ-SQL için herhangi bir şekilde bağlı değildir olduğunu anlamak önemlidir. ASP.NET MVC, tüm veri erişim teknolojisi ile uyumludur.

## <a name="create-a-movie-database"></a>Film veritabanı oluşturma

Bu öğreticide--modeli sınıfları--nasıl oluşturulacağına göstermek için biz basit bir filmi veritabanı uygulaması oluşturun. İlk adım, yeni bir veritabanı oluşturmaktır. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinin veri klasörü **Ekle, yeni öğe**. SQL Server veritabanı şablonu seçin, MoviesDB.mdf adını verin ve tıklatın **Ekle** (bkz: Şekil 1) düğmesine tıklayın.


[![Yeni bir SQL Server veritabanı ekleme](creating-model-classes-with-linq-to-sql-vb/_static/image2.png)](creating-model-classes-with-linq-to-sql-vb/_static/image1.png)

**Şekil 01**: yeni bir SQL Server veritabanı ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image3.png))


Yeni veritabanı oluşturduktan sonra uygulama MoviesDB.mdf dosyasını çift tıklatarak veritabanını açabilirsiniz\_veri klasörü. MoviesDB.mdf dosyasını çift tıklayarak Sunucu Gezgini penceresini açar (bkz: Şekil 2).

|  | Sunucu Gezgini penceresini Visual Web Developer kullanırken veritabanı Gezgini penceresi adı verilir. |
| --- | --- |


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-vb/_static/image5.png)](creating-model-classes-with-linq-to-sql-vb/_static/image4.png)

**Şekil 02**: Sunucu Gezgini penceresini kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image6.png))


Bir tablo bizim filmler temsil eden bizim veritabanına eklemek gerekir. Tables klasörünü sağ tıklatın ve menü seçeneğini seçmek **Yeni Tablo Ekle**. Bu menü seçeneği Tablo Tasarımcısı açar (bkz: Şekil 3).


[![Sunucu Gezgini penceresini kullanma](creating-model-classes-with-linq-to-sql-vb/_static/image8.png)](creating-model-classes-with-linq-to-sql-vb/_static/image7.png)

**Şekil 03**: Tablo Tasarımcısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image9.png))


Aşağıdaki sütunlar bizim veritabanı tablosuna eklemek ihtiyacımız var:

| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Director | nvarchar(50) | False |

İki özel ID sütunu için yapmanız gerekir. İlk olarak, Tablo Tasarımcısı'nda sütunun seçilmesi ve bir anahtar simgesine tıklayarak ID sütunu birincil anahtar sütun olarak işaretlemek gerekir. LINQ-SQL gerçekleştirme ekler veya veritabanına karşı güncelleştirdiğinde, birincil anahtar sütunlarının belirtmenizi gerektirir.

Ardından, değeri Evet atayarak ID sütunu bir kimlik sütunu işaretlemek ihtiyacınız **Is Identity** özelliği (bkz: Şekil 3). Bir kimlik sütunu bir tablo için yeni bir veri satırı eklediğinizde, yeni bir sayı otomatik olarak atanan bir sütundur.

Bu değişiklikleri yaptıktan sonra tablo adı tblMovie ile kaydedin. Kaydet düğmesine tıklayarak tablo kaydedebilirsiniz.

## <a name="create-linq-to-sql-classes"></a>LINQ-SQL sınıfları oluşturma

MVC modelimizi LINQ tblMovie veritabanı tablosu temsil eden SQL'e sınıflarını içerir. Bu LINQ'ten SQL'e sınıflarını oluşturmak için en kolay yoludur modeller klasörü sağ tıklatın, seçin **Ekle, yeni öğe**, LINQ to SQL sınıfları şablonu seçin, sınıfları Movie.dbml adını verin ve tıklatın **Ekle**(bkz: Şekil 4) düğmesine tıklayın.


[![LINQ SQL sınıfları için oluşturma](creating-model-classes-with-linq-to-sql-vb/_static/image11.png)](creating-model-classes-with-linq-to-sql-vb/_static/image10.png)

**Şekil 04**: SQL sınıfları için oluşturma LINQ ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image12.png))


Hemen film LINQ-SQL sınıfları oluşturduktan sonra Nesne İlişkisel Tasarımcısı görüntülenir. LINQ-SQL belirli veritabanı tablolarını temsil eden sınıfları oluşturmak için Nesne İlişkisel Tasarımcısı Sunucu Gezgini penceresinden veritabanı tablolarını sürükleyin. Nesne İlişkisel Tasarımcısı üzerine tblMovie veritabanı tablosuna eklemek ihtiyacımız (Şekil 4'e bakın).


[![Nesne İlişkisel Tasarımcısı'nı kullanarak](creating-model-classes-with-linq-to-sql-vb/_static/image14.png)](creating-model-classes-with-linq-to-sql-vb/_static/image13.png)

**Şekil 05**: Nesne İlişkisel Tasarımcısı'nı kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image15.png))


Varsayılan olarak, Nesne İlişkisel Tasarımcısı tasarımcısına sürükleyin veritabanı tablosunun çok aynı ada sahip bir sınıf oluşturur. Ancak, biz bizim sınıfı tblMovie çağrısı istemezsiniz. Bu nedenle, Sınıf Tasarımcısı'nda adına tıklayın ve film sınıfın adını değiştirin.

Son olarak, tıklatın unutmayın **kaydetmek** SQL'e sınıflarını LINQ kaydetmek için (disket resim) düğmesine tıklayın. Aksi takdirde, LINQ to SQL sınıfları Nesne İlişkisel Tasarımcısı tarafından oluşturulan olmaz.

## <a name="using-linq-to-sql-in-a-controller-action"></a>LINQ-SQL kullanarak bir denetleyici eylemi

Bizim LINQ'ten SQL'e sınıflarını sahibiz, biz bu sınıfların veritabanından veri almak için kullanabilirsiniz. Bu bölümde, LINQ için bir denetleyici eylemi içinde doğrudan SQL sınıfların nasıl kullanılacağını öğrenin. Biz tblMovies veritabanı tablosundan filmler listesi MVC görünümünde görüntülersiniz.

İlk olarak, biz HomeController sınıfı değiştirmeniz gerekir. Bu sınıf, uygulamanızın denetleyicileri klasöründe bulunabilir. Sınıfı, listeleme 1 sınıfında benzer şekilde değiştirin.

**Kod 1 –`Controllers\HomeController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample1.vb)]

Listeleme 1 İNDİS() eylemde MoviesDB veritabanı temsil etmek için bir LINQ to SQL DataContext sınıfı (MovieDataContext) kullanır. MoveDataContext sınıfı, Visual Studio nesne ilişkisel tasarımcısı tarafından oluşturuldu.

LINQ Sorgu tüm filmler tblMovies veritabanı tablosundan almak için DataContext karşı yapılır. Film listesi filmler adlı yerel bir değişkene atanır. Son olarak, filmler listesi görünüm verilerine görünümüne geçirilir.

Film gösterebilmeniz biz sonraki dizin görünümünün değiştirmeniz gerekir. Dizin görünümünün Views\Home\ klasöründe bulabilirsiniz. Dizin görünümünün listeleme 2 görünümünde benzer şekilde güncelleştirin.

**Kod 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample2.aspx)]

Değiştirilen dizin görünümü içerir bildirimi bir &lt;% @ alma ad alanı %&gt; görünümü üstündeki yönergesi. Bu yönerge MvcApplication1 ad alanını alır. Model sınıflarıyla – özellikle, film sınıfı--görünümünde çalışması için bu ad alanı gerekir.

For listeleme 2 görünümünde içeren ViewData.Model özelliği tarafından temsil edilen öğelerin tümünü dolaşır her bir döngü. Başlık özelliğinin değeri, her film görüntülenir.

ViewData.Model özelliğinin değeri bir IEnumerable dönüştürüldüğüne dikkat edin. ViewData.Model içeriği döngü için bu gereklidir. Kesin türü belirtilmiş görünüm oluşturmak için burada başka bir seçenek değil. Kesin türü belirtilmiş bir görünüm oluşturduğunuzda, bir görünümün arka plan kodu sınıfında belirli bir tür ViewData.Model özelliğine dönüştürün.

HomeController sınıfı ve dizin görünümünün değiştirme sonra uygulama çalıştırırsanız, boş bir sayfa alırsınız. TblMovies veritabanı tablosunda hiçbir film kayıt olduğundan boş bir sayfa elde edersiniz.

Kayıtları tblMovies veritabanı tablosuna eklemek için Sunucu Gezgini penceresi (Visual Web Developer veritabanı Gezgini penceresinde) tblMovies veritabanı tablosunda sağ tıklayın ve menü seçeneğini **Show Table Data**. (Bkz. Şekil 5) görünür kılavuz kullanarak film kayıtları ekleyebilirsiniz.


[![Film ekleme](creating-model-classes-with-linq-to-sql-vb/_static/image17.png)](creating-model-classes-with-linq-to-sql-vb/_static/image16.png)

**Şekil 06**: filmler ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image18.png))


Bazı veritabanı kayıtlarını tblMovies tabloya ekleyin ve uygulamayı çalıştırdıktan sonra Şekil 7'deki sayfası görürsünüz. Film veritabanı kayıtların tümünü madde işaretli listede görüntülenir.


[![Film dizin görünümünün ile görüntüleme](creating-model-classes-with-linq-to-sql-vb/_static/image20.png)](creating-model-classes-with-linq-to-sql-vb/_static/image19.png)

**Şekil 07**: dizin görünümüyle filmler görüntüleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-model-classes-with-linq-to-sql-vb/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Depo düzeni kullanma

Önceki bölümde SQL sınıflara doğrudan bir denetleyici eylemi içinde LINQ kullanılır. MovieDataContext sınıfından doğrudan İNDİS() denetleyici eylemi kullandık. Basit bir uygulama durumunda bunu ile yanlış bir şey yoktur. Ancak, daha karmaşık bir uygulama oluşturmak ihtiyacınız olduğunda LINQ-SQL ile doğrudan çalışmak denetleyicisi sınıfında sorunları oluşturur.

LINQ-SQL içinde denetleyici sınıfını kullanarak veri erişim teknolojileri gelecekte geçiş yapmak zorlaştırır. Örneğin, veri erişim teknoloji Microsoft Entity Framework kullanarak Microsoft LINQ-SQL kullanarak geçiş karar verebilirsiniz. Bu durumda, veritabanı, uygulamanızın içinde erişen her bir denetleyicisi yeniden gerekir.

LINQ-SQL içinde denetleyici sınıfını kullanarak da, uygulamanız için birim testleri oluşturmak zorlaştırır. Normalde, birim testleri gerçekleştirirken bir veritabanıyla etkileşim kurmak istemezsiniz. Birim testleri test uygulamanızın mantığı ve olmayan veritabanı sunucunuz için kullanmak istediğiniz.

Bir MVC uygulaması oluşturmak için gelecekteki daha uyarlanabilir değişiklik ve daha kolay sınanabilir, havuz deseni kullanmayı düşünmeniz gerekir. Depo düzeni kullandığınızda, tüm veritabanı erişim mantığı içeren bir ayrı deposu sınıf oluşturun.

Depo sınıfını oluşturduğunuzda, tüm depo sınıfı tarafından kullanılan yöntemleri temsil eden bir arabirim oluşturun. Denetleyicilerinizi içinde kodunuzu depo yerine arabirimi karşı yazın. Bu şekilde, gelecekte farklı veri erişim teknolojileri kullanarak depoyu uygulayabilirsiniz.

Listeleme 3 arabiriminde IMovieRepository adlandırılır ve ListAll() adlı tek bir yöntemi temsil eder.

**Kod 3 –`Models\IMovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample3.vb)]

Listeleme 4 deposu sınıfında IMovieRepository arabirimini uygular. IMovieRepository arabirimi tarafından gerekli yöntemi karşılık gelen ListAll() adlı bir yöntem içerdiğine dikkat edin.

**4 listeleme –`Models\MovieRepository.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample4.vb)]

Son olarak, 5 listeleme MoviesController sınıfında havuz deseni kullanır. Artık LINQ SQL'e sınıflarını doğrudan kullanır.

**5 listeleme –`Controllers\MoviesController.vb`**

[!code-vb[Main](creating-model-classes-with-linq-to-sql-vb/samples/sample5.vb)]

Listeleme 5 MoviesController sınıfında iki oluşturucular olduğuna dikkat edin. Uygulamanızı çalıştırırken parametresiz bir kurucusu ilk Oluşturucusu çağrılır. Bu oluşturucu MovieRepository sınıfının bir örneği oluşturur ve ikinci oluşturucuya geçirir.

İkinci oluşturucu tek bir parametreye sahiptir: bir IMovieRepository parametresi. Bu oluşturucu, parametresinin değeri yalnızca adlı bir sınıf düzeyi alanına atar. \_deposu.

MoviesController sınıfı bağımlılık ekleme düzeni adlı bir yazılım tasarım modelinin avantajı, sürüyor. Özellikle, bir şey Oluşturucusu bağımlılık ekleme adlı kullanıyor. Daha fazla bilgiyi aşağıdaki makaleyi Martin Fowler tarafından okuyarak bu desen hakkında:

[http://martinfowler.com/articles/injection.HTML](http://martinfowler.com/articles/injection.html)

Tüm MoviesController kodda (ilk Oluşturucusu hariç olmak üzere) sınıf bildirimi gerçek MovieRepository sınıfı yerine IMovieRepository arabirimi ile etkileşim kurar. Kod arabirimin somut bir uygulama yerine soyut bir arabirim ile etkileşim kurar.

Uygulama tarafından kullanılan veri erişim teknolojisi değiştirmek isterseniz alternatif veritabanı erişim teknolojisini kullanan bir sınıfı IMovieRepository arabirimiyle yalnızca uygulayabilirsiniz. Örneğin, bir EntityFrameworkMovieRepository sınıfı veya SubSonicMovieRepository sınıfı oluşturabilirsiniz. Denetleyici sınıfı karşı arabirimi programlanmış çünkü IMovieRepository yeni bir uygulama denetleyicisi sınıfı geçirebilir ve sınıfı çalışmaya devam eder.

MoviesController sınıfı test etmek isterseniz, ayrıca, ardından bir sahte film depo sınıfına MoviesController geçirebilirsiniz. Gerçekte veritabanına erişim değil ancak tüm IMovieRepository arabirimi gerekli yöntemleri içeren bir sınıf IMovieRepository sınıfıyla uygulayabilirsiniz. Bu şekilde, gerçek bir veritabanı erişmeden birim testi MoviesController sınıfı olabilir.

## <a name="summary"></a>Özet

Microsoft LINQ SQL yararlanarak MVC model sınıfları nasıl oluşturabileceğinizi göstermek için bu öğreticinin amacı oluştu. Bir ASP.NET MVC uygulamasında veritabanı veri görüntüleme iki stratejileri bileşen başvuru incelenir. İlk olarak, biz LINQ SQL'e sınıflarını oluşturulur ve doğrudan bir denetleyici eylemi içinde yer alan sınıfları kullanılır. SQL sınıfları için bir denetleyici için LINQ kullanarak hızlı bir şekilde sağlar ve kolayca bir MVC uygulamasında veritabanı veri görüntüleme.

Ardından, veritabanı verilerini görüntülemek için biraz daha zor ancak kesinlikle daha virtuous bir yolu incelediniz. Biz depo modelinin avantajı, sürdü ve tüm veritabanı erişimi mantığımızı ayrı deposu sınıfında yerleştirilir. Bizim denetleyicisi tüm kodumuza arabirimin somut bir sınıf yerine karşı yazdığımız. Depo modelinin avantajı, bize kolayca veritabanı erişim teknolojileri gelecekte değiştirmek etkinleştirir ve bize kolayca bizim denetleyicisi sınıfları test etmek yararlanmanızı sağlamasıdır.

>[!div class="step-by-step"]
[Önceki](creating-model-classes-with-the-entity-framework-vb.md)
[sonraki](displaying-a-table-of-database-data-vb.md)
