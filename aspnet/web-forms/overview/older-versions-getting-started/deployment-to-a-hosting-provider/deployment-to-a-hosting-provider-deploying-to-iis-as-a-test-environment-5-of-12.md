---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: bir Test Ortamı - 12 5 IIS dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16050455c161c8ced1f954bfce9c2d9a44c522b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889676"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: IIS'ye bir Test Ortamı - 12 5 dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici, IIS yerel bilgisayardaki bir ASP.NET web uygulamasına dağıtma gösterilmektedir.

Bir uygulama geliştirirken, Visual Studio'da çalıştırarak genellikle sınayın. Varsayılan olarak, bu Visual Studio geliştirme Server (Cassini olarak da bilinir) kullanmakta olduğunuz anlamına gelir. Visual Studio geliştirme sunucusu Visual Studio'da geliştirme sırasında test kolaylaştırır, ancak tam olarak IIS gibi çalışmaz. Sonuç olarak, Visual Studio'da test, ancak bir barındırma ortamında IIS dağıtıldığında başarısız olduğunda bir uygulamanın düzgün çalışacağını mümkündür.

Uygulamanızı daha güvenli şekilde bu şekilde test edebilirsiniz:

1. Visual Studio'da geliştirme sırasında test ettiğinizde Visual Studio geliştirme sunucusu yerine IIS Express'i veya tam IIS kullanın. Bu yöntem genellikle daha doğru bir şekilde siteniz IIS altında nasıl çalışacağını öykünür. Ancak, bu yöntem değil dağıtım işleminizi test veya dağıtım işleminin sonucunu doğru bir şekilde çalışacağını doğrulayın.
2. IIS uygulama geliştirme bilgisayarınızda daha sonra üretim ortamınıza dağıtmak için kullanacağınız aynı işlemi kullanarak dağıtın. Bu yöntem, uygulamanızın doğru IIS altında çalışacağı doğrulama ek olarak, dağıtım işleminin doğrular.
3. Üretim ortamınıza mümkün olduğunca yakın bir test ortamı uygulamayı dağıtın. Üretim ortamında bu öğreticileri için üçüncü taraf barındırma sağlayıcısı olduğundan, ideal sınama ortamı barındırma sağlayıcısına sahip ikinci bir hesabı olacaktır. Yalnızca test etmek için bu ikinci hesap kullanırsınız, ancak üretim hesabı olarak da aynı şekilde ayarlanır.

Bu öğretici 2 seçeneğin adımlarını gösterir. Seçenek 3 için yönergeler sonunda sağlanan [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici, bu öğreticinin sonunda bağlantıları için 1. seçenek kaynakları vardır.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Orta güven çalıştırmak için uygulamayı yapılandırma

IIS yükleyip için dağıtmadan önce sitenin daha tipik bir paylaşılan barındırma ortamında olacak gibi çalışması için bir Web.config dosyası ayarı değiştireceksiniz.

Barındırma hizmeti sağlayıcıları genellikle çalıştırmak, web sitenizi *Orta güven*, yani bu izin verilmeyen yapmak için bazı şeyler vardır. Örneğin, uygulama kodu Windows kayıt defterine erişim ve olamaz okuyamıyor veya uygulamanızın klasör hiyerarşisi dışında olan dosyaları yazamıyor. Varsayılan olarak, uygulamanın çalıştığı *yüksek güven* yerel bilgisayarınızda başka bir deyişle, uygulama üretime dağıttığınızda, hata veren şeyler mümkün olabilir. Bu nedenle, üretim ortamında daha doğru bir şekilde yansıtmak test ortamı yapmak için uygulamayı Orta güven çalışacak şekilde yapılandıracaksınız.

Uygulamanın Web.config dosyasına ekleyerek bir **güven** öğesinde **system.web** Bu örnekte gösterildiği gibi öğesi.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Uygulama artık IIS'de Orta güven içindeki bile yerel bilgisayarınızda çalışır. Bu ayar, olabildiğince erken üretim aşamasında hata veren bir şeyler için uygulama kodu tarafından girişimlerini yakalamak sağlar.

> [!NOTE]
> Entity Framework Code First Migrations kullanıyorsanız, sürüm 5.0 sahip olduğunuzu veya üstü yüklü olun. Entity Framework sürümü 4.3'da, geçişler gerektirir tam güven veritabanı şeması güncelleştirmek için.


## <a name="installing-iis-and-web-deploy"></a>IIS ve Web yüklenmesini dağıtma

Geliştirme bilgisayarınızda IIS dağıtmak için IIS ve Web dağıtımı yüklenmiş olmalıdır. Bu varsayılan Windows 7 yapılandırmada dahil edilmez. IIS ve Web dağıtımı zaten yüklediyseniz, sonraki bölüme atlayın.

Kullanarak [Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx) IIS ve Web dağıtımı, Web Platformu yükleyicisi, IIS için önerilen bir yapılandırma yükler ve IIS ve Web önkoşulları otomatik olarak yükler çünkü yüklemek için tercih edilen yolu Gerekirse dağıtın.

IIS ve Web dağıtımı yüklemek için Web Platformu yükleyicisi çalıştırmak için aşağıdaki bağlantıyı kullanın. IIS, Web dağıtımı veya herhangi birini gerekli bileşenleri zaten yüklediyseniz, Web Platformu yükleyicisi yalnızca neyin eksik olduğu yükler.

- [IIS ve Webpı kullanarak Web dağıtımı yükleme](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Varsayılan uygulama havuzunu .NET 4 ayarlama

IIS'yi yükledikten sonra çalıştırmak **IIS Yöneticisi'ni** .NET Framework sürüm 4 varsayılan uygulama havuzuna atanmış olduğundan emin olmak için.

Windows **Başlat** menüsünde, select **çalıştırmak**"inetmgr" girin ve ardından **Tamam**. (Varsa **çalıştırmak** komut içinde değil, **Başlat** menüsünde, R ve Windows tuşu açmak için basabilirsiniz. Veya görev çubuğunda sağ tıklayın, **özellikleri**seçin **Başlat menüsü** sekmesini tıklatın, **Özelleştir**seçip **komutu çalıştırın**.)

İçinde **bağlantıları** bölmesinde sunucu düğümünü genişletin ve seçin **uygulama havuzları**. İçinde **uygulama havuzları** bölmesinde, varsa **DefaultAppPool** olan .NET framework sürüm 4 Aşağıdaki çizimde olduğu gibi atanan, sonraki bölüme atlayın.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Yalnızca iki uygulama havuzları görmek ve bunların her ikisi de .NET Framework 2.0 ayarlanır, ASP.NET 4 IIS yüklemeniz gerekir:

- Sağ tıklayarak bir komut istemi penceresi açın **komut istemi** Windows **Başlat** menü ve seçerek **yönetici olarak çalıştır**. Ardından çalıştırın [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) aşağıdaki komutları kullanarak IIS, ASP.NET 4 yüklemek için. (64-bit sistemlerinde "Framework" "Framework64" ile değiştirin.)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Bu komut, yeni uygulama havuzları için .NET Framework 4 oluşturur, ancak varsayılan uygulama havuzunu hala 2.0 için ayarlanacak. Böylece uygulama havuzunu .NET 4'e değiştirmek zorunda, bir uygulama hedefleyen .NET 4, bu uygulama havuzuna dağıtma.

Kapattıysanız **IIS Yöneticisi'ni**yeniden çalıştırın, sunucu düğümünü genişletin ve tıklatın **uygulama havuzları** görüntülemek için **uygulama havuzları** yeniden bölmesi.

İçinde **uygulama havuzları** bölmesinde tıklatın **DefaultAppPool**ve ardından **Eylemler** bölmede **temel ayarları**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

İçinde **uygulama havuzunu Düzenle** iletişim kutusu, değişiklik **.NET Framework sürüm** için **.NET Framework v4.0. 30319** tıklatıp **Tamam**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Şimdi IIS yayımlamaya hazırsınız.

## <a name="publishing-to-iis"></a>IIS yayımlama

Visual Studio 2010'u ve Web dağıtımı kullanarak dağıtabilirsiniz birkaç yolu vardır:

- Tek tıklamayla yayımlama Visual Studio'yu kullanın.
- Oluşturma bir *dağıtım paketi* ve IIS Yöneticisi kullanıcı Arabirimi kullanarak yükleyin. Dağıtım paketi oluşan bir *.zip* tüm dosyaları ve IIS'de bir site yüklemek için gereken meta verileri içeren dosya.
- Bir dağıtım paketi oluşturun ve komut satırını kullanarak yükleyin.

Visual Studio'yu ayarlama otomatikleştirirken ayarlamak için önceki öğreticileri dağıtım görevleri bu üç yöntem tümüne uygulanır gittiğiniz işlemi. Bu öğreticiler bu yöntemlerin ilki kullanacaksınız. Dağıtım paketleri kullanma hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx).

Yayımlanmadan önce Visual Studio'yu Yönetici modunda çalışır durumda olduğundan emin olun. (Windows 7'de **Başlat** menüsünde, kullanmakta olduğunuz Visual Studio sürümünü simgesine sağ tıklayın ve seçin **yönetici olarak çalıştır**.) Yönetici modu, yalnızca olduğunda IIS yerel bilgisayarda yayımladığınız yayımlamak için gereklidir.

İçinde **Çözüm Gezgini**, ContosoUniversity projesine (ContosoUniversity.DAL projeye değil) sağ tıklatın ve **Yayımla**.

**Web'i Yayımla** Sihirbazı görünür.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Aşağı açılan listesinde seçin  **&lt;yeni... &gt;**.

İçinde **yeni bir profil** iletişim kutusu, "Test" girin ve ardından **Tamam**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Bu ad Web.Test.config Orta düğümünün aynı dönüşüm daha önce oluşturduğunuz dosyası belirtin. Bu ilişkiyi ne bu profili kullanarak yayımladığınızda, uygulanacak Web.Test.config dönüşümleri neden olur.

Sihirbaz için otomatik olarak ilerler **bağlantı** sekmesi.

İçinde **hizmeti URL'si** kutusuna *localhost*.

İçinde **Site/uygulama** kutusuna *varsayılan Web sitesi/ContosoUniversity*.

İçinde **hedef URL** kutusuna `http://localhost/ContosoUniversity`.

**Hedef URL** ayar gerekli değildir. Visual Studio uygulama dağıtma sona erdiğinde, bu URL için varsayılan tarayıcı otomatik olarak açılır. Dağıtım sonrasında otomatik olarak açmak için tarayıcı istemiyorsanız, bu kutuyu boş bırakın.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Tıklatın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve yerel bilgisayarda IIS bağlanabilir doğrulanamadı.

Yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Tıklatın **sonraki** ilerletmek için **ayarları** sekmesi.

**Yapılandırma** açılan kutusu dağıtmak üzere derleme yapılandırmasını belirtir. İstediğiniz sürüm varsayılan değerdir.

Bırakın **hedefteki ek dosyaları Kaldır** onay kutusunu. Bu ilk dağıtımınızı olduğuna göre olmayacaktır herhangi bir dosya hedef klasörde henüz.

İçinde **veritabanları** bölümünde, bağlantı dizesi kutusunda şu değeri girin **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Dağıtım işlemi bu bağlantı dizesi dağıtılmış Web.config dosyasında çünkü sokar **çalışma zamanında Bu bağlantı dizesini kullan** seçilir.

Ayrıca altında **SchoolContext**seçin **geçerli Code First Migrations**. Bu seçenek belirtmek için Dağıtılmış Web.config dosyası yapılandırmak dağıtım işlemi neden `MigrateDatabaseToLatestVersion` Başlatıcısı. Uygulama veritabanı dağıtımdan sonra ilk kez eriştiğinde bu Başlatıcı veritabanını en son sürüme otomatik olarak güncelleştirir.

Bağlantı dizesi kutusunda **DefaultConnection**, şu değeri girin:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Bırakın **güncelleştirme veritabanı** temizlendi. Üyelik veritabanının uygulamada .sdf dosyasını kopyalayarak dağıtılacak\_veri ve bu veritabanı ile başka bir şey yapmak için dağıtım işlemi istemiyorsanız.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Tıklatın **sonraki** ilerletmek için **Önizleme** sekmesi.

İçinde **Önizleme** sekmesini tıklatın, **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Tıklatın **yayımlama**.

Visual Studio'yu Yönetici modunda değilse, izinleri hata bildiren bir hata iletisi alabilirsiniz. Bu durumda, Visual Studio'yu kapatın, Yönetici modunda açın ve yeniden yayımlamayı deneyin.

Visual Studio'yu Yönetici modunda değilse **çıkış** başarılı penceresi raporları oluşturmak ve yayımlamak.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Tarayıcı, yerel bilgisayarda IIS'de çalışan Contoso University giriş sayfasına otomatik olarak açılır.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Test ortamında test etme

Ortam göstergesi "(Test)" gösterdiğine dikkat edin "(geliştirme yerine)", gösterir, *Web.config* dönüştürme ortamı göstergesi için başarılı oldu.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Çalıştırma **Öğrenciler** sayfa dağıtılan veritabanı hiçbir Öğrenciler sahip olduğunu doğrulayın. Bu sayfayı seçtiğinizde Code First veritabanı oluşturur ve çalıştırır yüklemek için birkaç dakika sürebilir `Seed` yöntemi. (Uygulama veritabanı henüz erişmeye oldu çünkü giriş sayfasında olduğu durumlarda, bunu alamadık.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Çalıştırma **Eğitmen** Code First Eğitmen veri veritabanıyla sağlanmış olduğunu doğrulamak için sayfa:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Seçin **eklemek Öğrenciler** gelen **Öğrenciler** menüsünde öğrencinin ekleyin ve ardından yeni Öğrenci görüntüleyin **Öğrenciler** sayfasını veritabanına başarıyla yazabilirsiniz doğrulamak için :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Gelen **kurslar** menüsünde, select **güncelleştirme krediler**. **Güncelleştirme krediler** sayfa yönetici izinleri gerekir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "Pas$ w0rd") oluşturulan yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme krediler** sayfası görüntülenirse, önceki öğreticide oluşturduğunuz yönetici hesabı için test ortamı doğru bir şekilde dağıtıldı doğrular.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Doğrulayın bir *Elmah* klasörü var. yalnızca yer tutucu dosyasını da ile.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Code First geçişleri için otomatik Web.config değişiklikleri gözden geçirme

Açık *Web.config* dağıtılan uygulamayı dosyasında *C:\inetpub\wwwroot\ContosoUniversity* ve dağıtım işlemini Code First geçişleri için otomatik olarak yapılandırdığınız yerde görebilirsiniz veritabanını en son sürüme güncelleştirin.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Dağıtım işlemi de veritabanı şeması güncelleştirmek için özel olarak kullanmak Code First geçişleri için yeni bir bağlantı dizesi oluşturuldu:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Bu ek bağlantı dizesi veritabanı şema güncelleştirmeleri için bir kullanıcı hesabı ve uygulama veri erişimi için farklı bir kullanıcı hesabı belirtmenizi sağlar. Örneğin, db atamalısınız\_sahip Code First geçişleri ve db rolüne\_datareader ve db\_uygulamaya datawriter rolleri. Veritabanı şeması değiştirmesini uygulamada kötü amaçlı kodlar engeller ortak bir savunma düzeni budur. (Örneğin, bu başarılı bir SQL ekleme saldırısında meydana gelir.) Bu deseni bu öğreticileri tarafından kullanılmaz. SQL Server Compact uygulanmaz ve bu serideki sonraki öğretici SQL Server'a geçirdiğinizde uygulanmaz. Cytanium site Cytanium oluşturma SQL Server veritabanına erişmek için tek bir kullanıcı hesabı sunar. Bu desen senaryonuzda uygulayabilirsiniz istiyorsanız şu adımları izleyerek yapabilirsiniz:

1. İçinde **ayarları** sekmesinde **Web'i Yayımla** Sihirbazı, tam veritabanı şema güncelleştirmesi izinlere sahip bir kullanıcının belirttiği bağlantı dizesi girin ve temizleyin **Bu bağlantı dizesini kullan Çalışma zamanında** onay kutusu. Dağıtılmış Web.config dosyasında bu olur `DatabasePublish` bağlantı dizesi.
2. Çalışma zamanında kullanmak için uygulamayı istediğiniz bağlantı dizesi için bir Web.config dosyası dönüşüm oluşturun.

Artık geliştirme bilgisayarınızda IIS uygulamanıza dağıttıktan ve orada test. Bu, dağıtım işlemi uygulamanın içeriği (dağıtmak için istemediğiniz dosyaları dışında) doğru konuma kopyalanır ve ayrıca, Web dağıtımı IIS dağıtımı sırasında yapılandırıldığını doğrular. Sonraki öğreticide henüz yapılmadı bir dağıtım görev bulduğu daha fazla test çalıştıracaksınız: klasör izinleri ayarlama *Elmah* klasör.

## <a name="more-information"></a>Daha fazla bilgi

Visual Studio'da IIS veya IIS Express çalıştırma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [IIS Express genel bakış](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) IIS.NET sitesinde.
- [IIS Express Tanıtımı](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie'nın blogunda.
- [Nasıl yapılır: Visual Studio'da Web projeleri için Web sunucusunu belirtmek](https://msdn.microsoft.com/library/ms178108.aspx).
- [Çekirdek arasındaki farklar IIS ve ASP.NET Geliştirme Sunucusu](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET sitesinde.
- [ASP.NET MVC veya Web Forms uygulama IIS 7 üzerinde 30 saniye içinde test](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) Rick Anderson'un blogunda. Bu girdi neden (Cassini) Visual Studio geliştirme sunucusu ile test IIS Express'te sınama olarak kadar güvenilir değil ve neden IIS Express'te sınama IIS'de sınama olarak kadar güvenilir değil örnekleri sağlar.

Uygulamanızı Orta güven çalıştığında hangi sorunlar hakkında bilgi doğabilecek için bkz: [ASP.NET uygulamalarında barındırma Orta güven](http://www.4guysfromrolla.com/articles/100307-1.aspx) Rolla sitesinden 4 yazarlar üzerinde.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
