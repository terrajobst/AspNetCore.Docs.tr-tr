---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web dağıtımı - önerilen kaynakları | Microsoft Docs
author: rick-anderson
description: Bu konuda belgeleri (ASP.NET Yayımlama) web kaynakları nasıl dağıtılacağı hakkında bağlantılar sağlanmaktadır Visual Web De Visual Studio 2010 kullanarak IIS uygulamaları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 78ff183394b5ff92f789b50551d01d28f9bff93b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048174"
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web dağıtımı - önerilen kaynakları
====================
> Bu konuda belgeleri (ASP.NET Yayımlama) web kaynakları nasıl dağıtılacağı hakkında bağlantılar sağlanmaktadır Visual Studio 2010, Visual Web Developer 2010 ve sonraki sürümleri kullanarak uygulamaları IIS.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya yararlı olacak herhangi bir bağlantıyı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) bağlantı.
> 
> > [!NOTE] 
> > 
> > Bu kaynakların birçoğunu yalnızca yeni bir sürümünü yüklerseniz kullanılabilir dağıtım özelliklerini açıklayan [Visual Studio Web yayımlama güncelleştirme](https://go.microsoft.com/fwlink/?LinkID=208120). Bazı özellikler, yalnızca Visual Studio 2012 veya Visual Studio 2013'te kullanılabilir.


Bu konu aşağıdaki bölümleri içermektedir:

- [Web projeleri için dağıtım seçeneklerini anlama](#understanding)
- [Barındırma sağlayıcılarının bir ASP.NET uygulaması için bulma](#findinghosting)
- [Visual Studio'dan bir web uygulaması dağıtma](#fromvs)
- [Bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma](#package)
- [Sürekli Tümleştirme (CI) işlemi kullanan bir web uygulaması dağıtma](#ci)
- [Dağıtım sırasında hedef Web.config veya app.config dosyasını ayarları değiştirmek için Web.config dönüştürmeleri kullanma](#transforms)
- [Dağıtım sırasında hedef web uygulaması ayarlarını değiştirmek için Web dağıtımı parametreleri kullanma](#webdeployparms)
- [Bir uygulama dağıtımı sırasında çevrimdışı olduğundan emin](#appoffline)
- [Bir veritabanı veya değişiklikleri dağıtmak için web uygulama dağıtımının bir parçası olarak bir veritabanı](#databasewithweb)
- [Web uygulama dağıtım veritabanından ayrı olarak dağıtma](#databaseseparate)
- [Üyelik ve profil oluşturma gibi hizmetleri, ASP.NET uygulaması kullanan bir web uygulaması dağıtma](#aspnetmembership)
- [Dağıtım için önceden derleme](#precompiling)
- [Bir intranet web uygulaması dağıtma](#intranet)
- [Kutudan çıktığında otomatik değil genel dağıtım görevleri otomatikleştirme](#automating)
- [Geliştiriciler için Web dağıtımı kullanarak web uygulamaları dağıtabileceğiniz web sunucularını yapılandırma](#configuringservers)
- [Bir barındırma sağlayıcısı için sunucuları yapılandırma](#hostingprovider)
- [Dağıtım sorunlarını giderme](#troubleshooting)
- [Belirli bir dağıtım soru ilgili Yardım alma](#gettinghelp)
- [Ek kaynaklar](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Web projeleri için dağıtım seçeneklerini anlama

- [Visual Studio ve ASP.NET Web dağıtımına genel bakış](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Windows Azure Web dahil olmak üzere siteleri için web projeleri dağıtmak için kaynaklara seçenekleri ve bağlantıları açıklar [kesintisiz teslim](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (gelen otomatik [kaynak denetimi](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) Visual Studio kullanarak yanı sıra.
- [Visual Studio 2012 Web yayımlama geliştirmeleri](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Scott Hanselman Video).
- [VS 2010 Web dağıtımı için genel bakış Post](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi'nın blogu). Eski bir blog gönderisi ancak, bazı Visual Studio 2010 kaynaklara Visual Studio 2012 için hala ilgili bilgiler için bağlantılar içerir.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Barındırma sağlayıcılarının bir ASP.NET uygulaması için bulma

- [ASP.NET barındırma](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Visual Studio'dan bir web uygulaması dağıtma

- [Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Seçenekleri açıklar ve Windows Azure Web siteleri için web projeleri dağıtımıyla ilgili kaynaklara bağlantılar sağlar. Visual Studio'dan dağıtma hakkında bir bölüm içerir.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümlü öğretici serisi, SQL Server veritabanları ile web uygulamalarını dağıtma gösterilir. Veritabanı için dağıtım dbDacFx sağlayıcısı ve Entity Framework Code First geçişleri kullanır. Ayrıca aşağıdakiler hakkında bilgiler içerir [Web.config dosyası dönüşümleri](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [tek tek dosyaların dağıtma](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [komut satırı dağıtım](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), ve [nasıl yapılır Visual Studio web özelleştirme .pubxml dosyalarını düzenleyerek yayımlama kanalı](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Web Forms, MVC ve Web API'sini dahil olmak üzere tüm ASP.NET web projeleri için geçerlidir.)
- [Nasıl yapılır: bir Web projesi kullanarak tek tıklamayla yayımlama Visual Studio'da dağıtmak](https://msdn.microsoft.com/library/dd465337.aspx) (başvuru bilgileri için Visual Studio Web Yayımlama Sihirbazı'nı.)
- [SQL Server Visual Studio kullanarak Compact ile ASP.NET Web uygulaması dağıtma](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Önceki bir sürümünü budur **Visual Studio kullanarak ASP.NET Web dağıtımı** bu bölümün başında listelenen. Çoğunlukla yararlı şu an SQL Server Compact veritabanları dağıtmak ve SQL Server Compact'dan SQL Server'ın tam sürüme geçirme hakkında bilgi için.
- [.NET çok katmanlı uygulama kullanarak depolama tabloları, kuyrukları ve Blobları](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). 5 bölümlü öğretici serisi, bir MVC projesi oluşturun ve bir Windows Azure bulut hizmetine dağıtma gösterilir.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma

- [Nasıl yapılır: Visual Studio'da bir Web dağıtım paketi oluşturma](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Nasıl yapılır: Visual Studio tarafından oluşturulan deploy.cmd dosyası kullanarak bir dağıtım paketi yükleme](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Geliştirme kutusundaki IIS ve bir üçüncü taraf konak dağıtmak için Web dağıtımı paketi kullanarak](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (Sayed Hashimi'nın blogu). IIS Yöneticisi'ni bir dağıtım paketi IIS'de yerel bilgisayara yükleyip bir barındırma adresindeki, şirket için nasıl kullanılacağını uzaktan yönetim için IIS Yöneticisi'ni destekler.
- [Web dağıtımı paketi gelen Visual Studio 2010 oluşturma](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET web sitesi). Komut satırı paket oluşturma ve yükleme için yönergeler içerir.
- [Bir kez yayımlama her yerden paketini](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi'nın blogu). Bir paket birden çok sunucuya dağıtabilirsiniz, böylece birden fazla hedef ortam için Web.config dosyasının dönüştürme işlemini otomatikleştiren bir NuGet paketi tanıtır. Ayrıca bkz. [PackageWeb video](https://www.youtube.com/watch?v=-LvUJFI8CzM) Sayed Hashimi tarafından.

Ayrıca aşağıdaki bölüme bakın.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Sürekli Tümleştirme (CI) işlemi kullanan bir web uygulaması dağıtma

- [Sürekli tümleştirme ve kesintisiz teslim (Windows Azure ile gerçek bulut uygulamaları oluşturma).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Sürekli tümleştirme ve kesintisiz teslim tanıtır E-kitap bölüm.
- [Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Windows Azure Web siteleri için web projeleri dağıtmak için kaynaklara seçenekleri ve bağlantıları açıklanmaktadır. Kaynak denetiminden dağıtım otomatikleştirme hakkında bir bölüm içerir.
- [Kuruluş senaryolarında Web uygulamalarını dağıtma](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 bölümlü öğretici serisi, Visual Studio 2010 ve Team Foundation Server 2010'u kullanarak bir CI işlemi dağıtımda otomatikleştirmek nasıl gösterilir.
- [Microsoft Build Engine içinde: MSBuild ve Team Foundation derlemesi Sayed Hashimi ve William Bartholomew kullanarak](http://msbuildbook.com). Bu kitap, bir web kaynağını, ancak MSBuild sürekli tümleştirme senaryoları için yapılandırmak nasıl öğrenmek için temel bir kılavuz gelir.
- [MSBuild uzantısı paketi](https://github.com/mikefourie/MSBuildExtensionPack). Dağıtım görevleri içerir.
- [Team Foundation Yapı Özelleştirme Kılavuzu](https://aka.ms/vsarsolutions). Team Foundation Server'ı ayarlama belgeleri ALM Rangers tarafından web dağıtımı kapsar ve öğreticiler ve videolar içerir.
- [SlowCheetah XML dönüştüren bir CI sunucusundan](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Sayed Hashimi'nın blogu). Bir Visual Studio app.config ve diğer XML dosyaları dönüştürme eklentisi SlowCheetah kullanımı açıklanmaktadır.

Ayrıca bkz. [bir uygulama olduğundan emin olarak çevrimdışı dağıtımı sırasında](aspnet-web-deployment-content-map.md#appoffline) daha sonra bu sayfayı.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Dağıtım sırasında hedef Web.config veya app.config dosyasını ayarları değiştirmek için Web.config dönüştürmeleri kullanma

- [Web.config dosyası dönüşümleri](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Visual Studio kullanarak Web projesi dağıtımı için Web.config dönüşümü sözdizimi](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Araçlar 2012.2 - web.config dönüşümler web](https://www.youtube.com/watch?v=HdPK8mxpKEI) (YouTube videosu Sayed Hashimi tarafından). Ayarlama ve Web.config dönüşümler Önizleme gösterilmektedir.
- [Web.config dönüştürmesini nasıl devre dışı bırakabilirim?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Web dağıtımı parametreleri Web.config dönüşümleri yerine ne zaman kullanmalıyım?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Yararlanmasına imkan üzerinde yayımlanan bir XDT (XML belge dönüştürme)](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (.NET Web geliştirme ve araçları blog). Web.config dosyası dönüştürme altyapısı için kaynak kodunu kullanılabilirliğini olduğunu bildirir ve bunu kullanan bazı araçları listeler.
- [Windows Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blogu). Hedef ortamınızı Microsoft Azure Web siteleri ve dönüştürmek istediğiniz alternatif Web.config dönüştüren `appSettings` veya `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Dağıtım sırasında hedef web uygulaması ayarlarını değiştirmek için Web dağıtımı parametreleri kullanma

- [Nasıl yapılır: kullanım Web Dağıtımı Web dağıtım paketi parametrelerinde](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Yayımla uygulama ayarlarını güncelleştirmek nasıl tabanlı yayımlama profilinde](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (Sayed Hashimi'nın blogu). Visual Studio'ya parametreleri nasıl tümleştirmek için Web dağıtımı yayımlama profillerini gösterir.
- [Web dağıtımı parametrelemeyi](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET web sitesi).
- [Web dağıtmak parametrelemeyi eylem](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi'nın blogu).
- [Web dağıtımı parametrelemeyi vs. Web.config dönüştürmesini](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Vishal Joshi'nın blogu).
- [Windows Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blogu). Web için bir alternatif dağıtırsanız parametreleri hedef ortamınızı Microsoft Azure Web siteleri ve Parametreleştirme istiyorsanız `appSettings` veya `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Bir uygulama dağıtımı sırasında çevrimdışı olduğundan emin

- [Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirme dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Bölümüne bakın **uygulama dağıtımı sırasında çevrimdışı gerçekleştirin.**
- [Bir uygulamayı yayımlamadan önce çevrimdışına](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.NET site). Web dağıtımı bir uygulama işlenmesini otomatikleştiren 3.0 yerleşik bir özelliği açıklanmıştır\_offline.htm dosya. Bu özellik ile özel uygulama çalışmaz\_offline.htm dosyaları.
- [Yayımlama sırasında çevrimdışı web uygulamanızı nasıl](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (Sayed Hashimi'nın blogu). Özel bir uygulama kullanarak sürecini otomatik hale getirmek nasıl\_offline.htm dosya.
- [Web uygulaması çevrimdışı ve usechecksum Yayımlama güncelleştirmeleri](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Microsoft Web geliştirme blog). Uygulamasının kullanımına otomatikleştirmek için başka bir seçenek\_offline.htm dosya.
- [Web dağıtımı 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.NET site). Özel uygulama için yeni Web dağıtımı 3.5 özelliği\_offline.htm dosyaları.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Bir veritabanı veya değişiklikleri dağıtmak için web uygulama dağıtımının bir parçası olarak bir veritabanı

- [Visual Studio'da veritabanı dağıtımı yapılandırma](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Bir veritabanı ile bir web projesi dağıtma seçeneklerine genel bakış.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümlü öğretici serisi dbDacFx sağlayıcısı ve Entity Framework Code First Migrations kullanılarak veritabanı dağıtımı gösterilir.
- [Nasıl yapılır: bir Web dağıtımı tek tıklatmayla kullanarak proje Visual Studio'ya Yayımla](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Windows Azure Web sitesine üyeliği, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC 5 uygulaması dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Derlemeler ve tek bir SQL Server kullanan bir uygulama dağıtır uzun bir öğretici hem de üyelik ve uygulama verileri için veritabanı.
- [SQL Server Visual Studio kullanarak Compact ile ASP.NET Web uygulaması dağıtma](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12 bölümlü öğretici serisi, SQL Server Compact veritabanları dağıtma ve SQL Server Compact'dan SQL Server'ın tam sürüme geçirmek nasıl gösterir.

Ayrıca bir web uygulaması oluşturma ve bir web dağıtım paketi yükleyerek ve bu sayfada bir sürekli tümleştirme (CI) işlemi kullanarak bir web uygulaması dağıtma dağıtma konusuna bakın.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Web uygulama dağıtım veritabanından ayrı olarak dağıtma

- [SQL Server veri Araçları](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Bir SQL Server veritabanı projesi içinde verileri dahil olmak üzere](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (SQL Server veri araçları ekip blogu). Nasıl hem şema hem de veri bir veritabanı dağıtımı sırasında dağıtılır.
- [Bir veritabanı için Windows Azure dağıtma](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure site)
- [Windows Azure SQL veritabanı'nı (önceki adıyla SQL Azure) geçirme veritabanlarına](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Bir veritabanını SQL SSDT kullanarak Azure'a geçirme](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (SQL Server veri araçları ekip blogu).
- [Windows Azure veri merkezli uygulamaları geçirme](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [SQL Server veritabanları Windows Azure SQL veritabanına geçirme](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Üyelik ve profil oluşturma gibi hizmetleri, ASP.NET uygulaması kullanan bir web uygulaması dağıtma

- [Windows Azure Web sitesine üyeliği, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC 5 uygulaması dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Derlemeler ve tek bir SQL Server kullanan bir uygulama dağıtır uzun bir öğretici hem de üyelik ve uygulama verileri için veritabanı.
- [ASP.NET Identity](https://asp.net/identity/). ASP.NET kimliği için kaynaklar.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümlü öğretici serisi, bir ASP.NET üyelik veritabanından dağıtma gösterilir.
- [Uygulama hizmetleri kullanan bir Web sitesi yapılandırma](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Web sitesi için projeleri ancak aynı zamanda web uygulama projeleri için ilgili.
- [Kullanıcılar ve roller üretim Web sitesinde](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Web sitesi için projeleri ancak aynı zamanda web uygulama projeleri için ilgili.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Dağıtım için önceden derleme

- [ASP.NET Web uygulaması projesi ön derleme genel bakış](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Paket/yayımlama Web sekmesi, proje özellikleri](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Gelişmiş Ayarlar iletişim kutusu ön derleme yap](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Bir intranet web uygulaması dağıtma

- [Visual Studio 2013'te ASP.NET ile şirket içi Kurumsal kimlik doğrulama seçeneği (ADFS) kullanmak](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog Vittorio Bertocci tarafından.).
- [ASP.NET MVC kullanarak Intranet sitesine oluşturma](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Visual Studio 2010 için eski izlenecek writen Visual Studio 2013'te tanıtılan intranet proje şablonları önemli değişikliklere yansıtacak değil.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Kutudan çıktığında otomatik değil genel dağıtım görevleri otomatikleştirme

- [Visual Studio kullanarak ASP.NET Web Dağıtımı: dağıtma ek dosyaları](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Web yayımlama klasör izinleri ayarlama](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (Sayed Hashimi'nın blogu).
- [Hedefleri dosyasını bir web projesi paketi için kayıt defteri ayarları içerecek şekilde genişletmek nasıl](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (Web geliştirme araçları blog).
- [XML (Web.config) dönüştürme genişletme](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (Sayed Hashimi'nın blogu). Özel XDT dönüşümler oluşturulacağını gösterir.
- [Web Dağıtım Aracı (MSDeploy) özel sağlayıcı Al 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Sayed Hashimi'nın blogu). Web dağıtımı özel bir sağlayıcı oluşturulacağını gösterir.
- [Paket ve COM bileşenleri dağıtma](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Web geliştirme araçları blog).
- [Paket .NET derlemelerini nasıl](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (Web geliştirme araçları blog). GAC derlemeleri dağıtma konusunda.
- [Her şeyin - Initialize bilgisayarınızı Windows Azure VM Web sunucunuzu IIS, Web dağıtımı ve diğer hizmetler için komut dosyası](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (Tugberk Ugurlu'nın blogu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Geliştiriciler için Web dağıtımı kullanarak web uygulamaları dağıtabileceğiniz web sunucularını yapılandırma

- [Yönetici ve yönetici olmayan dağıtımlar için yükleme ve yapılandırma Web dağıtımı](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.NET site).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Bir barındırma sağlayıcısı için sunucuları yapılandırma

- [Microsoft ASP.NET 4 barındırma Dağıtım Kılavuzu'na](https://go.microsoft.com/fwlink/?LinkId=191365) (Microsoft İndirme Merkezi).
- [Bir profili XML dosyası oluşturma](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.NET site).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Dağıtım sorunlarını giderme

- [Windows Azure Web siteleri Visual Studio'daki sorun giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure site).
- [Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Sorun giderme ortak sorunları Web dağıtımı](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web dağıtımı hata kodları](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.NET site).
- [Web dağıtımı ile ilgili SSS Visual Studio ve ASP.NET için](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Çekirdek IIS ve ASP.NET geliştirme sunucusu arasındaki farklar](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Geliştirme ve üretim ortak yapılandırma farklılıkları](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Orta güven ASP.NET uygulamalarında barındırma](http://www.4guysfromrolla.com/articles/100307-1.aspx) (Rolla sitesinden 4 yazarlar).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Belirli bir dağıtım soru ilgili Yardım alma

- [ASP.NET yapılandırma ve dağıtım forumunu](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Ek Kaynaklar

Bu bölümde, Visual Studio ve IIS dağıtım araçlarının nasıl kullanılacağı hakkında daha fazla bilgi edinmek için yararlı olabilecek ek kaynaklara bağlantılar sağlanmaktadır.

Aşağıdaki Bloglara sık Visual Studio web dağıtımı hakkında bilgi içerir:

- [Web geliştirme araçları Microsoft Web günlüğü](https://blogs.msdn.com/b/webdevtools/).
- [Sayed Hashimi'nın blogu](http://www.sedodream.com/).

Aşağıdaki kaynaklar, Web dağıtımı, web uygulama projesi dağıtım görevlerini gerçekleştirmek için Visual Studio kullanan IIS framework ile ilgili belgeler sağlar. Web dağıtımı hakkında sorular sorabilirsiniz [Web dağıtım aracı Forumu](https://go.microsoft.com/fwlink/?LinkId=149411) IIS.NET web sitesinde.

- [Web giriş dağıtmak](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Yükleme ve Web yapılandırma dağıtmak](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Web otomatikleştirmek için PowerShell betiklerini Dağıt Kurulum](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Web dağıtım aracı](https://go.microsoft.com/fwlink/?LinkId=151481). İçeriği düğümü TechNet sitesindeki belgeleri Web dağıtımı için en üst düzey tablosu. Sayfaları yıldır güncelleştirilmediği TechNet çoğunu faydalı başvuru bilgileri içerir.
- [Microsoft.Web.deployment'un Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). API belgelerine değil güncelleştirilmiştir 1.0 sürümünden beri.
- [Microsoft Web dağıtım ekibi Web günlüğü](https://blogs.iis.net/msdeploy/default.aspx).
- [Sekme IIS.NET web sitesinde yayımlamak](https://www.iis.net/learn/publish).
