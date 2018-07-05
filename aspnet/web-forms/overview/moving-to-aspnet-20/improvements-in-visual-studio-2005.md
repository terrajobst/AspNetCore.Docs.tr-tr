---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Visual Studio 2005 geliştirmeleri | Microsoft Docs
author: microsoft
description: Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 35ebb6e94c3da105b802c843e36f193f81a45782
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397645"
---
<a name="improvements-in-visual-studio-2005"></a>Visual Studio 2005 geliştirmeleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar.


Visual Studio 2005 iyileştirmeler ve geliştirmeler Web projeleri için uzun bir liste ile Web uygulaması geliştiricileri sağlar. Güçlü Visual Studio .NET 2002 ve 2003 olduğu gibi vardı birçok şikayetlerinin şekilde Web projeleri işlendi. Visual Studio 2005 bu şikayetlerinin değinmek için çok sayıda yeni özellikleri ekler. Kişiler için Visual Studio .NET 2003 Web uygulamalarının derlenmesi işlenme tercih, bkz: [Web Uygulama projeleri](https://go.microsoft.com/fwlink/?LinkId=57870).

Bu modülde, Web projesi oluşturma, yönetim ve geliştirme iyileştirmeleri de içerir. Daha sonra bir modülde, Web projeleri oluşturmak ve bunları dağıtmaya yönelik geliştirmeler de kapsar.

## <a name="frontpage-server-extensions"></a>FrontPage Server Extensions

Visual Studio .NET 2002 ve 2003 FrontPage Server Extensions kutusunda veya Web projeleri oluşturma için gereklidir. Geliştiriciler sahip iki farklı erişim modları (FrontPage Server Extensions veya dosya erişim modu) arasında seçim yapma, FrontPage Server Extensions hem de uygulama kökü ayarlama, IIS, vb. gibi görevleri gerçekleştirmek için kullanılır.

FrontPage Server Extensions güvenme yerel projeleri için Visual Studio 2005 kaldırır. Visual Studio 2005 artık IIS metabase FrontPage Server Extensions kullanmak yerine doğrudan erişir. Visual Studio 2005 ayrıca FrontPage Server Extensions gerek kalmadan uzak proje erişimi sağlayan FTP için destek ekler.

Kendi projeleri FrontPage Server Extensions kullanmak istediğiniz geliştiriciler seçenek yine de kullanılabilir. Ancak, ASP.NET Geliştirici topluluğu güçlü görüşleri göre bu bir gereksinim değildir.

> [!NOTE]
> FrontPage Server Extensions uzak proje oluşturma, açma, vb. için hala gereklidir.


## <a name="aspnet-development-server"></a>ASP.NET Geliştirme Sunucusu

Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen yeni bir Web sunucusu ile birlikte gelir. (Bu Web sunucusu daha önce Cassini biliniyordu.)

ASP.NET Geliştirme Sunucusu, çeşitli avantajları vardır.

- Yönetici olmayanlar için geliştirme ve bir Web sunucusunda hata ayıklama artık mümkündür.
- ASP.NET Geliştirme Sunucusu herhangi bir yere sanal dizinler için esnek bir proje konumları sağlayan dosya sistemindeki dinamik olarak eşler.
- IIS zaten kullanan kullanıcılar Windows XP Professional, artık dosya veya klasör yapısı, kendi varsayılan Web sitesi IIS'de etkilemez yeni Web uygulamaları oluşturmak mümkün olacaktır.

ASP.NET Geliştirme Sunucusu yararlanmak için özel bir yapılandırma gerekir. Dosya sistemi üzerinde barındırılan bir Web projesi hata ayıklaması veya göz, Visual Studio 2005 ASP.NET Geliştirme Sunucusu örneği isteğe hizmet vermek için rastgele bir bağlantı noktası üzerinde otomatik olarak başlatın.

ASP.NET geliştirme Sunucusu'nda daha sonra bu modül hakkında daha fazla bilgi ele alınmaktadır.

## <a name="improved-file-management"></a>Gelişmiş dosya yönetimi

Visual Studio 2002 ve 2003'te bir proje dosyası (.vbproj VB.NET için) ve C# için .csproj Web uygulamasındaki tüm dosyalar bilgileri depolanır. Çözüm Gezgini görünen proje dosyasındaki dosya bilgilere dayanır. Bu nedenle, Çözüm Gezgini'nde genellikle hatalı bilgilerin dış düzenleyicileri kullanıldığı durumlarda görüntülenebilir. Visual Studio 2002 ve 2003 genellikle dosya değişiklikleri üzerine mi dosyaların en son sürümünü görüntüleme.

Visual Studio 2005 yerine proje dosyasıyla birlikte yapar. Bunun yerine, projenizdeki dosyaları doğru bir görünümünü výsledek doğrudan diskten dosya ve klasör bilgilerini okur. Visual Studio 2002 ve 2003'nde başvurular klasörünün Web uygulamanızda gerçek bir klasör göstermediğinden, Visual Studio 2005 başvuruları klasörü Çözüm Gezgini'nden de kaldırır. Visual Studio 2005 projeniz için başvuruları erişmek için proje için özellik sayfaları kullanmanız gerekir.

## <a name="creating-web-projects"></a>Web projeleri oluşturma

Web geliştiricileri, Visual Studio 2005'te proje oluşturmaya yönelik birçok yeni seçeneğiniz vardır. Web siteleri artık herhangi bir dosya sisteminde oluşturulabilir ve ardından ayıklanabilir veya yeni ASP.NET geliştirme sunucusu kullanarak göz. Geliştiriciler ayrıca FTP kullanarak yeni Web siteleri oluşturabilirsiniz.

Visual Studio 2005'te Web projeleri oluşturma videosu görüntülemek için buraya tıklayın.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Açık tam ekran görüntü](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Dosya sistemi projeleri

Video kılavuzda gördüğünüz gibi yerel makinede veya bir dosya paylaşımı aracılığıyla uzak bir konumda dosya sistemi Web siteleri oluşturmak seçebilirsiniz. Dosya sistemi üzerinde oluşturulan Web siteleri, taranan ve ASP.NET geliştirme sunucusu kullanarak görüntüde hata ayıklamayı.

> [!NOTE]
> ASP.NET Geliştirme Sunucusu müşteriler için karışıklığa neden olabilir. IISs dizin yapısına (örneğin c:/inetpub/wwwroot) dosya sistemindeki bir Web projesi oluşturduysanız, yine de Web sitesi ile ASP.NET geliştirme sunucusu içinde Visual Studio 2005 başlatıldığında taranmasına. Bu nedenle, herhangi bir IIS yapılandırması (yani, kimlik doğrulama yöntemleri) geçerli değil.


Varsayılan web projesi de çok kaldırır yük tarafından yalnızca bir Default.aspx sayfasında, default.cs dosya ve uygulama/_veri klasör içerir. Gerektiğinde özel klasör (yani uygulama/_kod) ve web.config eklenir. Web projenizi yalnızca gereksinim duyduğunuz klasör ve dosyaları içerir.

### <a name="http-projects"></a>HTTP projeleri

HTTP projeleri yerel IIS Web sitesinde veya bir uzak Web sitesinde oluşturulan projeleri ya da olabilir. Varsayılan proje konumu `http://localhost`. Göz at düğmesine tıklarsanız, HTTP iki seçenek vardır: Yerel IIS ve uzak Site. Bu iki seçenek de temel fark, web sitesi bilgilerin konumu seçin iletişim kutusunda ve dosyalar Web sunucusuna nasıl kopyalanır görüntülenir yöntemidir.

Yerel IIS seçeneği metatabanı yerel makinede site bilgilerini okur ve dosya sistemi kullanılarak dosyalar kopyalanır. Uzak Site seçeneğini FrontPage Server Extensions ve site bilgilerini kullanır ve HTTP kullanarak dosyalar kopyalanır ve FrontPage Server Extensions RPC çağırır.

> [!NOTE]
> Artık get/_aspx/_ver.aspx ve vs###/_tmp.htm dosya sürüm bilgisi belirlemek için kullanılır.


Varsayılan HTTP yerel IIS seçenektir. Bu seçenek, hangi siteleri kullanılabilir belirlemek için IIS metatabanı ve içerik oluşturulacağı konumu okur. Ağaç görünümünde seçerek farklı bir klasör veya sanal dizin seçebilirsiniz. Ayrıca yeni bir sanal dizin oluşturma, klasörleri uygulamalar olarak işaretlemek, yapabilir bu iletişim kutusundan mevcut sanal dizinleri silin.


![Konum iletişim seçin](improvements-in-visual-studio-2005/_static/image1.gif)

**Şekil 1**: konumu iletişim seçin


Farklı Visual Studio'nun, işaretlerseniz, önceki sürümlerde **Güvenli Yuva Katmanı kullan** , yaptığınız isteyen bir güvenlik uyarısı iletişim kutusu ile sunulan, onay ve SSL sertifikasını kullandığınız URL eşleşmiyor devam etmek istiyor. Eşleşen bir sertifika olmasaydı Visual Studio .NET 2003 kullanarak, proje oluşturma başarısız olur.


![Güvenlik Uyarısı ilgili SSL sertifikası](improvements-in-visual-studio-2005/_static/image2.gif)

**Şekil 2**: SSL sertifikası ile ilgili güvenlik uyarısı


### <a name="note-on-host-headers"></a>Konak üstbilgileri ilgili not

Bir sitede belirli bir IP için bağlı bir Web uygulaması oluşturuyorsanız, bir konak üstbilgisi yapılandırıldığından emin olmanız gerekir. Aksi takdirde, Visual Studio sitede oluşturacak `http://localhost`, ancak IP adresi doğru olduğunda site göz veya gelen ve IDE içinde hata ayıklama çözümlemez.

Uzak Site seçeneğini seçerseniz, yeni Web sitesi için hedef URL'sini olanak tanımak için iletişim değiştirir. Bu URL, FrontPage Server Extensions etkin olan bir sunucu üzerinde olması gerekir. FrontPage Server Extensions'ı kullanarak yerel Web sunucunuz ile çalışmak istiyorsanız, uzak Site seçeneğini kullanın ve yerel bir URL belirtin.


![Uzak bir sunucuda bir Web sitesi oluşturma](improvements-in-visual-studio-2005/_static/image1.jpg)

**Şekil 3**: Uzak bir sunucuda bir Web sitesi oluşturma


SSL sertifikası eşleşmiyorsa SSL üzerinden uzak bir sitede bir uygulama oluştururken, onay iletişim kutusunda yerel IIS seçeneği kullanılırken görüntülenen iletişim biraz farklıdır.


![Uzak Site Güvenlik Uyarısı](improvements-in-visual-studio-2005/_static/image3.gif)

**Şekil 4**: Uzak Site Güvenlik Uyarısı


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 FTP üzerinden Web siteleri oluşturmak için bu seçeneği sunar. Bu seçeneği kullandığınızda, IDE dosyaları yerel olarak kullanıcıların geçici klasörde oluşturur ve ardından dosyaları FTP konuma taşımak için FTP kullanır.

> [!NOTE]
> Geçici klasör konumu c:/belgeler ve ayarlar olduğu /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;sunucu&gt;/_&lt;uygulama adı&gt;


FTP seçeneği kullanılırken, bir konum seçin iletişim kutusu ile sunulur. Aşağıda gösterildiği gibi bu iletişim kutusunda, gerekli FTP bağlantı bilgilerini girin.


![FTP konumu iletişim seçin](improvements-in-visual-studio-2005/_static/image2.jpg)

**Şekil 5**: FTP konumu iletişim seçin


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratuvar: Kurulum FTP sitesi ve bir proje oluşturun

Bir kullanıcı yalnızca bunlar için FTP aracılığıyla karşıya yükleyebilecek bir konuma sahip olacak şekilde aşağıdaki adımları FTP sitesini yapılandırın.

### <a name="install-the-ftp-service"></a>FTP hizmetinin yükleme

1. Program Ekle/Kaldır'ı açın, Windows Bileşenlerini Ekle/Kaldır'ı seçin
2. Internet Information Services (Windows 2003'te uygulama sunucusu) seçin ve tıklayın **ayrıntıları**.
3. Denetleme **Dosya Aktarım Protokolü (FTP) hizmet** tıklatıp **Tamam**.
4. Tıklayın **sonraki** FTP hizmeti yüklemek için.

### <a name="create-a-new-folder-for-content"></a>Yeni bir klasör için içerik oluşturun

1. Windows Gezgini'nde, adlı yeni bir klasör oluşturma **User1** inetpub/c:/wwwroot içinde.

#### <a name="configure-folders-and-permissions-on-folders"></a>Klasörleri ve izinlerini klasörleri yapılandırın.

1. Internet Information Services ek bileşenini Yönetim Araçları'ndan açın. Bilgisayar adı düğümün altında bir FTP siteleri klasörünü şimdi gerekir.
2. Genişletin **FTP siteleri**.
3. Sağ **varsayılan FTP sitesi**seçin **yeni**, ardından **sanal dizin**, ardından **sonraki**.
4. Girin **User1** tıklayın ve sanal dizin adı için **sonraki**.
5. Girin **c:/inetpub/wwwroot/User1** tıklayın ve yolu için **sonraki**.
6. Tıklayın **sonraki** ardından **son** Sihirbazı tamamlayın.
7. Sağ **User1** varsayılan FTP sitesi ve select altında sanal dizin **özellikleri**.
8. Denetleme **yazma** onay kutusunu tıklatıp **Tamam** iletişim kutusunu kapatmak için.
9. Sağ **varsayılan FTP sitesi** seçip **özellikleri**.
10. Üzerinde **güvenlik hesapları** sekmesinde, onay kutusunu temizleyin **anonim bağlantılara izin ver**.
11. Tıklayın **Evet** devam etmek isteyip istemediğinizi soran iletişim kutusunda.
12. Tıklayın **Tamam** iletişim kutusunu kapatmak için.
13. Genişletin **varsayılan Web sitesi** altında **Web siteleri** düğümü.
14. Sağ **User1** dizin ve select **özellikleri**
15. İçinde **uygulama ayarları** bölümünde **Oluştur** klasörü bir uygulama olarak işaretlenecek.
16. Tıklayın **Tamam** iletişim kutusunu kapatmak için.
17. Internet Information Services eklentisini kapatın.

### <a name="create-web-project"></a>Web projesi oluşturma

1. Visual Studio 2005'i açın.
2. Gelen **dosya** menüsünde **yeni Web sitesi**.
3. İçinde **konumu** açılır menüsünde, select **FTP**.
4. **Gözat**'ı tıklatın.
5. Girin **localhost** içinde **sunucu** metin.
6. Girin **User1** dizin metin kutusuna.
7. Tıklayın **açık**. FTP konumu yeni Web sitesi iletişim kutusuna girilir.
8. **Tamam**'ı tıklatın.
9. Onay kutusunu temizleyin **anonim oturum açma** FTP oturum açma iletişim kutusunda, kimlik bilgilerinizi girin ve tıklayın **Tamam**.
10. Proje URL'si nedir? (Çözüm Gezgini'nde proje URL'sini görüntülenir.)
11. Gelen **derleme** menüsünde **derleme Web sitesi** veya **Çözümü Derle**.
12. Çözüm Gezgini içinde default.aspx öğesini sağ tıklatın ve seçin **tarayıcıda görüntüle**.
13. Web sitesi URL'si gerekli iletişim kutusuna girin `http://localhost/user1` tıklatın ve URL için **Tamam**.

> [!NOTE]
> Türü /_Default yüklenecek bağlantı kurma sorunu olduğunu belirten bir hata alırsanız, Web siteniz ve daha önceki bir sürümü üzerinde ASP.NET 2.0 çalıştığından emin olun. Internet Information Services'ın ASP.NET sekmesinden bunu yapabilirsiniz.


## <a name="opening-web-projects"></a>Açılış Web projeleri

Web projelerini açmak, proje oluşturma işlemiyle benzerdir. Aşağıdaki bölümlerde, çıkış için IDE içinde çalışırken takip etmek için alanları ilgilendiren. Ayrıca, HTTP ve FTP kullanarak Web projeleri ile çalışmayı kapsar.

Bir Web projesi açmak için Dosya menüsünden Web sitesini Aç'ı seçin. Daha önce ele aynı konumu seçin iletişim kutusu ile istenir ve kullanabileceğiniz aynı dört seçenekleriniz vardır: dosya sistemi, yerel IIS, FTP ve uzak Site.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Dosya sistemi

Bu modülde daha önce belirtildiği gibi Visual Studio artık proje dosyası kullanır. Bu nedenle, dosya sisteminden bir Web sitesini açmak isterseniz, aslında seçtiğiniz klasöre başlangıçta Visual Studio'da bir Web projesi olarak oluşturulmamış olsa bile, istediğiniz herhangi bir klasör seçme seçeneğiniz vardır. Örneğin, bir Web sitesi olarak Belgelerim klasörünü açmak seçin ve Visual Studio sonsuza dek açın ve aşağıda gösterildiği gibi dosyalarınızı görüntüleyin.


![Bir Web sitesi açılan Belgelerim](improvements-in-visual-studio-2005/_static/image3.jpg)

**Şekil 6**: *Belgelerim* bir Web sitesi açılır


Visual Studio, yalnızca ek dosyaları ve klasörleri gerektiğinde oluşturduğundan, hiçbir ek dosya veya klasör, açık konuma eklenir. Bu mimarinin yan etkisi, Web siteleri dosya sisteminde iç içe engellenmesidir. Örneğin, aşağıdaki dizin yapısını göz önünde bulundurun.

C:/numaralı Web projesi

C:/numaralı/iç içe başka bir web projesi

C:/numaralı Web sitesinde açtığınızda, iç içe geçmiş klasör bu uygulamanın bir alt klasör görünür.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

HTTP üzerinden Web siteleri açarken ayarları (yerel IIS) IIS metatabanı veya FrontPage Server Extensions (Uzak Site.) kullanarak okunur İç içe geçmiş web uygulamaları varsa, bunlar da bunları bir uygulama olarak tanımlayan bir simge ile gösterilir. FrontPage web uygulamaları ile birlikte çalışma bilginiz varsa, Visual Studio 2005 davranış benzerdir.

Visual Studio IDE içinde şu anda açıldığında uygulamanın altında iç içe uygulamalar için bir simge görüntüler olsa da, bunları içeriklerini görmek için genişletmenize sağlayamaz. Ancak, bunları açmak için çift tıklayabilirsiniz. Bunu yaptığınızda ya da web uygulamasını (ve şu anda açık olan çözümü değiştirmek için) isteyen bir iletişim kutusu ile sunulur veya geçerli çözümünüzden Web uygulamasına ekleyin.


![İç içe geçmiş uygulama simgesini çift ile bu iletişim kutusunu gösterir](improvements-in-visual-studio-2005/_static/image4.jpg)

**Şekil 7**: iç içe geçmiş uygulama simgesini çift sunar, bu iletişim kutusunda


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP sitesi

FTP aracılığıyla bir siteyi açtığınızda, dosyalar tüm yerel geçici klasörünüze kopyalanır. Yerel depolama konumu için tam yol, proje için Özellikler bölmesi görüntülenir ve aşağıdaki biçimi kullanarak oluşturulur.

C:/belgeler ve ayarlar /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;sunucu&gt;/_&lt;uygulama adı&gt;

FTP kullanarak, Visual Studio projeniz için temel URL'yi aşağıda gösterildiği gibi gözatabilirsiniz belirtmeniz gerekir. Temel bir URL belirtmezseniz, Visual Studio, bunları Web sitesinde bir sayfaya göz atın girişimi ilk kez isteriz.


![FTP siteleri için temel URL belirtme](improvements-in-visual-studio-2005/_static/image5.jpg)

**Şekil 8**: FTP siteleri için temel URL belirtme


## <a name="improvements-in-compilation"></a>Derleme geliştirmeleri

Web uygulamaları Visual Studio 2005 ile çalışma önceki sürümlerden önemli ölçüde daha hızlı gerçekleşir. Derleme mimari değişiklikler nedeniyle hiçbir küçük bir bölümü budur.

Visual Studio 2002 ve 2003'te Web uygulamaları / Bin klasöründe bulunan bir birincil derlemesini derlendi. Visual Studio 2005'te, bir uygulama/_kod klasör eklendi. Sınıfları ve diğer UI olmayan kod uygulama/_kod klasörüne eklenir. Visual Studio projeyi oluşturduğunda, uygulama/_kod klasördeki tüm dosyaları tek bir App/_Code.dll dosyasına derlenir. Bu değişikliğin sonucu ardışık derlemeler önceki sürümlerde çok daha hızlı olmasıdır.

> [!NOTE]
> MSBuild komut satırı yardımcı programı, ASP.NET Web uygulamaları oluşturmak için de kullanılabilir. Bu aracı 9 modülünde ele alınmaktadır.


Başka bir derleme geliştirme derle menüsünde yeni yapı sayfası seçeneğidir. Bu özellik, böylece değişiklikleri daha hızlı bir şekilde derlenebilir yalnızca geçerli sayfa (ile birlikte, kurs ve bağımlılıkları) yeniden derlemek bir geliştirici sağlar. C#, IntelliSense, vb. güncelleştirmek amacıyla arka plan derlemesi sağlamaz çünkü yalnızca tek bir sayfayı yeniden oluşturmayı hızlı bir şekilde güncelleştirilmesi IntelliSense izin bunlar iyice bu özellikten yararlanabilirsiniz.

Proje derleme özelliklerini başlangıç sayfasını yürütülmeden önce oluşan yapı türünü yapılandırmanıza olanak sağlar. Geliştiriciler Visual Studio uygulamalarında kod değişikliklerinden sonra daha hızlı hata ayıklama başlayabilmesi yalnızca geçerli sayfayı yapı seçebilirsiniz.


![Derleme sayfası başlangıç eylemi](improvements-in-visual-studio-2005/_static/image6.jpg)

**Şekil 9**: derleme sayfa başlangıç eylemi


Visual Studio ve ASP.NET mimarisinin harika geliştirme başka bir düzenleme alanındadır ve devam edin. Geliştiriciler Visual Studio 2005'te bir projesinde hata ayıklamaya başlama ve hata ayıklayıcı ayırma olmadan projede kod değişiklikleri yapabilir. Aslında, gerçek anlamda bir projede hata ayıklamaya başlayabilirsiniz yeni bir sınıf daha ekleyin, o sınıf için kod ekleme, kod bu sınıfın yeni bir örneğini oluşturan sayfanıza eklemek ve sınıfın tüm hata ayıklayıcı ayırma olmadan bir yöntem yürütülmeye. Yeni kod yürüten tarayıcıyı yenilemeyi olarak tam anlamıyla oldukça kolaydır!

Video düzenleme bkz ve Visual Studio 2005'te özellik devam etmek için buraya tıklayın.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Açık tam ekran görüntü](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Güçlü düzenleyebilir veya işlevselliği ASP.NET 2.0 ile devam edin ve bir değişimdir ASP.NET uygulamaları için Visual Studio 2005 kaynaklanır. ASP.NET'te 1.x, Visual Studio 2002/2003'te oluşturulan uygulamalar / bin klasöründeki birincil bir bütünleştirilmiş kod içine derlenmiş. Tüm sınıflar, sayfalar, vb. uygulama derlenmiş için tek bir DLL. Ardından çalışma zamanında ASP.NET tüm denetimleri, biçimlendirme ve sayfaları içinde ASP.NET kodu derleyin ve bu DLL'leri ASP.NET geçici klasöre kopyalayın.

ASP.NET 2. 0'da, çalışma zamanında iki derleme modelleri (Visual Studio için bir tane) ve ASP.NET için yukarıda ana hatlarıyla kullanarak Visual Studio 2005 ortak derleme modeline birleştirildi. Tüm derleme sorunları artık yerine geliştirme aşamasında çalışma zamanında yakalanabilmesini ve anlamına gelir. Tasarımcı ve kullanıcı denetimleri ve ana sayfalar gibi özellikler için IntelliSense desteği de sağlar.

Kullanıcı denetimleri için tasarımcı desteği videosu görmek için buraya tıklayın.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Açık tam ekran görüntü](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Bir kullanıcı denetimi bir sayfadan kaldırıldığında @Register yönergesi işaretlemede kalır ve kullanıcı denetimi Web sitesinden silinirse ayrıştırıcı hatalarını önlemek için el ile kaldırılması gerekiyor.


Visual Studio derleme modelinde başka bir geliştirme, Web sitesi yayımlama özelliğidir. Geliştiriciler, Yayımla özelliğini Web sitesi işlemini gerçekleştirir. çünkü derleme isteğe bağlı herhangi bir şey yapmak zorunda eklenen performansı keyfini çıkarabilirsiniz. Dağıtılacak kaynak kodu yok sahip olacak şekilde, aynı zamanda uygulama/_kod klasöründeki tüm kaynak kodu bir DLL içine işlemini gerçekleştirir.


![Yayımla Web sitesi iletişim kutusu](improvements-in-visual-studio-2005/_static/image7.jpg)

**Şekil 10**: Yayımla Web sitesi iletişim kutusu


> [!NOTE]
> Aspnet/_compile.exe yardımcı programı, bir ASP.NET Web uygulamasına önceden derlemek için de kullanılabilir. Bu aracı 9 modülünde ele alınmaktadır.


Ne zaman aşağıda gösterildiği gibi Yayımla bir Web sitesi önceden derlenmiş dosyaları ASP.NET dosyaları klasöründe depolanır. İle dosyaları bir *.compiled* dosya uzantısı olan belirli DLL'ler için bağımlıkları tanımlama XML dosyaları. Herhangi bir Webform veya kullanıcı denetimleri ile başlayan rastgele DLL'leri içine derlenmiş *uygulama /_Web /_*.

Bırakırsanız *güncelleştirilebilir bu önceden derlenmiş sitenin izin* onay kutusu seçili, biçimlendirme, Webforms ve kullanıcı denetimleri içinde dağıtımdan sonra değişiklik yapmanızı sağlayan bir DLL içine önceden derlenmiş olmayacak. Böylece değişiklikleri dağıtılan içerik için izin verilmeyen biçimlendirme kilitlemek tercih ederseniz, bu kutunun işaretini kaldırın.

*Kullanım sabit adlandırma ve tek sayfa bütünleştirilmiş kodlarını* , böylece her sayfada sabit adlandırılmış bir bütünleştirilmiş kod içine derlenmiş toplu derlemeyi devre dışı bırakmak onay kutusunu olanak sağlar. Bu kutusunun işaretlenmemesi Toplu derleme avantajlarından yararlanmanıza olanak sağlar.

*Önceden derlenmiş derlemeleri etkinleştir üzerinde güçlü adlandırma* onay verir tanımlayıcı ad için önceden derlenmiş bütünleştirilmiş kodlarınızı.

> [!NOTE]
> ASP.NET'te 1.x, tanımlayıcı adlı derlemeler genel derleme önbelleği (GAC) içinde yüklü gerekiyordu. ASP.NET 2. 0 ', katı adlı derlemeler GAC içine yüklemek için gerekli değildir.


![Bir ASP.NET uygulamaları önceden derlenmiş dosyalar](improvements-in-visual-studio-2005/_static/image8.jpg)

**Şekil 11**: bir ASP.NET uygulamaları önceden derlenmiş dosyalar


> [!NOTE]
> Yukarıdaki uygulama web.config dosyası yoktu. Geliştirilmişse, onu çağrılmış *PrecompiledApp.config* sonra yayımlama Web sitesi işlemi.


## <a name="improvements-in-deployment"></a>Dağıtım geliştirmeleri

Olarak bir kopyası proje özelliği Visual Studio 2005 Visual Studio 2002 ve 2003 ile sunar. Ancak, özellik Visual Studio 2005'te barındıracak ve artık Web sitesini kopyalama çağrılır.

Kopyalama Web sitesi iletişim kutusunda, sol çerçeve ve doğru bir çerçeve ayrılır. Sol çerçeve kaynak Web sitesinde ve uzak Web sitesinde doğru çerçeve olarak adlandırılır. Bazı geliştiriciler karışıklığa neden olabilir bir şey doğru çerçevede gösterilen site mutlaka bir uzak site olmamasıdır. Yerel dosya sisteminde veya yerel IIS örneği üzerinde bir site olabilir. İletişim kutusu uzak Web sitesinden yayımlamanıza olanak tanır. Ayrıca, sol çerçevede gösterilen site mutlaka kaynak Web sitesinde olmadığından *için* kaynak Web sitesinde.

Bu site, uzak bir Web sitesi için bir proje kopyalıyorsanız FrontPage Server Extensions'in yüklü olması gerekir. Aksi takdirde, FTP kullanarak bağlanmak gerekir. Öte yandan, bir proje için yerel IIS örneği kopyalanıyorsa, FrontPage Server Extensions gerekli değildir.

> [!NOTE]
> Yerel IIS örneğinde yeni bir Web sitesi oluşturmak deneyin ve FrontPage 2002 Sunucu Uzantıları yüklenir, Web siteleri oluşturma bir SharePoint sunucusuna desteklenmediğini söyleyen bir hata iletisi alırsınız. Bu durumda, FrontPage 2000 Server Uzantıları yükleme veya FrontPage Server Extensions kaldırma seçeneğiniz vardır.


Web sitesini kopyalama özelliğinin bir videosu için buraya tıklayın.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Açık tam ekran görüntü](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Hata ayıklama geliştirmeleri

Visual Studio 2005'te hata ayıklama dört önemli geliştirmeleri vardır.

- Yerel yönetici olmayan hata ayıklama, kullanıma hazır mümkündür.
- Derleme ögesi hata ayıklama özelliği şimdi varsayılan olarak false değil.
- Uzaktan hata ayıklama kurulumu ve yapılandırması, önce daha kolay olur.
- Artık bir Web sitesi FTP konumu açılan ayıklayabilirsiniz.

## <a name="debugging-as-a-non-administrator"></a>Yönetici olmayan hata ayıklama

ASP.NET Geliştirme Sunucusu eklenmesi yönetici olmayanların kolayca çıktığı ASP.NET uygulamalarının hatalarını ayıklamanızı sağlar. Yerel dosya sistemi üzerinde çalışan bir ASP.NET uygulamanın hataları ayıklanırken, Visual Studio ASP.NET geliştirme sunucusu altında oturum açmış kullanıcının bağlamını başlatır. Bu kullanıcı ardından herhangi bir ek yapılandırma olmadan bu uygulamada hata ayıklaması yapabilirsiniz.

## <a name="debug-is-false-by-default"></a>Hata ayıklama False olduğundan varsayılan olarak

ASP.NET'te 1.x, *hata ayıklama* özniteliğini *derleme* web.config dosyasının sonuna öğe ayarlanmıştır *true* varsayılan olarak. Geliştiriciler bu öznitelik ayarlanmış olduğunu her zaman öneriliyordu *false* üretim, bir uygulamayı dağıtmadan önce ancak çoğu Geliştirici hata ayıklama özniteliği bırakarak sonuçlarını tam olarak anlamadığınız TRUE, bunlar yalnızca olarak sol-olduğu.

En önemli bir sorun hata ayıklama özniteliğine sahip ile true ASP.NETs Toplu derleme modeli devre dışı olarak ayarlanması. Bu nedenle, her sayfaya ayrı bir DLL içine derlenir. Anlamına gelir bir Web sayfaları (değil duyulmamış herhangi bir yolla olarak), binlerce uygulama oluşur, birkaç bin küçük DLL'leri o uygulama tarafından oluşturulur. Bu DLL'leri boyutunda küçük olsa da, bunlar herhangi belirli bir konuma bellekteki yüklü değildir. Bu nedenle, sistem belleğini parçalanmasına neden ve OutOfMemoryException oluşum katkıda bulunabilir.

ASP.NET 2. 0'da, hata ayıklama öznitelik varsayılan olarak false olarak ayarlanır. Zaten, ne zaman bir geliştirici bir ASP.NET uygulamasını Visual Studio 2005 ' te hata ayıklamasına gördüğünüz gibi hata ayıklama etkin olan web.config dosyası eklemeniz istenir. Bunun yapılması, ASP.NET'te mevcut aynı dezavantajları doğurur 1.x, ama şimdi Geliştirici açıkça uyarılır uygulamayı üretim ortamına geçmeden önce özniteliği false değerine sıfırlanmalıdır.

## <a name="remote-debugging-setup-and-configuration"></a>Uzaktan hata ayıklama kurulumu ve yapılandırması

Visual Studio 2002/2003'te, uzaktan hata ayıklama için Makine Hata Ayıklama Yöneticisi (mdm.exe) ve vs7jit.exe işlem yararlandı. Nedeniyle, uzaktan hata ayıklama sorunlarını giderme genellikle olan müşteriler için bir siyah kutu ve PSS için çok daha iyi değildir.

Visual Studio 2005 mdm.exe ve vs7jit.exe işlemleri güvenme kaldırır. Bunun yerine, artık uzaktan hata ayıklama İzleyicisi (msvsmon.exe) hizmeti kullanır

Visual Studio 2005'te uzaktan hata ayıklama gereksinimi oldukça basittir. Hata ayıklama önce uzak sunucuda msvsmon.exe çalıştırmanız gerekir. Visual Studio CD'den uzaktan hata ayıklama İzleyicisi'ni yükleyebilirsiniz veya hiçbir şey hiç Web sunucusunda yüklemeden msvsmon.exe bir paylaşımdan yalnızca çalıştırabilirsiniz.

Msvsmon.exe çalıştırdığınızda, uzaktan hata ayıklama için engellenme bağlantı noktaları hakkında şikayet olasıdır. Neyse ki, kolayca bağlantı noktalarından sağ Uyarısı iletişim kutusu içinde aşağıda gösterildiği gibi engelini kaldırabilirsiniz.


![Windows Güvenlik Duvarı uzaktan hata ayıklamayı engelleyen olduğunu söyleyen bir bildirim](improvements-in-visual-studio-2005/_static/image9.jpg)

**Şekil 12**: bildirim, Windows Güvenlik duvarı olup engelleme uzaktan hata ayıklama


Hata ayıklama için gerekli bağlantı noktalarını engeli kaldırılmış sonra aşağıda gösterildiği gibi uzaktan hata ayıklama İzleyicisi'ni göreceksiniz. Bu arabirimden bağlantıları izleyebilir ve izinler bir kolayca hata ayıklama değiştirin.


![Uzaktan hata ayıklama İzleyicisi](improvements-in-visual-studio-2005/_static/image10.jpg)

**Şekil 13**: uzaktan hata ayıklama İzleyicisi


FTP aracılığıyla açık bir Web uygulaması uzaktan hata ayıklamak mümkündür. Adımları bu daha önce ele aynıdır. Ancak, bu modülün daha önce belirtildiği gibi FTP projeye göz atmak için bir temel URL'si belirtmeniz gerekecektir.

## <a name="lab-2"></a>Laboratuvar 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Visual Studio 2005 ile uzaktan hata ayıklama

Bu Laboratuvar, Visual Studio 2005 ile uzaktan hata ayıklama aracılığıyla anlatılmaktadır.

Bu laboratuvarı bir videosu için buraya tıklayın.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Açık tam ekran görüntü](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Bu Laboratuvar çalışan Visual Studio 2005 ve diğer çalışan IIS 5 veya daha büyük olmak üzere iki makine olması gerekir.

1. Visual Studio 2005'i açın ve uzak sunucuda yeni bir Web sitesi oluşturun.

> [!NOTE]
> Uzak bir IIS örneği veya FTP üzerinden Web sitesi oluşturabilirsiniz.


1. Uzak Web sunucusundan bir UNC yolu kullanarak geliştirme makinede msvsmon.exe bulun ve yürütün.  
 //Server/c$/Program dosyaları/Microsoft Visual Studio 8/Common7/IDE/uzaktan hata ayıklayıcı/x86 msvsmon.exe için varsayılan konumdur.
2. Uzaktan hata ayıklama için bağlantı noktalarını engellemesini kaldırmak isteyip istemediğiniz sorulduğunda bunu yapın.
3. Geliştirme makinesinden Default.aspx için gerideki açın ve sayfa/_yük yöntemde bir kesme noktası ayarlayın.
4. Geliştirme makinesinden hatalarını ayıklamaya başlayın.

Beklenen şekilde kesme noktasına isabet.

## <a name="aspnet-development-server"></a>ASP.NET Geliştirme Sunucusu

Zaten ele weve Visual Studio 2005 ASP.NET geliştirme sunucusu olarak adlandırılan bir Web sunucusu ile birlikte gelir. (ASP.NET Geliştirme Sunucusu bazen Cassini adlandırılır.) Bu Web sunucusu, göz atmak ve dosya sistemi üzerinde çalışan Web uygulamalarında hata ayıklama için kullanışlı bir yoludur.

ASP.NET Geliştirme Sunucusu kısıtlı bir Web sunucusudur. Uzak bağlantılara izin vermediğinden, uygulamanın tüm istekleri Web sunucusu başlatan kullanıcının dışındaki herhangi bir kullanıcının izin verilmez. ASP sayfalarını sunmadan yeteneği de yok. Yalnızca ASP.NET ve HTML kaynaklarının (görüntüler, CSS dosyaları, vb. dahil) sunulur.

ASP.NET Geliştirme Sunucusu c:/Windows/Microsoft.NET/Framework/v2.0./ bulunan WebDev.WebServer.exe dosyasını çalıştırarak komut satırı üzerinden başlatılabilir*/* /  */*/*. Kullanılabilir parametreler aşağıdaki iletişim kutusunu görüntüler.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Şekil 14**


> [!NOTE]
> ASP.NET Geliştirme Sunucusu açıkça komut satırı üzerinden başlatıldığında desteklenmiyor.
