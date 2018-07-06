---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: ASP.NET MVC ile (C#) 15 dakika içinde bir film veritabanı uygulaması oluşturma | Microsoft Docs
author: StephenWalther
description: Stephen Walther tamamlamak için bir tüm veritabanı odaklı ASP.NET MVC uygulaması oluşturur. Bu öğreticide, yeni t kişiler için harika bir giriş olduğundan...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 41b387a8ccb843249f9adfaf300d7cad16bed685
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802889"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>ASP.NET MVC ile (C#) 15 dakika içinde bir film veritabanı uygulaması oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[Kodu indir](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther tamamlamak için bir tüm veritabanı odaklı ASP.NET MVC uygulaması oluşturur. Bu öğreticide, ASP.NET MVC çerçevesi için yeni olan ve bir ASP.NET MVC uygulaması oluşturma işleminin bir fikir edinmek isteyen kişiler için harika bir giriş niteliğindedir.


"Gibi nedir" bir fikir vermek için bu öğreticinin amacı olan bir ASP.NET MVC uygulamasını oluşturmak için. Bu öğreticide, tamamlanması başından tüm ASP.NET MVC uygulaması oluşturma sürecinde ilerleyin. Ben nasıl listesinde, oluşturmak ve veritabanı kayıtlarını düzenleme gösteren basit bir veritabanı odaklı uygulama oluşturma işlemini göstermektedir.

Uygulamamızı oluşturmaya işlemini basitleştirmek için Visual Studio 2008 yapı iskelesi özelliklerinden atacağız. Visual Studio başlangıç kodunu ve denetleyicileri, modelleri, görünümleri ve içerik oluşturmak vereceğiz.

Active Server Pages ya da ASP.NET ile çalıştıysanız, daha sonra ASP.NET MVC bilgili bulmanız gerekir. ASP.NET MVC görünümleri Active Server Pages uygulama sayfaları gibi çok fazla. Ve yalnızca bir geleneksel ASP.NET Web Forms uygulaması gibi ASP.NET MVC, zengin dil ve .NET framework tarafından sağlanan sınıfları tam erişim sağlar.

Bu öğreticide bir ASP.NET MVC uygulaması oluşturma deneyimini benzer ve farklı bir Active Server Pages veya ASP.NET Web Forms uygulaması oluşturma deneyimini nasıl bir fikir verir My ümit olur.

## <a name="overview-of-the-movie-database-application"></a>Film veritabanı uygulaması genel bakış

Hedefimiz, basit bir anlatım gözetildiği için olduğundan, çok basit bir film veritabanı uygulaması oluşturacağız. Bizim basit bir film veritabanı uygulaması üç şeyleri bize izin verir:

1. Bir film veritabanı kayıt kümesi listesi
2. Yeni bir film veritabanı kaydı oluşturma
3. Var olan bir film veritabanı kaydını Düzenle

Yeniden örneği basit tutmak istediğimizden, biz en az sayıda uygulamamız oluşturmak için gereken ASP.NET MVC çerçevesi özelliklerinin avantajlarından yararlanın. Örneğin, biz Test-Driven geliştirme yararlanarak gerekmez.

Uygulamamızı oluşturmak için aşağıdaki adımların her biri tamamlanması gerekiyor:

1. ASP.NET MVC Web uygulaması projesi oluşturma
2. Veritabanı oluşturma
3. Veritabanı modeli oluşturma
4. ASP.NET MVC denetleyicisi oluşturma
5. ASP.NET MVC görünümleri oluşturma

## <a name="preliminaries"></a>Başlangıç kuralları

Visual Studio 2008 veya Visual Web Developer 2008 Express bir ASP.NET MVC uygulamasını oluşturmak için ihtiyacınız olacak. ASP.NET MVC çerçevesi indirmek gerekir.

Visual Studio 2008 sahibi siz değilseniz, Visual Studio 2008'in 90 günlük deneme sürümü bu Web sitesinden indirebilirsiniz:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternatif olarak, Visual Web Developer Express 2008 ile ASP.NET MVC uygulamaları oluşturabilirsiniz. Visual Web Developer Express kullanmaya karar verirseniz, Service Pack 1 yüklü olması gerekir. Visual Web Developer 2008 Express Service Pack 1 Bu Web sitesinden indirebilirsiniz:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Visual Studio 2008 veya Visual Web Developer 2008 yükledikten sonra ASP.NET MVC Çerçevesi'ni yüklemeniz gerekir. ASP.NET MVC çerçevesi aşağıdaki Web sitesinden indirebilirsiniz:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> ASP.NET framework ve ASP.NET MVC çerçevesi ayrı ayrı indirmek yerine Web Platformu yükleyicisi yararlanabilir. Web Platformu yükleyicisi, yüklü uygulamalar kolayca yönetmenize olanak sağlayan bir uygulama bilgisayarınıza vardır:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Bir ASP.NET MVC Web uygulaması projesi oluşturma

Visual Studio 2008'de yeni bir ASP.NET MVC Web uygulaması projesi oluşturarak başlayalım. Menü seçeneğini **dosya, yeni proje** Şekil 1'deki yeni proje iletişim kutusu görürsünüz. C# programlama dili olarak seçin ve ASP.NET MVC Web uygulaması proje şablonunu seçin. Projenize MovieApp ad verin ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Yeni Proje iletişim kutusunun üstündeki aşağı açılan listeden seçtiğiniz .NET Framework 3.5 veya ASP.NET MVC Web uygulaması proje şablonu görünmez emin olun.


Yeni bir MVC Web uygulaması projesi oluşturduğunuzda, Visual Studio ayrı birim testi projesi oluşturmak isteyip istemediğinizi sorar. Şekil 2'de bir iletişim kutusu görüntülenir. Testler bu öğreticide zaman kısıtlamaları nedeniyle oluşturmakta gerekmez (ve Evet, biz bunu biraz yapanın neden gelecektir için) seçin **Hayır** seçeneğini ve tıklayın **Tamam** düğmesi.

> [!NOTE] 
> 
> Visual Web Developer test projelerini desteklemez.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Şekil 02**: birim testi projesi oluşturma iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Bir ASP.NET MVC uygulaması klasörleri standart bir dizi vardır: modelleri, görünümleri ve denetleyicileri klasör. Bu standart bir Çözüm Gezgini penceresinde klasörler kümesi olduğunu görebilirsiniz. Dosyaları her modelleri, görünümleri ve denetleyicileri klasörleri bizim film veritabanı uygulaması oluşturmak için eklemeniz gerekecektir.

Visual Studio ile yeni bir MVC uygulaması oluşturduğunuzda, bir örnek uygulaması edinir. Sıfırdan başlayın istediğimizden, bu örnek uygulama için içeriği silmeniz gerekir. Aşağıdaki dosya ve şu klasörü silin yapmanız gerekir:

- Controllers\HomeController.cs
- Görünümler/giriş

## <a name="creating-the-database"></a>Veritabanı oluşturma

Film veritabanı Kayıtlarımız tutmak için bir veritabanı oluşturmanız gerekir. Neyse ki Visual Studio, SQL Server Express adlı ücretsiz bir veritabanı içerir. Veritabanı oluşturmak için aşağıdaki adımları izleyin:

1. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinde veri klasörü **Ekle, yeni öğe**.
2. Seçin **veri** kategori seçip alt **SQL Server veritabanı** şablonu (bkz: Şekil 3).
3. Yeni veritabanınızın adı *MoviesDB.mdf* tıklatıp **Ekle** düğmesi.

Veritabanınızı oluşturduktan sonra uygulamada bulunan MoviesDB.mdf dosyasını çift tıklayarak veritabanına bağlanabilir\_veri klasörü. MoviesDB.mdf dosyasına çift tıklayarak Sunucu Gezgini penceresini açar.

> [!NOTE] 
> 
> Sunucu Gezgini penceresi Visual Web Developer durumunda veritabanı Gezgini penceresi olarak adlandırılır.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Şekil 03**: Microsoft SQL Server veritabanı oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Ardından, size yeni bir veritabanı tablosu oluşturmanız gerekir. Sunucu Gezgini penceresi içinde tablolar klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**. Bu menü seçeneğini belirleyerek veritabanı Tablo Tasarımcısı açılır. Şu veritabanı sütunları oluşturun:

<a id="0.1_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | nvarchar(100) | False |
| Direktörü | nvarchar(100) | False |
| DateReleased | DateTime | False |


İlk sütun, kimlik sütunu iki özel özelliğe sahiptir. Öncelikle, kimlik sütunu birincil anahtar sütunu olarak işaretlemek gerekir. Kimlik sütunu seçtikten sonra **birincil anahtarı Ayarla** (bir anahtar gibi görünen simge iş) düğmesi. İkinci olarak, kimlik sütunu bir kimlik sütunu olarak işaretlemek gerekir. Sütun Özellikleri penceresinde kimlik belirtimi bölümüne inin ve genişletin. Değişiklik **olan kimlik** özellik değerine **Evet**. İşlemi tamamladığınızda, tabloda Şekil 4 gibi görünmelidir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Şekil 04**: film veritabanı tablosu ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


Son adım, yeni bir tablo tasarruf etmektir. Kaydet düğmesine (disket simgesi) tıklayın ve yeni bir tablo adı filmler verin.

Tablo oluşturma işlemini tamamladıktan sonra bazı film kayıtları tablosuna ekleyin. Sunucu Gezgini penceresinde filmler tabloya sağ tıklayıp menü seçeneğini **tablo verilerini Göster**. (Bkz: Şekil 5), en sevdiğiniz film listesini girin.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Şekil 05**: film kayıtlarını girmeyi ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Model oluşturma

Biz ardından veritabanımızdaki temsil eden sınıf kümesi oluşturmanız gerekir. Bir veritabanı modeliniz için oluşturmamız gerekir. Biz, veritabanı modelimizi sınıflarını otomatik olarak oluşturmak için Microsoft Entity Framework avantajlarından yararlanmak.

> [!NOTE] 
> 
> Microsoft Entity Framework için ASP.NET MVC çerçevesi bağlı değildir. Nesne İlişkisel eşleme çeşitli avantajlarından yararlanarak veritabanı modeli sınıfları oluşturabilirsiniz (veya / M) gibi LINQ to SQL ve Subsonic NHibernate araçları.


Varlık veri modeli Sihirbazı başlatmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini penceresinde ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.
2. Seçin **veri** kategori seçip alt **ADO.NET varlık veri modeli** şablonu.
3. Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.

Ekle düğmesine tıkladıktan sonra (bkz. Şekil 6) varlık veri modeli Sihirbazı görüntülenir. Sihirbazı tamamlamak için aşağıdaki adımları izleyin:

1. İçinde **Choose Model Contents** adım, select **veritabanından Oluştur** seçeneği.
2. İçinde **veri bağlantınızı seçin** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları için. Tıklayın **sonraki** düğmesi.
3. İçinde **veritabanı nesnelerinizi seçin** adım, tablolar düğümünü genişletin, filmler tabloyu seçin. Ad alanı girin *MovieApp.Models* tıklatıp **son** düğmesi.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Şekil 06**: Varlık veri modeli Sihirbazı ile bir veritabanı modeli oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Varlık veri modeli Sihirbazı tamamladıktan sonra varlık veri modeli Tasarımcısı açılır. Tasarımcı film veritabanı tablosu görüntülenmesi gerekir (bkz. Şekil 7).


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Şekil 07**: Varlık veri modeli Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Biz devam etmeden önce bir değişiklik yapmak ihtiyacımız var. Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* film veritabanı tablosunu temsil eden. Filmler sınıfı belirli bir filmi temsil etmek için kullanacağız çünkü olması için sınıfın adını değiştirmek ihtiyacımız *film* yerine *filmler* (tekil yerine çoğul).

Tasarımcı yüzeyinde sınıf adına çift tıklayın ve sınıfın adını film film değiştirin. Bu değişikliği yaptıktan sonra tıklayın **Kaydet** (disket simgesi) düğmesine film sınıfı oluşturun.

## <a name="creating-the-aspnet-mvc-controller"></a>ASP.NET MVC denetleyicisi oluşturma

Sonraki adım, ASP.NET MVC denetleyicisinin oluşturmaktır. Bir kullanıcı bir ASP.NET MVC uygulaması ile nasıl etkileştiğini denetlemek için bir denetleyici sorumludur.

Aşağıdaki adımları uygulayın:

1. Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici**.
2. Denetleyici Ekle iletişim kutusunda, adını *HomeController* ve etiketli onay **oluşturma, güncelleştirme ve ayrıntıları senaryoları için eylem yöntemleri ekleyin** (bkz. Şekil 8).
3. Tıklayın **Ekle** düğmesini projenize yeni denetleyici ekleyin.

Bu adımları tamamladıktan sonra Denetleyici 1 listesi oluşturulur. Dizin, Ayrıntılar, oluşturma, adlandırılmış yöntemler içerdiğine dikkat edin ve düzenleyin. Aşağıdaki bölümlerde, bu yöntemlerin işe almak için gerekli kodu ekleyeceğiz.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Şekil 08**: yeni bir ASP.NET MVC denetleyicisi ekleme ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**1 – Controllers\HomeController.cs listeleme**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Veritabanı kayıtlarını listeleme

Giriş denetleyicisine İNDİS() yöntemi, bir ASP.NET MVC uygulaması için varsayılan yöntemdir. Bir ASP.NET MVC uygulamasını çalıştırdığınızda, İNDİS() yöntemi çağrılan ilk denetleyicisi yöntemidir.

Film veritabanı tablosundan kayıt listesini görüntülemek için İNDİS() yöntemi kullanacağız. Biz veritabanına daha önce oluşturduğumuz İNDİS() yöntemiyle film veritabanı kayıtlarını almak için model sınıfları yararlanın.

Adlı yeni bir özel alan içeren listeleme 2 HomeController sınıfında değiştirdiğiniz \_db. Veritabanı modelimizi MoviesDBEntities sınıfı temsil eder ve bu sınıf, veritabanı ile iletişim kurmak için kullanacağız.

Listeleme 2 İNDİS() yöntemi değiştiren aynı zamanda. İNDİS() yöntemi, tüm film kayıtlar film veritabanı tablosundan almak için MoviesDBEntities sınıfını kullanır. İfade  *\_db. MovieSet.ToList()* film veritabanı tablosundan tüm film kayıtlarının bir listesini döndürür.

Filmler listesini görünüme iletilir. Görünüm veri View() yönteme herhangi bir şey görünümüne geçirilir.

**2 – Controllers/HomeController.cs (değiştirilmiş dizin yöntemi) listeleme**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

Dizin adlı bir görünüm İNDİS() yöntemi döndürür. Film veritabanı kayıtlarını listesini görüntülemek için bu görünümü oluşturmak ihtiyacımız var. Aşağıdaki adımları uygulayın:


Projenizi oluşturmanız (menü seçeneğini **yapı, yapı çözümü**) açmadan önce **Görünüm Ekle** iletişim kutusu veya sınıf görünür **görüntülemek veri sınıfı** açılır liste.


1. Kod düzenleyicisinde İNDİS() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 9).
2. Görünüm Ekle iletişim kutusunda, onay kutusunu etiketli olduğunu doğrulayın. **kesin türü belirtilmiş görünüm oluşturmak** denetlenir.
3. Gelen **içeriği görüntüle** açılan listesinde, bir değer seçin *listesi*.
4. Gelen **görüntülemek veri sınıfı** açılan listesinde, bir değer seçin *MovieApp.Models.Movie*.
5. Yeni oluşturmak için Ekle düğmesine tıklayın (bkz. Şekil 10) görüntüleyin.

Bu adımları tamamladıktan sonra Index.aspx adlı yeni bir görünümü görünümler/giriş klasörüne eklenir. Dizin görünümünün içeriklerini listeleme 3'te dahil edilir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Şekil 09**: görünüm denetleyicisi eylemden ekleme ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Şekil 10**: Görünüm Ekle iletişim kutusunda yeni bir görünüm oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**3 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

Dizin görünümünün tüm film veritabanı tablosundan bir HTML tablosu içinde film kayıtları görüntüler. Görünüm ViewData.Model özelliği tarafından temsil edilen her filmin üzerinden yinelenen bir foreach döngüsü içeriyor. Ardından F5 tuşuna tuşlarına basarak uygulamanızı çalıştırın, Şekil 11'de web sayfasını görürsünüz.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Şekil 11**: dizin görünümünün ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Yeni veritabanı kayıtlarını oluşturma

Önceki bölümde oluşturduğumuz dizin görünümünün, yeni veritabanı kayıtlarını oluşturmaya yönelik bir bağlantı içerir. Yeni bir ubuntu mantığını ve yeni bir film veritabanı kayıtlar oluşturmak için gerekli görünümü oluşturmak.

Giriş denetleyicisine Create() adlı iki yöntemi içerir. İlk Create() yöntem hiç parametre yok. Create() yönteminin bu aşırı yüklemesi, yeni bir film veritabanı kaydı oluşturma için HTML formu görüntülemek için kullanılır.

İkinci Create() yöntemi bir FormCollection parametreye sahiptir. Create() yönteminin bu aşırı yüklemesi, sunucuya yeni bir film oluşturmak için HTML form gönderildiğinde çağrılır. Bu ikinci Create() yöntemi bir HTTP POST işlemi gerçekleştirilmiyorsa çağrılan yöntem engelleyen AcceptVerbs öznitelik olduğuna dikkat edin.

Bu ikinci Create() yöntemi listeleme 4 güncelleştirilmiş HomeController sınıfında değiştirildi. Create() yöntemi yeni sürümünü film parametre kabul eder ve yeni bir film film veritabanı tablosuna eklemek için mantığı içerir.

> [!NOTE] 
> 
> Bind özniteliği dikkat edin. HTML formundaki film kimliği özelliğini güncelleştirme yoksa istediğimizden, bu özelliği açıkça dışarıda gerekir.


**4 – Controllers\HomeController.cs (değiştirilmiş oluşturma yöntemi) listeleme**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio'nun yeni bir film veritabanı oluşturmak için form oluşturmanın kolaylaştırır (bkz. Şekil 12) kaydedin. Aşağıdaki adımları uygulayın:

1. Kod düzenleyicisinde Create() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle**.
2. Onay kutusunu etiketli olduğunu doğrulayın **kesin türü belirtilmiş görünüm oluşturmak** denetlenir.
3. Gelen **içeriği görüntüle** açılan listesinde, bir değer seçin *Oluştur*.
4. Gelen **görüntülemek veri sınıfı** açılan listesinde, bir değer seçin *MovieApp.Models.Movie*.
5. Tıklayın **Ekle** yeni görünümü oluşturmak için düğme.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Şekil 12**: oluşturma görünümü ekleme ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio görünümünü listeleme 5'te otomatik olarak oluşturur. Bu görünüm, her film sınıfının özelliklerine karşılık gelen alan içeren bir HTML formuna içerir.

**5-Views\Home\Create.aspx listeleme**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> Görünüm Ekle iletişim kutusu tarafından oluşturulan HTML formundaki bir kimliği form alanı oluşturur. Kimlik sütunu bir kimlik sütunu olduğundan, bu form alanı gerekmez ve güvenli bir şekilde kaldırabilirsiniz.


Oluştur görünümünün ekledikten sonra veritabanına yeni film kayıtlar ekleyebilirsiniz. F5 tuşuna basarak uygulamanızı çalıştırın ve Yeni Oluştur Şekil 13 biçiminde görmek için bağlantıya tıklayın. Tamamlayın ve form gönderme, yeni bir film veritabanı kaydı oluşturulur.

Form doğrulama otomatik olarak Al dikkat edin. Bir filmi için bir yayın tarihi girin ihmal ya da geçersiz yayın tarihi girin, formun görünürler ve yayın tarih alanı vurgulanır.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Şekil 13**: yeni bir film veritabanı kaydı oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Var olan veritabanı kayıtlarını düzenleme

Önceki bölümlerde, liste ve yeni veritabanı kaydı oluşturma nasıl ele almıştık. Bu son bölümde, var olan veritabanı kayıtlarını düzenleme nasıl ele alır.

İlk olarak biz düzenleme formunda oluşturmanız gerekir. Visual Studio düzenleme formunda bizim için otomatik olarak oluşturacak olduğundan bu adım kolaydır. HomeController.cs sınıf Visual Studio Kod Düzenleyicisi'nde açın ve aşağıdaki adımları izleyin:

1. Kod düzenleyicisinde Edit() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 14).
2. Etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**.
3. Gelen **içeriği görüntüle** açılan listesinde, bir değer seçin *Düzenle*.
4. Gelen **görüntülemek veri sınıfı** açılan listesinde, bir değer seçin *MovieApp.Models.Movie*.
5. Tıklayın **Ekle** yeni görünümü oluşturmak için düğme.

Bu adımları görünümler/giriş klasörüne Edit.aspx adlı yeni bir görünümü ekler. Bu görünüm, film kaydı düzenlemek için bir HTML formuna içerir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Şekil 14**: düzenleme görünümü ekleme ([tam boyutlu görüntüyü görmek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> Düzenleme görünümü film ID özelliğine karşılık gelen bir HTML form alanından içerir. ID özelliği değerini düzenleme kişiler istemediğinden, bu form alanı kaldırmanız gerekir.


Son olarak, bir veritabanı kaydını düzenleme destekler, böylece giriş denetleyicisini değiştirmek ihtiyacımız var. Güncelleştirilmiş HomeController sınıfı listeleme 6'da yer alır.

**6 – Controllers\HomeController.cs (düzenleme metotlarını) listeleme**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Listeleme 6'da, her iki Edit() yöntemi aşırı yüklemeleri için ilave bir mantık ekledim. İlk Edit() yöntem, yönteme ID parametresine karşılık gelen bir film veritabanı kaydını döndürür. İkinci aşırı yükleme, veritabanında bir film kayda yönelik güncelleştirmelerin gerçekleştirir.

Özgün film almalı ve ardından ApplyPropertyChanges() veritabanında var olan film güncelleştirmek için arama, dikkat edin.

## <a name="summary"></a>Özet

Bu öğreticide bir ASP.NET MVC uygulaması oluşturma deneyiminin bir fikir vermek için oluştu. Bir ASP.NET MVC web uygulaması oluşturma Active Server Pages ya da ASP.NET uygulama oluşturma deneyimi için çok benzer olduğunu bulunan umarım.

Bu öğreticide, biz yalnızca en temel özellikleri ASP.NET MVC çerçevesi incelenir. Sonraki öğreticilerde, size daha ayrıntılı denetleyicileri, denetleyici eylemlerini, görünümleri, görünüm verilerini ve HTML yardımcıları gibi konular derinlerine.

> [!div class="step-by-step"]
> [Next](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
