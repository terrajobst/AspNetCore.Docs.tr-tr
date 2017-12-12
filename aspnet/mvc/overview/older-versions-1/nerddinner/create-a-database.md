---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: "Bir veritabanı oluşturun | Microsoft Docs"
author: microsoft
description: "2. adım Yemeği tümünün bulunduran bir veritabanı oluşturun ve veri NerdDinner uygulamamız için RSVP adımlarını gösterir."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 7635722fc357356edd06fb4cff301a8c4dfebbef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="create-a-database"></a>Bir veritabanı oluşturun
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 2 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 2. adım Yemeği tümünün bulunduran bir veritabanı oluşturun ve veri NerdDinner uygulamamız için RSVP adımlarını gösterir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner adım 2: veritabanı oluşturma

Biz bir veritabanı NerdDinner uygulamamız için tüm Yemeği ve RSVP verileri depolamak için kullanırsınız.

Ücretsiz SQL Server Express edition'ı kullanarak veritabanı oluşturma adımları Göster (hangi V2 birini kullanarak kolayca yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)). SQL Server Express ve tam SQL Server ile tüm yazma kod çalışır.

### <a name="creating-a-new-sql-server-express-database"></a>Yeni bir SQL Server Express veritabanı oluşturma

Biz bizim web projeye sağ tıklayarak başlayın ve ardından **Ekle -&gt;yeni öğe** menü komutu:

![](create-a-database/_static/image1.png)

Bu, Visual Studio'nun "Yeni Öğe Ekle" iletişim kutusunu getirir. Biz "Data" kategoriye göre filtre ve "SQL Server veritabanı" öğe şablonu seçin:

![](create-a-database/_static/image2.png)

Biz "NerdDinner.mdf" oluşturabilir ve Tamam isabet istiyoruz SQL Server Express veritabanı adı. Visual Studio sonra bize Biz bu dosya için bizim \App eklemek isteyip istemediğinizi sorar\_veri dizini (bir dizin zaten olduğu kurulum ile hem de okuma ve güvenlik ACL yazma):

![](create-a-database/_static/image3.png)

"Evet"'yi ve bizim yeni bir veritabanı oluşturulur ve bizim çözüm Gezgini'ne eklenir:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Bizim veritabanındaki tabloları oluşturma

Şimdi yeni bir boş veritabanı sunuyoruz. Bazı tablolar için ekleyelim.

Bunu yapmak için biz "Sunucu Gezgini" sekmesi penceresi bize veritabanları ve sunucuları yönetmek sağlayan Visual Studio içinde gitmek. SQL Server Express veritabanları \App depolanan\_uygulamamız veri klasörü otomatik olarak görünmesini sağlar Sunucu Gezgini içinde. Biz isteğe bağlı olarak "Connect to Database" simgesini "Sunucu Gezgini" pencerenin üst kısmında (yerel ve uzak) ek SQL Server veritabanları listesine eklemek için kullanabilirsiniz:

![](create-a-database/_static/image5.png)

Bizim NerdDinner veritabanına – bizim azalma ve diğer kişilere RSVP kabul izlemek için depolamak için iki tablo ekleyeceğiz. Yeni tablolar Veritabanımıza içinde "Tablo" klasöre sağ tıklayarak ve "Yeni Tablo Ekle" menü komutu seçme oluşturabilirsiniz:

![](create-a-database/_static/image6.png)

Bu bizim tablonun şeması yapılandırma kurmamızı sağlayan bir Tablo Tasarımcısı açar. Bizim "Azalma" tablosu için 10 veri sütunlarının ekleyeceğiz:

![](create-a-database/_static/image7.png)

"DinnerID" sütun tablosu için benzersiz bir birincil anahtar olmasını istiyoruz. Biz "DinnerID" sütunu sağ tıklayıp "Birincil anahtarı Ayarla" menü öğesini seçerek yapılandırabilirsiniz:

![](create-a-database/_static/image8.png)

Birincil anahtar DinnerID yapmanın yanı sıra da değeri otomatik olarak artar yeni veri satırları tabloya eklenen bir "kimlik" sütunu olarak yapılandırma istiyoruz (ilk eklenen Yemeği satıra 1 DinnerID olacaktır anlamına gelir, ikinci satır eklenir bir DinnerID sahip olur 2, vb.).

"DinnerID" sütun seçerek bunu ve "Evet" sütununa "(kimlik)" özelliğini ayarlamak için "Sütun özellikleri" Düzenleyicisi'ni kullanın. Standart kimlik Varsayılanları kullanacağız (1'den başlar ve her yeni Yemeği satırda 1 Artır):

![](create-a-database/_static/image9.png)

Biz sonra bizim tablo kullanarak veya Ctrl-S yazarak kurtulmuş olursunuz **dosya -&gt;kaydetmek** menü komutu. Bu tablo adı için bize ister. Biz "Azalma" adlandırın:

![](create-a-database/_static/image10.png)

Bizim yeni azalma tablo sonra Veritabanımıza server Explorer'da içinde görünecektir.

Biz ardından yukarıdaki adımları yineleyin ve bir "RSVP" tablo oluşturun. Bu tablo ile 3 sütuna sahip. Biz birincil anahtar olarak RsvpID sütun Kurulum ve ayrıca bir kimlik sütunu yapar:

![](create-a-database/_static/image11.png)

Biz kaydetmek ve "Cane" adını verin.

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Tablolar arasında yabancı anahtar ilişkisi ayarlama

Şimdi iki tablo Veritabanımıza içinde sunuyoruz. Böylece biz her Yemeği satır uygulayan sıfır veya daha fazla RSVP satır ilişkilendirebilir bizim son şema tasarım adımı – bu iki tablo arasında bir "bir-çok" ilişkisi kurmak için olacaktır. Bu "Azalma" tablosunda "DinnerID" sütunu için yabancı anahtar ilişkisi için RSVP tablonun "DinnerID" sütunu yapılandırarak gerçekleştiririz.

Bunu yapmak için şu Tablo Tasarımcısı'nda RSVP tablosu sunucu Gezgini'nde çift tıklayarak açacaksınız. Biz "DinnerID" sütun içerdiği, ardından seçersiniz sağ tıklatın ve "Relationshps..." i seçin bağlam menüsü komutu:

![](create-a-database/_static/image12.png)

Bu kurulum tablolar arasında ilişkiler için kullanabileceğiniz bir iletişim kutusu getirir:

![](create-a-database/_static/image13.png)

Biz iletişim kutusuna yeni bir ilişki eklemek için "Ekle" düğmesini tıklatın. İlişki eklendikten sonra biz "Tablo ve sütun belirtimi" ağaç görünümü ve özellik ızgarasının içinde iletişim kutusunun sağında düğümünü ve sağındaki "..." düğmesini tıklatıp:

![](create-a-database/_static/image14.png)

"..." Düğmesini tıklatarak hangi tablolar ve sütunlar ilişkide bulunan, aynı zamanda ilişki adı etmemizi sağlayan belirtmeniz kurmamızı sağlayan başka bir iletişim kutusunu getirir.

Biz "Azalma" olması için birincil anahtar tablosu değiştirin ve "DinnerID" sütun azalma tablodaki birincil anahtar olarak seçin. Bizim RSVP tablosu yabancı anahtar tablosu ve RSVP olacaktır. DinnerID sütun yabancı anahtar ilişkili olacaktır:

![](create-a-database/_static/image15.png)

Şimdi RSVP tablodaki her satır Yemeği tablosunda bir satırı ile ilişkilendirilir. SQL Server başvuru bütünlüğü bize – korumak ve bize geçerli Yemeği satır işaret etmiyorsa yeni RSVP satır ekleme engelleyebilirsiniz. Bu da bize hala kendisine başvuran satır RSVP varsa bir Yemeği satırın silinmesi engeller.

### <a name="adding-data-to-our-tables"></a>Veri bizim tablolarına ekleme

Şimdi bizim azalma tabloya bazı örnek veri ekleyerek tamamlayın. Sunucu Gezgini içinde sağ tıklayarak ve "Tablo verileri göster" komutunu seçerek biz verileri bir tabloya ekleyebilirsiniz:

![](create-a-database/_static/image16.png)

Biz daha sonra uygulamayı uygulama başlangıç olarak kullanabileceğiniz Yemeği veri birkaç satırı ekleyeceğiz:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Sonraki adım

Biz Veritabanımıza oluşturma işlemini tamamladınız. Şimdi biz sorgulamak ve güncelleştirmek için kullanabileceğiniz modeli sınıfları oluşturalım.

>[!div class="step-by-step"]
[Önceki](create-a-new-aspnet-mvc-project.md)
[sonraki](build-a-model-with-business-rule-validations.md)
