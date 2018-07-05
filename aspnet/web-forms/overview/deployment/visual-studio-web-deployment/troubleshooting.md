---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 0b57a91c29ba15463e6c534693b951aee8286ad2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364008"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


Bu sayfa, Visual Studio kullanarak bir ASP.NET web uygulamasına dağıtırken karşılaşabileceğiniz bazı yaygın sorunlar açıklanmaktadır. Her biri için bir veya daha fazla olası nedenleri ve karşılık gelen bir çözüm sağlanır.

Gösterilen senaryo, hem Azure hem de üçüncü taraf barındırma sağlayıcıları için geçerlidir. Azure App service'taki web apps sorunlarını giderme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Azure App Service'te Web uygulamalarını izleme](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [.NET için Windows Azure SDK 2.0 sürümü ile tanışın](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Scottgu'nun blogu, Visual Studio'da tanılama günlüklerini almak nasıl gösterir)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Sunucu hatası '/' uygulamasında - geçerli özel hata ayarları hatanın ayrıntıları uzaktan görüntülenmesini engeller

### <a name="scenario"></a>Senaryo

Bir siteyi uzak bir konağa dağıttıktan sonra customErrors ayarı Web.config dosyasında bahsetmeleri ancak hatanın asıl nedenini neydi göstermez bir hata iletisi alın:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Yalnızca web uygulamanızı yerel bilgisayarda çalışırken varsayılan olarak, ASP.NET ayrıntılı hata bilgileri gösterir. Genellikle saldırganlar bu bilgileri uygulamada güvenlik açıklarına kullanın mümkün olabilir çünkü web uygulamanıza Internet üzerinden genel kullanıma açık olduğunda, ayrıntılı hata bilgilerini görüntülemek istediğiniz yok. Ancak, bir site veya güncelleştirmeleri bir site için dağıtım yaparken, bazen bir şeyler yanlış gidecek ve gerçek hata iletisini almanız gerekir.

Uzak ana bilgisayarda çalıştığında, ayrıntılı hata iletilerini görüntülemek uygulamayı etkinleştirmek için customErrors modunu ayarlayın, uygulama yeniden dağıtın ve uygulamayı yeniden çalıştırmak için Web.config dosyasını düzenleyin:

1. Uygulamanın Web.config dosyasını thesystem.web öğesinde acustomErrors öğe varsa, "kapalı" themode özniteliği değiştirin. Aksi takdirde acustomErrors öğesi thesystem.web öğesinde "kapalı", themode özniteliği ile aşağıdaki örnekte gösterildiği gibi ekleyin: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Uygulamayı dağıtın.
3. Uygulamayı çalıştırmak ve ne olursa olsun daha önce gerçekleşmesi hataya neden yaptığınız yineleyin. Şimdi gerçek hata iletisidir görebilirsiniz.
4. Hata çözdükten sonra özgün customErrors ayarına geri yükleme ve uygulamayı yeniden dağıtın.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Oluşturma/kopyalama 'ContosoUniversity' zaten mevcut olduğunda gölge olamaz.

### <a name="scenario"></a>Senaryo

Visual Studio'da bir proje çalıştırmayı denediğinizde bir hata sayfası aşağıdaki örnekte olduğu gibi bir ileti ile olursunuz:

'/' Uygulamasında sunucu hatası. Oluşturma/kopyalama 'ContosoUniversity' zaten mevcut olduğunda gölge olamaz.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir dakika bekleyin ve tarayıcıyı yenileyin veya site yeniden derlemeniz ve yeniden çalıştırmayı deneyin.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Kullanan SQL Server Compact, Web sayfasına erişim reddedildi

### <a name="scenario"></a>Senaryo

SQL Server Compact kullanan bir siteyi dağıtma ve dağıtılan sitede veritabanına erişen bir sayfa çalıştırıldığında, aşağıdaki hata iletisini görürsünüz:

Erişim reddedildi. (HRESULT özel durum: 0x80070005 (E\_erişim ENGELLENDİ))

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucusunda ağ hizmeti hesabı olduğundan hizmet SQL Compact yerel ikili dosyaları okuyabilir gerekiyor *bin\amd64* veya *bin\x86* klasör, ancak okuma izinleri bu klasörleri için. Set okuma izni için ağ hizmeti üzerinde *bin* klasör izinlerini alt klasörlere genişletmek emin olun.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor

### <a name="scenario"></a>Senaryo

' A tıkladığınızda Visual Studio yayımlama IIS yerel makinenizde bir uygulamayı dağıtmak için düğmeye başarısız yayımlama ve **çıkış** penceresi bir hata iletisi şuna benzer gösterir:

' Makine/yeniden yönlendirme' IIS yapılandırma dosyası okunurken bir hata oluştu. Bu işlemi gerçekleştirmeden kimlik oluştu... Hata: yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Tek tıklamayla kullanmak için yerel makinenizde IIS yayımlamak, Visual Studio'yu yönetici izinleriyle çalıştırılmalıdır. Visual Studio'yu kapatın ve yönetici izinleriyle yeniden başlatın.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Hedef bilgisayara bağlanamadı... Belirtilen işlem kullanma

### <a name="scenario"></a>Senaryo

' A tıkladığınızda Visual Studio yayımlama bir uygulamayı dağıtmak için düğmeye başarısız yayımlama ve **çıkış** penceresi bir hata iletisi şuna benzer gösterir:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir proxy sunucusu hedef sunucu ile iletişimi kesintiye. Windows Denetim Masası'ndan veya Internet Explorer'ı seçin **Internet Seçenekleri** seçip **bağlantıları** sekmesi. İçinde **Internet Özellikleri** iletişim kutusu, tıklayın **LAN Ayarları**. İçinde **yerel alan ağı (LAN) ayarları** iletişim kutusu, NET **ayarlarını otomatik olarak algıla** onay kutusu. Ardından Yayımla düğmesine tekrar tıklayın.

Sorun devam ederse, proxy veya güvenlik duvarı ayarları ile yapılabilir belirlemek için sistem yöneticinize başvurun. Standart olmayan bağlantı noktası (8172); Web yönetimi hizmeti dağıtımı için kullandığı Web dağıtımı için sorun olur. diğer bağlantılar için Web dağıtımı, 80 numaralı bağlantı noktasını kullanır. Web yönetimi hizmeti, bir üçüncü taraf barındırma sağlayıcısına dağıttığınız zaman, genellikle kullanıyor.

## <a name="default-net-40-application-pool-does-not-exist"></a>Varsayılan .NET 4.0 uygulama havuzu yok.

### <a name="scenario"></a>Senaryo

.NET Framework 4 gerektiren bir uygulamayı dağıttığınızda, aşağıdaki hata iletisini görürsünüz:

Varsayılan .NET 4.0 uygulama havuzu yok veya Uygulama eklenemedi. ASP.NET 4.0 Bu makinede yüklü olduğunu doğrulayın.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4 IIS'de yüklü değil. Dağıtım yaptığınız sunucunun geliştirme bilgisayarınıza ve Visual Studio 2010 'un yüklü, ASP.NET 4 bu bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıtım yaptığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS içinde ASP.NET 4 yükleyin:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Varsayılan uygulama havuzu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için bir Test Ortamı Öğreticide bu seri olarak IIS'ye dağıtma bakın.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Başlatma dizesinin biçimi, 0 dizininde başlayan belirtime uymuyor.

### <a name="scenario"></a>Senaryo

Tek tıklamayla kullanarak bir uygulamayı dağıttıktan sonra veritabanına erişen bir çalıştırırsanız, aşağıdaki hata iletisini aldığınızda, yayımlama:

Başlatma dizesinin biçimi, 0 dizininde başlayan belirtime uymuyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Açık *Web.config* dosyasında dağıtılan sitesi ve bağlantı dizesi değerlerini $ ile başlayan olup olmadığını denetleyin (ReplacableToken\_, aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Bağlantı dizelerini şu örnekteki gibi bakarsanız, proje dosyasını düzenleyin ve tüm derleme yapılandırmaları için PropertyGroup öğesi için aşağıdaki özelliği ekleyin:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Ardından, uygulamayı yeniden dağıtın.

## <a name="http-500-internal-server-error"></a>HTTP 500 İç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, hatanın nedenini gösteren belirli bilgiler aşağıdaki hata mesajını görebilirsiniz:

HTTP Hatası 500 - İç sunucu hatası.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

500 hataları birçok neden vardır, ancak bu öğreticileri takip ediyorsanız olası nedenlerinden biri, bir XML öğesi yanlış yere Web.config dönüşümü dosyalarının birinde yerleştirin olmasıdır. Örneğin, eklediği dönüştürme koyarsanız bu hatayı elde edebileceğiniz bir &lt;konumu&gt; öğesi altında &lt;system.web&gt; yerine doğrudan altında &lt;yapılandırma&gt;. Dönüşümler beklendiği gibi çalıştığını doğrulamak için Web.config dönüşümü önizleme özelliğini kullanabilirsiniz. Yanlış kodlanmış bir dönüşüm bulursanız dönüşüm dosyasını düzeltin ve yeniden dağıtma çözümdür. Bir hata açık değilse, dönüşümler yorum deneyin ve hangisinin görmek için yeniden dağıtım 500 hataya neden olan.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 iç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

HTTP Hatası 500.21 - iç sunucu hatası. İşleyici "PageHandlerFactory-tümleşik" hatalı bir modül, modül listesinde "ManagedPipelineHandler" vardır.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Site ASP.NET 4 ancak ASP.NET 4 kayıtlı değil IIS'de sunucu üzerinde hedefleri dağıttım. Sunucusunda yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak ASP.NET 4 kaydedin:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Varsayılan uygulama havuzu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için bir Test Ortamı Öğreticide bu seri olarak IIS'ye dağıtma bakın.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Oturum açma başarısız oldu, açılış SQL Server Express veritabanı uygulamasında\_veri

### <a name="scenario"></a>Senaryo

Sizi *Web.config* dosya SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi bir *.mdf* dosyası, *uygulama\_veri* klasörünü ve ilk Aşağıdaki hata iletisini gördüğünüz uygulama çalışma zamanı:

System.Data.SqlClient.SqlException: "oturum açma tarafından istenen VeritabanıAdı" veritabanı açılamıyor. Oturum açma başarısız.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Adını *.mdf* dosya sildiğiniz olsa bile bilgisayarınızda hiç olmadığı kadar mevcut SQL Server Express veritabanı adını eşleşemez *.mdf* önceden var olan veritabanının dosya. Adını değiştirmek *.mdf* dosya hiçbir zaman bir veritabanı adı ve değişiklik olarak kullanılmış bir ad *Web.config* dosyasını yeni bir ad kullanın. Alternatif olarak, kullandığınız [SQL Server Management Studio Express'i](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) önceden var olan SQL Server Express silmek için veritabanları.

## <a name="model-compatibility-cannot-be-checked"></a>Uyumluluk modeli olamaz denetlenmesi

### <a name="scenario"></a>Senaryo

Sizi *Web.config* dosya yeni bir SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi ve ilk kez uygulamayı çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

Veritabanı model meta verilerini içermediğinden modeli uyumluluk iade edilemez. IncludeMetadataConvention DbModelBuilder kurallarına eklendiğinden emin olun.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Veritabanı adı Web.config dosyasına yerleştirirseniz, bilgisayarınızda, bir veritabanı zaten bazı tablolarda ile bulunmayabilir önce hiç olmadığı kadar kullanıldı. Önce bilgisayarınıza ve Değiştir üzerinde kullanılmamış olan yeni bir ad seçin *Web.config* dosya bu yeni veritabanı adını kullanacak şekilde yönlendirin. Alternatif olarak, kullandığınız [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) veya [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) mevcut veritabanı silinemiyor.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Bir komut dosyası, kullanıcılar ya da rolleri oluşturma girişiminde bulunduğunda SQL hatası

### <a name="scenario"></a>Senaryo

Yapılandırılan veritabanı dağıtımı kullanıyorsanız **SQL Paketle/Yayımla** sekmesinde, dağıtım sırasında çalıştırılan SQL betikleri içeren oluşturma kullanıcı veya rol Oluştur komutlar ve komut yürütme başarısız Bu komutlar çalıştırıldığında. Daha ayrıntılı iletiler, aşağıdaki gibi görebilirsiniz:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Veritabanı dağıtımda yapılandırıldığında bu hata oluşursa **Web'i Yayımla** Sihirbazı yerine **SQL Paketle/Yayımla** sekmesinde, bir dizi oluşturun [yapılandırma ve Dağıtım](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum ve çözüm, bu sorun giderme sayfasına eklenir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtım gerçekleştirmek için kullandığınız kullanıcı hesabı, kullanıcılar ya da rolleri oluşturma izni yok. Örneğin, barındırma şirketinin db atayabilirsiniz\_datareader, db\_datawriter ve db\_ddladmin rollere sizin için ayarlayan kullanıcı hesabı. Bunlar yeterli çoğu veritabanı nesneleri oluşturmak için ancak kullanıcılar ya da roller oluşturma değil. Hatayı önlemek için bir veritabanı dağıtımdan dışlama kullanıcıları ve rolleri tarafından yoludur. Aşağıdaki öznitelikler içeren PreSource öğe için veritabanının otomatik olarak oluşturulan betiği düzenleyerek bunu yapabilirsiniz:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Proje dosyasındaki PreSource öğesi düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: Proje dosyasında dağıtım ayarlarını Düzenle](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Kullanıcıları veya geliştirme veritabanı rolleri hedef veritabanında gerekiyorsa, Yardım için barındırma sağlayıcınızda başvurun.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Dağıtım sırasında özel betikleri çalıştırarak SQL Server zaman aşımı hatası

### <a name="scenario"></a>Senaryo

Dağıtım sırasında çalıştırılacak özel SQL komut dosyalarını belirttiğiniz ve Web dağıtımı bunları çalıştığında, zaman aşımına.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Farklı işlem modları sahip birden çok betikleri çalıştırma zaman aşımı hatalara neden olabilir. Varsayılan olarak, bir işlem içinde otomatik olarak üretilen betikleri çalıştırın, ancak özel betikler yapın. Seçerseniz **çekme veri ve/veya varolan bir veritabanını şema** seçeneğini **SQL Paketle/Yayımla** sekmesinde ve özel SQL komut dosyası eklerseniz, bazı kodlar üzerinde işlem ayarları değiştirmeniz gerekir böylece tüm betikler, aynı işlem ayarları kullanın. Daha fazla bilgi için [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343.aspx).

Tümü aynı şekilde işlem ayarları yapılandırdınız, ancak bu hatayı almaya devam ediyorsanız, komut dosyalarını ayrı olarak çalıştırmak için olası bir geçici çözüm olduğunu. İçinde **veritabanı betikleri** kılavuzunda **Paketle/Yayımla** SQL sekmesi, NET **INCLUDE** zaman aşımı hatasına neden olur betik için onay kutusunu sonra projeyi yayınlayın. Uygulamasına geri gidip ardından **veritabanı betikleri** kılavuz, bu betiğin seçin **Ekle** onay kutusunu işaretleyin ve Temizle **Ekle** diğer komut dosyaları için onay kutularını. Daha sonra projeyi yeniden yayımlayın. Bu kez yayımladığınızda, yalnızca seçilen özel betiği çalıştırır.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Site bildiriminin Stream veriler henüz kullanılamıyor

### <a name="scenario"></a>Senaryo

Kullanarak bir paket yüklerken *deploy.cmd* dosya t (test) seçeneği ile aşağıdaki hata iletisini görürsünüz:

Hata: Veri akışı, ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' henüz kullanılamıyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Hata iletisi komut bir test raporu üretemiyor anlamına gelir. Y (gerçek yükleme) seçeneğini kullanırsanız, ancak komutu çalıştırabilirsiniz. İleti, yalnızca komut test modunda çalışan bir sorun olduğunu gösterir.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Bu uygulama, ManagedRuntimeVersion v4.0 gerektirir.

### <a name="scenario"></a>Senaryo

Dağıtma girişiminde bulunduğunuzda aşağıdaki hata iletisini görürsünüz:

Kullanmaya çalıştığınız uygulama havuzu 'v2.0' için 'managedRuntimeVersion' özelliği var. Bu uygulama, 'v4.0' gerektirir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4 IIS'de yüklü değil. Dağıtım yaptığınız sunucunun geliştirme bilgisayarınıza ve Visual Studio 2010 'un yüklü, ASP.NET 4 bu bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıtım yaptığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS içinde ASP.NET 4 yükleyin:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions dönüştürme yapılamıyor

### <a name="scenario"></a>Senaryo

Bir paketi dağıtırken, aşağıdaki hata iletisini görürsünüz:

Cast 'Microsoft.Web.Deployment.DeploymentProviderOptions' için ' Microsoft.Web.Deployment.DeploymentProviderOptions' türündeki nesne oluşturulamıyor.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS Web dağıtımı 2.0 yüklü bir sunucu için Web dağıtımı 1.1 UI kullanarak Yöneticisi'nden dağıtmaya çalıştığınız. Bir paket onay içeri aktararak dağıtmak için IIS Uzak Yönetim Aracı'nı kullanıyorsanız **yeni özellikler kullanılabilir** bağlantı oluşturduğunuzda iletişim kutusu. (Bu iletişim kutusu yalnızca bir kez zaman önce bağlantı kurulana görüntülenebilir. Bağlantı temizlemek ve baştan başlamak için IIS Yöneticisi'ni kapatın ve başlatma, yeniden inetmgr girerek/komut isteminde sıfırlayın.) Listelenen özelliklerden biri ise **Web UI dağıtma**ve 8'den daha düşük bir sürüm numarasına sahip, 1.1 ve 2.0 sürümlerinde yüklü Web dağıtımı için dağıttığınız sunucunun olabilir. 2.0 yüklü bir istemciden dağıtmak için sunucu yalnızca Web dağıtımı 2.0 yüklü olması gerekir. Bu sorunu çözmek için barındırma sağlayıcınızla bağlantı kurun gerekecektir.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact yerel bileşenleri yüklenemedi

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

SQL Server sürümü 8482 ADO.NET sağlayıcısı için karşılık gelen Compact yerel bileşenlerinin yüklenemiyor. SQL Server Compact doğru sürümünü yükleyin. Daha fazla ayrıntı için 974247 KB makalesine başvurun.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtılan site olmayan *amd64* ve *x86* bunları yerel uygulamanın altında derlemelerde alt *bin* klasör. Yerel derlemeler bulunan SQL Server yüklü Compact olan bir bilgisayara *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Visual Studio projesinde doğru klasörlerde doğru dosyaları almak için en iyi NuGet SqlServerCompact paketini yüklemek için yoludur. Paket yüklemesi ekler yerel derlemelerine kopyalamak için bir derleme sonrası betik *amd64* ve *x86*. Bu dağıtılacak şekilde sırayla ancak, el ile projeye dahil etmek gerekir. Daha fazla bilgi için [SQL Server Compact dağıtma](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) öğretici.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First uygulama dağıttıktan sonra "Yolu geçerli değil" hatasını

### <a name="scenario"></a>Senaryo

Entity Framework Code First Migrations'ı ve DBMS gibi SQL Server veritabanını bir uygulama dosyasında depoladığı Compact kullanan bir uygulamayı dağıtırsanız\_veri klasörü. Code First Migrations, ilk dağıtımdan sonra veritabanını oluşturmak için yapılandırılmış var. Uygulamayı çalıştırdığınızda aşağıdaki örneğe benzer bir hata iletisi alın:

Yol geçerli değil. Veritabanı için dizini kontrol edin. [Yolu c:\inetpub\wwwroot\App =\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kod ilk veritabanı ancak uygulama oluşturma deniyor\_veri klasörü mevcut değil. Herhangi bir dosya ya da gerekmedi *uygulama\_veri* dağıttığınız veya seçtiğiniz klasör **hariç uygulama\_veri** üzerinde **Web'iPaketle/Yayımla** Proje Özellikleri penceresini sekmesi. Sunucuya kopyalanmasını klasöründeki dosya yoksa, dağıtım işlemi sunucuda bir klasör oluşturmaz. Site için ayarlanan veritabanı zaten varsa dağıtım işleminin olan dosyaları siler ve *uygulama\_veri* klasör seçtiyseniz, kendisini **hedefteki ek dosyaları Kaldır** içinde Yayımlama profili. Sorunu çözmek için bir .txt dosyasına gibi yer tutucu dosyası yerleştirme *uygulama\_veri* klasöründe izniniz olduğundan emin olun **hariç uygulama\_veri** seçili ve yeniden dağıtın.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>", Temel alınan RCW ayrılmış bir COM nesnesi kullanılamaz."

### <a name="scenario"></a>Senaryo

Başarılı olan uygulamanızı dağıtmak için tek tıklamayla yayımlama ve sonra bu hatayı alma başlatın:

Web dağıtım görevi başarısız oldu. (Uzak Aracı URL'sine isteği tamamlayamadı '<https://serverurl.com/msdeploy.axd?site=sitename>'.)  
 Uzak Aracı URL'sine isteği tamamlayamadı '<https://url/msdeploy.axd?site=sitename>'.  
İstek iptal edildi: istek iptal edildi.  
, Temel alınan RCW ayrılmış bir COM nesnesi kullanılamaz.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kapatma ve Visual Studio'yu yeniden başlatmayı genellikle bu hatayı çözmek için gereken tek şey.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Dağıtım başarısız olduğundan kullanıcı kimlik bilgileri için kullanılan yayımlama yok setACL yetkilisi

### <a name="scenario"></a>Senaryo

Belirten bir hata ile yayımlama başarısız (kullandığınız kullanıcı hesabının setACL yetkilisi yok) klasör izinlerini yetkisi yok.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitenin kök klasör izinlerini okuma ve yazma izinleri uygulama\_veri klasörü. Site klasörlerine varsayılan izinlerini doğru olduğundan ve ayarlanması gerekmez biliyorsanız, bu davranışı ekleyerek devre dışı **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (etkileyen tüm profiller için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını Düzenle (.pubxml) profil dosyaları](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Bir uygulama klasörüne yazmak uygulama çalıştığında, erişim reddedildi hataları

### <a name="scenario"></a>Senaryo

Bu klasör için yazma yetkilisi olmadığından oluşturduğunuzda veya düzenlediğinizde uygulama klasörlerden birine dosyasında çalıştığında, uygulamanızın uygulama hataları.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Varsayılan olarak, Visual Studio kümeleri sitenin kök klasör izinlerini okuma ve yazma izinleri uygulama\_veri klasörü. Uygulamanız bir alt klasöre yazma erişimi gerekiyorsa, gösterildiği gibi klasör izinlerini ayarlama ve dağıtma için üretim ortamında bu serideki Eğitmenleri bu klasörün izinlerini ayarlayabilirsiniz. Uygulamanız, sitenizin kök klasörüne yazma erişimi gerekiyorsa, kök klasöründe ekleyerek salt okunur erişim ayarından önlemek sahip **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (etkileyen tüm profiller için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını Düzenle (.pubxml) profil dosyaları](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Yapılandırma hatası - targetFramework özniteliği yüklü .NET Framework sürümünden daha sonraki bir sürümüne başvuruyor.

### <a name="scenario"></a>Senaryo

ASP.NET 4.5 hedefleyen bir web projesi başarıyla yayımlandı, ancak (customErrors modu "kapalı" Web.config dosyasına ayarlayın) uygulamayı çalıştırdığınızda aşağıdaki hatayı alıyorsunuz:

'targetFramework' özniteliği &lt;derleme&gt; Web.config dosyasının öğesi, yalnızca hedef sürüm 4.0 ve daha sonra .NET Framework'ün kullanılır (örneğin, '&lt;derleme targetFramework = "4.0"&gt;'). 'TargetFramework' özniteliği, şu anda yüklü .NET Framework sürümünden daha sonraki bir sürümüne başvuruyor. .NET Framework'ün geçerli hedef sürümü belirtin veya .NET Framework'ün gerekli sürümü yükleyin.

Hata sayfasının kaynak hata kutusu aşağıdaki satırı Web.config hatanın nedenini vurgular:

&lt;derleme targetFramework = "4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucu ASP.NET 4.5 desteklemez. Ne zaman ve ASP.NET 4.5 için destek eklenip eklenemeyeceğini belirlemek için barındırma sağlayıcısına başvurun. ASP.NET 4 veya önceki sürümlerini hedefleyen bir web projesi dağıtmak zorunda sunucusunun yükseltilmesi bir seçenek değilse, bunun yerine.

Aynı hedef için bir ASP.NET 4 veya önceki web proje dağıtırsanız seçin **hedefteki ek dosyaları Kaldır** onay kutusunu **ayarları** sekmesinde **Web'i Yayımla**Sihirbazı. Seçmezseniz **hedefteki ek dosyaları Kaldır**, yapılandırma hatası sayfayı almaya devam edersiniz.

Proje **özellikleri** windows hedef framework açılan listesini içerir, ancak bu sorun, yalnızca değiştirerek çözümlenemiyor **.NET Framework 4.5** için **.NET Framework 4**. Hedef Framework'ü önceki bir framework sürümüne değiştirirseniz, projeyi sonraki framework sürümün derlemelere başvuruları çözümlenmedi ve çalışmaz. El ile bu başvuruları değiştirin veya .NET Framework 4 veya önceki sürümlerini hedefleyen yeni bir proje oluşturmak gerekir. Daha fazla bilgi için [.NET Framework'ü hedefleyen Web siteleri için](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Orta güven hataları

### <a name="scenario"></a>Senaryo

Uygulamanızı üretimde çalıştırdığınızda, Orta güven ilgili bir hata alır.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Çok sayıda üçüncü taraf barındırma sağlayıcıları, web sitenizin bazı şeyler yapmak için izin verilmiyor olduğu anlamına gelir, Orta güvende çalıştırın. Örneğin, uygulama kodu Windows kayıt defterine erişim ve olamaz okunamıyor veya uygulamanızın klasör hiyerarşisi dışında dosyaları yazma. Varsayılan olarak, uygulamanın çalıştığı *tam güven* yerel bilgisayarınızda, yani uygulama üretime dağıtırken başarısız olacağı şeyleri yapabilmesine imkan olabilir.

Uygulamayı gidermek amacıyla yerel IIS ortamında Orta güven içinde çalışacak şekilde yapılandırabilirsiniz. Bunu yapmak için uygulamayı açmak *Web.config* dosya ve ekleme bir **güven** öğesinde **system.web** öğesi, bu örnekte gösterildiği gibi.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Uygulamayı şimdi Orta güven IIS'de bile yerel bilgisayarınızda çalıştırır.

Bunu yapma Azure Orta güven gerektirmediğinden, Azure App Service'e dağıtma. Bu öğretici yazıldığı Şubat 2012'de, zaman içinde Orta güven uygulamanızın çalışması için bu yöntemi kullanarak bir hata Azure'da neden olur.

Entity Framework Code First Migrations'ı kullanıyorsanız ve orta güven uygulamanızın çalıştığı bir barındırma sağlayıcısına dağıttığınız veya üzerinin yüklü olduğundan emin sürüm 5.0 sahip olun. Entity Framework sürümü 4.3, geçişler tam güven veritabanı şemasını güncelleştirmek için gerekiyor.

## <a name="http-40417-not-found-error"></a>HTTP 404,17 bulunamadı hatası

### <a name="scenario"></a>Senaryo

Geliştirme bilgisayarınızda IIS sitesi dağıtıldı çalıştırdığınızda, sunucu Default.aspx işleyemiyor raporlama aşağıdaki hata iletisini görürsünüz:

HTTP Hatası 404,17 - bulunamadı

Talep edilen içeriği, betik gibi görünüyor ve statik dosya işleyici tarafından sunulan değil.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4.5, bilgisayarınızda yüklü değil. Bir Test Ortamı öğreticide ASP.NET 4.5 yüklemek nasıl açıklayan bu seri olarak IIS'ye dağıtma adımları bakın.

> [!div class="step-by-step"]
> [Önceki](deploying-extra-files.md)
