---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: (12, 12) sorunlarını giderme | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2923501289f31243a7341848ed3f7c2142c98e75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754727"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: (12, 12) sorunlarını giderme
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Windows Azure Web sitelerine dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


Bu sayfa, Visual Studio kullanarak bir ASP.NET web uygulamasına dağıtırken karşılaşabileceğiniz bazı yaygın sorunlar açıklanmaktadır. Her biri için bir veya daha fazla olası nedenleri ve karşılık gelen bir çözüm sağlanır.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Sunucu hatası '/' uygulamasında - geçerli özel hata ayarları hatanın ayrıntıları uzaktan görüntülenmesini engeller

### <a name="scenario"></a>Senaryo

Bir siteyi uzak bir konağa dağıttıktan sonra customErrors ayarı Web.config dosyasında bahsetmeleri ancak hatanın asıl nedenini neydi göstermez bir hata iletisi alın:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Yalnızca web uygulamanızı yerel bilgisayarda çalışırken varsayılan olarak, ASP.NET ayrıntılı hata bilgileri gösterir. Genellikle saldırganlar bu bilgileri uygulamada güvenlik açıklarına kullanın mümkün olabilir çünkü web uygulamanıza Internet üzerinden genel kullanıma açık olduğunda, ayrıntılı hata bilgilerini görüntülemek istediğiniz yok. Ancak, bir site veya güncelleştirmeleri bir site için dağıtım yaparken, bazen bir şeyler yanlış gidecek ve gerçek hata iletisini almanız gerekir.

Uzak ana bilgisayarda çalıştığında, ayrıntılı hata iletilerini görüntülemek uygulamayı etkinleştirmek için ayarlanacak Web.config dosyasını düzenleme `customErrors` modu kapalı, uygulama dağıtmanız ve uygulamayı yeniden çalıştırın:

1. Uygulamanın Web.config dosyasını varsa bir `customErrors` öğesinde `system.web` öğe, değişiklik `mode` "kapalı" özniteliği. Aksi halde eklemeniz bir `customErrors` öğesinde `system.web` öğeyle `mode` öznitelik kümesi "kapalı olarak" aşağıdaki örnekte gösterildiği gibi:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Uygulamayı dağıtın.
3. Uygulamayı çalıştırmak ve ne olursa olsun daha önce gerçekleşmesi hataya neden yaptığınız yineleyin. Şimdi gerçek hata iletisidir görebilirsiniz.
4. Özgün hata çözdükten sonra geri `customErrors` ayarlama ve uygulamayı yeniden dağıtın.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Kullanan SQL Server Compact, Web sayfasına erişim reddedildi

### <a name="scenario"></a>Senaryo

SQL Server Compact kullanan bir siteyi dağıtma ve dağıtılan sitede veritabanına erişen bir sayfa çalıştırıldığında, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucusunda ağ hizmeti hesabı olduğundan hizmet SQL Compact yerel ikili dosyaları okuyabilir gerekiyor *bin\amd64* veya *bin\x86* klasör, ancak okuma izinleri bu klasörleri için. Set okuma izni için ağ hizmeti üzerinde *bin* klasör izinlerini alt klasörlere genişletmek emin olun.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Yetersiz izinler nedeniyle yapılandırma dosyası okunamıyor

### <a name="scenario"></a>Senaryo

' A tıkladığınızda Visual Studio yayımlama IIS yerel makinenizde bir uygulamayı dağıtmak için düğmeye başarısız yayımlama ve **çıkış** penceresi bir hata iletisi şuna benzer gösterir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Tek tıklamayla kullanmak için yerel makinenizde IIS yayımlamak, Visual Studio'yu yönetici izinleriyle çalıştırılmalıdır. Visual Studio'yu kapatın ve yönetici izinleriyle yeniden başlatın.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Hedef bilgisayara bağlanamadı... Belirtilen işlem kullanma

### <a name="scenario"></a>Senaryo

' A tıkladığınızda Visual Studio yayımlama bir uygulamayı dağıtmak için düğmeye başarısız yayımlama ve **çıkış** penceresi bir hata iletisi şuna benzer gösterir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Bir proxy sunucusu hedef sunucu ile iletişimi kesintiye. Windows Denetim Masası'ndan veya Internet Explorer'ı seçin **Internet Seçenekleri** seçip **bağlantıları** sekmesi. İçinde **Internet Özellikleri** iletişim kutusu, tıklayın **LAN Ayarları**. İçinde **yerel alan ağı (LAN) ayarları** iletişim kutusu, NET **ayarlarını otomatik olarak algıla** onay kutusu. Ardından Yayımla düğmesine tekrar tıklayın.

Sorun devam ederse, proxy veya güvenlik duvarı ayarları ile yapılabilir belirlemek için sistem yöneticinize başvurun. Standart olmayan bağlantı noktası (8172); Web yönetimi hizmeti dağıtımı için kullandığı Web dağıtımı için sorun olur. diğer bağlantılar için Web dağıtımı, 80 numaralı bağlantı noktasını kullanır. Web yönetimi hizmeti, bir üçüncü taraf barındırma sağlayıcısına dağıttığınız zaman, genellikle kullanıyor.

## <a name="default-net-40-application-pool-does-not-exist"></a>Varsayılan .NET 4.0 uygulama havuzu yok.

### <a name="scenario"></a>Senaryo

.NET Framework 4 gerektiren bir uygulamayı dağıttığınızda, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4 IIS'de yüklü değil. Dağıtım yaptığınız sunucunun geliştirme bilgisayarınıza ve Visual Studio 2010 'un yüklü, ASP.NET 4 bu bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıtım yaptığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS içinde ASP.NET 4 yükleyin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Varsayılan uygulama havuzu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için [bir Test ortamı olarak IIS'ye dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Başlatma dizesinin biçimi, 0 dizininde başlayan belirtime uymuyor.

### <a name="scenario"></a>Senaryo

Tek tıklamayla kullanarak bir uygulamayı dağıttıktan sonra veritabanına erişen bir çalıştırırsanız, aşağıdaki hata iletisini aldığınızda, yayımlama:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Açık *Web.config* dosyasında bağlantı dizesi değerleri ile başlayan olup olmadığını kontrol edin ve dağıtılan site `$(ReplacableToken_`, aşağıdaki örnekte olduğu gibi:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Bağlantı dizelerini şu örnekteki gibi bakarsanız, proje dosyasını düzenleyin ve aşağıdaki özelliği ekleyin `PropertyGroup` tüm derleme yapılandırmaları için olan öğeyi:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Ardından, uygulamayı yeniden dağıtın.

## <a name="http-500-internal-server-error"></a>HTTP 500 İç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda, hatanın nedenini gösteren belirli bilgiler aşağıdaki hata mesajını görebilirsiniz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

500 hataları birçok neden vardır, ancak bu öğreticileri takip ediyorsanız olası nedenlerinden biri, bir XML öğesi bir XML dönüşümü dosyaların yanlış yere yerleştirin olmasıdır. Örneğin, eklediği dönüştürme koyarsanız bu hatayı elde edebileceğiniz bir `<location>` öğesi altında `<system.web>` yerine doğrudan altında `<configuration>`. Bu durumda XML dönüşümü dosyasını düzeltin ve yeniden dağıtma çözümdür.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 iç sunucu hatası

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Site ASP.NET 4 ancak ASP.NET 4 kayıtlı değil IIS'de sunucu üzerinde hedefleri dağıttım. Sunucusunda yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak ASP.NET 4 kaydedin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Varsayılan uygulama havuzu .NET Framework sürümünü el ile ayarlamanız gerekebilir. Daha fazla bilgi için [bir Test ortamı olarak IIS'ye dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Oturum açma başarısız oldu, açılış SQL Server Express veritabanı uygulamasında\_veri

### <a name="scenario"></a>Senaryo

Sizi *Web.config* dosya SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi bir *.mdf* dosyası, *uygulama\_veri* klasörünü ve ilk Aşağıdaki hata iletisini gördüğünüz uygulama çalışma zamanı:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Adını *.mdf* dosya sildiğiniz olsa bile bilgisayarınızda hiç olmadığı kadar mevcut SQL Server Express veritabanı adını eşleşemez *.mdf* önceden var olan veritabanının dosya. Adını değiştirmek *.mdf* dosya hiçbir zaman bir veritabanı adı ve değişiklik olarak kullanılmış bir ad *Web.config* dosyasını yeni bir ad kullanın. Alternatif olarak, kullandığınız [SQL Server Management Studio Express'i](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) önceden var olan SQL Server Express silmek için veritabanları.

## <a name="model-compatibility-cannot-be-checked"></a>Uyumluluk modeli olamaz denetlenmesi

### <a name="scenario"></a>Senaryo

Sizi *Web.config* dosya yeni bir SQL Server Express veritabanına işaret edecek şekilde bağlantı dizesi ve ilk kez uygulamayı çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Veritabanı adı Web.config dosyasına yerleştirirseniz, bilgisayarınızda, bir veritabanı zaten bazı tablolarda ile bulunmayabilir önce hiç olmadığı kadar kullanıldı. Önce bilgisayarınıza ve Değiştir üzerinde kullanılmamış olan yeni bir ad seçin *Web.config* dosya bu yeni veritabanı adını kullanacak şekilde yönlendirin. Alternatif olarak, kullandığınız [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) veya [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) mevcut veritabanı silinemiyor.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Bir komut dosyası, kullanıcılar ya da rolleri oluşturma girişiminde bulunduğunda SQL hatası

### <a name="scenario"></a>Senaryo

Yapılandırılan veritabanı dağıtımı kullanıyorsanız **SQL Paketle/Yayımla** sekmesinde, dağıtım sırasında çalıştırılan SQL betikleri içeren oluşturma kullanıcı veya rol Oluştur komutlar ve komut yürütme başarısız Bu komutlar çalıştırıldığında. Daha ayrıntılı iletiler, aşağıdaki gibi görebilirsiniz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Veritabanı dağıtımda yapılandırıldığında bu hata oluşursa **Web'i Yayımla** Sihirbazı yerine **SQL Paketle/Yayımla** sekmesinde, bir dizi oluşturun [yapılandırma ve Dağıtım](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum ve çözüm, bu sorun giderme sayfasına eklenir.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtım gerçekleştirmek için kullandığınız kullanıcı hesabı, kullanıcılar ya da rolleri oluşturma izni yok. Örneğin, barındırma şirketinin atayabilirsiniz `db_datareader`, `db_datawriter`, ve `db_ddladmin` sizin için ayarlayan kullanıcı hesabı için rolleri. Bunlar yeterli çoğu veritabanı nesneleri oluşturmak için ancak kullanıcılar ya da roller oluşturma değil. Hatayı önlemek için bir veritabanı dağıtımdan dışlama kullanıcıları ve rolleri tarafından yoludur. Düzenleyerek bunu yapabilirsiniz `PreSource` öğe veritabanı için otomatik olarak oluşturulan betik böylece aşağıdaki öznitelikler içerir:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Düzenleme hakkında daha fazla bilgi için `PreSource` öğesinin proje dosyasında [nasıl yapılır: Proje dosyasında dağıtım ayarlarını Düzenle](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Kullanıcıları veya geliştirme veritabanı rolleri hedef veritabanında gerekiyorsa, Yardım için barındırma sağlayıcınızda başvurun.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Dağıtım sırasında özel betikleri çalıştırarak SQL Server zaman aşımı hatası

### <a name="scenario"></a>Senaryo

Dağıtım sırasında çalıştırılacak özel SQL komut dosyalarını belirttiğiniz ve Web dağıtımı bunları çalıştığında, zaman aşımına.

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Farklı işlem modları sahip birden çok betikleri çalıştırma zaman aşımı hatalara neden olabilir. Varsayılan olarak, bir işlem içinde otomatik olarak üretilen betikleri çalıştırın, ancak özel betikler yapın. Seçerseniz **çekme veri ve/veya varolan bir veritabanını şema** seçeneğini **SQL Paketle/Yayımla** sekmesinde ve özel SQL komut dosyası eklerseniz, bazı kodlar üzerinde işlem ayarları değiştirmeniz gerekir böylece tüm betikler, aynı işlem ayarları kullanın. Daha fazla bilgi için [nasıl yapılır: bir veritabanı ile bir Web uygulaması projesi dağıtma](https://msdn.microsoft.com/library/dd465343.aspx).

Tümü aynı şekilde işlem ayarları yapılandırdınız, ancak bu hatayı almaya devam ediyorsanız, komut dosyalarını ayrı olarak çalıştırmak için olası bir geçici çözüm olduğunu. İçinde **veritabanı betikleri** kılavuzunda **Paketle/Yayımla** SQL sekmesi, NET **INCLUDE** zaman aşımı hatasına neden olur betik için onay kutusunu sonra projeyi yayınlayın. Uygulamasına geri gidip ardından **veritabanı betikleri** kılavuz, bu betiğin seçin **Ekle** onay kutusunu işaretleyin ve Temizle **Ekle** diğer komut dosyaları için onay kutularını. Daha sonra projeyi yeniden yayımlayın. Bu kez yayımladığınızda, yalnızca seçilen özel betiği çalıştırır.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Site bildiriminin Stream veriler henüz kullanılamıyor

### <a name="scenario"></a>Senaryo

Kullanarak bir paket yüklerken *deploy.cmd* ile dosya `t` (test) seçeneği, aşağıdaki hata mesajını görebilirsiniz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Hata iletisi komut bir test raporu üretemiyor anlamına gelir. Komutu kullanırsanız ancak çalıştırabilirsiniz `y` (gerçek yükleme) seçeneği. İleti, yalnızca komut test modunda çalışan bir sorun olduğunu gösterir.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Bu uygulama, ManagedRuntimeVersion v4.0 gerektirir.

### <a name="scenario"></a>Senaryo

Dağıtma girişiminde bulunduğunuzda aşağıdaki hata iletisini görürsünüz:

 Hata: Veri akışı, ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' henüz kullanılamıyor. Kullanmaya çalıştığınız uygulama havuzu 'v2.0' için 'managedRuntimeVersion' özelliği var. Bu uygulama, 'v4.0' gerektirir. 

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

ASP.NET 4 IIS'de yüklü değil. Dağıtım yaptığınız sunucunun geliştirme bilgisayarınıza ve Visual Studio 2010 'un yüklü, ASP.NET 4 bu bilgisayarda yüklü ancak IIS'de yüklü değil. Dağıtım yaptığınız sunucuda, yükseltilmiş bir komut istemi açın ve aşağıdaki komutları çalıştırarak IIS içinde ASP.NET 4 yükleyin:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions dönüştürme yapılamıyor

### <a name="scenario"></a>Senaryo

Bir paketi dağıtırken, aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

IIS Web dağıtımı 2.0 yüklü bir sunucu için Web dağıtımı 1.1 UI kullanarak Yöneticisi'nden dağıtmaya çalıştığınız. Bir paket onay içeri aktararak dağıtmak için IIS Uzak Yönetim Aracı'nı kullanıyorsanız **yeni özellikler kullanılabilir** bağlantı oluşturduğunuzda iletişim kutusu. (Bu iletişim kutusu yalnızca bir kez zaman önce bağlantı kurulana görüntülenebilir. Bağlantı temizlemek ve baştan başlamak için IIS Yöneticisi'ni kapatın ve başlatma, yeniden girerek `inetmgr /reset` komut isteminde.) Listelenen özelliklerden biri ise **Web UI dağıtma**ve 8'den daha düşük bir sürüm numarasına sahip, 1.1 ve 2.0 sürümlerinde yüklü Web dağıtımı için dağıttığınız sunucunun olabilir. 2.0 yüklü bir istemciden dağıtmak için sunucu yalnızca Web dağıtımı 2.0 yüklü olması gerekir. Bu sorunu çözmek için barındırma sağlayıcınızla bağlantı kurun gerekecektir.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>SQL Server Compact yerel bileşenleri yüklenemedi

### <a name="scenario"></a>Senaryo

Dağıtılan site çalıştırdığınızda aşağıdaki hata iletisini görürsünüz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Dağıtılan site olmayan *amd64* ve *x86* bunları yerel uygulamanın altında derlemelerde alt *bin* klasör. Yerel derlemeler bulunan SQL Server yüklü Compact olan bir bilgisayara *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Visual Studio projesinde doğru klasörlerde doğru dosyaları almak için en iyi NuGet SqlServerCompact paketini yüklemek için yoludur. Paket yüklemesi ekler yerel derlemelerine kopyalamak için bir derleme sonrası betik *amd64* ve *x86*. Bu dağıtılacak şekilde sırayla ancak, el ile projeye dahil etmek gerekir. Daha fazla bilgi için [SQL Server Compact dağıtma](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) öğretici.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Entity Framework Code First uygulama dağıttıktan sonra "Yolu geçerli değil" hatasını

### <a name="scenario"></a>Senaryo

Entity Framework Code First Migrations'ı ve DBMS gibi SQL Server veritabanını bir uygulama dosyasında depoladığı Compact kullanan bir uygulamayı dağıtırsanız\_veri klasörü. Code First Migrations, ilk dağıtımdan sonra veritabanını oluşturmak için yapılandırılmış var. Uygulamayı çalıştırdığınızda aşağıdaki örneğe benzer bir hata iletisi alın:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Kod ilk veritabanı ancak uygulama oluşturma deniyor\_veri klasörü mevcut değil. Herhangi bir dosya ya da gerekmedi *uygulama\_veri* dağıttığınız veya seçtiğiniz klasör **hariç uygulama\_veri** üzerinde **Web'iPaketle/Yayımla** sekmesinde **proje özellikleri** penceresi. Sunucuya kopyalanmasını klasöründeki dosya yoksa, dağıtım işlemi sunucuda bir klasör oluşturmaz. Site için ayarlanan veritabanı zaten varsa dağıtım işleminin olan dosyaları siler ve *uygulama\_veri* klasör seçtiyseniz, kendisini **hedefteki ek dosyaları Kaldır** içinde Yayımlama profili. Sorunu çözmek için bir .txt dosyasına gibi yer tutucu dosyası yerleştirme *uygulama\_veri* klasöründe izniniz olduğundan emin olun **hariç uygulama\_veri** seçili ve yeniden dağıtın. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>", Temel alınan RCW ayrılmış bir COM nesnesi kullanılamaz."

### <a name="scenario"></a>Senaryo

Başarılı olan uygulamanızı dağıtmak için tek tıklamayla yayımlama ve sonra bu hatayı alma başlatın:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

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

Varsayılan olarak, Visual Studio kümeleri sitenin kök klasör izinlerini okuma ve yazma izinleri uygulama\_veri klasörü. Uygulamanız bir alt klasöre yazma erişimi gerekiyorsa, gösterildiği gibi bu klasörün izinlerini ayarlayabilirsiniz [klasör izinlerini ayarlama](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) ve [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğreticiler. Uygulamanız, sitenizin kök klasörüne yazma erişimi gerekiyorsa, kök klasöründe ekleyerek salt okunur erişim ayarından önlemek sahip **&lt;IncludeSetACLProviderOn hedef&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** yayımlama profili dosyasını (tek bir profil etkilemek için) veya wpp.targets dosyasına (etkileyen tüm profiller için). Bu dosyaları düzenleme hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım ayarlarını Düzenle (.pubxml) profil dosyaları](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Yapılandırma hatası - targetFramework özniteliği yüklü .NET Framework sürümünden daha sonraki bir sürümüne başvuruyor.

### <a name="scenario"></a>Senaryo

ASP.NET 4.5 hedefleyen bir web projesi başarıyla yayımlandı ancak uygulamayı çalıştırdığınızda (ile `customErrors` modunu ayarla "Web.config dosyasında kapalı olarak") aşağıdaki hatayı alıyorsunuz:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Hata sayfasının kaynak hata kutusu aşağıdaki satırı Web.config hatanın nedenini vurgular:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Olası nedeni ve çözümü

Sunucu ASP.NET 4.5 desteklemez. Ne zaman ve ASP.NET 4.5 için destek eklenip eklenemeyeceğini belirlemek için barındırma sağlayıcısına başvurun. ASP.NET 4 veya önceki sürümlerini hedefleyen bir web projesi dağıtmak zorunda sunucusunun yükseltilmesi bir seçenek değilse, bunun yerine. Aynı hedef için bir ASP.NET 4 veya önceki web proje dağıtırsanız seçin **hedefteki ek dosyaları Kaldır** onay kutusunu **ayarları** sekmesinde **Web'i Yayımla**Sihirbazı. Seçmezseniz **hedefteki ek dosyaları Kaldır**, yapılandırma hatası sayfayı almaya devam edersiniz.

Proje **özellikleri** windows hedef framework açılan listesini içerir, ancak bu sorun, yalnızca değiştirerek çözümlenemiyor **.NET Framework 4.5** için **.NET Framework 4**. Hedef Framework'ü önceki bir framework sürümüne değiştirirseniz, projeyi sonraki framework sürümün derlemelere başvuruları çözümlenmedi ve çalışmaz. El ile bu başvuruları değiştirin veya .NET Framework 4 veya önceki sürümlerini hedefleyen yeni bir proje oluşturmak gerekir. Daha fazla bilgi için [.NET Framework'ü hedefleyen Web siteleri için](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
