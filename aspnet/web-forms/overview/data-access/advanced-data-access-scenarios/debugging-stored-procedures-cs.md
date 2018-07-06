---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Saklı yordamları (C#) hata ayıklama | Microsoft Docs
author: rick-anderson
description: Visual Studio Professional ve takım sistemi sürümleri, kesme noktaları ayarlayın ve SQL Server saklı yordamlar için adım depolanan hata ayıklama yapmadan izin ver...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: e1eb9f6ea7d1704061157cca6898ad8330780929
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824218"
---
<a name="debugging-stored-procedures-c"></a>Hata ayıklama saklı yordamları (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) veya [PDF olarak indirin](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Visual Studio Professional ve takım sistemi sürümleri, kesme noktaları ayarlayın ve SQL Server saklı yordamlar için adım uygulama kodu ayıklanması kadar kolay saklı yordamların hatalarını ayıklama yapmadan olanak sağlar. Bu öğretici, doğrudan veritabanı hata ayıklamaya ve uygulama saklı yordamların hatalarını ayıklama gösterir.


## <a name="introduction"></a>Giriş

Visual Studio, zengin bir hata ayıklama deneyimi sunar. Birkaç tuş vuruşlarını veya fare tıklamaları, s bir programın yürütülmesini durdurmak ve onun durumunu ve denetim akışı incelemek için kesme noktaları kullanma olanağı. Visual Studio, uygulama kodu hata ayıklama yanı sıra, SQL Server saklı yordamlardan hata ayıklama desteği sunar. Bir ASP.NET arka plan kod sınıfı veya iş mantığı katmanı sınıfı kod içinde kesme noktaları sadece ayarlanabilir gibi için saklı yordamları içinden bunlar çok yerleştirilebilir.

Bu öğreticide gelindiğinde kesme noktaları ayarlamak için ne zaman saklı yordamı çalışan ASP.NET uygulamasından çağrılır nasıl saklı yordamlara sunucu Gezgini'nde Visual Studio içinden de Adımlama görünecektir.

> [!NOTE]
> Ne yazık ki, saklı yordamlar yalnızca içine girdiğiniz ve Visual Studio Professional ve takım sistemleri sürümleri hata ayıklama. Visual Web Developer veya Visual Studio standart sürümünü kullanıyorsanız, biz saklı yordamları debug gereken adımlarda size yol, ancak bu adımları makinenizde çoğaltmak mümkün olmayacaktır boyunca okuma davetlidir.


## <a name="sql-server-debugging-concepts"></a>SQL Server hata ayıklama kavramları

Microsoft SQL Server 2005 ile tümleştirme sağlamak üzere tasarlanmış [ortak dil çalışma zamanı (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), tüm .NET derlemeleri tarafından kullanılan çalışma zamanı olduğu. Sonuç olarak, SQL Server 2005 yönetilen veritabanı nesnelerini destekler. Diğer bir deyişle, C# sınıfı yöntemleri olarak veritabanı nesneleri gibi saklı yordamlar ve kullanıcı tanımlı işlevlerle (UDF) oluşturabilirsiniz. Bu saklı yordamlar ve UDF'ler .NET Framework ve kendi özel sınıflar işlevselliğinden yararlanmasına olanak tanır. Elbette, SQL Server 2005 T-SQL veritabanı nesneleri için de destek sağlar.

SQL Server 2005 T-SQL hem de yönetilen veritabanı nesneleri için hata ayıklama desteği sunar. Ancak, bu nesneler yalnızca Visual Studio 2005 Professional ve takım sistemleri sürümleri ayıklanabilir. Bu öğreticide hata ayıklama T-SQL veritabanı nesnelerini inceleyeceğiz. Sonraki öğreticide, yönetilen bir veritabanı nesnelerinde hata ayıklama sırasında arar.

[Genel bakış, T-SQL ve CLR hata ayıklama SQL Server 2005'te](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) blog girişi [SQL Server 2005 CLR tümleştirme takım](https://blogs.msdn.com/sqlclr/default.aspx) SQL Server 2005 nesneleri Visual Studio'dan hata ayıklama için üç yol vurgular:

- **Doğrudan veritabanı hata ayıklama (DDD)** - biz adım saklı yordamlar ve UDF'ler gibi tüm T-SQL veritabanı nesnesine Sunucu Gezgini'nden. Adım 1'deki DDD inceleyeceğiz.
- **Uygulama hata ayıklama** -biz bir veritabanı nesnesi içinde kesme noktaları ayarlayın ve ardından ASP.NET uygulamamız çalıştırın. Veritabanı nesnesi yürütüldüğünde, kesme noktasına isabet ve denetimi hata ayıklayıcıyı açık. Uygulamanın hatalarını ayıklamaya biz bir veritabanı nesnesine ait uygulama kodu girilemiyor olduğunu unutmayın. Biz açıkça kesme noktaları bu saklı yordamları ya da UDF'ler durdurmak için hata ayıklayıcı istediğimiz ayarlamanız gerekir. Uygulama hata ayıklama, 2. adımda başlangıç incelenir.
- **Bir SQL Server projeden hata ayıklama** -Visual Studio Professional ve takım sistemleri sürümleri yönetilen nesneleri oluşturmak için yaygın olarak kullanılan bir SQL Server Proje türü içerir. SQL Server projeleri kullanarak inceleyeceğiz ve bunların içeriğini sonraki öğreticide hata ayıklama.

Visual Studio, yerel ve uzak SQL Server örneklerinde saklı yordamlar ayıklayabilirsiniz. Yerel bir SQL Server örneği, Visual Studio ile aynı makinede yüklü bir uygulamadır. Kullanmakta olduğunuz SQL Server veritabanı geliştirme makinenizde yer almıyorsa, uzak bir örneği değerlendirilir. Bu öğreticiler için yerel SQL Server örneklerini kullandığımız. Uzak bir SQL server örneği üzerinde saklı yordamların hatalarını ayıklama, yerel bir örneğinde saklı yordamların hatalarını ayıklama değerinden daha fazla yapılandırma adımı gerektirir.

Yerel bir SQL Server örneği kullanıyorsanız 1. adımla başlamak ve Bu öğreticide sonuna çalışır. Ancak, uzak bir SQL Server örneği kullanıyorsanız, hata ayıklama sırasında emin olmak için ilk gerek geliştirme makinenize uzak örneğinde bir SQL Server oturum bilgilerine sahip bir Windows kullanıcı hesabı ile oturum olacaktır. Bu veritabanı oturum açma hem çalışan ASP.NET uygulamasından veritabanına bağlanmak için kullanılan veritabanı oturum açma Moveover alanının üyesi olmalıdır `sysadmin` rol. Hata ayıklama T-SQL veritabanı nesneleri, uzak bir örneğini hata ayıklamak için Visual Studio ve SQL Server yapılandırma hakkında daha fazla bilgi için bu öğreticinin sonunda uzak örnekler bölümüne bakın.

Son olarak, hata ayıklama desteği T-SQL veritabanı nesneleri için hata ayıklama desteği için .NET uygulamaları olarak zengin bir özellik olarak olduğunu anlayın. Hata ayıklama windows kümesini yalnızca kullanılabilir, örneğin, kesme noktası koşulları ve filtreleri, desteklenmez, Düzenle ve devam et kullanamazsınız, gereksiz vb. hemen penceresinde oluşturulur. Bkz: [hata ayıklayıcı komutları ve özellikleri üzerinde sınırlamalar](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) daha fazla bilgi için.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>1. adım: Doğrudan bir saklı yordam Adımlama

Visual Studio, doğrudan bir veritabanı nesnesi hatalarını ayıklamanızı kolaylaştırır. S içine adımlamak için doğrudan veritabanı hata ayıklama (DDD) özelliğini kullanmak nasıl bakmak istiyorum `Products_SelectByCategoryID` Northwind veritabanında saklı yordamı. Adından da anlaşılacağı gibi `Products_SelectByCategoryID` döndürür; belirli bir kategorideki ürün bilgileri oluşturulduğu [kullanarak mevcut saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) öğretici. Sunucu Gezginine giderek başlatın ve Northwind veritabanı düğümünü genişletin. Ardından, saklı yordamlar klasörüne detaya, sağ `Products_SelectByCategoryID` saklı yordam ve bağlam menüsünden adım içinde saklı yordamı seçeneği belirleyin. Bu, hata ayıklayıcıyı başlatır.

Bu yana `Products_SelectByCategoryID` saklı yordam bekliyor bir `@CategoryID` giriş parametresi, biz istenir bu değeri belirtin. 1, İçecekler hakkında bilgi döndürür girin.


![' % S'değeri 1 için kullanmak @CategoryID parametresi](debugging-stored-procedures-cs/_static/image1.png)

**Şekil 1**: ' % s'değeri 1 için kullanmak `@CategoryID` parametresi


Değeri sağlama sonra `@CategoryID` saklı yordam parametresi yürütülür. Tamamlanana kadar çalıştırmak yerine, ancak, hata ayıklayıcı yürütme ilk deyimindeki durdurur. Saklı yordam geçerli konumu gösteren sarı ok kenar unutmayın. Görüntüleyebilir ve İzleme penceresi yoluyla veya saklı yordam içinde parametre adının üzerine geldiğinizde parametre değerlerini düzenleyin.


[![Hata ayıklayıcı saklı yordam üzerinde ilk deyimi durdu](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Şekil 2**: hata ayıklayıcı saklı yordam üzerinde ilk deyimi işlemi durduruldu ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image4.png))


Aynı anda saklı yordamı bir deyim üzerinden adımlamak için araç çubuğundaki Step Over düğmesine tıklayın veya F10 tuşuna basın. `Products_SelectByCategoryID` Saklı yordam içeren tek bir `SELECT` F10 ulaşmaktan tek deyimi üzerinden adımla şekilde deyimi ve saklı yordam yürütme tamamlayın. Saklı yordam tamamlandıktan sonra çıktısını çıkış penceresinde görüntülenir ve hata ayıklayıcı sonlandırılır.

> [!NOTE]
> T-SQL hata ayıklama deyimi düzeyinde gerçekleşir; Adımlama edilemez bir `SELECT` deyimi.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>2. adım: uygulama hata ayıklama için Web sitesi yapılandırma

Bir saklı yordamı Sunucu Gezgini'nden doğrudan hata ayıklama kullanışlı olsa da, birçok senaryoda bizim ASP.NET uygulamasından çağrıldığında saklı yordamı hata ayıklamaya daha ilgi duyuyoruz. Visual Studio içinden bir saklı yordamı kesme noktaları eklemek ve ardından ASP.NET uygulama hata ayıklama başlatın. Uygulamadan kesme noktaları olan bir saklı yordam çağrıldığında, yürütme kesme noktasında durdurulur ve biz görüntüleyebilir ve saklı yordam s parametre değerlerini değiştirmek ve adım 1'de yaptığımız gibi kendi deyimleri adım.

Biz uygulamadan çağrılan saklı yordamların hatalarını ayıklama başlamadan önce SQL Server hata ayıklayıcısı ile tümleştirmek için ASP.NET web uygulaması isteyin gerekir. Başlangıç Web sitesi adı Çözüm Gezgini'nde sağ tıklayarak (`ASPNET_Data_Tutorial_74_CS`). Bağlam menüsünden özellik sayfaları seçeneğini seçin, sol taraftaki başlangıç seçenekleri öğeyi seçin ve hata ayıklayıcılar bölümündeki SQL Server ile ilgili onay kutusunu işaretleyin (bkz: Şekil 3).


[![Uygulama s özellik sayfaları SQL Server onay kutusunu işaretleyin](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Şekil 3**: özellik sayfaları uygulama s SQL Server onay kutusunu işaretleyin ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image7.png))


Ayrıca, uygulama tarafından kullanılan bağlantı havuzunu devre dışı veritabanı bağlantı dizesini güncellemek ihtiyacımız var. Bir veritabanına bir bağlantı kapatıldığında, karşılık gelen `SqlConnection` nesne kullanılabilir bağlantılar bir havuzda yerleştirilir. Bir veritabanıyla bağlantı kurarken bir kullanılabilir bağlantı nesnesi bu havuzdan alınabilir yerine oluşturun ve yeni bir bağlantı kurmak zorunda. Bu bağlantı nesneleri havuzu bir performans geliştirmedir ve varsayılan olarak etkindir. Ancak, hata ayıklama sırasında hata ayıklama altyapısı doğru havuzdan alındığı bir bağlantıyla çalışırken kurulmaz değil çünkü bağlantı havuzu kapalı etkinleştirmek istiyoruz.

Devre dışı bırakılmış bağlantı havuzu için güncelleştirme `NORTHWNDConnectionString` içinde `Web.config` ayarı içerir böylece `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> İşlemi tamamladıktan sonra SQL Server ASP.NET uygulaması hata ayıklama kaldırarak bağlantı havuzu yeniden devreye sokma emin `Pooling` alınan bağlantı dizesi ayarlama (veya ayarlayarak `Pooling=true` ).


Bu noktada ASP.NET uygulaması, web uygulaması aracılığıyla çağrıldığında SQL Server veritabanı nesnelerini hata ayıklamak için Visual Studio'yu izin verecek şekilde yapılandırıldı. Artık kalan tüm olduğu için bir saklı yordam bir kesme noktası ekleyin ve hata ayıklamaya başlayın!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>3. adım: bir kesme noktası ekleme ve hata ayıklama

Açık `Products_SelectByCategoryID` başlangıcında bir kesme noktası ayarlayın ve saklı yordamı `SELECT` deyimi uygun bir yerdeki kenar tıklayarak ya da başlangıcında imleci yerleştirerek `SELECT` deyim veya F9 tuşuna basarak. Şekil 4'te gösterildiği gibi kesme noktası kenar kırmızı bir daire olarak görünür.


[![Products_SelectByCategoryID içinde bir kesme noktası ayarlamak depolanan yordamı](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Şekil 4**: bir kesim noktası `Products_SelectByCategoryID` saklı yordam ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image10.png))


Bir SQL veritabanı nesnesi bir istemci uygulaması hata ayıklaması için sırada veritabanı uygulamasında hata ayıklama desteklemek için yapılandırılması zorunludur. Önce bir kesme noktası ayarlarsanız, bu ayar otomatik olarak açık, ancak sağlayamazsanız akıllıca olur. Sağ `NORTHWND.MDF` Sunucu Gezgininde. Bağlam menüsü öğesi uygulamasında hata ayıklama adlı bir işaretli menü içermelidir.


![Uygulama hata ayıklama seçeneğinin etkin olduğundan emin olun](debugging-stored-procedures-cs/_static/image11.png)

**Şekil 5**: uygulama hata ayıklama seçeneğinin etkin olduğundan emin olun


Kesme noktası ayarlama ve uygulama hata ayıklama seçeneğinin etkin ile ASP.NET uygulamasından çağrıldığında saklı yordamı hata ayıklamak hazırız. Hata ayıklama menüsüne giderek hata ayıklayıcıyı başlatın ve F5'e basarak veya yeşil tıklayarak hata ayıklamayı Başlat, seçme, araç çubuğunda simge yürütebilirsiniz. Bu, hata ayıklayıcıyı başlatın ve Web sitesini başlatın.

`Products_SelectByCategoryID` Saklı yordam içinde oluşturulduğu [kullanarak mevcut saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) öğretici. Karşılık gelen web sayfasını (`~/AdvancedDAL/ExistingSprocs.aspx`) Bu saklı yordam tarafından döndürülen sonuçları gösteren GridView içerir. Tarayıcı yoluyla bu sayfasını ziyaret edin. Sayfasında, kesme noktasına ulaşma bağlı `Products_SelectByCategoryID` saklı yordam basın ve denetimi için Visual Studio döndürdü. Tıpkı adım 1'de, saklı yordam s deyimleri ve görünüm adımla ve parametre değerlerini değiştirebilirsiniz.


[![ExistingSprocs.aspx sayfa İçecekler başlangıçta görüntüler.](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Şekil 6**: `ExistingSprocs.aspx` sayfası ilk başta İçecekler görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image14.png))


[![Saklı yordam s kesme noktasına ulaşıldı](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Şekil 7**: saklı yordam s kesme noktasına ulaşıldı ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image17.png))


Şekil 7 gösterir, değerini İzleme penceresinde olarak `@CategoryID` parametre 1'dir. Bunun nedeni, `ExistingSprocs.aspx` sayfası olan İçecekler kategorisindeki ilk başta ürünleri görüntüler bir `CategoryID` 1 değeri. Aşağı açılan listeden farklı bir kategori seçin. Bunun yapılması, geri göndermeye neden olur ve yeniden yürütür `Products_SelectByCategoryID` saklı yordamı. Yeniden ancak bu kez kesme noktasına isabet `@CategoryID` parametre s değeri yansıtır s Seçili aşağı açılan liste öğesi `CategoryID`.


[![Aşağı açılan listeden farklı bir kategori seçin](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Şekil 8**: aşağı açılan listeden farklı bir kategori seçin ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID Parametre Web sayfasından seçilen kategori yansıtır](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Şekil 9**: `@CategoryID` parametresi yansıtan kategoriyi seçilen Web sayfasındaki ([tam boyutlu görüntüyü görmek için tıklatın](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Kesme noktasına `Products_SelectByCategoryID` saklı yordam değil ziyaret isabet `ExistingSprocs.aspx` sayfasında, SQL Server onay kutusunu ASP.NET uygulamasının s özellikleri sayfasında hata ayıklayıcıları bölümünde denetlendi, bağlantı havuzu aktarıldığından emin emin olun devre dışı bırakıldı ve veritabanı s uygulamasında hata ayıklama seçeneği etkinleştirilir. Re yine sorun, Visual Studio'yu yeniden başlatın ve yeniden deneyin.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>T-SQL veritabanı nesnelerini uzak örneklerinde hata ayıklama

Visual Studio ile aynı makinede SQL Server veritabanı örneği olduğunda, veritabanı nesneleri Visual Studio ile hata ayıklama oldukça basittir. SQL Server ve Visual Studio farklı makinelerde bulunuyorsa ancak ardından dikkatli bir yapılandırma her şeyin düzgün çalışmasını almak için gereklidir. İki temel görevler ile kalmaktadır vardır:

- ADO.NET ile veritabanına bağlanmak için kullanılan oturum açma ait olduğundan emin olun `sysadmin` rol.
- Geliştirme makinenizde Visual Studio tarafından kullanılan Windows kullanıcı hesabı ait geçerli bir SQL Server oturum açma hesabı olduğundan emin olun `sysadmin` rol.

İlk adımı oldukça basittir. İlk olarak, ASP.NET uygulamasından veritabanına bağlanmak ve ardından SQL Server Management Studio'dan ilgili oturum açma hesabı eklemek için kullanılan kullanıcı hesabını tanımlama `sysadmin` rol.

İkinci görev, uygulamada hata ayıklamak için kullandığınız Windows kullanıcı hesabı Uzak veritabanı geçerli bir oturum açma olmasını gerektirir. Ancak, büyük olasılıkla iş istasyonunuzu ile oturum açmış Windows hesap SQL Server üzerinde geçerli bir oturum değil uygulanır. Belirli bir oturum açma hesabınızın SQL Server'a eklemek yerine, bazı Windows kullanıcı hesabı SQL Server hata ayıklama hesabı olarak atamak için daha iyi bir seçenek olacaktır. Ardından, uzak bir SQL Server örneğinin veritabanı nesnelerini hata ayıklamak için Visual Studio, Windows oturum açma hesabı s kimlik bilgilerini kullanarak çalıştırın.

Bir örnek noktalar açıklığa yardımcı olmalıdır. Adlı bir Windows hesabı olduğunu hayal `SQLDebug` Windows etki alanı içinde. Bu hesap, uzak SQL Server örneği için geçerli bir oturum açma ve bir üyesi olarak eklenmesi gerekir `sysadmin` rol. Ardından, Visual Studio'dan uzak SQL Server örneğinin hata ayıklamak için Visual Studio olarak çalıştırmak ihtiyacımız `SQLDebug` kullanıcı. Bu, bizim iş istasyonu dışında olarak yeniden oturum açarak yapılabilir `SQLDebug`, ve ardından Visual Studio, ancak basit bir yaklaşım başlatma kendi kimlik bilgilerini kullanarak, iş istasyonunda oturum açmak ve ardından olacak `runas.exe` olarak Visual Studio'yu başlatmak için `SQLDebug` kullanıcı. `runas.exe` farklı bir kullanıcı hesabı altında gerçekleştirebilirler yürütülecek belirli bir uygulama sağlar. Visual Studio olarak başlatmak için `SQLDebug`, komut satırına aşağıdaki ifadeyi girebilirsiniz:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Bu işlemle ilgili daha ayrıntılı bir açıklaması için bkz: [William r Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Visual Studio ve SQL Server, yedinci Edition kılavuz* yanı [nasıl yapılır: SQL Server izinleri ayarlayın hata ayıklama](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Geliştirme makinenizde Windows XP Service Pack 2 olarak çalışıyorsa Internet Bağlantısı Güvenlik Duvarı uzaktan hata ayıklamaya izin verecek şekilde yapılandırmanız gerekir. [Nasıl için: SQL Server 2005 ayıklamayı etkinleştirin](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) makale notları bu iki adımdan oluşur: (a) Visual Studio ana makinede eklemelisiniz `Devenv.exe` özel durumlar listesini ve açık TCP 135 bağlantı noktası; ve (b) uzak makinede (SQL) açmanız gerekir TCP 135 bağlantı noktası ve ekleme `sqlservr.exe` özel durumlar listesine. Etki alanı ilkeniz ağ iletişimi, IPSec yapılması gerekiyorsa, UDP 4500 ile UDP 500 bağlantı noktalarını açmanız gerekir.


## <a name="summary"></a>Özet

.NET uygulama kodu için hata ayıklama desteği sağlamanın yanı sıra Visual Studio hata ayıklama seçenekleri SQL Server 2005'e yönelik çeşitli de sağlar. Bu öğreticide bu seçenekleri ikilisi incelemiştik: doğrudan veritabanı hata ayıklamaya ve uygulamada hata ayıklama. Doğrudan bir T-SQL veritabanı nesnesinin hata ayıklamak için Sunucu Gezgini aracılığıyla nesneyi bulmak sağ tıklayın ve içine adımla seçin. Bu, hata ayıklayıcıyı başlatır ve ilk ifade bu noktada nesne s deyimleri ve görünüm adım ve parametre değerlerini değiştirmek veritabanı nesnesi, üzerinde durdurur. Adım 1'de bu yaklaşım içine adımlamak için kullandığımız `Products_SelectByCategoryID` saklı yordamı.

Uygulama hata ayıklama, doğrudan veritabanı nesnelerinde ayarlanması için kesme noktaları sağlar. Bir veritabanı nesnesi ile kesme noktaları (örneğin, bir ASP.NET web uygulaması) bir istemci uygulamasından çağrıldığında, hata ayıklayıcı yararlanırken programı durdurur. Uygulama hata ayıklama daha net bir şekilde uygulama eylemi belirli bir veritabanı nesnesi çağrılmasına neden olur gösterdiği için kullanışlıdır. Ancak, biraz daha fazla yapılandırma ve Kurulum doğrudan veritabanı hata ayıklamaya daha gerektirir.

Veritabanı nesneleri ayrıca, SQL Server projeleri ayıklanabilir. Şu SQL Server projeleri kullanacaksınız ve bunları oluşturmak ve hata ayıklama için kullanılacak veritabanı nesneleri sonraki öğreticide yönetilen.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](protecting-connection-strings-and-other-configuration-information-cs.md)
> [İleri](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
