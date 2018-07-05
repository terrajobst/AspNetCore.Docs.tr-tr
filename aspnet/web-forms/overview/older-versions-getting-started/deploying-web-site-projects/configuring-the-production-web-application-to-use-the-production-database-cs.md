---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Üretim Web uygulamasını üretim veritabanını (C#) kullanacak şekilde yapılandırma | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde açıklandığı gibi geliştirme ve üretim ortamları arasında farklı yapılandırma bilgileri için sık karşılaşılan bir durum değil. Bu, es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: b6899c199ea593e74d4d423cfc3f832e38bd7e57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374956"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Üretim Web uygulamasını üretim veritabanını (C#) kullanacak şekilde yapılandırma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Önceki öğreticilerde açıklandığı gibi geliştirme ve üretim ortamları arasında farklı yapılandırma bilgileri için sık karşılaşılan bir durum değil. Veritabanı bağlantı dizelerini geliştirme ve üretim ortamları arasında farklı olduğundan bu veri tabanlı web uygulamaları için özellikle doğrudur. Bu öğretici, daha ayrıntılı olarak uygun bir bağlantı dizesi eklemek için üretim ortamı yapılandırmak için yöntemleri ele alıyor.


## <a name="introduction"></a>Giriş

Veri odaklı web uygulamaları, üretim ortamında genellikle geliştirme, zamana göre farklı bir veritabanı kullanır. Üretim veritabanı bir web barındırma şirketi s tesis veritabanı sunucuda barındırılıyor olsa bir web ana bilgisayar sağlayıcı tarafından barındırılan ve yerel olarak geliştirilmiş uygulamaları için geliştirme veritabanı Geliştirici s bilgisayarda genellikle bulunur. Veri odaklı web uygulamasını dağıtma üretim veritabanı sunucusuna geliştirme veritabanı kopyalama kapsar. Önceki Öğreticide bu adımı tamamlamak için yöntemler incelemiştik.

Bilgiler, web uygulamasını kullanan bir *bağlantı dizesi* veritabanı ile bağlantı kurmak için. Genellikle depolanan bağlantı dizesini `Web.config`, veritabanı sunucusu adı, veritabanı, güvenlik bağlamını ve diğer bilgileri adını belirtir. Web uygulaması geliştirme veya üretim ortamlarında kullanılıp kullanılmadığını web uygulaması tarafından kullanılan veritabanı bağlı olduğundan, bağlantı dizeleri iki ortam arasında farklı olmalıdır.

Geliştirme ve üretim ortamları arasında farklı yapılandırma bilgileri için sık karşılaşılan bir durum değil. *Yaygın yapılandırma farklılıkları arasında geliştirme ve üretim* öğreticide ele alınan teknikleri hakkında kısa bir açıklama yanı sıra, bu iki ortam arasında ayrı yapılandırma bilgilerini koruma veritabanı bağlantı dizeleri. Bu öğretici, daha ayrıntılı olarak uygun bir bağlantı dizesi eklemek için üretim ortamı yapılandırmak için yöntemleri ele alıyor.

## <a name="examining-the-connection-string-information"></a>Bağlantı dizesi bilgilerini İnceleme

Kitap incelemeleri web uygulaması tarafından kullanılan bağlantı dizesi uygulama s yapılandırma dosyasında depolanan `Web.config`. `Web.config` bağlantı dizeleri, aptly adlı depolamak için özel bir bölüm içerir [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). `Web.config` Adlı bu bölümünde tanımlanmış bir bağlantı dizesi Kitap incelemeleri Web sitesi için dosya `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

-Veri kaynağı bağlantı dizesi =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated güvenlik = True; Kullanıcı örneği = True - seçeneği/değer çiftleri noktalı virgül ve her seçeneği tarafından ayrılmış ve bir eşittir işareti ile ayrılmış değerine sahip bir dizi seçenek ve değerlerini, oluşur. Bu bağlantı dizesinde kullanılan dört Seçenekler şunlardır:

- `Data Source` -(varsa) veritabanı sunucusu ve veritabanı sunucusu örnek adı konumunu belirtir. Değer `.\SQLEXPRESS`, bir örnek bir veritabanı sunucusu ve örnek adı olduğu. Veritabanı sunucusunun, uygulama ile aynı bilgisayarda olduğundan belirler; örnek adı `SQLEXPRESS`.
- `AttachDbFilename` -veritabanı dosyasının konumunu belirtir. Yer tutucu değeri içeren `|DataDirectory|`, uygulama s tam yolunu çözümlenmiş olduğu `App_Data` zamanında klasör.
- `Integrated Security` -Belirtilen bir kullanıcı adı/parola (false) veritabanı veya geçerli Windows hesap kimlik bilgilerini (true) bağlanırken kullanılıp kullanılmayacağını belirten bir Boole değeri.
- `User Instance` -SQL Server Express sürümleri yerel bilgisayarda yönetici olmayan kullanıcılar eklemek ve SQL Server Express Edition veritabanına bağlanma izin verilip verilmeyeceğini gösteren özel bir yapılandırma seçeneği. Bkz: [SQL Server Express kullanıcı örnekleri](https://msdn.microsoft.com/library/ms254504.aspx) bu ayarı hakkında daha fazla bilgi için.
  

İzin verilen bağlantı dizesi seçeneklerinin bağlandığınız veritabanı ve kullanılan ADO.NET veritabanı sağlayıcısı bağlıdır. Bir Microsoft SQL veritabanı farklı bir Oracle veritabanına bağlanmak için kullanılan sunucuya bağlanmak için örneğin, bağlantı dizesi. Benzer şekilde, SqlClient Sağlayıcısı'nı kullanarak Microsoft SQL Server veritabanına bağlanma, OLE DB sağlayıcısı kullanırken daha farklı bir bağlantı dizesi kullanır.

Veritabanı bağlantı dizesi, bir site gibi el ile oluşturabilirsiniz [ConnectionStrings.com](http://www.connectionstrings.com/) bir kaynak için hangi seçenekler kullanılabilir. Ancak, Visual Studio'da Sunucu Gezgini veritabanı eklemek için daha kolay yaklaşım ve sonra Özellikler penceresinde bağlantı dizesini almak için. Üretim veritabanı sunucusu için bağlantı dizesi oluşturmak için ikinci bu tekniği kullanması s olanak tanır.

Visual Studio'yu açın ve ardından Sunucu Gezgini penceresine gidin (Visual Web Developer veritabanı Gezgini bu pencereyi denir). Veri bağlantıları seçeneği sağ tıklayın ve bağlam menüsünden Bağlantı Ekle seçeneğini seçin. Bu sihirbazın Şekil 1'de gösterilen getirir. Uygun veri kaynağını seçin ve devam'ı tıklatın.


[![Sunucu Gezgini için yeni bir veritabanı eklemek seçin](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Şekil 1**: Sunucu Gezginine yeni bir veritabanı eklemek seçin ([tam boyutlu görüntüyü görmek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


Ardından, çeşitli veritabanı bağlantı bilgilerini belirtin. (bkz: Şekil 2). Web barındırma şirketi ile kaydolurken bunlar bilgileri veritabanına - veritabanı sunucu adı, veritabanı adı, kullanıcı adı ve parola veritabanına bağlanmak ve benzeri için kullanılacak bağlanma sağlamış olması gerekir. Bu bilgileri girdikten sonra bu sihirbazı tamamlayın ve Sunucu Gezgini veritabanı eklemek için Tamam'a tıklayın.


[![Veritabanı bağlantı bilgilerini belirtin](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Şekil 2**: veritabanı bağlantı bilgilerini belirtme ([tam boyutlu görüntüyü görmek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


Üretim ortamında veritabanı sunucu Gezgini'nde listelenmiş olmalıdır. Sunucu Gezgini'nden veritabanını seçin ve Özellikler penceresine gidin. Burada veritabanı s bağlantı dizesiyle bağlantı dizesi adlı bir özellik bulacaksınız. Üretim ve SqlClient sağlayıcısı bir Microsoft SQL Server veritabanı kullandığınız varsayılarak, bağlantı dizenizi aşağıdakine benzer görünmelidir:

<strong>Veri kaynağı =<em>serverName</em>; Initial Catalog =<em>databaseName</em>; Kalıcı güvenlik bilgisi = True; Kullanıcı Kimliği =<em>kullanıcıadı</em>; Parola =*parola</strong>*

Burada *serverName*, *databaseName*, *kullanıcıadı*, ve *parola* değerlerle veritabanı sunucu adı, veritabanı için olan ad ve kullanıcı adı ve parola, web ana bilgisayar şirketiniz tarafından sağlanan.

## <a name="deploying-the-book-reviews-web-application"></a>Kitap incelemeleri Web uygulaması dağıtma

Önceki öğreticide, üretim ortamına geliştirme veritabanı kopyalama aracılığıyla basamaklı, ancak veri odaklı uygulama dağıtımı keşfedin değil. Bu noktada üretim ortamına veritabanı içeriyor ancak statik incelemeleriyle Kitap incelemeleri uygulamanın sürümünü kullanıyor. Üretim sunucusunu güncelleştirilmiş yapılandırma bilgileriyle birlikte yeni, veri temelli uygulamayı dağıtmak ihtiyacımız var.

Veri odaklı uygulama geliştirme ortamından üretime dağıtmak için bir dakikanızı ayırın. Bu işlem, önceki öğreticilerdeki ayrıntılı olarak çalışmasındaki. Bilgilerinizi tazelemeniz gerekiyorsa bkz *sitenizi FTP istemcisi kullanarak dağıtma* veya *dağıtma uygulamanızın Web sitesini kullanarak Visual Studio* öğreticiler. Bir başka deyişle üretim ortamında kullanılan üretim veritabanı bağlantı dizesi olmasını sağlamak ihtiyacınız olacak `Web.config` dosya dağıtılmalıdır. Özellikle, bu değişiklik `Web.config` dosyası s `<connectionStrings>` öğesi üretim veritabanı bağlantı dizesi içermesi gerekir ve aşağıdakine benzer görünmelidir:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Bağlantı dizesi Not `<connectionStrings>` öğesi aynı adlandırılır (`ReviewsConnectionString`), ancak artık geliştirme veritabanı bağlantı dizesi yerine üretim veritabanı bağlantı dizesi içerir.

Daha resmileştirilmiş dağıtımı iş akışı yoksa ya da el ile değiştirmeniz `Web.config` (geliştirme veritabanı bağlantı dizesi kullanarak geri dönmek hatırlamak dağıtmadan önce üretim veritabanı bağlantı dizesini kullanmak için dosyası Daha sonra) veya ayrı bir bakımını `Web.config` üretim ortamına dağıtım işleminin bir parçası yüklenen üretim ortamı yapılandırma bilgileri içeren dosya.

> [!NOTE]
> Yanlışlıkla dağıtırsanız bir `Web.config` olacaktır sonra bir hata uygulamayı üretim veritabanına bağlanmaya çalıştığında, geliştirme veritabanı bağlantı dizesi içeren dosya. Bu hata olarak bildirimleri bir `SqlException` sunucu bulunamadı veya erişilebilir durumda değildi raporlama bir ileti ile.


Site üretime dağıtıldıktan sonra tarayıcınız üzerinden, üretim sitesini ziyaret edin. Görebilir ve veri odaklı uygulama yerel olarak çalıştırılırken aynı kullanıcı deneyimini keyfini. Elbette, üretim Web sitesini ziyaret ettiğinizde geliştirme ortamında bir Web sitesini ziyaret veritabanı geliştirme kullanır ancak site üretim veritabanı sunucusu tarafından desteklenir. Şekil 3 gösterir *öğretin kendiniz ASP.NET 3.5 24 saat içindeki* (tarayıcı s Adres çubuğundaki URL'yi Not) üretim ortamında Web sitesinden sayfasını inceleyin.


[![Şirket artık kullanılabilir üretim veri tabanlı uygulamadır!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Şekil 3**: The Data-Driven uygulamasıdır üzerinde artık kullanılabilir üretim! ([Tam boyutlu görüntüyü görmek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Depolama bağlantı dizelerini ayrı bir yapılandırma dosyası

Geliştirme ve üretim ortamlarında ayrı yapılandırma bilgilerini korumak için genel bir tekniktir iki sürümünü sağlamaktır `Web.config`: biri geliştirme ortamı, diğeri üretim için. Dağıtma-zaman uygun `Web.config` sürüm, üretim ortamına kopyalanabilir. İdeal olarak, bu işlemi dağıtım iş akışının bir parçası otomatik.

İki ayrı yerine `Web.config` isteğe bağlı olarak, şunları yapabilirsiniz dosyaları daha belirgin farklar sağlar. Oluşturan öğeleri `Web.config` dosya içinde başvurulan dış yapılandırma dosyalarında tanımlanabilir `Web.config` dosya. Bir sahip içinde koysalar `Web.config` bağlantı dizelerini içerecektir bir databaseConnectionStrings.config dosyasına başvuran her iki ortama uygulama tarafından kullanılan ve her ortam için benzersiz olacaktır dosya. Farklı yapılandırma bilgilerini ayrı dosyalara ayıran bir tidier sağladığını bulabilirim ve daha basit `Web.config` dosyası ve daha fazlasını açıkça geliştirme ile üretim ortamları arasında yapılandırma farklılıkları özetler.

Bu tekniği kullanması için yeni bir klasör adlı web uygulaması oluşturma işlemiyle başlayın `ConfigSections`. Ardından, iki dosya databaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config adlı bu yeni klasöre ekleyin. Ardından, kopyalama `<connectionStrings>` öğesinden `Web.config` databaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config dosyalarına ve sonra bağlantı dizesini değiştirme databaseConnectionStrings.production.config dosya böylece üretim veritabanı bağlantı dizesini belirtir. Örneğin, databaseConnectionStrings.dev.config dosyasını içermesi gerekir. yalnızca `<connectionStrings>` başvuran geliştirme veritabanı bağlantı dizesine sahip öğe:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

Benzer şekilde, databaseConnectionStrings.production.config dosya yalnızca içermelidir bir `<connectionStrings>` öğesi, ancak bir üretim veritabanı bağlantı dizesi.

DatabaseConnectionStrings.dev.config dosyanın bir kopyasını alın ve databaseConnectionStrings.config adlandırın.

> [!NOTE]
> D gibi istediğiniz varsa, yapılandırma dosyası, databaseConnectionStrings.config dışında bir şey adlandırabilirsiniz `connectionStrings.config` veya `dbInfo.config`. Ancak, içeren dosya adını mutlaka bir `.config` uzantısı olarak `.config` dosyaları varsayılan olmayan sunulan ASP.NET altyapısı tarafından. Dosya başka bir ad kullanırsanız, ister `connectionStrings.txt`, bir kullanıcı için kullanıcının tarayıcıyı işaret ediyor olabilir [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) ve dosyanın içeriğini görüntüleyin!


Bu noktada `ConfigSections` klasörü (bkz. Şekil 4) üç dosyayı içermelidir. DatabaseConnectionStrings.dev.config ve databaseConnectionStrings.production.config dosyalar sırasıyla geliştirme ve üretim ortamları için bağlantı dizelerini içerir. DatabaseConnectionStrings.config dosyanın çalışma zamanında web uygulaması tarafından kullanılan bağlantı dizesi bilgilerini içerir. Üretimde databaseConnectionStrings.config dosya aynı olmalıdır ancak sonuç olarak, databaseConnectionStrings.config dosya geliştirme ortamını databaseConnectionStrings.dev.config dosyasında aynı olmalıdır databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Şekil 4**: ConfigSections ([tam boyutlu görüntüyü görmek için tıklatın](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Artık istemek ihtiyacımız `Web.config` databaseConnectionStrings.config dosyayı kendi bağlantı dize deposu için kullanılacak. Açık `Web.config` ve varolan `<connectionStrings>` aşağıdaki öğe:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource` Özniteliği belirtir bir fiziksel yola göreli `Web.config` dosya. Dış `.config` dosyasıdır aynı dizinde `Web.config` bu öznitelik için dosya adını ayarlayın `.config` dosya. Varsa, bir alt dizinde s databaseConnectionStrings.config, olduğu gibi alt klasör ve dosya adları, ConfigSections\databaseConnectionStrings.config gibi sınırlandırmak için bir ters eğik çizgi kullanarak belirtin.

Bu değişiklikle geliştirme ve üretim ortamlarını aynı içeren `Web.config` dosya. Artık tek fark databaseConnectionStrings.config dosyasıdır. Üretim databaseConnectionStrings.production.config dosya kopyalamak ve databaseConnectionStrings.config için yeniden adlandırın. Gelecekte, varsa, değişiklikler üretim veritabanı bağlantı dizesine, bunları databaseConnectionStrings.production.config dosyada değişiklik yapmak ve sonra üretime kadar bu dosyayı karşıya yükleyin databaseConnectionStrings.config yeniden adlandırmadan gerekecektir.

> [!NOTE]
> Herhangi bir bilgi belirtebilir `Web.config` ayrı dosya ve kullanım öğesinde `configSource` içinden bu dosyaya başvurmak için öznitelik `Web.config`.


## <a name="summary"></a>Özet

Veri odaklı uygulamalar, geliştirme ve üretim ortamlarında farklı veritabanları genellikle kullanın. Sonuç olarak, web uygulama s yapılandırmasında depolanan veritabanı bağlantı dizelerini ortam başına benzersiz olması gerekir. Bu öğreticide iki ortam içinde benzersiz bir bağlantı dizesi bilgilerini korumak için yolu ve üretim veritabanı bağlantı dizesi belirleme inceledik.

Mutlu programlama!

#### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Bağlantı Dizeleri ve Yapılandırma Dosyaları](https://msdn.microsoft.com/library/ms254494.aspx)
- [Veritabanı yapılandırma bilgilerini @ ConnectionStrings.com dizeleri](http://www.connectionstrings.com/)
- [Web.config dosyasının dışında taşıma ayarları](http://www.asp101.com/tips/index.asp?id=154)
- [İçin teknik belgeler &lt;connectionStrings&gt; öğesi](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-a-database-cs.md)
> [İleri](configuring-a-website-that-uses-application-services-cs.md)
