---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Visual Studio 2005'te geliştirmeleri | Microsoft Docs
author: microsoft
description: Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2018
ms.locfileid: "28987996"
---
<a name="improvements-in-visual-studio-2005"></a>Visual Studio 2005'te geliştirmeleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar.


Visual Studio 2005 yenilikleri ve geliştirmeleri Web projeleri için uzun bir listesi ile Web uygulaması geliştiricileri sağlar. Visual Studio .NET 2002 ve 2003 olarak güçlü olarak, vardı birçok şikayetlerinden Web projeleri işlenen biçiminde. Visual Studio 2005 bu şikayetlerinden değinmek için çok sayıda yeni özellikleri ekler. Kişiler için Visual Studio .NET 2003 derleme Web uygulamalarının işlenme tercih ederseniz, bkz: [Web Uygulama projeleri](https://go.microsoft.com/fwlink/?LinkId=57870).

Bu modül de Web projesi oluştururken, yönetim ve geliştirme geliştirmeler kapsar. Bir sonraki modüle Web projeleri oluşturmak ve bunları dağıtma iyileştirmeleri de kapsar.

## <a name="frontpage-server-extensions"></a>FrontPage Server Extensions

Visual Studio .NET 2002 ve 2003 FrontPage Server Extensions kutusunda oluşturun veya Web projeleri oluşturmak için gereklidir. Geliştiriciler iki farklı erişim modları (FrontPage Server Extensions veya dosya erişim modu) arasında bir seçim sahip, FrontPage Server Extensions hem de uygulama kökü ayarlama, IIS vb. gibi görevleri gerçekleştirmek için kullanılır.

Visual Studio 2005 FrontPage Server Extensions bağımlılık yerel projeleri için kaldırır. Visual Studio 2005 şimdi IIS metatabanı FrontPage Server Extensions kullanmak yerine doğrudan erişir. Visual Studio 2005 ayrıca FrontPage Server Extensions gerek kalmadan uzak proje erişim sağlayan FTP için destek ekler.

FrontPage Server Extensions kendi projelerine kullanmak istediğiniz geliştiriciler seçeneği kullanılabilir durumda kalır. Ancak, güçlü ASP.NET Geliştirici topluluğu görüşleri göre bu zorunlu değildir.

> [!NOTE]
> FrontPage Server Extensions uzak proje oluşturma, açma, vb. için hala gereklidir.


## <a name="aspnet-development-server"></a>ASP.NET Geliştirme Sunucusu

Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen yeni bir Web sunucusu ile birlikte gelir. (Bu Web sunucusu daha önce Cassini bilinir.)

ASP.NET Geliştirme Sunucusu çeşitli avantajları vardır.

- Şimdi yönetici olmayanların geliştirmek ve bir Web sunucusuna karşı hata ayıklamak mümkündür.
- ASP.NET Geliştirme Sunucusu herhangi bir yere sanal dizinler için esnek proje konumlarını izin vererek dosya sistemindeki dinamik olarak eşler.
- IIS zaten kullanan kullanıcılar Windows XP Professional şimdi dosya veya klasör yapısı, kendi varsayılan Web sitesi IIS'de etkilemez yeni Web uygulamaları oluşturmak mümkün olacaktır.

ASP.NET Geliştirme Sunucusu yararlanmak için özel yapılandırma gerekir. Dosya sistemi üzerinde barındırılan bir Web projesi hata ayıklaması veya taranan, Visual Studio 2005 ASP.NET Geliştirme Sunucusu örneği isteğe hizmet verecek bir rastgele bağlantı noktası otomatik olarak başlar.

Daha fazla bilgi ASP.NET geliştirme sunucusu bu modüldeki daha sonra ele alınacaktır.

## <a name="improved-file-management"></a>İyileştirilmiş dosya yönetimi

Visual Studio 2002 ve 2003'te bir proje dosyası (.vbproj VB.NET için) ve C# .csproj bilgi Web uygulamasındaki tüm dosyalarda depolanır. Çözüm Gezgini ekran proje dosyasında dosya bilgileri temel alır. Bu nedenle, Çözüm Gezgini genellikle yanlış bilgi dış düzenleyicileri kullanıldığı durumlarda görüntüleyecektir. Visual Studio 2002 ve 2003 genellikle dosya değişiklikleri üzerine veya dosyaların en son sürümünü görüntülemeyecek.

Visual Studio 2005 hemen proje dosyasıyla birlikte yapar. Bunun yerine, doğrudan projenizdeki dosyaların doğru bir görüntüde kaynaklanan diskten, dosya ve klasör bilgileri okur. Visual Studio 2002 ve 2003 başvuruları klasöründe, Web uygulamanızı gerçek bir klasörde göstermiyor olduğundan, Visual Studio 2005 başvurular klasörünü Çözüm Gezgini'nden de kaldırır. Projeniz Visual Studio 2005'te başvurularını erişmek için proje için özellik sayfaları kullanmanız gerekir.

## <a name="creating-web-projects"></a>Web projeleri oluşturma

Web geliştiricileri Visual Studio 2005'te proje oluşturmak için kullanılabilir birçok yeni seçeneğiniz vardır. Web siteleri artık herhangi bir yere dosya sistemini de oluşturulabilir ve sonra hata ayıklaması yapılabilir veya yeni ASP.NET geliştirme sunucusu kullanarak taranan. Geliştiriciler ayrıca FTP kullanarak yeni Web siteleri oluşturabilirsiniz.

Visual Studio 2005'te Web projeleri oluşturma videosu görüntülemek için burayı tıklatın.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Açık Tam Ekran Video](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Dosya sistemi projeleri

Videosu içinde anlatıldığı gibi yerel makine üzerinde veya bir dosya paylaşımı üzerinden uzak bir konumdan dosya sisteminde Web siteleri oluşturmayı seçebilirsiniz. Dosya sisteminde oluşturan Web siteleri taranan ve ASP.NET geliştirme sunucusu kullanarak hata ayıklaması.

> [!NOTE]
> ASP.NET Geliştirme Sunucusu müşteriler için bazı karışıklığa neden olabilir. IISs dizin yapısına (yani c:/inetpub/wwwroot) dosya sisteminde bir Web projesi oluşturduysanız, Visual Studio 2005 içinde başlatıldığında ASP.NET geliştirme sunucusu aracılığıyla Web sitesi yine taranmasına. Bu nedenle, herhangi bir IIS yapılandırması (yani, kimlik doğrulama yöntemleri) geçerli değil.


Varsayılan web projesi de çok kaldırır yük tarafından yalnızca Default.aspx sayfasında, default.cs dosya ve uygulama/_Data klasör içerir. Bunlar gerektiğinde özel klasör (örn. uygulama/_code) ve web.config eklenir. Web projenize yalnızca gereksinim duyduğunuz klasörleri ve dosyaları içerir.

### <a name="http-projects"></a>HTTP projeleri

HTTP projeleri ya da yerel bir IIS Web sitesi veya uzak bir Web sitesinde oluşturulan projeleri olabilir. Varsayılan proje konumu `http://localhost`. Gözat düğmesine tıklayın, iki HTTP seçenek vardır: Yerel IIS ve uzak Site. Bu iki seçenek ana fark web sitesi bilgilerini Choose Location iletişim ve dosyalar Web sunucusuna nasıl kopyalanır görüntülenir yöntemidir.

Yerel IIS seçeneği metatabanı yerel makinedeki site bilgilerini okur ve dosya sistemi kullanılarak dosyalar kopyalanır. Uzak Site seçeneği FrontPage Server Extensions ve site bilgilerini kullanır, HTTP kullanarak dosyalar kopyalanır ve FrontPage Server Extensions RPC çağırır.

> [!NOTE]
> Artık get/_aspx/_ver.aspx ve vs###/_tmp.htm dosya sürüm bilgisi belirlemek için kullanılır.


Varsayılan HTTP yerel IIS seçeneğidir. Bu seçenek, hangi siteleri kullanılabilir olduğunu belirlemek için IIS metatabanı ve içerik oluşturulacağı konum okur. Ağaç görünümünde seçerek farklı bir klasör veya sanal dizin seçebilirsiniz. Ayrıca yeni bir sanal dizin oluşturma, klasörleri uygulamaları olarak işaretle yanı bu iletişim kutusundan mevcut sanal dizinleri silin.


![Konum iletişim seçin](improvements-in-visual-studio-2005/_static/image1.gif)

**Şekil 1**: konumu iletişim seçin


Farklı görsel, onay Studio'nun önceki sürümlerinde **kullanım Güvenli Yuva Katmanı** onay ve SSL sertifikası gözatma URL eşleşmiyor, varsa yaptığınız soran bir güvenlik uyarısı iletişim kutusu sunulur devam etmek istiyor. Sertifika eşleşen bir olmadıysa Visual Studio .NET 2003 kullanarak proje oluşturma başarısız olur.


![Güvenlik Uyarısı ilgili SSL sertifikası](improvements-in-visual-studio-2005/_static/image2.gif)

**Şekil 2**: SSL sertifikası ile ilgili güvenlik uyarısı


### <a name="note-on-host-headers"></a>Ana bilgisayar üst bilgileri Not

Bir sitede belirli bir IP bağlı bir Web uygulaması oluşturuyorsanız, bir ana bilgisayar üstbilgisi yapılandırıldığından emin olmanız gerekir. Aksi takdirde, Visual Studio sitede oluşturacak `http://localhost`, ancak IP adresi doğru olduğunda site göz atılamaz veya gelen ve IDE içinde hata ayıklaması çözümlemez.

Uzak Site seçeneğini seçerseniz, yeni Web sitesi için hedef URL girin olanak tanımak için iletişim değiştirir. Bu URL FrontPage Server Extensions etkin olan bir sunucuda olmalıdır. FrontPage Server Extensions kullanarak yerel Web sunucunuz ile çalışmak istiyorsanız, uzak Site seçeneğini kullanın ve yerel bir URL belirtin.


![Uzak bir sunucuda bir Web sitesi oluşturma](improvements-in-visual-studio-2005/_static/image1.jpg)

**Şekil 3**: Uzak bir sunucuda bir Web sitesi oluşturma


SSL sertifikası eşleşmiyorsa, SSL üzerinden uzak bir sitedeki bir uygulama oluştururken, onaylama iletişim kutusunda yerel IIS seçeneği kullanılırken görüntülenen iletişim biraz farklıdır.


![Uzak Site Güvenlik Uyarısı](improvements-in-visual-studio-2005/_static/image3.gif)

**Şekil 4**: Uzak Site Güvenlik Uyarısı


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 FTP üzerinden Web siteleri oluşturma seçeneği sunar. Bu seçeneği kullandığınızda, IDE kullanıcıların geçici klasörde dosyaları yerel olarak oluşturur ve ardından dosyaları FTP konumuna taşımak için FTP kullanır.

> [!NOTE]
> Geçici klasör konumu c:/belgeler ve ayarlar olduğu /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;Server&gt;/_&lt;uygulama adı&gt;


FTP seçeneğini kullanırken, bir konum seçin iletişim kutusu sunulur. Aşağıda gösterildiği gibi bu iletişim kutusunu içine FTP bağlantı bilgileri girin.


![Konum iletişim FTP seçin](improvements-in-visual-studio-2005/_static/image2.jpg)

**Şekil 5**: FTP konumu iletişim seçin


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Laboratuvar: Kurulum FTP sitesi ve bir proje oluşturun

Bir kullanıcı yalnızca bunlar için FTP aracılığıyla karşıya yükleyebilir bir konuma sahip olması aşağıdaki adımları FTP sitesini yapılandırın.

### <a name="install-the-ftp-service"></a>FTP hizmetini yükleme

1. Program Ekle veya Kaldır'ni açın, Windows Bileşenlerini Ekle/Kaldır
2. Internet Information Services (Windows 2003'te uygulama sunucusu) seçin ve tıklatın **ayrıntıları**.
3. Denetleme **Dosya Aktarım Protokolü (FTP) hizmeti** tıklatıp **Tamam**.
4. Tıklatın **sonraki** FTP hizmetini yüklemek için.

### <a name="create-a-new-folder-for-content"></a>İçerik için yeni bir klasör oluşturun

1. Windows Gezgini'nde adlı yeni bir klasör oluşturun **kullanıcı1** inetpub/c:/wwwroot içinde.

#### <a name="configure-folders-and-permissions-on-folders"></a>Klasörleri ve izinlerini klasörlerde yapılandırın.

1. Internet Information Services eklentisini Yönetim Araçları'ndan açın. Bilgisayar adı düğümü altında bir FTP siteleri klasörünü artık bulunacaktır.
2. Genişletme **FTP siteleri**.
3. Sağ **varsayılan FTP sitesi**seçin **yeni**, ardından **sanal dizin**, ardından **sonraki**.
4. Girin **kullanıcı1** tıklatın ve sanal dizin adı için **sonraki**.
5. Girin **c:/inetpub/wwwroot/kullanıcı1** tıklayın ve yolu için **sonraki**.
6. Tıklatın **sonraki** ve ardından **son** Sihirbazı tamamlayın.
7. Sağ **kullanıcı1** varsayılan FTP sitesi ve select altında sanal dizin **özellikleri**.
8. Denetleme **yazma** onay kutusunu tıklatıp **Tamam** iletişim kutusunu kapatmak için.
9. Sağ **varsayılan FTP sitesi** seçip **özellikleri**.
10. Üzerinde **güvenlik hesapları** sekmesinde, işaretini **anonim bağlantılara izin ver**.
11. Tıklatın **Evet** devam etmek isteyip istemediğinizi soran iletişim.
12. Tıklatın **Tamam** iletişim kutusunu kapatmak için.
13. Genişletme **varsayılan Web sitesi** altında **Web siteleri** düğümü.
14. Sağ **kullanıcı1** seçin ve dizin **özellikleri**
15. İçinde **uygulama ayarları** 'yi tıklatın **oluşturma** klasörü bir uygulama olarak işaretlenecek.
16. Tıklatın **Tamam** iletişim kutusunu kapatmak için.
17. Internet Information Services eklentisini kapatın.

### <a name="create-web-project"></a>Web projesi oluşturma

1. Visual Studio 2005 açın.
2. Gelen **dosya** menüsünde, select **yeni Web sitesi**.
3. İçinde **konumu** açılan listesinde, select **FTP**.
4. **Gözat**'ı tıklatın.
5. Girin **localhost** içinde **Server** metin kutusu.
6. Girin **kullanıcı1** dizin metin kutusuna.
7. Tıklatın **açık**. Yeni Web sitesi iletişim kutusuna FTP konumunu girilecektir.
8. **Tamam**'ı tıklatın.
9. İşaretini **anonim oturum** FTP oturum açma iletişim kutusunda, kimlik bilgilerinizi girin ve **Tamam**.
10. Proje için URL nedir? (URL projesi için Çözüm Gezgini'nde görüntülenir.)
11. Gelen **yapı** menüsünde, select **yapı Web sitesi** veya **yapı çözümü**.
12. Çözüm Gezgini'nde Default.aspx sağ tıklatın ve seçin **tarayıcıda görüntüle**.
13. Web sitesi URL'si gerekli iletişim kutusuna girin `http://localhost/user1` tıklatın ve URL için **Tamam**.

> [!NOTE]
> Türü /_Default yükleme sorunu bildiren bir hata alırsanız, ASP.NET 2.0 Web sitenizi ve önceki bir sürümünü çalıştırdığından emin olun. Internet Information Services'ın ASP.NET sekmesinden, yapabilirsiniz.


## <a name="opening-web-projects"></a>Açılış Web projeleri

Web projeleri açma projeleri oluşturmaya benzer. Aşağıdaki bölümlerde bir çıkışı için IDE içinde çalışırken takip etmek alanları çıkışı çağırın. HTTP ve FTP kullanarak Web projeleri ile çalışma ele alınmaktadır.

Bir Web projesi açmak için Dosya menüsünden açmak Web sitesini seçin. Daha önce ele alınan aynı konumu seçin iletişim kutusu ile istenir ve kullanabileceğiniz aynı dört seçeneğiniz vardır: dosya sistemi, yerel IIS, FTP ve uzak Site.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Dosya sistemi

Bu modülde daha önce belirtildiği gibi Visual Studio artık bir proje dosyası kullanır. Dosya sisteminden bir Web sitesini açmak seçerseniz, bu nedenle, aslında seçtiğiniz klasör başlangıçta Visual Studio'da bir Web projesi olarak oluşturulmamış olsa bile, istediğiniz herhangi bir klasör seçme seçeneğiniz. Örneğin, bir Web sitesi olarak Belgelerim klasörünü açmak seçebilirsiniz ve Visual Studio sonsuza dek açmak ve aşağıda gösterildiği gibi dosyalarınızı görüntülemek.


![Bir Web sitesi olarak açılmış Belgelerim](improvements-in-visual-studio-2005/_static/image3.jpg)

**Şekil 6**: *Belgelerim* bir Web sitesi olarak açılmış


Visual Studio, yalnızca ek dosyalar ve klasörler gerektiğinde oluşturduğundan, hiçbir ek dosya veya klasör açtığınız konuma eklenir. Bu mimarinin bir yan etkisi, bu, Web siteleri dosya sisteminde iç içe engellediğini ' dir. Örneğin, aşağıdaki dizin yapısını göz önünde bulundurun.

Web project at C:/MyWebSite

Another web project at C:/MyWebSite/Nested

C:/numaralı Web sitesinde açtığınızda, iç içe klasör uygulamasının bir alt klasör olarak görünür.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Web siteleri HTTP üzerinden açarken ayarları (yerel IIS) IIS metatabanı veya FrontPage Server Extensions (Uzak Site.) kullanarak okunur İç içe geçmiş web uygulaması varsa, bunlar da bir uygulama olarak tanımlayan bir simgeyle görüntülenir. Visual Studio 2005 davranış FrontPage web uygulamaları ile çalışma ile bilginiz varsa, benzer.

Visual Studio IDE içinde şu anda açıldığında uygulama altındaki iç içe uygulamalar için bir simge görüntülenir olsa bile, onu içeriklerini bakmalarını genişletmenize izin vermez. Ancak, bunlar üzerinde bunları açmak için çift tıklatabilirsiniz. Bunu yaptığınızda, ya da web uygulaması açın (ve şu anda açık olan çözüm değiştirin) isteyen bir iletişim kutusu sunulur veya geçerli çözümünüz için Web uygulaması ekleyebilirsiniz.


![İç içe geçmiş uygulama simgesini çift tıklatarak bu iletişim kutusunda gösterir](improvements-in-visual-studio-2005/_static/image4.jpg)

**Şekil 7**: iç içe geçmiş uygulama simgesini çift gösterir, bu iletişim kutusunda


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP sitesi

Bir siteyi FTP aracılığıyla açtığınızda, dosyalar tüm yerel geçici klasörünüze kopyalanır. Yerel depolama konumunun tam yolunu projesi için Özellikler bölmesinde görüntülenir ve aşağıdaki biçimi kullanarak oluşturulur.

C:/belgeler ve ayarlar /&lt;kullanıcı&gt;/yerel ayarları/Temp/VWDWebCache/&lt;Server&gt;/_&lt;uygulama adı&gt;

FTP kullanırken, Visual Studio aşağıda gösterildiği gibi gözatabilirsiniz projeniz için temel URL belirtmeniz gerekecektir. Temel bir URL belirtmezseniz, Visual Studio için Web sitesindeki bir sayfasına göz girişimi ilk kez istenir.


![FTP siteleri için temel URL belirtme](improvements-in-visual-studio-2005/_static/image5.jpg)

**Şekil 8**: FTP siteleri için temel URL belirtme


## <a name="improvements-in-compilation"></a>Derleme geliştirmeleri

Visual Studio 2005'te Web uygulamaları ile çalışma önemli ölçüde önceki sürümlerden daha hızlıdır. Bu derleme mimarisi yapılan değişiklikler nedeniyle küçük bir parçası olur.

Visual Studio 2002 ve 2003, Web uygulamaları / bin'in klasöründe bulunan bir birincil derlemesini derlendi. Visual Studio 2005'te, bir uygulama/_Code klasör eklendi. Sınıfları ve başka bir kullanıcı Arabirimi kodu uygulama/_Code klasörüne eklenir. Visual Studio proje oluşturduğunda, uygulama/_Code klasördeki tüm dosyaları tek bir App/_Code.dll dosyasında derlenir. Bu değişikliğin sonucu sonraki derlemeleri önceki sürümlerde çok daha hızlı olmasıdır.

> [!NOTE]
> MSBuild komut satırı yardımcı programı, ASP.NET Web uygulamaları geliştirmek için de kullanılabilir. Bu aracı Modülü 9 ele alınacaktır.


Başka bir derleme geliştirme, derleme menüsünde yeni yapı sayfasını seçenektir. Bu özellik, böylece değişiklikleri daha hızlı derlenebilir yalnızca geçerli sayfa (ile birlikte, indirmelere ve bağımlılıkları) yeniden oluşturmak bir geliştirici sağlar. C# arka plan derleme güncelleştirme IntelliSense, vb. amacıyla sağlamadığından, yalnızca tek bir sayfayla yeniden oluşturma hızlı bir şekilde güncelleştirilmesi IntelliSense için izin verdiğinden bunlar çok bu özelliğinden yararlanabilir.

Proje derleme özellikleri başlangıç sayfasını yürütülmeden önce oluşan yapı türü yapılandırmanıza olanak tanır. Geliştiriciler Visual Studio kod değişikliklerinden sonra daha hızlı uygulamalarında hata ayıklama başlayabilmeniz için yalnızca geçerli sayfayı oluşturmak seçebilirsiniz.


![Derleme sayfası başlangıç eylemi](improvements-in-visual-studio-2005/_static/image6.jpg)

**Şekil 9**: derleme sayfa başlangıç eylemi


Visual Studio ve ASP.NET mimarisi için başka bir önemli geliştirme düzenleme alanındadır ve devam edin. Geliştiriciler Visual Studio 2005'te Proje hata ayıklamayı Başlat ve hata ayıklayıcı ayırma olmadan projede kod değişiklikleri yapabilir. Aslında, bir proje hata ayıklama tam anlamıyla başlatmak için yeni bir sınıf ekleyin, o sınıfın kodu ekleyin, kod sayfanıza o sınıfın yeni bir örneğini oluşturur ekleyin ve sınıfın tüm hata ayıklayıcı ayırma olmadan bir yöntem yürütülmeye. Yeni kod yürütmek tarayıcıyı yenilemeyi olarak tam anlamıyla kadar kolaydır!

Videosu düzenleme görmek ve Visual Studio 2005'te özellik devam etmek için burayı tıklatın.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Açık Tam Ekran Video](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Sağlam düzenleyin ve işlevsellik ASP.NET 2.0 ile devam etmek ve Visual Studio 2005 ASP.NET uygulamaları için Mimari değişikliği nedeniyle. ASP.NET 1.x, Visual Studio 2002/2003'te oluşturulan uygulamaların / bin'in klasöründe depolanan birincil bir bütünleştirilmiş koda derlenmiş. Tüm sınıflar, sayfalar, vb. uygulama derlenmiş için bu bir DLL'e. Ardından çalışma zamanında ASP.NET tüm denetimler, biçimlendirme ve ASP.NET kodu sayfaları içinde derleyin ve bu DLL'ler ASP.NET geçici klasöre kopyalayın.

ASP.NET 2. 0'da, çalışma zamanında (Visual Studio için bir tane) ve bir ASP.NET için yukarıdaki iki derleme modelleri anahat kullanarak Visual Studio 2005'te bir ortak derleme modeline birleştirildi. Tüm derleme sorunları şimdi yerine geliştirme aşaması sırasında çalışma zamanında yakalanan olduğunu anlamına gelir. Ayrıca Tasarımcısı ve kullanıcı denetimleri ve ana sayfalar gibi özellikler için IntelliSense desteği sağlar.

Videosu kullanıcı denetimleri için tasarımcı desteği görmek için burayı tıklatın.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Açık Tam Ekran Video](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Bir kullanıcı denetimi bir sayfadan kaldırıldığında @Register yönergesi biçimlendirme içinde kalır ve kullanıcı denetimini Web sitesinden silinirse ayrıştırıcı hataları önlemek için el ile kaldırılması.


Visual Studio derleme modelinde başka bir geliştirme Web sitesi yayımlama özelliğidir. Web sitesi Yayımlama özelliği işlemini gerçekleştirir olduğundan, geliştiricilerin isteğe bağlı herhangi bir şey derlemek olmamasından eklenen performansı keyfini çıkarabilirsiniz. Dağıtılacak hiçbir kaynak koduna sahip olması, ayrıca uygulama/_Code klasöründeki tüm kaynak kodun bir DLL'e işlemini gerçekleştirir.


![Yayımla Web sitesi iletişim kutusu](improvements-in-visual-studio-2005/_static/image7.jpg)

**Şekil 10**: Yayımla Web sitesi iletişim kutusu


> [!NOTE]
> Aspnet/_compile.exe yardımcı programı, bir ASP.NET Web uygulaması önceden derlemek için de kullanılabilir. Bu aracı Modülü 9 ele alınacaktır.


Ne zaman aşağıda gösterildiği gibi Yayımla bir Web sitesi önceden derlenmiş dosyaları ASP.NET dosyaları klasöründe depolanır. İle dosyaları bir *.compiled* dosya uzantısı olan belirli DLL'ler için bağımlıkları tanımlama XML dosyaları. Tüm Webform ya da kullanıcı denetimleri ile başlayan rastgele DLL'leri içine derlenen *uygulama /_Web /_*.

Bırakır *güncelleştirilebilir olması için bu önceden derlenmiş sitenin izin* onay kutusu işaretli değilse, biçimlendirme Webforms ve kullanıcı denetimleri içinde dağıtımdan sonra değişiklik olanak tanıyan bir DLL içerisine önceden derlenmiş olmayacak. Böylece değişiklikleri dağıtılan içerik için izin verilmeyen biçimlendirme kilitlemek tercih ederseniz, bu kutunun işaretini kaldırın.

*Kullanım sabit adlandırma ve tek sayfa derlemelerinin* , böylece her sayfanın sabit adlı bir derlemeye derlenmiş toplu iş derlemesi devre dışı bırakmak onay kutusunu izin verir. Bu kutu işaretlenmemesi, toplu iş derlemesi yararlanmak sağlar.

*Önceden derlenmiş derlemeleri etkinleştir üzerinde güçlü adlandırma* onay kutusunu verir tanımlayıcı adlı için derlenmiş derlemeleriniz.

> [!NOTE]
> ASP.NET 1.x, tanımlayıcı adlı derlemeler gerekiyordu Genel Derleme Önbelleği (GAC) içinde yüklü olması. ASP.NET 2. 0'da, siz tanımlayıcı adlı derlemeler GAC içine yüklemek için gerekli değildir.


![Bir ASP.NET uygulamaları önceden derlenmiş dosyalar](improvements-in-visual-studio-2005/_static/image8.jpg)

**Şekil 11**: bir ASP.NET uygulamaları önceden derlenmiş dosyalar


> [!NOTE]
> Yukarıdaki uygulamada herhangi bir web.config dosyası vardı. Geliştirilmişse, onu çağrılmış *PrecompiledApp.config* sonra yayımlama Web sitesi işlemi.


## <a name="improvements-in-deployment"></a>Dağıtım yenilikleri

Visual Studio 2002 ve 2003, Visual Studio 2005 kopyalama proje özellik önerir. Ancak, özellik Visual Studio 2005'te barındıracak ve şimdi Web sitesini kopyalama çağrılır.

Kopyalama Web sitesi iletişim kutusunda, sol çerçeve ve doğru bir çerçeve ayrılır. Sol çerçeve kaynak Web sitesi adı verilir ve sağ çerçeve uzak Web sitesi adı verilir. Bazı geliştiriciler karışıklığa neden olabilir bir şey sağ çerçevede görüntülenen site mutlaka bir uzak site olmamasıdır. Yerel dosya sistemine veya IIS yerel örneği bir site olabilir. İletişim kutusu, Uzak Web sitesinden yayımlamak tanır Ayrıca, sol çerçevede görüntülenen site mutlaka kaynak Web sitesi olmadığından *için* kaynak Web sitesi.

Bu site, Uzak Web sitesi için bir proje kopyalıyorsanız FrontPage Server Extensions yüklü olması gerekir. Yoksa, FTP kullanarak bağlanmak gerekir. Diğer taraftan, yerel IIS örneği için bir proje kopyalıyorsanız FrontPage Server Extensions gerekli değildir.

> [!NOTE]
> Yerel IIS örneğinde yeni bir Web sitesi oluşturmayı deneyin ve FrontPage 2002 Sunucu Uzantıları yüklü Web siteleri oluşturma bir SharePoint sunucusu üzerine desteklenmiyor bildiren bir hata iletisi alırsınız. Bu durumda, FrontPage 2000 Server Extensions yükleme veya FrontPage Server Extensions kaldırma seçeneğiniz vardır.


Web sitesini kopyalama özelliğinin bir videosu için burayı tıklatın.


![](improvements-in-visual-studio-2005/_static/image4.png)


[Açık Tam Ekran Video](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Hata ayıklama içinde geliştirmeleri

Visual Studio 2005'te hata ayıklama, dört anahtar geliştirmeleri vardır.

- Yerel bir yönetici olmayan hata ayıklama kutu dışı mümkündür.
- Hata ayıklama için derleme öğesi artık varsayılan olarak false özniteliktir.
- Uzaktan hata ayıklama kurulumu ve yapılandırması önce daha kolay olur.
- Şimdi bir Web sitesi FTP konumu açılan ayıklayabilirsiniz.

## <a name="debugging-as-a-non-administrator"></a>Yönetici olmayan hata ayıklama

ASP.NET Geliştirme Sunucusu eklenmesi, yönetici olmayanların kolayca kutunun sağ dışında ASP.NET uygulamalarında hata ayıklamasını sağlar. Yerel dosya sistemi üzerinde çalışan bir ASP.NET uygulaması hata ayıklaması, Visual Studio oturum açan kullanıcının bağlamında ASP.NET geliştirme sunucusu başlatır. Bu kullanıcının bu uygulamayı herhangi bir ek yapılandırma olmadan sonra ayıklayabilirsiniz.

## <a name="debug-is-false-by-default"></a>Hata ayıklama değeri: False varsayılan

ASP.NET 1.x, *hata ayıklama* özniteliğini *derleme* web.config dosyasının öğesi ayarlandığı *true* varsayılan olarak. Bu her zaman geliştiriciler bu öznitelik ayarlanırsa, önerilen *false* üretim, uygulamaya dağıtmadan önce ancak çoğu Geliştirici ayarlamak hata ayıklama özniteliği bırakarak sonuçlarıyla tam olarak anlamadığınız TRUE, bunlar yalnızca olarak sol-değil.

En ciddi sorun hata ayıklama özniteliğine sahip olan true, ASP.NETs Toplu derleme modeli devre dışı bırakır kümesidir. Bu nedenle, her sayfanın ayrı bir DLL'e derlenir. Anlamına gelir bir Web sayfası (değil unheard herhangi bir yolla süresi), binlerce uygulama oluşur, birkaç bin küçük DLL'leri uygulama tarafından oluşturulur. Bu DLL'ler boyutunda küçük olsa da, bunlar belirli bir konuma bellekte yüklü değildir. Bu nedenle, sistem belleğini parçalanması neden ve OutOfMemoryException tekrarlara katkıda bulunabilir.

ASP.NET 2. 0'da, hata ayıklama özniteliği varsayılan olarak false değerine ayarlanır. Zaten, ne zaman bir ASP.NET uygulaması Visual Studio 2005 ' te bir geliştirici debugs gördüğünüz gibi hata ayıklama etkin durumdayken bir web.config dosyası eklemeniz istenir. Bunun yapılması, ASP.NET mevcut aynı dezavantajları doğurur 1.x ancak şimdi Geliştirici açıkça uyarılır uygulama üretime geçmeden önce öznitelik false değerine sıfırlanmalıdır.

## <a name="remote-debugging-setup-and-configuration"></a>Uzaktan hata ayıklama kurulumu ve yapılandırması

Visual Studio 2002/2003'te, uzaktan hata ayıklama için Makine Hata Ayıklama Yöneticisi (mdm.exe) ve vs7jit.exe işlem dayanıyordu. Nedeniyle, uzaktan hata ayıklama sorunlarını giderme genellikle müşteriler için siyah bir kutu oldu ve PSS için daha iyi genellikle değil.

Visual Studio 2005 mdm.exe ve vs7jit.exe işlemleri bağımlılığı ortadan kaldırır. Bunun yerine, uzaktan hata ayıklama İzleyicisi hizmet (msvsmon.exe) şimdi kullanır

Visual Studio 2005'te uzaktan hata ayıklama gereksinimi oldukça basittir. Hata ayıklama önce uzak sunucuda msvsmon.exe çalıştırmanız gerekir. Visual Studio CD'den uzaktan hata ayıklama İzleyicisi'ni yükleyebilirsiniz veya hiçbir şey hiç Web sunucusunda yüklemeden msvsmon.exe bir paylaşımdan basitçe çalıştırabilirsiniz.

Msvsmon.exe çalıştırdığınızda, uzaktan hata ayıklama için engellenme bağlantı noktaları hakkında şikayetçi olasıdır. Neyse ki, kolayca uyarı iletişim kutusu içinde sağa bağlantı noktalarından aşağıda gösterildiği gibi engellemesini kaldırabilirsiniz.


![Windows Güvenlik Duvarı Engelleme uzaktan hata ayıklama olduğunu söyleyen bir bildirim](improvements-in-visual-studio-2005/_static/image9.jpg)

**Şekil 12**: Windows Güvenlik Duvarı bildirimidir engelleme uzaktan hata ayıklama


Hata ayıklama için gerekli bağlantı noktalarını engellemesini sonra aşağıda gösterildiği gibi uzaktan hata ayıklama İzleyicisi'ni görürsünüz. Bu arabirimden bağlantılarını izleyebilir ve izinleri kolayca hata ayıklama değiştirebilirsiniz.


![Uzaktan hata ayıklama İzleyicisi](improvements-in-visual-studio-2005/_static/image10.jpg)

**Şekil 13**: uzaktan hata ayıklama İzleyicisi


FTP aracılığıyla açılan bir Web uygulaması uzaktan hata ayıklama mümkündür. Adımları daha önce ele aynıdır. Ancak, daha önce bu modülde özetlendiği gibi FTP projesi göz atmak için bir temel URL'yi belirtmek gerekir.

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Visual Studio 2005 ile uzaktan hata ayıklama

Bu Laboratuvar Visual Studio 2005 ile uzaktan hata ayıklama size yol gösterir.

Bu laboratuvarı bir videosu için burayı tıklatın.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Açık Tam Ekran Video](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Bu Laboratuvar iki makine, bir çalışan Visual Studio 2005 ve diğer çalışan IIS 5 veya üzeri olmasını gerektirir.

1. Visual Studio 2005 açın ve uzak sunucuda yeni bir Web sitesi oluşturun.

> [!NOTE]
> Uzak bir IIS örneği veya FTP üzerinden Web sitesi oluşturabilirsiniz.


1. Uzak Web sunucusundan bir UNC yolu kullanarak geliştirme makinenizde msvsmon.exe bulun ve yürütebilirsiniz.  
 //Server/c$/Program dosyaları/Microsoft Visual Studio 8/Common7/IDE/uzaktan hata ayıklayıcı/x86 msvsmon.exe için varsayılan konumdur.
2. Uzaktan hata ayıklama için bağlantı noktalarını engellemesini kaldırmak isteyip istemediğiniz sorulduğunda bunu yapar.
3. Geliştirme makineden arka plan koduna Default.aspx açın ve sayfa/_yük yönteminde kesme noktası ayarlayın.
4. Geliştirme makineden hata ayıklamayı Başlat.

Beklendiği gibi kesme noktası isabet.

## <a name="aspnet-development-server"></a>ASP.NET Geliştirme Sunucusu

Zaten ele weve Visual Studio 2005 ASP.NET Geliştirme Sunucusu adı verilen bir Web sunucusu ile birlikte gelir. (ASP.NET Geliştirme Sunucusu bazen Cassini adlandırılır.) Bu Web sunucusu göz atmak ve dosya sistemi üzerinde çalışan Web uygulamalarında hata ayıklamak için kullanışlı bir yoludur.

ASP.NET Geliştirme Sunucusu kısıtlı bir Web sunucusudur. Uzak bağlantılara izin vermez, onu tüm istekler Web sunucusu başlatan kullanıcı dışındaki herhangi bir kullanıcıdan izin vermiyor. Ayrıca ASP sayfalarını sunmadan özelliği yok. Yalnızca ASP.NET ve HTML kaynaklarının (görüntüleri, CSS dosyaları, vb. dahil) sunulur.

ASP.NET Geliştirme Sunucusu c:/Windows/Microsoft.NET/Framework/v2.0./ bulunan WebDev.WebServer.exe dosyasını çalıştırarak komut satırı üzerinden başlatılabilir*/* /  */*/*. Kullanılabilir parametreler aşağıdaki iletişim kutusunu görüntüler.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Şekil 14**


> [!NOTE]
> ASP.NET Geliştirme Sunucusu açıkça komut satırı başlatıldığında desteklenmiyor.
