---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Ek kullanıcı bilgileri (C#) depolama | Microsoft Docs
author: rick-anderson
description: Bu öğreticide çok ilkel Konuk uygulama oluşturarak bu soruya yanıt. Böylece, modeli için farklı seçenekleri atacağız...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b73a19758c420f09d178ebd395d5c89ad493505
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819834"
---
<a name="storing-additional-user-information-c"></a>Ek kullanıcı bilgileri depolama (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> Bu öğreticide çok ilkel Konuk uygulama oluşturarak bu soruya yanıt. Bunun yapılması, iptal eder kullanıcı bilgilerini bir veritabanında modelleme için farklı seçenekleri bakmak ve ardından üyelik framework tarafından oluşturulan kullanıcı hesapları ile bu verileri ilişkilendirmek bkz.


## <a name="introduction"></a>Giriş

ASP. NET üyelik framework kullanıcıları yönetmek için esnek bir arabirim sunar. Üyelik API'si kimlik bilgilerini doğrulama, o anda oturum açmış kullanıcı hakkında bilgi almak, yeni bir kullanıcı hesabı oluşturma ve diğerlerinin yanı sıra bir kullanıcı hesabının silinmesi için yöntemler içerir. Her kullanıcı hesabı üyelik Framework yalnızca kimlik doğrulama ve temel kullanıcı hesabıyla ilgili görevleri gerçekleştirmek için gereken özellikleri içerir. Bu özellikleri ve yöntemleri yi [ `MembershipUser` sınıfı](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), bir kullanıcı hesabı üyelik Framework modeller. Bu sınıf gibi özelliklere sahiptir [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), ve [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), ve yöntemler gibi [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) ve [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Önerilmesine, uygulamalar üyelik framework bulunmayan ek kullanıcı bilgileri depolamak gerekir. Örneğin, bir çevrimiçi satış şirketi, her kullanıcının kendi dağıtımı ve fatura adresleri, ödeme bilgilerini, teslim tercihlerini depolamak ve telefon numarası izin gerekebilir. Ayrıca, sistem her sırada belirli bir kullanıcı hesabıyla ilişkilidir.

`MembershipUser` Sınıfı gibi özellikleri içermez `PhoneNumber` veya `DeliveryPreferences` veya `PastOrders`. Bu nedenle nasıl biz uygulama için gerekli kullanıcı bilgileri izlemek ve bu üyelik framework ile tümleştirin? Bu öğreticide çok ilkel Konuk uygulama oluşturarak bu soruya yanıt. Bunun yapılması, iptal eder kullanıcı bilgilerini bir veritabanında modelleme için farklı seçenekleri bakmak ve ardından üyelik framework tarafından oluşturulan kullanıcı hesapları ile bu verileri ilişkilendirmek bkz. Haydi başlayalım!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>1. adım: Konuk uygulamanın veri modeli oluşturma

Bir veritabanında kullanıcı bilgilerini yakalama ve üyelik framework tarafından oluşturulan kullanıcı hesapları ile ilişkilendirmek için kullanılan teknikleri çeşitli vardır. Bu teknikler göstermek için biz bir çeşit kullanıcı ile ilgili verileri yakalar, böylece Eğitmen web uygulamasını genişletmek gerekir. (Uygulama hizmetleri için gerekli tabloları yalnızca şu anda, uygulamanın veri modelini içeriyor `SqlMembershipProvider`.)

Kimliği doğrulanmış bir kullanıcı bir yorum yere bırakabilirsiniz bir çok basit Konuk uygulaması oluşturalım. Konuk yorumların depolanması yanı sıra, şimdi her bir kullanıcı kendi giriş Şehir, giriş sayfası ve imza depolamak izin verin. Sağlanmışsa, kullanıcının giriş Şehir, giriş sayfası ve imza kendisinin defterinizde ayrıldı her bir ileti görüntülenir.

### <a name="adding-theguestbookcommentstable"></a>Ekleme`GuestbookComments`tablo

Konuk yorumları yakalamak için adlı bir veritabanı tablosu oluşturmak gerekiyor `GuestbookComments` gibi sütunları olan `CommentId`, `Subject`, `Body`, ve `CommentDate`. Ayrıca her bir kaydın zorunda ihtiyacımız `GuestbookComments` tablo başvurusu yorumu yapan kullanıcı.

Bu tablo eklemek için Visual Studio'da veritabanı Gezgini gidin ve detayına `SecurityTutorials` veritabanı. Tabloları klasörü sağ tıklatın ve Yeni Tablo Ekle öğesini seçin. Bu yeni tablo sütunlarını tanımlamak olanak sağlayan bir arabirim getirir.


[![SecurityTutorials veritabanına yeni bir tablo ekleme](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Şekil 1**: yeni bir tabloya ekleyin `SecurityTutorials` veritabanı ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image3.png))


Ardından, tanımlama `GuestbookComments`'s sütunları. Başlangıç adlı bir sütunu ekleyerek `CommentId` türü `uniqueidentifier`. Bu sütun her Konuk açıklamada benzersiz şekilde tanımlamak, bu nedenle izin vermeyin `NULL` s ve tablonun birincil anahtarı olarak işaretleyin. İçin bir değer sağlanması yerine `CommentId` her alan `INSERT`, size göstermek yeni bir `uniqueidentifier` değeri otomatik olarak oluşturulacak Bu alan için şirket `INSERT` sütunun varsayılan değer ayarlayarak `NEWID()`. Varsayılan değeri, birincil anahtarı ve ayarları işaretlemek, bu ilk alan eklendikten sonra ekranınızın Şekil 2'de gösterilen ekran şuna benzemelidir.


[![CommentId adlı birincil sütun ekleme](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Şekil 2**: birincil adlı sütun ekleme `CommentId` ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image6.png))


Ardından, adlı bir sütun ekleyin `Subject` türü `nvarchar(50)` adlı bir sütun `Body` türü `nvarchar(MAX)`, engelleyerek `NULL` hem sütunlardaki s. Adlı bir sütun ekleyin, `CommentDate` türü `datetime`. İzin verme `NULL` s ve kümesi `CommentDate` sütunun varsayılan değere `getdate()`.

Kalan tek şey, bir kullanıcı hesabı ile konuk açıklamaları ilişkilendiren bir sütun eklemek için. Adlı bir sütun eklemek için bir seçenek olacaktır `UserName` türü `nvarchar(256)`. Bu uygun bir üyelik sağlayıcısı dışında kullanıldığında `SqlMembershipProvider`. Ancak kullanırken `SqlMembershipProvider`, Bu öğretici serisinde, bizim yaptığımız gibi `UserName` sütununda `aspnet_Users` tablo benzersiz olması garanti edilmez. `aspnet_Users` Tablonun birincil anahtarı `UserId` ve tür `uniqueidentifier`. Bu nedenle, `GuestbookComments` adlı bir sütun tablonun `UserId` türü `uniqueidentifier` (engelleyerek `NULL` değerler). Devam edin ve bu sütun ekleyin.

> [!NOTE]
> Açıkladığımız gibi [ *SQL Server'da üyelik şeması oluşturma* ](creating-the-membership-schema-in-sql-server-cs.md) Öğreticisi, üyelik framework aynı paylaşmak birden çok web uygulaması farklı kullanıcı hesapları ile etkinleştirmek için tasarlanmıştır Kullanıcı deposu. Bunu farklı uygulamalara kullanıcı hesapları olarak bölümleyerek yapar. Ve her kullanıcı bir uygulama içinde benzersiz olması garanti edilir olsa da aynı kullanıcı adı kullanarak aynı kullanıcı deposu farklı uygulamalarda kullanılabilir. Bir bileşik yoktur `UNIQUE` kısıtlamasında `aspnet_Users` üzerinde tablo `UserName` ve `ApplicationId` alanlar, ancak biri değil yalnızca `UserName` alan. Sonuç olarak, ASP.NET için olası\_kullanıcıların iki (veya daha fazla) kayıtları aynı tabloya `UserName` değeri. Yoktur, ancak bir `UNIQUE` kısıtlaması `aspnet_Users` tablonun `UserId` (birincil anahtar olduğundan) alan. A `UNIQUE` kısıtlaması, biz arasında bir yabancı anahtar kısıtlaması olmadan oluşturamıyor çünkü önemlidir `GuestbookComments` ve `aspnet_Users` tablolar.


Ekledikten sonra `UserId` sütun, araç Kaydet simgesine tıklayarak tabloyu kaydedin. Yeni tablo adı `GuestbookComments`.

İle katılmak için bir son çıkış sahibiz `GuestbookComments` tablosu: oluşturmamız gerekir bir [yabancı anahtar kısıtlaması](https://msdn.microsoft.com/library/ms175464.aspx) arasında `GuestbookComments.UserId` sütun ve `aspnet_Users.UserId` sütun. Bunu başarmak için yabancı anahtar ilişkileri iletişim kutusunu başlatmak için araç çubuğunda ilişki simgesine tıklayın. (Alternatif olarak, bu iletişim kutusu Tablo Tasarımcısı menüsüne giderek ve ilişkileri seçmek başlatabilirsiniz.)

Yabancı anahtar ilişkileri iletişim kutusunun sol alt köşedeki Ekle düğmesine tıklayın. Biz yine de bir ilişkide yer alan tabloları tanımlama gerekir ancak bu bir yeni yabancı anahtar kısıtlamasını ekler.


[![Bir tablonun yabancı anahtar kısıtlamalarını yönetmek için yabancı anahtar ilişkileri iletişim kutusunu kullanın.](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Şekil 3**: bir tablonun yabancı anahtar kısıtlamalarını yönetmek için yabancı anahtar ilişkileri iletişim kutusunu kullanın ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image9.png))


Ardından, sağdaki "Tablo ve sütun belirtimlerini" satırdaki üç nokta simgesine tıklayın. Bu, biz belirtebilirsiniz birincil anahtar tablo ve sütun ve yabancı anahtar sütunu tablolar ve sütunlar iletişim kutusu başlatacak `GuestbookComments` tablo. Özellikle, seçin `aspnet_Users` ve `UserId` birincil anahtar tablo ve sütun ve `UserId` gelen `GuestbookComments` tablosu yabancı anahtar sütunu olarak (bkz: Şekil 4). Birincil ve yabancı anahtar tabloları ve sütunları tanımladıktan sonra yabancı anahtar ilişkileri iletişim kutusuna dönmek için Tamam'ı tıklatın.


[![Bir yabancı anahtar kısıtlaması arasında aspnet_Users ve GuesbookComments tablolar oluşturun](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Şekil 4**: bir yabancı anahtar kısıtlaması arasında kurmak `aspnet_Users` ve `GuesbookComments` tablolar ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image12.png))


Bu noktada, yabancı anahtar kısıtlaması kuruldu. Bu kısıtlama varlığını sağlar [ilişkisel bütünlüğü](http://en.wikipedia.org/wiki/Referential_integrity) hiçbir zaman olacağına dair bir mevcut olmayan kullanıcı hesabına başvuran bir konuk girişi güvence altına almak tarafından iki tablo arasında. Varsayılan olarak, bir yabancı anahtar kısıtlaması karşılık gelen alt kayıtları silinmesi için üst kaydına izin vermez. Kendi konuk açıklamaları ilk silinene kadar diğer bir deyişle, bir kullanıcı bir veya daha fazla Konuk yorumlar yapar ve ardından bu kullanıcı hesabını silmek denediğimiz silme başarısız olur.

Yabancı anahtar kısıtlamaları, bir üst kaydı silindiğinde otomatik olarak ilgili alt kayıtları silmek için yapılandırılabilir. Diğer bir deyişle, her kullanıcı hesabı silindiğinde kullanıcının Konuk girişleri otomatik olarak silinir, böylece Biz bu yabancı anahtar kısıtlaması ayarlayabilirsiniz. Bunu gerçekleştirmek için "INSERT ve UPDATE tarifi" bölümü genişletin ve Cascade için "Kural silme" özelliğini ayarlayın.


[![Art arda silme için yabancı anahtar kısıtlamasını yapılandırma](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Şekil 5**: yabancı anahtar kısıtlaması Cascade siler için yapılandırma ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image15.png))


Yabancı anahtar kısıtlaması kaydetmek için yabancı anahtar ilişkilerini dışında çıkmak için Kapat düğmesine tıklayın. Ardından tabloda ve bu kaydetmek için araç çubuğundaki Kaydet simgesine tıklayın ilişki.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Kullanıcının giriş Şehir, giriş sayfası ve imza depolama

`GuestbookComments` Tablo kullanıcı hesapları ile bire çok ilişkisi paylaşan bilgileri depolamak nasıl gösterir. Her kullanıcı hesabı yorumlara rastgele bir sayıdan olabileceği bağlantıları açıklamaları belirli bir kullanıcıya geri bir sütun içeren açıklamaları kümesini tutmak üzere bir tablo oluşturarak bu ilişkiyi modellenir. Kullanırken `SqlMembershipProvider`, bu bağlantı, en iyi adlı bir sütun oluşturarak kurulduktan `UserId` türü `uniqueidentifier` ve bu sütunu arasında bir yabancı anahtar kısıtlaması ve `aspnet_Users.UserId`.

Artık üç sütun, kullanıcının giriş Şehir, giriş sayfası ve kendi konuk açıklamaları görünür imzası, depolamak için her kullanıcı hesabıyla ilişkilendirmek ihtiyacımız var. Vardır bunu gerçekleştirmek için farklı yollar sayısı:

- <strong>Yeni sütun eklemek</strong><strong>`aspnet_Users`</strong><strong>veya</strong><strong>`aspnet_Membership`</strong><strong>tablolar.</strong> Tarafından kullanılan şema değiştireceği ben bu yaklaşım önerilir değil `SqlMembershipProvider`. Bu karar, yol haunt için geri dönen. Örneğin, ASP.NET gelecek bir sürümünde farklı bir kullanır ne `SqlMembershipProvider` şema. ASP.NET 2.0 geçirmek için bir aracı Microsoft içerebilir `SqlMembershipProvider` ASP.NET 2.0 değiştirdiniz, ancak yeni şemaya veri `SqlMembershipProvider` şema, böyle bir dönüştürme olabilir mümkün.

- **ASP kullanın. NET profili framework, giriş Şehir, giriş sayfası ve imza için bir profil özelliği tanımlama.** ASP.NET ek kullanıcıya özgü verileri depolamak için tasarlanmış bir profili çerçevesi içerir. Üyelik framework gibi profil framework sağlayıcı modeli yerleşik olarak bulunur. .NET Framework ile birlikte gelen bir `SqlProfileProvider` sthat profil verileri bir SQL Server veritabanında depolar. Aslında, veritabanımızdaki tarafından kullanılan tablo zaten var. `SqlProfileProvider` (`aspnet_Profile`), uygulama hizmetlerini yeniden eklenen eklendiği gibi <a id="_msoanchor_2"> </a> [ *SQL üyelik şeması oluşturma Sunucu* ](creating-the-membership-schema-in-sql-server-cs.md) öğretici.   
  Ana profili framework'ün profili özelliklerini tanımlamak, geliştiriciler için izin verdiğini avantajdır `Web.config` – hiçbir kod için ve temel alınan veri deposundan profil verilerini seri hale getirmek için yazılması gerekir. Kısacası, profili özellikler kümesini tanımlar ve kod içinde bunlarla çalışmak için son derece kolay. Ancak, bu sürümü için söz konusu olduğunda istenen için çok fazla profil sistemi bırakır bunu burada daha sonra veya var olanları kaldırılmış veya değiştirilmiş için eklenecek yeni kullanıcıya özgü özellikleri beklediğiniz sonra profili framework olmayabilir neden olan bir uygulamanız varsa  en iyi seçenek. Ayrıca, `SqlProfileProvider` profil özelliklerini, sonraki için (örneğin, kaç bir giriş Şehir, New York olmalıdır) doğrudan profil verileri karşı sorguları çalıştırmak imkansızdır yüksek oranda normalleştirilmişlikten çıkarılmış bir şekilde depolar.   
  Profil framework hakkında daha fazla bilgi için bu öğreticinin sonunda "Başka okumalar" bölümüne bakın.

- <strong>Bu üç sütun veritabanında yeni bir tablo ekleyin ve bu tablo arasında bire bir ilişki kurmak ve</strong><strong>`aspnet_Users`</strong><strong>.</strong> Bu yaklaşım, biraz daha fazla iş profili framework ile ilgilidir, ancak ek kullanıcı özelliklerini veritabanında nasıl modellenir maksimum esneklik sunar. Bu öğreticide kullanacağız seçenek budur.

Adlı yeni bir tablo oluşturacağız `UserProfiles` giriş Şehir, giriş sayfası ve her kullanıcı için imza kaydetmek için. Veritabanı Gezgini penceresinde tabloları klasörüne sağ tıklayın ve yeni bir tablo oluşturulacağını seçin. İlk sütun adı `UserId` ve kendi tür kümesine `uniqueidentifier`. İzin verme `NULL` değerleri ve sütun birincil anahtar olarak işaretleyin. Ardından, adlandırılmış sütunlar ekleyin: `HomeTown` türü `nvarchar(50)`; `HomepageUrl` türü `nvarchar(100)`; ve imza türü `nvarchar(500)`. Bu üç sütunların kabul edebilen bir `NULL` değeri.


[![UserProfiles tablosu oluşturma](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Şekil 6**: oluşturma `UserProfiles` tablo ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image18.png))


Tabloyu kaydettikten ve adlandırın `UserProfiles`. Son olarak, bir yabancı anahtar kısıtlaması arasında kurmak `UserProfiles` tablonun `UserId` alan ve `aspnet_Users.UserId` alan. Arasında yabancı anahtar kısıtlaması ile eşleşmediğinden `GuestbookComments` ve `aspnet_Users` tablolar siler basamaklı bu kısıtlamayı sahip. Bu yana `UserId` alanındaki `UserProfiles` birincil anahtar, bu birden fazla kayıt olacaktır sağlar `UserProfiles` her kullanıcı hesabı için tablo. Bu ilişki türünde, için bire bir olarak adlandırılır.

Oluşturulan veri modelini sahibiz, biz bunu kullanmak hazır olursunuz. Adım 2 ve 3 biz nasıl şu anda oturum açmış olan kullanıcının görüntüleyebilir ve giriş Şehir, giriş sayfası ve imza bilgilerini düzenleyerek görünecektir. Adım 4'te yeni konuk açıklamaları Gönder ve mevcut cihazlarınızı görüntülemek kimliği doğrulanmış kullanıcılar için arabirimi oluşturacağız.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>2. adım: kullanıcının giriş Şehir, giriş sayfası ve imza görüntüleme

Çeşitli şekillerde kendi giriş Şehir, giriş sayfası ve imza bilgilerini görüntülemek ve düzenlemek o anda oturum açmış kullanıcı izni vardır. TextBox ile kullanıcı arabirimini el ile oluşturabilir ve etiket denetimleri ya da biz DetailsView denetimi gibi Web denetimleri veri birini kullanabilirsiniz. Veritabanı gerçekleştirmek için `SELECT` ve `UPDATE` size ADO.NET yazabilirsiniz deyimleri kod sayfamızı ait arka plan kod sınıfında veya alternatif olarak, bildirim temelli bir yaklaşım SqlDataSource ile kullanın. İdeal olarak uygulamamız biz ya da programlı olarak sayfa arka plan kod sınıfı veya bildirimli olarak ObjectDataSource Denetimi aracılığıyla çağırabilirsiniz katmanlı bir mimari içerecektir.

Bu öğretici serisinin form kimlik doğrulaması, yetkilendirme, kullanıcı hesaplarını ve rolleri üzerinde odaklanır. bu yana olmayacaktır kapsamlı bir tartışma bu farklı veri erişim seçenekleri ve neden katmanlı bir mimari SQL deyimleri yürütmeyi üzerinden doğrudan tercih edilir ASP.NET sayfasından. Bir DetailsView ve SqlDataSource-hızlı ve en kolay seçenek – kullanarak yol şuraya atlıyorum ancak açıklanan kavramları kesinlikle alternatif Web denetimleri ve veri erişim mantığı için uygulanabilir. ASP.NET'te verileri ile çalışma hakkında daha fazla bilgi için başvurmak my *[ASP.NET 2.0 verilerle çalışmaya](../../data-access/index.md)* öğretici serisi.

Açık `AdditionalUserInfo.aspx` sayfasını `Membership` klasörü ve ayarı sayfasına bir DetailsView denetimi ekleyin, `ID` özelliğini `UserProfile` ve temizleme kendi `Width` ve `Height` özellikleri. DetailsView'ın akıllı etiket genişletin ve yeni bir veri kaynak denetimine bağlamak seçin. Bu veri kaynağı Yapılandırma Sihirbazı başlatılır (bkz. Şekil 7). İlk adım, veri kaynağı türü belirtmenizi ister. Doğrudan bağlanın kullanacağız beri `SecurityTutorials` veritabanı simgesini seçin, veritabanı belirtme `ID` olarak `UserProfileDataSource`.


[![UserProfileDataSource adlı yeni bir SqlDataSource denetimi ekleme](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Şekil 7**: yeni bir SqlDataSource denetimi adlı ekleme `UserProfileDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image21.png))


Sonraki ekranda veritabanını kullanacak şekilde ister. Bağlantı dizesinde tanımladığımız zaten `Web.config` için `SecurityTutorials` veritabanı. Bu bağlantı dizesi adı – `SecurityTutorialsConnectionString` – aşağı açılan listesinde olmalıdır. Bu seçeneği belirleyin ve İleri'ye tıklayın.


[![SecurityTutorialsConnectionString aşağı açılan listeden seçin.](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Şekil 8**: seçin `SecurityTutorialsConnectionString` aşağı açılan listeden ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image24.png))


Sonraki ekranda tablo ve sütunları sorgu belirtmek için bize ister. Seçin `UserProfiles` tablo aşağı açılan listeden ve tüm sütunları denetleyin.


[![Getir tüm sütunları UserProfiles tablosundan yedekleme](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Şekil 9**: getir geri tüm sütun `UserProfiles` tablo ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image27.png))


Şekil 9 döndürür geçerli sorgu *tüm* kayıtlarında `UserProfiles`, ancak yalnızca o anda oturum açmış kullanıcının kayıt ilgilenen duyuyoruz. Eklemek için bir `WHERE` yan tümcesi tıklayın `WHERE` Ekle getirmek için düğme `WHERE` yan tümce iletişim kutusu (bkz. Şekil 10). Burada filtre sütunu, işleci ve filtre parametresi kaynağını seçebilirsiniz. Seçin `UserId` sütun ve "İşleci olarak =".

Ne yazık ki şu anda oturum açmış kullanıcının döndürmek için yerleşik parametre kaynağı yok `UserId` değeri. Bu değer programlı olarak alıp gerekecektir. Bu nedenle, "None," Ekle parametre eklemek için düğmesini, Tamam'ı tıklatın ve kaynak açılan listeye ayarlayın.


[![Bir filtre parametresi kullanıcı kimliği sütunu Ekle](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Şekil 10**: filtre parametresi eklemek `UserId` sütun ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image30.png))


Tamam'a tıkladıktan sonra Şekil 9'da gösterilen ekrana döndürülür. Bu kez, Bununla birlikte, ekranın alt kısmındaki SQL sorgusu içermelidir bir `WHERE` yan tümcesi. "Test sorgusu" ekrana geçmek için İleri'ye tıklayın. Sorguyu buraya çalıştırın ve sonuçlarını görebilirsiniz. Sihirbazı tamamlamak için Son'u tıklatın.

Visual Studio, veri kaynağı Yapılandırma Sihirbazı'nı tamamladıktan sonra sihirbazda belirttiğiniz ayarlara dayanan SqlDataSource denetimi oluşturur. Ayrıca, el ile BoundFields SqlDataSource tarafından 's döndürülen her sütun için DetailsView ekler `SelectCommand`. Gösterilecek gerek yoktur `UserId` kullanıcı, bu değer bilmeniz gerekmez. bu yana DetailsView alanındaki. DetailsView denetimin bildirim temelli biçimlendirmeden doğrudan bu alanı kaldırabilir veya akıllı etiketinde bağlantı "Alanları Düzenle" tıklayarak.

Bu noktada bildirim temelli işaretleme, sayfanın aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

SqlDataSource denetimin program üzerinden ayarlamak ihtiyacımız `UserId` parametre için şu anda oturum açmış kullanıcının `UserId` veri seçilmeden önce. Bu olay işleyicisi SqlDataSource için 's oluşturarak gerçekleştirilebilir `Selecting` olay ve aşağıdaki ekleme kodu vardır:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Yukarıdaki kod, çağırarak o anda oturum açmış kullanıcıya bir başvuru elde ederek başlatır `Membership` sınıfın `GetUser` yöntemi. Bu döndürür bir `MembershipUser` nesnesi `ProviderUserKey` özelliği içeren `UserId`. `UserId` Değeri SqlDataSource için 's atanır `@UserId` parametresi.

> [!NOTE]
> `Membership.GetUser()` Yöntemi o anda oturum açmış kullanıcı hakkındaki bilgileri döndürür. Anonim kullanıcı sayfasını ziyaret edin, değeri döndüreceği `null`. Böyle bir durumda, bu neden bir `NullReferenceException` okunmaya çalışırken kod aşağıdaki satırda `ProviderUserKey` özelliği. Elbette, biz hakkında endişelenmenize gerek yok `Membership.GetUser()` döndüren bir `null` değerini `AdditionalUserInfo.aspx` yalnızca kimliği doğrulanmış kullanıcılar, bu klasördeki ASP.NET kaynaklara erişebilir, böylece biz URL yetkilendirmesi önceki bir öğreticide yapılandırıldığı için sayfa. Şu anda oturum açmış olan kullanıcının anonim erişime izin verilir burada sayfasındaki bilgilerine erişmeniz gerekiyorsa, bu maddeyi emin olun olmayan bir`null MembershipUser` nesne döndürülür `GetUser()` özelliklerini başvuran önce yöntemi.


Ziyaret ederse `AdditionalUserInfo.aspx` sayfası bir tarayıcıdan herhangi bir satır eklemek henüz çünkü boş bir sayfa görürsünüz `UserProfiles` tablo. Adım 6'da otomatik olarak yeni bir satır eklemek için CreateUserWizard denetimi özelleştirmek nasıl atacağız `UserProfiles` yeni bir kullanıcı hesabı oluşturulduğunda tablo. Şimdilik, ancak biz bir kayıt tablodaki el ile oluşturmanız gerekir.

Visual Studio'da veritabanı Gezgini gidin ve tabloları klasörünü genişletin. Sağ `aspnet_Users` tablosu ve "Tablo verilerini tablodaki kayıtları gösterme" seçin; aynı şeyi yapmak `UserProfiles` tablo. Şekil 11, dikey olarak döşenmiş zaman şu sonuçları gösterir. My veritabanında şu anda işaretli olan `aspnet_Users` Bruce, Gamze ve Tito kaydeder, ancak hiç kayıt `UserProfiles` tablo.


[![Aspnet_Users içeriğini ve UserProfiles tabloları görüntülenir](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Şekil 11**: içeriğini `aspnet_Users` ve `UserProfiles` tabloları görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image33.png))


Yeni bir kayıt eklemek `UserProfiles` değerleri el ile yazarak tablo `HomeTown`, `HomepageUrl`, ve `Signature` alanları. Geçerli bir almak için en kolay yolu `UserId` yeni değer `UserProfiles` kaydıdır seçmek için `UserId` belirli bir kullanıcı hesabında alanını `aspnet_Users` tablo kopyalayın ve yapıştırın `UserId` alanındaki `UserProfiles`. Şekil 12 gösterir `UserProfiles` yeni bir kayıt için Bruce eklendikten sonra tablo.


[![Bir kayıt için UserProfiles Bruce için eklendi](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Şekil 12**: A kaydı eklendiği `UserProfiles` Bruce için ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image36.png))


Geri dönüp `AdditionalUserInfo.aspx` Bruce oturum açmış sayfası. Şekil 13 gösterildiği gibi Bruce'nın ayarları görüntülenir.


[![Gösterilen HIS ayarları şu anda ziyaret eden kullanıcının olduğu](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Şekil 13**: şu anda ziyaret kullanıcıdır gösterilen HIS ayarları ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> İleri ve el ile kayıt ekleyelim `UserProfiles` tablo her bir üyelik kullanıcısı için. Adım 6'da otomatik olarak yeni bir satır eklemek için CreateUserWizard denetimi özelleştirmek nasıl atacağız `UserProfiles` yeni bir kullanıcı hesabı oluşturulduğunda tablo.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>3. adım: Kullanıcının kendi giriş Şehir, giriş sayfası ve imza düzenlemesine izin verme

Bu noktada şu anda oturum açmış olan kullanıcının kendi giriş Şehir, giriş sayfası ve imza ayarı görüntüleyebilirsiniz, ancak bunları henüz değiştiremez. Böylece veri düzenlenebilir DetailsView denetiminde güncelleştirelim.

Yapmamız gereken ilk şey eklemektir bir `UpdateCommand` SqlDataSource için belirtme `UPDATE` yürütmek için deyimi ve karşılık gelen parametreleri. SqlDataSource seçip, Özellikler penceresinden komut ve parametre Düzenleyicisi iletişim kutusunu Getir veUpdateQuery özelliğin yanındaki üç noktaya tıklayın. Aşağıdakileri girin `UPDATE` textbox INTO deyimi:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Ardından, bir parametre SqlDataSource denetiminin oluşturur "Parametreleri Yenile" düğmesini tıklatın `UpdateParameters` her parametrelerinde toplamayı `UPDATE` deyimi. Tüm parametreler kümesi için kaynak yok olarak bırakın ve iletişim kutusunu doldurmak için Tamam düğmesine tıklayın.


[![SqlDataSource'nın UpdateCommand ve UpdateParameters belirtin](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Şekil 14**: SqlDataSource's belirtin `UpdateCommand` ve `UpdateParameters` ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image42.png))


Eklemeleri nedeniyle düzenleme denetimi artık destekleyebilir DetailsView SqlDataSource denetimi yaptık. DetailsView'ın akıllı etiketten "Düzenlemeyi etkinleştir" onay kutusunu işaretleyin. Bu denetimin bir CommandField ekler `Fields` koleksiyonuyla kendi `ShowEditButton` özelliği True olarak ayarlayın. DetailsView salt okunur modda ve güncelleştirme görüntülenir ve görüntülenen zaman iptal düğmeleri düzenleme modu, bir düzenleme düğmesi oluşturur. Düzenle'ye tıklayın kullanıcının gerek kalmadan, ancak biz DetailsView işleme "her zaman düzenlenebilir" bir durumda DetailsView denetimin ayarlayarak olabilir [ `DefaultMode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) için `Edit`.

Bu değişikliklerle DetailsView denetiminizin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

CommandField eklenmesini unutmayın ve `DefaultMode` özelliği.

Devam edin ve bu sayfada bir tarayıcı aracılığıyla test edin. Bir kullanıcıyla ilgili bir kayıt vardır ziyaret `UserProfiles`, kullanıcı ayarlarını düzenlenebilir bir arabirimde görüntülenir.


[![DetailsView düzenlenebilir bir arabirim oluşturur.](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Şekil 15**: DetailsView düzenlenebilir bir arabirim oluşturur ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image45.png))


Deneyin değiştirerek ve güncelleştir düğmesine tıklayarak. Hiçbir şey olmuyor gibi görünür. Bir geri gönderme yoktur ve değerleri veritabanına kaydedilir, ancak kaydetme oluştu hiçbir görsel geri bildirim yoktur.

Bu sorunu gidermek için Visual Studio'ya geri dönün ve DetailsView yukarıda etiket denetimi ekleyin. Ayarlama, `ID` için `SettingsUpdatedMessage`, kendi `Text` "ayarlarınızı güncellendi," özelliği ve kendi `Visible` ve `EnableViewState` özelliklerine `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Görüntülenecek ihtiyacımız `SettingsUpdatedMessage` DetailsView güncelleştirildiğinde etiketleyin. Bunu gerçekleştirmek için bir olay işleyicisi oluşturun DetailsView için 's `ItemUpdated` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Geri dönüp `AdditionalUserInfo.aspx` sayfasında bir tarayıcıdan ve verileri güncelleştirin. Bu kez, bir yardımcı durum iletisi görüntülenir.


[![Kısa bir ileti görüntülenir, ayarların güncelleştirildiğinden olduğu](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Şekil 16**: ayarları güncelleştirildiğinde kısa bir ileti görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> DetailsView denetiminde arabirimi bırakır, istenen için çok fazla düzenleme kullanıcının. Standart boyutlu metin kutuları kullanır, ancak imza alan büyük olasılıkla çok satırlı bir metin olmalıdır. Giriş sayfası URL girdiyseniz, "http://" veya "https://" ile başladığından emin olmak için bir RegularExpressionValidator kullanılmalıdır. Ayrıca, denetime sahip DetailsView beri kendi `DefaultMode` özelliğini `Edit`, iptal düğmesine hiçbir şey yapmaz. Ya da kaldırılması gerektiğini veya tıklandığında yönlendirilmeniz kullanıcı başka bir sayfaya (gibi `~/Default.aspx`). Bu geliştirmeler için okuyucu bir alıştırma olarak bırakın.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Bağlantı ekleme`AdditionalUserInfo.aspx`ana sayfa içinde sayfa

Şu anda Web sitesi bağlantılarını sağlamaz `AdditionalUserInfo.aspx` sayfası. Ulaşmak için tek yolu, sayfanın URL'sini doğrudan tarayıcının adres çubuğuna girmektir. Bir bağlantı içinde bu sayfaya ekleyelim `Site.master` ana sayfa.

Ana sayfaya bir LoginView Web denetimi içerdiğini geri çağırma kendi `LoginContent` farklı biçimlendirme için kimliği doğrulanmış ve anonim ziyaretçiler görüntüler ContentPlaceHolder. LoginView denetimin güncelleştirme `LoggedInTemplate` bir bağlantı eklemek için `AdditionalUserInfo.aspx` sayfası. LoginView bu değişiklikleri yaptıktan sonra denetimin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Ek unutmayın `lnkUpdateSettings` köprü denetimini `LoggedInTemplate`. Bu bağlantıyla bir yerde kimliği doğrulanmış kullanıcılar sayfasına görüntülemek ve giriş Şehir, giriş sayfası ve imza ayarlarını değiştirmek için hızla atlayabilirsiniz.

## <a name="step-4-adding-new-guestbook-comments"></a>4. adım: Yeni Konuk açıklama ekleme

`Guestbook.aspx` Sayfasıdır burada kimliği doğrulanmış kullanıcılar Konuk görüntüleyebilir ve bir yorum yazın. Yeni Konuk yorum eklemek için arabirimi oluşturmayla başlayalım.

Açık `Guestbook.aspx` sayfasında Visual Studio'da ve yeni açıklamanın konu biri diğeri kendi gövdesi için iki TextBox oluşan bir kullanıcı arabirimi oluşturun. İlk metin kutusu denetiminin ayarlayın `ID` özelliğini `Subject` ve kendi `Columns` 40 özelliğini; ayarlanmış ikinci kişinin `ID` için `Body`, kendi `TextMode` için `MultiLine`ve onun `Width` ve `Rows` "% 95" ve 8, özellikleri sırasıyla. Kullanıcı arabirimini tamamlanması adlı bir düğme Web denetimi ekleme `PostCommentButton` ve kendi `Text` özelliğini "Your Açıklama Gönder".

Konuk açıklamaları, konu ve gövde gerektirdiğinden, bir RequiredFieldValidator her metin kutuları ekleyin. Ayarlama `ValidationGroup` özelliğini "EnterComment"; Bu denetimlere benzer şekilde, ayarlayın `PostCommentButton` denetimin `ValidationGroup` "EnterComment" özelliğini. ASP hakkında daha fazla bilgi için. NET doğrulama denetimleri, kullanıma [ASP.NET Form doğrulaması](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [doğrulama denetimleri ASP.NET 2.0 ayrıntıları](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)ve [doğrulama sunucu denetimleri öğretici](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) üzerinde [W3Schools](http://www.w3schools.com/).

Kullanıcı arabirimi kaynaklı sonra bildirim temelli işaretleme, sayfanın aşağıdaki gibi görünmelidir:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Kullanıcı arabirimi tam olarak, bizim sonraki görevi, yeni bir kayıt eklemek `GuestbookComments` tablosundan `PostCommentButton` tıklandığında. Bu çeşitli yollarla gerçekleştirilebilir: düğmenin ADO.NET kod yazabiliriz `Click` olay işleyicisi; biz sayfaya SqlDataSource denetimi ekleyin, yapılandırma, `InsertCommand`ve sonra çağrı kendi `Insert` yönteminden `Click` olay işleyici; ya da Biz yeni konuk açıklamaları ekleme için sorumlu bir orta katman oluşturmak ve bu işlevi çağırmak `Click` olay işleyicisi. Adım 3'te bir SqlDataSource kullanarak incelemiştik olduğundan, ADO.NET kod burada kullanalım.

> [!NOTE]
> Verileri bir Microsoft SQL Server veritabanındaki verileri program aracılığıyla erişmek için kullanılan ADO.NET sınıflarını bulunan `System.Data.SqlClient` ad alanı. Bu ad alanı sayfanızın kod arkası sınıfı almanız gerekebilir (yani, `using System.Data.SqlClient;`).


İçin bir olay işleyicisi oluşturun `PostCommentButton`'s `Click` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

`Click` Olay işleyicisi, kullanıcı tarafından sağlanan verilerin geçerli olup olmadığını denetleyerek başlar. Yüklü değilse, olay işleyicisi, bir kaydı eklemeden önce çıkar. Sağlanan veriler geçerli olduğunu varsayarak o anda oturum açmış kullanıcının `UserId` değer alınan ve depolanan `currentUserId` yerel değişken. Bu değer sağlamanız gerekir çünkü gerekli bir `UserId` değeri bir kayıtta eklerken `GuestbookComments`.

Bağlantı dizesi için `SecurityTutorials` veritabanı alınır `Web.config` ve `INSERT` SQL deyimi belirtildi. A `SqlConnection` nesne sonra oluşturulup açılır. Ardından, bir `SqlCommand` nesnesi oluşturulduğunda ve parametreler için değerleri kullanılan `INSERT` sorgu atanır. `INSERT` Deyimi sonra yürütülür ve bağlantı kapalı. Olay işleyicisi sonunda `Subject` ve `Body` metin kutuları `Text` özellikleri temizlenir böylece kullanıcının değerleri arasında bir geri gönderme kalıcı değildir.

Devam edin ve bu sayfası tarayıcıda test edin. Bu sayfa kullanıldığından `Membership` klasörü, anonim yayımlanacağını ve Ziyaretçiler erişilebilir değil. Bu nedenle, (zaten varsa) ilk kez oturum gerekecektir. İçine bir değer girin `Subject` ve `Body` metin kutuları tıklatıp `PostCommentButton` düğmesi. Bu eklenmesi yeni bir kayıt neden `GuestbookComments`. Geri göndermede, konu ve gövde sağladığınız kutularındaki metinleri temizlenir.

' I tıklattıktan sonra `PostCommentButton` düğme var olan hiçbir görsel geri bildirim için konuk Açıklama eklenmiş. Biz yine de 5. adımda yapacağız mevcut konuk açıklamaları görüntülemek için bu sayfayı güncelleştirmeniz gerekir. Biz bunu yapmaya sonra yeni eklenen açıklama yeterli görsel geri bildirim sağlama yorumları listesinde görünür. Şimdilik, Konuk yorumunuzu içeriğini inceleyerek kaydedildiğini onaylamak `GuestbookComments` tablo.

Şekil 17 içeriğini gösterir `GuestbookComments` iki açıklamalar bırakıldı sonra tablo.


[![Konuk yorumları GuestbookComments tabloda görebilirsiniz.](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Şekil 17**: Konuk açıklamalarda gördüğünüz `GuestbookComments` tablo ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Tehlikeli biçimlendirme – HTML – ASP.NET gibi bir kullanıcı potansiyel olarak içeren bir konuk açıklama eklemeye çalışırsa oluşturmaz bir `HttpRequestValidationException`. Neden durum, bu özel durum ve potansiyel olarak tehlikeli değerleri göndermek için kullanıcılara izin ver hakkında daha fazla bilgi için başvurun [istek doğrulama teknik incelemesi](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>5. adım: var olan konuk açıklamaları listeleme

Ek açıklamalar ziyaret eden bir kullanıcının bırakarak `Guestbook.aspx` sayfa Konuk'ın var olan açıklamaları görüntülemek için olmalıdır. Bunu gerçekleştirmek için adlı ListView denetimine ekleme `CommentList` sayfanın altına.

> [!NOTE]
> ListView denetimi, sürüm 3.5 ASP.NET için yeni bir özelliktir. Öğe listesi çok özelleştirilebilir ve esnek bir düzende görüntüler, ancak yine de yerleşik düzenleme, ekleme, silme, sayfalama ve sıralama GridView gibi işlevler sunar için tasarlanmıştır. ASP.NET 2.0 kullanıyorsanız, DataList veya Repeater denetim kullanmanız gerekir. ListView kullanma hakkında daha fazla bilgi için bkz. [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog girişine [asp: ListView denetimi](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)ve benim makale [ListView denetimi ile veri görüntüleme](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


ListView'ın akıllı etiket açın ve veri kaynağı Seç açılan listeden, Denetim yeni bir veri kaynağına bağlama. 2. adımda gördüğümüz gibi bu veri kaynağı Yapılandırma Sihirbazı başlatılır. Veritabanı simgesini seçin, sonuçta elde edilen SqlDataSource ad `CommentsDataSource`, Tamam'ı tıklatın. Ardından, `SecurityTutorialsConnectionString` bağlantı dizesi aşağı açılan listeden ve İleri'ye tıklayın.

Bu noktada 2. adımda sorgu veri çekme tarafından belirlemiş `UserProfiles` tablo aşağı açılan listeden ve döndürülecek olan sütunları seçme (Şekil 9'a geri bakın). Bu kez, ancak yalnızca kayıtlardan geri çeker bir SQL deyimi çalışıyorlardı istiyoruz `GuestbookComments`, ancak aynı zamanda yorum yapan'ın Giriş Şehir, giriş sayfası, imza ve kullanıcı adı. Bu nedenle, "özel bir SQL deyimi veya saklı yordam belirtin" radyo düğmesini seçin ve İleri'ye tıklayın.

Bu, "Tanımlama özel deyimleri veya saklı yordamlar" ekranını getirir. Grafik sorgusu oluşturmak için Sorgu Oluşturucu düğmesine tıklayın. Sorgu Oluşturucusu sorgu için istediğimiz tabloları belirtmek için bize isteyerek başlar. Seçin `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tablolar ve Tamam'a tıklayın. Bu üç sorgularınızdan tasarım yüzeyine ekleyin. Yabancı anahtar kısıtlamaları arasında olduğundan `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tabloları, Sorgu Oluşturucusu otomatik olarak `JOIN` s Bu tablolar.

Kalan tek şey döndürülecek olan sütunları belirlemek için. Gelen `GuestbookComments` tablo seçin `Subject`, `Body`, ve `CommentDate` sütunları; return `HomeTown`, `HomepageUrl`, ve `Signature` sütunlarından `UserProfiles` tablo; ve dönüş `UserName` gelen`aspnet_Users`. Ayrıca, "`ORDER BY CommentDate DESC`" sonuna `SELECT` böylece en son gönderiler döndürülen ilk sorgu. Bu seçimleri yaptıktan sonra Sorgu Oluşturucu Arabiriminizin Şekil 18'ekran şuna benzemelidir.


[![Constructed sorgu GuestbookComments UserProfiles ve aspnet_Users tabloları birleştirir.](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Şekil 18**: oluşturulan sorgu `JOIN` s `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tablolar ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image54.png))


Sorgu Oluşturucu pencereyi kapatın ve "Tanımlayan özel deyimleri veya saklı yordamlar" ekranına geri dönmek için Tamam'ı tıklatın. Gelişmiş sorgu sonuçları Test sorgusu düğmesini tıklatarak görüntüleyebileceğiniz "Test sorgusu" ekran için İleri'yi tıklatın. Hazır olduğunuzda, veri kaynağı Yapılandırma Sihirbazı'nı tamamlamak için Son'u tıklatın.

Veri Kaynağı Yapılandırma Sihirbazı'nda Adım 2, ilişkili DetailsView denetiminde tamamlandığında size `Fields` koleksiyon tarafından döndürülen her sütun için bir BoundField içerecek şekilde güncelleştirildi `SelectCommand`. ListView ancak değişmeden kalır; Biz yine de kendi düzen tanımlamanız gerekir. ListView'ın düzen, bildirim temelli biçimlendirme veya "Yapılandırma ListView" seçeneği, akıllı etiket, el ile oluşturulabilir. Ben genellikle biçimlendirme el ile tanımlama tercih eder, ancak sizin için en iyi yöntem kullanın.

Aşağıdakileri kullanarak miyim sonuçlandı `LayoutTemplate`, `ItemTemplate`, ve `ItemSeparatorTemplate` my ListView denetimi için:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

`LayoutTemplate` Tanımlar biçimlendirme denetimi tarafından yayılan, while `ItemTemplate` SqlDataSource tarafından döndürülen her bir öğe işler. `ItemTemplate`'S elde edilen biçimlendirme yerleştirilir `LayoutTemplate`'s `itemPlaceholder` denetimi. Ek olarak `itemPlaceholder`, `LayoutTemplate` gösteren sayfa (varsayılan) başına yalnızca 10 konuk açıklamaları için ListView sınırlayan bir DataPager denetimi içerir ve bir disk belleği arabirimi işler.

My `ItemTemplate` her Konuk açıklamanın konu görüntüler bir `<h4>` konu yer gövdesi olan öğe. Gövde görüntülemek için kullanılan sözdizimi tarafından döndürülen veri aldığını unutmayın `Eval("Body")` veri bağlama ifadesi, bir dizeye dönüştürür ve değiştirir satır sonları ile `<br />` öğesi. Bu dönüştürme, boşluk, HTML tarafından göz ardı edilir olduğundan açıklama gönderirken girilen satır sonlarını göstermek için gereklidir. Kullanıcının imzası, italik, kullanıcının giriş Şehir, kendi giriş sayfası, tarih ve zaman yorumun yapıldığı ve yorumu yapan kişinin kullanıcı adı için bir bağlantı ardından gövdesinde altında görüntülenir.

Bir tarayıcı aracılığıyla sayfasını görüntülemek için bir dakikanızı ayırın. Burada görüntülenen adım 5'te Konuk eklenen açıklamalar görmeniz gerekir.


[![Konuk'ın açıklamaları Guestbook.aspx şimdi görüntüler](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Şekil 19**: `Guestbook.aspx` artık Konuk'ın açıklamaları görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image57.png))


Yeni bir açıklama için konuk eklemeyi deneyin. Üzerine tıklayarak `PostCommentButton` düğmesi sayfanın geri gönderir ve yorum veritabanına eklenir, ancak ListView denetimi yeni yorum gösterecek şekilde güncelleştirilmez. Bu durum ya da düzeltilebilir:

- Güncelleştirme `PostCommentButton` düğmenin `Click` BT'nin ListView denetimin çağırır, böylece olay işleyicisi `DataBind()` yöntemi veritabanına yeni açıklama ekleme sonra veya
- ListView denetim ayarı `EnableViewState` özelliğini `false`. Denetimin görünüm durumunu devre dışı bırakarak, temel alınan veriler üzerinde her geri gönderme için yeniden bağlamanız gerekir çünkü bu yaklaşım çalışır.

Bu öğreticide indirilebilir Eğitmen Web sitesi iki teknik gösterilmektedir. ListView denetimin `EnableViewState` özelliğini `false` ve ListView verileri programlı olarak yeniden bağlamak için gereken kodu varsa `Click` olay işleyicisi, devre dışı bırakılmışsa ancak.

> [!NOTE]
> Şu anda `AdditionalUserInfo.aspx` sayfası sağlar kullanıcının kendi giriş Şehir, giriş sayfası ve imza ayarlarını görüntüleyin ve düzenleyin. Güncelleştirmek kullanışlı olabilir `AdditionalUserInfo.aspx` oturum açmış kullanıcının konuk açıklamaları görüntülemek için. Diğer bir deyişle, inceleme ve kendi bilgi değiştirme ek olarak, bir kullanıcı ziyaret edebilirsiniz `AdditionalUserInfo.aspx` geçmişte yapılan Filiz hangi konuk açıklamaları görmek için sayfayı. Ben bunu bir alıştırma olarak için ilginizi okuyucu bırakın.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>6. adım: Giriş Şehir, giriş sayfası ve imza için bir arabirim içerecek şekilde CreateUserWizard denetimini özelleştirme

`SELECT` Sorgu tarafından kullanılan `Guestbook.aspx` sayfasında kullanan bir `INNER JOIN` ilgili kayıtlar arasında birleştirilecek `GuestbookComments`, `UserProfiles`, ve `aspnet_Users` tablolar. Hiçbir kayıt sahip bir kullanıcı `UserProfiles` Konuk hale açıklama, yorum olmaz görüntülenecek ListView içinde olduğundan `INNER JOIN` yalnızca döndürür `GuestbookComments` eşleşen kayıtları olduğunda kayıtları `UserProfiles` ve `aspnet_Users`. Ve bir kullanıcı bir kayıt sahip olmayan, adım 3'te gördüğümüz gibi `UserProfiles` Filiz görüntüleyemez veya düzenleyemezsiniz kendi ayarlarında `AdditionalUserInfo.aspx` sayfası.

Needless çok deyin nedeniyle bizim tasarım kararları önemlidir her kullanıcı hesabı üyelik sistemini eşleştirmesi olan kaydetmek `UserProfiles` tablo. İstediğimiz eklenecek karşılık gelen bir kayıt içindir. `UserProfiles` yeni bir üyelik kullanıcı hesabı CreateUserWizard oluşturulan her.

Bölümünde açıklandığı gibi [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-cs.md) CreateUserWizard denetim yeni bir üyelik kullanıcı hesabı oluşturulduktan sonra öğretici başlatır, [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Biz bu olay için bir olay işleyicisi oluşturun, yeni oluşturulan kullanıcının kullanıcı kimliği alır ve ardından bir kayıtta Ekle `UserProfiles` tablo için varsayılan değerlerle `HomeTown`, `HomepageUrl`, ve `Signature` sütun. Üstelik ek metin kutuları içerecek şekilde CreateUserWizard denetimin arabirimi özelleştirerek kullanıcıdan bu değerleri için mümkündür.

İlk yeni satır ekleme göz atalım `UserProfiles` tablosundaki `CreatedUser` varsayılan değerlerle olay işleyicisi. Yeni kullanıcının giriş Şehir, giriş sayfası ve imza toplamak için ek form alanlarını içerecek şekilde CreateUserWizard denetimin kullanıcı arabirimini özelleştirmek nasıl görürsünüz.

### <a name="adding-a-default-row-touserprofiles"></a>Varsayılan satır ekleme`UserProfiles`

İçinde [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-cs.md) CreateUserWizard denetime ekledik öğretici `CreatingUserAccounts.aspx` sayfasını `Membership` klasör. Denetim CreateUserWizard sahip olmak için bir kayda ekleme `UserProfiles` tablo kullanıcı hesabı oluşturulduktan sonra biz CreateUserWizard denetiminin işlevselliğini güncelleştirmeniz gerekir. Bu değişiklikleri yapmak yerine `CreatingUserAccounts.aspx` sayfasında, bunun yerine yeni CreateUserWizard denetime ekleyelim `EnhancedCreateUserWizard.aspx` sayfasında ve vardır, Bu öğretici için değişiklikleri yapın.

Açık `EnhancedCreateUserWizard.aspx` sayfasında Visual Studio'da ve sayfaya araç kutusundan bir CreateUserWizard denetimi sürükleyin. CreateUserWizard denetimin ayarlamak `ID` özelliğini `NewUserWizard`. Açıkladığımız gibi <a id="_msoanchor_5"> </a> [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-cs.md) öğretici CreateUserWizard'ın varsayılan kullanıcı arabirimi ziyaretçi için gerekli bilgileri ister. Bu bilgiler sağlanan sonra denetimi dahili olarak yeni bir kullanıcı hesabı üyelik Framework'teki tüm ABD tek satır kod yazmanıza gerek kalmadan oluşturur.

CreateUserWizard denetimi olay sayısı, iş akışı sırasında başlatır. Bir ziyaretçi istek bilgileri sağlayan ve formu gönderdikten sonra CreateUserWizard denetimi başlangıçta harekete kendi [ `CreatingUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Oluşturma işlemi sırasında bir sorun varsa [ `CreateUserError` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) tetiklenir; ancak, kullanıcı başarıyla oluşturulursa, ardından [ `CreatedUser` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) tetiklenir. İçinde <a id="_msoanchor_6"> </a> [ *kullanıcı hesapları oluşturma* ](creating-user-accounts-cs.md) öğretici için bir olay işleyicisi oluşturduğumuz `CreatingUser` gelen tüm boşlukları sağlanan kullanıcı adını içermiyordu emin olmak için olay veya sondaki boşlukları ve kullanıcı adı parolanın her yerde görülmedi.

Bir satır eklemek için `UserProfiles` tablo için bir olay işleyicisi oluşturmak ihtiyacımız yeni oluşturulan kullanıcı için `CreatedUser` olay. Zamana göre `CreatedUser` olay harekete, kullanıcı hesabının, hesabın kimliği değerini almak bize etkinleştirme üyelik framework oluşturmuş oldunuz.

İçin bir olay işleyicisi oluşturun `NewUserWizard`'s `CreatedUser` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Yeni eklenen kullanıcı hesabının kullanıcı kimliği alarak Yukarıdaki kod için önemlidir. Bu kullanılarak başarılır `Membership.GetUser(username)` bir belirli kullanıcı ve sonra kullanma hakkında bilgi döndürmek için yöntemin `ProviderUserKey` kendi kullanıcı kimliğini almak için özellik. CreateUserWizard denetiminin kullanıcı tarafından girilen kullanıcı adı aracılığıyla kullanılabilir kendi [ `UserName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Ardından, bağlantı dizesinin alındığı `Web.config` ve `INSERT` deyimi belirtildi. Gerekli ADO.NET nesneleri örneği oluşturulur ve komut yürütülür. Kod atar bir [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) için örnek `@HomeTown`, `@HomepageUrl`, ve `@Signature` veritabanı ekleme etkisi olan parametreleri `NULL` değerleri `HomeTown`, `HomepageUrl`, ve `Signature` alanları.

Ziyaret `EnhancedCreateUserWizard.aspx` sayfasında bir tarayıcıdan ve yeni bir kullanıcı hesabı oluşturun. Bunu yaptıktan sonra Visual Studio'ya geri dönün ve içeriğini inceleyin `aspnet_Users` ve `UserProfiles` (Şekil 12'deki yaptığımız gibi) tablolar. Yeni kullanıcı hesabını görmelisiniz `aspnet_Users` ve karşılık gelen `UserProfiles` satır (ile `NULL` değerleri `HomeTown`, `HomepageUrl`, ve `Signature`).


[![Yeni kullanıcı hesabı ve UserProfiles kaydı eklenir](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Şekil 20**: yeni bir kullanıcı hesabı ve `UserProfiles` kaydı eklenir ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image60.png))


Ziyaretçi kendi yeni hesap bilgileri sağladığı ve "Kullanıcı oluştur" düğmesine tıkladı, kullanıcı hesabı oluşturulur ve bir satır eklenir sonra `UserProfiles` tablo. Ardından CreateUserWizard görüntüler, `CompleteWizardStep`, bir başarı iletisi ve bir devam düğmesi görüntüler. Devam düğmesine tıklayarak bir geri göndermenin neden olur, ancak hiçbir işlem yapılmadı, kullanıcı bırakarak takılı üzerinde `EnhancedCreateUserWizard.aspx` sayfası.

Devam düğmesine CreateUserWizard denetimin tıklandığında için kullanıcıya gönderilecek bir URL belirttiğimiz [ `ContinueDestinationPageUrl` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Ayarlama `ContinueDestinationPageUrl` özelliğini "~ / Membership/AdditionalUserInfo.aspx". Bu yeni kullanıcıya sürer `AdditionalUserInfo.aspx`, burada görüntüleyebilir ve ayarlarını güncelleştirin.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Yeni kullanıcının giriş Şehir, giriş sayfası ve imza sor CreateUserWizard'ın arabirime özelleştirme

CreateUserWizard denetimin varsayılan arabirim adı, parola ve e-posta gibi yalnızca çekirdek kullanıcı hesabı bilgilerini nerede toplanan basit hesap oluşturma senaryoları için yeterli olur. Ancak ne ziyaretçi hesabını oluştururken kendi giriş Şehir, giriş sayfası ve imza girmek için istemleri istedik? Kayıt sırasında ek bilgi toplamak için CreateUserWizard denetimin arayüzünü özelleştirmek mümkündür ve bu bilgileri kullanılıyor olabilecek `CreatedUser` ek kayıtlar temel alınan veritabanına eklemek için olay işleyicisi.

Sıralı bir dizi tanımlamak bir sayfa Geliştirici sağlayan bir denetimi olan ASP.NET sihirbazından denetim CreateUserWizard denetim genişletir `WizardSteps`. Sihirbaz Denetimi Etkin adım işler ve bu adımları taşımak ziyaretçi izin veren bir gezinti arabirimi sağlar. Sihirbaz denetimi, uzun bir görev birden çok kısa adımlara bölmek için idealdir. Sihirbaz denetimi hakkında daha fazla bilgi için bkz. [ASP.NET 2.0 Sihirbazı denetimi ile bir adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

İki CreateUserWizard denetimin varsayılan biçimlendirme tanımlar `WizardSteps`: `CreateUserWizardStep` ve `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

İlk `WizardStep`, `CreateUserWizardStep`, kullanıcı adı, parola, e-posta ve benzeri için ister arabirimi oluşturur. Ziyaretçi bu bilgileri sağlayan ve "Create User" tıkladığında sonra Filiz gösterilen `CompleteWizardStep`, başarılı iletisi ve bir devam düğmesi gösterilir.

Ek form alanlarını içerecek şekilde CreateUserWizard denetimin arayüzünü özelleştirmek için yapabiliriz:

- <strong>Bir veya daha yeni Oluştur</strong><strong>`WizardStep`</strong><strong>ek kullanıcı arabirimi öğeleri içerecek şekilde s</strong>. Yeni bir `WizardStep` CreateUserWizard için tıklayın "Ekle/Kaldır `WizardSteps`" Akıllı kendi başlatmak için etiket bağlantıdan `WizardStep` Koleksiyonu Düzenleyicisi. Buradan ekleyin, kaldırın veya sihirbazdaki adımları yeniden sıralayabilir. Bu öğretici için kullanacağız yaklaşım budur.

- <strong>Dönüştürme</strong><strong>`CreateUserWizardStep`</strong><strong>içine düzenlenebilir bir</strong><strong>`WizardStep`</strong><strong>.</strong> Bu değiştirir `CreateUserWizardStep` ile eşdeğer `WizardStep` , biçimlendirme, eşleşen bir kullanıcı arabirimi tanımlar `CreateUserWizardStep`' s. Dönüştürerek `CreateUserWizardStep` içine bir `WizardStep` biz de denetimleri yeniden konumlandırma veya ek kullanıcı arabirimi öğeleri için bu adımı ekleyin. Dönüştürülecek `CreateUserWizardStep` veya `CompleteWizardStep` içine düzenlenebilir bir `WizardStep`tıklayın "Özelleştir kullanıcı oluşturma adım" veya "Tamamlandı adımında özelleştirme" akıllı etiket denetimin bağlantı.

- **İki Yukarıdaki seçeneklerin bir bileşimi kullanın.**

Akılda tutulması gereken önemli bir unsur olan "Kullanıcı oluştur" düğmesine içinden tıklandığında CreateUserWizard denetimin, kullanıcı hesabı oluşturma işlemi yürütür, `CreateUserWizardStep`. Varsa ek farketmez `WizardStep` %s ifadesinden sonra `CreateUserWizardStep` veya yok.

Özel bir eklerken `WizardStep` CreateUserWizard denetime özel ek kullanıcı girişini toplamak için `WizardStep` önce veya sonra yerleştirilebilir `CreateUserWizardStep`. Önce geliyorsa, `CreateUserWizardStep` ardından ek kullanıcı girişini toplanan özel `WizardStep` kullanılabilir `CreatedUser` olay işleyicisi. Ancak, özel `WizardStep` sonra gelen `CreateUserWizardStep` ardından zamana göre özel `WizardStep` görüntülenir yeni kullanıcı hesabının zaten oluşturulmuş ve `CreatedUser` olay zaten harekete.

Şekil 21 iş akışı gösterir, eklenen `WizardStep` önündeki `CreateUserWizardStep`. Ek kullanıcı bilgileri zamanında toplanmış olan bu yana `CreatedUser` olayı tetiklendiğinde, tüm yapmak için sahip olduğumuz olan güncelleştirme `CreatedUser` bu girişler alma ve olanlar için olay işleyicisi `INSERT` deyimin parametre değerlerini (yerine `DBNull.Value`).


[![CreateUserWizard ek bir WizardStep CreateUserWizardStep'e önündeki olduğunda iş akışı](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Şekil 21**: CreateUserWizard iş akışı, bir ek `WizardStep` Precedes `CreateUserWizardStep` ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image63.png))


Varsa özel `WizardStep` yerleştirilir *sonra* `CreateUserWizardStep`, kullanıcının kendi giriş Şehir, giriş sayfası veya imza girmek için bir fırsat önce ancak, kullanıcı hesabı oluşturma işlemi gerçekleşir. Böyle bir durumda, bu ek bilgiler Şekil 22 gösterildiği gibi kullanıcı hesabı oluşturulduktan sonra veritabanına eklenmesi gerekir.


[![CreateUserWizard ek bir WizardStep CreateUserWizardStep'e sonra söz konusu olduğunda iş akışı](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Şekil 22**: CreateUserWizard iş akışı, bir ek `WizardStep` gelen sonra `CreateUserWizardStep` ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image66.png))


Bir kayıtta eklemek için Şekil 22'de gösterilen iş akışı bekler `UserProfiles` kadar 2. adım tamamlandıktan sonra tablo. Ziyaretçi 1. adımdan sonra her tarayıcı kapatırsa, ancak burada bir kullanıcı hesabı oluşturuldu, ancak hiçbir kayıt eklenmiş olan bir durum ulaştık `UserProfiles`. Bir çözüm olan bir kayıt için `NULL` veya varsayılan değerlerin eklendiği `UserProfiles` içinde `CreatedUser` (1. adımdan sonra ateşlenir) olay işleyicisini ve 2. adım tamamlandıktan sonra bu kaydı güncelleştirin. Bu, sağlar bir `UserProfiles` kaydı kullanıcı kayıt işlem sürecin yarısında çıkar olsa bile kullanıcı hesabı için eklenir.

Bu öğretici için yeni bir oluşturalım `WizardStep` sonra oluşan `CreateUserWizardStep` önce `CompleteWizardStep`. Şimdi de WizardStep yerleştirin ve yapılandırılmış ilk alın ve ardından koda göz atmak.

CreateUserWizard denetimin akıllı etiketten seçin "Ekle/Kaldır `WizardStep` s", hangi getirir `WizardStep` Koleksiyonu Düzenleyicisi iletişim kutusu. Yeni bir `WizardStep`, ayar, `ID` için `UserSettings`, kendi `Title` "Ayarlarınızı" için ve kendi `StepType` için `Step`. Böylece sonra gelen konumlandırın `CreateUserWizardStep` ("kaydolma yeni hesabınız") ve önce `CompleteWizardStep` ("tam"), Şekil 23'te gösterildiği gibi.


[![Yeni bir WizardStep CreateUserWizard denetimine ekleme](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Şekil 23**: yeni bir ekleme `WizardStep` CreateUserWizard denetlemek için ([tam boyutlu görüntüyü görmek için tıklatın](storing-additional-user-information-cs/_static/image69.png))


Kapatmak için Tamam'a tıklayın `WizardStep` Koleksiyonu Düzenleyicisi iletişim kutusu. Yeni `WizardStep` CreateUserWizard denetimin güncelleştirilmiş bildirim temelli biçimlendirme tarafından yi:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Yeni Not `<asp:WizardStep>` öğesi. Yeni kullanıcının giriş Şehir, giriş sayfası ve burada İmza toplamak için kullanıcı arabirimi eklemeniz gerekir. Bu içerik, bildirim temelli söz dizimi veya Tasarımcısı aracılığıyla girebilirsiniz. Tasarımcıyı kullanmak için "Ayarlarınızı" adım adım Tasarımcısı'nda görmek için akıllı etiket aşağı açılan listeden seçin.

> [!NOTE]
> Akıllı etiketin açılır listede ilerleyin seçerek güncelleştirmeleri CreateUserWizard denetimin [ `ActiveStepIndex` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), başlangıç adım dizinini belirtir. Tasarımcıda "Ayarlarınızı" adımı düzenlemek için bu açılan listeyi kullanın, bu nedenle, bu adım, kullanıcılar ilk kez ziyaret ettiğinizde gösterilir böylece geri "oturum Up for Your yeni hesap" ayarladığınızdan emin olun `EnhancedCreateUserWizard.aspx` sayfası.


Adlı üç TextBox denetimleri içeren "Ayarlarınızı" adımı içerisinde bir kullanıcı arabirimi oluşturma `HomeTown`, `HomepageUrl`, ve `Signature`. Bu arabirim oluşturduktan sonra bildirim temelli CreateUserWizard'ın işaretleme aşağıdakine benzer görünmelidir:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Devam edin ve bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve giriş Şehir, giriş sayfası ve imza için değerler belirten yeni bir kullanıcı hesabı oluşturun. Tamamladıktan sonra `CreateUserWizardStep` kullanıcı hesabı üyelik çerçevesinde oluşturulur ve `CreatedUser` yeni bir satır ekler olay işleyicisi çalışır `UserProfiles`, ancak bir veritabanıyla `NULL` değerini `HomeTown`, `HomepageUrl`, ve `Signature`. Giriş Şehir, giriş sayfası ve imza için girilen değerler hiçbir zaman kullanılmaz. Yeni bir kullanıcı hesabı ile net sonucu olan bir `UserProfiles` ayarlanmış kayıt `HomeTown`, `HomepageUrl`, ve `Signature` alanınız Henüz belirtilmelidir.

Kullanıcı tarafından girilen giriş Şehir, honepage ve imza değerlerini alır ve uygun güncelleştiren "Ayarlarınızı" adımından sonra kodu çalıştırmak ihtiyacımız `UserProfiles` kaydı. Kullanıcı hareket Sihirbazdaki adımlar arasında her zaman kontrol, sihirbazın [ `ActiveStepChanged` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) ateşlenir. Bu olay ve güncelleştirme için bir olay işleyicisi oluşturabiliriz `UserProfiles` "Ayarlarınızı" adım tamamlandıktan sonra tablo.

Bir olay işleyicisi ekleme CreateUserWizard için 's `ActiveStepChanged` olay ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Yukarıdaki kod, biz yalnızca "Tam" adım ulaştıysa belirleyerek başlatır. "Tamamlandı" adımından hemen "Ayarlarınızı" adımından sonra gerçekleştiğinde olduğundan, ziyaretçi ulaştığında ardından "Tamamlandı", Filiz, yalnızca "Ayarlarınızı" adım tamamlandı anlamına gelir. adımı.

Böyle bir durumda, biz TextBox denetimi içinde program aracılığıyla başvurmanız gerekir `UserSettings WizardStep`. Bu ilk kullanılarak başarılır `FindControl` programlı olarak başvuran yöntemi `UserSettings WizardStep`ve ardından tekrar metin kutuları içinden başvurmak için `WizardStep`. Metin kutuları başvurulan sonra yürütülecek hazırız `UPDATE` deyimi. `UPDATE` Deyimi sahip aynı sayıda parametre olarak `INSERT` deyiminde `CreatedUser` olay işleyicisi, ancak kullanıcı tarafından sağlanan giriş Şehir, giriş sayfası ve imza değerleri burada kullanırız.

Bu olay işleyicisi ile yerinde ziyaret `EnhancedCreateUserWizard.aspx` sayfasında bir tarayıcıdan ve giriş Şehir, giriş sayfası ve imza için değerler belirten yeni bir kullanıcı hesabı oluşturun. Yeni hesap oluşturduktan sonra için yeniden yönlendirilmesi gereken `AdditionalUserInfo.aspx` just-girilen giriş Şehir, giriş sayfası ve imza bilgileri görüntüleyen sayfa.

> [!NOTE]
> Web sitemizi şu anda, ziyaretçi yeni bir hesap oluşturabilirsiniz iki sayfası vardır: `CreatingUserAccounts.aspx` ve `EnhancedCreateUserWizard.aspx`. Web sitesinin site haritası ve oturum açma sayfasına gelin `CreatingUserAccounts.aspx` sayfasında, ancak `CreatingUserAccounts.aspx` sayfa kullanıcıdan giriş Şehir, giriş sayfası ve imza bilgilerini sormaz ve karşılık gelen bir satır eklemez `UserProfiles`. Bu nedenle, ya da güncelleştirme `CreatingUserAccounts.aspx` bu işlevselliği sunar, böylece sayfa veya başvurmak için site haritası ve oturum açma sayfası `EnhancedCreateUserWizard.aspx` yerine `CreatingUserAccounts.aspx`. İkinci seçeneği tercih ederseniz güncelleştirdiğinizden emin olun `Membership` klasörün `Web.config` anonim kullanıcılar erişmesine izin vermek için dosyanın `EnhancedCreateUserWizard.aspx` sayfası.


## <a name="summary"></a>Özet

Bu öğreticide üyelik framework içindeki kullanıcı hesapları ile ilgili veri modelleme teknikleri inceledik. Özellikle, bire bir ilişki paylaşan verilerin yanı sıra kullanıcı hesapları ile bire çok ilişkisi paylaşan varlıkları modelleme incelemiştik. Ayrıca, bu bilgi görüntülenen, eklenen ve SqlDataSource denetimi ve diğer kullanarak bazı örnekler ile güncelleştirilemez ilişkisini gördüğümüz ADO.NET kod kullanarak.

Bu öğretici, kullanıcı hesaplarını bizim göz tamamlar. Sonraki öğretici ile başlayan uygulamamızla rollerine kaldıracağız. Sonraki birkaç öğreticiler size rolleri framework'görünür bkz yeni roller oluşturma nasıl hangi rolleri belirlemek için bir kullanıcının ait olduğu ve nasıl rolleri, kullanıcılara atamak rol tabanlı yetkilendirme uygulamak için.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Erişim ve ASP.NET 2.0 verileri güncelleştirme](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [ASP.NET 2.0 Sihirbazı denetimi ile adım adım kullanıcı arabirimi oluşturma](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Özel veri kaynağı denetim parametreleri oluşturma](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [CreateUserWizard denetimini özelleştirme](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [DetailsView denetiminde hızlı Başlangıçlar](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [ListView denetimi ile verileri görüntüleme](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [ASP.NET 2.0 doğrulama denetimleri ayrıntıları](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [INSERT düzenleme ve verileri silme](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [ASP.NET form doğrulama](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Özel kullanıcı kayıt bilgileri toplanıyor](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 profilleri](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView denetimi](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Kullanıcı profilleri hızlı başlangıç](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](user-based-authorization-cs.md)
> [İleri](creating-the-membership-schema-in-sql-server-vb.md)
