---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Veritabanı oluşturma | Microsoft Docs
author: microsoft
description: 2. adım, NerdDinner uygulamamız için veri LCV yanıtı gönderin ve tüm Akşam Yemeği tutan veritabanı oluşturma adımları gösterilmektedir.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756544"
---
<a name="create-a-database"></a>Bir veritabanı oluşturun
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 2 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 2. adım, NerdDinner uygulamamız için veri LCV yanıtı gönderin ve tüm Akşam Yemeği tutan veritabanı oluşturma adımları gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner 2. adım: veritabanı oluşturma

Bir veritabanı NerdDinner uygulamamız için tüm Akşam Yemeği ve RSVP verileri depolamak için kullanacağız.

Ücretsiz SQL Server Express edition'ı kullanarak veritabanı oluşturma adımları Göster (V2 sürümleriyle kullanarak kolayca yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)). SQL Server Express ve tam SQL Server ile tüm kodlar yazma çalışır.

### <a name="creating-a-new-sql-server-express-database"></a>Yeni bir SQL Server Express veritabanı oluşturma

Biz bizim web proje üzerinde sağ tıklayarak başlayın ve ardından **Add -&gt;yeni öğe** menü komutu:

![](create-a-database/_static/image1.png)

Bu, Visual Studio'nun "Yeni Öğe Ekle" iletişim kutusu getirir. Biz "Veri" kategoriye göre filtreleme ve "SQL Server veritabanı" öğe şablonu seçin:

![](create-a-database/_static/image2.png)

Biz "NerdDinner.mdf" oluşturun ve Tamam isabet istiyoruz SQL Server Express veritabanı adı. Visual Studio ardından bize Biz bu dosya için sunduğumuz \App eklemek isteyip istemediğinizi sorar\_veri dizini (zaten olan bir dizin hem okuma hem de ile Kurulum ve güvenlik ACL'leri yazma):

![](create-a-database/_static/image3.png)

"Evet"'yi ve müşterilerimize yeni bir veritabanı oluşturulur ve bizim çözüm Gezgini'ne eklenir:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Bizim veritabanı içinde tablo oluşturma

Artık yeni bir boş veritabanı var. Bazı tablolar için ekleyelim.

Bunu yapmak için veritabanlarını ve sunucuları yönetmek bize sağlayan Visual Studio "Sunucu Gezgini" sekmesinde penceresine giderek. SQL Server Express veritabanı \App içinde depolanan\_uygulamamız veri klasörü otomatik olarak görünür Sunucu Gezgini içinde. İsteğe bağlı olarak "Veritabanına bağlan" simge "Sunucu Gezgini" penceresinin üst kısmındaki (yerel ve uzak) ek SQL Server veritabanlarını listesine eklemek için kullanabiliriz:

![](create-a-database/_static/image5.png)

Bizim NerdDinner veritabanı – bizim azalma ve diğer RSVP edenleri bunları izlemek üzere depolamak için iki tablo ekleyeceğiz. Yeni tablolar veritabanımızdaki içindeki "Tablo" klasöre sağ tıklayarak ve "Yeni Tablo Ekle" menü komutu seçme oluşturabilirsiniz:

![](create-a-database/_static/image6.png)

Bu kurmamızı tablomuza şemasını yapılandırmak bir Tablo Tasarımcısı açılır. 10 veri sütunlarının "Azalma" tablomuza ekleyeceğiz:

![](create-a-database/_static/image7.png)

"DinnerID" sütunu tablo için benzersiz bir birincil anahtar olmasını istiyoruz. Biz "DinnerID" sütuna sağ tıklayıp "Birincil anahtarı Ayarla" menü öğesi'i seçerek yapılandırabilirsiniz:

![](create-a-database/_static/image8.png)

Bir birincil anahtar DinnerID hale getirmenin yanı sıra, ayrıca değeri tabloya yeni satır veri eklendikçe otomatik olarak artırılır "kimlik" sütunu olarak yapılandırır istiyoruz (ilk eklenen Dinner satıra bir DinnerID 1 olacaktır anlamına gelir, ikinci satır eklendi bir DinnerID olacaktır 2, vb.).

Bunu "DinnerID" sütun seçerek ve ardından "(kimlik mi)" özelliği "Evet" sütununda ayarlamak için "Sütun özellikleri" Düzenleyicisi'ni kullanın. Standart kimlik Varsayılanları kullanacağız (1'den başlar ve her yeni Dinner satırında 1 Artır):

![](create-a-database/_static/image9.png)

Biz ardından tablomuza kullanarak veya Ctrl-S yazarak kazanırsınız **File -&gt;Kaydet** menü komutu. Bu tablo adı için bize ister. Biz, bunu "Azalma" ad:

![](create-a-database/_static/image10.png)

Yeni azalma tablomuza veritabanımızda yer Sunucu Gezgini içinde sonra görünür.

Biz ardından yukarıdaki adımları yineleyin ve "RSVP" tablosu oluşturun. Bu tablo ile 3 sütun olması. Biz RsvpID sütun birincil anahtar olarak ayarlama ve ayrıca bir kimlik sütunu kolaylaştırır:

![](create-a-database/_static/image11.png)

Biz, kaydedin ve "RSVP" adını verin.

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Tablolar arasında yabancı anahtar ilişkisi ayarlama

Artık iki tablo veritabanımızdaki içinde var. Böylece her Akşam Yemeği satır biz buna sıfır veya daha fazla RSVP satır ile ilişkilendirebilirsiniz: Bu iki tablo arasında bir "bir-çok" ilişki kurmak için son şema tasarım adımımız olacaktır. RSVP tablonun "DinnerID" sütunun "Azalma" tablosunda "DinnerID" sütunu için yabancı anahtar ilişkisi yapılandırarak bunu.

Bunu yapmak için biz Tablo Tasarımcısı içine RSVP tablosunu sunucu Gezgini'nde çift tıklayarak açacaksınız. Ardından, "DinnerID" sütunda seçeneğini belirleyeceğiz sağ tıklayın ve "Relationshps..." bağlam menüsü komutu seçin:

![](create-a-database/_static/image12.png)

Bu tablolar arasındaki ilişkileri kurulumu için kullanabileceğiniz bir iletişim kutusu getirir:

![](create-a-database/_static/image13.png)

İletişim kutusuna yeni bir ilişki eklemek için "Ekle" düğmesine tıkladıktan. İlişki eklendikten sonra biz iletişim kutusunun sağında özellik kılavuzunda "Tabloları ve sütun belirtimi" ağaç görünümü düğümü genişletin ve ardından sağındaki "..." düğmesine tıklayın:

![](create-a-database/_static/image14.png)

"..." Düğmesine tıklayarak, hangi tablolar ve sütunlar ilişkide bulunan, hem de ilişki adı olanak belirtmenizi sağlayan başka bir iletişim kutusu getirir.

Biz "Azalma" olarak birincil anahtar tablosu değiştirin ve birincil anahtar olarak azalma tablo içindeki "DinnerID" sütunu seçin. RSVP tablomuza yabancı anahtar tablosuna ve RSVP olacaktır. Yabancı anahtar DinnerID sütun ilişkilendirilecek:

![](create-a-database/_static/image15.png)

RSVP tablodaki her satır Şimdi Akşam Yemeği tablosunda bir satıra ile ilişkili olacaktır. SQL Server, bizim için – başvurusal bütünlüğü korumak ve bize geçerli bir Akşam Yemeği satıra işaret etmemesi durumunda yeni RSVP satır eklemesini engellemek. Bu da bize varsa yine de ona başvuran satır RSVP Dinner satır silme engeller.

### <a name="adding-data-to-our-tables"></a>Bizim tablolarına veri ekleme

Şimdi bazı örnek veriler azalma tablomuza ekleyerek tamamlayın. Sunucu Gezgini içinde sağ tıklayarak ve "Tablo verilerini Göster" komutunu seçerek bir tabloya veri ekleyebiliriz:

![](create-a-database/_static/image16.png)

Daha sonra uygulamayı uygulama başlangıç olarak kullanabiliriz Dinner veri birkaç satırı ekleyeceğiz:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Sonraki adım

Size sunduğumuz veritabanı oluşturma işlemini tamamladınız. Artık sorgulamak ve güncelleştirmek için kullanabiliriz model sınıfları oluşturalım.

> [!div class="step-by-step"]
> [Önceki](create-a-new-aspnet-mvc-project.md)
> [İleri](build-a-model-with-business-rule-validations.md)
