---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: testine dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: dc11072e053cbddd089e5df4bcea6d2a7af864fc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Test için dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici, IIS yerel bilgisayardaki bir ASP.NET web uygulamasına dağıtma gösterilmektedir.

Bir uygulama geliştirirken, Visual Studio'da çalıştırarak genellikle sınayın. Varsayılan olarak, web uygulama projeleri Visual Studio 2012'de IIS Express geliştirme web sunucusu olarak kullanır. IIS Express daha tam IIS Visual Studio geliştirme varsayılan olarak Visual Studio 2010 kullanan sunucudan (Cassini olarak da bilinir) gibi davranır. Ancak hiçbiri geliştirme web sunucusu tam olarak IIS gibi çalışır. Sonuç olarak, Visual Studio'da test ancak IIS dağıtıldığında başarısız olduğunda bir uygulamanın düzgün çalışacağını mümkündür.

Uygulamanızı daha güvenli şekilde bu şekilde test edebilirsiniz:

1. IIS uygulama geliştirme bilgisayarınızda daha sonra üretim ortamınıza dağıtmak için kullanacağınız aynı işlemi kullanarak dağıtın. Visual Studio web projesini çalıştırın, ancak, bunu dağıtım işleminizi test edersiniz değil IIS kullanacak şekilde yapılandırabilirsiniz. Bu yöntem, uygulamanızın doğru IIS altında çalışacağı doğrulama ek olarak, dağıtım işleminin doğrular.
2. Üretim ortamınıza neredeyse aynıdır bir test ortamı uygulamayı dağıtın. Üretim ortamında bu öğreticileri için Azure App Service'te Web uygulamalarını olduğundan, ideal sınama ortamı Azure App Service'te oluşturulan bir ek web uygulamasıdır. Yalnızca test etmek için bu ikinci web uygulamasını kullanırsınız, ancak üretim web uygulaması olarak da aynı şekilde ayarlanır.

Seçenek 2 test etmek için en güvenilir yoludur ve bunu yaparsanız, mutlaka seçenek 1 gerekmez. Ancak, için dağıtıyorsanız bir üçüncü taraf barındırma sağlayıcı seçeneği 2 uygulanabilir olmayabilir veya her iki yöntem de Bu öğretici seri gösterecek şekilde pahalı olabilir. Seçenek 2 için yönergeler sağlanır [üretim ortamına dağıtma](deploying-to-production.md) Öğreticisi.

Visual Studio'da web sunucuları kullanma hakkında daha fazla bilgi için bkz: [ASP.NET Web projeleri için Visual Studio'da Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="install-iis"></a>IIS yükleme

Geliştirme bilgisayarınızda IIS dağıtmak için IIS ve Web dağıtımı yüklenmiş olmalıdır. Varsayılan olarak Visual Studio ile Web dağıtımı yüklenmiş ancak IIS varsayılan Windows 8 veya Windows 7 yapılandırmada dahil edilmez. IIS zaten yüklü ve varsayılan uygulama havuzunu zaten .NET 4'e ayarlayın, geçin [sonraki bölümde](#sqlexpress).

1. Kullanarak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) IIS ve Web dağıtımı, Web Platformu yükleyicisi, IIS için önerilen bir yapılandırma yükler ve IIS ve Web önkoşulları otomatik olarak yükler çünkü yüklemek için tercih edilen yolu Gerekirse dağıtın.

    IIS ve Web dağıtımı yüklemek için Web Platformu yükleyicisi çalıştırmak için aşağıdaki bağlantıyı kullanın. IIS, Web dağıtımı veya herhangi birini gerekli bileşenleri zaten yüklediyseniz, Web Platformu yükleyicisi yalnızca neyin eksik olduğu yükler.

   - [IIS ve Webpı kullanarak Web dağıtımı yükleme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     IIS 7 yüklü olduğunu belirten bir ileti görürsünüz. Bağlantı works Windows 8'de IIS 8, ancak Windows 8 için ASP.NET 4.5 aşağıdaki adımları gerçekleştirerek yüklendiğinden emin olun:

   - Açık **Denetim Masası**, **programlar ve Özellikler**, **kapatma Windows özelliklerini aç veya Kapat**.
   - Genişletme **Internet Information Services**, **World Wide Web Hizmetleri**, ve **uygulama geliştirme özellikleri**.
   - Olduğundan emin olun **ASP.NET 4.5** seçilir.

      ![ASP.NET 4.5 seçin](deploying-to-iis/_static/image1.png)

IIS'yi yükledikten sonra çalıştırmak **IIS Yöneticisi'ni** .NET Framework sürüm 4 varsayılan uygulama havuzuna atanmış olduğundan emin olmak için.

1. Açmak için WINDOWS + R tuşuna basın **çalıştırmak** iletişim kutusu.

    (Veya "Çalıştır" Windows 8'de girin **Başlat** sayfasında ya da Windows 7'de seçin **çalıştırmak** gelen **Başlat** menüsü. Varsa **çalıştırmak** değil **Başlat** menüsünde, görev çubuğunun sağ tıklayın, **özellikleri**seçin **Başlat menüsü** sekmesini ve ardından **Özelleştir**seçip **komutu çalıştırın**.)
2. "İnetmgr" girin ve ardından **Tamam**.
3. İçinde **bağlantıları** bölmesinde sunucu düğümünü genişletin ve seçin **uygulama havuzları**. İçinde **uygulama havuzları** bölmesinde, varsa **DefaultAppPool** olan .NET framework sürüm 4 Aşağıdaki çizimde olduğu gibi atanan, sonraki bölüme atlayın.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. IIS'de ASP.NET 4 yüklemek zorunda yalnızca iki uygulama havuzları görürseniz ve bunların her ikisi de .NET Framework 2.0 için ayarlanır.

    Bu ASP.NET 4.5 emin yüklüdür bölümünde veya bkz yönergeler önceki Windows 8 için bkz [Bu KB makalesi](https://support.microsoft.com/kb/2736284). Windows 7 için sağ tıklayarak bir komut istemi penceresi açın **komut istemi** Windows **Başlat** menü ve seçerek **yönetici olarak çalıştır**. Ardından çalıştırın [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) aşağıdaki komutları kullanarak IIS, ASP.NET 4 yüklemek için. (32-bit sistemlerinde "Framework64" "Framework" ile değiştirin.)

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Bu komut, yeni uygulama havuzları için .NET Framework 4 oluşturur, ancak varsayılan uygulama havuzunu hala 2.0 için ayarlanacak. Böylece uygulama havuzunu .NET 4'e değiştirmek zorunda, bir uygulama hedefleyen .NET 4, bu uygulama havuzuna dağıtma.
5. Kapattıysanız **IIS Yöneticisi'ni**yeniden çalıştırın, sunucu düğümünü genişletin ve tıklatın **uygulama havuzları** görüntülemek için **uygulama havuzları** yeniden bölmesi.
6. İçinde **uygulama havuzları** bölmesinde tıklatın **DefaultAppPool**ve ardından **Eylemler** bölmede **temel ayarları**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. İçinde **uygulama havuzunu Düzenle** iletişim kutusu, değişiklik **.NET Framework sürüm** için **.NET Framework v4.0. 30319** tıklatıp **Tamam**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS artık bir web uygulamasına yayımlamak hazır, ancak bunu yapmadan önce test ortamında kullanacağınız veritabanları oluşturmanız gerekir.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Install SQL Server Express

Yerel veritabanı test ortamınız için SQL Server Express'in yüklü olması gerekir. böylece IIS, çalışmak üzere tasarlanmamıştır. Visual Studio 2010 SQL Server Express kullanıyorsanız, zaten varsayılan olarak yüklenir. Visual Studio 2012 kullanıyorsanız, bunu yüklemeniz gerekir.

SQL Server Express'i yükleyebilmek için şuradan yükleyin [İndirme Merkezi: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) tıklayarak [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) veya [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Yanlış bir seçerseniz sisteminizi yükleme başarısız olur ve diğerinde deneyebilirsiniz.

SQL Server Yükleme Merkezi'ni ilk sayfasında **yeni SQL Server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**, varsayılan seçimleri kabul yönergeleri izleyin. Yükleme Sihirbazı'nda, varsayılan ayarları kabul edin. Yükleme seçenekleri hakkında daha fazla bilgi için bkz: [SQL Server 2012 Yükleme Sihirbazı'nı (Kurulum) yükleme](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Sınama ortamı için SQL Server Express veritabanı oluşturma

İki veritabanı Contoso University uygulama vardır: Üyelik veritabanı ve uygulama veritabanı. İki ayrı veritabanlarını veya tek bir veritabanı için bu veritabanlarını dağıtabilirsiniz. Uygulama veritabanınız ve üyelik veritabanınızı arasında veritabanı birleştirme kolaylaştırmak için birleştirip isteyebilirsiniz. Bir üçüncü taraf barındırma sağlayıcısına dağıtıyorsanız, barındırma planınıza bunları birleştirmek için bir neden de sağlayabilir. Örneğin, barındırma sağlayıcısının birden çok veritabanları için daha fazla ücret talep edebilir veya birden fazla veritabanı bile izin vermeyebilir.

Bu öğreticide, test ortamında iki veritabanı ve hazırlama ve üretim ortamlarında bir veritabanına dağıtacaksınız.

Gelen **Görünüm** Visual Studio seçin menüde **Sunucu Gezgini** (**Database Explorer** Visual Web Developer), sağ tıklatın **veri bağlantıları**  seçip **oluşturma yeni SQL Server veritabanı**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

İçinde **yeni SQL Server veritabanı oluşturma** iletişim kutusuna ". \SQLExpress" içinde **sunucu adı** kutusu ve "aspnet-ContosoUniversity" içinde **yeni bir veritabanı adı** kutusunda, ardından tıklatın **Tamam**.

![ASPNET ContosoUniversity oluşturma](deploying-to-iis/_static/image9.png)

"ContosoUniversity" adlı yeni bir SQL Server Express Okul veritabanı oluşturmak için aynı yordamı izleyin.

**Sunucu Gezgini** şimdi iki yeni veritabanları gösterir.

![Server Explorer'da yeni veritabanları](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Yeni veritabanları için grant komut dosyası oluşturma

Uygulama geliştirme bilgisayarınızda IIS içinde çalıştığında, uygulamanın varsayılan uygulama havuzu kimlik bilgileri kullanılarak veritabanına erişir. Ancak, varsayılan olarak, uygulama havuzu kimliği veritabanlarını açma izni yok. Bu nedenle bu izni vermek için bir komut dosyasını çalıştırmanız gerekir. Bu bölümde IIS çalıştırıldığında, uygulama veritabanlarını açabilir emin olmak için sonraki çalıştıracaksınız komut dosyası oluşturun.

Çözüm (değil projelerden biri) sağ tıklayın ve **Yeni Öğe Ekle**ve ardından yeni bir **SQL dosyası** adlı *Grant.sql*. Aşağıdaki SQL komutlarını dosyaya kopyalayın ve ardından dosyasını kaydedip kapatın:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Bu komut, bu öğreticide belirtilerek SQL Server Express 2012 ve Windows 8 veya Windows 7 IIS ayarlarında ile çalışmak üzere tasarlanmıştır. SQL Server veya Windows farklı bir sürümünü kullanıyorsanız veya IIS bilgisayarınızda farklı şekilde ayarlarsanız, bu komut dosyası değişiklikler yapılması. SQL Server komut dosyaları hakkında daha fazla bilgi için bkz: [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Güvenlik Notu** db bu komut dosyası sağlar\_sahip izinleri üretim ortamında sahip olacaksınız olan çalışma zamanında veritabanına erişen kullanıcı. Bazı senaryolarda yalnızca dağıtım izinlerini güncelleştirmek ve okumak ve yazmak için izinlere sahip farklı bir kullanıcı çalışma zamanını belirtin tam veritabanı şemasının sahip bir kullanıcı belirtmek isteyebilirsiniz. Daha fazla bilgi için bkz: [Code First geçişleri için otomatik Web.config değişiklikleri gözden](#reviewingmigrations) Bu öğreticide daha sonra.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uygulama veritabanındaki grant betiği çalıştırın

Bu veritabanı dağıtımı dbDacFx sağlayıcısı kullandığından grant betik dağıtımı sırasında üyelik veritabanında çalıştırmak için yayımlama profili yapılandırabilirsiniz. Betikleri uygulama veritabanının nasıl dağıtıyorsunuz olan Code First Migrations dağıtımı sırasında çalıştırılamıyor. Bu nedenle, el ile dağıtımdan önce uygulama veritabanındaki betiği çalıştırın gerekir.

1. Visual Studio'da açın *Grant.sql* daha önce oluşturduğunuz dosya.
2. **Bağlan**'a tıklayın. 

    ![Düğme Bağlan](deploying-to-iis/_static/image11.png)
3. İçinde **sunucuya Bağlan** iletişim kutusunda, girin *. \SQLExpress* olarak **sunucu adı**ve ardından **Bağlan**.
4. Veritabanı aşağı açılan listeden seçin **ContosoUniversity**ve ardından **yürütme**. 

    ![](deploying-to-iis/_static/image12.png)

Varsayılan uygulama havuzu kimliği yeterli izinlere uygulamayı çalıştırdığında, veritabanı tablolarını oluşturmak Code First geçişleri için uygulama veritabanında artık sahiptir.

## <a name="publish-to-iis"></a>IIS yayımlama

Visual Studio ve Web dağıtımı kullanarak IIS dağıtabilirsiniz birkaç yolu vardır:

- Tek tıklamayla yayımlama Visual Studio'yu kullanın.
- Komut satırından yayımlayın.
- Oluşturma bir *dağıtım paketi* ve IIS Yöneticisi kullanıcı Arabirimi kullanarak yükleyin. Dağıtım paketi oluşan bir *.zip* tüm dosyaları ve IIS'de bir site yüklemek için gereken meta verileri içeren dosya.
- Bir dağıtım paketi oluşturun ve komut satırını kullanarak yükleyin.

Visual Studio'yu ayarlama otomatikleştirirken ayarlamak için önceki öğreticileri dağıtım görevlerini tüm bu yöntemleri için geçerlidir gittiğiniz işlemi. Bu öğreticiler bu yöntemlerin ilk iki kullanacaksınız. Dağıtım paketleri kullanma hakkında daha fazla bilgi için bkz: [bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) Visual Studio ve ASP.NET için Web dağıtımı içerik haritası içindeki.

Yayımlanmadan önce Visual Studio'yu Yönetici modunda çalışır durumda olduğundan emin olun. Görmüyorsanız, **(Yönetici)** başlık çubuğunda Visual Studio'yu kapatın. Windows 8'deki **Başlat** sayfa veya Windows 7'yi **Başlat** menüsünde, kullanmakta olduğunuz Visual Studio sürümünü simgesine sağ tıklayın ve seçin **yönetici olarak çalıştır**. Yönetici modu, yalnızca olduğunda IIS yerel bilgisayarda yayımladığınız yayımlamak için gereklidir.

### <a name="create-the-publish-profile"></a>Yayımlama profili oluştur

1. İçinde **Çözüm Gezgini**, ContosoUniversity projesine (ContosoUniversity.DAL projeye değil) sağ tıklatın ve **Yayımla**.

    **Web'i Yayımla** Sihirbazı görünür.

    ![Web Sihirbazı profili sekmesi yayımlama](deploying-to-iis/_static/image13.png)
2. Aşağı açılan listesinde seçin  **&lt;yeni... &gt;**. (Güncelleştirmesinin yüklü olduğu en son Visual Studio, aşağı açılan liste yoktur ve düğme sıfırdan yeni bir profil oluşturmak için tıklatın **özel**.)
3. İçinde **yeni bir profil** iletişim kutusu, "Test" girin ve ardından **Tamam**.

    Sihirbaz için otomatik olarak ilerler **bağlantı** sekmesi.
4. İçinde **hizmeti URL'si** kutusuna *localhost*.
5. İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*
6. İçinde **hedef URL** kutusuna, girin `http://localhost/ContosoUniversity`

    **Hedef URL** ayar gerekli değildir. Visual Studio uygulama dağıtma sona erdiğinde, bu URL için varsayılan tarayıcı otomatik olarak açılır. Dağıtım sonrasında otomatik olarak açmak için tarayıcı istemiyorsanız, bu kutuyu boş bırakın.
7. Tıklatın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve yerel bilgisayarda IIS bağlanabilir doğrulanamadı.

    Yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

    ![Web Sihirbazı bağlantı sekmesi yayımlama](deploying-to-iis/_static/image14.png)
8. Tıklatın **sonraki** ilerletmek için **ayarları** sekmesi.
9. **Yapılandırma** açılan kutusu dağıtmak üzere derleme yapılandırmasını belirtir. Bu yayın varsayılan değerine ayarlı bırakın. Bu öğreticide hata ayıklama derlemeleri dağıtma olmaz.
10. Genişletme **dosya yayımlama seçeneği**ve ardından **uygulamadan dosyaları dışlama\_veri klasörü**.

    Uygulamayı yerel SQL Server Express örneğinde oluşturulan veritabanlarına erişim test ortamında .mdf dosyaları *uygulama\_veri* klasör.
11. Bırakın **yayımlama sırasında ön derleme** ve **hedefteki ek dosyaları Kaldır** onay kutularının işaretini.

    ![Ayarlar sekmesinde dosya yayımlama seçeneği](deploying-to-iis/_static/image15.png)

    Önceden derleme çoğunlukla çok büyük siteleri için kullanışlı bir seçenektir; Sayfa başlatma süresi site yayımlandıktan sonra bir sayfa istenen ilk kez azaltabilirsiniz.

    Bu ilk dağıtımınızı ve olmayacaktır herhangi bir dosya hedef klasörde henüz bu yana ek dosyaları Kaldır gerek yoktur.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Seçerseniz **hedefteki ek dosyaları Kaldır** aynı siteye bir sonraki dağıtım için önceden dağıtmadan önce hangi dosyaların silinecek görebilmesi önizleme özelliği kullandığınızdan emin olun. Web dağıtımı projenizde silinmiş hedef sunucuda dosyaları siler beklenen davranıştır. Ancak, tüm klasör yapısını kaynak ve hedef klasörler altında karşılaştırılır ve bazı senaryolarda Web dağıtımı silmek istemediğiniz dosyaları silebilir.
    > 
    > Örneğin, sunucu üzerindeki bir alt web uygulamanız varsa kök klasöre bir projeyi dağıttığınızda alt silinir. Contoso.com ana sitede için bir proje ve başka bir proje için bir blog contoso.com/blog adresindeki olabilir. Bir alt klasöre blog uygulamasıdır. Ana site dağıttığınızda hedefteki ek dosyaları Kaldır seçeneğini belirlerseniz, blog uygulama silinir.
    > 
    > Başka bir örnek, uygulamanız için\_veri klasörü silinmiş beklenmedik biçimde. SQL Server Compact gibi belirli veritabanlarını uygulamada veritabanı dosyalarını depolamak\_veri klasörü. İlk dağıtımdan sonra hariç uygulama seçebilmek veritabanı dosyalarını sonraki dağıtımlarda kopyalama korumak istemediğiniz\_Web'i Paketle/Yayımla sekmesinde veri. Seçili hedefteki ek dosyaları Kaldır, veritabanı dosyalarınızı ve uygulama varsa, yaptıktan sonra\_veri klasörü kendisini sonraki yayımladığınızda silinecek.

### <a name="configure-deployment-for-the-membership-database"></a>Üyelik veritabanı için dağıtımı yapılandırma

Aşağıdaki adımları uygulamak **DefaultConnection** veritabanını **veritabanları** iletişim kutusunun bölümü.

1. İçinde **uzak bağlantı dizesi** kutusuna, yeni SQL Server Express üyelik veritabanına işaret eden aşağıdaki bağlantı dizesi girin.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

    Bağlantı dizesini de alabilirsiniz **Sunucu Gezgini**. İçinde **Sunucu Gezgini**, genişletin **veri bağlantıları** seçip  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Veritabanı, ardından gelen **özellikleri** penceresi kopyalama **bağlantı dizesi** değeri. Bağlantı dizesi, silebilirsiniz bir ek ayarı gerekir: `Pooling=False`.
2. Seçin **güncelleştirme veritabanı**.

    Bu, dağıtım sırasında hedef veritabanında oluşturulacak veritabanı şeması neden olur. Aşağıdaki adımlarda çalıştırmak için gereken ek komut belirtin: varsayılan uygulama havuzunu ve veri dağıtmak için bir veritabanı erişim vermek için.
3. Tıklatın **yapılandırma veritabanı güncelleştirmeleri**.
4. İçinde **yapılandırma veritabanı güncelleştirmeleri** iletişim kutusu, tıklatın **SQL komut dosyası Ekle** ve ardından gidin *Grant.sql* çözüm klasöründe daha önce kaydettiğiniz komut dosyası.
5. Eklemek için bu işlemi yinelemeniz *aspnet veri dev.sql* komut dosyası.

    ![Üyelik veritabanı için veritabanı Güncelleştirmeler'i yapılandırma](deploying-to-iis/_static/image16.png)
6. **Kapat**'ı tıklatın.

### <a name="configure-deployment-for-the-application-database"></a>Uygulama veritabanı için dağıtımı yapılandırma

Visual Studio bir Entity Framework algıladığında `DbContext` sınıfı, bir giriş oluşturur **veritabanları** sahip bölüm bir **Code First Migrations yürütme** onay kutusunu yerine bir  **Veritabanını güncelleştirme** onay kutusu. Bu öğretici için önce kod uygulamalı geçişler dağıtım belirtmek için bu onay kutusunu kullanırsınız.

Bazı senaryolarda, kullanıyor olabilecek bir `DbContext` veritabanı ancak veritabanını dağıtmak için geçişler yerine dbDacFx sağlayıcısı kullanmak istiyor. Bu durumda, bkz: [geçişler olmaksızın Code First bir veritabanına nasıl dağıtırım?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) MSDN'de ASP.NET Web dağıtımı SSS.

Aşağıdaki adımları uygulamak **SchoolContext** veritabanını **veritabanları** iletişim kutusunun bölümü.

1. İçinde **uzak bağlantı dizesi** kutusuna, yeni SQL Server Express uygulama veritabanına işaret eden aşağıdaki bağlantı dizesi girin.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

    Uygulama veritabanı bağlantı dizesini de alabilirsiniz **Sunucu Gezgini** üyelik aldığınız aynı şekilde veritabanı bağlantı dizesi.
2. Seçin **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**.

    Bu seçenek belirtmek için Dağıtılmış Web.config dosyası yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcısı. Uygulama veritabanı dağıtımdan sonra ilk kez eriştiğinde bu Başlatıcı veritabanını en son sürüme otomatik olarak güncelleştirir.

### <a name="configure-publish-profile-transforms"></a>Yapılandırma profili dönüşümler yayımlama

1. Tıklatın **Kapat**ve ardından **Evet** değişiklikleri kaydetmek isteyip istemediğinizi sorulduğunda.
2. İçinde **Çözüm Gezgini**, genişletin **özellikleri**, genişletin **PublishProfiles**.
3. Rright tıklatma *Test.pubxml,* ve ardından **eklemek Config dönüştürmesi**.

    ![Config dönüştürmesi menü ekleme](deploying-to-iis/_static/image17.png)

    Visual Studio oluşturur *Web.Test.config* dönüştürme dosyası ve açar.
4. İçinde *Web.Test.config* dönüşüm dosyasında, açtıktan hemen sonra aşağıdaki kodu ekleyin yapılandırma etiketi.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Test kullandığınızda yayımlama profili, "Test" ortam göstergesi bu dönüşüm ayarlar. Dağıtılan sitede "(Test)" sonra "Contoso University" H1 başlığını görürsünüz.
5. Dosyayı kaydedin ve kapatın.
6. Sağ *Web.Test.config* dosya ve tıklayın **Önizleme dönüştürme** , kodlanmış dönüştürme beklenen değişiklikleri üreten emin olmak için.

    **Web.config Önizleme** penceresi, her ikisi de uygulanıyor sonucunu gösterir *Web.Release.config* dönüştürür ve *Web.Test.config* dönüştürür.

### <a name="preview-the-deployment-updates"></a>Dağıtım güncelleştirmeleri Önizleme

1. Açık **Web'i Yayımla** Sihirbazı'nı yeniden (ContosoUniversity projeyi sağ tıklatın ve **Yayımla**).
2. İçinde **Önizleme** sekmesinde, olduğundan emin olun **Test** profilidir halen seçili ve ardından **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için.

    ![Önizleme düğmesi](deploying-to-iis/_static/image18.png)

    ![Yayımlama önizlemesi](deploying-to-iis/_static/image19.png)

    Tıklatarak **Önizleme veritabanı** üyelik veritabanında çalışacak komut dosyaları görmek için bağlantı. (Bu yüzden uygulama veritabanı için Önizleme için hiçbir şey yok kodlar Code First Migrations dağıtımı için çalıştırılır.)
3. Tıklatın **yayımlama**.

    Visual Studio'yu Yönetici modunda değilse, izinleri hata bildiren bir hata iletisi alabilirsiniz. Bu durumda, Visual Studio'yu kapatın, Yönetici modunda açın ve yeniden yayımlamayı deneyin.

    Visual Studio'yu Yönetici modunda değilse **çıkış** başarılı penceresi raporları oluşturmak ve yayımlamak.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    URL'de girdiyseniz **hedef URL** yayımlama profili kutusunu **bağlantı** sekmesi, tarayıcı otomatik olarak açılır IIS'de yerel bilgisayarda çalışan Contoso University giriş sayfası.

## <a name="test-in-the-test-environment"></a>Test ortamında test

Ortam göstergesi "(Test)" gösterdiğine dikkat edin "(geliştirme yerine)", gösterir, *Web.config* dönüştürme ortamı göstergesi için başarılı oldu.

Çalıştırma **Eğitmen** Code First Eğitmen veri veritabanıyla sağlanmış olduğunu doğrulamak için sayfa. Bu sayfayı seçtiğinizde, Code First veritabanı oluşturur ve çalıştırır yüklemek için birkaç dakika sürebilir `Seed` yöntemi. (Uygulama veritabanı henüz erişmeye oldu çünkü giriş sayfasında olduğu durumlarda, bunu alamadık.)

Tıklatın **Öğrenciler** sekmesini dağıtılan veritabanı hiçbir Öğrenciler sahip olduğunu doğrulayın.

Seçin **eklemek Öğrenciler** gelen **Öğrenciler** menüsünde öğrencinin ekleyin ve ardından yeni Öğrenci görüntüleyin **Öğrenciler** sayfasını veritabanına başarıyla yazabilirsiniz doğrulamak için .

Gelen **kurslar** menüsünde, select **güncelleştirme krediler**. **Güncelleştirme krediler** sayfa yönetici izinleri gerekir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "devpwd") oluşturulan yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme krediler** sayfası görüntülenirse, önceki öğreticide oluşturduğunuz yönetici hesabı için test ortamı doğru bir şekilde dağıtıldı doğrular.

Doğrulayın bir *Elmah* klasör bulunmaktadır *c:\inetpub\wwwroot\ContosoUniversity* yalnızca yer tutucu dosyası da içeren klasör.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First geçişleri için otomatik Web.config değişiklikleri gözden geçirin

Açık *Web.config* dağıtılan uygulamayı dosyasında *C:\inetpub\wwwroot\ContosoUniversity* ve dağıtım işlemini Code First geçişleri için otomatik olarak yapılandırdığınız yerde görebilirsiniz veritabanını en son sürüme güncelleştirin.

![](deploying-to-iis/_static/image21.png)

Dağıtım işlemi de veritabanı şeması güncelleştirmek için özel olarak kullanmak Code First geçişleri için yeni bir bağlantı dizesi oluşturuldu:

![Database_Publish bağlantı dizesi](deploying-to-iis/_static/image22.png)

Bu ek bağlantı dizesi veritabanı şema güncelleştirmeleri için bir kullanıcı hesabı ve uygulama veri erişimi için farklı bir kullanıcı hesabı belirtmenizi sağlar. Örneğin, atamalısınız **db\_sahibi** Code First Migrations rolüne ve **db\_datareader** ve **db\_datawriter**uygulamanın rolleri. Veritabanı şeması değiştirmesini uygulamada kötü amaçlı kodlar engeller ortak bir savunma düzeni budur. (Örneğin, bu başarılı bir SQL ekleme saldırısında meydana gelir.) Bu deseni bu öğreticileri tarafından kullanılmaz. Bu desen senaryonuzda uygulamak istiyorsanız, şu adımları izleyerek yapabilirsiniz:

1. İçinde **ayarları** sekmesinde **Web'i Yayımla** Sihirbazı, tam veritabanı şema güncelleştirmesi izinlere sahip bir kullanıcının belirttiği bağlantı dizesi girin ve temizleyin **Bu bağlantı dizesini kullan Çalışma zamanında** onay kutusu. Dağıtılmış Web.config dosyasında bu olur `DatabasePublish` bağlantı dizesi.
2. Çalışma zamanında kullanmak için uygulamayı istediğiniz bağlantı dizesi için bir Web.config dosyası dönüşüm oluşturun.

## <a name="summary"></a>Özet

Artık geliştirme bilgisayarınızda IIS uygulamanıza dağıttıktan ve orada test.

![Test giriş sayfası](deploying-to-iis/_static/image23.png)

Bu, dağıtım işlemi uygulamanın içeriği (dağıtmak için istemediğiniz dosyaları dışında) doğru konuma kopyalanır ve ayrıca, Web dağıtımı IIS dağıtımı sırasında yapılandırıldığını doğrular. Sonraki öğreticide henüz yapılmadı bir dağıtım görev bulduğu daha fazla test çalıştıracaksınız: klasör izinleri ayarlama *Elmah* klasör.

## <a name="more-information"></a>Daha fazla bilgi

Visual Studio'da IIS veya IIS Express çalıştırma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS Express genel bakış](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.NET sitesinde.
- [IIS Express Tanıtımı](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie'nın blogunda.
- [ASP.NET Web projeleri için Web sunucuları Visual Studio'da](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Çekirdek arasındaki farklar IIS ve ASP.NET Geliştirme Sunucusu](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET sitesinde.

Uygulamanızı Orta güven çalıştığında hangi sorunlar hakkında bilgi doğabilecek için bkz: [ASP.NET uygulamalarında barındırma Orta güven](http://www.4guysfromrolla.com/articles/100307-1.aspx) Rolla sitesinden 4 yazarlar üzerinde.

> [!div class="step-by-step"]
> [Önceki](project-properties.md)
> [sonraki](setting-folder-permissions.md)
