---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
title: Çekirdek IIS ve ASP.NET Geliştirme Sunucusu (C#) arasındaki farklar | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET uygulamasını yerel olarak test edilirken ASP.NET Geliştirme Web sunucusu kullanıyorsanız yüksektir. Ancak, büyük olasılıkla pow üretim Web sitesidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 13a5a423-9235-4dde-b408-2fd10f791d63
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs
msc.type: authoredcontent
ms.openlocfilehash: e343a6eac39d7959718cb791012cfa3b931ae33f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-c"></a>IIS ve ASP.NET Geliştirme Sunucusu (C#) arasındaki temel farklar
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_cs.pdf)

> Bir ASP.NET uygulamasını yerel olarak test edilirken ASP.NET Geliştirme Web sunucusu kullanıyorsanız yüksektir. Ancak, büyük olasılıkla güç beslemeli IIS üretim Web sitesidir. Bu web sunucuları istekleri nasıl işleneceğini arasındaki bazı farklar vardır ve bu farklılıklar önemli sonuçlar doğurabilir. Bu öğreticide daha başlığıyla ilgili farklılıklar araştırır.


## <a name="introduction"></a>Giriş

Bir ASP.NET uygulamasını kullanıcı ziyaret her kendi tarayıcı Web sitesine bir isteği gönderir. Bu istek oluşturmak ve istenen kaynak içeriğini döndürmek için bir ASP.NET çalışma zamanı koordinatları web sunucusu yazılımı tarafından toplanma. [**I** nternet **ı** ilgi **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) ortak Internet tabanlı işlevsellik için sağladığı hizmetler dizisi olan Windows sunucuları. IIS, ASP.NET uygulamalarının üretim ortamlarında en yaygın kullanılan web sunucusudur; Bu büyük olasılıkla ASP.NET uygulamanıza hizmet edecek şekilde web ana bilgisayar sağlayıcınız tarafından kullanılan web sunucusu yazılımıdır. IIS de kullanılabilir web sunucusu yazılım geliştirme ortamında olarak bu IIS yükleme kapsar rağmen ve düzgün şekilde yapılandırma.


ASP.NET Geliştirme Sunucusu bir geliştirme ortamı için alternatif web sunucusu seçeneğidir; ile birlikte ve Visual Studio'ya tümleşiktir. Web uygulamasını IIS kullanmak üzere yapılandırılmış sürece, ASP.NET Geliştirme Sunucusu otomatik olarak başlatıldı ve ilk Visual Studio'dan bir web sayfasından ziyaret ettiğinizde web sunucusu olarak kullanılır. Oluşturduğumuz geri demo web uygulamalarına [ *belirleme dosyaları gerekenler dağıtılacağını için* ](determining-what-files-need-to-be-deployed-cs.md) öğretici olan IIS kullanmak üzere yapılandırılmamış hem dosya sistemi tabanlı web uygulamaları. Bu nedenle, Visual Studio içinde bu Web sitelerinden birini ziyaret eden ASP.NET geliştirme sunucusu kullanılır.

İdeal şartlar altında geliştirme ve üretim ortamlarını aynı olacaktır. Biz önceki öğreticide açıklandığı gibi ancak, farklı yapılandırma ayarlarını sağlamak ortamları için seyrek değil. Farklı bir web sunucusu yazılım ortamlarda kullanarak bir uygulama dağıtırken dikkate alınması gereken başka bir değişken ekler. Bu öğretici, IIS ve ASP.NET geliştirme sunucusu arasındaki önemli farklılıkları kapsar. Bu farklılıklar nedeniyle flawlessly geliştirme ortamında çalışan kod bir özel durum oluşturur veya üretimde çalıştırıldığında farklı şekilde davranan olduğu senaryolar vardır.

## <a name="security-context-differences"></a>Güvenlik bağlamı farklar

Web sunucusu yazılımı gelen istek işleme olduğunda bu isteği belirli güvenlik bağlamı ile ilişkilendirir. Bu güvenlik bağlamı bilgilerini işletim sistemi tarafından hangi eylemleri izin verilen istek tarafından belirlemek için kullanılır. Örneğin, bir ASP.NET sayfasının bazı ileti disk üzerindeki bir dosyaya kaydeder kod içerebilir. Hatasız yürütmek bu ASP.NET sayfası, güvenlik bağlamı uygun dosya sistemi düzeyi izinleri, yani bu dosyanın izinlerini yazma olması gerekir.

ASP.NET Geliştirme Sunucusu gelen istekleri şu anda oturum açmış olan kullanıcının güvenlik bağlamı ile ilişkilendirir. Masaüstünüzü yönetici olarak oturum açtıysanız, ASP.NET geliştirme sunucusu tarafından sunulan ASP.NET sayfaları'nün bir yönetici olarak aynı erişim haklarına sahip olur. Bununla birlikte, IIS tarafından işlenen ASP.NET isteklerini belirli bir makine hesabıyla ilişkilendirilmiş. Her müşteri için web ana makine sağlayıcısı benzersiz bir hesabı yapılandırmış olabilir ancak varsayılan olarak, ağ hizmeti makine hesabı sürüm 6 ve 7, IIS tarafından kullanılır. Daha web ana bilgisayar sağlayıcınızın büyük olasılıkla bu makine hesabının sınırlı izin verdiği. Geliştirme ortamında hatasız yürütün, ancak üretim ortamında barındırıldığında yetkilendirmeyle ilgili özel durum oluşturmak web sayfaları olabilir net sonucudur.

Bu tür hataların en son tarih ve saat depoladığı disk üzerindeki bir dosya birinin oluşturduğu Kitap incelemeleri Web sitesi bir sayfa oluşturdum uygulamalı olarak göstermek için görüntülenen *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki* gözden geçirin. Takip açmak `~/Tech/TYASP35.aspx` sayfasında ve aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:

[!code-csharp[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample1.cs)]

> [!NOTE]
> [ `File.WriteAllText` Yöntemi](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) yok ve belirtilen içerikleri yazar varsa yeni bir dosya oluşturur. Dosya zaten mevcutsa, var olan içeriğin üzerine yazılır.


Ardından, ziyaret *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki* defteri gözden geçirme sayfasını ASP.NET geliştirme sunucusu kullanarak geliştirme ortamında. Oturum açmış varsayılarak bilgisayarınızı oluşturmak ve bir metin dosyasına web değiştirmek için yeterli izinlere sahip bir hesapla uygulamanızın kök dizininde kitap gözden geçirme önceki ile aynı görünür, ancak sayfa her zaman tarih ve saat ve kullanıcının ziyaret  IP adresi depolanır `LastTYASP35Access.txt` dosya. Tarayıcınız bu dosyaya işaret; Şekil 1'de gösterilene benzer bir ileti görürsünüz.


[![Son tarih ve saat kitap gözden geçirme ziyaret metin dosyasını içerir](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image1.png)

**Şekil 1**: metin dosyası son tarih ve saat kitap gözden geçirme ziyaret içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image3.png))


Üretim web uygulamasına dağıtmak ve barındırılan ziyaret *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki* defteri İnceleme sayfası. Bu noktada normal veya Şekil 2'de gösterilen hata iletisi olarak ya da kitap gözden geçirme sayfasını görürsünüz. Bazı web ana bilgisayar sağlayıcıları durumda sayfa hatasız çalışır anonim ASP.NET makine hesabı yazma izinleri verin. Ancak, web ana bilgisayar sağlayıcınız anonim hesap için yazma erişimi engeller, sonra bir [ `UnauthorizedAccessException` özel durum](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) zaman tetiklenir `TYASP35.aspx` sayfa geçerli tarih ve saat için yazma girişiminde `LastTYASP35Access.txt` dosya.


[![IIS tarafından kullanılan varsayılan makine hesabı dosya sistemine yazma iznine sahip değil](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image4.png)

**Şekil 2**: varsayılan makine hesabı tarafından kullanılan IIS mu değil izinleri dosya sistemine yazma ([tam boyutlu görüntüyü görüntülemek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image6.png))


İyi haber çoğu web ana bilgisayar sağlayıcıları, Web sitenizdeki dosya sistemi izinleri belirtmenize olanak tanır izinler aracı çeşit olmasıdır. Kitap gözden geçirme sayfasını yeniden ziyaret ve kök dizinine anonim ASP.NET hesabı yazma erişimi verin. (Gerekirse, web ana makine sağlayıcısı varsayılan ASP.NET hesabını yazma izinleri konusunda yardım için başvurun.) Sayfanın hata yük bu süre ve `LastTYASP35Access.txt` dosya başarılı bir şekilde oluşturulmalıdır.

Take hemen İşte ASP.NET Geliştirme Sunucusu IIS daha farklı güvenlik bağlamı altında çalıştığından, okuma veya dosya sistemine yazma, ASP.NET sayfaları okuma veya Windows olay günlüğüne yazma veya okuma veya Windows yazma olduğunu mümkündür  kayıt defteri geliştirmeye beklendiği gibi çalışır ancak üretim olduğunda özel durumları oluşturur. Bir web uygulaması oluşturma bir paylaşılan web barındırma ortamına dağıtılır, okumaz veya olay günlüğü veya Windows kayıt defteri yazma. Ayrıca, okuma veya okuma izni ve ilgili klasörleri ayrıcalıklarına üretim ortamında yazma gerekebilir gibi dosya sistemine yazma ASP.NET sayfaları not edin.

## <a name="differences-on-serving-static-content"></a>Statik içerik sunan üzerinde farklar

Başka bir çekirdek IIS ve ASP.NET geliştirme sunucusu arasında nasıl bunlar statik içerik için istekleri işleyecek farktır. ASP.NET Geliştirme Sunucusu bir ASP.NET sayfası, bir görüntü veya bir JavaScript dosyası olup olmadığını gelen her istek ASP.NET çalışma zamanı tarafından işlenir. Bir istek için bir ASP.NET web sayfası, Web hizmeti ve diğerleri gibi bir ASP.NET kaynağı geldiğinde varsayılan olarak, IIS ASP.NET çalışma zamanı yalnızca çağırır. Statik içerik - görüntüleri, CSS dosyaları, JavaScript dosyaları, PDF dosyaları, ZIP dosyaları ve benzeri - istekleri IIS tarafından ASP.NET çalışma zamanı katılımı alınır. (Bunu statik içerik sunan ASP.NET çalışma zamanı ile çalışmak için IIS istemeniz mümkündür; Bu öğreticide daha fazla bilgi için "Gerçekleştirme form tabanlı kimlik doğrulaması ve IIS 7 statik dosyaları URL kimlik doğrulaması" bölümüne bakın.)

ASP.NET çalışma zamanı çeşitli (istek sahibinin tanımlayan) kimlik doğrulama ve yetkilendirme (istek sahibinin istenen içeriği görüntüleme izni olup olmadığını belirleme) gibi istenen içeriği oluşturmak için adımları gerçekleştirir. Yaygın olarak kullanılan kimlik doğrulama biçimidir *form tabanlı kimlik doğrulama*, içinde bir web sayfasında metin kutuları - genellikle bir kullanıcı adı ve parola - kimlik bilgilerini girerek bir kullanıcı tanımlanır. Web sitesi kimlik bilgilerini doğrulama sırasında depolar bir *kimlik doğrulaması bileti* sonraki her istek ile Web sitesine gönderilen ve kullanıcının kimliğini doğrulamak için kullanılan kullanıcı tarayıcının tanımlama bilgisi. Ayrıca, belirlemek mümkün *URL yetkilendirmesi* hangi kullanıcıların dikte kuralları olabilir veya belirli bir klasöre erişilemiyor. Birçok ASP.NET Web sitesi form tabanlı kimlik doğrulama ve URL yetkilendirmesi kullanıcı hesapları desteklemek ve kimliği doğrulanmış kullanıcılara veya belirli bir role ait kullanıcılar yalnızca erişilebilir site bölümlerini tanımlamak için kullanın.

> [!NOTE]
> ASP kapsamlı incelenmesi için. NET'in form tabanlı kimlik doğrulaması, URL yetkilendirme ve hesap ile ilgili diğer kullanıcı özellikleri, kullanıma emin my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Kullanıcı hesaplarını kullanarak form tabanlı yetkilendirmeyi destekler ve kullanmakta, URL yetkilendirmesi, yalnızca kimliği doğrulanmış kullanıcılara izin verecek şekilde yapılandırılmış bir klasör olan bir Web sitesi göz önünde bulundurun. Bu klasör, ASP.NET sayfaları içerir ve PDF dosyaları ve amacı, yalnızca kimliği doğrulanmış kullanıcılar ise bu PDF dosyaları görüntüleyebilirsiniz düşünün.

Bu PDF dosyaları birini doğrudan kendi tarayıcınızın adres çubuğuna URL'yi girerek görüntülemek bir ziyaretçi denemesi olursa ne olur? Öğrenmek için şimdi Kitap incelemeleri sitede yeni bir klasör oluşturun, bazı PDF dosyalarını ekleyin ve siteyi anonim kullanıcıların bu klasör ziyaret engellemek için URL yetkilendirmesi kullanacak şekilde yapılandırın. Demo uygulamayı karşıdan yüklemeniz halinde adlı bir klasör oluşturduğunuz görürsünüz `PrivateDocs` ve bir PDF eklenen my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (nasıl sığdırma!). `PrivateDocs` Klasörü de içeren bir `Web.config` anonim kullanıcılar reddetmek için URL yetkilendirme kuralları belirten dosyası:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample2.xml)]

Son olarak, ı güncelleştirerek form tabanlı kimlik doğrulaması kullanmak için web uygulaması yapılandırılmış `Web.config` kök dizindeki dosyayı değiştirme:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample3.xml)]

İle:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample4.xml)]

ASP.NET geliştirme sunucusu kullanarak sitesini ziyaret edin ve PDF dosyalarını tarayıcınızın adres çubuğuna birine doğrudan URL'yi girin. URL gibi görünmelidir Bu öğretici ile ilişkili Web sitesi yüklediyseniz: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Bu URL adres çubuğuna girerek ASP.NET Geliştirme Sunucusu dosyası için bir istek göndermek tarayıcı neden olur. İstek işleme için ASP.NET çalışma zamanı için ASP.NET Geliştirme Sunucusu ellerini. Biz henüz oturum açmamış olan olduğundan ve çünkü `Web.config` içinde `PrivateDocs` klasör anonim erişimi reddetmek üzere yapılandırıldı, ASP.NET çalışma zamanı otomatik olarak bize oturum açma sayfasına yeniden yönlendirir `Login.aspx` (bkz: Şekil 3). Kullanıcı oturum açma sayfasında için yönlendirirken ASP.NET içeren bir `ReturnUrl` olan kullanıcı sayfasını gösteren sorgu dizesi parametresi çalışırken görüntülemek. Kullanıcı başarıyla oturum sonra bu sayfaya geri döndürülebilir.


[![Yetkisiz kullanıcıların otomatik olarak yeniden yönlendirilen oturum açma sayfası olan](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image7.png)

**Şekil 3**: yetkisiz kullanıcıların olan otomatik olarak yeniden yönlendirilen oturum açma sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image9.png))


Şimdi bu üretimde nasıl davranacağını görelim. Uygulamanızı dağıtmak ve PDF birine doğrudan URL'yi girin `PrivateDocs` üretim klasöründe. Bu dosya için IIS istek göndermek için tarayıcınızı ister. Statik bir dosya istediğinden IIS alır ve ASP.NET çalışma zamanı çağırmadan dosyayı döndürür. Sonuç olarak, gerçekleştirilen URL yetkilendirme Denetimsiz oluştu; beklendiği gibi özel PDF içeriğini dosyaya doğrudan URL'yi bilen herkes tarafından erişilebilir.


[![Anonim kullanıcılar dosyaya doğrudan URL'yi girerek özel PDF dosyalarını indirebilirsiniz](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image10.png)

**Şekil 4**: anonim kullanıcılar indirebilir özel PDF dosyaları tarafından girme doğrudan URL dosyasına ([tam boyutlu görüntüyü görüntülemek için tıklatın](core-differences-between-iis-and-the-asp-net-development-server-cs/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>IIS 7 statik dosyaları form tabanlı kimlik doğrulaması ve URL kimlik gerçekleştiriliyor

Statik içerik yetkisiz kullanıcılara karşı korumak için kullanabileceğiniz teknikler birkaç vardır. IIS 7 sunulan *tümleşik ardışık*, IIS ASP.NET çalışma zamanındaki iş akışı iş akışıyla evlenir. Buna koysalar ASP.NET zamanının kimlik doğrulama ve yetkilendirme modülleri (PDF dosyaları gibi statik içerik dahil) gelen tüm istekleri çağırmak için IIS söyleyebilirsiniz. Tümleşik ardışık kullanmak için Web sitenizi yapılandırmak nasıl öğrenmek için web ana bilgisayar sağlayıcınıza başvurun.

IIS kullanmak üzere yapılandırıldıktan sonra tümleşik ardışık eklemek için aşağıdaki biçimlendirme `Web.config` kök dizindeki dosyayı:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-cs/samples/sample5.xml)]

Bu biçimlendirme IIS 7 ASP.NET tabanlı kimlik doğrulama ve yetkilendirme modülleri kullanılacağını söyler. Uygulamanızı yeniden dağıtmanıza ve PDF dosyasını yeniden ziyaret edin. Zaman IIS isteği işleyen bu süre ASP.NET zamanının kimlik doğrulama ve yetkilendirme mantığı istek İnceleme fırsatı sunar. Yalnızca kimliği doğrulanan kullanıcılar içeriği görüntüleme yetkiniz çünkü `PrivateDocs` klasörü, anonim ziyaretçi otomatik olarak oturum açma sayfasına yeniden yönlendirildiği (Şekil 3'e yeniden bakın).

> [!NOTE]
> IIS 6, web ana makine sağlayıcısı hala kullanıyorsanız, tümleşik ardışık özelliği kullanamazsınız. Bir geçici bir çözüm değildir, özel HTTP erişimi engelliyor bir klasörde koyulamıyor (gibi `App_Data`) ve sonra bu belgeleri sunmak için bir sayfa oluşturulur. Bu sayfayı adlı `GetPDF.aspx`ve bir sorgu dizesi parametresi aracılığıyla PDF adını geçirilir. `GetPDF.aspx` Sayfasında ilk doğrulayın kullanıcı dosyayı görüntülemek için izni olduğundan ve bu durumda, kullanacağınız emin, [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) istenen PDF dosyasının içeriğini istekte bulunan istemciye geri gönderilecek yöntemi. Tümleşik ardışık etkinleştirmek istediğiniz değil, bu yöntem aynı zamanda IIS 7 için çalışır.


## <a name="summary"></a>Özet

Bir üretim ortamında Web uygulamaları, Microsoft IIS web sunucusu yazılımı kullanılarak barındırılır. Geliştirme ortamında, IIS veya ASP.NET geliştirme sunucusu kullanarak uygulama ancak barındırılabilir. İdeal olarak, farklı yazılım kullanarak başka bir değişken karışımında eklediğinden aynı web sunucusu yazılımı hem ortamlarda kullanılmalıdır. Ancak, ASP.NET Geliştirme Sunucusu, kullanım kolaylığı yayınlayabilirsiniz geliştirme ortamında kolaylaştırır. İyi haber yalnızca IIS ve ASP.NET geliştirme sunucusu arasındaki bazı temel farklar vardır ve bu farkların bilincinde varsa uygulamanın çalışır ve aynı işlevleri sağlamaya yardımcı olmak için adımları biçimi ne olursa olsun, alabilir olduğu ortam.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS 7.0 ile ASP.NET tümleştirme](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Tüm IIS 7 üzerinde içerik türlerini ASP.NET forumları kimlik doğrulaması kullanarak](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (Video)
- [Visual Web Developer Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Önceki](common-configuration-differences-between-development-and-production-cs.md)
> [sonraki](deploying-a-database-cs.md)
