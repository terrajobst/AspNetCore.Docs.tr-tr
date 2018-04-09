---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Saklı yordamlar (C#) hata ayıklama | Microsoft Docs
author: rick-anderson
description: Visual Studio Professional ve Team System sürümleri kesme noktalarını ayarlayın ve saklı yordamlar SQL Server içinde adım depolanan hata ayıklama yapmadan izin ver...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 52bb409798dae550c664b78521f0fb4793464833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="debugging-stored-procedures-c"></a>Hata ayıklama saklı yordamlar (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) veya [PDF indirin](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Visual Studio Professional ve Team System sürümleri saklı yordamlar uygulama kodunda hata ayıklama olarak kadar kolay hata ayıklama yapmadan kesme noktalarını ayarlayın ve saklı yordamlar SQL Server içinde adımını olanak sağlar. Bu öğretici, doğrudan veritabanı hata ayıklama ve saklı yordamlar uygulamada hata ayıklama gösterir.


## <a name="introduction"></a>Giriş

Visual Studio hata ayıklama bir deneyim sağlar. Birkaç tuş vuruşları veya fare tıklatma ile bu s bir programın yürütülmesini Durdur ve onun durumunu ve denetim akışı incelemek için kesme noktaları kullanmak mümkün. Visual Studio, uygulama kodunda hata ayıklama yanı sıra, SQL Server saklı yordamlardan hata ayıklama için destek sunar. Kesme noktaları bir ASP.NET arka plandaki kod sınıfı veya iş mantığı katmanı sınıfın içindeki kod yalnızca ayarlanabilir gibi şekilde saklı yordamlar bunlar çok yerleştirilebilir.

Bu öğreticide ulaşıldığından kesme noktaları ayarlamak için ne zaman saklı yordamı çalışan ASP.NET uygulamasından çağrıldığını nasıl saklı yordamlar Visual Studio içinde Sunucu Gezgini'nden de atlama adresindeki arar.

> [!NOTE]
> Ne yazık ki, saklı yordamlar yalnızca halinde adım adım ve Visual Studio Professional ve takım sistemleri sürümleri hata ayıklaması. Visual Web Developer veya Visual Studio standart sürümünü kullanıyorsanız, biz saklı yordamlar hata ayıklamak gereken adımlarda size yol, ancak adımları makinenizdeki çoğaltılacağı mümkün olmaz boyunca okuma Hoş Geldiniz.


## <a name="sql-server-debugging-concepts"></a>SQL Server hata ayıklama kavramları

Microsoft SQL Server 2005 ile tümleştirme sağlamak üzere tasarlanmış [ortak dil çalışma zamanı (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), tüm .NET derlemelerini tarafından kullanılan çalışma zamanı olduğu. Sonuç olarak, SQL Server 2005 yönetilen veritabanı nesnelerini destekler. Diğer bir deyişle, bir C# sınıfı yöntemleri olarak veritabanı nesnelerini saklı yordamları ve kullanıcı tanımlı işlevler (UDF'ler) gibi oluşturabilirsiniz. Bu, bu saklı yordamları ve .NET Framework ve kendi özel sınıflardan faydalanmak için UDF'ler sağlar. Elbette, SQL Server 2005 T-SQL veritabanı nesneleri için de destek sağlar.

SQL Server 2005 T-SQL ve yönetilen veritabanı nesneleri için hata ayıklama desteği sunar. Ancak, bu nesneler yalnızca Visual Studio 2005 Professional ve takım sistemleri sürümleri hata ayıklaması yapılabilir. Bu öğreticide hata ayıklama T-SQL veritabanı nesnelerini inceleyeceğiz. Sonraki öğretici, yönetilen nesneleri hata ayıklama sırasında arar.

[Genel bakış, T-SQL ve CLR hata ayıklama SQL Server 2005'te](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) blog girdisinden [SQL Server 2005 CLR tümleştirmesi takım](https://blogs.msdn.com/sqlclr/default.aspx) SQL Server 2005 nesneleri Visual Studio'da hata ayıklama için üç yol vurgular:

- **Veritabanı hata ayıklama (DDD) doğrudan** - biz adım saklı yordamlar ve UDF'ler gibi tüm T-SQL veritabanı nesnesine Sunucu Gezgini'nden. Adım 1'deki DDD inceleyeceğiz.
- **Uygulama hata ayıklama** -biz kesme bir veritabanı nesnesi içinde ve ASP.NET uygulamamızı çalıştırmak. Veritabanı nesnesi yürütüldüğünde, kesme noktası isabet ve denetimi için hata ayıklayıcı açık. Uygulama hata ayıklamaya biz bir veritabanı nesnesine uygulama kodundan adım edilemez olduğunu unutmayın. Biz açıkça kesme noktaları bu saklı yordamları veya UDF'ler durdurmak için hata ayıklayıcı burada istiyoruz ayarlamanız gerekir. Uygulama hata ayıklama 2. adımda başlangıç incelenir.
- **Bir SQL Server projesinde hata ayıklamayı** -Visual Studio Professional ve takım sistemleri sürümleri yönetilen nesneleri oluşturmak için yaygın olarak kullanılan bir SQL Server Proje türü içerir. SQL Server projeleri kullanarak inceleyeceğiz ve içeriklerini sonraki öğreticide hata ayıklama.

Visual Studio yerel ve uzak SQL Server örnekleri üzerindeki saklı yordamlar ayıklayabilirsiniz. Yerel bir SQL Server örneğini Visual Studio ile aynı makinede yüklü biridir. Sonra kullanmakta olduğunuz SQL Server veritabanı geliştirme makinenizde bulunmuyorsa, bir uzak örneğini kabul edilir. Bu öğreticileri için biz yerel SQL Server örnekleri kullanmakta olduğunuz. Uzak SQL server örneği üzerinde saklı yordamlar hata ayıklama yerel örneği üzerinde saklı yordamlar, hata ayıklama'den daha fazla yapılandırma adımı gerektirir.

Yerel bir SQL Server örneği kullanıyorsanız, 1. adımla başlamak ve Bu öğreticide sonuna çalışır. Ancak, uzak bir SQL Server örneği kullanıyorsanız, hata ayıklama sırasında emin olmak için ilk gerek geliştirme makinenize bir SQL Server oturumu uzak örneğinde sahip bir Windows kullanıcı hesabı ile oturum olur. Moveover, bu veritabanı oturum açmayı ve çalışan ASP.NET uygulamasından veritabanına bağlanmak için kullanılan veritabanı oturum açma üyesi olmalıdır `sysadmin` rol. Hata ayıklama T-SQL veritabanı nesneleri, Visual Studio ve SQL Server Uzak bir örneğini hata ayıklamak için yapılandırma hakkında daha fazla bilgi için bu öğreticinin sonunda uzak örnekleri bölümüne bakın.

Son olarak, hata ayıklama desteği T-SQL veritabanı nesneleri için .NET uygulamaları için destek hata ayıklama olarak zengin özellik olarak olduğunu anlayın. Hata ayıklama windows kümesini yalnızca kullanılabilir, örneğin, kesme noktası koşulları ve filtreleri, desteklenmez Düzenle ve devam et kullanamazsınız, komut penceresi yaramaz vb. işlenir. Bkz: [hata ayıklayıcı komutları ve Özellikler sınırlamalar](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) daha fazla bilgi için.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>1. adım: Doğrudan bir saklı yordam Adımlama

Visual Studio, doğrudan bir veritabanı nesnesi hata ayıklamak kolaylaştırır. Adımla için doğrudan veritabanı hata ayıklama (DDD) özelliğini kullanmak nasıl bakmak s izin `Products_SelectByCategoryID` Northwind veritabanı içinde saklı yordamı. Adından da anlaşılacağı gibi `Products_SelectByCategoryID` belirli bir kategorideki; ürün bilgilerini döndürür oluşturulduğu [kullanarak var olan saklı yordamlar için yazılan veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Öğreticisi. Sunucu Gezgini giderek başlatın ve Northwind veritabanı düğümünü genişletin. Ardından, saklı yordamlar klasörüne detaya, sağ tıklayın `Products_SelectByCategoryID` saklı yordamı ve bağlam menüsünden adım içine saklı yordam seçeneğini belirleyin. Bu hata ayıklayıcı başlatır.

Bu yana `Products_SelectByCategoryID` saklı yordam bekliyor bir `@CategoryID` giriş parametresi istendi bu değeri belirtin. Meşrubat hakkında bilgi döndürür 1 girin.


![Değer 1 @CategoryID parametresi](debugging-stored-procedures-cs/_static/image1.png)

**Şekil 1**: ' % s'değeri 1 için kullanmak `@CategoryID` parametresi


Değeri sağladığını sonra `@CategoryID` parametresi, saklı yordam gerçekleştirilir. Tamamlanıncaya kadar çalıştıran yerine rağmen hata ayıklayıcı ilk ifade yürütülmesine durur. Saklı yordam geçerli konumu belirten sarı ok kenar boşluğunda unutmayın. Görüntüleyin ve parametre değerlerini Gözcü penceresi yoluyla veya saklı yordamı parametre adı üzerine gelerek veya onları düzenleyin.


[![Hata ayıklayıcı saklı yordam üzerinde ilk deyimi durdu](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Şekil 2**: hata ayıklayıcı saklı yordam üzerinde ilk deyimi işlemi durduruldu ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image4.png))


Saklı yordam bir deyim birer birer adım adım için araç çubuğunda Step Over düğmesini tıklatın veya F10 tuşuna basın. `Products_SelectByCategoryID` Saklı yordam içeren tek bir `SELECT` F10 basarsa tek bir deyimde adım şekilde deyimi ve saklı yordamının yürütülmesi tamamlayın. Saklı yordam tamamlandıktan sonra çıktısını çıkış penceresinde görüntülenir ve hata ayıklayıcı sona erdirir.

> [!NOTE]
> Hata ayıklama T-SQL deyimi düzeyinde gerçekleşir; Adımla olamaz bir `SELECT` deyimi.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>2. adım: uygulama hata ayıklama için Web sitesi yapılandırma

Doğrudan Sunucu Gezgini'nden bir saklı yordam hata ayıklama kullanışlı olsa da, birçok senaryoda ASP.NET uygulamamız çağrıldığında saklı yordamı hata ayıklamaya daha ilgi duyuyoruz. Visual Studio içinde bir saklı yordam kesme noktaları eklemek ve ASP.NET uygulamasını hata ayıklamayı Başlat. Uygulamadan kesme noktaları olan bir saklı yordam çağrıldığında, yürütme kesme noktasında durdurulur ve biz görüntüleyebilir ve saklı yordam s parametre değerlerini değiştirmek ve 1. adımda komutlarında yaptığımız gibi kendi deyimleri adım.

Biz uygulamadan çağrılan saklı yordamlar hata ayıklama başlamadan önce SQL Server hata ayıklayıcısı ile tümleştirmek için ASP.NET web uygulaması istemeniz gerekir. Başlangıç Çözüm Gezgini'nde Web sitesi adına sağ tıklayarak (`ASPNET_Data_Tutorial_74_CS`). Özellik sayfaları seçeneği bağlam menüsünden, soldaki başlangıç seçenekleri öğeyi seçin ve SQL Server hata ayıklayıcıları bölümdeki onay (bkz: Şekil 3).


[![Uygulama s özellik sayfaları SQL Server onay kutusunu işaretleyin](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Şekil 3**: SQL Server onay uygulama s özellik sayfaları ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image7.png))


Ayrıca, biz bağlantı havuzu devre dışı uygulama tarafından kullanılan veritabanı bağlantı dizesini güncellemeniz gerekir. Bir veritabanına bir bağlantı kapatıldığında, karşılık gelen `SqlConnection` nesnesi kullanılabilir bağlantıları havuzunda yerleştirilir. Bir veritabanına bir bağlantı kurulurken bir bağlantı nesnesi bu havuzundan alınabilir yerine oluşturun ve yeni bir bağlantı kurmak zorunda. Bu bağlantı nesnelerin havuzu performans geliştirmesi olduğundan ve varsayılan olarak etkindir. Ancak, hata ayıklarken hata ayıklama altyapı doğru havuzdan alınan bağlantısı ile çalışırken, kurulmaz değil çünkü bağlantı havuzu devre dışı bırakma istiyoruz.

Devre dışı bağlantı havuzu için güncelleştirme `NORTHWNDConnectionString` içinde `Web.config` ayarı içeren `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> İşlemi tamamladıktan sonra SQL Server ASP.NET uygulamasında hata ayıklama kaldırarak bağlantı havuzu yeniden devreye sokmanız emin `Pooling` bağlantı dizesi ayarı (veya ayarlayarak `Pooling=true` ).


Bu noktada ASP.NET uygulaması, web uygulaması aracılığıyla çağrıldığında SQL Server veritabanı nesneleri hata ayıklamak Visual Studio izin verecek şekilde yapılandırıldı. Şimdi kalan tek şey için bir saklı yordam bir kesme noktası ekleyin ve hata ayıklamayı başlatmak için!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>3. adım: bir kesme noktası ekleme ve hata ayıklama

Açık `Products_SelectByCategoryID` saklı yordamı ve başlangıcında bir kesme noktası belirleyerek `SELECT` deyimi ilgili yerde kenar boşluğunda tıklayarak ya da imlecinizi başlangıcında yerleştirerek `SELECT` deyimi ve F9 tuşuna basarak. Şekil 4'te gösterildiği gibi kesme kenar boşluğunda kırmızı bir daire olarak görünür.


[![Products_SelectByCategoryID bir kesme noktası belirleyerek saklı yordamı](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Şekil 4**: bir kesme noktası kümesinde `Products_SelectByCategoryID` saklı yordam ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image10.png))


Bir istemci uygulaması ayıklanacak SQL veritabanı nesnesi için sırayla veritabanı uygulamasında hata ayıklama destekleyecek şekilde yapılandırılması zorunludur. Önce bir kesme noktası ayarladığınızda, bu ayar otomatik olarak açık, ancak iki kez kontrol akıllıca olur. Sağ `NORTHWND.MDF` Sunucu Gezgininde. Bağlam menüsü öğesi uygulamada hata ayıklama adlı bir checked menü içermelidir.


![Uygulama hata ayıklama seçeneği etkin olduğundan emin olun](debugging-stored-procedures-cs/_static/image11.png)

**Şekil 5**: uygulama hata ayıklama seçeneği etkin olduğundan emin olun


Kesme noktası ayarlama ve uygulama hata ayıklama seçeneği etkin, biz ASP.NET uygulamasından çağrıldığında saklı yordam hata ayıklamak hazır olursunuz. Hata ayıklayıcısı hata ayıklama menüsüne giderek başlatın ve hata ayıklamayı Başlat, F5 tuşuna veya yeşil tıklatarak seçme, araç çubuğunda simge yürütebilirsiniz. Hata ayıklayıcı başlatın ve Web sitesini başlatın.

`Products_SelectByCategoryID` Saklı yordam içinde oluşturulduğu [kullanarak var olan saklı yordamlar için yazılan veri kümesi s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Öğreticisi. Karşılık gelen web sayfasını (`~/AdvancedDAL/ExistingSprocs.aspx`) Bu saklı yordam tarafından döndürülen sonuçları görüntüler GridView içerir. Tarayıcı yoluyla bu sayfasını ziyaret edin. Sayfasında, kesme ulaştıktan sonra `Products_SelectByCategoryID` saklı yordam edeceği ve denetim için Visual Studio döndürdü. Gibi adım 1'de, saklı yordam s deyimleri ve görünüm adım ve parametre değerlerini değiştirebilirsiniz.


[![ExistingSprocs.aspx sayfası başlangıçta Meşrubat görüntüler](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Şekil 6**: `ExistingSprocs.aspx` sayfası başlangıçta Meşrubat görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image14.png))


[![Saklı yordam s kesme noktasına ulaşıldı](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Şekil 7**: kesme noktası ulaşıldı saklı yordam s ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image17.png))


Şekil 7 gösterir, değerini Gözcü penceresinde olarak `@CategoryID` 1 parametresidir. Bunun nedeni, `ExistingSprocs.aspx` sayfası sahip Meşrubat kategorisinde başlangıçta ürünleri görüntüler bir `CategoryID` 1 değeri. Farklı bir kategori aşağı açılan listeden seçin. Bunun yapılması geri gönderimin neden olur ve yeniden yürüten `Products_SelectByCategoryID` saklı yordamı. Yeniden ancak bu kez kesme noktası isabet `@CategoryID` parametre s değeri yansıtır s Seçilen aşağı açılan liste öğesi `CategoryID`.


[![Aşağı açılan listeden farklı bir kategori seçin](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Şekil 8**: aşağı açılan listeden farklı bir kategori seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID Parametresi Web sayfasından seçilen kategori yansıtır](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Şekil 9**: `@CategoryID` parametresi yansıtan kategoriyi Seçili Web sayfasından ([tam boyutlu görüntüyü görüntülemek için tıklatın](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Varsa kesme `Products_SelectByCategoryID` saklı yordam ziyaret eden değil isabet `ExistingSprocs.aspx` sayfasında, SQL Server onay kutusu ASP.NET uygulamasının s özellikleri sayfasında hata ayıklayıcıları bölümünde seçili olduğunu, bağlantı havuzu olduğunu emin olun devre dışı ve veritabanı s uygulamada hata ayıklama seçeneği etkinleştirilir. Re devam ediyorsanız, sorunlarınız Visual Studio'yu yeniden başlatın ve yeniden deneyin.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>T-SQL veritabanı nesnelerini uzak örneklerinde hata ayıklama

Visual Studio ile aynı makinede SQL Server veritabanı örneği olduğunda, veritabanı nesnelerini Visual Studio ile hata ayıklama oldukça basittir. SQL Server ve Visual Studio farklı makinelerde bulunuyorsa, ancak sonra bazı dikkatli yapılandırma düzgün çalışmasını her şeyi almak için gereklidir. Biz ile karşılaştığı iki çekirdek görevler şunlardır:

- ADO.NET aracılığıyla veritabanına bağlanmak için kullanılan oturum açma ait olduğundan emin olun `sysadmin` rol.
- Geliştirme makinenizde Visual Studio tarafından kullanılan Windows kullanıcı hesabı ait geçerli bir SQL Server oturum açma hesabı olduğundan emin olun `sysadmin` rol.

İlk adım oldukça basittir. İlk olarak, ASP.NET uygulamasından veritabanına bağlanmak ve SQL Server Management Studio'dan bu oturum açma hesabı eklemek için kullanılan kullanıcı hesabını tanımlama `sysadmin` rol.

İkinci görev uygulamada hata ayıklama için kullandığınız Windows kullanıcı hesabı Uzak veritabanı üzerinde geçerli bir oturum açma olmasını gerektirir. Ancak, büyük olasılıkla ile iş istasyonunuzu oturum Windows hesabı SQL Server üzerinde geçerli bir oturum değil değildir. Belirli bir oturum açma hesabınızın SQL Server'a eklemek yerine, bazı Windows kullanıcı hesabı SQL Server hata ayıklama hesabı olarak belirlemek için daha iyi bir seçim olacaktır. Ardından, uzak bir SQL Server örneğinin veritabanı nesnelerini hata ayıklamak için Visual Studio, Windows oturum açma hesabı s kimlik bilgilerini kullanarak çalışır.

Bir örnek noktalar açıklığa kavuşturmak yardımcı olmalıdır. Adlı bir Windows hesabı olduğunu düşünün `SQLDebug` Windows etki alanı içinde. Bu hesap için Uzak SQL Server örneği geçerli bir oturum açma ve bir üyesi olarak eklenmesi gerekir `sysadmin` rol. Ardından, Visual Studio'dan uzak SQL Server örneği hata ayıklamak için Visual Studio olarak çalıştırmak ihtiyacımız `SQLDebug` kullanıcı. Bu bizim iş istasyonu dışında olarak oturum açmayı tekrar açarak yapılabilir `SQLDebug`, ve ardından Visual Studio, ancak basit bir yaklaşım başlatma kendi kimlik bilgilerini kullanarak bizim iş istasyonunda oturum açmak ve ardından olacaktır `runas.exe` olarak Visual Studio'yu başlatmak için `SQLDebug` kullanıcı. `runas.exe` farklı bir kullanıcı hesabı gerçekleştirebilirler altında yürütülmek üzere belirli bir uygulama sağlar. Visual Studio olarak başlatmak için `SQLDebug`, komut satırında şu deyimi girebilirsiniz:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Bu işlemi hakkında daha ayrıntılı bir açıklaması için bkz: [William r Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Visual Studio ve SQL Server, yedinci Edition kılavuz* yanı [nasıl yapılır: SQL Server izinleri ayarlayın hata ayıklama için](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Geliştirme makinenizde Windows XP Service Pack 2 çalışıyorsa, Internet Bağlantısı Güvenlik Duvarı uzaktan hata ayıklama izin verecek şekilde yapılandırmanız gerekir. [Nasıl için: SQL Server 2005 hata ayıklamayı etkinleştir](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) makale notları bu iki adımdan oluşur: (a) Visual Studio ana makinede eklemelisiniz `Devenv.exe` özel durumlar listesine ve açık TCP 135 bağlantı noktası; ve (b) uzak makinede (SQL) açmanız gerekir TCP 135 bağlantı noktası ve ekleme `sqlservr.exe` özel durumlar listesine. Etki alanı İlkesi üzerinden IPSec yapılması ağ iletişimi gerektiriyorsa, UDP 4500 ve UDP 500 bağlantı noktalarını açmanız gerekir.


## <a name="summary"></a>Özet

Hata ayıklama desteği için .NET uygulama kodunun sağlamasının yanı sıra, Visual Studio hata ayıklama seçeneklerini SQL Server 2005, çeşitli de sağlar. Bu öğreticide şu iki Bu seçenekler Aranan: veritabanı doğrudan hata ayıklama ve uygulamada hata ayıklama. Doğrudan bir T-SQL veritabanı nesnesi hata ayıklamak için nesnesi Sunucu Gezgini üzerinden bulduktan sonra üzerinde sağ tıklayıp Step Into seçin. Bu hata ayıklayıcı başlatır ve bu noktada nesne s deyimleri ve görünüm adım ve parametre değerlerini değiştirmek veritabanı nesnesi ilk deyiminde durur. Adım 1'de bu yaklaşım adımla için kullandık `Products_SelectByCategoryID` saklı yordamı.

Uygulama hata ayıklama, doğrudan veritabanı nesnelerinde ayarlanması için kesme noktaları sağlar. Kesme noktaları olan bir veritabanı nesnesi (örneğin, bir ASP.NET web uygulaması) istemci uygulamasından çağrıldığında, hata ayıklayıcı yararlanırken programı durdurur. Uygulama hata ayıklama yararlıdır çünkü hangi uygulama eylem çağrılacak belirli bir veritabanı nesnesini neden olan daha net bir şekilde gösterir. Ancak, biraz daha fazla yapılandırma ve veritabanı doğrudan hata ayıklama daha Kurulum gerektirir.

Veritabanı nesnelerini ayrıca SQL Server projeleri hata ayıklaması yapılabilir. Biz SQL Server projeleri kullanarak arar ve bunları oluşturmak ve hata ayıklamak için nasıl kullanılacağını veritabanı nesnelerini sonraki öğreticide yönetilen.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](protecting-connection-strings-and-other-configuration-information-cs.md)
> [sonraki](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
