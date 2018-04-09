---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Ek kullanıcı bilgilerini (VB) depolamak | Microsoft Docs
author: rick-anderson
description: Bu öğreticide çok ilkel Konuk uygulama oluşturarak Biz bu soruyu yanıtlayın. Bunu yaparken, modeli için farklı seçenekleri ele alacağız...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9a8673e764ae94b12fbc01f81ef12ea4c133b7d5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="storing-additional-user-information-vb"></a>Ek kullanıcı bilgileri depolayan (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) veya [PDF indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> Bu öğreticide çok ilkel Konuk uygulama oluşturarak Biz bu soruyu yanıtlayın. Bunu yaparken, biz kullanıcı bilgilerini bir veritabanında modelleme için farklı seçenekler bakın ve ardından bu veriler üyelik çerçevesi tarafından oluşturulan kullanıcı hesaplarını ilişkilendirmek bkz.


## <a name="introduction"></a>Giriş

ASP. NET'in üyelik framework kullanıcıları yönetmek için esnek bir arabirim sunar. Üyelik API'si için kimlik bilgileri doğrulanıyor, şu anda oturum açmış kullanıcı hakkında bilgi alma, yeni bir kullanıcı hesabı oluşturma ve diğerlerinin yanı sıra bir kullanıcı hesabı silme yöntemlerini içerir. Her kullanıcı hesabı üyelik Framework'te yalnızca kimlik doğrulama ve temel kullanıcı hesabıyla ilgili görevleri gerçekleştirmek için gereken özellikleri içerir. Bu yöntemleri ve özellikleri tarafından yi [ `MembershipUser` sınıfı](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), bir kullanıcı hesabı üyelik Framework'te modeller. Bu sınıf gibi özelliklere sahip [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), ve [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), yöntemleri gibi ve [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) ve [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Görmemeleri, uygulamalar üyelik framework bulunmayan ek kullanıcı bilgilerini depolamak gerekir. Örneğin, bir çevrimiçi satıcısı kendi sevkiyat ve fatura adresleri, ödeme bilgileri, teslim Tercihler depolamak ve telefon numarası her bir kullanıcı izin gerekebilir. Ayrıca, sistemdeki her sipariş belirli kullanıcı hesabıyla ilişkilidir.

`MembershipUser` Sınıfı gibi özellikleri içermez `PhoneNumber` veya `DeliveryPreferences` veya `PastOrders`. Bu nedenle nasıl biz uygulama tarafından gerek duyulan kullanıcı bilgilerini izlemek ve sahip üyelik framework ile tümleştirmek? Bu öğreticide çok ilkel Konuk uygulama oluşturarak Biz bu soruyu yanıtlayın. Bunu yaparken, biz kullanıcı bilgilerini bir veritabanında modelleme için farklı seçenekler bakın ve ardından bu veriler üyelik çerçevesi tarafından oluşturulan kullanıcı hesaplarını ilişkilendirmek bkz. Haydi başlayalım!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>1. adım: Konuk uygulamanın veri modeli oluşturma

Bir veritabanı kullanıcı bilgilerini yakalama ve üyelik çerçevesi tarafından oluşturulan kullanıcı hesaplarıyla ilişkilendirmek için kullanılan teknikleri çeşitli vardır. Bu teknikler göstermek için biz böylece kullanıcı ile ilgili veriler çeşit yakalar öğretici web uygulamasını genişletmek gerekir. (Uygulama hizmetleri için gerekli tabloları yalnızca şu anda, uygulamanın veri modeli içeriyor `SqlMembershipProvider`.)

Kimliği doğrulanmış bir kullanıcı bir yorum burada bırakabilirsiniz bir çok basit Konuk uygulaması oluşturalım. Konuk açıklamaları depolamaya ek olarak, şimdi her bir kullanıcı kendi giriş Şehir, giriş sayfası ve imza depolamak izin ver. Sağlanırsa, kullanıcının ev Şehir, giriş sayfası ve imza kendisinin defterinizde ayrıldı her bir ileti görüntülenir.

### <a name="adding-theguestbookcommentstable"></a>Ekleme`GuestbookComments`tablosu

Konuk açıklamaları yakalamak üzere adlı bir veritabanı tablosu oluşturmak ihtiyacımız `GuestbookComments` gibi sütunlar olan `CommentId`, `Subject`, `Body`, ve `CommentDate`. Ayrıca her kayıt zorunda ihtiyacımız `GuestbookComments` tablo başvurusu yorum sol kullanıcı.

Bu tablo Veritabanımıza eklemek için Visual Studio'da Database Explorer gidin ve ayrıntılarına `SecurityTutorials` veritabanı. Tabloları klasörü sağ tıklatın ve eklemek yeni bir tablo seçin. Bu yeni bir tablo sütunları tanımlamak için bize sağlayan bir arabirim getirir.


[![Yeni bir tablo SecurityTutorials veritabanına ekleyin](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Şekil 1**: yeni bir tabloya eklemek `SecurityTutorials` veritabanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image3.png))


Ardından, tanımlamak `GuestbookComments`'s sütun. Adlı bir sütun eklemeye başlayın `CommentId` türü `uniqueidentifier`. Bu sütun her Konuk açıklamada benzersiz şekilde tanımlamak, bu nedenle izin vermeyecek `NULL` s ve tablonun birincil anahtarı olarak işaretleyin. İçin bir değer sağlama yerine `CommentId` her alan `INSERT`, biz olduğunu belirten yeni bir `uniqueidentifier` değeri otomatik olarak oluşturulacağı Bu alan için üzerinde `INSERT` sütunun varsayılan değer ayarlayarak `NEWID()`. Varsayılan değerini, birincil anahtar ve ayarları işaretleme bu ilk alan eklendikten sonra ekranınızın Şekil 2'de gösterilen ekran benzer görünmelidir.


[![CommentId adlı birincil bir sütun ekleyin](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Şekil 2**: birincil adlı sütun ekleme `CommentId` ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image6.png))


Ardından, adlı bir sütun ekler `Subject` türü `nvarchar(50)` ve adlı bir sütun `Body` türü `nvarchar(MAX)`, engelleyerek `NULL` hem sütunlardaki s. Adlı bir sütun ekleyin, `CommentDate` türü `datetime`. İzin verme `NULL` s ve kümesi `CommentDate` sütunun varsayılan değer olarak `getdate()`.

Kalan tek şey, bir kullanıcı hesabı her Konuk açıklama ile ilişkilendiren bir sütun eklemek için. Adlı bir sütun eklemek için bir seçenek olacaktır `UserName` türü `nvarchar(256)`. Bu uygun bir seçenek üyelik sağlayıcısı dışında kullanıldığında `SqlMembershipProvider`. Ancak kullanırken `SqlMembershipProvider`, biz Bu öğretici serisinde olarak `UserName` sütununda `aspnet_Users` tablo benzersiz olması garanti edilmez. `aspnet_Users` Tablonun birincil anahtarı `UserId` ve türü `uniqueidentifier`. Bu nedenle, `GuestbookComments` tablo gereken adlı bir sütun `UserId` türü `uniqueidentifier` (vermemek `NULL` değerleri). Devam etmek ve bu sütun ekleyin.

> [!NOTE]
> Biz anlatıldığı gibi [ *SQL Server üyelik şema oluşturma* ](creating-the-membership-schema-in-sql-server-vb.md) Öğreticisi, üyelik framework aynı paylaşmak birden çok web uygulamasına farklı kullanıcı hesapları ile etkinleştirmek için tasarlanmıştır Kullanıcı deposu. Kullanıcı hesapları farklı uygulamalara bölümleme olarak bunu yapar. Ve her kullanıcı adı bir uygulama içinde benzersiz olması garanti olsa da, aynı kullanıcı adı aynı kullanıcı deposunda kullanarak farklı uygulamalarda kullanılabilir. Bileşik yoktur `UNIQUE` kısıtlamasında `aspnet_Users` üzerinde tablo `UserName` ve `ApplicationId` alanlar, ancak üzerinde bir yalnızca `UserName` alan. Sonuç olarak, ASP.NET için olası\_iki (veya daha fazla) kayıtları aynı olması gereken kullanıcılar tablo `UserName` değeri. Yoktur, ancak bir `UNIQUE` kısıtlaması `aspnet_Users` tablonun `UserId` (birincil anahtar olduğundan) alan. A `UNIQUE` kısıtlaması önemlidir olmadan biz arasında yabancı anahtar kısıtlamasını kurulamıyor çünkü `GuestbookComments` ve `aspnet_Users` tabloları.


Ekledikten sonra `UserId` sütununda, araç çubuğunda Kaydet simgesine tıklayarak tablo kaydedin. Yeni bir tablo adı `GuestbookComments`.

İle katılmayı son sorunlardan biri sahibiz `GuestbookComments` tablosu: oluşturmamız gerekir bir [yabancı anahtar kısıtlaması](https://msdn.microsoft.com/library/ms175464.aspx) arasında `GuestbookComments.UserId` sütun ve `aspnet_Users.UserId` sütun. Bunu başarmak için yabancı anahtar ilişkileri iletişim kutusunu başlatmak için araç çubuğunda ilişki simgesine tıklayın. (Alternatif olarak, bu iletişim kutusunu Tablo Tasarımcısı menüsüne giderek ve ilişkileri seçerek başlatabilirsiniz.)

Yabancı anahtar ilişkilerini iletişim kutusunun sol alt köşesindeki Ekle düğmesini tıklatın. Biz yine de ilişkisine katılmak tabloları tanımlamanız gerekir ancak bu bir yeni yabancı anahtar kısıtlaması ekler.


[![Bir tablonun yabancı anahtar kısıtlamaları yönetmek için yabancı anahtar ilişkileri iletişim kutusunu kullanın](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Şekil 3**: bir tablonun yabancı anahtar kısıtlamaları yönetmek için yabancı anahtar ilişkileri iletişim kutusunu kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image9.png))


Ardından, "Tablo ve sütun belirtimlerini" satır sağındaki üç nokta simgesine tıklayın. Bu, biz belirtebilirsiniz birincil anahtar tablo ve sütun ve yabancı anahtar sütunu tablolar ve sütunlar iletişim kutusu başlatacak `GuestbookComments` tablo. Özellikle, seçin `aspnet_Users` ve `UserId` birincil anahtar tablo ve sütun olarak ve `UserId` gelen `GuestbookComments` tablosu yabancı anahtar sütunu olarak (Şekil 4'e bakın). Birincil ve yabancı anahtar tablolar ve sütunlar tanımladıktan sonra yabancı anahtar ilişkilerini iletişim kutusuna dönmek için Tamam'ı tıklatın.


[![Bir yabancı anahtar kısıtlaması arasında aspnet_Users ve GuesbookComments tabloları oluşturma](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Şekil 4**: bir yabancı anahtar kısıtlaması arasında kurmak `aspnet_Users` ve `GuesbookComments` tabloları ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image12.png))


Bu noktada yabancı anahtar kısıtlaması kuruldu. Bu sınırlama varlığını sağlar [ilişkisel bütünlüğü](http://en.wikipedia.org/wiki/Referential_integrity) hiçbir zaman olacağını mevcut olmayan kullanıcı hesabına başvuran bir konuk girişi güvence altına almak tarafından iki tablo arasında. Varsayılan olarak, bir yabancı anahtar kısıtlaması karşılık gelen alt kayıtları silinmesi için üst kaydına izin vermez. Kendi konuk açıklamaları ilk silinene kadar başka bir deyişle, bir kullanıcı bir veya daha fazla konuk açıklamaları yapar ve biz bu kullanıcı hesabını silmek denemesi, silme başarısız olur.

Yabancı anahtar kısıtlamaları, bir üst kaydı silindiğinde ilişkili alt kayıtları otomatik olarak silmek için yapılandırılabilir. Diğer bir deyişle, her kullanıcı hesabı silindiğinde, bir kullanıcının Konuk girişleri otomatik olarak silinir, böylece Biz bu yabancı anahtar kısıtlaması ayarlayabilirsiniz. Bunu başarmak için "Ekleme ve güncelleştirme belirtimi" bölümünü genişletin ve Cascade için "Kuralı sil" özelliğini ayarlayın.


[![Art arda silme için yabancı anahtar kısıtlamasını yapılandırma](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Şekil 5**: Cascade siler için yabancı anahtar kısıtlaması yapılandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image15.png))


Yabancı anahtar kısıtlaması kaydetmek için yabancı anahtar ilişkilerini dışında çıkmak için Kapat düğmesini tıklatın. Tablo ve bu kaydetmek için araç Kaydet simgeyi tıklatın ilişki.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Kullanıcının giriş Şehir, giriş sayfası ve imza depolanması

`GuestbookComments` Tabloda bir-çok ilişkisi olan kullanıcı hesaplarını paylaşır bilgilerini depolamak nasıl gösterilmektedir. Her kullanıcı hesabına ait yorumlara rastgele sayıda olabilir olduğundan, bu ilişki bağlantılar her açıklama belirli bir kullanıcıya geri bir sütun içeren yorum kümesini tutmak üzere bir tablo oluşturarak modellenir. Kullanırken `SqlMembershipProvider`, bu bağlantı, en iyi adlı bir sütun oluşturarak kurulduktan `UserId` türü `uniqueidentifier` ve bu sütun arasında yabancı anahtar kısıtlamasını ve `aspnet_Users.UserId`.

Biz artık üç sütun kullanıcının ev Şehir, giriş sayfası ve kendi konuk açıklamaları görünür imza depolamak için her kullanıcı hesabı ile ilişkilendirmeniz gerekir. Vardır bunu gerçekleştirmek için farklı yollar sayısı:

- <strong>Yeni sütun eklemek</strong><strong>`aspnet_Users`</strong><strong>veya</strong><strong>`aspnet_Membership`</strong><strong>tablo.</strong> Tarafından kullanılan şemasını değiştirdiğinden t bu yaklaşım önerilir değil `SqlMembershipProvider`. Bu karara yol haunt için geri gelin. Örneğin, ASP.NET gelecek bir sürümünde farklı bir kullanır ne `SqlMembershipProvider` şema. Microsoft ASP.NET 2.0 geçirmek için bir araç içerebilir `SqlMembershipProvider` ASP.NET 2.0 değiştirilmiş, ancak yeni şemaya veri `SqlMembershipProvider` şema, bu tür dönüştürme olmayabilir mümkün.

- **ASP kullanın. Ev Şehir, giriş sayfası ve imza için bir profil özelliği tanımlama NET'in profili çerçevesi.** ASP.NET ek kullanıcıya özgü verileri depolamak için tasarlanmış bir profil framework içerir. Üyelik framework gibi profili framework sağlayıcı modeli üzerinde oluşturulmuştur. .NET Framework ile birlikte bir `SqlProfileProvider` profil verilerini bir SQL Server veritabanında depolar. Aslında, bizim veritabanında tarafından kullanılan tablo zaten var. `SqlProfileProvider` (`aspnet_Profile`), uygulama hizmetlerini yeniden eklediğimiz zaman eklendiği gibi [ *SQL Server üyelik şema oluşturma* ](creating-the-membership-schema-in-sql-server-vb.md)Öğreticisi.   
  Profil framework'ün ana avantajı, profil özelliklerini tanımlamak geliştiricilere vermesidir `Web.config` – kod için ve temel alınan veri deposundan profil verileri seri hale getirmek için yazılması gerekir. Kısacası, profil özellikler kümesini tanımlar ve bunlarla kodunda çalışmak için son derece kolaydır. Ancak, profil sistemi sürüm oluşturma için geldiğinde istenen için çok bırakır, bunu burada, daha sonra veya var olanları kaldırılamaz veya değiştirilemez için eklenecek yeni kullanıcıya özgü özellikleri beklediğiniz sonra profili framework olmayabilir bir uygulamanız varsa  en iyi seçeneği. Ayrıca, `SqlProfileProvider` profil özelliklerini sonraki imkansız için (örneğin, bir ev Şehir, New York kaç kullanıcınız) doğrudan profil verileri karşı sorguları çalıştırmak yapma, yüksek oranda Normalleştirilmemiş bir şekilde depolar.   
  Profil framework hakkında daha fazla bilgi için bu öğreticinin sonunda "Başka okumalar" bölümüne bakın.

- <strong>Veritabanında yeni bir tablo için bu üç sütun ekleyin ve bu tablo arasında bire bir ilişki kurmak ve</strong><strong>`aspnet_Users`</strong><strong>.</strong> Bu yaklaşım değerinden biraz daha fazla iş profili framework ile ilgilidir, ancak ek kullanıcı özelliklerini veritabanında nasıl modellenir en büyük esneklik sağlar. Bu öğreticide kullanacağız seçenek budur.

Adlı yeni bir tablo oluşturacağız `UserProfiles` ev Şehir, giriş sayfası ve her kullanıcı için imza kaydetmek için. Veritabanı Gezgini penceresinde tabloları klasörü sağ tıklatın ve yeni bir tablo oluşturmak seçin. İlk sütun adı `UserId` ve türünü ayarlamak `uniqueidentifier`. İzin verme `NULL` değerleri ve sütun birincil anahtar olarak işaretleyin. Ardından, adlandırılmış sütunlar ekleyin: `HomeTown` türü `nvarchar(50)`; `HomepageUrl` türü `nvarchar(100)`; ve imza türü `nvarchar(500)`. Bu üç sütunların her biri kabul edebileceği bir `NULL` değeri.


[![UserProfiles tablosu oluşturma](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Şekil 6**: oluşturmak `UserProfiles` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image18.png))


Tabloyu kaydedin ve adlandırın `UserProfiles`. Son olarak, bir yabancı anahtar kısıtlaması arasında kurmak `UserProfiles` tablonun `UserId` alan ve `aspnet_Users.UserId` alan. Arasında yabancı anahtar kısıtlaması ile yaptığımız gibi `GuestbookComments` ve `aspnet_Users` tablolarda, siler basamaklı bu sınırlama vardır. Bu yana `UserId` alanındaki `UserProfiles` birincil anahtar, bu birden fazla kaydında olacaktır sağlar `UserProfiles` her kullanıcı hesabı için tablo. Bu tür ilişki bire olarak adlandırılır.

Oluşturulan veri modeli sahibiz, biz kullanmaya hazır olursunuz. Adım 2 ve 3'te biz nasıl şu anda oturum açmış kullanıcı görüntüleyebilir ve ev Şehir, giriş sayfası ve imza bilgilerini düzenle adresindeki arar. Adım 4'te Konuk yeni açıklamaları göndermek veya var olanları görüntülemek kimliği doğrulanmış kullanıcılar için arabirimi oluşturacağız.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>2. adım: kullanıcının giriş Şehir, giriş sayfası ve imza görüntüleme

Çeşitli şekillerde kendi giriş Şehir, giriş sayfası ve imza bilgilerini görüntülemek ve düzenlemek şu anda oturum açmış kullanıcı izni vardır. El ile kullanıcı arabirimi kutusuyla oluşturuyoruz ve etiket denetimleri veya biz DetailsView denetimi gibi Web denetimleri veri birini kullanabilirsiniz. Veritabanı gerçekleştirmek için `SELECT` ve `UPDATE` biz ADO.NET yazabilirsiniz deyimleri kod sayfamızı ait arka plandaki kod sınıfı veya alternatif olarak, bildirim temelli bir yaklaşım SqlDataSource ile uygulamadığınız. İdeal olarak uygulamamız biz ya da program aracılığıyla sayfanın arka plandaki kod sınıfı veya bildirimli olarak ObjectDataSource Denetimi aracılığıyla çağıramadı bir katmanlı mimarisi içerecektir.

Bu öğretici seri form kimlik doğrulaması, yetkilendirme, kullanıcı hesapları ve rolleri odaklanan beri olmayacaktır kapsamlı bir tartışma bu farklı veri erişim seçenekleri veya neden bir katmanlı mimarisi SQL deyimlerini doğrudan Yürütülüyor üzerinden tercih edilir ASP.NET sayfasından. Size yol DetailsView ve SqlDataSource – hızlı ve kolay seçeneği – kullanarak kullanacağım ancak açıklanan kavramları kesinlikle alternatif Web denetimleri ve veri erişim mantığı için uygulanabilir. ASP.NET verilerle çalışma hakkında daha fazla bilgi için başvurmak my *[, ASP.NET 2.0 verilerle çalışma](../../data-access/index.md)* öğretici serisi.

Açık `AdditionalUserInfo.aspx` sayfasındaki `Membership` klasörü ve ID özelliğini ayarlamak bu sayfaya DetailsView denetimini ekleme `UserProfile` ve temizleme kendi `Width` ve `Height` özellikleri. DetailsView'un akıllı etiket genişletin ve yeni bir veri kaynağı denetime bağlamak seçin. Bu veri kaynağı Yapılandırma Sihirbazı'nı başlatır (bkz. Şekil 7). İlk adım, veri kaynağı türünü belirtmenizi ister. Biz doğrudan bağlanın kalacaklarını beri `SecurityTutorials` veritabanı, veritabanı simgesini seçin belirtme `ID` olarak `UserProfileDataSource`.


[![UserProfileDataSource adlı yeni bir SqlDataSource denetim ekleme](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Şekil 7**: yeni bir SqlDataSource denetimi adlı eklemek `UserProfileDataSource` ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image21.png))


Sonraki ekranda veritabanını kullanacak şekilde ister. Biz bağlantı dizesinde tanımladığınız `Web.config` için `SecurityTutorials` veritabanı. Bu bağlantı dizesi adı – `SecurityTutorialsConnectionString` – aşağı açılan listesinde olmalıdır. Bu seçeneği seçin ve İleri'yi tıklatın.


[![Aşağı açılan listeden SecurityTutorialsConnectionString seçin](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Şekil 8**: seçin `SecurityTutorialsConnectionString` aşağı açılan listeden ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image24.png))


Sonraki ekranda bize ve sütunların sorguya belirtmenizi ister. Seçin `UserProfiles` tablo aşağı açılan listeden ve tüm sütunları denetleyin.


[![Getir tüm sütunları UserProfiles tablosundan yedekleyin](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Şekil 9**: getir geri tüm sütun `UserProfiles` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image27.png))


Şekil 9 döndürür geçerli sorgu *tüm* kayıtlarının `UserProfiles`, ancak biz yalnızca şu anda oturum açmış kullanıcının kaydında ilgilendiğiniz. Eklemek için bir `WHERE` yan tümcesi tıklatın `WHERE` Ekle getirmek için düğmesi `WHERE` yan tümcesi iletişim kutusu (bkz. Şekil 10). Burada sütunu filtre, işleç ve filtre parametrenin kaynağı seçebilirsiniz. Seçin `UserId` sütun ve "İşleci olarak =" olarak.

Ne yazık ki şu anda oturum açmış kullanıcının döndürmek için yerleşik parametre kaynağı yok `UserId` değeri. Bu değer programlı olarak yakalayın gerekecektir. Bu nedenle, "None," Ekle parametre eklemek için düğmesine tıklayın ve Tamam'ı tıklatın Kaynak aşağı açılan listeye ayarlayın.


[![Kullanıcı kimliği sütunu bir filtre parametresi ekleyin](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Şekil 10**: bir filtre parametresi eklemek `UserId` sütun ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image30.png))


Tamam'ı tıklattıktan sonra Şekil 9'da gösterilen ekranına döndürülür. Bu süre, ancak, ekranın altındaki SQL sorgusu içermelidir bir `WHERE` yan tümcesi. "Test sorgu" ekranına geçmek için İleri'yi tıklatın. Burada, sorgu çalıştırabilirsiniz ve sonuçlarını görebilirsiniz. Sihirbazı tamamlamak için Son'u tıklatın.

Veri Kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra Visual Studio Sihirbazı'nda belirtilen ayarlara dayanan SqlDataSource denetimi oluşturur. Ayrıca, el ile BoundFields SqlDataSource tarafından 's döndürülen her sütun için DetailsView ekler `SelectCommand`. Göstermek için gerekli `UserId` kullanıcı, bu değer bilmeniz gerekmez. bu yana DetailsView alanındaki. Bu alan DetailsView denetimin bildirim temelli biçimlendirmeden doğrudan kaldırın veya "Alanları Düzenle" Akıllı etiketten bağlantısını tıklatarak.

Bu noktada sayfanızın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

SqlDataSource denetiminin programlı olarak ayarlamak ihtiyacımız `UserId` parametresi için geçerli oturum açmış kullanıcının `UserId` veri seçilmeden önce. Bu olay işleyicisi SqlDataSource için 's oluşturarak gerçekleştirilebilir `Selecting` olay ve aşağıdaki ekleme kodu vardır:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Yukarıdaki kod, şu anda oturum açmış kullanıcıya başvuru çağırarak elde ederek başlatır `Membership` sınıfının `GetUser` yöntemi. Bu döndürür bir `MembershipUser` nesnesi, özelliği `ProviderUserKey` özelliği içeren `UserId`. `UserId` Değeri SqlDataSource için 's sonra atanan `@UserId` parametresi.

> [!NOTE]
> `Membership.GetUser()` Yöntemi şu anda oturum açmış kullanıcı hakkındaki bilgileri döndürür. Anonim kullanıcı sayfasını ziyaret ettiğinizde, bir değeri döndürür `Nothing`. Böyle bir durumda, bu neden bir `NullReferenceException` okumaya çalışırken kod aşağıdaki satırda `ProviderUserKey` özelliği. Elbette, biz hakkında endişelenmeniz gerekmez `Membership.GetUser()` hiçbir döndürme `AdditionalUserInfo.aspx` yalnızca kimliği doğrulanan kullanıcılar bu klasördeki ASP.NET kaynaklarına erişebilir böylece biz URL yetkilendirmesi önceki öğreticide yapılandırıldığı sayfa. Anonim erişime izin verilir burada sayfasında şu anda oturum açmış kullanıcı hakkındaki bilgileri erişmeniz gerekiyorsa, denetlediğinizden emin olun `MembershipUser` döndürülen nesne `GetUser()` yöntemi değil hiçbir şey özelliklerini başvuran önce.


Ziyaret ettiğiniz varsa `AdditionalUserInfo.aspx` sayfa bir tarayıcı üzerinden herhangi bir satır eklemek henüz için boş bir sayfa görürsünüz `UserProfiles` tablo. Adım 6'otomatik olarak yeni bir satır eklemek için CreateUserWizard denetimi özelleştirmek nasıl ele alacağız `UserProfiles` yeni bir kullanıcı hesabı oluşturulduğunda, tablo. Şimdilik, ancak biz el ile tabloda bir kayıt oluşturmak gerekir.

Visual Studio'da Database Explorer gidin ve Tablolar klasörünü genişletin. Sağ `aspnet_Users` tablo ve "Tablo verileri tablodaki kayıtları görmek için Göster" seçin; aynı şeyi yapmak `UserProfiles` tablo. Şekil 11 dikey olarak döşenir olduğunda bu sonuçları gösterir. My veritabanında şu anda yok `aspnet_Users` Bruce'a, Gamze ve Tito kaydeder, ancak hiç kayıt `UserProfiles` tablo.


[![Aspnet_Users içeriğini ve UserProfiles tabloları görüntülenir](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Şekil 11**: içeriğini `aspnet_Users` ve `UserProfiles` tabloları görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image33.png))


Yeni bir kayıt eklemek `UserProfiles` için değerler el ile yazarak tablo `HomeTown`, `HomepageUrl`, ve `Signature` alanları. Geçerli bir almak için en kolay yolu `UserId` yeni değer `UserProfiles` kaydıdır seçmek için `UserId` belirli kullanıcı hesabında alanının `aspnet_Users` tablo ve kopyalayın ve yapıştırın `UserId` alanındaki `UserProfiles`. Şekil 12 gösterir `UserProfiles` Bruce'a için yeni bir kayıt eklendikten sonra tablo.


[![Bir kayıt için UserProfiles için Bruce'a eklendi](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Şekil 12**: A kaydı eklendiği `UserProfiles` Bruce'a için ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image36.png))


Geri dönüp `AdditionalUserInfo.aspx page`, Bruce'a olarak oturum açmış. Şekil 13 gösterildiği gibi Bruce'a'nın ayarları görüntülenir.


[![Şu anda ziyaret gösterilen HIS ayarları kullanıcıdır](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Şekil 13**: şu anda ziyaret kullanıcıdır gösterilen HIS ayarları ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> İleri ve el ile kayıtları eklemek Git `UserProfiles` her üye kullanıcı için tablo. Adım 6'otomatik olarak yeni bir satır eklemek için CreateUserWizard denetimi özelleştirmek nasıl ele alacağız `UserProfiles` yeni bir kullanıcı hesabı oluşturulduğunda, tablo.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>3. adım: Kullanıcı kendi giriş Şehir, giriş sayfası ve imza düzenlemek izin verme

Şu anda oturum açmış olan kullanıcının kendi giriş Şehir, giriş sayfası ve imza ayarı bu noktada görüntüleyebilirsiniz, ancak henüz bunları değiştiremezler. Şimdi veri düzenlenebilir şekilde DetailsView denetimi güncelleştirin.

Biz yapmanız gereken ilk şey eklemektir bir `UpdateCommand` SqlDataSource için belirtme `UPDATE` execute deyimi ve ilgili parametreleri. SqlDataSource seçin ve Özellikler penceresinden komut ve parametre Düzenleyicisi iletişim kutusu çağrılırken veUpdateQuery özelliğinin yanındaki üç noktaya tıklayın. Aşağıdaki girin `UPDATE` textbox INTO deyimi:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Ardından, bir parametre SqlDataSource denetiminin oluşturacak "Parametreleri Yenile" düğmesini tıklatın `UpdateParameters` her parametre için koleksiyon `UPDATE` deyimi. Tüm parametreleri kümesi için kaynak None olarak bırakın ve iletişim kutusunu doldurmak için Tamam düğmesini tıklatın.


[![SqlDataSource'nın UpdateCommand ve UpdateParameters belirtin](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Şekil 14**: SqlDataSource's belirtin `UpdateCommand` ve `UpdateParameters` ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image42.png))


Eklemeleri nedeniyle düzenleme denetimi artık destekleyebilir DetailsView SqlDataSource denetimi yapılan. DetailsView'un akıllı etiketten "Düzenlemeyi etkinleştir" onay kutusunu işaretleyin. Bu denetimin bir CommandField ekler `Fields` koleksiyonuyla kendi `ShowEditButton` özelliği True olarak ayarlanmış. Bu bir Düzenle düğmesi DetailsView salt okunur modda ve Güncelleştirmesi'nde görüntülenir ve görüntülenen zaman iptal düğmeleri düzenleme moduna işler. Düzenle'yi tıklatın kullanıcının gerektiren yerine ancak biz DetailsView işleme "her zaman düzenlenebilir" bir durumda DetailsView denetimin ayarlayarak olabilir [ `DefaultMode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) için `Edit`.

Bu değişikliklerle DetailsView denetiminizin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

CommandField eklenmesi unutmayın ve `DefaultMode` özelliği.

Devam etmek ve bu sayfayı bir tarayıcı aracılığıyla test. Karşılık gelen bir kayda sahip bir kullanıcı ile ziyaret eden `UserProfiles`, kullanıcının ayarları düzenlenebilir arabiriminde görüntülenir.


[![Düzenlenebilir bir arabirim DetailsView işler](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Şekil 15**: DetailsView düzenlenebilir bir arabirim oluşturur ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image45.png))


Deneyin Güncelleştir düğmesini tıklatarak ve değerlerini değiştirme. Hiçbir şey olmaz gibi görünür. Geri gönderimin yoktur ve değerleri veritabanına kaydedilir, ancak kaydetme oluştu hiçbir visual geri bildirim yoktur.

Bu sorunu gidermek için Visual Studio'ya geri dönün ve etiket denetimi DetailsView üstüne ekleyin. Ayarlama, `ID` için `SettingsUpdatedMessage`, kendi `Text` "ayarlarınızı güncellendi," özelliği ve kendi `Visible` ve `EnableViewState` özelliklerine `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

Görüntülenecek ihtiyacımız `SettingsUpdatedMessage` DetailsView güncelleştirildiğinde etiket. Bunu gerçekleştirmek için DetailsView'un için bir olay işleyicisi oluşturun `ItemUpdated` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Geri dönüp `AdditionalUserInfo.aspx` sayfasında bir tarayıcıdan ve verileri güncelleştirin. Bu süre, yararlı durum iletisi görüntülenir.


[![Kısa bir ileti görüntülenen zaman ayarları güncel değil](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Şekil 16**: ayarları güncelleştirildiğinde kısa bir ileti görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> İstenen için çok arabirimi bırakır düzenleme kullanıcının DetailsView denetimi. Standart boyutlu metin kutuları kullanır, ancak imza alan büyük olasılıkla çok satırlı metin kutusu olmalıdır. Giriş sayfası URL'si girdiyseniz, "http://" veya "https://" ile başladığından emin olmak için bir RegularExpressionValidator kullanılmalıdır. Ayrıca, denetiminin DetailsView itibaren kendi `DefaultMode` özelliğini `Edit`, İptal düğmesi hiçbir şey yapmaz. Ya da kaldırılması gerekir, tıklatıldığında yönlendirme kullanıcı başka bir sayfaya (gibi `~/Default.aspx`). Bu geliştirmeler için okuyucu bir alıştırma olarak bırakın.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Bağlantı ekleme`AdditionalUserInfo.aspx`ana sayfa içinde sayfa

Şu anda Web sitesi için tüm bağlantılar sağlamaz `AdditionalUserInfo.aspx` sayfası. Bunu ulaşmak için tek yolu, doğrudan tarayıcının adres çubuğuna sayfanın URL'sini girmektir. Bu sayfaya bir bağlantı ekleyelim `Site.master` ana sayfa.

Ana sayfaya LoginView Web denetiminde içerdiğini geri çağırma kendi `LoginContent` kimliği doğrulanmış ve anonim ziyaretçiler için farklı bir biçimlendirme görüntüler ContentPlaceHolder. LoginView denetimin güncelleştirme `LoggedInTemplate` bir bağlantı eklemek için `AdditionalUserInfo.aspx` sayfası. (Link) bu değişiklikleri yaptıktan sonra denetimin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Eklenmesi Not `lnkUpdateSettings` köprü denetimine `LoggedInTemplate`. Yerinde Bu bağlantı ile kimliği doğrulanan kullanıcılar hızlı bir şekilde görüntülemek ve ev Şehir, giriş sayfası ve imza ayarlarını değiştirmek için sayfasına atlayabilirsiniz.

## <a name="step-4-adding-new-guestbook-comments"></a>4. adım: Yeni Konuk açıklama ekleme

`Guestbook.aspx` Sayfasıdır, kimliği doğrulanmış kullanıcıların Konuk görüntülemek ve bir yorum yazın. Yeni konuk açıklamaları eklemek için arabirimi oluşturmayla başlayalım.

Açık `Guestbook.aspx` sayfasında Visual Studio'da ve biri yeni açıklamanın konu diğeri kendi gövdesi için iki TextBox oluşan bir kullanıcı arabirimi oluşturmak. İlk metin kutusu denetiminin ayarlamak `ID` özelliğine `Subject` ve kendi `Columns` 40 özelliğine; ayarlanmış ikinci 's `ID` için `Body`, kendi `TextMode` için `MultiLine`ve kendi `Width` ve `Rows` "% 95" ve 8 özelliklerini sırasıyla. Kullanıcı arabirimi tamamlamak için adlı bir düğme Web denetim eklemek `PostCommentButton` ve kendi `Text` "Post bilgisayarınızı açıklamaya" özelliği.

Her Konuk yorum bir konu ve gövde gerektirdiğinden, bir RequiredFieldValidator her metin kutuları için ekleyin. Ayarlamak `ValidationGroup` "EnterComment"; Bu denetimlere özelliğine benzer şekilde, ayarlama `PostCommentButton` denetimin `ValidationGroup` "EnterComment" özelliğine. ASP hakkında daha fazla bilgi için. NET'in doğrulama denetimleri, kullanıma [ASP.NET Form doğrulama](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [ASP.NET 2. 0 doğrulama denetimleri Dissecting](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)ve [doğrulama sunucu denetimleri öğretici](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) üzerinde [W3Schools](http://www.w3schools.com/).

Kullanıcı arabirimi hazırlayın sonra sayfanızın bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Kullanıcı arabirimi tam bizim sonraki yeni bir kayıt eklemek için bir görevdir `GuestbookComments` tablosundan `PostCommentButton` tıklandığında. Bu çeşitli şekillerde gerçekleştirilebilir: ADO.NET kodu düğmenin yazabilirsiniz `Click` olay işleyicisi; SqlDataSource denetimi sayfasına ekleme, yapılandırın, `InsertCommand`ve ardından arama kendi `Insert` yönteminden `Click` olay işleyici; veya Biz yeni konuk açıklamaları eklemek için sorumlu bir orta katman oluşturun ve bu işlevi çağırmak `Click` olay işleyicisi. Adım 3'te bir SqlDataSource kullanarak Aranan olduğundan, ADO.NET kodu buraya kullanalım.

> [!NOTE]
> Bir Microsoft SQL Server veritabanından veri programlı olarak erişmek için kullanılan ADO.NET sınıflarını bulunan `System.Data.SqlClient` ad alanı. Bu ad alanı sayfanızın arka plan kodu sınıfına içe aktarmanız gerekebilir (yani, `Imports System.Data.SqlClient`).


İçin bir olay işleyicisi oluşturun `PostCommentButton`'s `Click` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click` Olay işleyicisini başlatır kullanıcı tarafından sağlanan verilerin geçerli olup olmadığını kontrol ederek. Değilse, bir kayıt eklemeden önce olay işleyicisi çıkar. Sağlanan veri geçerli olduğunu varsayarak şu anda oturum açmış kullanıcının `UserId` değeri alınan ve depolanan `currentUserId` yerel değişken. Bu değer sağlamanız gerekir çünkü gerekli bir `UserId` değeri bir kayıtta eklerken `GuestbookComments`.

Bağlantı dizesi için `SecurityTutorials` veritabanı alınır `Web.config` ve `INSERT` SQL deyimi belirtildi. A `SqlConnection` nesne sonra oluşturulan ve açılır. Ardından, bir `SqlCommand` nesnesi oluşturulur ve parametre değerlerini kullanılan `INSERT` sorgu atanır. `INSERT` Deyimi sonra yürütülür ve bağlantı kapatıldı. Olay işleyicisi sonunda `Subject` ve `Body` kutularındaki `Text` özellikleri temizlenmiştir böylece kullanıcının değerleri arasında geri gönderme kalıcı değildir.

Devam etmek ve bu sayfasını bir tarayıcıda çıkışı test. Bu sayfa olduğundan `Membership` klasörü, anonim ziyaretçiler için erişilebilir değil. Bu nedenle, (zaten varsa) ilk kez oturum gerekecektir. İçine bir değer girin `Subject` ve `Body` kutularındaki tıklatıp `PostCommentButton` düğmesi. Eklenecek yeni bir kayıt neden olacak `GuestbookComments`. Geri göndermede, konu ve gövde sağladığınız kutularındaki metinleri temizlenir.

' I tıklattıktan sonra `PostCommentButton` düğmesi var olan hiçbir görsel geribildirim yorum Konuk eklenen. Biz yine adım 5'te gerçekleştiririz varolan konuk açıklamaları görüntülemek için bu sayfayı güncelleştirmeniz gerekir. Biz bunu yapmaya sonra yeni eklenen açıklamayı yeterli visual geribildirim sağlama Açıklamalar listesinde görünür. Şimdilik, Konuk yorumunuzu içeriğini inceleyerek kaydedilmiş onaylayın `GuestbookComments` tablo.

Şekil 17 gösterir içeriğini `GuestbookComments` iki açıklamaları sol sonra tablo.


[![Konuk yorumları GuestbookComments tablosundaki görebilirsiniz](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Şekil 17**: konuk açıklamaları görebilirsiniz `GuestbookComments` tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Bir kullanıcı olası içeren Konuk açıklama ekleme girişiminde bulunursa tehlikeli biçimlendirmeyi – HTML – ASP.NET özel durum oluşturacak bir `HttpRequestValidationException`. Neden durum, bu özel durumun ve potansiyel olarak tehlikeli olabilecek değerleri göndermek için kullanıcılara izin ver hakkında daha fazla bilgi için başvurun [istek doğrulama teknik](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>5. adım: Varolan Konuk yorumları listeleme

Ziyaret eden kullanıcı yorumları bırakarak yanı sıra `Guestbook.aspx` sayfa Konuk ait varolan yorumları görmeye olmalıdır. Bunu gerçekleştirmek için adlı ListView denetimine ekleme `CommentList` sayfanın altına.

> [!NOTE]
> ListView denetimi ASP.NET sürüm 3.5 yeni bir özelliktir. Çok özelleştirilebilir ve esnek bir düzende öğelerinin bir listesini görüntülemek, ancak yine yerleşik düzenleme, ekleme, silme, disk belleği ve GridView gibi işlevsellik sıralama sunmak üzere tasarlanmıştır. ASP.NET 2.0 kullanıyorsanız, DataList veya yineleyici denetim kullanmanız gerekir. ListView kullanma hakkında daha fazla bilgi için bkz: [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog girişi, [asp: ListView denetimi](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)ve my makale [ListView denetimi ile verilerde görüntüleme](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


ListView kullanıcının akıllı etiket açın ve veri kaynağı Seç açılan listeden denetimini yeni bir veri kaynağına bağlama. 2. adımda gördüğümüz gibi bu veri kaynağı Yapılandırma Sihirbazı'nı başlatır. Veritabanı simgesini seçin, sonuçta elde edilen SqlDataSource ad `CommentsDataSource`, Tamam'ı tıklatın. Ardından, `SecurityTutorialsConnectionString` bağlantı dizesi aşağı açılan listeden ve İleri'yi tıklatın.

Bu noktada 2. adımda biz sorgu için veri çekme tarafından belirtilen `UserProfiles` tablo aşağı açılan listeden ve döndürülecek olan sütunları seçerek (geri Şekil 9'a bakın). Bu süre, ancak yalnızca kayıtlarından geri çeker bir SQL deyimi oluşturabilir istiyoruz `GuestbookComments`, ancak aynı zamanda commenter'ın ev Şehir, giriş sayfası, imza ve kullanıcı adı. Bu nedenle, "özel bir SQL deyimi veya saklı yordam belirtin" radyo düğmesini seçin ve İleri'yi tıklatın.

Bu "Tanımlamak özel deyimleri veya saklı yordamlar" ekranını getirir. Grafik sorgusu oluşturmak için Sorgu Oluşturucusu düğmesini tıklatın. Sorgu Oluşturucusu sorgu istiyoruz tabloları belirtmek için bize isteyerek başlar. Seçin `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tablolar ve Tamam'ı tıklatın. Bu, tüm üç tabloları tasarım yüzeyine ekler. Yabancı anahtar kısıtlamaları arasından olduğundan `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tabloları, Sorgu Oluşturucu otomatik olarak `JOIN` s Bu tablolar.

Kalan tek şey döndürülecek olan sütunları belirtmek için. Gelen `GuestbookComments` tablo seçin `Subject`, `Body`, ve `CommentDate` sütunları; return `HomeTown`, `HomepageUrl`, ve `Signature` sütunlarından `UserProfiles` tablo; ve geri dönüp `UserName` gelen`aspnet_Users`. Ayrıca, Ekle "`ORDER BY CommentDate DESC`" sonuna `SELECT` böylece en son gönderileri ilk döndürülen sorgu. Bu seçimleri yaptıktan sonra Sorgu Oluşturucusu arabiriminizi Şekil 18'ekran şuna benzemelidir.


[![Constructed sorgu GuestbookComments, UserProfiles ve aspnet_Users tablolarını birleştirir](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Şekil 18**: oluşturulan sorgula `JOIN` s `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tabloları ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image54.png))


Sorgu Oluşturucu penceresini kapatın ve "Tanımlamak özel deyimleri veya saklı yordamlar" ekranına geri dönmek için Tamam'ı tıklatın. Gelişmiş sorgu sonuçlarını Test Query düğmesini tıklatarak görüntüleyebileceğiniz "Test sorgu" ekran için İleri'yi tıklatın. Hazır olduğunuzda, veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın.

Veri Kaynağı Yapılandırma Sihirbazı'nda Adım 2, ilişkili DetailsView denetimi biz tamamlanmış olduğunda `Fields` koleksiyonu BoundField tarafından döndürülen her sütun için eklenecek güncelleştirildi `SelectCommand`. ListView ancak değişmeden kalır; Biz yine düzenini tanımlamanız gerekir. ListView'ın düzeni el ile bildirim temelli biçimlendirme veya akıllı etiket "ListView yapılandırma" seçeneğinden oluşturulabilir. I genellikle işaretleme el ile tanımlama tercih eder, ancak ne olursa olsun, en iyi bir yöntemdir kullanın.

I aşağıdakileri kullanarak sonuçlandı `LayoutTemplate`, `ItemTemplate`, ve `ItemSeparatorTemplate` my ListView denetimi için:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate` İşaretleme tanımlar denetim tarafından yayılan, while `ItemTemplate` SqlDataSource tarafından döndürülen her bir öğeyi işler. `ItemTemplate`'S elde edilen biçimlendirme yerleştirilir `LayoutTemplate`'s `itemPlaceholder` denetim. Ek olarak `itemPlaceholder`, `LayoutTemplate` sayfa (varsayılan) başına yalnızca 10 konuk açıklamaları gösteren için ListView sınırlar bir DataPager denetim içerdiğini ve disk belleği arabirimi işler.

My `ItemTemplate` her Konuk açıklamanın konu görüntüler bir `<h4>` konu yer gövdesi bir öğesiyle. Gövde görüntülemek için kullanılan sözdizimi tarafından döndürülen veri alır `Eval("Body")` Veri bağlamada bildirimi, onu bir dizeye dönüştürür ve değiştirir satır sonları ile `<br />` öğesi. Bu dönüştürme boşluk HTML tarafından göz ardı edilir beri yorum gönderirken girilen satır sonları göstermek için gereklidir. Kullanıcının imza italik, kullanıcının ev Şehir, bir bağlantı kendi giriş sayfası, tarih ve saat açıklama yapıldı ve yorum sol kişiye kullanıcı adı ve ardından gövdesinde altında görüntülenir.

Bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Adım 5'i burada görüntülenen defterinizde eklenecek yorumlar görmeniz gerekir.


[![Guestbook.aspx şimdi Konuk'ın açıklamaları görüntüler](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Şekil 19**: `Guestbook.aspx` şimdi Konuk'ın açıklamaları görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image57.png))


Yeni bir açıklama için konuk eklemeyi deneyin. Üzerine tıklayarak `PostCommentButton` düğmesi sayfaya geri gönderir ve yorum veritabanına eklenir ancak ListView denetimine yeni yorum göstermek için güncelleştirilmez. Bu durum ya da düzeltilebilir:

- Güncelleştirme `PostCommentButton` düğmenin `Click` olan ListView denetimin çağırır için olay işleyicisini `DataBind()` yeni yorum veritabanına yerleştirilmesi sonra yöntemi veya
- ListView denetimin ayarı `EnableViewState` özelliğine `False`. Denetimin görünüm durumunu devre dışı bırakarak, temel alınan veriler her geri gönderme üzerinde rebind gerekir çünkü bu yaklaşım çalışır.

Bu öğretici indirilebilir öğretici Web sitesi her iki tekniği de gösterilmektedir. ListView denetimin `EnableViewState` özelliğine `False` ve ListView verileri programla rebind için gereken kodu yok `Click` olay işleyicisi kılınmıştır ancak.

> [!NOTE]
> Şu anda `AdditionalUserInfo.aspx` sayfası, kendi giriş Şehir, giriş sayfası ve imza ayarlarını görüntülemek ve düzenlemek kullanıcı sağlar. Güncelleştirmek iyi olabilir `AdditionalUserInfo.aspx` oturum açan kullanıcının konuk açıklamaları görüntülemek için. Diğer bir deyişle, inceleme ve kendi bilgi değiştirme ek olarak, bir kullanıcı ziyaret edebilirsiniz `AdditionalUserInfo.aspx` aynen geçmişte yapıldığında hangi konuk açıklamaları görmek için sayfayı. I bunu bir alıştırma olarak ilgi Okuyucu için bırakın.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>6. adım: Giriş Şehir, giriş sayfası ve imza için bir arabirim eklemek için CreateUserWizard denetimini özelleştirme

`SELECT` Tarafından kullanılan sorgu `Guestbook.aspx` sayfasında kullanan bir `INNER JOIN` ilgili kayıtları arasında birleştirmek için `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tabloları. Hiçbir kayıt sahip bir kullanıcı `UserProfiles` Konuk hale açıklama, yorum olmaz görüntülenmesi ListView içinde olduğundan `INNER JOIN` yalnızca döndürür `GuestbookComments` eşleşen kayıtları olduğunda kayıtları `UserProfiles` ve `aspnet_Users`. Ve bir kullanıcı bir kayda sahip olmayan, adım 3'te gördüğümüz gibi `UserProfiles` aynen görüntüleyemez veya kendi ayarları düzenleyin `AdditionalUserInfo.aspx` sayfası.

Needless çok deyin nedeniyle bizim tasarım kararları önemlidir üyelik sistemi her kullanıcı hesabında bir eşleştirme olduğunu kaydetmek `UserProfiles` tablo. Ne istiyoruz eklenecek karşılık gelen bir kayıt içindir. `UserProfiles` yeni bir üyelik kullanıcı hesabı ile CreateUserWizard oluşturulduğu zaman.

' Da anlatıldığı gibi [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-vb.md) CreateUserWizard denetim yeni bir üyelik kullanıcı hesabı oluşturulduktan sonra öğretici başlatır, [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Biz bu olay için bir olay işleyicisi oluşturun, USERID için yeni oluşturulan kullanıcı alır ve bir kayıtta Ekle `UserProfiles` tablosu için varsayılan değerlerle `HomeTown`, `HomepageUrl`, ve `Signature` sütun. Daha, bu değerleri ek metin kutuları içerecek şekilde CreateUserWizard denetimin arabirimi özelleştirerek kullanıcıdan mümkündür.

Önce yeni bir satır ekleme bakalım `UserProfiles` tablosundaki `CreatedUser` varsayılan değerlerle olay işleyicisi. Yeni kullanıcının ev Şehir, giriş sayfası ve imza toplamak için ek form alanları içerecek şekilde CreateUserWizard denetimin kullanıcı arabirimini özelleştirmek nasıl göreceksiniz.

### <a name="adding-a-default-row-touserprofiles"></a>Varsayılan satır ekleme`UserProfiles`

İçinde [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-vb.md) CreateUserWizard denetime eklediğimiz öğretici `CreatingUserAccounts.aspx` sayfasındaki `Membership` klasör. Denetim CreateUserWizard olması için bir kayıt eklemek `UserProfiles` tablo kullanıcı hesabı oluşturma sırasında şu CreateUserWizard denetimin işlevselliği güncelleştirmeniz gerekir. Bu değişiklikleri yapmak yerine `CreatingUserAccounts.aspx` sayfasında, bunun yerine yeni bir CreateUserWizard denetim ekleyelim `EnhancedCreateUserWizard.aspx` sayfasında ve var. Bu öğretici için değişiklikleri yapın.

Açık `EnhancedCreateUserWizard.aspx` sayfasında Visual Studio'da ve CreateUserWizard denetimini sayfaya araç çubuğuna sürükleyin. CreateUserWizard denetimin ayarlamak `ID` özelliğine `NewUserWizard`. Biz anlatıldığı gibi [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-vb.md) Öğreticisi, CreateUserWizard'ın varsayılan kullanıcı arabirimi ziyaretçi için gerekli bilgileri ister. Bu bilgiler sağlanan sonra denetimi dahili olarak yeni bir kullanıcı hesabı üyelik Framework'te tüm bize tek satırlık bir kod yazmak zorunda oluşturur.

CreateUserWizard denetim olayların sayısı, iş akışı sırasında başlatır. Bir ziyaretçi istek bilgileri sağlayan ve formu gönderdikten sonra CreateUserWizard denetim başlangıçta ateşlenir kendi [ `CreatingUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Oluşturma işlemi sırasında bir sorun varsa [ `CreateUserError` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) tetiklenir; ancak, kullanıcı başarıyla oluşturulduysa, sonra [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) tetiklenir. İçinde [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-vb.md) öğreticisi için bir olay işleyicisi oluşturduğumuz `CreatingUser` sağlanan kullanıcı tüm başında veya sonunda boşluk ve içermiyordu emin olmak için olay Kullanıcı adı parola herhangi bir yere görülmedi.

Bir satır eklemek için `UserProfiles` tablo için bir olay işleyicisi oluşturmak ihtiyacımız yeni oluşturulan kullanıcı için `CreatedUser` olay. Zamana göre `CreatedUser` olay harekete, kullanıcı hesabı hesabın UserID değerini almak için bize etkinleştirme üyelik framework oluşturuldu.

İçin bir olay işleyicisi oluşturun `NewUserWizard`'s `CreatedUser` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Yeni eklenen kullanıcı hesabının kullanıcı kimliği alarak Yukarıdaki kod için önemlidir. Bu kullanarak gerçekleştirilir `Membership.GetUser(username)` bir belirli kullanıcı ve sonra kullanma hakkında bilgi döndürmek için yöntemin `ProviderUserKey` kendi UserID almak için özellik. CreateUserWizard denetim kullanıcı tarafından girilen kullanıcı adının aracılığıyla kullanılabilen kendi [ `UserName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Ardından, bağlantı dizesinin alındığı `Web.config` ve `INSERT` deyimi belirtildi. Gerekli ADO.NET nesneleri örneği ve komutu. Kodu atar bir [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) için örnek `@HomeTown`, `@HomepageUrl`, ve `@Signature` veritabanı ekleme etkisi parametreleri `NULL` değerleri `HomeTown`, `HomepageUrl`, ve `Signature` alanları.

Ziyaret `EnhancedCreateUserWizard.aspx` sayfasında bir tarayıcıdan ve yeni bir kullanıcı hesabı oluşturun. Bunu yaptıktan sonra Visual Studio'ya geri dönün ve içeriğini inceleyin `aspnet_Users` ve `UserProfiles` tabloları (Şekil 12'de yaptığımız gibi). Yeni kullanıcı hesabında görmelisiniz `aspnet_Users` ve karşılık gelen `UserProfiles` satır (ile `NULL` değerleri `HomeTown`, `HomepageUrl`, ve `Signature`).


[![Yeni kullanıcı hesabı ve UserProfiles kaydı eklenmiştir](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Şekil 20**: A yeni kullanıcı hesabı ve `UserProfiles` kayıt eklenmiştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image60.png))


Ziyaretçi kendi yeni hesap bilgileri sağladığı ve "Kullanıcı oluştur" düğmesine tıklandığında, kullanıcı hesabı oluşturulur ve bir satır eklenir sonra `UserProfiles` tablo. CreateUserWizard ardından görüntüler kendi `CompleteWizardStep`, bir başarı iletisi ve devam et düğmesi görüntülenir. Devam Et düğmesine tıklayarak bir geri gönderme neden olur, ancak hiçbir işlem yapılmadı, kullanıcı bırakarak takılmış `EnhancedCreateUserWizard.aspx` sayfası.

Devam Et düğmesine CreateUserWizard denetimin tıklatıldığında için kullanıcıya gönderilecek bir URL belirttiğimiz [ `ContinueDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Ayarlama `ContinueDestinationPageUrl` özelliğine "~ / Membership/AdditionalUserInfo.aspx". Bu yeni bir kullanıcıya sürer `AdditionalUserInfo.aspx`, burada görüntüleyebilir ve ayarlarını güncelleştirin.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Yeni kullanıcının giriş Şehir, giriş sayfası ve imza sor CreateUserWizard'ın arabirimine özelleştirme

CreateUserWizard denetimin varsayılan arabirim adı, parola ve e-posta gibi yalnızca çekirdek kullanıcı hesabı bilgilerini nerede toplanan basit hesap oluşturma senaryoları için yeterli olur. Ancak biz hesabını oluştururken kendi giriş Şehir, giriş sayfası ve imza girmesini ziyaretçi sor isterseniz? Kaydolma sırasında ek bilgi toplamak için CreateUserWizard denetimin arabirimini özelleştirmek mümkündür ve bu bilgileri, kullanılabilir `CreatedUser` temel veritabanına ek kayıtları eklemek için olay işleyicisi.

Sıralı bir dizi tanımlamak bir sayfa Geliştirici sağlayan bir denetimdir ASP.NET sihirbaz denetimi CreateUserWizard denetim genişletir `WizardSteps`. Sihirbaz Denetimi Etkin adım işler ve bu adımları uygularken size taşımak ziyaretçi bir gezinti arabirimi sağlar. Sihirbaz denetimi, uzun bir görev birkaç kısa adımlara bölmek için idealdir. Sihirbaz denetimi hakkında daha fazla bilgi için bkz: [ASP.NET 2.0 Sihirbazı denetimi ile bir adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

CreateUserWizard denetimin varsayılan biçimlendirme iki tanımlar `WizardSteps`: `CreateUserWizardStep` ve `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

İlk `WizardStep`, `CreateUserWizardStep`, kullanıcı adı, parola, e-posta vb. için ister arabirimi oluşturur. Ziyaretçi bu bilgileri sağlayan ve "Oluştur" kullanıcı sonra aynen gösterilen `CompleteWizardStep`, başarı iletisi ve Devam düğmesini gösterir.

Ek form alanları içerecek şekilde CreateUserWizard denetimin arabirimini özelleştirmek için şu olabilir:

- <strong>Bir veya daha yeni Oluştur</strong><strong>`WizardStep`</strong><strong>ek kullanıcı arabirimi öğeleri içerecek şekilde s</strong>. Yeni bir eklemek için `WizardStep` CreateUserWizard için tıklatın "Ekle/Kaldır `WizardStep` s" Akıllı başlatmak için etiketini bağlantısından `WizardStep` Koleksiyonu Düzenleyicisi. Buradan eklemek, kaldırmak veya yeniden sıralamak ve sihirbazdaki adımları. Bu, Bu öğretici için kullanacağız yaklaşımdır.

- <strong>Dönüştürme</strong><strong>`CreateUserWizardStep`</strong><strong>bir düzenlenebilir içine</strong><strong>`WizardStep`</strong><strong>.</strong> Bu değiştirir `CreateUserWizardStep` eşdeğer bir ile `WizardStep` , biçimlendirme eşleşen bir kullanıcı arabirimi tanımlar `CreateUserWizardStep`' s. Dönüştürerek `CreateUserWizardStep` içine bir `WizardStep` biz denetimleri yeniden konumlandırmak veya ek kullanıcı arabirimi öğeleri için bu adımı ekleyin. Dönüştürülecek `CreateUserWizardStep` veya `CompleteWizardStep` bir düzenlenebilir içine `WizardStep`, "kullanıcı Özelleştir oluşturma adım"'yi tıklatın veya "Tamamlandı adımında özelleştirme" denetimin akıllı etiketten bağlantı.

- **Yukarıdaki iki seçeneklerin bir bileşimi kullanın.**

Göz önünde bulundurmanız gereken önemli bir unsur olan "Kullanıcı oluştur" düğmesine içinden tıklandığında CreateUserWizard denetimi, kullanıcı hesabı oluşturma işlemi yürütür kendi `CreateUserWizardStep`. Varsa ek önemli değildir `WizardStep` sonra s `CreateUserWizardStep` veya değil.

Özel bir eklerken `WizardStep` ek kullanıcı girişi, özel toplama için CreateUserWizard denetimine `WizardStep` önce veya sonra yerleştirilebilir `CreateUserWizardStep`. Önce geliyorsa `CreateUserWizardStep` sonra ek kullanıcı girişini toplanan özel `WizardStep` için kullanılabilir `CreatedUser` olay işleyicisi. Ancak, varsa özel `WizardStep` sonra gelir `CreateUserWizardStep` sonra zamana göre özel `WizardStep` görüntülenir yeni kullanıcı hesabının zaten oluşturulmuş ve `CreatedUser` olay zaten gönderildi.

Şekil 21 gösterir iş akışı, eklenen `WizardStep` önündeki `CreateUserWizardStep`. Ek kullanıcı bilgilerini zamanına göre toplanan beri `CreatedUser` olay, tüm yapmak için sahip olduğumuz ateşlenir güncelleştirme `CreatedUser` bu girişleri almak ve bunlar için kullanmak için olay işleyicisini `INSERT` deyiminin parametre değerlerini (yerine `DBNull.Value`).


[![CreateUserWizard ek WizardStep CreateUserWizardStep'e önündeki zaman iş akışı](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Şekil 21**: CreateUserWizard iş akışı, bir ek `WizardStep` Precedes `CreateUserWizardStep` ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image63.png))


Varsa özel `WizardStep` yerleştirilir *sonra* `CreateUserWizardStep`, kullanıcının kendi giriş Şehir, giriş sayfası veya imza girmek için bir fırsat dolmadığı önce oluşturma kullanıcı hesabı işlemi ancak oluşur. Böyle bir durumda, bu ek bilgiler kullanıcı hesabı oluşturulduktan sonra Şekil 22 gösterildiği gibi veritabanına eklenmesi gerekir.


[![CreateUserWizard ek WizardStep sonra CreateUserWizardStep'e geldiğinde iş akışı](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Şekil 22**: CreateUserWizard iş akışı, bir ek `WizardStep` gelen sonra `CreateUserWizardStep` ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image66.png))


Şekil 22'de gösterilen iş akışı bir kayıtta eklemek için bekleyeceği `UserProfiles` kadar 2. adım tamamlandıktan sonra tablo. Ziyaretçi 1. adımdan sonra her tarayıcı kapatırsa, ancak bir durumda olduğu bir kullanıcı hesabı oluşturuldu, ancak hiçbir kayıt eklendi eriştik `UserProfiles`. Bir geçici bir çözüm değildir içeren bir kayıt için `NULL` veya varsayılan değerleri eklenen `UserProfiles` içinde `CreatedUser` (1. adımdan sonra ateşlenir) olay işleyici ve ardından güncelleştirme 2. adım tamamlandıktan sonra bu kaydedin. Bu sağlar bir `UserProfiles` kaydı kullanıcı kayıt işlemi yarıda aracılığıyla çıkar olsa bile kullanıcı hesabı için eklenir.

Bu öğretici için yeni bir oluşturalım `WizardStep` sonra oluşan `CreateUserWizardStep` önce `CompleteWizardStep`. Şimdi WizardStep yerleştirin ve yapılandırılmış ilk alın ve ardından koda bakacağız.

CreateUserWizard denetimin akıllı etiketten seçin "Ekle/Kaldır `WizardStep` s", hangi getirir `WizardStep` koleksiyon Düzenleyicisi iletişim. Yeni bir ekleme `WizardStep`, ayar kendi `ID` için `UserSettings`, kendi `Title` "Ayarlarınızı" için ve kendi `StepType` için `Step`. Ardından yerleştirin sonra gelir `CreateUserWizardStep` ("Kaydol yeni hesabınız için") ve önce `CompleteWizardStep` ("tam"), Şekil 23'te gösterildiği gibi.


[![Yeni bir WizardStep CreateUserWizard denetimine ekleme](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Şekil 23**: bir yeni Ekle `WizardStep` CreateUserWizard denetlemek için ([tam boyutlu görüntüyü görüntülemek için tıklatın](storing-additional-user-information-vb/_static/image69.png))


Kapatmak için Tamam'ı `WizardStep` koleksiyon Düzenleyicisi iletişim. Yeni `WizardStep` CreateUserWizard denetimin güncelleştirilmiş bildirim temelli biçimlendirme tarafından yi:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Yeni Not `<asp:WizardStep>` öğesi. Yeni kullanıcının ev Şehir, giriş sayfası ve burada İmza toplamak için kullanıcı arabirimi eklemeniz gerekir. Bildirim temelli söz dizimi veya Tasarımcısı aracılığıyla, bu içerik girebilirsiniz. Tasarımcısını kullanmak için "Ayarlarınızı" adım adım Tasarımcısı'nda görmek için akıllı etiket aşağı açılan listeden seçin.

> [!NOTE]
> Akıllı etiketin aşağı açılan listesi kullanılarak bir adım seçerek güncelleştirmeleri CreateUserWizard denetimin [ `ActiveStepIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), başlangıç adım dizinini belirtir. Tasarımcısı'nda "Ayarlarınızı" adımı düzenlemek için bu açılan listeyi kullanın, bu nedenle, böylece kullanıcılar ilk kez ziyaret ettiğinizde bu adımı gösterilen geri "oturum yukarı için yeni hesabınız için" ayarladığınızdan emin olun `EnhancedCreateUserWizard.aspx` sayfası.


Adlı üç TextBox denetimleri içeren "Ayarlarınızı" adım içinde bir kullanıcı arabirimi oluşturma `HomeTown`, `HomepageUrl`, ve `Signature`. Bu arabirim oluşturduktan sonra CreateUserWizard'ın bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Şimdi bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve ev Şehir, giriş sayfası ve imza için değerler belirten yeni bir kullanıcı hesabı oluşturun. Tamamladıktan sonra `CreateUserWizardStep` üyelik Framework'te kullanıcı hesabı oluşturulur ve `CreatedUser` için yeni bir satır ekler olay işleyicisi çalışır `UserProfiles`, ancak bir veritabanı ile `NULL` değerini `HomeTown`, `HomepageUrl`, ve `Signature`. Ev Şehir, giriş sayfası ve imza için girilen değerler hiçbir zaman kullanılır. Yeni bir kullanıcı hesabıyla net sonucu olan bir `UserProfiles` özelliği kayıt `HomeTown`, `HomepageUrl`, ve `Signature` alanınız Henüz belirtilmelidir.

Kullanıcı tarafından girilen ev Şehir, honepage ve imza değerlerini alır ve uygun güncelleştiren "Ayarlarınızı" adımından sonra kod yürütmek için ihtiyacımız `UserProfiles` kaydı. Kullanıcı hareket bir Sihirbazı'ndaki adımları arasında her zaman kontrol, sihirbazın [ `ActiveStepChanged` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) etkinleşir. Bu olay ve güncelleştirme için bir olay işleyicisi oluşturabiliriz `UserProfiles` "Ayarlarınızı" adım tamamlandığında tablo.

Olay işleyicisi CreateUserWizard için 's eklemek `ActiveStepChanged` olay ve aşağıdaki kodu ekleyin:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Yukarıdaki kod, biz yalnızca "Tamamlandı" adım ulaştıysanız belirleyerek başlatır. "Tam" adımından hemen "Ayarlarınızı" adımından sonra gerçekleştiğinde olduğundan, ziyaretçi ulaştığında sonra "Tamamlandı" aynen yalnızca "Ayarlarınızı" adım tamamlandı anlamına gelen adım.

TextBox denetimleri içinde programlı olarak başvurmak ihtiyacımız böyle bir durumda `UserSettings WizardStep`. Bu ilk kullanarak gerçekleştirilir `FindControl` programlı olarak başvuran yöntemi `UserSettings WizardStep`, ardından yeniden kutularındaki içinden başvurmak için `WizardStep`. Metin kutuları başvurulan sonra yürütülmeye hazır ki `UPDATE` deyimi. `UPDATE` Deyimi sahip aynı sayıda parametre olarak `INSERT` deyiminde `CreatedUser` olay işleyicisi, ancak kullanıcı tarafından sağlanan ev Şehir, giriş sayfası ve imza değerleri burada kullanırız.

Yerinde bu olay işleyicisi ile ziyaret `EnhancedCreateUserWizard.aspx` sayfasında bir tarayıcıdan ve ev Şehir, giriş sayfası ve imza için değerler belirten yeni bir kullanıcı hesabı oluşturun. Yeni hesabı oluşturduktan sonra için yeniden yönlendirilmesi gereken `AdditionalUserInfo.aspx` sayfası, burada yalnızca girilen ev Şehir, giriş sayfası ve imza bilgi görüntülenir.

> [!NOTE]
> Bizim Web sitesinde şu anda içinden bir ziyaretçi yeni bir hesap oluşturabilirsiniz iki sayfa vardır: `CreatingUserAccounts.aspx` ve `EnhancedCreateUserWizard.aspx`. Web sitesinin site haritası ve oturum açma sayfasına işaret `CreatingUserAccounts.aspx` sayfasında, ancak `CreatingUserAccounts.aspx` sayfası kullanıcı için giriş Şehir, giriş sayfası ve imza bilgilerini istenmez ve karşılık gelen bir satır eklemez `UserProfiles`. Bu nedenle, ya da güncelleştirme `CreatingUserAccounts.aspx` bu işlevselliği sunar, böylece sayfa veya başvurmak için site haritası ve oturum açma sayfası güncelleştirme `EnhancedCreateUserWizard.aspx` yerine `CreatingUserAccounts.aspx`. İkinci seçeneği seçerseniz, güncelleştirdiğinizden emin olun `Membership` klasörün `Web.config` anonim kullanıcıların erişmesine izin vermek amacıyla dosya `EnhancedCreateUserWizard.aspx` sayfası.


## <a name="summary"></a>Özet

Bu öğreticide üyelik framework içinde kullanıcı hesaplarına ilgili veri modelleme teknikleri inceledik. Özellikle, bir-çok ilişkisi bire bir ilişki paylaşan veri yanı sıra kullanıcı hesapları ile paylaşmak varlıklar modelleme Aranan. Ayrıca, bu bilgi görüntülenen, eklenen ve SqlDataSource denetimi ve diğerleri kullanan bazı örnekler ile güncelleştirildiğinde ilişkisini gördüğümüz ADO.NET kodu kullanarak.

Bu öğretici, kullanıcı hesaplarını bizim bakma tamamlar. Sonraki öğretici ile başlayan uygulamamızla rollere açar. Sonraki biz rolleri framework görünür birkaç öğreticilere bakın yeni rolleri oluşturmak nasıl nasıl hangi rollerin belirlemek için bir kullanıcının ait olduğu nasıl ve ne rolleri, kullanıcılara atamak rol tabanlı yetkilendirme uygulamak için.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Verilere erişme ve verileri ASP.NET 2.0 güncelleştirme](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi ile bir adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Özel veri kaynağı denetim parametreleri oluşturma](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [CreateUserWizard denetimini özelleştirme](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [DetailsView denetimini QuickStarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [ListView denetimi ile verileri görüntüleme](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET 2.0 doğrulama denetimleri dissecting](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [INSERT düzenleme ve verileri silme](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [ASP.NET form doğrulama](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Özel kullanıcı kaydı bilgileri toplanıyor](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 profillerinde](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView denetimi](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Kullanıcı profillerini hızlı başlangıç](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](user-based-authorization-vb.md)
