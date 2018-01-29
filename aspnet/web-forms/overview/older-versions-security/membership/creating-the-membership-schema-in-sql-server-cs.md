---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: "SQL Server (C#) üyelik şema oluşturma | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide gerekli şema SqlMembershipProvider kullanmak için veritabanına eklemek için teknikleri inceleyerek başlar. Aşağıdaki biz wi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 38fc60b79a348ab198069a9a80a085e0dc4bcb88
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>SQL Server (C#) üyelik şema oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Bu öğreticide gerekli şema SqlMembershipProvider kullanmak için veritabanına eklemek için teknikleri inceleyerek başlar. Biz şema anahtar tablolarda inceleyin ve amaçları ve önem tartışın. Bu öğretici üyelik framework kullanması gereken hangi sağlayıcı bir ASP.NET uygulaması anlatma göz ile biter.


## <a name="introduction"></a>Giriş

Web sitenizin ziyaretçileri tanımlamak için form kimlik doğrulaması kullanılarak incelenmesi önceki iki öğreticileri. Forms kimlik doğrulaması framework geliştiriciler bir kullanıcının bir Web sitesinde oturum açmak ve onları sayfa ziyareti kimlik doğrulama biletlerini kullanımıyla arasında anımsaması kolay hale getirir. `FormsAuthentication` Sınıfı, ziyaretçi tanımlama bilgilerini ekleme ve bileti üretmek için yöntemler içerir. `FormsAuthenticationModule` Gelen tüm istekleri inceler ve bu geçerli bir kimlik doğrulama bileti ile oluşturur ve ilişkilendirir bir `GenericPrincipal` ve `FormsIdentity` geçerli istek nesnesi. Form kimlik doğrulaması için de ve sonraki isteklerde kullanıcının kimliğini belirlemek için bu anahtar ayrıştırma oturum açarken bir ziyaretçi için bir kimlik doğrulaması bileti verme yalnızca bir mekanizmadır. Bir web uygulaması kullanıcı hesaplarını desteklemek hala bir kullanıcı deposunun uygulamak ve kimlik bilgilerini doğrulamak için yeni kullanıcılar ve diğer kullanıcı hesabı ile ilgili görevleri sayıda kayıt işlevsellik eklemek ihtiyacımız.

ASP.NET 2.0 önce tüm bu kullanıcı hesabıyla ilgili uygulamak için kanca geliştiriciler bulunduğunuz. Neyse ki ASP.NET takım bu eksiklikleri tanınan ve üyelik framework ASP.NET 2.0 ile sunulan. Üyelik framework, .NET Framework'teki çekirdek kullanıcı hesabıyla ilgili görevleri gerçekleştirmeye programa dayalı bir arabirim sağlayan sınıflar kümesidir. Bu framework üzerinde oluşturulan [sağlayıcı modeli](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), geliştiricilerin özelleştirilmiş bir uygulama standartlaştırılmış bir API takın izin verir.

' Da anlatıldığı gibi <a id="Tutorial1"> </a> [ *güvenlik temel kavramları ve ASP.NET Destek* ](../introduction/security-basics-and-asp-net-support-cs.md) öğretici, .NET Framework iki yerleşik üyelik sağlayıcıları ile birlikte gelir: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) ve [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Adından da anlaşılacağı gibi `SqlMembershipProvider` bir Microsoft SQL Server veritabanı kullanıcı deposu olarak kullanır. Bu sağlayıcı bir uygulamada kullanabilmek için biz deposu olarak kullanmak için hangi veritabanı sağlayıcısı bildirmeniz gerekir. Tahmin edebileceğiniz gibi `SqlMembershipProvider` kullanıcı deposu veritabanı belirli veritabanı tabloları, görünümleri ve saklı yordamlar olmasını bekler. Bu beklenen şema seçili veritabanına eklemek gerekir.

Bu öğreticide gerekli şema kullanmak için veritabanına eklemek için teknikleri inceleyerek başlar `SqlMembershipProvider`. Biz şema anahtar tablolarda inceleyin ve amaçları ve önem tartışın. Bu öğretici üyelik framework kullanması gereken hangi sağlayıcı bir ASP.NET uygulaması anlatma göz ile biter.

Haydi başlayalım!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>1. adım: kullanıcı deposunda nereye yerleştireceğinizi karar verme

Bir ASP.NET uygulama verilerini bir veritabanında tablolarda, çeşitli yaygın olarak depolanır. Uygularken `SqlMembershipProvider` biz gerekir karar mi üyelik şemanın uygulama verileri olarak aynı veritabanında veya alternatif bir veritabanına yerleştirmek veritabanı şeması.

I üyelik şemanın uygulama verileri olarak aynı veritabanında aşağıdaki nedenlerle bulma önerilir:

- **Bakım** anlamak, korumanıza ve iki ayrı veritabanları olan bir uygulamayı dağıtmak verileri bir veritabanında kapsüllenmiş uygulama kolaydır.
- **İlişkisel bütünlüğü** uygulama, tablolar olarak aynı veritabanında üyelik ilişkili tabloları bularak oluşturmak mümkündür [yabancı anahtar kısıtlamaları](http://en.wikipedia.org/wiki/Foreign_key) birincil anahtarlar arasındaki Üyelik ilişkili tabloları ve ilişkili uygulama tabloları.

Her ayrı veritabanlarını kullanır ancak genel bir kullanıcı deposu paylaşmaya gereksinim birden çok uygulamalarınız varsa ayrı veritabanlarına kullanıcı deposu ve uygulama verilerini kesilmesi yalnızca anlamlıdır.

### <a name="creating-a-database"></a>Veritabanı oluşturma

Biz bu yana ikinci öğretici oluşturmakta uygulama bir veritabanı henüz gerek. Bir artık, ancak kullanıcı deposu için ihtiyacımız var. Şimdi bir tane oluşturun ve ardından gerekli şema ekleyin `SqlMembershipProvider` sağlayıcısı (2. adım bakın).

> [!NOTE]
> Bu öğretici seri biz kullanacağınız bir [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) bizim uygulama tabloları depolamak için veritabanı ve `SqlMembershipProvider` şema. Bu karara iki nedenden dolayı yapıldı: ilk olarak, kendi maliyet nedeniyle - boş - Express Edition en readably erişilebilir sürümü, SQL Server 2005;. İkinci olarak, SQL Server 2005 Express Edition veritabanları doğrudan web uygulamasının içinde yerleştirilebilir `App_Data` klasörü, veritabanının paketini ve birlikte bir ZIP dosyasında web uygulaması ve tüm özel kurulum yönergeleri yeniden dağıtmak için bağlamayı yapma veya yapılandırma seçenekleri. SQL Server olmayan - Express Edition sürümü kullanılarak takip tercih ediyorsanız, büyük/küçük harf çekinmeyin. Adımları neredeyse aynıdır. `SqlMembershipProvider` Şema herhangi bir Microsoft SQL Server 2000 sürümü ile çalışma ve yukarı.


Çözüm Gezgini'nden sağ `App_Data` klasörü ve Yeni Öğe Ekle'i seçin. (Görmüyorsanız, bir `App_Data` , projenizin klasöründe Çözüm Gezgini'nde projeye sağ tıklayın, ASP.NET klasörü Ekle seçin ve çekme `App_Data`.) Adlı yeni bir SQL veritabanı eklemek Yeni Öğe Ekle iletişim kutusundan seçin `SecurityTutorials.mdf`. Bu öğreticide ekleyeceğiz `SqlMembershipProvider` şeması bu veritabanında; biz oluşturacak ek sonraki öğreticilerde bizim uygulama verilerini yakalamak için tablo.


[![App_Data klasöründe SecurityTutorials.mdf veritabanına adlı yeni bir SQL veritabanı Ekle](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Şekil 1**: yeni bir SQL veritabanı adlı eklemek `SecurityTutorials.mdf` için veritabanı `App_Data` klasörü ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Bir veritabanına ekleme `App_Data` klasörü otomatik olarak veritabanı Gezgini görünümünde bunu içerir. (Visual Studio olmayan - Express Edition sürümü, veritabanı Gezgin Sunucu Gezgini olarak adlandırılır.) Veritabanı Gezgini'ne gidin ve yeni eklenen genişletin `SecurityTutorials` veritabanı. Ekranda Database Explorer görmüyorsanız Görünüm menüsüne gidin ve veritabanı Explorer'ı seçin veya Ctrl + Alt + S isabet. Şekil 2'de görüldüğü gibi `SecurityTutorials` veritabanı boşsa - hiçbir tablo, Görünüm ve hiçbir saklı yordamları içerir.


[![SecurityTutorials veritabanı şu anda boştur](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Şekil 2**: `SecurityTutorials` veritabanı şu anda boştur ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>2. adım: Ekleme`SqlMembershipProvider`veritabanı şeması

`SqlMembershipProvider` Belirli kümesi tabloları, görünümleri ve saklı yordamlar kullanıcı deposu veritabanında yüklü olmasını gerektirir. Bu gerekli veritabanı nesnelerini kullanılarak eklenebilir [ `aspnet_regsql.exe` aracı](https://msdn.microsoft.com/library/ms229862.aspx). Bu dosya bulunan `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` klasör.

> [!NOTE]
> `aspnet_regsql.exe` Aracı komut satırı işlevselliği ve grafik kullanıcı arabirimi sunar. Grafik arabirim daha kullanıcı dostu ve Bu öğreticide ne inceleyeceğiz. Komut satırı arabirimini yararlıdır eklenmesi `SqlMembershipProvider` şema otomatik olarak gerekiyor, yapı olduğu gibi komut dosyaları veya test senaryolarını otomatik.


`aspnet_regsql.exe` Aracı eklemek veya kaldırmak için kullanılan *ASP.NET uygulama hizmetleri* belirtilen SQL Server veritabanı. ASP.NET uygulama hizmetleri şemaları kapsayan `SqlMembershipProvider` ve `SqlRoleProvider`, şemaları SQL tabanlı sağlayıcıları diğer ASP.NET 2.0 çerçeveler için birlikte. İki BITS bilgileri sağlamak ihtiyacımız `aspnet_regsql.exe` aracı:

- Eklemek veya uygulama hizmetlerini kaldırmak istiyoruz ve
- Uygulama Hizmetleri şeması ekleyip için veritabanından

Veritabanını kullanacak şekilde isteyen içinde `aspnet_regsql.exe` aracı veritabanının bulunduğu, veritabanına bağlanmak için güvenlik kimlik bilgileri sunucusunun adını ve veritabanı adını sağlamamız sorar. Express sürümü, SQL Server dışı kullanıyorsanız, bir ASP.NET web sayfası aracılığıyla veritabanı ile çalışırken, bir bağlantı dizesini sağlamalısınız aynı bilgilerin olduğu gibi bu bilgileri zaten bilmeniz gerekir. Sunucu ve veritabanı adı bir SQL Server 2005 Express Edition veritabanında kullanırken belirleme `App_Data` klasörü, ancak, biraz daha karmaşık.

Aşağıdaki bölümde bir SQL Server 2005 Express Edition veritabanı sunucusu ve veritabanı adı belirtmek için basit bir yol inceler `App_Data` klasör. SQL Server 2005 Express Edition kullanımında yükleme İleri atlayabilirsiniz ücretsiz kullanmıyorsanız uygulama Hizmetleri bölümü.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Bir SQL Server 2005 Express Edition veritabanı için veritabanı adı ve sunucu belirleme`App_Data`klasörü

Kullanmak için `aspnet_regsql.exe` aracı biz sunucusunu ve veritabanı adları bilmeniz gerekir. Sunucu adı `localhost\InstanceName`. Büyük olasılıkla *InstanceName* olan `SQLExpress`. Ancak, SQL Server 2005 Express Edition'ı el ile yüklü değilse (diğer bir deyişle, onu otomatik olarak Visual Studio yüklenirken yüklemedi), farklı bir örnek adı seçili mümkündür.

Veritabanı adı belirlemek biraz daha değil. İçinde veritabanları `App_Data` genellikle klasörünüz içeren bir veritabanı adı bir [genel benzersiz tanımlayıcı](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) birlikte veritabanı dosyasının yolu. Uygulama Hizmetleri şeması aracılığıyla eklemek için bu veritabanının adını belirlemek ihtiyacımız `aspnet_regsql.exe`.

Veritabanı adı olmadığından emin olmak için en kolay yolu, bu SQL Server Management Studio incelemektir. SQL Server 2005 veritabanlarını yönetmek için SQL Server Management Studio'da bir grafik arabirim sağlar, ancak Express sürümü, SQL Server 2005 ile birlikte gelmez. İyi haber olan [indirebilirsiniz](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) ücretsiz Express sürümü, SQL Server Management Studio.

> [!NOTE]
> SQL Server 2005'in masaüstünüzde yüklü olmayan - Express Edition sürümü de yüklüyse Management Studio'nın tam sürümünü olasılıkla yüklenir. Tam sürümü, veritabanı adı, Express Edition için aşağıda özetlendiği gibi aynı adımları izleyerek belirlemek için kullanabilirsiniz.


Veritabanı dosyasını Visual Studio tarafından uygulanan kilitleri kapalı olduğundan emin olmak için Visual Studio kapatarak başlatın. Ardından, SQL Server Management Studio'yu başlatın ve bağlanmak `localhost\InstanceName` veritabanı SQL Server 2005 Express Edition için. Daha önce belirtildiği gibi büyük olasılıkla örnek adı olan `SQLExpress`. Windows kimlik doğrulaması için kimlik doğrulaması seçeneği seçin.


[![SQL Server 2005 Express sürüm bağlanın](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Şekil 3**: SQL Server 2005 Express Edition örneğine bağlanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


SQL Server 2005 Express Edition örneğine bağlandıktan sonra Management Studio klasörleri veritabanları, güvenlik ayarlarını, sunucu nesneleri ve benzeri için görüntüler. Veritabanları sekmesini genişletirseniz göreceksiniz `SecurityTutorials.mdf` veritabanı *değil* veritabanı örneğinde - kayıtlı biz veritabanı ilk eklemeniz gerekir.

Veritabanları klasörü sağ tıklatın ve bağlam menüsünden Ekle'yi seçin. Bu veritabanları ekleme iletişim kutusu görüntüler. Buradan, Ekle düğmesini tıklatın, Gözat `SecurityTutorials.mdf` veritabanı ve Tamam'ı tıklatın. Şekil 4 sonra veritabanları ekleme iletişim kutusu gösterir `SecurityTutorials.mdf` veritabanı seçilmiş. Veritabanı başarıyla eklendi sonra Şekil 5 Management Studio'nun Object Explorer gösterir.


[![SecurityTutorials.mdf veritabanı ekleme](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Şekil 4**: Attach `SecurityTutorials.mdf` veritabanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![Veritabanları klasörünü SecurityTutorials.mdf veritabanı görünür](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Şekil 5**: `SecurityTutorials.mdf` veritabanı görünür veritabanları klasöründe ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Şekil 5 gösterildiği gibi `SecurityTutorials.mdf` veritabanı yerine abstruse bir ada sahip. Daha etkileyici (ve daha kolay yazın) değiştirelim adı. Veritabanını sağ tıklatın, yeniden adlandırma bağlam menüsünden seçin ve yeniden adlandırmak `SecurityTutorialsDatabase`. Bu dosya adını değiştirmez, yalnızca veritabanı adını kendisini SQL Server'a tanımlamak için kullanır.


[![SecurityTutorialsDatabase için veritabanını yeniden adlandırın](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Şekil 6**: için veritabanını yeniden adlandırın `SecurityTutorialsDatabase`([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


Bu noktada için sunucu ve veritabanı adları biliyoruz `SecurityTutorials.mdf` veritabanı dosyası: `localhost\InstanceName` ve `SecurityTutorialsDatabase`sırasıyla. Biz şimdi aracılığıyla uygulama hizmetlerini yüklemek için hazır `aspnet_regsql.exe` aracı.

### <a name="installing-the-application-services"></a>Uygulama Hizmetleri Yükleniyor

Başlatmak için `aspnet_regsql.exe` aracı, Başlat menüsüne gidin ve Çalıştır'ı seçin. Girin `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` textbox içine ve Tamam'ı tıklatın. Alternatif olarak, çift tıklayın ve aşağıya doğru uygun klasör incelemek için Windows Gezgini'ni kullanabilirsiniz `aspnet_regsql.exe` dosya. Her iki yaklaşım aynı sonuçları net.

Çalışan `aspnet_regsql.exe` aracını tüm komut satırı bağımsız değişkenleri olmadan ASP.NET SQL Server Kurulum Sihirbazı grafik kullanıcı arabirimi başlatır. Sihirbazın ASP.NET uygulama Hizmetleri belirtilen veritabanında ekleyip daha kolay hale getirir. Şekil 7'de gösterilen Sihirbazı ilk ekran aracının amacı açıklar.


[![ASP.NET SQL Server Kurulum Sihirbazı'nı yapar üyelik şema eklemek için kullanın](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Şekil 7**: ASP.NET SQL Sunucusu Kurulum Sihirbazı'nı yapar üyelik şema eklemek için kullanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


Sihirbazın ikinci adımda bize biz uygulama hizmetlerini eklemek veya bunları kaldırmak isteyip istemediğinizi sorar. Biz tabloları, görünümleri ve saklı yordamlar için gerekli eklemek istediğiniz beri `SqlMembershipProvider`, uygulama hizmetleri seçeneği için yapılandırma SQL Server'ı seçin. Daha sonra bu şemayı veritabanından kaldırmak istiyorsanız, bu sihirbazı yeniden çalıştırın, ancak bunun yerine var olan bir veritabanı seçeneğinden Kaldır uygulama hizmetleri bilgilerini seçin.


[![Seçin uygulama hizmetleri seçeneği için SQL Server yapılandırma](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Şekil 8**: uygulama hizmetleri seçeneği için SQL Server'ı Yapılandır'ı seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


Üçüncü adım veritabanı bilgileri ister: sunucu adını, kimlik doğrulama bilgilerini ve veritabanı adı. İle birlikte bu öğreticiyi izlemek ve eklemiş olduğunuz `SecurityTutorials.mdf` için veritabanı `App_Data`, ekli `localhost\InstanceName`ve ona yeniden adlandırılmış `SecurityTutorialsDatabase`, aşağıdaki değerleri kullanın:

- Server: `localhost\InstanceName`
- Windows kimlik doğrulaması
- Veritabanı:`SecurityTutorialsDatabase`


[![Veritabanı bilgilerini girin](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Şekil 9**: veritabanı bilgilerini girin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Veritabanı bilgileri girdikten sonra İleri'yi tıklatın. Son adım gerçekleştirilecek adımları özetlemektedir. Uygulama hizmetlerini yükleyin ve Sihirbazı tamamlamak için son için İleri'yi tıklatın.

> [!NOTE]
> Veritabanını ve veritabanı dosyasını yeniden adlandırmak için Management Studio kullandıysanız, veritabanını ayırma ve Visual Studio yeniden açmadan önce Management Studio'yu kapatın emin olun. Kullanımdan çıkarmak için `SecurityTutorialsDatabase` veritabanı için veritabanı adını sağ tıklatın ve diğer görevler menüsünden ayırma.


Tamamlandıktan sonra sihirbazın, Visual Studio'ya geri dönün ve veritabanı Gezgini'ne gidin. Tables klasörünü genişletin. Bir dizi adları önek ile başlayan tablo görmelisiniz `aspnet_`. Benzer şekilde, görünümler ve saklı yordamlar, çeşitli görünümleri ve saklı yordamlar klasörleri altında bulunabilir. Bu veritabanı nesnelerini uygulama hizmetleri şemanın olun. Adım 3'te üyelik ve rol özel veritabanı nesnelerini inceleyeceğiz.


[![Tabloları, görünümleri ve saklı yordamlar, çeşitli veritabanına eklenmiş.](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Şekil 10**: in A çeşitli tabloları, görünümleri ve saklı yordamları eklenmiştir veritabanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe` Aracın grafik kullanıcı arabirimi tüm uygulama hizmetleri şeması yükler. Ancak yürütülürken `aspnet_regsql.exe` komut satırından hangi belirli uygulama Hizmetleri bileşenleri yüklemek (veya kaldırmak için) belirtebilirsiniz. Yalnızca tabloları eklemek istiyorsanız, bu nedenle, görünümler ve saklı yordamlar için gerekli `SqlMembershipProvider` ve `SqlRoleProvider` çalıştırmak sağlayıcıları `aspnet_regsql.exe` komut satırından. Alternatif olarak, el ile uygun çalıştırabilirsiniz T-SQL kısmı betikler tarafından kullanılan oluşturmak `aspnet_regsql.exe`. Bu komut dosyalarını bulunan `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` klasörü gibi adlarla `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`ve benzeri.


Bu noktada gerekli veritabanı nesnelerini oluşturduk `SqlMembershipProvider`. Ancak, yine kullanması gerektiğini üyelik framework istemek üzere ihtiyacımız `SqlMembershipProvider` (karşı söyleyin, `ActiveDirectoryMembershipProvider`) ve `SqlMembershipProvider` kullanması gereken `SecurityTutorials` veritabanı. Hangi sağlayıcı belirtme ve adım 4'te seçili Sağlayıcısı'nın ayarlarını özelleştirmek nasıl inceleyeceğiz. Ancak önce yeni oluşturduğunuz veritabanı nesnelerini daha derin bir göz atalım.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>3. adım: Göz şema çekirdek tabloları

Bir ASP.NET uygulamasında üyelik ve roller çerçeveleri ile çalışırken, uygulama ayrıntılarını sağlayıcı tarafından kapsüllenir. Gelecekte biz arabirim .NET Framework'ün aracılığıyla bu çerçeveleri ile öğreticileri `Membership` ve `Roles` sınıfları. Bu üst düzey API'leri kullanırken biz kendisini sorguları yürütülen veya hangi tabloları değiştiren gibi alt düzey ayrıntıların ile ilgili gerekmez `SqlMembershipProvider` ve `SqlRoleProvider`.

Bu verildiğinde, 2. adımda oluşturulan veritabanı şeması incelediniz olmadan biz üyelik ve roller çerçeveleri güvenle kullanabilirsiniz. Ancak, uygulama verilerini depolamak için tabloları oluştururken, biz kullanıcıları veya rolleriyle ilgili varlıklar oluşturmanız gerekebilir. Bir alışkanlığına sahip yardımcı `SqlMembershipProvider` ve `SqlRoleProvider` yabancı kurarken şemaları anahtar kısıtlamalarını uygulama veri tabloları ve 2. adımda oluşturulan bu tablolar arasında. Ayrıca, bazı nadir durumlarda biz ile kullanıcı arabirimi gerekebilir ve rol depolar doğrudan veritabanı düzeyinde (aracılığıyla yerine `Membership` veya `Roles` sınıfları).

### <a name="partitioning-the-user-store-into-applications"></a>Kullanıcı deposunda uygulamalara bölümlendirme

Üyelik ve roller çerçeveleri, tek bir kullanıcı ve rol deposu birçok farklı uygulamalar arasında paylaşılabilir şekilde tasarlanmıştır. Üyelik veya rol çerçeveleri kullanan ASP.NET uygulaması kullanmak için hangi uygulama bölümü belirtmeniz gerekir. Kısacası, birden çok web uygulamaları, aynı kullanıcı ve rol depolarını kullanabilirsiniz. Şekil 11 üç uygulamalara bölümlenir kullanıcı ve rol depolarını gösterilmektedir: HRSite, CustomerSite ve SalesSite. Her bu üç web uygulamaları, kendi benzersiz kullanıcılar ve roller sahip, ancak tüm fiziksel olarak kullanıcı hesabı ve rol bilgilerini aynı veritabanı tablolarında depoladıkları.


[![Kullanıcı hesapları arasında birden çok uygulama bölümlendirilebilir](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Şekil 11**: kullanıcı hesapları olabilir olması bölümlenmiş birden çok uygulamaları arasında ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


`aspnet_Applications` Tablodur ne bu bölümleri tanımlar. Kullanıcı hesabı bilgilerini depolamak için veritabanını kullanan her bir uygulama bu tablodaki satır ile temsil edilir. `aspnet_Applications` Tablo dört sütun vardır: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, ve `Description`. `ApplicationId`tür [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) ve tablonun birincil anahtarı; `ApplicationName` her uygulama için benzersiz bir insan kolay ad sağlar.

Üyelik ve rol ilgili diğer tablolarla bağlantı geri `ApplicationId` alanındaki `aspnet_Applications`. Örneğin, `aspnet_Users` her kullanıcı hesabı için bir kayıt içerir, tablosunda bir `ApplicationId` yabancı anahtar alanı; ditto için `aspnet_Roles` tablo. `ApplicationId` Rol ait veya bu tablolar alanında uygulama bölümü kullanıcı hesabı belirtir.

### <a name="storing-user-account-information"></a>Kullanıcı hesabı bilgilerini depolama

Kullanıcı hesabı bilgilerini iki tablo yerleştirilebilir: `aspnet_Users` ve `aspnet_Membership`. `aspnet_Users` Tablosu temel kullanıcı hesabı bilgilerini tutmak alanları içerir. Üç en uygun sütun şunlardır:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId`birincil anahtar (ve türü `uniqueidentifier`). `UserName`tür `nvarchar(256)` ve parola ile birlikte, kullanıcının kimlik bilgilerini sağlar. (Bir kullanıcının parolasını depolanan `aspnet_Membership` tablo.) `ApplicationId` kullanıcı hesabının belirli bir uygulamada bağlantılar `aspnet_Applications`. Bileşik yoktur [ `UNIQUE` kısıtlaması](https://msdn.microsoft.com/library/ms191166.aspx) üzerinde `UserName` ve `ApplicationId` sütun. Bu, belirli bir uygulamada her kullanıcı adı benzersiz olduğundan, ancak aynı sağlar sağlar `UserName` farklı uygulamalarda kullanılacak.

`aspnet_Membership` Tablosu kullanıcının parolasını, e-posta adresi, son oturum açma tarihi ve saati ve diğerleri gibi ek kullanıcı hesabı bilgilerini içerir. Kayıtları arasında bire bir ilişkisi yok `aspnet_Users` ve `aspnet_Membership` tabloları. Bu ilişki tarafından güvence altına `UserId` alanındaki `aspnet_Membership`, tablonun birincil anahtarı olarak görev yapar. Gibi `aspnet_Users` tablo `aspnet_Membership` içeren bir `ApplicationId` bağlar, bu bilgileri belirli bir uygulama bölümünün alan.

### <a name="securing-passwords"></a>Parolaların güvenliğini sağlama

Parola bilgilerini depolanır `aspnet_Membership` tablo. `SqlMembershipProvider` Aşağıdaki üç teknikleri birini kullanarak veritabanında depolanan parolaları için izin verir:

- **Clear** -parola düz metin olarak veritabanında depolanır. Kesinlikle bu seçeneği kullanarak önerilmemektedir. Veritabanı riske - arka kapı veya veritabanı erişimi - bir işlem, kızgın çalışanı bulur bir korsan tarafından olması durumunda her tek bir kullanıcının kimlik bilgilerini almak için vardır.
- **Karma** -parolaların karma tek yönlü karma algoritması ve rastgele oluşturulmuş bir salt değer kullanılarak. Bu karma değer (birlikte salt) veritabanında depolanır.
- **Şifrelenmiş** -parola şifreli sürümünü veritabanında depolanır.

Kullanılan parola depolama teknik bağlıdır `SqlMembershipProvider` belirtilen ayarları `Web.config`. Biz özelleştirme adresindeki görüneceğini `SqlMembershipProvider` adım 4'te ayarlar. Parola karmasını depolamak için varsayılan davranıştır.

Parola depolamak için sorumlu sütunlar `Password`, `PasswordFormat`, ve `PasswordSalt`. `PasswordFormat`türünde bir alan olduğundan `int` değeri parola depolamak için kullanılan yöntemi belirtir: Temizle için 0, 1 için Hashed; şifreli için 2. `PasswordSalt`kullanılan parola depolama teknik bağımsız olarak rastgele oluşturulmuş bir dize atanır; değeri `PasswordSalt` yalnızca parola karmasını hesaplanırken kullanılır. Son olarak, `Password` gerçek parola veri sütunu içeriyor, düz metin parola, parola veya şifrelenmiş parola karmasını olabilir.

Tablo 1 ne bu üç sütun gibi çeşitli depolama teknikler için parola ettiyseniz depolarken görünebileceği gösterilmiştir! biçimindeki telefon numarasıdır.

| **Depolama Teknik&lt;\_o3a\_p /&gt;** | **Parola&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Temizle | Ettiyseniz! | 0 | tTnkPlesqissc2y2SMEygA== |
| Karma | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1. | wFgjUfhdUFOCKQiI61vtiQ== |
| Şifrelenmiş | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tablo 1**: parola ettiyseniz depolarken parola ilişkili alanlar için örnek değerler!

> [!NOTE]
> Tarafından kullanılan karma algoritması ve belirli şifreleme `SqlMembershipProvider` ayarlarında belirlenir `<machineKey>` öğesi. Biz bu yapılandırma öğesi adım 3'te açıklanan <a id="Tutorial3"> </a> [ *Forms kimlik doğrulaması yapılandırması ve Gelişmiş konular* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) Öğreticisi.


### <a name="storing-roles-and-role-associations"></a>Rolleri ve rol ilişkilendirmeleri depolanması

Rolleri framework roller kümesini tanımlamak ve hangi kullanıcıların hangi rollere ait belirlemek geliştiricilerin sağlar. Bu bilgiler iki tablo aracılığıyla veritabanında yakalanan: `aspnet_Roles` ve `aspnet_UsersInRoles`. Her bir kayıtta `aspnet_Roles` tablo belirli bir uygulama için bir rolü temsil eder. Çok benzer şekilde `aspnet_Users` tablo `aspnet_Roles` tabloda bizim tartışmaya ilgili üç sütun bulunur:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId`birincil anahtar (ve türü `uniqueidentifier`). `RoleName`tür `nvarchar(256)`. Ve `ApplicationId` kullanıcı hesabının belirli bir uygulamada bağlantılar `aspnet_Applications`. Bileşik yoktur `UNIQUE` kısıtlaması `RoleName` ve `ApplicationId` sütunları, her bir rol adı verilen bir uygulamada benzersiz olduğundan emin olmanın.

`aspnet_UsersInRoles` Kullanıcıları ve rolleri arasında bir eşleme tablosu görür. Yalnızca iki sütun - `UserId` ve `RoleId` - ve birleşik birincil anahtar birlikte yaptıkları.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>4. adım: sağlayıcı belirtme ve ayarlarını özelleştirme

Tüm - gibi üyelik ve roller çerçeveleri - sağlayıcı modelini destekleyen çerçeveleri uygulama ayrıntılarını kendilerini olmaması ve bunun yerine bir sağlayıcı sınıfı bu sorumluluğu atayabilirsiniz. Üyelik framework durumunda `Membership` sınıfı, kullanıcı hesaplarını yönetme için API tanımlar, ancak hiçbir kullanıcı deposu ile doğrudan etkileşime girmez. Bunun yerine, `Membership` sınıfının yöntemleri elle yapılandırılmış sağlayıcı - isteğine kapalı biz kullanacağınız `SqlMembershipProvider`. Ne zaman biz çağırma yöntemlerden birini `Membership` sınıfı, nasıl işlevini üyelik framework biliyor çağrısı temsilciye `SqlMembershipProvider`?

`Membership` Sınıfına sahip bir [ `Providers` özelliği](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) üyelik çerçevesi tarafından tüm kayıtlı sağlayıcısı sınıfları kullanılabilir bir başvuru içeriyor. Her kayıtlı sağlayıcı bir ilişkili ad ve türe sahip. Ad içinde belirli bir sağlayıcı başvurmak için İnsan kolay bir yol sunar `Providers` sağlayıcı sınıfı tanımlayan sırada koleksiyonu. Ayrıca, her kayıtlı sağlayıcı yapılandırma ayarlarını içerebilir. Üyelik framework için yapılandırma ayarlarını içeren `passwordFormat` ve `requiresUniqueEmail`, diğer birçok arasında. Tablo 2 tarafından kullanılan yapılandırma ayarlarının tam listesi için bkz: `SqlMembershipProvider`.

`Providers` Özelliğin içeriği, web uygulamasının yapılandırma ayarları'nda belirtilir. Varsayılan olarak, tüm web uygulamalarının adlı bir sağlayıcı sahip `AspNetSqlMembershipProvider` türü `SqlMembershipProvider`. Bu varsayılan üyelik sağlayıcısı kayıtlı `machine.config` (konumunda bulunan `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Gösterir, yukarıda biçimlendirmesi olarak [ `<membership>` öğesi](https://msdn.microsoft.com/library/1b9hw62f.aspx) sırasında üyelik framework için yapılandırma ayarlarını tanımlayan [ `<providers>` alt öğesi](https://msdn.microsoft.com/library/6d4936ht.aspx) kayıtlı belirtir sağlayıcıları. Sağlayıcıları eklenebilir veya kullanarak kaldırılan [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) veya [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) öğeleri; kullanım [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) öğesinin tüm şu anda kaldırmak için Kayıtlı sağlayıcıları. Gösterir, yukarıda biçimlendirmesi olarak `machine.config` adlı bir sağlayıcı ekler `AspNetSqlMembershipProvider` türü `SqlMembershipProvider`.

Ek olarak `name` ve `type` öznitelikleri `<add>` öğesi çeşitli yapılandırma ayarları için değerleri tanımlayan öznitelikleri içerir. Tablo 2 listeler kullanılabilir `SqlMembershipProvider`-açıklamalarıyla birlikte özel yapılandırma ayarları.

> [!NOTE]
> Tablo 2'de not ettiğiniz herhangi bir varsayılan değeri, tanımlanan varsayılan değerleri başvurmak `SqlMembershipProvider` sınıfı. Bu not Not tüm yapılandırma ayarlarını `AspNetSqlMembershipProvider` varsayılan değerlerine karşılık gelen `SqlMembershipProvider` sınıfı. Örneğin, bir üyelik sağlayıcısı belirtilmezse `requiresUniqueEmail` varsayılan ayarı true olarak. Ancak, `AspNetSqlMembershipProvider` açıkça değerini belirterek bu varsayılan değerini geçersiz kılar `false`.


| **Ayarı&lt;\_o3a\_p /&gt;** | **Açıklama&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Birden çok uygulama arasında bölümlenmesi tek bir kullanıcı deposunun üyelik framework verir geri çağırma. Bu ayar üyelik sağlayıcısı tarafından kullanılan uygulama bölümünün adını belirtir. Bu değer açıkça belirtilmezse, onu ayarlandığında, çalışma zamanında, uygulamanın sanal kök yolu değeri. |
| `commandTimeout` | SQL komutu zaman aşımı değerini (saniye cinsinden) belirtir. Varsayılan değer 30'dur. |
| `connectionStringName` | Bağlantı dizesinin adını `<connectionStrings>` kullanıcı deposu veritabanına bağlanmak için kullanılacak öğe. Bu değer gereklidir. |
| `description` | Kayıtlı sağlayıcı İnsan kolay açıklamasını sağlar. |
| `enablePasswordRetrieval` | Kullanıcılar kendi Unutulan parolayı alabilir olup olmadığını belirtir. Varsayılan değer `false` şeklindedir. |
| `enablePasswordReset` | Kullanıcıların parolalarını sıfırlamasına izin verilip verilmeyeceğini belirtir. Varsayılan olarak `true`. |
| `maxInvalidPasswordAttempts` | Belirli bir kullanıcı için belirtilen sırasında ortaya çıkabilecek başarısız oturum açma denemesi sayısı `passwordAttemptWindow` kullanıcı kilitlenmeden önce. Varsayılan değer 5'tir. |
| `minRequiredNonalphanumericCharacters` | Bir kullanıcının parolasını görünmesi gereken alfasayısal olmayan karakterlerin en küçük sayısı. Bu değer, 0 ile 128 arasında olmalıdır; Varsayılan değer 1'dir. |
| `minRequiredPasswordLength` | Parolada kullanılması gereken karakter minimum sayısı. Bu değer, 0 ile 128 arasında olmalıdır; Varsayılan değer 7 ' dir. |
| `name` | Kayıtlı sağlayıcısının adı. Bu değer gereklidir. |
| `passwordAttemptWindow` | Hangi sırasında dakika sayısını denemeleri izlenen oturum açma başarısız oldu. Bir kullanıcı geçersiz oturum açma kimlik bilgileri sağlarsa `maxInvalidPasswordAttempts` süreleri bu içinde belirtilen pencere, bunlar kilitlidir. Varsayılan değer 10'dur. |
| `PasswordFormat` | Parola depolama biçimi: `Clear`, `Hashed`, veya `Encrypted`. Varsayılan, `Hashed` değeridir. |
| `passwordStrengthRegularExpression` | Sağlanırsa, bu normal ifade parolasını değiştirirken veya yeni bir hesap oluştururken, kullanıcının seçili parola gücünü değerlendirmek için kullanılır. Varsayılan değer boş bir dizedir. |
| `requiresQuestionAndAnswer` | Bir kullanıcı alma veya parolasını sıfırlama kendi güvenlik sorusunu yanıtlamanız gerekir olup olmadığını belirtir. Varsayılan değer `true` şeklindedir. |
| `requiresUniqueEmail` | Tüm kullanıcı hesaplarını belirli uygulama bölümüne benzersiz e-posta adresi olması gerekip gerekmediğini gösterir. Varsayılan değer `true` şeklindedir. |
| `type` | Sağlayıcı türünü belirtir. Bu değer gereklidir. |

**Tablo 2**: üyeliği ve `SqlMembershipProvider` yapılandırma ayarları

Ek olarak `AspNetSqlMembershipProvider`, diğer üyelik sağlayıcıları benzer biçimlendirme ekleyerek bir uygulama tarafından uygulama temelinde kaydedilebilir `Web.config` dosya.

> [!NOTE]
> Rolleri framework çok aynı şekilde çalışır: varsayılan kayıtlı rol sağlayıcı yok `machine.config` ve uygulamalar tarafından düzenli olarak içinde kayıtlı sağlayıcıları özelleştirilebilir `Web.config`. Sonraki öğreticide rolleri framework ve kendi yapılandırma biçimlendirme ayrıntılı inceleyeceğiz.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Özelleştirme`SqlMembershipProvider`ayarları

Varsayılan `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) sahip kendi `connectionStringName` özniteliği kümesine `LocalSqlServer`. Gibi `AspNetSqlMembershipProvider` sağlayıcı, bağlantı dizesi adı `LocalSqlServer` tanımlanan `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Gördüğünüz gibi bu bağlantı dizesi veritabanı bulunan bir SQL 2005 Express sürüm tanımlar. | DataDirectory|aspnetdb.mdf. Dize | DataDirectory | işaret etmek için çalışma zamanında çevrilen `~/App_Data/` dizin, böylece veritabanı yolu | DataDirectory|aspnetdb.mdf"çevirir için `~/App_Data` / `aspnet.mdf`.

Biz herhangi bir üyelik sağlayıcısı bilgisi bizim uygulamanın içinde belirtmedi varsa `Web.config` dosya, uygulamayı kullanan kayıtlı varsayılan üyelik sağlayıcısı `AspNetSqlMembershipProvider`. Varsa `~/App_Data/aspnet.mdf` veritabanı yok, ASP.NET çalışma zamanı otomatik olarak oluşturun ve uygulama hizmetleri şema ekleyin. Ancak, biz kullanmak istemiyorsanız `aspnet.mdf` ; bunun yerine, biz kullanmak istediğiniz veritabanı `SecurityTutorials.mdf` 2. adımda oluşturduğumuz veritabanı. Bu değişikliği iki yoldan biriyle gerçekleştirilebilir:

- **İçin bir değer belirtin ***`LocalSqlServer`*** bağlantı dizesi adı ***`Web.config`***.** Üzerine tarafından `LocalSqlServer` bağlantı dizesi adı değerindeki `Web.config`, kayıtlı varsayılan üyelik sağlayıcısı kullanıyoruz (`AspNetSqlMembershipProvider`) ve düzgün çalışması `SecurityTutorials.mdf` veritabanı. Bu yaklaşım tarafından belirtilen yapılandırma ayarlarıyla memnunsanız uygundur `AspNetSqlMembershipProvider`. Bu teknik hakkında daha fazla bilgi için bkz: [Scott Guthrie](https://weblogs.asp.net/scottgu/)'s blog gönderisi [ASP.NET 2.0 uygulama hizmetlerini yapılandırma kullanım SQL Server 2000 veya SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- **Yeni kayıtlı sağlayıcısının türü *** eklemek`SqlMembershipProvider`*** ve yapılandırma, ***`connectionStringName`*** işaret edecek şekilde ayarı ***`SecurityTutorials.mdf`*** veritabanı.** Bu yaklaşım, veritabanı bağlantı dizesi yanı sıra diğer yapılandırma özellikleri özelleştirmek istediğiniz senaryolarda kullanışlıdır. Kendi projelerinde ı her zaman bu yaklaşımı, esneklik ve okunabilirliği nedeniyle kullanın.

Başvuran yeni bir kayıtlı sağlayıcı ekleyebilmeniz için önce `SecurityTutorials.mdf` veritabanı, ilk ihtiyacımız bir uygun bir bağlantı dizesi değerindeki eklemek `<connectionStrings>` bölümüne `Web.config`. Aşağıdaki biçimlendirmede adlı yeni bir bağlantı dizesi ekler `SecurityTutorialsConnectionString` SQL Server 2005 Express Edition başvuran `SecurityTutorials.mdf` veritabanını `App_Data` klasör.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Bir alternatif veritabanı dosyası kullanıyorsanız, bağlantı dizesi gerektiği gibi güncelleştirin. Doğru bağlantı dizesi oluşturma hakkında daha fazla bilgi için bkz [ConnectionStrings.com](http://www.connectionstrings.com/).

Ardından, aşağıdaki üyelik yapılandırma biçimlendirme eklemek `Web.config` dosya. Bu biçimlendirme adlı yeni bir sağlayıcı kaydeder `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Kaydetme yanı sıra `SecurityTutorialsSqlMembershipProvider` sağlayıcı, yukarıdaki biçimlendirme tanımlar `SecurityTutorialsSqlMembershipProvider` varsayılan sağlayıcı olarak (aracılığıyla `defaultProvider` özniteliğini `<membership>` öğesi). Geri çağırma üyelik framework birden çok kayıtlı sağlayıcıları olabilir. Bu yana `AspNetSqlMembershipProvider` ilk sağlayıcı olarak kayıtlı `machine.config`, aksi takdirde belirtmek sürece varsayılan sağlayıcı olarak görev yapar.

Şu anda, uygulamamız iki kayıtlı sağlayıcıları yok: `AspNetSqlMembershipProvider` ve `SecurityTutorialsSqlMembershipProvider`. Ancak, kaydetmeden önce `SecurityTutorialsSqlMembershipProvider` biz temizlenmiş tüm daha önce sağlayıcısı kayıtlı sağlayıcıları ekleyerek bir [ `<clear />` öğesi](https://msdn.microsoft.com/library/t062y6yc.aspx) hemen önce bizim `<add>` öğesi. Bu temizler `AspNetSqlMembershipProvider` kayıtlı sağlayıcıları listesinden, yani `SecurityTutorialsSqlMembershipProvider` yalnızca kayıtlı üyelik sağlayıcısı olacaktır. Bu yaklaşım kullandık sonra biz işaretlemek ihtiyaç duymaz `SecurityTutorialsSqlMembershipProvider` varsayılan sağlayıcı olarak beri bu olacaktır yalnızca kayıtlı üyelik sağlayıcısı. Kullanma hakkında daha fazla bilgi için `<clear />`, bkz: [kullanma `<clear />` zaman ekleme sağlayıcıları](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Unutmayın `SecurityTutorialsSqlMembershipProvider`'s `connectionStringName` yeni eklenen başvuruları ayarı `SecurityTutorialsConnectionString` bağlantı dizesi adı ve kendi `applicationName` ayarı SecurityTutorials bir değere ayarlandı. Ayrıca, `requiresUniqueEmail` ayarı ayarlanmış `true`. Diğer tüm yapılandırma seçenekleri değerler aynı `AspNetSqlMembershipProvider`. İsterseniz, burada, yapılandırma değişiklikleri yapmak çekinmeyin. Örneğin, parola gücünü bir yerine iki alfasayısal olmayan karakter gerektirerek ya da parola uzunluğu sekiz karakter yedi yerine artırarak sıkılaştırabilirsiniz.

> [!NOTE]
> Birden çok uygulama arasında bölümlenmesi tek bir kullanıcı deposunun üyelik framework verir geri çağırma. Üyelik sağlayıcısının `applicationName` sağlayıcı kullanan kullanıcı deposu ile çalışırken, hangi uygulama ayarını gösterir. İçin bir değer açıkça ayarlamak önemlidir `applicationName` çünkü yapılandırma ayarı, `applicationName` atanan çalışma zamanında web uygulamasının sanal kök yolu için açıkça ayarlanmış değil. Bu uygulamanın sanal kök yolu değiştirmez sürece, ancak farklı bir yol uygulamaya taşırsanız düzgün çalışır `applicationName` ayarı çok değiştirir. Bu durumda, üyelik sağlayıcısı daha önce kullanılandan farklı uygulama bölümü ile çalışmaya başlar. Taşımadan önce oluşturulan kullanıcı hesapları farklı uygulama bölümünde yer alacağını ve bu kullanıcıların siteye oturum açmaya devam edebilir. Bu konular hakkında daha ayrıntılı bir tartışma için bkz: [her zaman `applicationName` özelliği, yapılandırma ASP.NET 2.0 üyeliği ve diğer sağlayıcılar](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Özet

Bu noktada bir veritabanı yapılandırılmış uygulama hizmetleriyle sahibiz (`SecurityTutorials.mdf`) ve üyelik çerçevesi kullanır, böylece web uygulamamız yapılandırdıysanız `SecurityTutorialsSqlMembershipProvider` sağlayıcı biz yalnızca kayıtlı. Bu kayıtlı sağlayıcı türüdür `SqlMembershipProvider` ve kendi `connectionStringName` uygun bağlantı dizesine ayarlayın (`SecurityTutorialsConnectionString`) ve onun `applicationName` değeri açıkça ayarlanmış.

Biz şimdi uygulamamız üyelik çerçevesinden kullanıma hazır. Sonraki öğreticide yeni kullanıcı hesapları oluşturma inceleyeceğiz. Biz kullanıcı tabanlı yetkilendirme gerçekleştirme ve kullanıcı ilgili ek bilgiler veritabanında depolayarak kullanıcıların kimlik doğrulaması inceleyeceksiniz aşağıdaki.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Her zaman `applicationName` ASP.NET 2.0 yapılandırırken özelliği üyeliği ve diğer sağlayıcılar](https://weblogs.asp.net/scottgu/443634)
- [ASP.NET 2.0 yapılandırma uygulama kullanım SQL Server 2000 veya SQL Server 2005 Hizmetleri](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [SQL Server Management Studio Express Edition'ı karşıdan yükle](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [ASP.NET 2.0 inceleniyor s üyelik, roller ve profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>` Üyelik sağlayıcıları için öğesi](https://msdn.microsoft.com/library/whae3t94.aspx)
- [`<membership>` Öğesi](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [`<providers>` Üyelik için öğesi](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Kullanarak `<clear />` sağlayıcıları'ne zaman ekleme](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Doğrudan ile çalışma`SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda video eğitim

- [ASP.NET Üyeliklerini Anlama](../../../videos/authentication/understanding-aspnet-memberships.md)
- [SQL’yi Üyelik Şemalarıyla Çalışacak Biçimde Yapılandırma](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Varsayılan Üyelik Şemasında Üyelik Ayarlarını Değiştirme](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Yazar hakkında

Scott Mitchell, birden çok ASP/ASP.NET books yazar ve 4GuysFromRolla.com, kurucusu 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri  *[kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Tan adresindeki ulaşılabilir [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Alicja Maziarz oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Next](creating-user-accounts-cs.md)
