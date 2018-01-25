---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: (12 / 12) sorunlarını giderme | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: (12 / 12) sorunlarını giderme
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Windows Azure Web sitelerine dağıtma gösterir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


Bu sayfa, Visual Studio kullanarak bir ASP.NET web uygulamasını dağıttığınızda karşılaşabileceğiniz bazı yaygın sorunlar açıklanmaktadır. Her biri için bir veya daha fazla olası nedenleri ve karşılık gelen çözümleri verilmiştir.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Sunucu hatası '/' uygulamasında - geçerli özel hata ayarları hatanın ayrıntılarını uzaktan görüntülenmesine engelliyor

### <a name="scenario"></a>Senaryo

Uzak bir ana bilgisayara bir site dağıttıktan sonra Web.config dosyasında customErrors ayarını değinmektedir ancak hatanın asıl nedenini neydi göstermez bir hata iletisi alırsınız:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Yalnızca yerel bilgisayarda web uygulamanızı çalışırken varsayılan olarak, ASP.NET ayrıntılı hata bilgileri gösterir. Genellikle saldırganlar uygulamada güvenlik açıkları bulmak için bu bilgileri kullanabilmek için olabileceğinden, web uygulamanızın Internet üzerinden genel olarak kullanılabilir olduğunda ayrıntılı hata bilgilerini görüntülemek istediğiniz yok. Ancak, bir siteye bir site veya güncelleştirmeleri dağıtıyorsanız, bazen bir şeyler yanlış geçer ve gerçek hata iletisini almanız gerekir.

Uzak ana bilgisayarda çalıştırılan ayrıntılı hata iletilerini görüntülemek uygulamasını etkinleştirecek şekilde ayarlamak için Web.config dosyasını düzenleyebilir `customErrors` modu kapalı, uygulamanın yeniden dağıtın ve uygulamayı yeniden çalıştırın:

1. Uygulamanın Web.config dosyasını varsa bir `customErrors` öğesinde `system.web` öğe, değişiklik `mode` özniteliğini "kapalı". Aksi halde eklemeniz bir `customErrors` öğesinde `system.web` öğeyle `mode` özniteliği "kapalı" için aşağıdaki örnekte gösterildiği gibi:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Uygulamayı dağıtın.
3. Uygulamayı çalıştırın ve ne olursa olsun, daha önce hata oluşmasına neden oldu yineleyin. Şimdi, gerçek hata iletisi nedir görebilirsiniz.
4. Hata çözdükten sonra özgün geri `customErrors` ayarlama ve uygulamayı yeniden dağıtın.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Bir Web sayfasında kullandığı SQL Server Compact erişim reddedildi

### <a name="scenario"></a>Senaryo

SQL Server Compact kullanan bir siteye dağıtmak ve veritabanına erişir dağıtılan sitede bir sayfa çalıştırıldığında, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

NETWORK SERVICE hesabı sunucuda bulunan SQL Hizmeti Compact yerel ikili dosyaları okuyabilir gerekiyor *bin\amd64* veya *bin\x86* klasörü, ancak okuma bu klasörler için izinleri. Set okuma izni ağ hizmeti üzerinde *bin* klasör, alt klasörler için izinleri genişletmek emin olun.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor

### <a name="scenario"></a>Senaryo

Tıkladığınızda Visual Studio yayımlama IIS yerel makinenizde bir uygulamayı dağıtmak için düğmesini başarısız yayımlama ve **çıkış** penceresi bir hata iletisi bunun benzer gösterir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Tek tıklamayla kullanmak için IIS yerel makinenizde yayımlama, Visual Studio yönetici izinlerine sahip çalıştırıyor olması gerekir. Visual Studio'yu kapatın ve yönetici izinlerine sahip yeniden başlatın.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Hedef bilgisayara bağlanılamadı... Belirtilen işlem kullanma

### <a name="scenario"></a>Senaryo

Tıkladığınızda Visual Studio yayımlama bir uygulamayı dağıtmak için düğmesini başarısız yayımlama ve **çıkış** penceresi bir hata iletisi bunun benzer gösterir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir proxy sunucu hedef sunucuyla iletişim kesintiye. Windows Denetim Masası'ndan veya Internet Explorer'ı seçin **Internet Seçenekleri** seçip **bağlantıları** sekmesi. İçinde **Internet Özellikleri** iletişim kutusu, tıklatın **LAN Ayarları**. İçinde **yerel ağ (LAN) ayarları** iletişim kutusu, Temizle **ayarları otomatik olarak algıla** onay kutusu. Daha sonra yeniden Yayımla düğmesine tıklayın.

Sorun devam ederse, proxy veya güvenlik duvarı ayarlarını yapılabilir belirlemek için sistem yöneticinize başvurun. Web dağıtımı standart olmayan bağlantı noktası için Web Yönetim hizmeti dağıtımı (8172); kullandığından sorun olur diğer bağlantılar için Web dağıtımı, 80 numaralı bağlantı noktasını kullanır. Bir üçüncü taraf barındırma sağlayıcısına dağıtırken, genellikle Web yönetimi hizmeti kullanıyor.

## <a name="default-net-40-application-pool-does-not-exist"></a>Varsayılan .NET 4.0 uygulama havuzu yok

### <a name="scenario"></a>Senaryo

.NET Framework 4 gerektiren bir uygulama dağıttığınızda, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS'de ASP.NET 4 yüklü değil. Uygulamanızı geliştirme bilgisayarınızın dağıttığınız sunucu ise ve Visual Studio 2010 'un yüklü, ASP.NET 4 bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıttığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS ASP.NET 4 yükleyin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Varsayılan uygulama havuzunu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için bkz: [IIS'ye bir Test ortamı olarak dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Öğreticisi.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Başlatma dizesinin biçimi 0 dizininde başlayan belirtime uygun değil.

### <a name="scenario"></a>Senaryo

Tek tıklamayla kullanarak bir uygulamayı dağıttıktan sonra veritabanına erişen bir sayfa çalıştırdığınızda, aşağıdaki hata iletisini aldığınızda, yayımlayın:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Açık *Web.config* bağlantı dizesi değerleri ile başlayan olup olmadığını denetleyin ve dağıtılan site dosyasında `$(ReplacableToken_`, aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Bağlantı dizeleri aşağıdaki örnekte olduğu gibi görünüyorsa, proje dosyasını düzenleyin ve aşağıdaki özelliği Ekle `PropertyGroup` tüm yapılandırmaları derleme için olan öğeyi:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Sonra uygulamayı yeniden dağıtın.

## <a name="http-500-internal-server-error"></a>HTTP 500 İç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, hatanın nedenini gösteren belirli bilgiler olmadan aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

500 hataları pek çok nedeni vardır, ancak bu öğreticileri izliyorsanız olası bir nedeni XML dönüştürme dosyalardan biri yanlış yerinde bir XML öğesi koyduğunuz yerdir. Örneğin, ekler dönüştürme yerleştirirseniz bu hatayı elde edebileceğiniz bir `<location>` öğesinin altında `<system.web>` yerine doğrudan altında `<configuration>`. Bu durumda XML dönüştürme dosyasını düzeltin ve yeniden dağıtmak için çözümüdür.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 iç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Site ASP.NET 4 ancak ASP.NET 4 kaydedilmemiş IIS sunucu üzerinde hedefleri dağıtmış. Sunucusunda yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak ASP.NET 4 kaydedin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Varsayılan uygulama havuzunu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için bkz: [IIS'ye bir Test ortamı olarak dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Öğreticisi.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Oturum açma başarısız açılış SQL Server Express veritabanı uygulamasında\_veri

### <a name="scenario"></a>Senaryo

Güncelleştirilmiş *Web.config* dosya bir SQL Server Express veritabanı işaret etmek için bağlantı dizesi bir *.mdf* dosyasını, *uygulama\_veri* klasörü ve ilk Aşağıdaki hata iletisini görürsünüz uygulama çalışma zamanı:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Adını *.mdf* sildiğiniz olsa bile, dosya şimdiye kadar bilgisayarınızda var SQL Server Express veritabanının adını eşleşemez *.mdf* önceden var olan veritabanının dosya. Adını değiştirmek *.mdf* hiçbir zaman bir veritabanı adı ve değişiklik kullanılmış bir ad dosyasına *Web.config* dosya yeni bir ad kullanın. Alternatif olarak, kullandığınız [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) önceden var olan SQL Server Express silmek için veritabanları.

## <a name="model-compatibility-cannot-be-checked"></a>Model uyumluluğu olamaz denetlenmesi

### <a name="scenario"></a>Senaryo

Güncelleştirilmiş *Web.config* dosya yeni bir SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi ve ilk kez uygulamayı çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Veritabanı adı Web.config dosyasında yerleştirirseniz bilgisayarınızda, bir veritabanı zaten bazı tablolarda ile mevcut olabilir önce herhangi bir zamanda kullanıldı. Bilgisayarınızın önce ve değişiklik üzerinde kullanılmamış olan yeni bir ad seçin *Web.config* dosyayı bu yeni bir veritabanı adı kullanmak için işaretleyin. Alternatif olarak, kullandığınız [SQL Server Express yardımcı programı](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) veya [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) varolan veritabanı silinemiyor.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Kullanıcıları veya rolleriyle oluşturmak bir komut dosyası çalıştığında SQL hatası

### <a name="scenario"></a>Senaryo

Yapılandırılan veritabanı dağıtımı kullanarak **SQL Paketle/Yayımla** sekmesinde, dağıtım sırasında çalışan SQL komut dosyaları dahil kullanıcı oluştur veya rol Oluştur komutları ve komut dosyası yürütme başarısız Bu komutlar çalıştırıldığında. Daha ayrıntılı iletiler, aşağıdaki gibi görebilirsiniz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Veritabanı dağıtımı yapılandırıldığında bu hata oluşursa **Web'i Yayımla** Sihirbazı yerine **SQL Paketle/Yayımla** sekmesinde, bir iş parçacığı oluşturma [yapılandırma ve Dağıtım](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum ve çözüm, bu sorun giderme sayfasına eklenir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtım gerçekleştirmek için kullandığınız kullanıcı hesabı, kullanıcılar ya da roller oluşturma izni yok. Örneğin, barındırma şirketi atayabilecek `db_datareader`, `db_datawriter`, ve `db_ddladmin` sizin için ayarlayan kullanıcı hesabına rolleri. Bunlar yeterli çoğu veritabanı nesneleri oluşturma, ancak kullanıcılar ya da roller oluşturma değil. Hatayı önlemek için bir veritabanı dağıtımından kullanıcılar ve roller hariç tutarak yoludur. Düzenleyerek bunu yapabilirsiniz `PreSource` öğe veritabanı için otomatik olarak oluşturulan betik böylece aşağıdaki öznitelikleri içerir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Nasıl düzenleneceği hakkında bilgi için `PreSource` öğesi proje dosyasında bakın [nasıl yapılır: Proje dosyasında dağıtım ayarlarını Düzenle](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Kullanıcıları veya rolleriyle geliştirme veritabanınızdaki hedef veritabanında olması gerekiyorsa, Yardım için barındırma sağlayıcınızla bağlantı kurun.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server zaman aşımı özel komut dosyalarını dağıtım sırasında çalıştırılırken hata

### <a name="scenario"></a>Senaryo

Dağıtım sırasında çalıştırmak için özel SQL komut dosyaları belirttiğiniz ve Web dağıtımı bunları çalıştığında, bunlar zaman aşımına uğrar.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Farklı bir işlem modları sahip birden çok komut dosyası çalıştırarak zaman aşımı hataları neden olabilir. Varsayılan olarak, bir işlem içinde otomatik olarak oluşturulan komut dosyalarını çalıştırmak, ancak özel komut dosyaları yapın. Seçerseniz **çekme veri ve/veya varolan bir veritabanını şema** seçeneği **SQL Paketle/Yayımla** sekmesinde ve özel SQL komut dosyası eklerseniz, bazı kodlar işlem ayarlarını değiştirmeniz gerekir böylece tüm betikler işlem ayarların aynısını kullanın. Daha fazla bilgi için bkz: [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343.aspx).

Tümü aynı; böylece işlem ayarları yapılandırdınız, ancak bu hatayı almaya devam komut dosyalarını ayrı olarak çalıştırmak için bir olası geçici bir çözüm değildir. İçinde **veritabanı komut dosyalarında** kılavuzunda **Paketle/Yayımla** SQL sekmesi, Temizle **INCLUDE** zaman aşımı hatası neden olan komut dosyası için onay kutusunu işaretleyin, sonra proje yayımlayın. Uygulamasına geri gidin sonra **veritabanı komut dosyalarında** kılavuz, bu komut dosyanızın seçin **INCLUDE** onay kutusunu işaretleyin ve temizleyin **INCLUDE** diğer komutlar için onay kutularını. Sonra projeyi yeniden yayımlayın. Yayımladığınızda, bu zaman yalnızca seçilen özel komut dosyasını çalıştırır.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Site bildiriminin veri akışı henüz kullanılamıyor

### <a name="scenario"></a>Senaryo

Kullanarak bir paket yüklerken *deploy.cmd* ile dosya `t` (test) seçeneği, aşağıdaki hata iletisi görüntülenir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Hata iletisi, komutu bir test raporu üretemez anlamına gelir. Komutu kullanırsanız, ancak çalışabilir `y` (gerçek yükleme) seçeneği. İleti, yalnızca komut test modunda çalışan bir sorun olduğunu gösterir.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Bu uygulama ManagedRuntimeVersion v4.0 gerektirir

### <a name="scenario"></a>Senaryo

Dağıtma girişiminde bulunduğunuzda, aşağıdaki hata iletisini görürsünüz:

 Hata: Veri akışı, ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' henüz kullanılabilir değil. Kullanmaya çalıştığınız uygulama havuzu 'managedRuntimeVersion' özelliği 'v2.0' ayarlanmış. Bu uygulama 'v4.0' gerektirir. 

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS'de ASP.NET 4 yüklü değil. Uygulamanızı geliştirme bilgisayarınızın dağıttığınız sunucu ise ve Visual Studio 2010 'un yüklü, ASP.NET 4 bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıttığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS ASP.NET 4 yükleyin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions yayınlanamıyor

### <a name="scenario"></a>Senaryo

Bir paketi dağıtırken, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS Web dağıtımı 2.0 yüklü olan bir sunucu için Web dağıtımı 1.1 UI kullanarak Yöneticisi'nden dağıtmaya çalıştığınız. Bir paket onay içeri aktararak dağıtmak için IIS Uzak Yönetim Aracı'nı kullanıyorsanız **yeni özellikler kullanılabilir** bağlantısı kurarken iletişim kutusu. (Bu iletişim kutusunu yalnızca bir kez bağlantı ilk ne zaman kurulur görüntülenebilir. Bağlantı temizlemek ve baştan başlamak için IIS Yöneticisi'ni kapatın ve başlatma, yeniden girerek `inetmgr /reset` komut isteminden.) Özellikler listelenen ise **Web dağıtımı UI**ve 8'den daha düşük bir sürüm numarasına sahip, 1.1 ve 2.0 sürümleri yüklü Web dağıtımı dağıttığınız sunucunuz olabilir. 2.0 yüklü bir istemciden dağıtmak için sunucu yalnızca Web dağıtımı 2.0 yüklü olmalıdır. Bu sorunu gidermek için barındırma sağlayıcınızla bağlantı kurun gerekecektir.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact yerel bileşenlerinin yüklenemiyor

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtılan site yok *amd64* ve *x86* bunları yerel uygulamanın altında derlemelerde alt *bin* klasör. SQL Server yüklü Compact sahip bir bilgisayarda, yerel derlemeleri yer *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Visual Studio Proje doğru klasörlerde doğru dosyaları almak için en iyi yolu, NuGet SqlServerCompact paket yüklemektir. Paket yükleme ekler yerel derlemelerine kopyalamak için bir oluşturma sonrası betik *amd64* ve *x86*. Bunlar dağıtılması, ancak, el ile projeye dahil etmek olması. Daha fazla bilgi için bkz: [SQL Server Compact dağıtma](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Öğreticisi.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First uygulamanın dağıttıktan sonra "Yolu geçerli değil" hatası

### <a name="scenario"></a>Senaryo

SQL Server veritabanını uygulama dosyasında depolayan Compact gibi Entity Framework Code First geçişleri ve bir DBMS kullanan bir uygulamayı dağıtmak\_veri klasörü. İlk dağıtımdan sonra veritabanını oluşturmak için yapılandırılmış Code First Migrations sahip. Uygulamayı çalıştırdığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsınız:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kod ilk veritabanı ancak uygulama oluşturmak çalışıyor\_veri klasörü yok. Herhangi bir dosya ya da sahip kaydetmedi *uygulama\_veri* dağıttığınız veya seçtiğiniz klasör **hariç uygulama\_veri** üzerinde **Paketle/Yayımla Web** sekmesinde **proje özelliklerini** penceresi. Sunucusuna kopyalanması için klasöründeki dosya yoksa dağıtım işlemi sunucu üzerinde bir klasör oluşturmaz. Sitede ayarlanan veritabanı zaten sahipse, dağıtım işlemi dosyalarını siler ve *uygulama\_veri* klasörünün kendisi seçtiyseniz **hedefteki ek dosyaları Kaldır** içinde Yayımlama profili. Sorunu çözmek için bir .txt dosyasına gibi bir yer tutucu dosyasını put *uygulama\_veri* klasörü, sahip olmadığınız emin olun **hariç uygulama\_veri** seçili ve yeniden dağıtın. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Kendi temel RCW ayrılmış COM nesnesi kullanılamıyor."

### <a name="scenario"></a>Senaryo

Başarıyla silinmiş uygulamanızı dağıtmak için tek tıklamayla yayımlama ve sonra bu hatayı almaya başlayın:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kapatma ve Visual Studio'yu yeniden başlatmayı genellikle, bu hatayı gidermek için gereken tek şey.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Dağıtım başarısız olduğundan kullanıcı kimlik bilgileri için kullanılan yayımlama değilsiniz setACL yetkilisi

### <a name="scenario"></a>Senaryo

Belirten bir hata ile yayımlama başarısız oluyor (kullanmakta olduğunuz kullanıcı hesabı setACL yetkisine sahip değil) klasör izinlerini yetkisi yok.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitesinin kök klasörü üzerindeki izinleri okuma ve yazma izinleri uygulamasını\_veri klasörü. Site klasörlerine varsayılan izinlerini doğru olduğundan ve ayarlanması gerekmez biliyorsanız, bu davranışı ekleyerek devre dışı  **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (tüm profiller etkilemek için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını düzenle profil (.pubxml) dosyaları](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Bir uygulama klasörüne yazmak uygulama çalıştığında, erişim reddedildi hataları

### <a name="scenario"></a>Senaryo

Bu klasörü için yazma yetkisi olmadığı için oluşturduğunuzda veya düzenlediğinizde uygulama klasörlerden biri dosyasında çalışıldığında, uygulama hataları.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitesinin kök klasörü üzerindeki izinleri okuma ve yazma izinleri uygulamasını\_veri klasörü. Uygulamanızı bir alt klasöre yazma erişimi gerekiyorsa, gösterildiği gibi bu klasör izinlerini ayarlayabilirsiniz [klasör izinlerini ayarlama](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) ve [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğreticileri. Uygulamanızı sitenin kök klasöre yazma erişimi gerekiyorsa, salt okunur erişim kök klasöründe ayarlamaktan ekleyerek engellemek için olması  **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (tüm profiller etkilemek için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını düzenle profil (.pubxml) dosyaları](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Yapılandırma hatası - targetFramework özniteliği .NET Framework'ün yüklü sürümden daha sonraki bir sürümüne başvuruyor.

### <a name="scenario"></a>Senaryo

ASP.NET 4.5 hedefleyen bir web projesi başarıyla yayımlandı ancak uygulamayı çalıştırdığınızda (ile `customErrors` modunu ayarla "Web.config dosyasında off olarak") aşağıdaki hatayı alıyorsunuz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Kaynak hata kutunun hata sayfasının Web.config aşağıdaki satırı hatanın nedenini vurgular:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucu, ASP.NET 4.5 desteklemiyor. Ne zaman ve ASP.NET 4.5 için destek eklenebilir, belirlemek için barındırma sağlayıcısına başvurun. ASP.NET 4 veya önceki hedefleyen bir web projesi dağıtmak zorunda server yükseltme bir seçenek değilse, bunun yerine. Aynı hedef için bir ASP.NET 4 veya önceki web projesi dağıtırsanız seçin **hedefteki ek dosyaları Kaldır** onay kutusunu **ayarları** sekmesinde **Web'i**Sihirbazı. Seçmezseniz, **hedefteki ek dosyaları Kaldır**, yapılandırma hata sayfası almaya devam edersiniz.

Proje **özellikleri** windows bir hedef framework aşağı açılan listesi içerir, ancak yalnızca değerinden değiştirerek bu sorunu çözümlenemiyor **.NET Framework 4.5** için **.NET Framework 4**. Bir önceki framework sürümü için hedef Framework'ü değiştirirseniz, proje sonraki framework sürümün derlemelerine başvurular hala sahip olur ve çalışmaz. El ile bu başvuruları değiştirmek veya .NET Framework 4 veya önceki hedefleyen yeni bir proje oluşturmak sahip. Daha fazla bilgi için bkz: [.NET Framework'ü hedefleme Web siteleri için](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
