---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: IIS'ye bir Test Ortamı - 12 5 dağıtma | Microsoft Docs"
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754160"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: IIS'ye bir Test Ortamı - 12 5 dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, IIS yerel bilgisayardaki bir ASP.NET web uygulamasına dağıtma işlemi gösterilmektedir.

Uygulama geliştirirken, genellikle Visual Studio'da çalıştırarak test edin. Varsayılan olarak, bu Visual Studio geliştirme sunucusu (Cassini olarak da bilinir) kullanmakta olduğunuz anlamına gelir. Visual Studio geliştirme sunucusu Visual Studio'da geliştirme sırasında test etmek kolaylaştırır, ancak tam olarak IIS gibi çalışmaz. Sonuç olarak, Visual Studio'da test, ancak IIS için bir barındırma ortamında dağıtıldığında başarısız olduğunda uygulamanın doğru şekilde çalışacağını mümkündür.

Uygulamanızı daha güvenilir bir şekilde bu şekilde test edebilirsiniz:

1. Visual Studio'da geliştirme sırasında test ettiğinizde, Visual Studio geliştirme sunucusu yerine IIS Express veya tam IIS kullanın. Bu yöntem genellikle daha doğru bir şekilde sitenizi IIS altında nasıl çalışacağını öykünür. Ancak, bu yöntem, dağıtım işleminizi test etmek veya desteklemez dağıtım işleminin sonucu doğru şekilde çalışacağını doğrulama.
2. Daha sonra üretim ortamınıza dağıtmak için kullanacağınız aynı işlemi kullanarak geliştirme bilgisayarınızda IIS uygulamasını dağıtın. Bu yöntem, uygulamanızın doğru şekilde IIS altında çalışacağı doğrulama ek olarak, dağıtım işleminin doğrular.
3. Uygulamayı üretim ortamınıza mümkün olduğunca yakın olan bir test ortamına dağıtın. Bu öğreticiler için üretim ortamında bir üçüncü taraf barındırma sağlayıcısı olduğundan, ideal test ortamı barındırma sağlayıcısına sahip ikinci bir hesabı olacaktır. Yalnızca test için bu ikinci bir hesap kullanırsınız, ancak üretim hesabı olarak da aynı şekilde ayarlanır.

Bu öğretici 2 seçeneğin adımlarını gösterir. Seçenek 3 Kılavuzu sonunda sağlanan [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici ve bu öğreticinin sonunda seçeneği 1 için kaynakların bağlantıları vardır.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Orta güven içinde uygulamayı çalıştırmak için yapılandırma

IIS yükleyip için dağıtmadan önce site daha tipik bir paylaşılan barındırma ortamında olur gibi çalışması için Web.config dosyası ayarı değiştireceksiniz.

Barındırma sağlayıcıları genellikle çalıştırmak, web sitenizi *Orta güven*, yani bu izin verilmiyor yapmak için bazı şeyler vardır. Örneğin, uygulama kodu Windows kayıt defterine erişim ve olamaz okunamıyor veya uygulamanızın klasör hiyerarşisi dışında dosyaları yazma. Varsayılan olarak, uygulamanın çalıştığı *yüksek güven* yerel bilgisayarınızda, yani uygulama üretime dağıtırken başarısız olacağı şeyleri yapabilmesine imkan olabilir. Bu nedenle, daha doğru bir şekilde yansıtmak test ortamı üretim ortamı olmak için uygulamayı Orta güven içinde çalışacak şekilde yapılandıracaksınız.

Uygulamanın Web.config dosyasında, ekleme bir **güven** öğesinde **system.web** öğesi, bu örnekte gösterildiği gibi.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Uygulamayı şimdi Orta güven IIS'de bile yerel bilgisayarınızda çalıştırır. Bu ayar, mümkün olduğunca erken üretim aşamasında hata veren bir şey yapmak için uygulama kodu tarafından girişimleri catch sağlar.

> [!NOTE]
> Entity Framework Code First Migrations'ı kullanıyorsanız veya üzerinin yüklü olduğundan emin sürüm 5.0 sahip olun. Entity Framework sürümü 4.3, geçişler tam güven veritabanı şemasını güncelleştirmek için gerekiyor.


## <a name="installing-iis-and-web-deploy"></a>IIS ve Web yüklenmesini dağıtma

Geliştirme bilgisayarınızda IIS dağıtmak için IIS ve Web dağıtımı yüklenmiş olmalıdır. Bu varsayılan Windows 7 yapılandırmasında dahil edilmez. IIS ve Web dağıtımı zaten yüklediyseniz, sonraki bölüme atlayın.

Kullanarak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) IIS ve Web dağıtımı, Web Platformu yükleyicisi, önerilen bir yapılandırma için IIS yükler ve IIS ve Web için önkoşulları otomatik olarak yükler nedeniyle yüklemek için tercih edilen yolu Gerekirse dağıtın.

IIS ve Web Dağıtımı'nı yüklemek için Web Platformu Yükleyicisi'ni çalıştırmak için aşağıdaki bağlantıyı kullanın. IIS, Web dağıtımı veya tüm gerekli bileşenleri zaten yüklediyseniz, Web Platformu yükleyicisi, yalnızca neler eksik yükler.

- [IIS ve Webpı kullanarak Web dağıtımı yükleme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Varsayılan uygulama havuzunu .NET 4 için ayarlama

IIS'yi yükledikten sonra Çalıştır **IIS Yöneticisi'ni** için .NET Framework sürüm 4 varsayılan uygulama havuzuna atanmış olduğundan emin olun.

Windows gelen **Başlat** menüsünde **çalıştırma**"inetmgr" girin ve ardından **Tamam**. (Varsa **çalıştırma** komut içinde değil, **Başlat** menüsünde, R ve Windows anahtarını açmak için basabilirsiniz. Veya görev çubuğunda sağ tıklayın, **özellikleri**seçin **Başlat menüsü** sekmesinde **Özelleştir**seçip **komutu çalıştırın**.)

İçinde **bağlantıları** bölmesinde sunucu düğümünü genişletin ve seçin **uygulama havuzları**. İçinde **uygulama havuzları** bölmesinde, **DefaultAppPool** olan .NET framework sürüm 4, aşağıdaki resimde olduğu gibi atanan, sonraki bölüme atlayın.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Yalnızca iki uygulama havuzları görürsünüz ve bunların her ikisi de .NET Framework 2.0 için ayarlama, IIS içinde ASP.NET 4'ü yükleyin vardır:

- Bir komut istemi penceresi açın, sağ tıklayarak **komut istemi** Windows içinde **Başlat** menü ve seçerek **yönetici olarak çalıştır**. Ardından çalıştırın [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) için aşağıdaki komutları kullanarak IIS, ASP.NET 4'ü yükleyin. (64-bit sistemlerde "Framework" "Framework64" ile değiştirin.)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Bu komut, .NET Framework 4 için yeni uygulama havuzları oluşturur, ancak varsayılan uygulama havuzunu hala 2.0 değerine ayarlanmış. Uygulama havuzu .NET 4'e değiştirmek zorunda uygulama hedefleyen .NET 4, uygulama havuzu için dağıtım.

Kapattıysanız **IIS Yöneticisi'ni**, yeniden çalıştırın, sunucu düğümünü genişletin ve tıklayın **uygulama havuzları** görüntülenecek **uygulama havuzları** bölmesinde tekrar.

İçinde **uygulama havuzları** bölmesinde tıklayın **DefaultAppPool**ve ardından **eylemleri** bölmede **temel ayarları**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

İçinde **uygulama havuzunu Düzenle** iletişim kutusunda, değişiklik **.NET Framework sürümünü** için **.NET Framework v4.0.30319** tıklatıp **Tamam**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

IIS yayımlamak artık hazırsınız.

## <a name="publishing-to-iis"></a>IIS yayımlama

Visual Studio 2010 ve Web dağıtımı kullanarak dağıtabileceğiniz birkaç yolu vardır:

- Tek tıklamayla yayımlama Visual Studio'yu kullanın.
- Oluşturma bir *dağıtım paketi* ve IIS Yöneticisi kullanıcı Arabirimi kullanarak yükleyin. Dağıtım paketi oluşan bir *.zip* tüm dosyaları ve IIS'de bir site yüklemek için gerekli meta veriler içeren dosya.
- Bir dağıtım paketi oluşturun ve komut satırını kullanarak yükleyin.

Visual Studio'yu otomatikleştirme ayarlamak için önceki öğreticilerdeki dağıtım görevlerini tüm bu üç yöntemi uygular gerçekleştirdiğiniz işlemi. Bu öğreticileri, bu yöntemlerin ilki olan kullanacaksınız. Dağıtım paketleri kullanma hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx).

Yayımlamadan önce Visual Studio'yu Yönetici modunda çalıştığından emin olun. (Windows 7'de **Başlat** menüsünde sürümü kullandığınız Visual Studio simgesine sağ tıklayın ve seçin **yönetici olarak çalıştır**.) Yönetici modunda, yalnızca olduğunda IIS yerel bilgisayarda yayımladığınız yayımlamak için gereklidir.

İçinde **Çözüm Gezgini**, ContosoUniversity projesine (ContosoUniversity.DAL proje değil) sağ tıklatın ve **Yayımla**.

**Web'i Yayımla** Sihirbazı görünür.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Aşağı açılan listesinde seçin  **&lt;yeni... &gt;**.

İçinde **yeni profili** iletişim kutusu, "Test" girin ve ardından **Tamam**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Bu ad, Web.Test.config Orta düğümünün aynı daha önce oluşturduğunuz dosya Dönüştür ' dir. Bu yazışma ne Web.Test.config dönüştürmeler bu profili kullanarak yayımladığınızda uygulanmasına neden olur.

Sihirbaz otomatik olarak ilerler **bağlantı** sekmesi.

İçinde **hizmet URL'si** kutusuna *localhost*.

İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*.

İçinde **hedef URL** kutusuna `http://localhost/ContosoUniversity`.

**Hedef URL** ayarı gerekli değildir. Visual Studio uygulama dağıtımı tamamlandığında, otomatik olarak varsayılan tarayıcınız bu URL açılır. Dağıtımdan sonra otomatik olarak açmak için tarayıcı istemiyorsanız, bu kutuyu boş bırakın.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Tıklayın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve yerel bilgisayarda IIS bağlanabilir doğrulayın.

Yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Tıklayın **sonraki** ilerletmek için **ayarları** sekmesi.

**Yapılandırma** açılan kutudan dağıtmak üzere derleme yapılandırmasını belirtir. Varsayılan değer istediğiniz sürümdür.

Bırakın **hedefteki ek dosyaları Kaldır** onay kutusunu. Bu ilk dağıtımınızı olduğundan, olmayacaktır tüm dosyaları hedef klasöre henüz.

İçinde **veritabanları** bölümünde, bağlantı dizesi kutusunda şu değeri girin **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

Ayrıca altında **SchoolContext**seçin **uygulamak Code First Migrations**. Bu seçeneği belirtmek için Dağıtılmış Web.config dosyasını yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcı. Uygulamayı dağıtımdan sonra ilk kez veritabanına eriştiğinde, bu Başlatıcı veritabanını en son sürüme otomatik olarak güncelleştirir.

Bağlantı dizesi kutusunda **DefaultConnection**, şu değeri girin:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Bırakın **veritabanını Güncelleştir** temizlenir. Üyelik veritabanı uygulamasında .sdf dosyasını kopyalayarak dağıtılacak\_veri ve dağıtım işlemi ile bu veritabanının başka bir şey yapmak istemezsiniz.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Tıklayın **sonraki** ilerletmek için **Önizleme** sekmesi.

İçinde **Önizleme** sekmesinde **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Tıklayın **yayımlama**.

Visual Studio'yu Yönetici modunda değilse, bir izin hatasıyla belirten bir hata iletisi alabilirsiniz. Bu durumda, Visual Studio'yu kapatın, Yönetici modunda açın ve yeniden yayımlamayı deneyin.

Visual Studio'yu Yönetici modunda değilse **çıkış** başarılı penceresi raporlar oluşturun ve yayınlayın.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Tarayıcı, yerel bilgisayarda IIS içinde çalışan Contoso University giriş sayfasına otomatik olarak açılır.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test ortamında test etme

Ortam göstergesi "(Test)" gösterdiğine dikkat edin "(Dev yerine)", gösterir, *Web.config* ortam göstergesi için dönüştürme başarılı oldu.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Çalıştırma **Öğrenciler** dağıtılan veritabanı öğrenci olduğunu doğrulamak için sayfa. Bu sayfa seçtiğinizde Code First veritabanı oluşturur ve çalıştırır çünkü yüklenmesi birkaç dakika sürebilir `Seed` yöntemi. (Henüz veritabanına erişmek uygulamayı denemek olmadı çünkü giriş sayfasında olduğu durumlarda, bunu kaydetmedi.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Çalıştırma **Eğitmenler** sayfa Code First Eğitmen veri veritabanıyla çekirdek değeri oluşturulmuş olduğunu doğrulamak için:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Seçin **ekleme Öğrenciler** gelen **Öğrenciler** menüsünden bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfasına veritabanına başarıyla yazabildiğinizi doğrulayın :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Gelen **kursları** menüsünde **güncelleştirme KREDİLERİ**. **Güncelleştirme KREDİLERİ** sayfa yönetici izinleri gerektirir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "$w0rd Pa'lar") oluşturduğunuz yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme KREDİLERİ** sayfası görüntülendiğinde, önceki öğreticide oluşturduğunuz yönetici hesabı test ortamı için doğru şekilde dağıtıldığını doğrular.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Doğrulayın bir *Elmah* klasör var. yalnızca yer tutucu dosyası da sahip.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First geçişleri için otomatik Web.config değişiklikleri gözden geçirme

Açık *Web.config* dağıtılan uygulamayı dosyasında *C:\inetpub\wwwroot\ContosoUniversity* ve nereye dağıtım işlemi Code First Migrations otomatik olarak yapılandırılmış görebilirsiniz. veritabanını en son sürüme güncelleştirin.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Dağıtım işlemi aynı zamanda da Code First Migrations'veritabanı şemasını güncelleştirmek için özel olarak kullanmak için yeni bir bağlantı dizesi oluşturursunuz:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Bu ek bağlantı dizesi, veritabanı şema güncelleştirmeleri için bir kullanıcı hesabı ve uygulama veri erişimi için farklı bir kullanıcı hesabı belirtmek sağlar. Örneğin, bir veritabanı atayabilirsiniz\_Code First Migrations ve veritabanı sahibi rolüne\_datareader ve db\_uygulamaya datawriter rolleri. Bu, kötü amaçlı olabilecek kod uygulama veritabanı şeması değiştirmesini engelleyen yaygın savunma modelidir. (Örneğin, bu başarılı SQL ekleme saldırısına içinde gerçekleşebilir.) Bu düzen şu öğreticilerden tarafından kullanılmaz. SQL Server Compact uygulanmaz ve bir sonraki Öğreticide bu serideki SQL Server'a geçiş sırasında uygulanmaz. Cytanium site Cytanium sırasında oluşturduğunuz SQL Server veritabanına erişmek için yalnızca bir kullanıcı hesabı sunar. Senaryonuzda Bu tasarımın tamamlayabilirseniz, aşağıdaki adımları uygulayarak yapabilirsiniz:

1. İçinde **ayarları** sekmesinde **Web'i Yayımla** sihirbazında tam veritabanı şema güncelleştirmesini izinleri ile bir kullanıcının belirttiği bağlantı dizesini girin ve Temizle **Bu bağlantı dizesini kullan Çalışma zamanında** onay kutusu. Bu dağıtılmış Web.config dosyasında olur `DatabasePublish` bağlantı dizesi.
2. Bir Web.config dosyası dönüştürme, uygulamanın çalışma zamanında kullanmasını istediğiniz bağlantı dizesi oluşturun.

Artık geliştirme bilgisayarınızda IIS uygulamanızı dağıttıktan ve orada test. Bu, dağıtım işlemi uygulamanın içeriği (dağıtmak için istemediğiniz dosyalar hariç) doğru konuma kopyalanır ve ayrıca, Web dağıtımı IIS dağıtım sırasında yapılandırıldığını doğrular. Sonraki öğreticide, bir dağıtım Görev henüz yapılmadı bulur bir daha fazla test çalıştırması: klasör izinlerini ayarlama *Elmah* klasör.

## <a name="more-information"></a>Daha fazla bilgi

IIS veya IIS Express Visual Studio'da çalıştırma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS Express genel bakış](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.NET sitesinde.
- [IIS Express ile tanışın](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie'nin blogundan.
- [Nasıl yapılır: Visual Studio'da Web projeleri için Web sunucusu belirtin](https://msdn.microsoft.com/library/ms178108.aspx).
- [Çekirdek arasındaki farklar IIS ve ASP.NET Geliştirme Sunucusu](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET sitesinde.
- [ASP.NET MVC veya Web Forms uygulaması IIS 7, 30 saniye içinde test](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) Rick Anderson'un blogunda. Bu giriş neden (Cassini) Visual Studio geliştirme sunucusu ile test IIS Express'te URL'i test olarak kadar güvenilir değil ve neden IIS Express'te URL'i test IIS'de test olarak kadar güvenilir değil örneklerini sağlar.

Orta güven, uygulamanızın çalıştığında hangi sorunlar hakkında bilgi çıkabilecek için bkz: [ASP.NET uygulamalarında barındırma Orta güven](http://www.4guysfromrolla.com/articles/100307-1.aspx) Rolla siteden 4 Guys üzerinde.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [İleri](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
