---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Çekirdek IIS ve ASP.NET Geliştirme Sunucusu (C#) arasındaki farkları | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET uygulamasını yerel olarak test ederken, ASP.NET Geliştirme Web sunucusu kullanıyorsanız yüksektir. Ancak, üretim Web sitesi büyük olasılıkla pow değildir...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: f4284a46fbae9ed609554b77c4e19f936b80595d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839693"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>IIS ve ASP.NET Geliştirme Sunucusu (C#) arasındaki temel farklılıklar
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Bir ASP.NET uygulamasını yerel olarak test ederken, ASP.NET Geliştirme Web sunucusu kullanıyorsanız yüksektir. Ancak, büyük olasılıkla güç beslemeli IIS üretim Web sitesi olur. Bu web sunucuları istekleri nasıl işleneceğini arasındaki bazı farklar vardır ve bu farklar önemli sonuçlar doğurabilir. Bu öğretici, daha fazla kodun farklılıklar açıklar.


## <a name="introduction"></a>Giriş

Her bir kullanıcı bir ASP.NET uygulamasını ziyaret kendi tarayıcı Web sitesine bir istek gönderir. Bu isteği oluşturmak ve istenen kaynak içeriğini döndürmek için ASP.NET çalışma zamanı ile koordine eden web sunucusu yazılımı tarafından seçilir. [**miyim** nternet **miyim** ilgi **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) olan bir paketi için Internet tabanlı ortak işlevselliği sağlayan hizmetler Windows sunucuları. IIS, üretim ortamlarında ASP.NET uygulamaları için en yaygın olarak kullanılan web sunucusu olabilir; Buna büyük olasılıkla ASP.NET uygulamanızı sunmak için web ana bilgisayar sağlayıcınız tarafından kullanılan web sunucusu yazılımı var. IIS de kullanılabilir olarak web sunucusu yazılım geliştirme ortamında, ancak bu IIS yüklenmesini kapsar ve düzgün şekilde yapılandırma.


ASP.NET Geliştirme Sunucusu bir geliştirme ortamı için alternatif bir web sunucusu seçeneğidir; Bu, birlikte ve Visual Studio'da tümleşiktir. Web uygulamasını IIS kullanmak üzere yapılandırılmadıysa, ASP.NET Geliştirme Sunucusu otomatik olarak başlatıldığından ve web sunucusu olarak kullanılan ilk kez Visual Studio'dan bir web sayfasını ziyaret edin. Oluşturduğumuz geri demo web uygulamaları [ *belirleme dosyaları gerekenler dağıtılabilir için* ](determining-what-files-need-to-be-deployed-cs.md) öğretici olan IIS kullanmak üzere yapılandırılmamış her iki dosya sistemi tabanlı web uygulamaları. Bu nedenle, bu Web sitelerini Visual Studio içinden birini ziyaret ASP.NET geliştirme sunucusu kullanılır.

Mükemmel bir dünyada geliştirme ve üretim ortamlarını aynı olacaktır. Önceki öğreticide ele aldığımız gibi ancak bu ortamlar için farklı yapılandırma ayarlarına sahip durumdur. Ortamlarda farklı bir web sunucusu yazılımı kullanarak, bir uygulama dağıtımı sırasında dikkate alınması gereken başka bir değişkeni ekler. Bu öğretici, IIS ve ASP.NET geliştirme sunucusu arasındaki önemli farklılıkları kapsar. Bu farklar nedeniyle burada geçitleriyle geliştirme ortamında çalışan kod bir özel durum oluşturur veya üretimde yürütüldüğünde farklı davranır senaryo vardır.

## <a name="security-context-differences"></a>Güvenlik bağlamı farkları

Web sunucusu yazılımı gelen istek işleme olduğunda bu isteği belirli güvenlik bağlamı ile ilişkilendirir. Bu güvenlik bağlamını, istek tarafından izin verilen eylemleri belirlemek için işletim sistemi tarafından kullanılır. Örneğin, bir ASP.NET sayfasında disk üzerindeki bir dosyaya ileti günlüğe kaydeden kod içerebilir. Hatasız yürütmek bu ASP.NET sayfası için güvenlik bağlamı dosya sistemi düzeyinde izinleri, yani bu dosyanın izinlerini yazma uygun olması gerekir.

ASP.NET Geliştirme Sunucusu gelen istekleri şu anda oturum açmış olan kullanıcının güvenlik bağlamı ile ilişkilendirir. Masaüstünüzün bir yönetici olarak oturum açtıysanız, ASP.NET geliştirme sunucusu tarafından sunulan ASP.NET sayfaları'nün bir yönetici olarak aynı erişim hakkına sahip olursunuz. Ancak, IIS tarafından işlenen ASP.NET isteklerini belirli bir makine hesabıyla ilişkilidir. Her müşteri için web ana bilgisayar sağlayıcınız benzersiz bir hesabı yapılandırmış olabilir ancak varsayılan olarak, ağ hizmeti makine hesabı sürüm 6 ve 7, IIS tarafından kullanılır. Web ana bilgisayar sağlayıcınız daha büyük olasılıkla bu makine hesabına sınırlı izinlere sağlamıştır. Geliştirme ortamındaki hatasız yürütün, ancak üretim ortamında barındırıldığında yetkilendirme ile ilgili olan özel durumlar oluşturmaya web sayfaları olabilir net sonucudur.

Bu tür ı oluşturduğunuz bir sayfaya en son tarih ve saat depolayan bir dosyayı rolündeyseniz Kitap incelemeleri Web sitesinde uygulamalı olarak göstermek için görüntülenen *öğretin kendiniz ASP.NET 3.5 24 saat içindeki* gözden geçirin. Örneği takip etmek için açık `~/Tech/TYASP35.aspx` sayfasında ve aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText` Yöntemi](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) yok ve belirtilen içerikleri yazar, yeni bir dosya oluşturur. Dosya zaten varsa, onun varolan içeriğin üzerine yazılır.


Ardından, ziyaret *öğretin kendiniz ASP.NET 3.5 24 saat içindeki* ASP.NET geliştirme sunucusu kullanarak geliştirme ortamında kitap İncele sayfası. Oturum varsayarak bilgisayarınızı oluşturmak ve bir metin dosyasına web değiştirmek için yeterli izinlere sahip bir hesapla oturum açın uygulamanın kök dizini kitap gözden önceki ile aynı görünür, ancak sayfa her zaman tarih ve saat ve kullanıcının ziyaret  IP adresi depolanan `LastTYASP35Access.txt` dosya. Tarayıcınız bu dosyayı işaret; Şekil 1'de gösterilene benzer bir ileti görürsünüz.


[![Son tarih ve saat kitap gözden edilmedi metin dosyası içerir.](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Şekil 1**: metin dosyası son tarih ve saat kitap gözden ziyaret içerir ([tam boyutlu görüntüyü görmek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


Üretim web uygulamasını dağıtma ve barındırılan ziyaret edip *öğretin kendiniz ASP.NET 3.5 24 saat içindeki* kitap İncele sayfası. Bu noktada normal veya Şekil 2'de gösterilen hata iletisi olarak ya da kitap gözden geçir sayfasıyla karşılaşırsınız. Bazı web ana bilgisayar sağlayıcıları çalışması sayfa hatasız çalışır anonim ASP.NET makine hesabı için yazma izinleri verin. Ancak, yazma erişimi anonim hesap için web ana bilgisayar sağlayıcınız yasaklar, sonra bir [ `UnauthorizedAccessException` özel durum](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) ne zaman tetiklenir `TYASP35.aspx` sayfası geçerli tarih ve saat için yazma girişimlerini `LastTYASP35Access.txt` dosya.


[![IIS tarafından kullanılan varsayılan makine hesabı dosya sistemine yazma iznine sahip değil](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Şekil 2**: varsayılan makine hesabı tarafından kullanılan IIS mu sahip izinlerin değil dosya sistemine yazma ([tam boyutlu görüntüyü görmek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


Güzel bir haberimiz var çoğu web ana bilgisayar sağlayıcıları siteniz dosya sistemi izinleri belirtmenize olanak tanır izinler aracı çeşit olmasıdır. Kök dizinine anonim ASP.NET hesabı yazma erişimi verin ve ardından kitap İncele sayfası yeniden ziyaret. (Gerekirse, web ana bilgisayar sağlayıcınıza varsayılan ASP.NET hesabı için yazma izinleri verme konusunda yardım almak için başvurun.) Sayfa hatasız yük, bu süre ve `LastTYASP35Access.txt` dosya başarılı bir şekilde oluşturulmalıdır.

Sınav zamanı hemen İşte ASP.NET Geliştirme Sunucusu IIS daha farklı güvenlik bağlamı altında çalışır olduğundan, onu okuma veya dosya sistemine yazma, ASP.NET sayfaları okuma veya Windows olay günlüğüne yazma veya okuma veya yazma için Windows olduğunu  kayıt defteri geliştirmeye beklendiği gibi çalışmayabilir ancak üretim sırasında özel durum oluşturur. Bir web uygulaması oluşturma, bir paylaşılan web barındırma ortamı dağıtılacak olduğunda veya onlara yazmadıklarından olay günlüğü veya Windows kayıt defteri yazma. Ayrıca, okuma veya okuma izni ve üretim ortamında uygun klasörlere ayrıcalıkları yazmak gereken dosya sistemine yazma herhangi bir ASP.NET sayfaları not alın.

## <a name="differences-on-serving-static-content"></a>Statik içerik sunan üzerinde farkları

Başka bir çekirdek IIS ile ASP.NET geliştirme sunucusu arasındaki statik içerik isteklerini nasıl işledikleri farktır. ASP.NET sayfası, resim veya bir JavaScript dosyası olup olmadığını ASP.NET geliştirme sunucusu içinde gelen her istek ASP.NET çalışma zamanı tarafından işlenir. Varsayılan olarak, bir istek, bir ASP.NET web sayfası, Web hizmeti ve diğerleri gibi ASP.NET kaynağa ait geldiğinde IIS ASP.NET çalışma zamanı yalnızca çağırır. Statik içerik - görüntüler, CSS dosyaları, JavaScript dosyaları, PDF dosyaları, ZIP dosyaları ve benzeri - istekleri IIS tarafından ASP.NET çalışma zamanı katılımı alınır. (Bu statik içerik sunan ASP.NET çalışma zamanı ile çalışmak için IIS istemek mümkündür; daha fazla bilgi için bu öğreticideki "Form tabanlı kimlik doğrulaması gerçekleştirme ve statik dosyalar ile IIS 7 üzerinde URL'yi kimlik doğrulaması" bölümüne bakın.)

ASP.NET çalışma zamanı çeşitli (istek sahibine tanımlayan) kimlik doğrulaması ve yetkilendirme (istek sahibine talep edilen içeriği görüntüleme izni varsa belirleme) dahil olmak üzere, talep edilen içeriği oluşturmak için adımları gerçekleştirir. Popüler kimlik doğrulama biçimidir *form tabanlı kimlik doğrulaması*, içinde bir web sayfasında metin kutuları içinde - genellikle bir kullanıcı adı ve parola - kimlik bilgilerini girerek bir kullanıcı, tanımlanır. Web sitesi depolar, kimlik bilgilerini doğrulama sırasında bir *kimlik doğrulaması bileti* sonraki her istekle Web sitesine gönderilen ve kullanıcının kimliğini doğrulamak için kullanılan kullanıcının tarayıcı tanımlama bilgisinde. Ayrıca, belirlemek mümkün *URL yetkilendirmesi* hangi kullanıcıların dikte kuralları olabilir veya belirli bir klasöre erişilemiyor. Birçok ASP.NET Web sitesi form tabanlı kimlik doğrulaması ve yetkilendirme URL'si kullanıcı hesaplarını destekler ve yalnızca kimliği doğrulanmış kullanıcılara veya belirli bir role ait kullanıcılar için erişilebilir olan site kısımları tanımlamak için kullanın.

> [!NOTE]
> ASP kapsamlı incelenmesi için. NET form tabanlı kimlik doğrulaması, URL yetkilendirmesi ve hesabı ile ilgili diğer kullanıcı özelliklerini atın my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Bir Web sitesi form tabanlı yetkilendirme kullanarak kullanıcı hesaplarını destekler ve kullanmakta, URL yetkilendirmesi, yalnızca kimliği doğrulanmış kullanıcılara izin verecek şekilde yapılandırılmış bir klasöre göz önünde bulundurun. Bu klasör, ASP.NET sayfaları içerir ve bu PDF dosyaları, PDF dosyaları ve amacı, kullanıcıların yalnızca kimliği doğrulanmış olduğunu görüntüleyebilirsiniz hayal edin.

Bu PDF dosyalardan biri doğrudan kendi tarayıcının adres çubuğuna URL'yi girerek görüntülemek bir ziyaretçi çalışırsa ne olur? Öğrenmek için şimdi Kitap incelemeleri sitede yeni bir klasör oluşturun, bazı PDF dosyalarını ekleyin ve siteyi URL yetkilendirmesi, anonim kullanıcılara bu klasör ziyaret etmenizi yasaklamak için kullanmak üzere yapılandırın. Demo uygulamayı indirme, adlı bir klasör oluşturduğum görürsünüz `PrivateDocs` ve PDF'den eklenen my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (nasıl sığdırma!). `PrivateDocs` Klasörünü de içeren bir `Web.config` anonim kullanıcı reddi sayısı için URL yetkilendirme kurallarını belirten dosyası:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Son olarak, form tabanlı kimlik doğrulaması güncelleştirerek kullanmak için web uygulaması yapılandırdım `Web.config` kök dizinindeki değiştirme:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

İle:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

ASP.NET geliştirme sunucusu kullanarak, siteyi ziyaret ederek doğrudan URL'sini tarayıcınızın adres çubuğuna PDF dosyaları birine girin. URL aşağıdakine benzer olmalıdır, Bu öğretici ile ilişkili Web sitesini indirdiyseniz: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Bu URL adres çubuğuna girerek ASP.NET Geliştirme Sunucusu dosyası için bir istek göndermek tarayıcı neden olur. ASP.NET Geliştirme Sunucusu bire bir izin isteği işlemek için ASP.NET çalışma zamanı. Biz henüz oturum açmamış olan olduğundan ve çünkü `Web.config` içinde `PrivateDocs` klasör anonim erişimi engellemek üzere yapılandırılmış, oturum açma sayfasına, bize otomatik olarak ASP.NET çalışma zamanı yönlendiren `Login.aspx` (bkz: Şekil 3). Kullanıcı oturum açma sayfasında için yönlendirirken, ASP.NET içeren bir `ReturnUrl` olan kullanıcı sayfasını gösteren querystring parametresi çalışırken görüntülemek. Kullanıcı başarıyla oturum sonra bu sayfaya geri döndürülebilir.


[![Otomatik olarak yeniden yönlendirilen oturum açma sayfasına yetkisiz kullanıcılardır](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Şekil 3**: yetkisiz kullanıcıların olduğundan otomatik olarak yeniden yönlendirilen oturum açma sayfasına ([tam boyutlu görüntüyü görmek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


Artık bu üretim nasıl davranacağını görelim. Uygulamanızı dağıtmak ve PDF'ler birine doğrudan URL'sini `PrivateDocs` üretimde klasör. Bu dosya için IIS istek göndermek için tarayıcınızı ister. Statik dosya istediğinden IIS alır ve ASP.NET çalışma zamanı çağırmadan dosyayı döndürür. Sonuç olarak, gerçekleştirilen herhangi bir URL yetkilendirme denetim oluştu; kullanılmasından özel PDF içeriğini, dosyayı doğrudan URL bilen herkes tarafından erişilebilir.


[![Anonim kullanıcılar dosyayı doğrudan URL girerek özel PDF dosyalarını indirebilirsiniz](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Şekil 4**: anonim kullanıcıların indirebileceği özel PDF dosyaları tarafından girerek doğrudan URL dosyasına ([tam boyutlu görüntüyü görmek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Statik dosyalar ile IIS 7 form tabanlı kimlik doğrulaması ve URL kimlik doğrulaması gerçekleştirme

Statik içeriği yetkisiz kullanıcılara karşı korumak için kullanabileceğiniz teknikleri birkaç vardır. IIS 7 sunulan *tümleşik ardışık*, IIS ASP.NET çalışma zamanı iş akışı iş akışıyla evlenir. Buna koysalar IIS ASP.NET çalışma zamanının kimlik doğrulaması ve yetkilendirme modülleri (PDF dosyaları gibi statik içerik dahil) gelen tüm istekleri çağrılacak bildirebilirsiniz. Tümleşik ardışık düzende kullanmak için Web sitesi yapılandırma konusunda bilgi edinmek için web ana bilgisayar sağlayıcınıza başvurun.

IIS kullanmak üzere yapılandırılmış bir kez tümleşik ardışık eklemek için aşağıdaki biçimlendirme `Web.config` dosyası kök dizini:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Bu işaretleme, IIS 7'in ASP.NET tabanlı kimlik doğrulaması ve yetkilendirme modüllerini bildirir. Uygulamanızı yeniden dağıtın ve ardından PDF dosyasını yeniden ziyaret edin. Şu olduğunda IIS isteği işleyen ASP.NET çalışma zamanının kimlik doğrulaması ve yetkilendirme mantığının istek İnceleme fırsatı verir. Yalnızca kimliği doğrulanmış kullanıcılar içerikleri görüntüleme yetkiniz olduğundan `PrivateDocs` klasörü, anonim ziyaretçi otomatik olarak oturum açma sayfasına yeniden yönlendirilmiş (Şekil 3'e yeniden bakın).

> [!NOTE]
> Web ana bilgisayar sağlayıcınız IIS 6 kullanıyorsa, tümleşik ardışık özelliğini kullanamazsınız. Bir çözüm olan özel belgelerinizi HTTP erişimini engelleyen bir klasöre koymak (gibi `App_Data`) ve ardından bu belgeleri sunmak için bir sayfa oluşturun. Bu sayfa adında `GetPDF.aspx`ve bir sorgu dizesi parametresi aracılığıyla PDF adını geçirilir. `GetPDF.aspx` Sayfa ilk kullanıcı dosyayı görüntülemek için izne sahip ve bu durumda, kullanacağınız emin olun, [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) istenen PDF dosyasının içeriğini istekte bulunan istemciye geri göndermek için yöntemi. Tümleşik ardışık düzende etkinleştirmek istediğiniz değil, bu yöntem Ayrıca IIS 7 için işe yarar.


## <a name="summary"></a>Özet

Bir üretim ortamında Web uygulamaları, Microsoft IIS web sunucusu yazılımı kullanılarak buluta barındırılır. Geliştirme ortamında, ancak IIS ya da ASP.NET geliştirme sunucusu kullanarak uygulama barındırılabilir. İdeal olarak, farklı yazılım kullanarak başka bir değişkene karışımında eklediğinden aynı web sunucusu yazılımı hem ortamlarında kullanılmalıdır. Ancak, kullanım kolaylığı, ASP.NET Geliştirme Sunucusu yayınlayabilirsiniz geliştirme ortamında kolaylaştırır. Yalnızca IIS ve ASP.NET geliştirme sunucusu arasındaki bazı temel farklar vardır ve bu farkların farkında olması durumunda uygulama çalışır ve aynı sağlamaya yardımcı olmak için adımları şekilde bakılmaksızın alabilir güzel bir haberimiz var olan ortam.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS 7.0 ile ASP.NET tümleştirmesi](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [ASP.NET forumları kimlik doğrulaması, IIS 7 üzerinde içerik tüm türleri kullanarak](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Visual Web Developer'da Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Önceki](common-configuration-differences-between-development-and-production-cs.md)
> [İleri](deploying-a-database-cs.md)
