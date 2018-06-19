---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 15bda09c59afaf9e5449c68c5206bb28de245541
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889283"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


Bu sayfa, Visual Studio kullanarak bir ASP.NET web uygulamasını dağıttığınızda karşılaşabileceğiniz bazı yaygın sorunlar açıklanmaktadır. Her biri için bir veya daha fazla olası nedenleri ve karşılık gelen çözümleri verilmiştir.

Gösterilen senaryo, hem Azure hem de üçüncü taraf barındırma hizmeti sağlayıcıları için geçerlidir. Azure App Service'deki web uygulamaları sorunlarını giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Azure App Service'te Web uygulamalarını izleme](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [.NET için Windows Azure SDK 2.0 sürümünü Duyurusu](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Scottgu'nun blogudur, Visual Studio tanılama günlüklerini alma gösterir)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Sunucu hatası '/' uygulamasında - geçerli özel hata ayarları hatanın ayrıntılarını uzaktan görüntülenmesine engelliyor

### <a name="scenario"></a>Senaryo

Uzak bir ana bilgisayara bir site dağıttıktan sonra Web.config dosyasında customErrors ayarını değinmektedir ancak hatanın asıl nedenini neydi göstermez bir hata iletisi alırsınız:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Yalnızca yerel bilgisayarda web uygulamanızı çalışırken varsayılan olarak, ASP.NET ayrıntılı hata bilgileri gösterir. Genellikle saldırganlar uygulamada güvenlik açıkları bulmak için bu bilgileri kullanabilmek için olabileceğinden, web uygulamanızın Internet üzerinden genel olarak kullanılabilir olduğunda ayrıntılı hata bilgilerini görüntülemek istediğiniz yok. Ancak, bir siteye bir site veya güncelleştirmeleri dağıtıyorsanız, bazen bir şeyler yanlış geçer ve gerçek hata iletisini almanız gerekir.

Uzak ana bilgisayarda çalıştırılan ayrıntılı hata iletilerini görüntülemek uygulama etkinleştirmek için customErrors modunu ayarlama, uygulamayı yeniden dağıt ve uygulamayı yeniden çalıştırmak için Web.config dosyasını düzenleyin:

1. Uygulamanın Web.config dosyasını thesystem.web öğesinde acustomErrors öğe varsa, "kapalı" themode özniteliğini değiştirin. Aksi takdirde acustomErrors öğesi thesystem.web öğesinde "kapalı", themode özniteliği ile aşağıdaki örnekte gösterildiği gibi ekleyin: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Uygulamayı dağıtın.
3. Uygulamayı çalıştırın ve ne olursa olsun, daha önce hata oluşmasına neden oldu yineleyin. Şimdi, gerçek hata iletisi nedir görebilirsiniz.
4. Hatanın çözülmüş özgün customErrors ayarına geri yükleme ve uygulamayı yeniden dağıtın.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Oluşturma/Kopyala 'ContosoUniversity' Bu dosya zaten varken gölge olamaz.

### <a name="scenario"></a>Senaryo

Visual Studio Proje çalıştırmayı denediğinizde aşağıdaki örneğe benzer bir ileti ile bir hata sayfası alın:

'/' Uygulamasında sunucu hatası. Oluşturma/Kopyala 'ContosoUniversity' Bu dosya zaten varken gölge olamaz.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir dakika bekleyin ve tarayıcıyı yenileyin veya site derlenir ve yeniden çalıştırmayı deneyin.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Bir Web sayfasında kullandığı SQL Server Compact erişim reddedildi

### <a name="scenario"></a>Senaryo

SQL Server Compact kullanan bir siteye dağıtmak ve veritabanına erişir dağıtılan sitede bir sayfa çalıştırıldığında, aşağıdaki hata iletisini görürsünüz:

Erişim reddedildi. (HRESULT özel durumu: 0x80070005 (E\_erişim ENGELLENDİ))

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

NETWORK SERVICE hesabı sunucuda bulunan SQL Hizmeti Compact yerel ikili dosyaları okuyabilir gerekiyor *bin\amd64* veya *bin\x86* klasörü, ancak okuma bu klasörler için izinleri. Set okuma izni ağ hizmeti üzerinde *bin* klasör, alt klasörler için izinleri genişletmek emin olun.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor

### <a name="scenario"></a>Senaryo

Tıkladığınızda Visual Studio yayımlama IIS yerel makinenizde bir uygulamayı dağıtmak için düğmesini başarısız yayımlama ve **çıkış** penceresi bir hata iletisi bunun benzer gösterir:

IIS yapılandırma dosyası ' Makine/yeniden yönlendirme' okunurken bir hata oluştu. Bu işlem gerçekleştirilirken kimlik oluştu... Hata: yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Tek tıklamayla kullanmak için IIS yerel makinenizde yayımlama, Visual Studio yönetici izinlerine sahip çalıştırıyor olması gerekir. Visual Studio'yu kapatın ve yönetici izinlerine sahip yeniden başlatın.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Hedef bilgisayara bağlanılamadı... Belirtilen işlem kullanma

### <a name="scenario"></a>Senaryo

Tıkladığınızda Visual Studio yayımlama bir uygulamayı dağıtmak için düğmesini başarısız yayımlama ve **çıkış** penceresi bir hata iletisi bunun benzer gösterir:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir proxy sunucu hedef sunucuyla iletişim kesintiye. Windows Denetim Masası'ndan veya Internet Explorer'ı seçin **Internet Seçenekleri** seçip **bağlantıları** sekmesi. İçinde **Internet Özellikleri** iletişim kutusu, tıklatın **LAN Ayarları**. İçinde **yerel ağ (LAN) ayarları** iletişim kutusu, Temizle **ayarları otomatik olarak algıla** onay kutusu. Daha sonra yeniden Yayımla düğmesine tıklayın.

Sorun devam ederse, proxy veya güvenlik duvarı ayarlarını yapılabilir belirlemek için sistem yöneticinize başvurun. Web dağıtımı standart olmayan bağlantı noktası için Web Yönetim hizmeti dağıtımı (8172); kullandığından sorun olur diğer bağlantılar için Web dağıtımı, 80 numaralı bağlantı noktasını kullanır. Bir üçüncü taraf barındırma sağlayıcısına dağıtırken, genellikle Web yönetimi hizmeti kullanıyor.

## <a name="default-net-40-application-pool-does-not-exist"></a>Varsayılan .NET 4.0 uygulama havuzu yok

### <a name="scenario"></a>Senaryo

.NET Framework 4 gerektiren bir uygulama dağıttığınızda, aşağıdaki hata iletisini görürsünüz:

Varsayılan .NET 4.0 uygulama havuzu yok veya Uygulama eklenemedi. ASP.NET 4.0 Bu makinede yüklü olduğunu doğrulayın.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS'de ASP.NET 4 yüklü değil. Uygulamanızı geliştirme bilgisayarınızın dağıttığınız sunucu ise ve Visual Studio 2010 'un yüklü, ASP.NET 4 bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıttığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS ASP.NET 4 yükleyin:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Varsayılan uygulama havuzunu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için IIS dağıtma bu serideki bir Test Ortamı öğretici olarak bakın.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Başlatma dizesinin biçimi 0 dizininde başlayan belirtime uygun değil.

### <a name="scenario"></a>Senaryo

Tek tıklamayla kullanarak bir uygulamayı dağıttıktan sonra veritabanına erişen bir sayfa çalıştırdığınızda, aşağıdaki hata iletisini aldığınızda, yayımlayın:

Başlatma dizesinin biçimi 0 dizininde başlayan belirtime uygun değil.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Açık *Web.config* bağlantı dizesi değerleri $ ile başlayan olup olmadığını denetleyin ve dağıtılan site dosyasında (ReplacableToken\_, aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Bağlantı dizeleri aşağıdaki örnekte olduğu gibi görünüyorsa, proje dosyasını düzenleyin ve aşağıdaki özelliği olan tüm derleme yapılandırmalarını PropertyGroup öğesi ekleyin:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Sonra uygulamayı yeniden dağıtın.

## <a name="http-500-internal-server-error"></a>HTTP 500 İç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, hatanın nedenini gösteren belirli bilgiler olmadan aşağıdaki hata iletisini görürsünüz:

HTTP hata 500 - İç sunucu hatası.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

500 hataları pek çok nedeni vardır, ancak bu öğreticileri izliyorsanız olası bir nedeni Web.config dönüşümü dosyalardan biri yanlış yerinde bir XML öğesi koyduğunuz yerdir. Örneğin, ekler dönüştürme yerleştirirseniz bu hatayı elde edebileceğiniz bir &lt;konumu&gt; öğesinin altında &lt;system.web&gt; yerine doğrudan altında &lt;yapılandırma&gt;. Dönüşümleri beklendiği gibi çalıştığını doğrulamak için Web.config dönüşümü önizleme özelliğini kullanabilirsiniz. Yanlış kodlanmış bir dönüşüm bulursanız dönüşüm dosyası düzeltin ve yeniden dağıtmak için çözümüdür. Hata açık değilse, dönüşümler yorum deneyin ve hangisinin görmek için dağıtarak ve 500 hatasına neden oluyor.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 iç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

HTTP Hatası 500.21 - iç sunucu hatası. İşleyici "PageHandlerFactory-tümleşik" modül listesinde hatalı bir modül "ManagedPipelineHandler" sahiptir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Site ASP.NET 4 ancak ASP.NET 4 kaydedilmemiş IIS sunucu üzerinde hedefleri dağıtmış. Sunucusunda yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak ASP.NET 4 kaydedin:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Varsayılan uygulama havuzunu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için IIS dağıtma bu serideki bir Test Ortamı öğretici olarak bakın.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Oturum açma başarısız açılış SQL Server Express veritabanı uygulamasında\_veri

### <a name="scenario"></a>Senaryo

Güncelleştirilmiş *Web.config* dosya bir SQL Server Express veritabanı işaret etmek için bağlantı dizesi bir *.mdf* dosyasını, *uygulama\_veri* klasörü ve ilk Aşağıdaki hata iletisini görürsünüz uygulama çalışma zamanı:

System.Data.SqlClient.SqlException: "oturum açma tarafından istenen VeritabanıAdı" veritabanı açılamıyor. Oturum açma başarısız.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Adını *.mdf* sildiğiniz olsa bile, dosya şimdiye kadar bilgisayarınızda var SQL Server Express veritabanının adını eşleşemez *.mdf* önceden var olan veritabanının dosya. Adını değiştirmek *.mdf* hiçbir zaman bir veritabanı adı ve değişiklik kullanılmış bir ad dosyasına *Web.config* dosya yeni bir ad kullanın. Alternatif olarak, kullandığınız [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) önceden var olan SQL Server Express silmek için veritabanları.

## <a name="model-compatibility-cannot-be-checked"></a>Model uyumluluğu olamaz denetlenmesi

### <a name="scenario"></a>Senaryo

Güncelleştirilmiş *Web.config* dosya yeni bir SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi ve ilk kez uygulamayı çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

Veritabanı model meta verilerini içermediğinden model uyumluluğu denetlenemiyor. DbModelBuilder kuralları IncludeMetadataConvention eklendiğinden emin olun.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Veritabanı adı Web.config dosyasında yerleştirirseniz bilgisayarınızda, bir veritabanı zaten bazı tablolarda ile mevcut olabilir önce herhangi bir zamanda kullanıldı. Bilgisayarınızın önce ve değişiklik üzerinde kullanılmamış olan yeni bir ad seçin *Web.config* dosyayı bu yeni bir veritabanı adı kullanmak için işaretleyin. Alternatif olarak, kullandığınız [SQL Server Express yardımcı programı](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) veya [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) varolan veritabanı silinemiyor.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Kullanıcıları veya rolleriyle oluşturmak bir komut dosyası çalıştığında SQL hatası

### <a name="scenario"></a>Senaryo

Yapılandırılan veritabanı dağıtımı kullanarak **SQL Paketle/Yayımla** sekmesinde, dağıtım sırasında çalışan SQL komut dosyaları dahil kullanıcı oluştur veya rol Oluştur komutları ve komut dosyası yürütme başarısız Bu komutlar çalıştırıldığında. Daha ayrıntılı iletiler, aşağıdaki gibi görebilirsiniz:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Veritabanı dağıtımı yapılandırıldığında bu hata oluşursa **Web'i Yayımla** Sihirbazı yerine **SQL Paketle/Yayımla** sekmesinde, bir iş parçacığı oluşturma [yapılandırma ve Dağıtım](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum ve çözüm, bu sorun giderme sayfasına eklenir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtım gerçekleştirmek için kullandığınız kullanıcı hesabı, kullanıcılar ya da roller oluşturma izni yok. Örneğin, barındırma şirketi db atayabilecek\_datareader, db\_datawriter ve db\_ddladmin rolleri sizin için ayarlayan kullanıcı hesabı. Bunlar yeterli çoğu veritabanı nesneleri oluşturma, ancak kullanıcılar ya da roller oluşturma değil. Hatayı önlemek için bir veritabanı dağıtımından kullanıcılar ve roller hariç tutarak yoludur. Aşağıdaki öznitelikler içeren veritabanı otomatik olarak oluşturulan komut dosyası için PreSource öğesi düzenleyerek bunu yapabilirsiniz:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Proje dosyasında PreSource öğesi düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: Proje dosyasında dağıtım ayarlarını Düzenle](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Kullanıcıları veya rolleriyle geliştirme veritabanınızdaki hedef veritabanında olması gerekiyorsa, Yardım için barındırma sağlayıcınızla bağlantı kurun.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server zaman aşımı özel komut dosyalarını dağıtım sırasında çalıştırılırken hata

### <a name="scenario"></a>Senaryo

Dağıtım sırasında çalıştırmak için özel SQL komut dosyaları belirttiğiniz ve Web dağıtımı bunları çalıştığında, bunlar zaman aşımına uğrar.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Farklı bir işlem modları sahip birden çok komut dosyası çalıştırarak zaman aşımı hataları neden olabilir. Varsayılan olarak, bir işlem içinde otomatik olarak oluşturulan komut dosyalarını çalıştırmak, ancak özel komut dosyaları yapın. Seçerseniz **çekme veri ve/veya varolan bir veritabanını şema** seçeneği **SQL Paketle/Yayımla** sekmesinde ve özel SQL komut dosyası eklerseniz, bazı kodlar işlem ayarlarını değiştirmeniz gerekir böylece tüm betikler işlem ayarların aynısını kullanın. Daha fazla bilgi için bkz: [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343.aspx).

Tümü aynı; böylece işlem ayarları yapılandırdınız, ancak bu hatayı almaya devam komut dosyalarını ayrı olarak çalıştırmak için bir olası geçici bir çözüm değildir. İçinde **veritabanı komut dosyalarında** kılavuzunda **Paketle/Yayımla** SQL sekmesi, Temizle **INCLUDE** zaman aşımı hatası neden olan komut dosyası için onay kutusunu işaretleyin, sonra proje yayımlayın. Uygulamasına geri gidin sonra **veritabanı komut dosyalarında** kılavuz, bu komut dosyanızın seçin **INCLUDE** onay kutusunu işaretleyin ve temizleyin **INCLUDE** diğer komutlar için onay kutularını. Sonra projeyi yeniden yayımlayın. Yayımladığınızda, bu zaman yalnızca seçilen özel komut dosyasını çalıştırır.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Site bildiriminin veri akışı henüz kullanılamıyor

### <a name="scenario"></a>Senaryo

Kullanarak bir paket yüklerken *deploy.cmd* dosya t (test) seçeneğiyle, aşağıdaki hata iletisini görürsünüz:

Hata: Veri akışı, ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' henüz kullanılabilir değil.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Hata iletisi, komutu bir test raporu üretemez anlamına gelir. Ancak, y (gerçek yükleme) seçeneğini kullanırsanız komutu çalıştırabilirsiniz. İleti, yalnızca komut test modunda çalışan bir sorun olduğunu gösterir.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Bu uygulama ManagedRuntimeVersion v4.0 gerektirir

### <a name="scenario"></a>Senaryo

Dağıtma girişiminde bulunduğunuzda, aşağıdaki hata iletisini görürsünüz:

Kullanmaya çalıştığınız uygulama havuzu 'managedRuntimeVersion' özelliği 'v2.0' ayarlanmış. Bu uygulama 'v4.0' gerektirir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS'de ASP.NET 4 yüklü değil. Uygulamanızı geliştirme bilgisayarınızın dağıttığınız sunucu ise ve Visual Studio 2010 'un yüklü, ASP.NET 4 bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıttığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS ASP.NET 4 yükleyin:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions yayınlanamıyor

### <a name="scenario"></a>Senaryo

Bir paketi dağıtırken, aşağıdaki hata iletisini görürsünüz:

Türü 'Microsoft.Web.Deployment.DeploymentProviderOptions' için ' Microsoft.Web.Deployment.DeploymentProviderOptions' nesne alınamıyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS Web dağıtımı 2.0 yüklü olan bir sunucu için Web dağıtımı 1.1 UI kullanarak Yöneticisi'nden dağıtmaya çalıştığınız. Bir paket onay içeri aktararak dağıtmak için IIS Uzak Yönetim Aracı'nı kullanıyorsanız **yeni özellikler kullanılabilir** bağlantısı kurarken iletişim kutusu. (Bu iletişim kutusunu yalnızca bir kez bağlantı ilk ne zaman kurulur görüntülenebilir. Bağlantı temizlemek ve baştan başlamak için IIS Yöneticisi'ni kapatın ve başlatma, yeniden inetmgr girerek/komut isteminde Sıfırla.) Özellikler listelenen ise **Web dağıtımı UI**ve 8'den daha düşük bir sürüm numarasına sahip, 1.1 ve 2.0 sürümleri yüklü Web dağıtımı dağıttığınız sunucunuz olabilir. 2.0 yüklü bir istemciden dağıtmak için sunucu yalnızca Web dağıtımı 2.0 yüklü olmalıdır. Bu sorunu gidermek için barındırma sağlayıcınızla bağlantı kurun gerekecektir.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact yerel bileşenlerinin yüklenemiyor

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, aşağıdaki hata iletisini görürsünüz:

SQL Server sürümü 8482 ADO.NET sağlayıcısı için karşılık gelen Compact yerel bileşenlerinin yüklenemiyor. SQL Server Compact doğru sürümünü yükleyin. Daha fazla ayrıntı için 974247 KB makalesine bakın.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtılan site yok *amd64* ve *x86* bunları yerel uygulamanın altında derlemelerde alt *bin* klasör. SQL Server yüklü Compact sahip bir bilgisayarda, yerel derlemeleri yer *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Visual Studio Proje doğru klasörlerde doğru dosyaları almak için en iyi yolu, NuGet SqlServerCompact paket yüklemektir. Paket yükleme ekler yerel derlemelerine kopyalamak için bir oluşturma sonrası betik *amd64* ve *x86*. Bunlar dağıtılması, ancak, el ile projeye dahil etmek olması. Daha fazla bilgi için bkz: [SQL Server Compact dağıtma](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Öğreticisi.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First uygulamanın dağıttıktan sonra "Yolu geçerli değil" hatası

### <a name="scenario"></a>Senaryo

SQL Server veritabanını uygulama dosyasında depolayan Compact gibi Entity Framework Code First geçişleri ve bir DBMS kullanan bir uygulamayı dağıtmak\_veri klasörü. İlk dağıtımdan sonra veritabanını oluşturmak için yapılandırılmış Code First Migrations sahip. Uygulamayı çalıştırdığınızda, aşağıdaki örneğe benzer bir hata iletisi alırsınız:

Yol geçerli değil. Veritabanı için dizini denetleyin. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kod ilk veritabanı ancak uygulama oluşturmak çalışıyor\_veri klasörü yok. Herhangi bir dosya ya da sahip kaydetmedi *uygulama\_veri* dağıttığınız veya seçtiğiniz klasör **hariç uygulama\_veri** üzerinde **Paketle/Yayımla Web** proje penceresinin sekmesi. Sunucusuna kopyalanması için klasöründeki dosya yoksa dağıtım işlemi sunucu üzerinde bir klasör oluşturmaz. Sitede ayarlanan veritabanı zaten sahipse, dağıtım işlemi dosyalarını siler ve *uygulama\_veri* klasörünün kendisi seçtiyseniz **hedefteki ek dosyaları Kaldır** içinde Yayımlama profili. Sorunu çözmek için bir .txt dosyasına gibi bir yer tutucu dosyasını put *uygulama\_veri* klasörü, sahip olmadığınız emin olun **hariç uygulama\_veri** seçili ve yeniden dağıtın.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Kendi temel RCW ayrılmış COM nesnesi kullanılamıyor."

### <a name="scenario"></a>Senaryo

Başarıyla silinmiş uygulamanızı dağıtmak için tek tıklamayla yayımlama ve sonra bu hatayı almaya başlayın:

Web dağıtım görevi başarısız oldu. (Uzak Aracı URL'sine yönelik istek tamamlanamadı '<https://serverurl.com/msdeploy.axd?site=sitename>'.)  
 Uzak Aracı URL'sine yönelik istek tamamlanamadı '<https://url/msdeploy.axd?site=sitename>'.  
İstek iptal edildi: istek iptal edildi.  
Temel alınan kendi RCW ayrılmış COM nesnesi kullanılamaz.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kapatma ve Visual Studio'yu yeniden başlatmayı genellikle, bu hatayı gidermek için gereken tek şey.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Dağıtım başarısız olduğundan kullanıcı kimlik bilgileri için kullanılan yayımlama değilsiniz setACL yetkilisi

### <a name="scenario"></a>Senaryo

Belirten bir hata ile yayımlama başarısız oluyor (kullanmakta olduğunuz kullanıcı hesabı setACL yetkisine sahip değil) klasör izinlerini yetkisi yok.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitesinin kök klasörü üzerindeki izinleri okuma ve yazma izinleri uygulamasını\_veri klasörü. Site klasörlerine varsayılan izinlerini doğru olduğundan ve ayarlanması gerekmez biliyorsanız, bu davranışı ekleyerek devre dışı **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (tüm profiller etkilemek için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını düzenle profil (.pubxml) dosyaları](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Bir uygulama klasörüne yazmak uygulama çalıştığında, erişim reddedildi hataları

### <a name="scenario"></a>Senaryo

Bu klasörü için yazma yetkisi olmadığı için oluşturduğunuzda veya düzenlediğinizde uygulama klasörlerden biri dosyasında çalışıldığında, uygulama hataları.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitesinin kök klasörü üzerindeki izinleri okuma ve yazma izinleri uygulamasını\_veri klasörü. Uygulamanızı bir alt klasöre yazma erişimi gerekiyorsa, gösterildiği gibi dağıtma ve klasör izinlerini ayarlama bu serideki üretim ortamına öğreticileri için bu klasörün izinlerini ayarlayabilirsiniz. Uygulamanızı sitenin kök klasöre yazma erişimi gerekiyorsa, salt okunur erişim kök klasöründe ayarlamaktan ekleyerek engellemek için olması **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (tüm profiller etkilemek için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını düzenle profil (.pubxml) dosyaları](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Yapılandırma hatası - targetFramework özniteliği .NET Framework'ün yüklü sürümden daha sonraki bir sürümüne başvuruyor.

### <a name="scenario"></a>Senaryo

ASP.NET 4.5 hedefleyen bir web projesi başarıyla yayımlandı ancak (customErrors modu "kapalı" Web.config dosyasına ayarlayın) uygulamayı çalıştırdığınızda, aşağıdaki hatayı alıyorsunuz:

'targetFramework' özniteliği &lt;derleme&gt; Web.config dosyasının öğesi, yalnızca hedef sürüm 4.0 ve daha sonra .NET Framework'ün kullanılır (örneğin, '&lt;derleme targetFramework "4.0"=&gt;'). 'TargetFramework' özniteliği şu anda .NET Framework'ün yüklü sürümden daha sonraki bir sürümüne başvuruyor. .NET Framework geçerli hedef sürümü belirtin veya .NET Framework'ün gerekli sürümü yükleyin.

Kaynak hata kutunun hata sayfasının Web.config aşağıdaki satırı hatanın nedenini vurgular:

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucu, ASP.NET 4.5 desteklemiyor. Ne zaman ve ASP.NET 4.5 için destek eklenebilir, belirlemek için barındırma sağlayıcısına başvurun. ASP.NET 4 veya önceki hedefleyen bir web projesi dağıtmak zorunda server yükseltme bir seçenek değilse, bunun yerine.

Aynı hedef için bir ASP.NET 4 veya önceki web projesi dağıtırsanız seçin **hedefteki ek dosyaları Kaldır** onay kutusunu **ayarları** sekmesinde **Web'i**Sihirbazı. Seçmezseniz, **hedefteki ek dosyaları Kaldır**, yapılandırma hata sayfası almaya devam edersiniz.

Proje **özellikleri** windows bir hedef framework aşağı açılan listesi içerir, ancak yalnızca değerinden değiştirerek bu sorunu çözümlenemiyor **.NET Framework 4.5** için **.NET Framework 4**. Bir önceki framework sürümü için hedef Framework'ü değiştirirseniz, proje sonraki framework sürümün derlemelerine başvurular hala sahip olur ve çalışmaz. El ile bu başvuruları değiştirmek veya .NET Framework 4 veya önceki hedefleyen yeni bir proje oluşturmak sahip. Daha fazla bilgi için bkz: [.NET Framework'ü hedefleme Web siteleri için](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Orta güven hataları

### <a name="scenario"></a>Senaryo

Üretimde uygulamanızı çalıştırdığınızda, Orta güven ilgili bir hata alır.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Çok sayıda üçüncü taraf barındırma hizmeti sağlayıcıları, web sitenizi yapmak için izin verilmeyen bazı şeyleri olduğu anlamına gelir, Orta güvende çalıştırın. Örneğin, uygulama kodu Windows kayıt defterine erişim ve olamaz okuyamıyor veya uygulamanızın klasör hiyerarşisi dışında olan dosyaları yazamıyor. Varsayılan olarak, uygulamanın çalıştığı *tam güven* yerel bilgisayarınızda başka bir deyişle, uygulama üretime dağıttığınızda, hata veren şeyler mümkün olabilir.

Sorun giderme için yerel IIS ortamında Orta güven çalıştırmak için uygulamayı yapılandırabilirsiniz. Bunu yapmak için uygulamayı açın *Web.config* dosya ve ekleme bir **güven** öğesinde **system.web** Bu örnekte gösterildiği gibi öğesi.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Uygulama artık IIS'de Orta güven içindeki bile yerel bilgisayarınızda çalışır.

Bunu yapma Azure Orta güven gerektirmediğinden, Azure App Service'e dağıttığınız. Şubat, 2012'de Bu öğretici yazılıyor zaman uygulamanızı Orta güven çalışmasını sağlamak için bu yöntemi kullanarak bir hata ile Azure neden olur.

Entity Framework Code First Migrations kullanıyorsanız ve orta güven uygulamanızı çalıştıran bir barındırma sağlayıcısına dağıtıyorsanız, sürüm 5.0 sahip olduğunuzu veya üstü yüklü olun. Entity Framework sürümü 4.3'da, geçişler gerektirir tam güven veritabanı şeması güncelleştirmek için.

## <a name="http-40417-not-found-error"></a>HTTP 404,17 bulunamadı hatası

### <a name="scenario"></a>Senaryo

Geliştirme bilgisayarınızda IIS sitesi dağıtıldı çalıştırdığınızda, sunucu Default.aspx işleyemiyor raporlama aşağıdaki hata iletisini görürsünüz:

HTTP Hatası 404,17 - bulunamadı

İstenen içerik komut dizisi olarak gözüküyor ve statik dosya işleyici tarafından sunulmayacak.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4.5, bilgisayarda yüklü değil. Bu serideki nasıl ASP.NET 4.5 yükleneceğini açıklayan bir Test Ortamı öğretici olarak dağıtma adımları IIS bakın.

> [!div class="step-by-step"]
> [Önceki](deploying-extra-files.md)
