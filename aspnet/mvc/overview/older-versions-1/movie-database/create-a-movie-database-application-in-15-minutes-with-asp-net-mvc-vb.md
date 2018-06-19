---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
title: ASP.NET MVC (VB) ile 15 dakika içinde bir film veritabanı uygulaması oluşturma | Microsoft Docs
author: StephenWalther
description: Stephen Walther tamamlamak için bir tüm veritabanı kaynaklı ASP.NET MVC uygulaması başından oluşturur. Bu öğretici, yeni t kişiler için harika bir giriş olduğunu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: e4ba9786-734c-4eb3-91bb-089793325d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb
msc.type: authoredcontent
ms.openlocfilehash: ecd5892457af5bc14a939672c64eed85fc05ec22
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876208"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-vb"></a>ASP.NET MVC (VB) ile 15 dakika içinde bir film veritabanı uygulaması oluşturma
====================
tarafından [Stephen Walther](https://github.com/StephenWalther)

[Kodu indirme](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_VB.zip)

> Stephen Walther tamamlamak için bir tüm veritabanı kaynaklı ASP.NET MVC uygulaması başından oluşturur. Bu öğreticide ASP.NET MVC çerçevesi için yeni olan ve bir ASP.NET MVC uygulaması oluşturma işleminin bir fikir edinmek isteyen kişiler için harika bir giriş değil.


"Gibi nedir" bir fikir vermek için bu öğreticinin amacı olan bir ASP.NET MVC uygulaması oluşturmak için. Bu öğreticide, tamamlanması başından tüm ASP.NET MVC uygulama oluşturma aracılığıyla ilerleyin. T nasıl listesinde, oluşturabilir ve veritabanı kayıtlarını düzenleme gösteren bir basit veritabanı kaynaklı uygulamasının nasıl oluşturulacağını gösterir.

Uygulamamızı oluşturmaya işlemini basitleştirmek için Visual Studio 2008'in Yapı iskelesi özelliklerinden gitmek. İlk kod ve bizim denetleyicileri, modelleri ve görünümler için içerik oluşturmak Visual Studio size bildiririz.

Active Server Pages veya ASP.NET ile çalıştıysanız, daha sonra ASP.NET MVC bilgili bulmanız gerekir. ASP.NET MVC çok benzer bir Active Server Pages uygulama sayfalarında görünümlerdir. Ve yalnızca bir geleneksel ASP.NET Web Forms uygulaması gibi ASP.NET MVC, diller ve .NET framework tarafından sağlanan sınıfları zengin kümesi tam erişim sağlar.

Bu öğretici bir ASP.NET MVC uygulaması oluşturmanın deneyimi benzer ve farklı bir Active Server Pages veya ASP.NET Web Forms uygulaması oluşturma deneyimini nasıl bir fikir verir My soluk olur.

## <a name="overview-of-the-movie-database-application"></a>Film veritabanı uygulaması genel bakış

Amacımız örneği basit tutmak için olduğundan, biz çok basit bir filmi veritabanı uygulaması derleme. Basit film veritabanı uygulamamız üç şey yapacağımız izin verir:

1. Film veritabanı kayıt kümesi listesi
2. Yeni bir filmi veritabanı kaydı oluşturma
3. Var olan bir filmi veritabanı kaydını düzenleme

Örneği basit tutmak istiyoruz olduğundan yeniden, biz uygulamamız oluşturmak için gereken ASP.NET MVC çerçevesi özelliklerini en az sayıda yararlanmak. Örneğin, biz Test-Driven geliştirme yararlanarak olmaz.

Uygulamamız oluşturmak için aşağıdaki adımların her biri tamamlamak ihtiyacımız var:

1. ASP.NET MVC Web uygulaması projesi oluşturma
2. Veritabanı oluşturma
3. Veritabanı modeli oluşturma
4. ASP.NET MVC denetleyicisi oluşturun
5. ASP.NET MVC görünümler oluşturma

## <a name="preliminaries"></a>Başlangıç kuralları

Visual Studio 2008 veya Visual Web Developer 2008 Express bir ASP.NET MVC uygulaması oluşturmak için gerekir. ASP.NET MVC çerçevesi indirmek gerekir.

Visual Studio 2008 sahibi siz değilseniz, bu Web sitesinden Visual Studio 2008 90 günlük deneme sürümü yükleyebilirsiniz:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Alternatif olarak, ASP.NET MVC uygulamaları Visual Web Developer Express 2008 ile oluşturabilirsiniz. Visual Web Developer Express kullanmaya karar verirseniz, Service Pack 1 yüklü olması gerekir. Visual Web Developer 2008 Express with Service Pack 1 Bu Web sitesinden indirebilirsiniz:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Visual Studio 2008 veya Visual Web Developer 2008 yükledikten sonra ASP.NET MVC çerçevesi yüklemeniz gerekir. ASP.NET MVC çerçevesi aşağıdaki Web sitesinden indirebilirsiniz:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> ASP.NET framework ve ASP.NET MVC çerçevesi ayrı ayrı indirmek yerine Web Platformu yükleyicisi yararlanabilir. Web Platformu yükleyicisi yüklü uygulamaları kolayca yönetmenize olanak sağlayan bir uygulama, bilgisayarınızın gösterilebilir:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Bir ASP.NET MVC Web uygulaması projesi oluşturma

Visual Studio 2008'de yeni bir ASP.NET MVC Web uygulama projesi oluşturarak başlayalım. Menü seçeneğini **dosya, yeni proje** ve Şekil 1'deki yeni proje iletişim kutusu görürsünüz. Visual Basic programlama dili olarak seçin ve ASP.NET MVC Web uygulaması proje şablonu seçin. Projeniz MovieApp adını verin ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.png))


.NET Framework 3.5 yeni proje iletişim kutusunun üstündeki aşağı açılan listeden seçin veya ASP.NET MVC Web uygulaması proje şablonu görünmez emin olun.


Yeni bir MVC Web uygulaması projesi oluşturduğunuzda, Visual Studio ayrı birim testi projesi oluşturmak isteyip istemediğinizi sorar. Şekil 2'deki iletişim kutusu görüntülenir. Testleri Bu öğreticide zaman kısıtlamaları nedeniyle oluşturuluyor olmaz (ve Evet, biz bu hakkında biraz yapanın neden geliyor olmalıdır çünkü) seçin **Hayır** seçeneğini ve tıklayın **Tamam** düğmesi.

> [!NOTE] 
> 
> Visual Web Developer testi projelerini desteklemez.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.png)

**Şekil 02**: birim testi projesi oluştur iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.png))


Bir ASP.NET MVC uygulaması klasörleri standart bir dizi vardır: bir modeller, görünümler ve denetleyiciler klasör. Bu standart klasörler Çözüm Gezgini penceresinde kümesini görebilirsiniz. Dosyaları her modeller, görünümler ve denetleyiciler klasörleri film veritabanı uygulamamız oluşturmak için Ekle gerekecektir.

Visual Studio ile yeni bir MVC uygulaması oluşturduğunuzda, örnek bir uygulama alın. Baştan başlamak istiyoruz için bu örnek uygulama içeriği silmeniz gerekiyor. Aşağıdaki dosya ve aşağıdaki klasörü silmeniz gerekir:

- Controllers\HomeController.vb
- Views\Home

## <a name="creating-the-database"></a>Veritabanı oluşturuluyor

Film veritabanı Kayıtlarımıza tutmak için bir veritabanı oluşturmak gerekir. Visual Studio, SQL Server Express adlı boş bir veritabanı içerir. Veritabanını oluşturmak için aşağıdaki adımları izleyin:

1. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinin veri klasörü **Ekle, yeni öğe**.
2. Seçin **veri** kategori ve select **SQL Server veritabanı** şablonu (bkz: Şekil 3).
3. Yeni veritabanınızın adını *MoviesDB.mdf* tıklatıp **Ekle** düğmesi.

Veritabanınızı oluşturduktan sonra uygulamada bulunan MoviesDB.mdf dosyasını çift tıklatarak veritabanına bağlanabildiğinden\_veri klasörü. MoviesDB.mdf dosyasını çift tıklayarak Sunucu Gezgini penceresini açar.

> [!NOTE] 
> 
> Sunucu Gezgini penceresini Visual Web Developer olması durumunda veritabanı Gezgini penceresi adı verilir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.png)

**Şekil 03**: Microsoft SQL Server veritabanı oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.png))


Ardından, yeni bir veritabanı tablosu oluşturmak ihtiyacımız. Sunucu Gezgini penceresi içinde tablolar klasörünü sağ tıklatın ve menü seçeneğini seçmek **Yeni Tablo Ekle**. Bu menü seçeneğini belirleyerek veritabanı Tablo Tasarımcısı'nı açar. Şu veritabanı sütunları oluşturun:

<a id="0.2_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(100) | False |
| Director | Nvarchar(100) | False |
| DateReleased | DateTime | False |


İlk sütun, kimliği sütun, iki özel özellikleri vardır. İlk olarak, kimlik sütunu birincil anahtar sütunu olarak işaretlemek gerekir. ID sütunu seçtikten sonra **birincil anahtarı ayarlama** düğmesini (bir anahtarı gibi görünüyor simgesi olduğunu). İkinci olarak, kimlik sütunu bir kimlik sütunu olarak işaretlemek gerekir. Sütun Özellikleri penceresinde kimlik belirtimi bölümüne gidin ve genişletin. Değişiklik **Is Identity** özelliğinin değeri **Evet**. İşiniz bittiğinde, tablonun Şekil 4 gibi görünmelidir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.png)

**Şekil 04**: filmler veritabanı tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.png))


Son adım, yeni bir tablo tasarruf etmektir. Kaydet düğmesine (disket simgesi) tıklayın ve yeni bir tablo adı filmler verin.

Tablo oluşturma işlemini tamamladıktan sonra bazı film kayıtlarını tabloya ekleyin. Sunucu Gezgini penceresinde filmler tabloyu sağ tıklatın ve menü seçeneğini **Show Table Data**. (Bkz. Şekil 5), en sevdiğiniz film listesini girin.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.png)

**Şekil 05**: film kayıtlarını girmeyi ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.png))


## <a name="creating-the-model"></a>Model oluşturma

Biz sonraki Veritabanımıza temsil eden sınıfları kümesi oluşturmanız gerekir. Veritabanı modeli oluşturmak gerekir. Biz bizim veritabanı modeli sınıflarını otomatik olarak oluşturmak üzere Microsoft Entity Framework yararlanmak.

> [!NOTE] 
> 
> ASP.NET MVC çerçevesi için Microsoft Entity Framework bağlı değildir. Nesne İlişkisel eşleme çeşitli yararlanarak veritabanınızı model sınıfları oluşturabilirsiniz (veya / M) LINQ to SQL, Subsonic ve NHibernate dahil olmak üzere araçlar.


Varlık veri modeli Sihirbazı'nı başlatmak için şu adımları izleyin:

1. Çözüm Gezgini penceresinin ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.
2. Seçin **veri** kategori ve select **ADO.NET varlık veri modeli** şablonu.
3. Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.

Ekle düğmesine tıkladıktan sonra (bkz. Şekil 6) varlık veri modeli Sihirbazı görünür. Sihirbazı tamamlamak için aşağıdaki adımları izleyin:

1. İçinde **Model içeriği seçin** adım, select **veritabanından Oluştur** seçeneği.
2. İçinde **veri bağlantınızı** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları. Tıklatın **sonraki** düğmesi.
3. İçinde **veritabanı nesnelerinizi** adım, tablolar düğümünü genişletin, filmler tabloyu seçin. Ad alanı girin *MovieApp.Models* tıklatıp **son** düğmesi.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.png)

**Şekil 06**: Varlık veri modeli Sihirbazı'nı kullanarak bir veritabanı modeli oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.png))


Entity Data Model Designer, varlık veri modeli Sihirbazı tamamladıktan sonra açılır. Tasarımcı filmler veritabanı tablosu görüntülenmelidir (bkz. Şekil 7).


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.png)

**Şekil 07**: Entity Data Model Designer ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.png))


Biz devam etmeden önce bir değişiklik yapmanız gerekir. Varlık veri Sihirbazı filmler veritabanı tablosu temsil eden filmler adlı bir model sınıfı oluşturur. Film sınıfı belirli bir filmi temsil etmek için kullanacağız için olması için sınıf adını değiştirmek gerekiyor *film* yerine *filmler* (tekil yerine çoğul).

Sınıf Tasarımcısı yüzeyinde adına çift tıklayın ve film film sınıfın adını değiştirin. Bu değişikliği yaptıktan sonra tıklatın **kaydetmek** film sınıfı oluşturmak için (disket simgesi) düğmesine tıklayın.

## <a name="creating-the-aspnet-mvc-controller"></a>ASP.NET MVC denetleyicisi oluşturma

Sonraki adım, ASP.NET MVC denetleyicisi oluşturmaktır. Bir kullanıcı bir ASP.NET MVC uygulaması ile nasıl etkileşim kurduğunu denetlemek için bir denetleyici sorumludur.

Aşağıdaki adımları uygulayın:

1. Çözüm Gezgini penceresinde denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini seçmek **Ekle, denetleyici**.
2. Adı denetleyici Ekle iletişim kutusuna girin *HomeController* ve etiketli onay kutusunu işaretleyin **oluşturma, güncelleştirme ve ayrıntıları senaryoları için eylem yöntemleri eklemek** (bkz. Şekil 8).
3. Tıklatın **Ekle** düğmesi yeni denetleyicisi projenize ekleyin.

Bu adımları tamamladıktan sonra denetleyici listeleme 1'de oluşturulur. Dizin, ayrıntıları, oluşturun, adlı yöntemleri içerdiğine dikkat edin ve düzenleyin. Aşağıdaki bölümlerde bu yöntemlerin işe alma gerekli kodu ekleyeceğiz.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image15.png)

**Şekil 08**: yeni bir ASP.NET MVC denetleyicisi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image16.png))


**Listing 1 – Controllers\HomeController.vb**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample1.vb)]

## <a name="listing-database-records"></a>Veritabanı kayıtlarını listeleme

Giriş denetleyicisinin İNDİS() yöntemi, bir ASP.NET MVC uygulaması için varsayılan yöntemdir. Bir ASP.NET MVC uygulamasını çalıştırdığınızda, İNDİS() yöntemi çağrılan ilk denetleyicisi yöntemidir.

Film veritabanı tablosundan kayıtların listesini görüntülemek için İNDİS() yöntemi kullanacağız. Biz veritabanını daha önce İNDİS() yöntemiyle film veritabanına kayıtları almak için oluşturduğumuz modeli sınıfları yararlanmak.

Adlı yeni bir özel alan içeren ı listeleme 2 HomeController sınıfında değiştirmiş \_db. Veritabanı modelimizi MoviesDBEntities sınıfı temsil eder ve bu sınıf, veritabanı ile iletişim kurmak için kullanacağız.

Listeleme 2 İNDİS() yönteminde değiştiren da. İNDİS() yöntemi MoviesDBEntities sınıfı film kayıtların tümünü filmler veritabanı tablosundan almak için kullanır. İfade  *\_db. MovieSet.ToList()* filmler veritabanı tablosundan film kayıtların tümünü listesini döndürür.

Film listesi görünümüne geçirilir. Görünüm verileri olarak View() yönteme geçirilen herhangi bir şey görünüme iletilir.

**2 – Controllers/HomeController.vb (değiştirilmiş dizin yöntemi) listeleme**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample2.vb)]

Dizin adlı bir görünümü İNDİS() yöntemi döndürür. Film veritabanı kayıtlarının listesini görüntülemek için bu görünümü oluşturmak gerekir. Aşağıdaki adımları uygulayın:


Projenizi derleme (menü seçeneğini belirleyin **Yapı Çözümü Derle**) açmadan önce **Görünüm Ekle** iletişim veya hiçbir sınıfları görünür **görüntülemek veri sınıfı** aşağı açılan liste.


1. Kod düzenleyicisinde İNDİS() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 9).
2. Görünüm Ekle iletişim kutusunda, onay kutusunu etiketli doğrulayın **kesin türü belirtilmiş görünüm oluşturmak** denetlenir.
3. Gelen **görüntülemek içerik** açılır listesinde, değeri seçin *listesi*.
4. Gelen **görüntülemek veri sınıfı** açılır listesinde, değeri seçin *MovieApp.Movie*.
5. Yeni oluşturmak için Ekle düğmesini tıklatın (bkz. Şekil 10) görüntüleyin.

Bu adımları tamamladıktan sonra Index.aspx adlı yeni bir görünüm görünümler klasörüne eklenir. Dizin görünümünün içeriğini listeleme 3'te dahil edilir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image17.png)

**Şekil 09**: bir denetleyici eylemi bir görünüm ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image18.png))


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image19.png)

**Şekil 10**: yeni bir görünüm için Görünüm Ekle iletişim kutusunu oluşturmayı ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image20.png))


[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample3.aspx)]

Dizin görünümünün bir HTML tablosu içindeki filmler veritabanı tablosundan film kayıtların tümünü görüntüler. Görünüm For içeriyor ViewData.Model özelliği tarafından temsil edilen her film dolaşır her bir döngü. Ardından, uygulamanızın F5 tuşuna basmak tarafından çalıştırırsanız, Şekil 11 web sayfasını görürsünüz.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image21.png)

**Şekil 11**: dizin görünümünün ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image22.png))


## <a name="creating-new-database-records"></a>Yeni veritabanı kayıtlarını oluşturma

Önceki bölümde oluşturduğumuz dizin görünümünün yeni veritabanı kayıtları oluşturmak için bir bağlantı içerir. Devam edelim ve mantığı uygular ve görünüm yeni film veritabanı kayıtları oluşturmak için gerekli oluşturun.

Giriş denetleyicisi Create() adlı iki yöntem içerir. İlk Create() yöntemini hiç parametre yok. Create() yönteminin bu aşırı yeni bir filmi veritabanı kaydı oluşturma için HTML formu görüntülemek için kullanılır.

İkinci Create() yöntemi FormCollection parametresine sahiptir. Create() yönteminin bu aşırı sunucuya yeni bir filmi oluşturmak için HTML form gönderildiğinde çağrılır. Bu ikinci Create() yöntemi yöntemi, bir HTTP Post işlem gerçekleştirilmezse adlı önler AcceptVerbs özniteliğine sahip olmadığına dikkat edin.

Bu ikinci Create() yöntemi listeleme 4 güncelleştirilmiş HomeController sınıfında değiştirildi. Create() yöntemi yeni sürümü film parametre kabul eder ve yeni bir filmi filmler veritabanı tablosuna eklemek için mantığını içerir.

> [!NOTE] 
> 
> Bind özniteliği dikkat edin. HTML formundaki film ID özelliği güncelleştirmek istemediğiniz için bu özelliği açıkça dışlamak gerekiyor.


**4 – Controllers\HomeController.vb (değiştirilmiş oluşturma yöntemi) listeleme**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample4.vb)]

Visual Studio yeni bir filmi veritabanı oluşturmak için form oluşturmanın kolaylaştırır (bkz. Şekil 12) kaydedin. Aşağıdaki adımları uygulayın:

1. Kod düzenleyicisinde Create() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle**.
2. Onay kutusu etiketli doğrulayın **kesin türü belirtilmiş görünüm oluşturmak** denetlenir.
3. Gelen **görüntülemek içerik** açılır listesinde, değeri seçin *oluşturma*.
4. Gelen **görüntülemek veri sınıfı** açılır listesinde, değeri seçin *MovieApp.Movie*.
5. Tıklatın **Ekle** düğmesi yeni bir görünüm oluşturun.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image23.png)

**Şekil 12**: oluşturma görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image24.png))


Visual Studio görünümünü listeleme 5'te otomatik olarak oluşturur. Bu görünüm, her film sınıfının özelliklerine karşılık gelen alanları içeren bir HTML formuna içerir.

**Listing 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample5.aspx)]

> [!NOTE] 
> 
> Görünüm Ekle iletişim kutusu tarafından oluşturulan HTML formundaki bir kimliği form alanı oluşturur. ID sütunu bir kimlik sütunu olduğundan, bu form alanı gerekmez ve güvenli bir şekilde kaldırabilirsiniz.


Oluştur görünümünün ekledikten sonra veritabanına yeni film kayıtları ekleyebilirsiniz. F5 tuşuna basarak uygulamanızı çalıştırın ve Şekil 13 formunda görmek için Yeni Oluştur bağlantıyı tıklatın. Yeni bir filmi veritabanı kaydı tamamlamak ve form gönderme değilse oluşturulur.

Form doğrulama otomatik olarak Al dikkat edin. Film için bir yayın tarihi girmek ihmal ya da geçersiz yayın tarihi girin formu görünürler ve yayın tarihi alanı vurgulanır.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image25.png)

**Şekil 13**: yeni bir filmi veritabanı kaydı oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image26.png))


## <a name="editing-existing-database-records"></a>Var olan veritabanı kayıtlarını düzenleme

Önceki bölümlerde nasıl listelemek ve yeni veritabanı kayıtlarını oluşturma açıklanmıştır. Son Bu bölümde, var olan veritabanı kayıtlarını düzenleme nasıl ele alınmıştır.

İlk olarak, düzenleme formu oluşturmak ihtiyacımız. Bu adım, Visual Studio düzenleme formu bize için otomatik olarak oluşturur beri kolaydır. HomeController.vb sınıfı Visual Studio Kod düzenleyicisinde açın ve aşağıdaki adımları izleyin:

1. Kod düzenleyicisinde Edit() yönteme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (Şekil 14 bakın).
2. Etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.
3. Gelen **görüntülemek içerik** açılır listesinde, değeri seçin *Düzenle*.
4. Gelen **görüntülemek veri sınıfı** açılır listesinde, değeri seçin *MovieApp.Movie*.
5. Tıklatın **Ekle** düğmesi yeni bir görünüm oluşturun.

Bu adımları tamamladıktan görünümler klasörüne Edit.aspx adlı yeni bir görünüm ekler. Bu görünüm bir filmi kaydını düzenlemek için bir HTML formuna içerir.


[![Yeni Proje iletişim kutusu](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image27.png)

**Şekil 14**: düzenleme görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/_static/image28.png))


> [!NOTE] 
> 
> Düzenleme görünümü film kimliği özelliğine karşılık gelen bir HTML form alanından içerir. ID özelliği değerini düzenleme kişilerin istemediğiniz için bu formu alanını kaldırmanız gerekir.


Son olarak, biz veritabanı kaydını düzenlemeyi destekler, böylece giriş denetleyicisi değiştirmeniz gerekir. Güncelleştirilmiş HomeController sınıfı listeleme 6'yer alır.

**6 – Controllers\HomeController.vb (düzenleme yöntemleri) listeleme**

[!code-vb[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb/samples/sample6.vb)]

Listeleme 6'da, her iki Edit() yöntemi aşırı yüklemeleri için ilave bir mantık ekledim. İlk Edit() yöntemi, yönteme geçirilen ID parametresi için karşılık gelen film veritabanı kaydı döndürür. İkinci aşırı veritabanında film kaydı güncelleştirmeleri gerçekleştirir.

Özgün film almalı ve veritabanındaki mevcut film güncelleştirmek için ApplyPropertyChanges() çağrısı olduğunu dikkat edin.

## <a name="summary"></a>Özet

Bir ASP.NET MVC uygulaması oluşturmanın deneyiminin bir fikir vermek için bu öğreticinin amacı oluştu. I ASP.NET MVC web uygulaması oluşturma Active Server Pages veya ASP.NET uygulama oluşturma deneyimi için çok benzer olduğunu bulunan umuyoruz.

Bu öğreticide, ASP.NET MVC çerçevesi, yalnızca en temel özellikleri incelendi. Sonraki öğreticilerde, biz daha derin denetleyicileri, denetleyici eylemleri, görünümler, verileri görüntüleme ve HTML yardımcıları gibi konular halinde daha yakından inceleyin.

> [!div class="step-by-step"]
> [Önceki](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs.md)
