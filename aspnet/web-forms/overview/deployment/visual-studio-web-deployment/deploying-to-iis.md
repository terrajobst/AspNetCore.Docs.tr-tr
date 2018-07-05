---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Test ortamına dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 8c5034dd4948d96c5722e2dcc960cc0349241a1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365691"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Test ortamına dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, IIS yerel bilgisayardaki bir ASP.NET web uygulamasına dağıtma işlemi gösterilmektedir.

Uygulama geliştirirken, genellikle Visual Studio'da çalıştırarak test edin. Varsayılan olarak, web uygulama projeleri Visual Studio 2012'de IIS Express geliştirme web sunucusu olarak kullanacak. IIS Express daha tam IIS Visual Studio geliştirme varsayılan olarak Visual Studio 2010 kullanan sunucudan (Cassini olarak da bilinir) gibi davranır. Ancak hiçbiri geliştirme web sunucusu IIS gibi tam olarak çalışır. Sonuç olarak, Visual Studio'da test, ancak IIS dağıtıldığında başarısız olduğunda uygulamanın doğru şekilde çalışacağını mümkündür.

Uygulamanızı daha güvenilir bir şekilde bu şekilde test edebilirsiniz:

1. Daha sonra üretim ortamınıza dağıtmak için kullanacağınız aynı işlemi kullanarak geliştirme bilgisayarınızda IIS uygulamasını dağıtın. Visual Studio web projesini çalıştırın, ancak, bunu dağıtım işleminizi test IIS kullanmak için yapılandırabilirsiniz. Bu yöntem, uygulamanızın doğru şekilde IIS altında çalışacağı doğrulama ek olarak, dağıtım işleminin doğrular.
2. Uygulamayı üretim ortamınıza neredeyse aynı olan bir test ortamına dağıtın. Üretim ortamı Bu öğretici için Azure App Service'te Web uygulamaları olduğundan, ideal bir test ortamında Azure App Service'te oluşturduğunuz bir ek web uygulamasıdır. Yalnızca test için bu ikinci bir web uygulamasını kullanırsınız, ancak üretim web uygulaması olarak aynı şekilde ayarlanır.

Seçenek 2 test etmek için en güvenilir yoludur ve bunu yaparsanız, mutlaka seçenek 1 gerekmez. Ancak, için dağıtıyorsanız bir üçüncü taraf barındırma sağlayıcı seçeneği 2 uygulanabilir olmayabilir veya her iki yöntem de Bu öğretici serisinin gösterecek şekilde pahalı olabilir. 2. seçenek için yönergeler sağlanır [üretim ortamına dağıtma](deploying-to-production.md) öğretici.

Visual Studio'da web sunucuları kullanma hakkında daha fazla bilgi için bkz. [ASP.NET Web projeleri için Visual Studio'daki Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="install-iis"></a>IIS yükleme

Geliştirme bilgisayarınızda IIS dağıtmak için IIS ve Web dağıtımı yüklenmiş olmalıdır. Web dağıtımı, Visual Studio ile varsayılan olarak yüklenir, ancak IIS varsayılan Windows 8 veya Windows 7 yapılandırmasında dahil değildir. IIS zaten yüklediyseniz ve varsayılan uygulama havuzunu .NET 4'e zaten ayarlanmış ise atlamak [sonraki bölümde](#sqlexpress).

1. Kullanarak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) IIS ve Web dağıtımı, Web Platformu yükleyicisi, önerilen bir yapılandırma için IIS yükler ve IIS ve Web için önkoşulları otomatik olarak yükler nedeniyle yüklemek için tercih edilen yolu Gerekirse dağıtın.

    IIS ve Web Dağıtımı'nı yüklemek için Web Platformu Yükleyicisi'ni çalıştırmak için aşağıdaki bağlantıyı kullanın. IIS, Web dağıtımı veya tüm gerekli bileşenleri zaten yüklediyseniz, Web Platformu yükleyicisi, yalnızca neler eksik yükler.

   - [IIS ve Webpı kullanarak Web dağıtımı yükleme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     IIS 7 yüklü olduğunu belirten bir ileti görürsünüz. Bağlantı works Windows 8'de IIS 8, ancak Windows 8 için ASP.NET 4.5 aşağıdaki adımları uygulayarak yüklü olduğundan emin olun:

   - Açık **Denetim Masası**, **programlar ve Özellikler**, **kapatma Windows özelliklerini aç veya Kapat**.
   - Genişletin **Internet Information Services**, **World Wide Web Hizmetleri**, ve **uygulama geliştirme özellikleri**.
   - Emin olun **ASP.NET 4.5** seçilir.

      ![ASP.NET 4.5 seçin](deploying-to-iis/_static/image1.png)

IIS'yi yükledikten sonra Çalıştır **IIS Yöneticisi'ni** için .NET Framework sürüm 4 varsayılan uygulama havuzuna atanmış olduğundan emin olun.

1. Açmak için WINDOWS + R tuşuna basın **çalıştırma** iletişim kutusu.

    (Veya Windows 8'de "Çalıştır" girin **Başlat** sayfasında, ya da Windows 7'de **çalıştırma** gelen **Başlat** menüsü. Varsa **çalıştırma** değil **Başlat** menüsünde, görev çubuğunun sağ tıklayın, **özellikleri**seçin **Başlat menüsü** sekmesine,**Özelleştir**seçip **komutu Çalıştır**.)
2. "İnetmgr" girin ve ardından **Tamam**.
3. İçinde **bağlantıları** bölmesinde sunucu düğümünü genişletin ve seçin **uygulama havuzları**. İçinde **uygulama havuzları** bölmesinde, **DefaultAppPool** olan .NET framework sürüm 4, aşağıdaki resimde olduğu gibi atanan, sonraki bölüme atlayın.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Yalnızca iki uygulama havuzları görürsünüz ve bunların her ikisi de .NET Framework 2.0 için ayarlama, IIS içinde ASP.NET 4 yüklemek zorunda.

    Windows 8 için ASP.NET 4.5 sağlamaktan yüklü bölüm veya bkz yönergelere önceki bakın [Bu KB makalesinde](https://support.microsoft.com/kb/2736284). Windows 7'de, bir komut istemi penceresi açın sağ tıklayarak **komut istemi** Windows içinde **Başlat** menü ve seçerek **yönetici olarak çalıştır**. Ardından çalıştırın [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) için aşağıdaki komutları kullanarak IIS, ASP.NET 4'ü yükleyin. (32-bit sistemlerde "Framework64" "Framework" ile değiştirin.)

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Bu komut, .NET Framework 4 için yeni uygulama havuzları oluşturur, ancak varsayılan uygulama havuzunu hala 2.0 değerine ayarlanmış. Uygulama havuzu .NET 4'e değiştirmek zorunda uygulama hedefleyen .NET 4, uygulama havuzu için dağıtım.
5. Kapattıysanız **IIS Yöneticisi'ni**, yeniden çalıştırın, sunucu düğümünü genişletin ve tıklayın **uygulama havuzları** görüntülenecek **uygulama havuzları** bölmesinde tekrar.
6. İçinde **uygulama havuzları** bölmesinde tıklayın **DefaultAppPool**ve ardından **eylemleri** bölmede **temel ayarları**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. İçinde **uygulama havuzunu Düzenle** iletişim kutusunda, değişiklik **.NET Framework sürümünü** için **.NET Framework v4.0.30319** tıklatıp **Tamam**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS, ona bir web uygulaması yayımlamak hazır hale gelir, ancak bunu yapmadan önce test ortamında kullanacağınız veritabanlarını oluşturmanız gerekir.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>SQL Server Express yükle

LocalDB, IIS, test ortamınız için SQL Server Express'in yüklü olması gerekir böylece çalışmak için tasarlanmamıştır. Zaten Visual Studio 2010 SQL Server Express kullanıyorsanız, varsayılan olarak yüklenir. Visual Studio 2012 kullanıyorsanız, bunu yüklemeniz gerekir.

SQL Server Express yüklemesi için buradan yükleyin [İndirme Merkezi: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) tıklayarak [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) veya [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Yanlış olanı tercih ederseniz sisteminiz için yükleme başarısız olur ve diğerinde deneyebilirsiniz.

SQL Server Yükleme Merkezi'ni ilk sayfasında tıklayın **yeni SQL Server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme**, varsayılan seçimleri kabul yönergeleri izleyin. Yükleme Sihirbazı'nda, varsayılan ayarları kabul edin. Yükleme seçenekleri hakkında daha fazla bilgi için bkz. [SQL Server 2012 yükleme (Kurulum) Yükleme sihirbazından](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Test ortamı için SQL Server Express veritabanı oluşturma

İki veritabanı Contoso University uygulama vardır: üyeliği ve uygulama veritabanlarını. Bu veritabanları, iki ayrı veritabanlarına veya tek bir veritabanı dağıtabilirsiniz. Uygulama veritabanınız ve üyelik veritabanınızı arasında yapılan birleştirmeleri kolaylaştırmak için bunları birleştirmek isteyebilirsiniz. Bir üçüncü taraf barındırma sağlayıcısına dağıttığınız durumlarda barındırma planınıza bunları birleştirmek için bir neden de sağlayabilir. Örneğin, barındırma sağlayıcısı, birden fazla veritabanı için daha fazla göre ücretlendiriyor olabilir veya birden fazla veritabanı bile izin vermeyebilir.

Bu öğreticide, hazırlama ve üretim ortamlarında bir veritabanı ve test ortamında iki veritabanı dağıtacaksınız.

Gelen **görünümü** Visual Studio seçim menüsünde **Sunucu Gezgini** (**veritabanı Gezgini** Visual Web Developer) ve ardından sağ tıklayarak **veri bağlantıları**  seçip **yeni SQL Server veritabanı oluşturma**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

İçinde **yeni SQL Server veritabanı oluşturma** iletişim kutusuna ". \SQLExpress" içinde **sunucu adı** kutusu ve "aspnet-ContosoUniversity" içinde **yeni veritabanı adı** kutusunda, ardından tıklayın **Tamam**.

![ASP.NET ContosoUniversity oluşturma](deploying-to-iis/_static/image9.png)

"ContosoUniversity" adlı yeni bir SQL Server Express School veritabanını oluşturmak için aynı yordamı izleyin.

**Sunucu Gezgini** artık iki yeni veritabanı gösterir.

![Sunucu Gezgini'ndeki yeni veritabanları](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Yeni veritabanları için bir verme betiği oluşturma

Uygulama geliştirme bilgisayarınızda IIS çalışırken, uygulama varsayılan uygulama havuzu kimlik bilgileri kullanılarak veritabanına erişir. Bununla birlikte, varsayılan olarak, uygulama havuzu kimliği veritabanlarını açmak için izni yok. Bu nedenle bu izni vermek için bir komut dosyası çalıştırmanız gerekir. Bu bölümde IIS çalıştırıldığında, uygulama veritabanlarını açabildiğinizden emin olmak için sonraki çalıştıracaksınız komut dosyası oluşturun.

Çözüm (değil projelerden birine) sağ tıklayın ve **Yeni Öğe Ekle**ve ardından yeni bir **SQL dosyası** adlı *Grant.sql*. Aşağıdaki SQL komutlarını, dosyaya kopyalayın ve ardından dosyayı kaydedip kapatın:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Bu betik, bu öğreticide belirtildikleri gibi SQL Server Express 2012 ve Windows 8 veya Windows 7 IIS ayarlarında ile çalışacak şekilde tasarlanmıştır. Windows veya SQL Server'ın farklı bir sürümünü kullanıyorsanız veya IIS bilgisayarınızda farklı şekilde ayarlarsanız, bu komut dosyası için değişiklik gerekli olabilir. SQL Server komut dosyaları hakkında daha fazla bilgi için bkz. [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Güvenlik Notu** bu betik db verir\_üretim ortamına sahip olacaksınız olan çalışma zamanında veritabanına erişen bir kullanıcı için sahip izinleri. Bazı senaryolarda yalnızca dağıtım izinlerini güncelleştirmek ve yalnızca veri okuma ve yazma izinlerine sahip farklı bir kullanıcı çalışma zamanını belirtin tam veritabanı şeması olan bir kullanıcının belirtmek isteyebilirsiniz. Daha fazla bilgi için [otomatik Web.config değişiklikleri gözden geçirme için Code First Migrations](#reviewingmigrations) Bu öğreticinin sonraki bölümlerinde.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uygulama veritabanında verme betiği çalıştırın

Yayımlama profili, bu veritabanı dağıtımı dbDacFx sağlayıcısını kullandığından verme betik dağıtımı sırasında üyelik veritabanında çalıştırmak için yapılandırabilirsiniz. Uygulama veritabanının nasıl dağıtıyorsanız Code First Migrations dağıtımı sırasında komut çalıştırılamıyor. Bu nedenle, dağıtım öncesinde bir betik uygulama veritabanında el ile çalıştırmanız gerekir.

1. Visual Studio'da açın *Grant.sql* daha önce oluşturduğunuz bir dosya.
2. **Bağlan**'a tıklayın. 

    ![Bağlanma düğmesi](deploying-to-iis/_static/image11.png)
3. İçinde **sunucuya Bağlan** iletişim kutusuna *. \SQLExpress* olarak **sunucu adı**ve ardından **Connect**.
4. Veritabanı aşağı açılan listeden seçin **ContosoUniversity**ve ardından **yürütme**. 

    ![](deploying-to-iis/_static/image12.png)

Varsayılan uygulama havuzu kimliği artık uygulama veritabanında Code First Migrations'uygulama çalıştığında, veritabanı tablolarını oluşturmak yeterli izinlere sahip.

## <a name="publish-to-iis"></a>IIS yayımlama

Visual Studio ve Web dağıtımı kullanarak IIS dağıtabileceğiniz birkaç yolu vardır:

- Tek tıklamayla yayımlama Visual Studio'yu kullanın.
- Komut satırından yayımlayın.
- Oluşturma bir *dağıtım paketi* ve IIS Yöneticisi kullanıcı Arabirimi kullanarak yükleyin. Dağıtım paketi oluşan bir *.zip* tüm dosyaları ve IIS'de bir site yüklemek için gerekli meta veriler içeren dosya.
- Bir dağıtım paketi oluşturun ve komut satırını kullanarak yükleyin.

Visual Studio'yu otomatikleştirme ayarlamak için önceki öğreticilerdeki dağıtım görevlerini geçerli tüm bu yöntemleri için gerçekleştirdiğiniz işlemi. Bu öğreticileri, bu yöntemlerin ilk iki kullanacaksınız. Dağıtım paketleri kullanma hakkında daha fazla bilgi için bkz: [bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) Visual Studio ve ASP.NET için Web dağıtımı içerik haritası'nda.

Yayımlamadan önce Visual Studio'yu Yönetici modunda çalıştığından emin olun. Görmüyorsanız **(Yönetici)** başlık çubuğunda Visual Studio'yu kapatın. Windows 8'deki **Başlat** sayfası veya Windows 7'yi **Başlat** menüsünde sürümü kullandığınız Visual Studio simgesine sağ tıklayın ve seçin **yönetici olarak çalıştır**. Yönetici modunda, yalnızca olduğunda IIS yerel bilgisayarda yayımladığınız yayımlamak için gereklidir.

### <a name="create-the-publish-profile"></a>Yayımlama profilini oluşturma

1. İçinde **Çözüm Gezgini**, ContosoUniversity projesine (ContosoUniversity.DAL proje değil) sağ tıklatın ve **Yayımla**.

    **Web'i Yayımla** Sihirbazı görünür.

    ![Web Sihirbazı profili sekmesinde Yayımla](deploying-to-iis/_static/image13.png)
2. Aşağı açılan listesinde seçin  **&lt;yeni... &gt;**. (Yüklü en son Visual Studio güncelleştirme ile aşağı açılan liste yoktur ve sıfırdan yeni bir profil oluşturmak için tıklayın düğme **özel**.)
3. İçinde **yeni profili** iletişim kutusu, "Test" girin ve ardından **Tamam**.

    Sihirbaz otomatik olarak ilerler **bağlantı** sekmesi.
4. İçinde **hizmet URL'si** kutusuna *localhost*.
5. İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*
6. İçinde **hedef URL** kutusuna `http://localhost/ContosoUniversity`

    **Hedef URL** ayarı gerekli değildir. Visual Studio uygulama dağıtımı tamamlandığında, otomatik olarak varsayılan tarayıcınız bu URL açılır. Dağıtımdan sonra otomatik olarak açmak için tarayıcı istemiyorsanız, bu kutuyu boş bırakın.
7. Tıklayın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve yerel bilgisayarda IIS bağlanabilir doğrulayın.

    Yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

    ![Web Sihirbazı bağlantı sekmesi yayımlama](deploying-to-iis/_static/image14.png)
8. Tıklayın **sonraki** ilerletmek için **ayarları** sekmesi.
9. **Yapılandırma** açılan kutudan dağıtmak üzere derleme yapılandırmasını belirtir. Bu yayın varsayılan değerine ayarlı bırakın. Bu öğreticide hata ayıklama yapıları dağıtmak gerekmez.
10. Genişletin **dosya yayımlama seçeneği**ve ardından **uygulamadan dosyaları dışlama\_veri klasörü**.

    Uygulama, yerel SQL Server Express örneği oluşturulan veritabanları erişecek test ortamında .mdf dosyaları *uygulama\_veri* klasör.
11. Bırakın **yayımlama sırasında ön derleme** ve **hedefteki ek dosyaları Kaldır** onay kutularının.

    ![Ayarlar sekmesinde dosya yayımlama seçeneği](deploying-to-iis/_static/image15.png)

    Önceden derleme çoğunlukla çok büyük siteleri için kullanışlı bir seçenektir; Bu sayfa başlangıç süresi site yayımlandıktan sonra bir sayfa istenen ilk kez azaltabilirsiniz.

    Bu ilk dağıtımınızı olduğundan ve olmayacaktır tüm dosyaları hedef klasöre henüz ek dosyaları Kaldır gerek yoktur.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Seçerseniz **ek dosyaları Kaldır** aynı site için bir sonraki dağıtım için önceden dağıtmadan önce hangi dosyaların silineceğini belirten görebilmesi için önizleme özelliğini kullandığınızdan emin olun. Web dağıtımı hedef sunucuda projenizde sildiğiniz dosyaları siler beklenen davranıştır. Ancak, tüm klasör yapısını kaynak ve hedef klasörler altında karşılaştırılır ve bazı senaryolarda Web dağıtımı silmek istemediğiniz dosyaları silebilirsiniz.
    > 
    > Örneğin, bir alt klasör sunucuda bir web uygulaması varsa, bir proje kök klasörüne dağıtırken alt silinir. Contoso.com konumundaki ana site için bir proje ve contoso.com/blog blog'da için başka bir projeye sahip olabilir. Bir alt klasöre blog uygulamasıdır. Ana site dağıttığınızda hedefteki ek dosyaları Kaldır seçeneğini belirlerseniz, blog uygulama silinecek.
    > 
    > Başka bir örnek, uygulamanız için\_veri klasörü silinmiş beklenmedik bir şekilde. SQL Server Compact gibi belirli veritabanlarını, veritabanı dosyalarını uygulamada depolamak\_veri klasörü. İlk dağıtımdan sonra veritabanı dosyalarını dışarıda uygulaması'nı seçersiniz sonraki dağıtımlarda kopyalama tutmak istemediğiniz\_Web'i Paketle/Yayımla sekmesinde veri. Seçili hedefteki ek dosyaları Kaldır, veritabanı dosyalarınızı ve uygulama varsa, yaptıktan sonra\_sonraki yayımladığınızda veri klasörü kendisini silinecektir.

### <a name="configure-deployment-for-the-membership-database"></a>Üyelik veritabanı dağıtımı yapılandırma

Aşağıdaki adımları uygulamak **DefaultConnection** veritabanını **veritabanları** ve iletişim kutusunun.

1. İçinde **uzak bağlantı dizesi** kutusunda, yeni SQL Server Express üyelik veritabanına işaret eden bağlantı dizesi girin.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

    Bağlantı dizesinden da edinebilirsiniz **Sunucu Gezgini**. İçinde **Sunucu Gezgini**, genişletme **veri bağlantıları** seçip  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Veritabanı, ardından gelen **özellikleri** penceresi kopyalama **bağlantı dizesi** değeri. Bağlantı dizesi silebilmeniz için bir ek ayar gerekir: `Pooling=False`.
2. Seçin **veritabanını Güncelleştir**.

    Bu, dağıtım sırasında hedef veritabanında oluşturulacak veritabanı şemasını neden olur. Aşağıdaki adımlarda çalıştırmak için gereken ek komut dosyalarını belirtin: varsayılan uygulama havuzunu ve verileri dağıtmak için bir veritabanı erişim vermek için.
3. Tıklayın **yapılandırma veritabanı güncelleştirmeleri**.
4. İçinde **veritabanı güncellemelerini yapılandırma** iletişim kutusu, tıklayın **SQL komut dosyası Ekle** gidin *Grant.sql* çözüm klasöründe daha önce kaydettiğiniz betiği.
5. Eklemek için işlemi yineleyin *aspnet veri dev.sql* betiği.

    ![Üyelik veritabanı için veritabanı güncellemelerini yapılandırma](deploying-to-iis/_static/image16.png)
6. **Kapat**'ı tıklatın.

### <a name="configure-deployment-for-the-application-database"></a>Uygulama veritabanı için dağıtımı yapılandırma

Visual Studio, Entity Framework algıladığında `DbContext` sınıfı, bir giriş oluşturur **veritabanları** olan bölüm bir **Code First Migrations yürütme** onay kutusunu yerine bir  **Veritabanını Güncelleştir** onay kutusu. Bu öğretici için Code First Migrations dağıtım belirtmek için bu onay kutusunu kullanacaksınız.

Bazı senaryolarda, kullanıyor olabilecek bir `DbContext` veritabanı ancak dbDacFx sağlayıcısı veritabanını dağıtmak için geçişler yerine kullanmak istediğiniz. Bu durumda bkz [Code First geçişleri veritabanından nasıl dağıtırım?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) MSDN üzerinde ASP.NET Web dağıtımı SSS.

Aşağıdaki adımları uygulamak **SchoolContext** veritabanını **veritabanları** ve iletişim kutusunun.

1. İçinde **uzak bağlantı dizesi** kutusunda, yeni SQL Server Express uygulama veritabanını gösteren bağlantı dizesi girin.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

    Uygulama veritabanı bağlantı dizesini de alabilirsiniz **Sunucu Gezgini** üyelik aldığınız aynı şekilde veritabanı bağlantı dizesi.
2. Seçin **yürütme Code First Migrations (uygulama başlatılırken çalışır)**.

    Bu seçeneği belirtmek için Dağıtılmış Web.config dosyasını yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcı. Uygulamayı dağıtımdan sonra ilk kez veritabanına eriştiğinde, bu Başlatıcı veritabanını en son sürüme otomatik olarak güncelleştirir.

### <a name="configure-publish-profile-transforms"></a>Yapılandırma profili dönüşümler yayımlama

1. Tıklayın **Kapat**ve ardından **Evet** yaptığınız değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda.
2. İçinde **Çözüm Gezgini**, genişletme **özellikleri**, genişletme **PublishProfiles**.
3. Rright tıklamayla *Test.pubxml,* ve ardından **ekleme Config dönüştürme**.

    ![Yapılandırma dönüşümü menü ekleme](deploying-to-iis/_static/image17.png)

    Visual Studio oluşturur *Web.Test.config* dönüşüm dosyasını ve onu açar.
4. İçinde *Web.Test.config* dönüşüm dosyasında, açıldıktan hemen sonra aşağıdaki kodu ekleyin yapılandırma etiketi.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Test kullandığınızda yayımlama profili, "Test" ortam göstergesi bu dönüşüm ayarlar. Dağıtılan sitede "(Test)" sonra "Contoso Üniversitesi" H1 başlığını görürsünüz.
5. Dosyayı kaydedin ve kapatın.
6. Sağ *Web.Test.config* tıklayın ve dosya **Önizleme dönüştürme** , kodlanmış dönüştürme beklenen değişiklikleri üretir emin olmak için.

    **Web.config Önizleme** penceresi gösterir hem de sonucu *Web.Release.config* dönüştürür ve *Web.Test.config* dönüştürür.

### <a name="preview-the-deployment-updates"></a>Dağıtım güncelleştirmeleri Önizleme

1. Açık **Web'i Yayımla** Sihirbazı yeniden (ContosoUniversity projeyi sağ tıklatıp **Yayımla**).
2. İçinde **Önizleme** sekmesinde, emin **Test** profilidir hala seçili ve ardından **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için.

    ![Önizleme düğmesi](deploying-to-iis/_static/image18.png)

    ![Yayımlama önizlemesi](deploying-to-iis/_static/image19.png)

    Ayrıca **veritabanı önizlemesi** üyelik veritabanında çalışacak komut dosyaları görmek için bağlantı. (Yani uygulama veritabanı için Önizleme için hiçbir şey betik Code First Migrations dağıtımı için çalıştırılır.)
3. Tıklayın **yayımlama**.

    Visual Studio'yu Yönetici modunda değilse, bir izin hatasıyla belirten bir hata iletisi alabilirsiniz. Bu durumda, Visual Studio'yu kapatın, Yönetici modunda açın ve yeniden yayımlamayı deneyin.

    Visual Studio'yu Yönetici modunda değilse **çıkış** başarılı penceresi raporlar oluşturun ve yayınlayın.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    URL girdiyseniz **hedef URL** yayımlama profilini kutusundaki **bağlantı** sekmesi, tarayıcının otomatik olarak açılır yerel bilgisayarda IIS içinde çalışan Contoso University giriş sayfasında.

## <a name="test-in-the-test-environment"></a>Test ortamında test edin

Ortam göstergesi "(Test)" gösterdiğine dikkat edin "(Dev yerine)", gösterir, *Web.config* ortam göstergesi için dönüştürme başarılı oldu.

Çalıştırma **Eğitmenler** sayfasına Code First Eğitmen veri veritabanıyla çekirdek değeri oluşturulmuş olduğunu doğrulayın. Bu sayfa seçtiğinizde, Code First veritabanı oluşturur ve çalıştırır çünkü yüklenmesi birkaç dakika sürebilir `Seed` yöntemi. (Henüz veritabanına erişmek uygulamayı denemek olmadı çünkü giriş sayfasında olduğu durumlarda, bunu kaydetmedi.)

Tıklayın **Öğrenciler** dağıtılan veritabanı öğrenci olduğunu doğrulamak için sekmesinde.

Seçin **ekleme Öğrenciler** gelen **Öğrenciler** menüsünden bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfasına veritabanına başarıyla yazabildiğinizi doğrulayın .

Gelen **kursları** menüsünde **güncelleştirme KREDİLERİ**. **Güncelleştirme KREDİLERİ** sayfa yönetici izinleri gerektirir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "devpwd") oluşturduğunuz yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme KREDİLERİ** sayfası görüntülendiğinde, önceki öğreticide oluşturduğunuz yönetici hesabı test ortamı için doğru şekilde dağıtıldığını doğrular.

Doğrulayın bir *Elmah* klasör bulunmaktadır *c:\inetpub\wwwroot\ContosoUniversity* yalnızca yer tutucu dosyası da içeren klasör.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations otomatik Web.config değişiklikleri gözden geçirin

Açık *Web.config* dağıtılan uygulamayı dosyasında *C:\inetpub\wwwroot\ContosoUniversity* ve nereye dağıtım işlemi Code First Migrations otomatik olarak yapılandırılmış görebilirsiniz. veritabanını en son sürüme güncelleştirin.

![](deploying-to-iis/_static/image21.png)

Dağıtım işlemi aynı zamanda da Code First Migrations'veritabanı şemasını güncelleştirmek için özel olarak kullanmak için yeni bir bağlantı dizesi oluşturursunuz:

![Database_Publish bağlantı dizesi](deploying-to-iis/_static/image22.png)

Bu ek bağlantı dizesi, veritabanı şema güncelleştirmeleri için bir kullanıcı hesabı ve uygulama veri erişimi için farklı bir kullanıcı hesabı belirtmek sağlar. Örneğin, atayabilirsiniz **db\_sahibi** Code First Migrations rolüne ve **db\_datareader** ve **db\_datawriter**uygulamanın rolleri. Bu, kötü amaçlı olabilecek kod uygulama veritabanı şeması değiştirmesini engelleyen yaygın savunma modelidir. (Örneğin, bu başarılı SQL ekleme saldırısına içinde gerçekleşebilir.) Bu düzen şu öğreticilerden tarafından kullanılmaz. Senaryonuzda bu deseni uygulamak istiyorsanız, aşağıdaki adımları uygulayarak yapabilirsiniz:

1. İçinde **ayarları** sekmesinde **Web'i Yayımla** sihirbazında tam veritabanı şema güncelleştirmesini izinleri ile bir kullanıcının belirttiği bağlantı dizesini girin ve Temizle **Bu bağlantı dizesini kullan Çalışma zamanında** onay kutusu. Bu dağıtılmış Web.config dosyasında olur `DatabasePublish` bağlantı dizesi.
2. Bir Web.config dosyası dönüştürme, uygulamanın çalışma zamanında kullanmasını istediğiniz bağlantı dizesi oluşturun.

## <a name="summary"></a>Özet

Artık geliştirme bilgisayarınızda IIS uygulamanızı dağıttıktan ve orada test.

![Test giriş sayfası](deploying-to-iis/_static/image23.png)

Bu, dağıtım işlemi uygulamanın içeriği (dağıtmak için istemediğiniz dosyalar hariç) doğru konuma kopyalanır ve ayrıca, Web dağıtımı IIS dağıtım sırasında yapılandırıldığını doğrular. Sonraki öğreticide, bir dağıtım Görev henüz yapılmadı bulur bir daha fazla test çalıştırması: klasör izinlerini ayarlama *Elmah* klasör.

## <a name="more-information"></a>Daha fazla bilgi

IIS veya IIS Express Visual Studio'da çalıştırma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS Express genel bakış](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.NET sitesinde.
- [IIS Express ile tanışın](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie'nin blogundan.
- [Web sunucuları Visual Studio'da ASP.NET Web projeleri için](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Çekirdek arasındaki farklar IIS ve ASP.NET Geliştirme Sunucusu](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET sitesinde.

Orta güven, uygulamanızın çalıştığında hangi sorunlar hakkında bilgi çıkabilecek için bkz: [ASP.NET uygulamalarında barındırma Orta güven](http://www.4guysfromrolla.com/articles/100307-1.aspx) Rolla siteden 4 Guys üzerinde.

> [!div class="step-by-step"]
> [Önceki](project-properties.md)
> [İleri](setting-folder-permissions.md)
