---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Test ortamına dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396343"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Test Ortamına Dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya Visual Studio 2017'yi kullanarak bir üçüncü taraf barındırma sağlayıcısı. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).

## <a name="overview"></a>Genel Bakış

Bu öğreticide, yerel bilgisayarınızda bir ASP.NET web uygulaması için Internet Information Server (IIS) dağıtacaksınız.

Genellikle bir uygulama geliştirdiğinizde, çalıştırmak ve Visual Studio'da test. Varsayılan olarak, web uygulama projeleri Visual Studio 2017'de IIS Express geliştirme web sunucusu olarak kullanacak. IIS Express daha tam IIS Visual Studio geliştirme varsayılan olarak Visual Studio 2017'yi kullanan sunucudan (Cassini olarak da bilinir) gibi davranır. Ancak hiçbiri geliştirme web sunucusu IIS gibi tam olarak çalışır. Sonuç olarak, bir uygulama çalıştırabilir ve doğru şekilde Visual Studio'da test ancak IIS dağıtıldığında başarısız.

Uygulamanızı iki şekilde güvenilir bir şekilde test edebilirsiniz:

1. Daha sonra üretim ortamınıza dağıtmak için kullanacağınız aynı işlemi kullanarak, geliştirme bilgisayarınızda IIS uygulamanızı dağıtın.

   Visual Studio web projesini çalıştırın, ancak dağıtım işleminizi test mıydı IIS kullanmak için yapılandırabilirsiniz. Bu yöntem, dağıtım işlemi doğrular ve uygulamanızı IIS altında düzgün şekilde çalışır.

2. Uygulamanızı üretim ortamınıza benzer bir test ortamına dağıtın. 
 
   Üretim ortamı için bu öğreticileri, Azure App service'taki Web Apps oluyor. İdeal bir test ortamı, Azure hizmetinde oluşturulan bir ek web uygulamasıdır. Bir üretim web uygulaması olarak aynı şekilde ayarlanır ancak test etmek için yalnızca kullanırsınız.

Seçenek 2, test etmek için en güvenilir yoludur. Seçenek 2 kullanıyorsanız, mutlaka 1. seçenek kullanmanız gerekmez. Ancak üçüncü taraf dağıtıyorsanız, barındırma sağlayıcısı, 2. seçenek uygulanabilir olmayabilir veya her iki yöntem de Bu öğretici serisinin gösterecek şekilde pahalı olabilir. 2. seçenek için yönergeler sağlanır [üretim ortamına dağıtma](deploying-to-production.md) öğretici.

Visual Studio'da web sunucuları kullanma hakkında daha fazla bilgi için bkz. [ASP.NET Web projeleri için Visual Studio'daki Web sunucuları](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Anımsatıcı: Bir hata iletisi alırsanız veya öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Contoso University başlangıç projenizi indirin

İndirin ve Contoso University Visual Studio Başlangıç çözüm ve proje yükleyin. Bu çözüm, tamamlanan öğretici içerir. 

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>IIS yükleme

Geliştirme bilgisayarınızda IIS dağıtmak için IIS ve Web dağıtımı yüklendiğinden emin olun. Varsayılan olarak, Visual Studio Web dağıtımı yükler, ancak IIS varsayılan Windows 10, Windows 8 veya Windows 7 yapılandırmasında dahil değildir. IIS zaten yüklediğiniz ve varsayılan uygulama havuzu zaten .NET 4'e ayarlayın, atlamak [sonraki bölümde](#sqlexpress).

1. Kullanmanız önerilir [Web Platformu Yükleyicisi (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) IIS ve Web Dağıtımı'nı yüklemek için. Gerekirse, IIS ve Web dağıtımı önkoşulları içeren bir önerilen IIS yapılandırması WPI yükler.

   IIS, Web dağıtımı ve gereken bileşenleri hiçbirini zaten yüklediyseniz, yalnızca neler eksik WPI yükler.

   * IIS ve Web Dağıtımı'nı yüklemek için Web Platformu Yükleyicisi'ni kullanın:
    
     ![WPI kullanarak IIS yükleme](deploying-to-iis/_static/image30.png)

     ![WPI kullanarak Web Dağıtımı'nı yükleyin](deploying-to-iis/_static/image31.png)

     IIS 7 yüklü olduğunu belirten bir ileti görürsünüz. Windows 8'de IIS 8 için bağlantıyı çalışır; Ancak, Windows 8 ve sonraki sürümlerinde, ASP.NET 4.7 yüklü olduğundan emin olmak için aşağıdaki adımları gidin:

   * Açık **Denetim Masası** > **programlar** > **programlar ve Özellikler** > **kapatma Windows özelliklerini aç veya kapat** .

   * Genişletin **Internet Information Services**, **World Wide Web Hizmetleri**, ve **uygulama geliştirme özellikleri**.
   
   * Onaylayın **ASP.NET 4.7** seçilir.

     ![ASP.NET 4.7 seçin](deploying-to-iis/_static/image1a.png)

   * Onaylayın **World Wide Web Hizmetleri** ve **IIS Yönetim Konsolu** seçilir. Bu, IIS ve IIS Yöneticisi'ni yükler.
    
     ![World Wide Web hizmetleri seçin](deploying-to-iis/_static/image24.png)    
  
   * **Tamam**’ı seçin. Yükleme gerçekleşen belirten bir iletişim kutusu iletileri görüntülenir.

IIS'yi yükledikten sonra Çalıştır **IIS Yöneticisi'ni** için .NET Framework sürüm 4 varsayılan uygulama havuzuna atanmış olduğundan emin olun.

1. Açmak için WINDOWS + R tuşuna basın **çalıştırma** iletişim kutusu.

   (Windows 8 veya daha sonra "Çalıştır" girin **Başlat** sayfası. Windows 7'de seçin **çalıştırma** gelen **Başlat** menüsü. Varsa **çalıştırma** değil **Başlat** menüsünde, görev çubuğunun sağ tıklayın, **özellikleri**seçin **Başlat menüsü** sekmesinde, seçin**Özelleştir**seçip **komutu Çalıştır**.)

2. "İnetmgr" girin ve seçin **Tamam**.

3. İçinde **bağlantıları** bölmesinde sunucu düğümünü genişletin ve seçin **uygulama havuzları**. İçinde **uygulama havuzları** bölmesi ise **DefaultAppPool** olan .NET framework sürüm 4, aşağıdaki resimde olduğu gibi atanan, sonraki bölüme atlayın.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. ASP.NET 4, yalnızca iki uygulama havuzları bakın ve hem de .NET Framework 2.0 için ayarlama, IIS yükleyin.

   Windows 8 veya üzeri yüklü, ASP.NET 4.7 emin olmak için önceki bölümün yönergeleri veya bakın [Windows 8 ve Windows Server 2012'de ASP.NET 4.5 yüklemek nasıl](https://support.microsoft.com/kb/2736284). Windows 7'de, bir komut istemi penceresi açın sağ tıklayarak **komut istemi** Windows içinde **Başlat** menü ve seçerek **yönetici olarak çalıştır**. Çalıştırma [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) için aşağıdaki komutları kullanarak IIS içinde ASP.NET 4'ü yükleyin. (32-bit sistemlerde "Framework64" "Framework" ile değiştirin.)

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Bu komut, yeni uygulama havuzları için .NET Framework 4, ancak varsayılan uygulama havuzunu kalacak 2.0 değerine ayarlanmış oluşturur. Bu uygulama havuzu .NET 4 hedefe şekilde değiştirmenizi uygulama havuzu .NET 4'e bir uygulama dağıtıyorsunuz.

5. Kapattıysanız **IIS Yöneticisi'ni**, yeniden çalıştırın, sunucu düğümünü genişletin ve seçin **uygulama havuzları**.

6. İçinde **uygulama havuzları** bölmesinde **DefaultAppPool**. İçinde **eylemleri** bölmesinde **temel ayarları**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. İçinde **uygulama havuzunu Düzenle** iletişim kutusunda, değişiklik **.NET CLR sürümü** için **.NET CLR v4.0.30319**. **Tamam**’ı seçin.

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Artık IIS'ye bir web uygulaması yayımlamaya hazırsınız. İlk olarak, ancak test etmek için veritabanları oluşturun.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>SQL Server Express yükle

LocalDB, IIS, kendi test ortamınıza yüklü SQL Server Express iznine sahip olması gerekir böylece çalışmak için tasarlanmamıştır. Visual Studio 2010 SQL Server Express kullanıyorsanız, zaten varsayılan olarak yüklenir. Visual Studio 2012 veya sonraki bir sürümü kullanıyorsanız, SQL Server Express'i yükler.

SQL Server Express'i yükleyebilmek için indirin ve yükleyin [İndirme Merkezi: Microsoft SQL Server Express 2017 sürümü](https://www.microsoft.com/sql-server/sql-server-editions-express). 

SQL Server Yükleme Merkezi'ni ilk sayfasında **yeni SQL Server tek başına yükleme veya mevcut bir yüklemeye özellikler ekleme** ve varsayılan seçimleri kabul yönergeleri izleyin. Yükleme Sihirbazı'nda, varsayılan ayarları kabul edin. Yükleme seçenekleri hakkında daha fazla bilgi için bkz. [SQL Server'dan yükleme (Kurulum) Yükleme Sihirbazı](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Test ortamı için SQL Server Express veritabanı oluşturma

Contoso University uygulama iki veritabanı sahiptir: 

1. Üyelik veritabanı 
2. Uygulama veritabanı 

Bu veritabanları, iki ayrı veritabanlarına veya tek bir veritabanı dağıtabilirsiniz. Bunları birleştirerek daha kolay bir şekilde bunlar arasında yapılan birleştirmeleri yapar. 

Bir üçüncü taraf barındırma sağlayıcısına dağıtıyorsanız, barındırma planı da, bunları birleştirmek için bir neden verebilir. Örneğin, sağlayıcı, daha fazla bilgi için birden çok veritabanı göre ücretlendiriyor olabilir veya birden fazla veritabanı bile izin vermeyebilir.

Bu öğreticide, iki veritabanı test ortamında ve hazırlama ve üretim ortamlarında bir veritabanı dağıtacaksınız.

Gelen **görünümü** Visual Studio'da seçim menüsünde **Sunucu Gezgini** (**veritabanı Gezgini** Visual Web Developer). Sağ **veri bağlantıları** seçip **yeni SQL Server veritabanı oluşturma**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

İçinde **yeni SQL Server veritabanı oluşturma** iletişim kutusuna ". \SQLExpress" içinde **sunucu adı** kutusu ve "aspnet-ContosoUniversity" içinde **yeni veritabanı adı** kutusu. **Tamam**’ı seçin.

![ASP.NET ContosoUniversity oluşturma](deploying-to-iis/_static/image9.png)

Adlı yeni bir SQL Server Express School veritabanını oluşturmak için aynı yordamı izleyin `ContosoUniversity`.

**Sunucu Gezgini** iki yeni veritabanı gösterir.

![Sunucu Gezgini'ndeki yeni veritabanları](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Yeni veritabanları için bir verme betiği oluşturma

Uygulama geliştirme bilgisayarınızda IIS çalışırken, uygulama varsayılan uygulama havuzu kimlik bilgileri veritabanına erişmek için kullanır. Ancak, varsayılan olarak, uygulama havuzu veritabanlarını açma iznine sahip değil. Başka bir deyişle, bu izni vermek için bir komut dosyası çalıştırmanız gerekir. Bu bölümde, bu komut dosyası oluşturabilir ve daha sonra IIS çalıştırıldığında, uygulama veritabanlarını açabildiğinizden emin olmak için çalıştırın.

Bir metin düzenleyicisinde, aşağıdaki SQL komutlarını yeni bir dosyaya kopyalayın ve kaydedileceği *Grant.sql*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

Visual Studio'da Contoso University çözümü açın. Çözüm (değil projelerden birine) sağ tıklayın ve seçin **Ekle**. Seçin **var olan öğe**, Gözat *Grant.sql*ve açın.

> [!NOTE]
> Bu öğreticide belirtildikleri gibi bu betik, SQL Server Express 2012 ile iş veya üzeri ve Windows 10, Windows 8 veya Windows 7 IIS ayarlarında tasarlanmıştır. SQL Server veya Windows farklı bir sürümünü kullanıyorsanız veya IIS bilgisayarınızda farklı şekilde ayarlarsanız, bu komut dosyası için değişiklik gerekli olabilir. SQL Server komut dosyaları hakkında daha fazla bilgi için bkz. [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Güvenlik Notu** bu betik verir `db_owner` üretim ortamına sahip olacaksınız olan çalışma zamanında veritabanına erişen kullanıcı izinleri. Bazı senaryolarda, yalnızca dağıtım izinlerini güncelleştirmek ve yalnızca veri okuma ve yazma izinlerine sahip farklı bir kullanıcı çalışma zamanını belirtin tam veritabanı şeması olan bir kullanıcının belirtmek isteyebilirsiniz. Daha fazla bilgi için [otomatik Web.config değişiklikleri gözden geçirme için Code First Migrations](#reviewingmigrations) Bu öğreticinin sonraki bölümlerinde.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Uygulama veritabanında verme betiği çalıştırın

Bu veritabanı dağıtımı kullandığından verme betik dağıtımı sırasında üyelik veritabanında çalıştırmak için yayımlama profili yapılandırabilirsiniz [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) sağlayıcısı. Uygulama veritabanının nasıl dağıtıyorsanız Code First Migrations dağıtımı sırasında komut çalıştırılamıyor. Bu, dağıtım öncesinde bir betik uygulama veritabanında el ile çalıştırmak sahip olduğunuz anlamına gelir.

1. Visual Studio'da açın *Grant.sql* daha önce oluşturduğunuz bir dosya.

2. **Bağlan**’ı seçin. 

    ![Bağlanma düğmesi](deploying-to-iis/_static/image11.png)

3. İçinde **sunucuya Bağlan** iletişim kutusuna *. \SQLExpress* olarak **sunucu adı**. **Bağlan**’ı seçin.

4. Veritabanı aşağı açılan listeden seçin **ContosoUniversity**. **Yürüt**’ü seçin. 

   ![](deploying-to-iis/_static/image12.png)

Varsayılan uygulama havuzu kimliği artık uygulama veritabanında Code First Migrations'uygulama çalıştığında, veritabanı tablolarını oluşturmak yeterli izinlere sahip.

## <a name="publish-to-iis"></a>IIS yayımlama

Visual Studio ve Web dağıtımı kullanarak IIS dağıtabileceğiniz birkaç yolu vardır:

* Tek tıklamayla yayımlama Visual Studio'yu kullanın.
* Komut satırından yayımlayın.
* Bir dağıtım paketini oluşturmak ve IIS Yöneticisi'ni kullanarak yükleyin. Paketin bir .zip dosyası tüm dosyaları ve IIS'de bir site yüklemek için gereken meta verileri gerekir.
* Bir dağıtım paketi oluşturun ve komut satırını kullanarak yükleyin.

Visual Studio'yu otomatikleştirme ayarlamak için önceki öğreticilerdeki dağıtım görevlerini geçerli tüm bu yöntemleri için gerçekleştirdiğiniz işlemi. Aşağıdaki öğreticilerde, ilk iki yöntem kullanacaksınız. Dağıtım paketleri kullanma hakkında daha fazla bilgi için bkz: [bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) Visual Studio ve ASP.NET için Web dağıtımı içerik haritası'nda.

Yayımlamadan önce Yönetici modunda Visual Studio çalıştırdığınızdan emin olun. Görmüyorsanız **(Yönetici)** başlık çubuğunda Visual Studio'yu kapatın. Windows 8 (veya üzeri) **Başlat** sayfası veya Windows 7'yi **Başlat** menüsünde, Visual Studio simgesine sağ tıklayın ve **yönetici olarak çalıştır**. Yönetici modunda, yalnızca yerel bilgisayarda IIS yayımlarken, yayımlama için gereklidir.

### <a name="create-the-publish-profile"></a>Yayımlama profilini oluşturma

1. İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje (değil **ContosoUniversity.DAL** Proje). **Yayımla**’yı seçin. **Yayımla** sayfası görüntülenir.

2. Seçin **yeni profili**. **Yayımlama hedefi seçin** iletişim kutusu görüntülenir.

3. Seçin **IIS, FTP, vb.**. Seçin **profili oluşturma**. **Yayımla** Sihirbazı görünür.

   ![Web Sihirbazı profili sekmesinde Yayımla](deploying-to-iis/_static/image26.png)

4. Gelen **yayımlama yöntemi** açılan menüsünde, select **Web dağıtımı**.

5. İçin **sunucu**, girin *localhost*.

6. İçin **Site adı**, girin *varsayılan Web sitesi/ContosoUniversity*.

7. İçin **hedef URL**, girin *http://localhost/ContosoUniversity*.

   **Hedef URL** ayarı gerekli değildir. Visual Studio uygulama dağıtımı tamamlandığında, otomatik olarak varsayılan tarayıcınız bu URL açılır. Dağıtımdan sonra otomatik olarak açmak için tarayıcı istemiyorsanız, bu kutuyu boş bırakın.

8. Seçin **bağlantıyı doğrula** ayarlarının doğru olduğundan ve yerel bilgisayarda IIS bağlanabilir doğrulayın.

   Yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

   ![Web Sihirbazı bağlantı sekmesi yayımlama](deploying-to-iis/_static/image27.png)

9. Seçin **sonraki** ilerletmek için **ayarları** sekmesi.

10. **Yapılandırma** açılan kutudan dağıtmak üzere derleme yapılandırmasını belirtir. Varsayılan değerine ayarlanmış olarak bırakın **yayın**. Bu öğreticide hata ayıklama yapıları dağıtmak gerekmez.

11. Genişletin **dosya Yayımlama seçenekleri**. Seçin **uygulamadan dosyaları dışlama\_veri klasörü**.

    Test ortamında uygulamanın yerel SQL Server Express örneği, .mdf dosyaları değil oluşturduğunuz veritabanları erişen *uygulama\_veri* klasör.

12. Bırakın **yayımlama sırasında ön derleme** ve **hedefteki ek dosyaları Kaldır** onay kutularının.

    ![Ayarlar sekmesinde dosya yayımlama seçeneği](deploying-to-iis/_static/image15a.png)

    Önceden derleme çoğunlukla büyük siteleri için kullanışlı bir seçenektir. Bu, başlangıç zamanı site yayımlandıktan sonra bir sayfa istenen ilk kez azaltabilirsiniz.

    Bu ilk dağıtımınızı olduğundan ve olmayacaktır tüm dosyaları hedef klasöre henüz ek dosyaları Kaldır gerek yoktur.

    > [!NOTE] 
    > Seçerseniz **hedefteki ek dosyaları Kaldır** aynı site için bir sonraki dağıtım için önceden dağıtmadan önce hangi dosyaların silineceğini belirten görebilmesi için önizleme özelliğini kullandığınızdan emin olun. Web dağıtımı hedef sunucuda projenizde sildiğiniz dosyaları siler beklenen davranıştır. Ancak, tüm klasör yapısını kaynak ve hedef klasörler altında karşılaştırılır; ve bazı senaryolarda, Web dağıtımı silmek istemediğiniz dosyaları silebilirsiniz.
    > 
    > Örneğin bir alt klasör sunucuda bir web uygulaması varsa, bir proje kök klasörüne dağıtırken alt silinecek. Contoso.com konumundaki ana site için bir proje ve contoso.com/blog blog'da için başka bir projeye sahip olabilir. Bir alt klasöre blog uygulamasıdır. Seçerseniz **hedefteki ek dosyaları Kaldır** ana siteye dağıttığınızda, blog uygulama silinecek.
    > 
    > Başka bir örnek, uygulamanız için\_veri klasörü silinmiş beklenmedik bir şekilde. SQL Server Compact gibi belirli veritabanlarını, veritabanı dosyalarını uygulamada depolamak\_veri klasörü. İlk dağıtımdan sonra seçtiğiniz için sonraki dağıtımlarda, veritabanı dosyalarını kopyalama tutmak istemediğiniz **hariç uygulama\_veri** Web'i Paketle/Yayımla sekmesinde. Olması durumunda yaptıktan sonra sahip olduğunuz **hedefteki ek dosyaları Kaldır** seçili, veritabanı dosyalarınızı ve uygulama\_sonraki yayımladığınızda veri klasörü kendisini silinecektir.

### <a name="configure-deployment-for-the-membership-database"></a>Üyelik veritabanı dağıtımı yapılandırma

Aşağıdaki adımları uygulamak **DefaultConnection** iletişim kutusunun veritabanında **veritabanları** bölümü.

1. İçinde **uzak bağlantı dizesi** kutusunda, yeni SQL Server Express üyelik veritabanına işaret eden bağlantı dizesi girin.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   Dağıtım işlemi olduğundan bu bağlantı dizesini dağıtılmış Web.config dosyasına koyar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

    Bağlantı dizesinden da edinebilirsiniz **Sunucu Gezgini**. İçinde **Sunucu Gezgini**, genişletme **veri bağlantıları** seçip  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Veritabanı, ardından gelen **özellikleri** penceresi kopyalama **bağlantı dizesi** değeri. Bağlantı dizesi silebilmeniz için bir ek ayar gerekir: `Pooling=False`.

2. Seçin **veritabanını Güncelleştir**.

   Bu, dağıtım sırasında hedef veritabanında oluşturulacak veritabanı şemasını neden olur. Sonraki adımlarda çalıştırmak için gereken ek komut dosyalarını belirtin: varsayılan uygulama havuzunu ve verileri dağıtmak için bir veritabanı erişim vermek için.

3. Seçin **yapılandırma veritabanı güncelleştirmeleri**.
 
4. İçinde **veritabanı güncellemelerini yapılandırma** iletişim kutusunda **SQL komut dosyası Ekle**. Gidin *Grant.sql* çözüm klasöründe daha önce kaydettiğiniz betiği.

5. Eklemek için işlemi yineleyin *aspnet veri dev.sql* betiği.

   ![Üyelik veritabanı için veritabanı güncellemelerini yapılandırma](deploying-to-iis/_static/image16.png)

6. Seçin **Kapat**.

### <a name="configure-deployment-for-the-application-database"></a>Uygulama veritabanı için dağıtımı yapılandırma

Visual Studio, Entity Framework algıladığında `DbContext` sınıfı, bir giriş oluşturur **veritabanları** olan bölüm bir **Code First Migrations yürütme** onay kutusunu yerine bir  **Veritabanını Güncelleştir** onay kutusu. Bu öğreticide, Code First Migrations dağıtım belirtmek için bu onay kutusunu kullanacaksınız.

Bazı senaryolarda, kullanıyor olabilecek bir `DbContext` veritabanı ancak dbDacFx sağlayıcısı veritabanını dağıtmak için geçişler yerine kullanmak istediğiniz. Bu durumda bkz [Code First geçişleri veritabanından nasıl dağıtırım?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) MSDN üzerinde ASP.NET Web dağıtımı SSS.

Aşağıdaki adımları uygulamak **SchoolContext** iletişim kutusunun veritabanında **veritabanları** bölümü.

1. İçinde **uzak bağlantı dizesi** kutusunda, yeni SQL Server Express uygulama veritabanını gösteren bağlantı dizesi girin.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   Dağıtım işlemi olduğundan bu bağlantı dizesini dağıtılmış Web.config dosyasına koyar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

   Uygulama veritabanı bağlantı dizesini de alabilirsiniz **Sunucu Gezgini** aynı şekilde üyelik veritabanı bağlantı dizesi alındı.

2. Seçin **yürütme Code First Migrations (uygulama başlatılırken çalışır)**.

   Bu seçeneği belirtmek için Dağıtılmış Web.config dosyasını yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcı. Uygulamayı dağıtımdan sonra ilk kez veritabanına eriştiğinde, bu Başlatıcı veritabanını en son sürüme otomatik olarak güncelleştirir.

### <a name="configure-publish-profile-transforms"></a>Yapılandırma profili dönüşümler yayımlama

1. Seçin **Kapat**. Seçin **Evet** yaptığınız değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda.

2. İçinde **Çözüm Gezgini**, genişletme **özellikleri**, genişletme **PublishProfiles**.

3. Sağ *CustomProfile.pubxml* ve yeniden adlandırmak *Test.pubxml*.

4. Sağ *Test.pubxml*. Seçin **yapılandırma dönüşümü Ekle**.

   ![Yapılandırma dönüşümü menü ekleme](deploying-to-iis/_static/image17.png)

   Visual Studio oluşturur *Web.Test.config* dönüşüm dosyasını ve onu açar.

5. İçinde *Web.Test.config* dönüşüm dosyasında, açıldıktan hemen sonra aşağıdaki kodu ekleyin yapılandırma etiketi.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Test kullandığınızda yayımlama profili, "Test" ortam göstergesi bu dönüşüm ayarlar. Sitesi dağıtıldı, "(Test)" sonra "Contoso Üniversitesi" H1 başlığını görürsünüz.

6. Dosyayı kaydedin ve kapatın.

7. Sağ *Web.Test.config* seçin ve dosya **Önizleme dönüştürme** , kodlanmış dönüştürme beklenen değişiklikleri üretir emin olmak için.

    **Web.config Önizleme** penceresi gösterir hem de sonucu *Web.Release.config* dönüştürür ve *Web.Test.config* dönüştürür.

### <a name="preview-the-deployment-updates"></a>Dağıtım güncelleştirmeleri Önizleme

1. Açık **Web'i Yayımla** Sihirbazı yeniden (ContosoUniversity projeye sağ tıklayın, **Yayımla**, ardından **Önizleme**).

2. İçinde **Önizleme** iletişim kutusunda **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için. 

   ![Yayımlama önizlemesi](deploying-to-iis/_static/image29.png)

   Belirleyebilirsiniz **veritabanı önizlemesi** üyelik veritabanında çalışacak komut dosyaları görmek için bağlantı. (Yani uygulama veritabanı için Önizleme için hiçbir şey betik Code First Migrations dağıtımı için çalıştırılır.)

3. **Yayımla**’yı seçin.

   Visual Studio'yu Yönetici modunda değilse, izinleri hata iletisi alabilirsiniz. Bu durumda, Visual Studio'yu kapatın, Yönetici modunda açın ve yeniden yayımlamayı deneyin.

   Visual Studio'yu Yönetici modunda değilse **çıkış** başarılı penceresi raporlar oluşturun ve yayınlayın.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   URL girdiyseniz **hedef URL** yayımlama profilini kutusundaki **bağlantı** sekmesi, tarayıcının otomatik olarak açılır bilgisayarınızda IIS içinde çalışan Contoso University giriş sayfasında.

## <a name="test-in-the-test-environment"></a>Test ortamında test edin

Ortam göstergesi "(Test)" gösterdiğine dikkat edin "(Dev yerine)," gösterir, *Web.config* ortam göstergesi için dönüştürme başarılı oldu.

Çalıştırma **Eğitmenler** sayfasına Code First Eğitmen veri veritabanıyla çekirdek değeri oluşturulmuş olduğunu doğrulayın. Bu sayfa seçtiğinizde, Code First veritabanı oluşturur ve çalıştırır çünkü yüklenmesi birkaç dakika sürebilir `Seed` yöntemi. (Henüz veritabanına erişmek uygulamayı denemek olmadı çünkü giriş sayfasında olduğu durumlarda, bunu kaydetmedi.)

Seçin **Öğrenciler** dağıtılan veritabanı öğrenci olduğunu doğrulamak için sekmesinde.

Seçin **ekleme Öğrenciler** gelen **Öğrenciler** menüsü. Bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfası. Bu veritabanına başarıyla yazabilirsiniz doğrular.

Gelen **kursları** menüsünde **güncelleştirme KREDİLERİ**. **Güncelleştirme KREDİLERİ** sayfa yönetici izinleri gerektirir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "devpwd") oluşturduğunuz yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme KREDİLERİ** sayfası görüntülenir. Bu, önceki öğreticide oluşturduğunuz yönetici hesabı test ortamı için doğru şekilde dağıtıldığını doğrular.

Doğrulayın bir *ELMAH* klasör bulunmaktadır *c:\inetpub\wwwroot\ContosoUniversity* yalnızca yer tutucu dosyası da içeren klasör.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First Migrations otomatik Web.config değişiklikleri gözden geçirin

Açık *Web.config* dağıtılan uygulamayı dosyasında *C:\inetpub\wwwroot\ContosoUniversity* ve nereye dağıtım işlemi Code First Migrations otomatik olarak yapılandırılmış görebilirsiniz. veritabanını en son sürüme güncelleştirin.

![](deploying-to-iis/_static/image21.png)

Dağıtım işlemi aynı zamanda da Code First Migrations'veritabanı şemasını güncelleştirmek için özel olarak kullanmak için yeni bir bağlantı dizesi oluşturursunuz:

![Database_Publish bağlantı dizesi](deploying-to-iis/_static/image22.png)

Bu ek bağlantı dizesi, veritabanı şema güncelleştirmeleri için bir kullanıcı hesabı ve uygulama veri erişimi için farklı bir kullanıcı hesabı belirtmek sağlar. Örneğin, atayabilirsiniz **db\_sahibi** Code First Migrations rolüne ve **db\_datareader** ile **db\_datawriter**uygulamanın rolleri. Bu, kötü amaçlı olabilecek kod uygulama veritabanı şeması değiştirmesini engelleyen yaygın savunma modelidir. (Örneğin, bu başarılı SQL ekleme saldırısına içinde gerçekleşebilir.) Bu öğreticiler, bu düzen kullanmayın. Senaryonuzda Bu desen uygulamak için şu adımları uygulayın:

1. İçinde **Web'i Yayımla** altında Sihirbazı **ayarları** sekmesinde, tam veritabanı şema güncelleştirmesini izinleri ile bir kullanıcının belirttiği bağlantı dizesini girin. NET **çalışma zamanında Bu bağlantı dizesini kullan** onay kutusu. Bu dağıtılmış Web.config dosyasında olur `DatabasePublish` bağlantı dizesi.

2. Bir Web.config dosyası dönüştürme, uygulamanın çalışma zamanında kullanmasını istediğiniz bağlantı dizesi oluşturun.

## <a name="summary"></a>Özet

Artık geliştirme bilgisayarınızda IIS, uygulamaya dağıtılan ve var. test.

![Test giriş sayfası](deploying-to-iis/_static/image23.png)

Bu, dağıtım işlemi uygulamanın içeriği (dağıtmak için istemediğiniz dosyalar hariç) doğru konuma kopyalanır ve ayrıca, Web dağıtımı IIS dağıtım sırasında yapılandırıldığını doğrular. Sonraki öğreticide, bir dağıtım Görev henüz yapılmadı bulur bir daha fazla test çalıştırması: klasör izinlerini ayarlama *istasyon ah* klasör.

## <a name="more-information"></a>Daha fazla bilgi

IIS veya IIS Express Visual Studio'da çalıştırma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS Express genel bakış](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.NET sitesinde.
- [IIS Express ile tanışın](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie'nin blogundan.
- [Web sunucuları Visual Studio'da ASP.NET Web projeleri için](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Çekirdek arasındaki farklar IIS ve ASP.NET Geliştirme Sunucusu](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET sitesinde.

Orta güven, uygulamanızın çalıştığında hangi sorunlar hakkında bilgi çıkabilecek için bkz: [ASP.NET uygulamalarında barındırma Orta güven](http://www.4guysfromrolla.com/articles/100307-1.aspx) üzerindeki dört Guys Rolla siteden.

> [!div class="step-by-step"]
> [Önceki](project-properties.md)
> [İleri](setting-folder-permissions.md)
