---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: "Üretim veritabanını (C#) kullanmak için üretim Web uygulaması için yapılandırma | Microsoft Docs"
author: rick-anderson
description: "Önceki eğitimlerine açıklandığı gibi geliştirme ve üretim ortamlarını arasında farklılık yapılandırma bilgileri için seyrek değil. Es budur..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: e99b74030104bf17d4d79cd7670cf903270fa51f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Üretim Web uygulamanızı üretim veritabanını (C#) kullanacak şekilde yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Önceki eğitimlerine açıklandığı gibi geliştirme ve üretim ortamlarını arasında farklılık yapılandırma bilgileri için seyrek değil. Veritabanı bağlantı dizeleri geliştirme ve üretim ortamlarını arasında farklı olduğundan bu veri tabanlı web uygulamaları için özellikle doğrudur. Bu öğreticide daha ayrıntılı olarak uygun bir bağlantı dizesi eklemek için üretim ortamını yapılandırmak için yolları araştırır.


## <a name="introduction"></a>Giriş

Veri tabanlı web uygulamaları geliştirme zaman zaman daha farklı bir veritabanında üretimde genellikle kullanır. Üretim veritabanını web şirket s tesis barındırma veritabanı sunucusunda barındırılan sırada bir web ana bilgisayar sağlayıcı tarafından barındırılan ve yerel olarak geliştirilen uygulamaları geliştirme veritabanını Geliştirici s bilgisayarda genellikle bulunur. Veri tabanlı web uygulaması dağıtma üretim veritabanı sunucusuna geliştirme veritabanı kopyalama kapsar. Önceki Öğreticide bu adımı gerçekleştirmenin yöntemler Aranan.

Bilgiler, web uygulamasını kullanan bir *bağlantı dizesi* veritabanı ile bağlantı kuramıyor. Genellikle depolanan bağlantı dizesini `Web.config`, veritabanı sunucusu adı, veritabanı, güvenlik bağlamı ve diğer bilgileri adını belirtir. Web uygulaması geliştirme veya üretim ortamlarında kullanılıp kullanılmadığını web uygulaması tarafından kullanılan veritabanı bağımlı olduğundan, bağlantı dizeleri iki ortamlar arasında farklı olmalıdır.

Yapılandırma bilgileri geliştirme ve üretim ortamlarını arasında farklılık seyrek değil. *Ortak yapılandırma farklılıkları arasında geliştirme ve üretim* öğretici açıklanan teknikleri hakkında kısa bir tartışma yanı sıra, bu iki ortam arasında ayrı yapılandırma bilgileri korumak için veritabanı bağlantı dizeleri. Bu öğreticide daha ayrıntılı olarak uygun bir bağlantı dizesi eklemek için üretim ortamını yapılandırmak için yolları araştırır.

## <a name="examining-the-connection-string-information"></a>Bağlantı dizesi bilgilerini inceleniyor

Kitap incelemeleri web uygulaması tarafından kullanılan bağlantı dizesi uygulama s yapılandırma dosyasında depolanan `Web.config`. `Web.config`bağlantı dizeleri, aptly adlı depolamak için özel bir bölüm içerir [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx). `Web.config` Adlı bu bölümünde tanımlanmış bir bağlantı dizesi Kitap incelemeleri Web sitesi için dosya `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

Bağlantı dizesi - veri kaynağı =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated güvenlik = True; Kullanıcı örneği = True - seçeneği/değer çifti noktalı virgül ve her seçeneğin tarafından ayrılmış ve bir eşittir işareti ayrılmış değerle seçenekleri ve değerleri, bir dizi oluşur. Bu bağlantı dizesi olarak kullanılır ve dört seçenekten şunlardır:

- `Data Source`-(varsa) veritabanı sunucusu ve veritabanı sunucusu örneği adı konumunu belirtir. Değer `.\SQLEXPRESS`, örnek bir veritabanı sunucusu ve bir örnek adı olduğu. Veritabanı sunucusunun uygulama ile aynı bilgisayarda olduğundan belirler; örnek adı `SQLEXPRESS`.
- `AttachDbFilename`-veritabanı dosyasının konumunu belirtir. Yer tutucu değerini içeren `|DataDirectory|`, uygulama s tam yoluna çözülmüş olduğu `App_Data` çalışma zamanında klasör.
- `Integrated Security`-Belirtilen bir kullanıcı adı/parola (false) veritabanı veya geçerli Windows hesabı kimlik bilgileri (true) bağlanırken kullanılıp kullanılmayacağını gösteren bir Boole değeri.
- `User Instance`-yerel bilgisayarda yönetici olmayan kullanıcıların ekleyin ve bir SQL Server Express Edition veritabanına bağlanan izin verilip verilmeyeceğini belirten SQL Server Express sürümleri için belirli bir yapılandırma seçeneği. Bkz: [SQL Server Express kullanıcı örnekleri](https://msdn.microsoft.com/en-us/library/ms254504.aspx) Bu ayar hakkında daha fazla bilgi için.
  

İzin verilen bağlantı dizesi seçenekleri bağlandığınız veritabanı ve kullanılan ADO.NET veritabanı sağlayıcısı bağlıdır. Bir Microsoft SQL veritabanı farklı bir Oracle veritabanına bağlanmak için kullanılan sunucusuna bağlanmak için örneğin, bağlantı dizesi. Benzer şekilde, SqlClient sağlayıcısı kullanarak Microsoft SQL Server veritabanına bağlanma OLE DB Sağlayıcısı'nı kullanırken daha farklı bir bağlantı dizesi kullanır.

Bir site gibi kullanarak el ile veritabanı bağlantı dizesi oluşturma [ConnectionStrings.com](http://www.connectionstrings.com/) hangi seçenekleri kullanılabilir bir kaynak olarak. Ancak, Visual Studio Server Explorer'da veritabanı eklemek için daha kolay bir yaklaşım olduğu ve sonra Özellikler penceresinde bağlantı dizesinden alın. Bu ikinci tekniği üretim veritabanı sunucusu ile bağlantı dizesi oluşturmak için kullanmak s olanak tanır.

Visual Studio'yu açın ve ardından Sunucu Gezgini penceresine gidin (Visual Web Developer veritabanı Gezgini bu pencereyi denir). Veri bağlantıları seçeneğini sağ tıklatın ve bağlam menüsünden Bağlantı Ekle seçeneğini belirleyin. Bu Şekil 1'de gösterilen Sihirbazı'nı açar. Uygun veri kaynağını seçin ve devam'ı tıklatın.


[![Sunucu Gezgini için yeni bir veritabanı eklemek için seçin](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Şekil 1**: yeni bir veritabanı için Sunucu Gezgini eklemek için seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Ardından, çeşitli veritabanı bağlantı bilgilerini belirtin (bkz: Şekil 2). Web ile kaydolurken bilgi nasıl üzerinde sağladıkları şirket barındırma veritabanı sunucusu adı, veritabanı adı, kullanıcı adı ve parola veritabanına bağlanmak için kullanılacak veritabanına bağlanmak ve benzeri. Bu bilgileri girdikten sonra bu sihirbazı tamamlamak için ve veritabanı için Sunucu Gezgini eklemek için Tamam'ı tıklatın.


[![Veritabanı bağlantısı bilgilerini belirtin](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Şekil 2**: veritabanı bağlantısı bilgilerini belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Üretim ortamında veritabanı sunucu Gezgini'nde listelenmiş olmalıdır. Server Explorer'dan veritabanını seçin ve Özellikler penceresine gidin. Veritabanı s bağlantı dizesini içeren bağlantı dizesini adlı bir özellik var. bulur. Üretim ve SqlClient sağlayıcısı bir Microsoft SQL Server veritabanı kullanılarak varsayarak, bağlantı dizenizi aşağıdakine benzer görünmelidir:

**Veri kaynağı =*serverName*; Initial Catalog =*databaseName*; Kalıcı güvenlik bilgisi = True; Kullanıcı Kimliği =*kullanıcıadı*; Parola =*parola***

Burada *serverName*, *databaseName*, *kullanıcıadı*, ve *parola* veritabanı sunucusu adı, veritabanı için değerlerle olan adını, kullanıcı adı ve parola, web ana bilgisayar şirketiniz tarafından sağlanan.

## <a name="deploying-the-book-reviews-web-application"></a>Kitap incelemeleri Web uygulaması dağıtma

Önceki öğretici geliştirme veritabanını üretim ortamına kopyalama aracılığıyla adım adım, ancak veri tabanlı uygulama dağıtma keşfedin değil. Bu noktada üretim ortamına veritabanı içeriyor ancak statik incelemeleri ile Kitap incelemeleri uygulamanın sürümünü kullanıyor. Üretim Sunucusu güncelleştirilmiş yapılandırma bilgilerinin yanı sıra yeni, veri güdümlü uygulamayı dağıtmak gerekir.

Veri tabanlı uygulamayı geliştirme ortamından üretim dağıtmak için bir dakikanızı ayırın. Bu işlem, önceki eğitimlerine ayrıntılı olarak ele. Yenileyici gerekirse bkz *bir FTP İstemcisi'ni kullanarak Web sitenizi dağıtma* veya *dağıtma bilgisayarınızı Web sitesini kullanarak Visual Studio* öğreticileri. Bir başka deyişle üretim ortamında kullanılan üretim veritabanı bağlantı dizesi olduğundan emin olmak gerekir `Web.config` dosya dağıtılmalıdır. Özellikle, bu değişiklik `Web.config` s dosyası `<connectionStrings>` öğesi üretim veritabanı bağlantı dizesi içermesi gerekir ve aşağıdakine benzer görünmelidir:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Bağlantı dizesi Not `<connectionStrings>` öğesi aynı adlandırılır (`ReviewsConnectionString`), ancak artık geliştirme veritabanı bağlantı dizesi yerine üretim veritabanı bağlantı dizesi içeriyor.

Daha fazla şekillendirilmiş bir dağıtımı iş akışı yoksa, ya da el ile değiştirmek `Web.config` (geliştirme veritabanı bağlantı dizesi kullanarak geri dönmek hatırlamak dağıtmadan önce üretim veritabanı bağlantı dizesi kullanmak için dosya Daha sonra) ya da ayrı bir `Web.config` üretim ortamına dağıtım işleminin bir parçası yüklenen üretim ortamı yapılandırma bilgilerini içeren dosya.

> [!NOTE]
> Yanlışlıkla dağıtırsanız bir `Web.config` olacaktır sonra bir hata üretim uygulamanın veritabanına bağlanma girişiminde bulunduğunda, geliştirme veritabanı bağlantı dizesi içeren dosya. Bu hata olarak bildirimleri bir `SqlException` bir iletiyle raporlama sunucusu bulunamadı veya erişilebilir değildi.


Site için üretim dağıtıldıktan sonra tarayıcınız üzerinden üretim sitesini ziyaret edin. Görebilir ve veri tabanlı uygulama yerel olarak çalıştırırken olarak aynı kullanıcı deneyimini keyfini çıkarın. Elbette üretim Web sitesini ziyaret ettiğinizde geliştirme ortamında Web sitesini ziyaret ederken veritabanı geliştirme kullanır ancak site üretim veritabanı sunucusu tarafından desteklenir. Şekil 3 gösterir *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki* (Not tarayıcı s Adres çubuğundaki URL'yi) üretim ortamında Web sayfasını inceleyin.


[![Üzerinde şimdi kullanılabilir üretim veri-odaklı uygulamasıdır!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Şekil 3**: The Data-Driven uygulamasıdır üzerinde şimdi kullanılabilir üretim! ([Tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Bağlantı dizeleri ayrı yapılandırma dosyasında depolama

Geliştirme ve üretim ortamları ayrı yapılandırma bilgilerini korumak için ortak iki sürümü için bir tekniktir `Web.config`: geliştirme ortamında, diğeri üretim için için. Dağıtma-zaman uygun `Web.config` sürümü üretim ortamına kopyalanabilir. İdeal olarak, bu işlemi dağıtım iş akışının bir parçası otomatik.

İki ayrı koruma yerine `Web.config` isteğe bağlı olarak, şunları yapabilirsiniz dosyaları daha ayrıntılı farklar sağlar. Oluşturan öğeler `Web.config` dosya, ardından başvurulan bir dış yapılandırma dosyalarında tanımlanabilir `Web.config` dosya. Bir olması buna koysalar `Web.config` dosya bağlantı dizeleri içerecektir bir databaseConnectionStrings.config dosyaya başvuruda bulunan her iki ortamlar uygulama tarafından kullanılan ve her ortam için benzersiz olacaktır. Farklı yapılandırma bilgilerini ayrı dosyalarına ayıran bir tidier sağladığını bulabilirim ve daha basit `Web.config` dosyası ve daha fazlasını açıkça geliştirme ve üretim ortamlarını yapılandırma farkları özetler.

Adlı web uygulamasında yeni bir klasör oluşturarak bu teknik kullanmaya başlamak `ConfigSections`. Ardından, iki dosya databaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config adlı bu yeni klasör ekleyin. Ardından, kopyalama `<connectionStrings>` öğesinden `Web.config` databaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config dosyalarına ve bağlantı dizesini değiştirin databaseConnectionStrings.production.config dosya böylece üretim veritabanı bağlantı dizesi belirtir. Örneğin, databaseConnectionStrings.dev.config dosyasını içermesi gerekir. yalnızca `<connectionStrings>` geliştirme veritabanını başvuruda bulunan bir bağlantı dizesi bir öğesiyle:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Benzer şekilde, databaseConnectionStrings.production.config dosyası yalnızca içermelidir bir `<connectionStrings>` öğesi, ancak bir üretim veritabanı bağlantı dizesi içeriyor.

DatabaseConnectionStrings.dev.config dosyasının bir kopyasını oluşturun ve databaseConnectionStrings.config olarak adlandırın.

> [!NOTE]
> D gibi istediğiniz varsa, yapılandırma dosyasını databaseConnectionStrings.config dışında bir şey adlandırabilirsiniz `connectionStrings.config` veya `dbInfo.config`. Ancak, dosya adını verdiğinizden emin olun bir `.config` uzantısı olarak `.config` dosyaları varsayılan olarak, değil sunulan ASP.NET altyapısı tarafından. Dosya başka bir ad kullanırsanız, ister `connectionStrings.txt`, bir kullanıcı tarayıcısında için işaret edebilir [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) ve dosyanın içeriğini görüntülemek!


Bu noktada `ConfigSections` klasörü üç dosyaları (bkz. Şekil 4) içermelidir. DatabaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config dosyaları sırasıyla geliştirme ve üretim ortamları için bağlantı dizelerini içerir. DatabaseConnectionStrings.config dosya çalışma zamanında web uygulaması tarafından kullanılan bağlantı dizesi bilgilerini içerir. Üretimde databaseConnectionStrings.config dosyası özdeş olmalıdır ancak sonuç olarak, databaseConnectionStrings.config dosya geliştirme ortamında databaseConnectionStrings.dev.config dosyasını özdeş olmalıdır databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Şekil 4**: ConfigSections ([tam boyutlu görüntüyü görüntülemek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Şimdi istemek üzere ihtiyacımız `Web.config` databaseConnectionStrings.config dosyasında, bağlantı dizesi depolama için kullanılacak. Açık `Web.config` ve varolan `<connectionStrings>` aşağıdaki öğeyle:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource` Özniteliği belirtir göreli fiziksel bir yola `Web.config` dosya. Dış `.config` dosyasıdır aynı dizinde `Web.config` sonra bu öznitelik için dosya adını ayarlayın `.config` dosya. Varsa, bir alt s databaseConnectionStrings.config, olduğu gibi bir ters eğik çizgi ConfigSections\databaseConnectionStrings.config gibi klasör ve dosya adları sınırlandırmak için kullanarak alt klasör belirtin.

Bu değişikliği ile geliştirme ve üretim ortamlarını aynı içeren `Web.config` dosya. Şimdi tek fark databaseConnectionStrings.config dosyasıdır. Üretim databaseConnectionStrings.production.config dosya kopyalamak ve databaseConnectionStrings.config için yeniden adlandırın. Gelecekte, varsa, üretim veritabanı bağlantı dizesi yapılan değişiklikler, databaseConnectionStrings.production.config dosyasına yapın ve üretim için bu dosyayı karşıya yüklemeyi databaseConnectionStrings.config yeniden adlandırılması gerekir.

> [!NOTE]
> Herhangi bir bilgi belirtebilir `Web.config` ayrı bir dosya ve kullanım öğesinde `configSource` dosyanın içinden başvurmak için öznitelik `Web.config`.


## <a name="summary"></a>Özet

Veri tabanlı uygulamalar, geliştirme ve üretim ortamlarında genellikle farklı veritabanlarını kullanır. Sonuç olarak, web uygulama s yapılandırmasında depolanan veritabanı bağlantı dizeleri ortamı benzersiz olması gerekir. Bu öğreticide üretim veritabanı bağlantı dizesi ve iki ortamın benzersiz bağlantı dizesi bilgilerini korumak için yollar belirleme inceledik.

Mutluluk programlama!

#### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bağlantı dizeleri ve yapılandırma dosyaları](https://msdn.microsoft.com/en-us/library/ms254494.aspx)
- [Veritabanı yapılandırma bilgilerini @ ConnectionStrings.com dizeleri](http://www.connectionstrings.com/)
- [Web.config dosyasının dışında taşıma ayarları](http://www.asp101.com/tips/index.asp?id=154)
- [İçin teknik belgeler &lt;connectionStrings&gt; öğesi](https://msdn.microsoft.com/en-us/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[Önceki](deploying-a-database-cs.md)
[sonraki](configuring-a-website-that-uses-application-services-cs.md)
