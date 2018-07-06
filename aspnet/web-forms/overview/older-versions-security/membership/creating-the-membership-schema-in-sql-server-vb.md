---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: (VB) SQL Server'da üyelik şeması oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide gerekli şema SqlMembershipProvider kullanmak için veritabanına ekleme teknikleri inceleyerek başlar. Ardından, biz wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 6fe5ab7e2748a7502f47cb29ecc0b93b6cb07d6b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396994"
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>(VB) SQL Server'da üyelik şeması oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> Bu öğreticide gerekli şema SqlMembershipProvider kullanmak için veritabanına ekleme teknikleri inceleyerek başlar. Şemada tuşu tablolarını inceleyin ve ederiz amaçları ve önem tartışın. Bu öğreticide, üyelik framework kullanması gereken hangi sağlayıcısı bir ASP.NET uygulaması anlatma göz ile sona erer.


## <a name="introduction"></a>Giriş

Web sitenizin ziyaretçileri tanımlamak için form kimlik doğrulaması kullanarak incelenmesi önceki iki öğreticiler. Forms kimlik doğrulaması framework, geliştiricilerin bir kullanıcı bir Web sitesinde oturum açmak ve parolaları kullanarak kimlik doğrulama biletlerini sayfa ziyareti arasında unutmayın kolaylaştırır. `FormsAuthentication` Sınıfı bilet oluşturma ve tanımlama bilgilerini ziyaretçi ekleme için yöntemler içerir. `FormsAuthenticationModule` Gelen tüm istekleri inceler ve bu geçerli bir kimlik doğrulama anahtarı ile oluşturur ve ilişkilendirir bir `GenericPrincipal` ve `FormsIdentity` geçerli istek nesnesi. Form kimlik doğrulaması için de ve sonraki isteklerde kullanıcının kimliğini belirlemek için bu anahtar ayrıştırma, oturum açarken bir ziyaretçi için bir kimlik doğrulaması bileti verme yalnızca bir mekanizmadır. Bir web uygulaması kullanıcı hesapları desteklemek yine de bir kullanıcı deposu uygulamak ve kimlik bilgilerini doğrulamak için yeni kullanıcıların ve diğer kullanıcı hesabı ile ilgili görevlerin çözümlenebilen kaydetme işlevselliği eklemek ihtiyacımız var.

ASP.NET 2.0 önce geliştiricilerin kullanıcı hesabıyla ilgili bu görevlerin tümünü uygulamak için kanca çalışıyor. Neyse ki ASP.NET takımı, bu eksiklikleri tanınan ve üyelik framework ASP.NET 2.0 ile kullanıma sunulmuştur. Üyelik, çekirdek kullanıcı hesabıyla ilgili görevleri yerine getirmeye için bir programlama arabirimi sağlayan sınıflar .NET Framework'teki birtakım çerçevedir. Bu bir framework üzerine inşa edilmiş [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), geliştiriciler standartlaştırılmış bir API özel bir uygulama takın.

Bölümünde açıklandığı gibi <a id="Tutorial1"> </a> [ *temel güvenlik kavramları ve ASP.NET desteği* ](../introduction/security-basics-and-asp-net-support-vb.md) öğretici, .NET Framework iki yerleşik üyelik sağlayıcıları ile birlikte gelir: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) ve [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Adından da anlaşılacağı gibi `SqlMembershipProvider` kullanıcı deposu olarak bir Microsoft SQL Server veritabanı kullanır. Uygulamada bu sağlayıcıyı kullanmak için deposu olarak kullanmak için hangi veritabanı sağlayıcısı bildirmek gerekiyor. Tahmin edebileceğiniz gibi `SqlMembershipProvider` kullanıcı deposu veritabanı, belirli veritabanı tablolarını, görünümlerini ve saklı yordamlar için bekliyor. Bu beklenen bir şema için seçilen veritabanı eklemek ihtiyacımız var.

Bu öğreticide gerekli şema kullanmak için veritabanına ekleme teknikleri inceleyerek başlar `SqlMembershipProvider`. Şemada tuşu tablolarını inceleyin ve ederiz amaçları ve önem tartışın. Bu öğreticide, üyelik framework kullanması gereken hangi sağlayıcısı bir ASP.NET uygulaması anlatma göz ile sona erer.

Haydi başlayalım!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>1. adım: kullanıcı Store yerleştirileceği konuma karar vermede

ASP.NET uygulama verilerine genellikle birkaç tablo bir veritabanında depolanır. Uygularken `SqlMembershipProvider` gerekir karar mi üyelik şeması uygulama verilerinin aynı veritabanında ya da alternatif bir veritabanına yerleştirmek veritabanı şeması.

Ben, aşağıdaki nedenlerden dolayı uygulama verisi olarak aynı veritabanında üyelik şeması bulma önerilir:

- **Bakım** anlamak, Bakım ve iki ayrı veritabanlarına sahip bir uygulama dağıtmak bir uygulama verileri bir veritabanında kapsüllenmiş kolaydır.
- **İlişkisel bütünlüğü** uygulama, tablolar gibi aynı veritabanında üyelik ilgili tabloları bularak kurmak mümkündür [yabancı anahtar kısıtlamalarını](http://en.wikipedia.org/wiki/Foreign_key) birincil anahtarları arasında Üyelik ilgili tabloları ve ilgili uygulama tablolar.

Her ayrı veritabanlarına kullanır ancak genel bir kullanıcı deposu paylaşmanız gerekir birden çok uygulama varsa, kullanıcı verilerini depolama ve uygulama ayrı veritabanlarına ayırma yalnızca mantıklıdır.

### <a name="creating-a-database"></a>Veritabanı oluşturma

Biz bu yana ikinci öğreticide oluşturmakta uygulama bir veritabanı henüz gerek. Bir artık, ancak kullanıcı mağazada ihtiyacımız var. Şimdi oluşturun ve ardından gerekli bir şema ekleyin `SqlMembershipProvider` sağlayıcısı (bkz. 2. adım).

> [!NOTE]
> Bu öğretici serisinin biz kullanacaklardır bir [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) uygulama tablomuz depolamak için veritabanı ve `SqlMembershipProvider` şema. Bu karar, iki nedenden dolayı yapıldı: ilk olarak, maliyetlerini nedeniyle - ücretsiz - Express Edition en readably erişilebilir sürümü, SQL Server 2005;. İkinci olarak, SQL Server 2005 Express Edition veritabanlarını doğrudan web uygulamasının içinde bulunabilecek `App_Data` klasörünü, veritabanı paketlemeyi ve birlikte bir ZIP dosyasına web uygulaması ve tüm özel kurulum yönergeleri yeniden dağıtmak bağlamayı kolaylaştırır veya yapılandırma seçenekleri. SQL Server olmayan - Express Edition sürümü kullanarak birlikte ilerlemek tercih ediyorsanız, büyük/küçük harf çekinmeyin. Adımlar neredeyse aynıdır. `SqlMembershipProvider` Şema herhangi bir Microsoft SQL Server 2000 sürümü ile çalışır ve ayarlama.

Çözüm Gezgini'nden sağ `App_Data` klasörü ve Yeni Öğe Ekle öğesini seçin. (Görmüyorsanız, bir `App_Data` klasör projenizde, Çözüm Gezgini'nde projeye sağ tıklayın, ASP.NET klasörü Ekle seçin ve çekme `App_Data`.) Adlı yeni bir SQL veritabanı eklemek Yeni Öğe Ekle iletişim kutusundan seçin `SecurityTutorials.mdf`. Bu öğreticide ekleyeceğiz `SqlMembershipProvider` şeması bu veritabanında; biz oluşturacaktır ek sonraki öğreticilerde uygulama verilerimizi yakalamak için tablolar.


[![App_Data klasöründe SecurityTutorials.mdf veritabanına adlı yeni bir SQL veritabanı Ekle](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Şekil 1**: yeni bir SQL veritabanı adlı ekleme `SecurityTutorials.mdf` veritabanını `App_Data` klasörü ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Bir veritabanına ekleme `App_Data` klasörü otomatik olarak veritabanı Gezgini görünümü'nde bunu içerir. (Visual Studio'nun olmayan - Express Edition sürümü veritabanı Gezgini Sunucu Gezgini adı verilir). Veritabanı Gezgini'ne gidin ve yeni eklenen genişletin `SecurityTutorials` veritabanı. Ekranda veritabanı Gezgini görmüyorsanız, Görünüm menüsüne gidin ve veritabanı Gezgini seçin veya Ctrl + Alt + S tuşlarına basın. Şekil 2 gösterildiği gibi `SecurityTutorials` veritabanı boş - hiç tablo, Görünüm ve saklı yordam içerir.


[![SecurityTutorials veritabanı şu anda boştur](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Şekil 2**: `SecurityTutorials` veritabanı şu anda boştur ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>2. adım: Ekleme`SqlMembershipProvider`veritabanı şeması

`SqlMembershipProvider` Belirli bir kümesini tablolar, görünümler ve saklı yordamlar kullanıcı deposuna veritabanında yüklü olmasını gerektirir. Bu gerekli veritabanı nesnelerini kullanarak eklenebilir [ `aspnet_regsql.exe` aracı](https://msdn.microsoft.com/library/ms229862.aspx). Bu dosya bulunan `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` klasör.

> [!NOTE]
> `aspnet_regsql.exe` Aracı komut satırı işlevselliği hem grafik kullanıcı arabirimi sunar. Grafik arabirimi daha kullanıcı dostu ve Bu öğreticide ne inceleyeceğiz. Komut satırı arabirimi yararlıdır eklenmesini `SqlMembershipProvider` şema otomatik olarak gerekiyor, gibi yapı komut dosyaları veya otomatik test senaryoları.

`aspnet_regsql.exe` Aracı eklemek veya kaldırmak için kullanılan *ASP.NET uygulama hizmetleri* belirtilen bir SQL Server veritabanı. ASP.NET uygulama hizmetleri için şemalar kapsayacak `SqlMembershipProvider` ve `SqlRoleProvider`, şemaları SQL tabanlı sağlayıcıları diğer ASP.NET 2.0 çerçeveleri için birlikte. İki bit bilgileri sağlamak için ihtiyacımız `aspnet_regsql.exe` aracı:

- Ekleme veya kaldırma uygulama hizmetleri, istediğimiz ve
- Uygulama Hizmetleri şeması ekleyip için veritabanından

Veritabanını kullanmak üzere isteyen içinde `aspnet_regsql.exe` araç veritabanının bulunduğu üzerindeki güvenlik kimlik bilgileri, veritabanına bağlanmak için sunucunun adını ve veritabanı adını sağlamamız sorar. Express Edition'ın SQL Server dışı kullanıyorsanız, bir ASP.NET web sayfası aracılığıyla veritabanı ile çalışırken bir bağlantı dizesi sağlamanız gerekir aynı bilgilerin olduğu gibi bu bilgiler, önceden bilmeniz gerekir. Bir SQL Server 2005 Express Edition veritabanına kullanırken sunucu ve veritabanı adını belirleme `App_Data` klasörü, ancak biraz daha karmaşık.

Aşağıdaki bölümde, bir SQL Server 2005 Express Edition veritabanı sunucusu ve veritabanı adını belirtmek için basit bir yol inceler `App_Data` klasör. SQL Server 2005 Express Edition'ın kullanım için yükleme atlayabilirsiniz ücretsiz kullanmıyorsanız uygulama Hizmetleri bölümü.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Sunucu ve veritabanı adı için bir SQL Server 2005 Express Edition veritabanına belirleme`App_Data`klasörü

Kullanmak için `aspnet_regsql.exe` ihtiyacımız sunucu ve veritabanı adlarını bilme aracı. Sunucu adı `localhost\InstanceName`. Büyük olasılıkla *InstanceName* olduğu `SQLExpress`. Ancak, SQL Server 2005 Express Edition'ın el ile yüklediyseniz (diğer bir deyişle, bu otomatik olarak Visual Studio yüklenirken yüklenmedi), seçtiğiniz başka bir örnek adını mümkündür.

Belirlemek biraz daha veritabanı adıdır. İçindeki veritabanları `App_Data` genellikle klasörünüz içeren bir veritabanı adı bir [genel benzersiz tanıtıcısı](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) birlikte veritabanı dosyasının yolu. Uygulama Hizmetleri şeması aracılığıyla eklemek için bu veritabanı adını belirlemek ihtiyacımız `aspnet_regsql.exe`.

Veritabanı adını belirlemek için en kolay yolu, bu SQL Server Management Studio incelemektir. SQL Server 2005 veritabanlarını yönetmek için SQL Server Management Studio bir grafik arabirim sağlar, ancak Express Edition'ın SQL Server 2005 ile birlikte gelmez. Güzel bir haberimiz var olan [indirebileceğiniz](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) ücretsiz Express Edition'ın SQL Server Management Studio.

> [!NOTE]
> SQL Server 2005 Management Studio'nun tam sürümünü büyük olasılıkla yüklendikten sonra masaüstünüzde yüklü olmayan - Express Edition sürümü de yüklüyse. Tam sürümü, veritabanı adı Express Edition için aşağıda belirtildiği gibi aynı adımları izleyerek belirlemek için kullanabilirsiniz.

Visual Studio'nun veritabanı dosyasını Visual Studio tarafından uygulanan kilitleri kapalı olduğundan emin olun kapatarak başlatın. Ardından, SQL Server Management Studio'yu başlatabilir ve bağlanma `localhost\InstanceName` SQL Server 2005 Express Edition için veritabanı. Daha önce belirtildiği gibi büyük olasılıkla, örnek adı olan `SQLExpress`. Kimlik doğrulama seçeneği için Windows kimlik doğrulamasını seçin.


[![SQL Server 2005 Express Edition örneğine bağlanın](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Şekil 3**: SQL Server 2005 Express Edition örneğine bağlanın ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


SQL Server 2005 Express Edition örneğine bağlandıktan sonra Management Studio klasörleri veritabanları, güvenlik ayarları, sunucu nesneleri ve benzeri için görüntüler. Veritabanları sekmesindeki genişletirseniz göreceksiniz `SecurityTutorials.mdf` veritabanı *değil* veritabanı örneğinde - kayıtlı ilk veritabanı eklemek ihtiyacımız.

Veritabanları klasörü sağ tıklatın ve bağlam menüsünde Ekle'seçeneğini belirleyin. Bu veritabanları ekleme iletişim kutusu görüntüler. Buradan, Ekle düğmesini tıklatın, göz atın `SecurityTutorials.mdf` veritabanı ve Tamam'a tıklayın. Şekil 4'te gösterildiği sonra veritabanları ekleme iletişim kutusu `SecurityTutorials.mdf` veritabanı seçilmedi. Veritabanı başarıyla eklendikten sonra Şekil 5 Management Studio Nesne Gezgini gösterir.


[![SecurityTutorials.mdf veritabanı ekleme](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Şekil 4**: ekleme `SecurityTutorials.mdf` veritabanı ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![SecurityTutorials.mdf veritabanı veritabanları klasöründe görünür.](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Şekil 5**: `SecurityTutorials.mdf` veritabanları klasörünü veritabanı görünür ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Şekil 5 gösterildiği gibi `SecurityTutorials.mdf` veritabanı yerine abstruse bir ada sahip. (Ve daha kolay yazın) daha etkileyici bir şekilde değiştirelim adı. Veritabanına sağ tıklayın, ardından bağlam menüsünden Yeniden Adlandır seçeneğini belirleyin ve yeniden adlandırmak `SecurityTutorialsDatabase`. Bu dosya değiştirmez, yalnızca veritabanı adı SQL Server için kendisini tanımlamak için kullanır.


[![Veritabanı için SecurityTutorialsDatabase yeniden adlandır](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Şekil 6**: veritabanına yeniden adlandır `SecurityTutorialsDatabase`([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


Bu noktada sunucu ve veritabanı adları için biliyoruz `SecurityTutorials.mdf` veritabanı dosyası: `localhost\InstanceName` ve `SecurityTutorialsDatabase`sırasıyla. Uygulama Hizmetleri aracılığıyla yüklemeye hazır sunmaktayız `aspnet_regsql.exe` aracı.

### <a name="installing-the-application-services"></a>Uygulama Hizmetleri Yükleniyor

Başlatmak için `aspnet_regsql.exe` aracı, Başlat menüsüne gidin ve Çalıştır'ı seçin. Girin `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` metin kutusu içine ve Tamam'a tıklayın. Alternatif olarak, uygun klasörü kadar detaya gidin ve çift Windows Gezgini'ni kullanabilirsiniz `aspnet_regsql.exe` dosya. Her iki yöntemle aynı sonuçları net.

Çalışan `aspnet_regsql.exe` aracını herhangi bir komut satırı bağımsız değişkeni olmadan ASP.NET SQL Sunucusu Kurulum Sihirbazı grafik kullanıcı arabirimi başlatır. Sihirbaz, belirtilen bir veritabanı üzerinde ASP.NET uygulama hizmetleri ekleyip kolaylaştırır. Şekil 7'de gösterilen sihirbazının ilk ekranında, aracın amacını açıklar.


[![ASP.NET SQL Server Kurulum Sihirbazı yapar üyelik şeması eklemek için kullanın](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Şekil 7**: ASP.NET SQL Sunucusu Kurulum Sihirbazı yapar üyelik şeması eklemek için kullanın ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


Sihirbazın ikinci adımda bize uygulama hizmetlerini ekleyin veya kaldırın istiyoruz ister. Tablolar, görünümler ve saklı yordamlar için gerekli eklemek istediğimiz beri `SqlMembershipProvider`, yapılandırma SQL Server için uygulama hizmetleri seçeneğini seçin. Daha sonra bu şemayı veritabanından kaldırmak istiyorsanız, bu sihirbazı yeniden çalıştırın, ancak bunun yerine mevcut bir veritabanı seçeneği uygulama hizmetleri bilgileri Kaldır'ı seçin.


[![Seçmek için uygulama hizmetleri seçeneği SQL Server'ı yapılandırma](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Şekil 8**: SQL sunucu yapılandırma için uygulama hizmetleri seçeneği seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


Üçüncü adım veritabanı bilgileri ister: sunucu adı, kimlik doğrulama bilgilerini ve veritabanı adı. Bu eğitimle birlikte aşağıdaki ve eklediğiniz `SecurityTutorials.mdf` veritabanını `App_Data`, bağlı `localhost\InstanceName`ve olarak yeniden adlandırıldı `SecurityTutorialsDatabase`, ardından aşağıdaki değerleri kullanın:

- Sunucu: `localhost\InstanceName`
- Windows kimlik doğrulaması
- Veritabanı: `SecurityTutorialsDatabase`


[![Veritabanı bilgilerini girin](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Şekil 9**: veritabanı bilgilerini girin ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Veritabanı bilgileri girdikten sonra İleri'ye tıklayın. Son adım, gerçekleştirilecek adımlar özetlenmektedir. Uygulama hizmetlerini yükleyin ve ardından Sihirbazı tamamlamak için tamamlamak için İleri'ye tıklayın.

> [!NOTE]
> Veritabanını ve veritabanı dosyasını yeniden adlandırmak için Management Studio kullandıysanız, Visual Studio yeniden açmadan önce Management Studio'yu kapatın ve veritabanını ayırma emin olun. Ayırma `SecurityTutorialsDatabase` veritabanı için veritabanı adını sağ tıklatın ve diğer görevler menüsünden ayırma seçin.

Sihirbaz tamamlandıktan sonra Visual Studio'ya geri dönün ve veritabanı Explorer'a gidin. Tabloları klasörünü genişletin. Bir dizi tablo adları önekiyle başlayan görmelisiniz `aspnet_`. Benzer şekilde, çeşitli görünümler ve saklı yordamlar, görünümler ve saklı yordamlar klasörleri altında bulunabilir. Bu veritabanı nesneleri, uygulama hizmetleri şemanın olun. Adım 3'te üyelik ve rol özel veritabanı nesnelerini inceleyeceğiz.


[![Çeşitli tablolar, görünümler ve saklı yordamlar veritabanına eklenmiş](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Şekil 10**:, A çeşitli tablolar, görünümler ve depolanan yordamları eklenmiştir veritabanına ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Aracı'nın grafik kullanıcı arabirimi, tüm uygulama hizmetleri şeması yükler. Ancak yürütülürken `aspnet_regsql.exe` komut satırından hangi belirli uygulama Hizmetleri bileşenleri yüklemek (veya kaldırmak için) belirtebilirsiniz. Yeni tablolar eklemek istiyorsanız, bu nedenle, görünümler ve saklı yordamlar için gerekli `SqlMembershipProvider` ve `SqlRoleProvider` çalıştırma sağlayıcıları `aspnet_regsql.exe` komut satırından. Alternatif olarak, el ile uygun çalıştırabileceğiniz T-SQL alt kümesi tarafından kullanılan betik Oluştur `aspnet_regsql.exe`. Bu betikler bulunan `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` klasör adları gibi `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`ve benzeri.

Bu noktada gerekli veritabanı nesnelerini oluşturduk `SqlMembershipProvider`. Ancak, yine de kullanması gerektiğini üyeliği framework istemek ihtiyacımız `SqlMembershipProvider` (versus, varsayalım, `ActiveDirectoryMembershipProvider`) ve `SqlMembershipProvider` kullanması gereken `SecurityTutorials` veritabanı. Hangi sağlayıcı belirtin ve adım 4'teki seçili sağlayıcının ayarlarını özelleştirmek şu konuları inceleyeceğiz. Ancak ilk olarak, yalnızca oluşturulan veritabanı nesneleri daha derin bir göz atalım.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>3. adım: Göz şema temel tabloları

Bir ASP.NET uygulamasında üyelik ve roller çerçeveleri ile çalışırken, uygulama ayrıntıları sağlayıcı tarafından kapsüllenir. Gelecekte biz arabirimi aracılığıyla .NET Framework'ün bu çerçevelerle öğreticiler `Membership` ve `Roles` sınıfları. Bu üst düzey API'ler kullanılırken biz kendimize sorguları çalıştırılır veya hangi tablolar değiştiren gibi alt düzey ayrıntıları ile uğraşmak zorunda değildir `SqlMembershipProvider` ve `SqlRoleProvider`.

Bunu göz önünde bulundurulduğunda, güvenle üyelik ve roller çerçeveleri 2. adımda oluşturulan veritabanı şemasını incelediniz olmadan kullanabiliriz. Ancak, uygulama verilerini depolamak için tabloları oluştururken biz kullanıcıların ya da rolleri ilişkili varlıklar oluşturmanız gerekebilir. Bir konusunda yardım ettiği `SqlMembershipProvider` ve `SqlRoleProvider` kısıtlamaları uygulama veri tabloları ve 2. adımda oluşturulan bu tablolar arasında yabancı kurarken şemaları anahtar. Ayrıca, bazı nadir durumlarda kullanıcı ile arabirim oluşturmak ihtiyacımız ve rol depolar doğrudan veritabanı düzeyinde (yerine aracılığıyla `Membership` veya `Roles` sınıfları).

### <a name="partitioning-the-user-store-into-applications"></a>Kullanıcı Store uygulamalarına bölümleme

Üyelik ve roller çerçeveleri, tek bir kullanıcı ve rol deposu, birçok farklı uygulamalar arasında paylaşılabilir şekilde tasarlanmıştır. Üyelik veya rol çerçeveleri kullanan bir ASP.NET uygulama kullanmak için hangi uygulama bölümü belirtmeniz gerekir. Kısacası, birden çok web uygulaması, aynı kullanıcı ve rol depoları kullanabilirsiniz. Şekil 11 üç uygulamalarınızı bölümlenir kullanıcı ve rol depoları gösterilmektedir: HRSite CustomerSite ve SalesSite. Her bu üç web uygulamaları, kendi benzersiz kullanıcılar ve roller sahip, ancak bunların tümü fiziksel olarak kullanıcı hesabı ve rol bilgilerini aynı veritabanı tablolarında depolama.


[![Birden çok uygulamada kullanıcı hesaplarını bölümlendirilebilir](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Şekil 11**: kullanıcı hesapları olabilir olması bölümlenmiş birden çok uygulamaları arasında ([tam boyutlu görüntüyü görmek için tıklatın](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications` Tablodur bu bölümleri tanımlar. Veritabanı kullanıcı hesabı bilgilerini depolamak için kullandığı her bir uygulama, bu tabloda bir satır tarafından temsil edilir. `aspnet_Applications` Tabloda dört sütun vardır: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, ve `Description`.`ApplicationId` tür [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) ve tablonun birincil anahtarı; `ApplicationName` her uygulama için benzersiz bir insan kolay adı sağlar.

Üyelik ve rol ilgili diğer tablolara bağlantı geri `ApplicationId` alanındaki `aspnet_Applications`. Örneğin, `aspnet_Users` her kullanıcı hesabı için bir kayıt içeren bir tablo olan bir `ApplicationId` yabancı anahtar alanı; için ditto `aspnet_Roles` tablo. `ApplicationId` Rolüne ait ya da bu tablolardaki alan kullanıcı hesabı uygulama bölümü belirler.

### <a name="storing-user-account-information"></a>Kullanıcı hesabı bilgileri depolama

Kullanıcı hesabı bilgilerini iki tablo bünyesinde: `aspnet_Users` ve `aspnet_Membership`. `aspnet_Users` Tablo önemli kullanıcı hesabı bilgilerini tutan alanları içerir. Üç en uygun sütunları şunlardır:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` birincil anahtarı (ve tür `uniqueidentifier`). `UserName` tür `nvarchar(256)` ve parola ile birlikte, kullanıcının kimlik bilgilerini sağlar. (Bir kullanıcının parolasını depolanan `aspnet_Membership` tablo.) `ApplicationId` kullanıcı hesabının içinde belirli bir uygulamaya bağlantı `aspnet_Applications`. Bir bileşik yoktur [ `UNIQUE` kısıtlaması](https://msdn.microsoft.com/library/ms191166.aspx) üzerinde `UserName` ve `ApplicationId` sütun. Bu, belirli bir uygulamada her bir kullanıcı adı benzersiz olan, ancak aynı sağlayan sağlar `UserName` farklı uygulamalarda kullanılacak.

`aspnet_Membership` Tablo, kullanıcının parolasını, e-posta adresi, son oturum açma tarihi ve saati ve diğerleri gibi ek kullanıcı hesabı bilgilerini içerir. Kayıtlar arasında bire bir ilişkisi yoktur `aspnet_Users` ve `aspnet_Membership` tablolar. Bu ilişki tarafından güvence altına `UserId` alanındaki `aspnet_Membership`, tablonun birincil anahtarı olarak hizmet verir. Gibi `aspnet_Users` tablo `aspnet_Membership` içeren bir `ApplicationId` belirli uygulama bölümü bu bilgileri bölümlere alan.

### <a name="securing-passwords"></a>Parolaların güvenliğini sağlama

İçinde depolanan parola bilgilerini `aspnet_Membership` tablo. `SqlMembershipProvider` Aşağıdaki üç tekniklerden birini kullanarak veritabanında depolanacak açmak için parolalara izin verir:

- **NET** -parola düz metin olarak veritabanında depolanır. Kesinlikle bu seçeneği kullanarak önerilmemektedir. Veritabanı riske - arka kapı veya veritabanı erişim - bir işlem, kızgın çalışan bulur bir korsan tarafından olması durumunda her tek bir kullanıcının kimlik bilgilerini almak için vardır.
- **Karma** -parolaları tek yönlü karma algoritması ve rastgele üretilen bir salt değer kullanılarak güvenliklidir. Bu karma değer (birlikte salt) veritabanında depolanır.
- **Şifrelenmiş** -parola şifrelenmiş bir sürümü veritabanında depolanır.

Kullanılan parola depolama teknik bağımlı `SqlMembershipProvider` belirtilen ayarları `Web.config`. Biz özelleştirme sırasında görüneceğini `SqlMembershipProvider` adım 4'te ayarlar. Parola Karması depolamak için varsayılan davranıştır.

Parolayı depolamak için sorumlu sütunlar `Password`, `PasswordFormat`, ve `PasswordSalt`. `PasswordFormat` bir alanı `int` değeri parolayı depolamak için kullanılan yöntemi belirtir: Temizle için 0, 1 için Hashed; şifreli için 2. `PasswordSalt` rastgele oluşturulmuş bir dize kullanılan parola depolama teknik bağımsız olarak atanır; değerini `PasswordSalt` yalnızca parola karması ile ilgili işlem yapılırken kullanılır. Son olarak, `Password` sütun gerçek parola verileri içeren, düz metin parola, parola veya şifreli bir parola karması.

Tablo 1, bu üç sütun için çeşitli depolama teknikleri ettiyseniz parola depolarken görünebileceği gösterilmiştir! biçimindeki telefon numarasıdır.

| **Depolama Teknik&lt;\_o3a\_p /&gt;** | **Parola&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Temizle | Ettiyseniz! | 0 | tTnkPlesqissc2y2SMEygA== |
| Karma | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1. | wFgjUfhdUFOCKQiI61vtiQ== |
| Şifreli | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tablo 1**: parola ettiyseniz depolarken parola ile ilgili alanlar için örnek değerler!

> [!NOTE]
> Tarafından kullanılan karma algoritması ve belirli şifreleme `SqlMembershipProvider` ayarlar tarafından belirlenir `<machineKey>` öğesi. Bu yapılandırma öğesi adım 3'te ele almıştık <a id="Tutorial3"> </a> [ *Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) öğretici.

### <a name="storing-roles-and-role-associations"></a>Rolleri ve rol ilişkisi depolama

Rolleri framework, geliştiricilerin rolleri tanımlamak ve hangi kullanıcıların hangi role ait belirtmek olanak tanır. Bu bilgiler, veritabanındaki iki tablo arasında yakalanır: `aspnet_Roles` ve `aspnet_UsersInRoles`. Her kayıtta `aspnet_Roles` tablo, belirli bir uygulama için bir rolü temsil eder. Benzer şekilde `aspnet_Users` tablo `aspnet_Roles` tablo bizim tartışmaya ilgili üç sütun vardır:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` birincil anahtarı (ve tür `uniqueidentifier`). `RoleName` tür `nvarchar(256)`. Ve `ApplicationId` kullanıcı hesabının içinde belirli bir uygulamaya bağlantı `aspnet_Applications`. Bir bileşik yoktur `UNIQUE` kısıtlaması `RoleName` ve `ApplicationId` sütunları, belirli bir uygulamada her rol adının benzersiz olmasını sağlama.

`aspnet_UsersInRoles` Tablo, kullanıcıları ve rolleri arasında bir eşleme görür. Yalnızca iki sütun - `UserId` ve `RoleId` - ve birlikte, bileşik bir birincil anahtar yapabilirsiniz.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>4. adım: sağlayıcı belirtme ve ayarlarını özelleştirme

Sağlayıcı modeli - gibi üyelik ve roller çerçeveleri - destekleyen çerçeveleri uygulama ayrıntılarını kendilerini eksik ve bunun yerine bir sağlayıcı sınıfı bu sorumluluğu temsilci. Üyelik framework söz konusu olduğunda `Membership` sınıfı, kullanıcı hesaplarını yönetmek için API tanımlar, ancak hiçbir kullanıcı deposu ile doğrudan etkileşime girmez. Bunun yerine, `Membership` izin isteği için yapılandırılan sağlayıcı - sınıfın yöntemlerini elde ediyoruz kullanacağınız `SqlMembershipProvider`. Ne zaman biz çağırma metotlarından birini `Membership` sınıfını nasıl üyelik framework bildiğinden temsilci çağrısı `SqlMembershipProvider`?

`Membership` Sınıfında bir [ `Providers` özelliği](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , üyelik framework tarafından kullanılabilmesi için kayıtlı sağlayıcı sınıfların tümü başvuru içeriyor. Her kayıtlı sağlayıcı, bir ilişkili adı ve türüne sahiptir. Ad içinde belirli bir sağlayıcı başvurmak için bir insan dostu sunmaktadır `Providers` sağlayıcı sınıfı tanımlayan sırada koleksiyonu. Ayrıca, her kayıtlı sağlayıcı yapılandırma ayarları içerebilir. Üyelik framework için yapılandırma ayarlarını içerir `PasswordFormat` ve `requiresUniqueEmail`, birçok diğerlerinin yanı sıra. Tarafından kullanılan yapılandırma ayarlarının tam listesi için bkz: Tablo 2 `SqlMembershipProvider`.

`Providers` Özelliğin içeriği, web uygulamasının yapılandırma ayarları'nda belirtilir. Varsayılan olarak, tüm web uygulamalarının adlı bir sağlayıcı sahip `AspNetSqlMembershipProvider` türü `SqlMembershipProvider`. Bu varsayılan üyelik sağlayıcısı kaydedilmiştir `machine.config` (konumundaki `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Yukarıda gösterildiği biçimlendirmesi olarak [ `<membership>` öğesi](https://msdn.microsoft.com/library/1b9hw62f.aspx) üyelik framework için yapılandırma ayarlarını tanımlayan [ `<providers>` alt öğesi](https://msdn.microsoft.com/library/6d4936ht.aspx) kayıtlı belirtir sağlayıcıları. Sağlayıcıları eklenebilir ya da kullanarak kaldırılan [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) veya [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) öğeleri; kullanım [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) öğesi şu anda tüm kaldırmak için Kayıtlı sağlayıcıları. Yukarıda gösterildiği biçimlendirmesi olarak `machine.config` adlı bir sağlayıcı ekler `AspNetSqlMembershipProvider` türü `SqlMembershipProvider`.

Ek olarak `name` ve `type` öznitelikleri `<add>` öğesi değerlerini çeşitli yapılandırma ayarlarını tanımlayan öznitelikleri içerir. Tablo 2 listeler kullanılabilir `SqlMembershipProvider`-açıklamalarıyla birlikte özel yapılandırma ayarları.

> [!NOTE]
> Tüm varsayılan değerlerle Tablo 2'de tanımlanan varsayılan değerleri bakın `SqlMembershipProvider` sınıfı. Bu not Not yapılandırma ayarlarının tümünü `AspNetSqlMembershipProvider` varsayılan değerlerine karşılık gelen `SqlMembershipProvider` sınıfı. Örneğin, bir üyelik sağlayıcısı belirtilmemişse `requiresUniqueEmail` Varsayılanları true olarak ayarlama. Ancak, `AspNetSqlMembershipProvider` açıkça değerini belirterek bu varsayılan değeri geçersiz kılar `false`.

| **Ayarı&lt;\_o3a\_p /&gt;** | **Açıklama&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Birden çok uygulamada bölümlenmesi tek bir kullanıcı deposunun üyelik framework sağlayan geri çağırma. Bu ayar üyelik sağlayıcısı tarafından kullanılan uygulama bölümü adını belirtir. Bu değeri açıkça belirtilmezse, ayarlanır, çalışma zamanında, uygulamanın sanal kök yolu değeri. |
| `commandTimeout` | SQL komutu zaman aşımı değerini (saniye cinsinden) belirtir. Varsayılan değer 30'dur. |
| `connectionStringName` | Bağlantı dizesinin adını `<connectionStrings>` kullanıcı deposu veritabanına bağlanmak için kullanılacak öğe. Bu değer gereklidir. |
| `description` | Kayıtlı sağlayıcı İnsan dostu açıklamasını sağlar. |
| `enablePasswordRetrieval` | Kullanıcıların Unutulan parolalarını alabilir olup olmadığını belirtir. Varsayılan değer `false` şeklindedir. |
| `enablePasswordReset` | Kullanıcıların kendi parolalarını sıfırlamasına izin verilip verilmeyeceğini belirtir. Varsayılan olarak `true`. |
| `maxInvalidPasswordAttempts` | Belirli bir kullanıcı için belirtilen sırasında oluşabilecek başarısız oturum açma denemesi sayısı `passwordAttemptWindow` kullanıcı kilitlenmeden önce. Varsayılan değer 5'tir. |
| `minRequiredNonalphanumericCharacters` | Bir kullanıcının parolasını gereken alfasayısal olmayan karakterlerin en küçük sayısı. Bu değer, 0 ile 128 arasında olmalıdır; Varsayılan değer 1'dir. |
| `minRequiredPasswordLength` | Parolada bulunması gereken karakterlerin en küçük sayısı. Bu değer, 0 ile 128 arasında olmalıdır; varsayılan 7'dir. |
| `name` | Kayıtlı sağlayıcı adı. Bu değer gereklidir. |
| `passwordAttemptWindow` | Hangi sırasında dakika sayısını oturum açma girişimleri izlenen başarısız oldu. Bir kullanıcı geçersiz oturum açma kimlik bilgileri sağlarsa `maxInvalidPasswordAttempts` belirtilen zamanlarda bu pencere, bunlar kilitlenir. Varsayılan değer 10'dur. |
| `PasswordFormat` | Parola depolama biçimi: `Clear`, `Hashed`, veya `Encrypted`. Varsayılan, `Hashed` değeridir. |
| `passwordStrengthRegularExpression` | Sağlanırsa, bu normal ifade gücü yeni hesabı oluştururken seçtiğiniz parolayı kullanıcı veya kullanıcının parolasını değiştirirken değerlendirmek için kullanılır. Varsayılan değer boş bir dizedir. |
| `requiresQuestionAndAnswer` | Bir kullanıcı alma veya parolasını sıfırlama kendi güvenlik sorusunu yanıtlamalıyız olup olmadığını belirtir. Varsayılan değer `true` şeklindedir. |
| `requiresUniqueEmail` | Tüm kullanıcı hesaplarını belirli uygulama bölümüne benzersiz e-posta adresi olması gerekip gerekmediğini gösterir. Varsayılan değer `true` şeklindedir. |
| `type` | Sağlayıcı türünü belirtir. Bu değer gereklidir. |

**Tablo 2**: üyelik ve `SqlMembershipProvider` yapılandırma ayarları

Ek olarak `AspNetSqlMembershipProvider`, diğer üyelik sağlayıcıları benzer işaretlemede ekleyerek bir uygulama tarafından uygulama temelinde kaydedilebilir `Web.config` dosya.

> [!NOTE]
> Rolleri framework çok benzer şekilde çalışır: varsayılan kayıtlı rol sağlayıcısı içinde `machine.config` ve bir uygulama tarafından uygulama temelinde kayıtlı sağlayıcılardan özelleştirilebilir `Web.config`. Bir sonraki öğreticide rolleri framework ve kendi yapılandırma biçimlendirme ayrıntılı inceleyeceğiz.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Özelleştirme`SqlMembershipProvider`ayarları

Varsayılan `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) sahip kendi `connectionStringName` özniteliğini `LocalSqlServer`. Gibi `AspNetSqlMembershipProvider` sağlayıcı, bağlantı dizesi adı `LocalSqlServer` tanımlanan `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Gördüğünüz gibi bu bağlantı dizesini bir SQL 2005 Express Edition'ı veritabanının konumu tanımlar. | DataDirectory|aspnetdb.mdf. Dize | DataDirectory | işaret edecek şekilde çalışma zamanında çevrilir `~/App_Data/` dizin, bu nedenle veritabanı yolu | DataDirectory|aspnetdb.mdf çeviren `~/App_Data` / `aspnet.mdf`.

Biz herhangi bir üyelik sağlayıcısı bilgisi bizim uygulamanın belirtmedi varsa `Web.config` dosya, uygulama kayıtlı varsayılan üyelik sağlayıcısı kullanır `AspNetSqlMembershipProvider`. Varsa `~/App_Data/aspnet.mdf` veritabanı yok, ASP.NET çalışma zamanı otomatik olarak oluşturun ve uygulama hizmetleri şeması ekleyin. Ancak biz kullanmak istemiyorsanız `aspnet.mdf` veritabanı; bunun yerine kullanılacak istiyoruz `SecurityTutorials.mdf` 2. adımda oluşturduğumuz veritabanı. Bu değişikliği iki yoldan biriyle gerçekleştirilebilir:

- <strong>İçin bir değer belirtin</strong><strong>`LocalSqlServer`</strong><strong>bağlantı dizesi adı</strong><strong>`Web.config`</strong><strong>.</strong> Üzerine tarafından `LocalSqlServer` bağlantı dizesi adı değerinin `Web.config`, kayıtlı varsayılan üyelik sağlayıcısı kullanabiliriz (`AspNetSqlMembershipProvider`) ve ile düzgün çalışmak `SecurityTutorials.mdf` veritabanı. Bu tarafından belirtilen yapılandırma ayarlarıyla memnunsanız iyi bir yaklaşımdır `AspNetSqlMembershipProvider`. Bu yöntem hakkında daha fazla bilgi için bkz. [Scott Guthrie](https://weblogs.asp.net/scottgu/)ait blog gönderisi [kullanım SQL Server 2000 veya SQL Server 2005 ASP.NET 2.0 uygulama hizmetlerini yapılandırma](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Yeni bir kayıtlı sağlayıcı türü Ekle</strong><strong>`SqlMembershipProvider`</strong><strong>ve yapılandırma,</strong><strong>`connectionStringName`</strong><strong>içinişaretedecekşekildeayarlama</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>veritabanı.</strong> Bu yaklaşım, veritabanı bağlantı dizesinin yanı sıra diğer yapılandırma özellikleri özelleştirmek için istediğiniz senaryolarda yararlıdır. Kendi projelerinde her zaman bu yaklaşım, esneklik ve okunabilirliği nedeniyle kullanıyorum.

Biz başvuran yeni bir kayıtlı sağlayıcı ekleyebilmeniz için önce `SecurityTutorials.mdf` veritabanı ilk ihtiyacımız bir uygun bir bağlantı dizesi değerindeki eklemek `<connectionStrings>` konusundaki `Web.config`. Adlı yeni bir bağlantı dizesi aşağıdaki biçimlendirme ekler `SecurityTutorialsConnectionString` SQL Server 2005 Express Edition başvuran `SecurityTutorials.mdf` veritabanını `App_Data` klasör.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Bir alternatif veritabanı dosyası kullanıyorsanız, bağlantı dizesi gerektiği gibi güncelleştirin. Doğru bağlantı dizesini oluşturma ile ilgili daha fazla bilgi için [ConnectionStrings.com](http://www.connectionstrings.com/).

Ardından, aşağıdaki üyelik yapılandırma biçimlendirme eklemek `Web.config` dosya. Bu işaretleme adlı yeni bir sağlayıcı kaydeder `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Kaydetme yanı sıra `SecurityTutorialsSqlMembershipProvider` sağlayıcı, yukarıdaki biçimlendirme tanımlar `SecurityTutorialsSqlMembershipProvider` varsayılan sağlayıcı olarak (aracılığıyla `defaultProvider` özniteliğini `<membership>` öğesi). Üyelik framework birden fazla kayıtlı sağlayıcı olduğunu hatırlayın. Bu yana `AspNetSqlMembershipProvider` ilk sağlayıcı olarak kayıtlı `machine.config`, aksi takdirde belirtmek sürece varsayılan sağlayıcı olarak görev yapar.

Şu an uygulamamız iki kayıtlı sağlayıcıları vardır: `AspNetSqlMembershipProvider` ve `SecurityTutorialsSqlMembershipProvider`. Ancak, kaydetmeden önce `SecurityTutorialsSqlMembershipProvider` biz işaretli tüm daha önce sağlayıcısı kayıtlı sağlayıcıları ekleyerek bir [ `<clear />` öğesi](https://msdn.microsoft.com/library/t062y6yc.aspx) hemen önce bizim `<add>` öğesi. Bu temizler `AspNetSqlMembershipProvider` kayıtlı sağlayıcıları listesinden güncelleştirmeyeceği `SecurityTutorialsSqlMembershipProvider` yalnızca kayıtlı üyelik sağlayıcısının olacaktır. Bu yaklaşım kullandık sonra biz işaretlemek ihtiyaç duymaz `SecurityTutorialsSqlMembershipProvider` varsayılan sağlayıcı olarak beri olacağını yalnızca kayıtlı üyelik sağlayıcısı. Kullanma hakkında daha fazla bilgi için `<clear />`, bkz: [kullanma `<clear />` eklediğinizi sağlayıcıları](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Unutmayın `SecurityTutorialsSqlMembershipProvider`'s `connectionStringName` yeni eklenen başvurular ayarını `SecurityTutorialsConnectionString` bağlantı dizesi adı ve kendi `applicationName` ayarı SecurityTutorials değerine ayarlandı. Ayrıca, `requiresUniqueEmail` ayarı sınıflandırmalara ayarlandığı `true`. Diğer tüm yapılandırma seçeneklerinin değerlerle aynıdır `AspNetSqlMembershipProvider`. Burada, tüm yapılandırma değişiklikleri yapmak istiyorsanız çekinmeyin. Örneğin, parola gücü gerektiren bir yerine iki alfasayısal olmayan karakterler ya da parola uzunluğu sekiz karakterden yedi yerine artan sıkılaştıran.

> [!NOTE]
> Birden çok uygulamada bölümlenmesi tek bir kullanıcı deposunun üyelik framework sağlayan geri çağırma. Üyelik sağlayıcısının `applicationName` sağlayıcısı kullanan kullanıcı deposu ile çalışırken, hangi uygulama ayarını gösterir. Açıkça bir değer ayarlamak önemlidir `applicationName` yapılandırma ayarlanması durumunda `applicationName` atanan çalışma zamanında web uygulamasının sanal kök yolu için açıkça ayarlanmış değil. Bu uygulamanın sanal kök yolu değişmez sürece, ancak farklı bir yol uygulamaya taşırsanız düzgün çalışır `applicationName` ayarı çok değiştirir. Bu durumda, üyelik sağlayıcısı daha önce kullanılandan farklı uygulama bölümüyle çalışmaya başlar. Taşımadan önce oluşturulan kullanıcı hesaplarını farklı uygulama bölümünde yer alır ve bu kullanıcıların siteye açmaya devam edebilir. Bu konular üzerinde daha ayrıntılı bir tartışma için bkz: [her zaman `applicationName` özelliği, yapılandırma ASP.NET 2.0 üyeliği ve diğer sağlayıcılar](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Özet

Bu noktada yapılandırılmış uygulama hizmetleri ile bir veritabanı sahibiz (`SecurityTutorials.mdf`) ve üyelik çerçevesi kullanır, böylece web uygulamamıza yapılandırdıysanız `SecurityTutorialsSqlMembershipProvider` yalnızca kayıtlı sağlayıcı. Bu kayıtlı sağlayıcı türüdür `SqlMembershipProvider` ve kendi `connectionStringName` uygun bağlantı dizesine ayarlayın (`SecurityTutorialsConnectionString`) ve onun `applicationName` değeri açıkça ayarlayın.

Şimdi uygulamamız üyelik Framework'ten kullanmak hazırız. Sonraki öğreticide yeni kullanıcı hesapları oluşturma işlemini inceleyeceğiz. Takip ediyoruz kullanıcı tabanlı yetkilendirme gerçekleştirme ve kullanıcı ilgili ek bilgiler veritabanında depolamak, kullanıcıların kimliklerini doğrulama inceleyeceksiniz.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Her zaman `applicationName` özelliği, yapılandırma ASP.NET 2.0 üyeliği ve diğer sağlayıcılar](https://weblogs.asp.net/scottgu/443634)
- [ASP.NET 2.0 yapılandırma uygulaması kullanım SQL Server 2000 veya SQL Server 2005'te Hizmetleri](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [SQL Server Management Studio Express Edition'ı indirin](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [ASP.NET 2.0 İnceleme s üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Sağlayıcıları için üyelik öğesi](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` Öğesi](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` Üyelik öğesi](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Kullanarak `<clear />` eklediğinizi sağlayıcıları](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Doğrudan ile çalışma `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda eğitim videosu

- [ASP.NET Üyeliklerini Anlama](../../../videos/authentication/understanding-aspnet-memberships.md)
- [SQL’yi Üyelik Şemalarıyla Çalışacak Biçimde Yapılandırma](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Varsayılan Üyelik Şemasında Üyelik Ayarlarını Değiştirme](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET Books yazar ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan  *[Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott, konumunda ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Alicja Maziarz oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](storing-additional-user-information-cs.md)
> [İleri](creating-user-accounts-vb.md)
