---
uid: whitepapers/aspnet-web-deployment-content-map
title: ASP.NET Web dağıtımı - önerilen kaynaklar | Microsoft Docs
author: rick-anderson
description: Bu konu, belgeleri (ASP.NET Yayımlama) nasıl dağıtılacağı hakkında daha fazla kaynak web bağlantılarını sağlar. Visual Studio 2010, Visual Web De kullanarak IIS uygulamaları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: d29ea65ee5b7056d04f2aa637c36b8216fdff411
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363189"
---
<a name="aspnet-web-deployment---recommended-resources"></a>ASP.NET Web dağıtımı - önerilen kaynaklar
====================
> Bu konu, belgeleri (ASP.NET Yayımlama) nasıl dağıtılacağı hakkında daha fazla kaynak web bağlantılarını sağlar. Visual Studio 2010 Visual Web Developer 2010 ve sonraki sürümler kullanarak IIS uygulamaları.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya kullanışlı olabilecek diğer bir bağlantı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) bağlantı.
> 
> > [!NOTE] 
> > 
> > Bu kaynakların çoğunu yalnızca en son sürümünü yüklerseniz kullanılabilen dağıtım özellikleri açıklayan [Visual Studio Web yayımlama güncelleştirme](https://go.microsoft.com/fwlink/?LinkID=208120). Bazı özellikler, yalnızca Visual Studio 2012 veya Visual Studio 2013'te kullanılabilir.


Bu konu aşağıdaki bölümleri içermektedir:

- [Web projeleri için dağıtım seçeneklerini anlama](#understanding)
- [Barındırma sağlayıcıları için bir ASP.NET uygulaması bulma](#findinghosting)
- [Visual Studio'dan bir web uygulaması dağıtma](#fromvs)
- [Bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma](#package)
- [Sürekli Tümleştirme (CI) işlem kullanarak bir web uygulaması dağıtma](#ci)
- [Dağıtım sırasında hedef Web.config veya app.config dosyası ayarlarını değiştirmek için Web.config Dönüşümleri kullanma](#transforms)
- [Dağıtım sırasında hedef web uygulaması ayarlarını değiştirmek için Web dağıtımı parametreleri kullanma](#webdeployparms)
- [Bir uygulama dağıtımı sırasında çevrimdışı olduğundan emin](#appoffline)
- [Bir veritabanı veya değişiklikleri dağıtmak için web uygulama dağıtımının parçası olarak veritabanı](#databasewithweb)
- [Web uygulaması dağıtım veritabanından ayrı olarak dağıtma](#databaseseparate)
- [Üyelik ve profil oluşturma gibi hizmet, ASP.NET uygulama kullanan bir web uygulaması dağıtma](#aspnetmembership)
- [Dağıtım için önceden derleme](#precompiling)
- [Bir intranet web uygulaması dağıtma](#intranet)
- [Kullanıma hazır otomatik değil genel dağıtım görevleri otomatikleştirme](#automating)
- [Geliştiriciler için Web dağıtımı kullanarak web uygulamalarını dağıtabilirsiniz böylece web sunucularını yapılandırma](#configuringservers)
- [Bir barındırma sağlayıcısı için sunucuları yapılandırma](#hostingprovider)
- [Dağıtım sorunlarını giderme](#troubleshooting)
- [Belirli bir dağıtım soru ilgili Yardım alma](#gettinghelp)
- [Ek kaynaklar](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Web projeleri için dağıtım seçeneklerini anlama

- [Visual Studio ve ASP.NET için Web dağıtımı genel bakış](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Bir Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Windows Azure Web dahil olmak üzere siteleri için web projeleri dağıtma için kaynaklar için seçenekleri ve bağlantıları açıklar [sürekli teslim](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (gelen otomatikleştirilmiş [kaynak denetimi](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)) Visual Studio kullanarak yanı sıra.
- [Visual Studio 2012 Web yayımlama geliştirmeleri](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (Scott Hanselman tarafından Video).
- [VS 2010 Web dağıtımı için genel bakış Post](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (Vishal Joshi'nın blogu). Eski bir blog gönderisi, ancak bazı Visual Studio 2010 kaynaklara hala Visual Studio 2012 için ilgili bilgiler için bağlantılar.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Barındırma sağlayıcıları için bir ASP.NET uygulaması bulma

- [ASP.NET barındırma](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Visual Studio'dan bir web uygulaması dağıtma

- [Bir Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Seçenekleri açıklar ve web projeleri için Windows Azure Web sitelerini dağıtmaya yönelik kaynaklara bağlantılar sağlanmaktadır. Visual Studio'dan dağıtma hakkında daha fazla bölüm içerir.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümden öğretici serisinde, SQL Server veritabanları ile web uygulamaları dağıtma gösterilmektedir. Veritabanı için dağıtım dbDacFx sağlayıcısı ve Entity Framework Code First Migrations'ı kullanır. Ayrıca hakkında bilgiler içerir [Web.config dosyası dönüşümleri](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [tek tek dosyaları dağıtma](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [komut satırı dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), ve [nasıl Visual Studio web özelleştirme .pubxml dosyalarını düzenleyerek yayımlama kanalı](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Web Forms, MVC ve Web API'sini dahil olmak üzere tüm ASP.NET web projeleri için geçerlidir.)
- [Nasıl yapılır: bir Web projesi kullanarak tek tıklamayla yayımlama Visual Studio'da dağıtma](https://msdn.microsoft.com/library/dd465337.aspx) (başvuru bilgilerini Visual Studio Web Yayımlama Sihirbazı'nı.)
- [SQL Server Visual Studio kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Önceki bir sürümünü budur **Visual Studio kullanarak ASP.NET Web dağıtımı** bu bölümün başında listelenen. Çoğunlukla yararlı şimdi SQL Server Compact veritabanları dağıtma ve SQL Server Compact'dan SQL Server'ın tam sürüme geçirme hakkında bilgi için.
- [.NET çok katmanlı depolama tabloları, kuyrukları ve Blobları](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). 5 bölümden oluşan öğretici, bir MVC projesi oluşturma ve bir Windows Azure bulut hizmetinde dağıtmayı gösterir.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Bir web uygulaması oluşturma ve bir web dağıtım paketi yükleme dağıtma

- [Nasıl yapılır: Visual Studio'da bir Web dağıtım paketi oluşturma](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Nasıl yapılır: Visual Studio tarafından oluşturulan deploy.cmd dosyasını kullanarak bir dağıtım paketi yükleme](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Geliştirme kutusunda IIS'de ve üçüncü taraf bir ana bilgisayara dağıtmak için bir Web dağıtımı paketi kullanarak](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (Sayed Hashimi'nın blogu). Bir dağıtım paketi, yerel bilgisayarda IIS yüklemek ve şirket, barındırma IIS Yöneticisi'ni kullanmaya nasıl uzaktan yönetim için IIS Yöneticisi'ni destekler.
- [Web dağıtımı paketi gelen Visual Studio 2010 oluşturmaya](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (IIS.NET web sitesi). Komut satırı paket oluşturma ve yükleme için yönergeler içerir.
- [Bir kez yayımlama herhangi bir paket](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (Sayed Hashimi'nın blogu). Bir paket birden çok sunucuya dağıtabilirsiniz böylece birden çok hedef ortam için Web.config dosyasına dönüştürme işlemini otomatikleştiren bir NuGet paketi sunar. Ayrıca bkz: [PackageWeb video](https://www.youtube.com/watch?v=-LvUJFI8CzM) Sayed Hashimi tarafından.

Ayrıca aşağıdaki bölüme bakın.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Sürekli Tümleştirme (CI) işlem kullanarak bir web uygulaması dağıtma

- [Sürekli tümleştirme ve sürekli teslim (Windows Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Sürekli tümleştirme ve sürekli teslim E-kitap bölümü.
- [Bir Windows Azure Web sitesinin nasıl dağıtılacağı](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Web projeleri için Windows Azure Web siteleri dağıtmak için kaynaklara seçenekleri ve bağlantıları açıklar. Kaynak denetiminden dağıtım otomatikleştirme hakkında bir bölüm içerir.
- [Kurumsal senaryolarda Web uygulamaları dağıtma](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). 40 bölümlü öğretici serisinde, Visual Studio 2010 ve Team Foundation Server 2010'u kullanarak bir CI işlemi dağıtımı otomatik hale getirmek nasıl gösterir.
- [Microsoft Build Engine içinde: MSBuild ve Team Foundation derlemesi Sayed Hashimi ve William Bartholomew kullanarak](http://msbuildbook.com). Bu, bir web kaynağı olmak üzere bir kitap, ancak öğrenme MSBuild sürekli tümleştirme senaryoları için nasıl yapılandıracağınızı öğrenmek için temel bir kılavuz değildir.
- [MSBuild uzantı paketi](https://github.com/mikefourie/MSBuildExtensionPack). Dağıtım görevlerini içerir.
- [Team Foundation Yapı Özelleştirme Kılavuzu](https://aka.ms/vsarsolutions). Team Foundation Server'ı ayarlama ALM Rangers tarafından belgeleri, web dağıtımı kapsar ve öğreticiler ve videolar içerir.
- [SlowCheetah XML dönüştüren bir CI sunucudan](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (Sayed Hashimi'nın blogu). Visual Studio app.config ve diğer XML dosyalarını dönüştürme eklentisi, SlowCheetah kullanmayı açıklar.

Ayrıca bkz: [bir uygulama olduğundan emin olarak çevrimdışı dağıtım sırasında](aspnet-web-deployment-content-map.md#appoffline) bu sayfada daha sonra.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Dağıtım sırasında hedef Web.config veya app.config dosyası ayarlarını değiştirmek için Web.config Dönüşümleri kullanma

- [Web.config dosyası dönüşümleri](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Visual Studio kullanarak Web projesi dağıtımı için Web.config dönüşümü sözdizimi](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Web araçları 2012.2 - web.config dönüşümleri](https://www.youtube.com/watch?v=HdPK8mxpKEI) (YouTube video Sayed Hashimi tarafından). Ayarlama ve Web.config dönüşümleri Önizleme gösterilmektedir.
- [Web.config dönüşümünün nasıl devre dışı bırakabilirim?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Web dağıtımı parametrelerini Web.config dönüşümlerini yerine ne zaman kullanmalıyım?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Codeplex.com'u üzerinde yayımlanan bir XDT (XML belge dönüştürme)](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (.NET Web geliştirme ve Araçlar blog). Web.config dosyası dönüştürme motoru için kaynak kodu kullanılabilirliğini sunar ve onu kullanan bazı araçları listeler.
- [Windows Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blogu). Hedef ortamınızı Windows Azure Web siteleri ve dönüştürmek istediğiniz alternatif Web.config dönüşümleri `appSettings` veya `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Dağıtım sırasında hedef web uygulaması ayarlarını değiştirmek için Web dağıtımı parametreleri kullanma

- [Nasıl yapılır: kullanım Web Dağıtımı Web dağıtım paketi parametrelerinde](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: Yayımla üzerinde uygulama ayarlarını güncelleştirme yayımlama profiline dayalı](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (Sayed Hashimi'nın blogu). Visual Studio'ya parametreleri nasıl tümleştirmek için Web dağıtımı yayımlama profilleri gösterir.
- [Web dağıtımı Parametreleştirme](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (IIS.NET web sitesi).
- [Web eylemi Parametreleştirme dağıtma](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (Vishal Joshi'nın blogu).
- [Web dağıtımı Parametreleştirme vs. Web.config dönüşümünün](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (Vishal Joshi'nın blogu).
- [Windows Azure Web siteleri: Nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blogu). Alternatif Web dağıtımı parametreleri hedef ortamınızı Windows Azure Web siteleri ve parametreleştirmek istediğiniz `appSettings` veya `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Bir uygulama dağıtımı sırasında çevrimdışı olduğundan emin

- [Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Bölümüne bakın **uygulama dağıtımı sırasında çevrimdışı duruma alın.**
- [Bir uygulamayı yayımlamadan önce çevrimdışına](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.NET site). Web dağıtımı otomatik hale getiren bir uygulamanın işleme 3.0 yerleşik bir özellik açıklanmaktadır\_offline.htm dosya. Bu özellik, özel uygulama ile çalışmıyor\_offline.htm dosyaları.
- [Yayımlama sırasında çevrimdışı web uygulamanızı nasıl](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (Sayed Hashimi'nın blogu). Özel bir uygulama kullanma işlemi otomatik hale getirmek nasıl\_offline.htm dosya.
- [Uygulama çevrimdışı ve usechecksum güncelleştirmelerin yayınlanması web](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (Microsoft Web geliştirme blogu). Uygulama kullanımını otomatikleştirme için başka bir seçenek\_offline.htm dosya.
- [Web dağıtımı 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.NET site). Özel uygulama için yeni Web dağıtımı 3.5 özellik\_offline.htm dosyaları.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Bir veritabanı veya değişiklikleri dağıtmak için web uygulama dağıtımının parçası olarak veritabanı

- [Visual Studio'da veritabanı dağıtımı yapılandırma](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Bir veritabanı ile bir web projesi dağıtma seçeneklerine genel bakış.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümden öğretici serisi dbDacFx sağlayıcısı ve Entity Framework Code First Migrations'ı kullanarak veritabanı dağıtımı gösterir.
- [Nasıl yapılır: bir Web dağıtımı kullanarak tek tıklamayla projeyi Visual Studio'da yayımlama](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Bir Windows Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtın](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Oluşturan ve dağıtan tek bir SQL Server kullanan bir uygulamayı uzun bir öğretici için üyeliği ve uygulama verilerini hem de veritabanı.
- [SQL Server Visual Studio kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). 12 bölümden öğretici serisinde, SQL Server Compact veritabanları dağıtma ve SQL Server Compact'dan SQL Server'ın tam sürüme geçirmek nasıl gösterir.

Ayrıca oluşturarak ve web dağıtım paketi yükleme ve bu sayfada daha önce bir sürekli tümleştirme (CI) işlem kullanarak bir web uygulaması dağıtma bir web uygulamasını dağıtma konusuna bakın.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Web uygulaması dağıtım veritabanından ayrı olarak dağıtma

- [SQL Server veri Araçları](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [SQL Server veritabanı projesi veriler dahil olmak üzere](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (SQL Server veri araçları ekip blogu). Bir veritabanı dağıtım yaparken hem şema hem de veri dağıtma
- [Bir veritabanını Windows Azure'a dağıtmak nasıl](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure site)
- [Windows Azure SQL veritabanı'nı (eski adı SQL Azure) geçirme veritabanlarına](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Bir veritabanı için SSDT ile SQL Azure geçiş](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (SQL Server veri araçları ekip blogu).
- [Windows azure'a geçişini veri odaklı uygulamaları](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [SQL Server veritabanları Windows Azure SQL veritabanı'na geçirme](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Üyelik ve profil oluşturma gibi hizmet, ASP.NET uygulama kullanan bir web uygulaması dağıtma

- [Bir Windows Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtın](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Oluşturan ve dağıtan tek bir SQL Server kullanan bir uygulamayı uzun bir öğretici için üyeliği ve uygulama verilerini hem de veritabanı.
- [ASP.NET Identity](https://asp.net/identity/). ASP.NET Identity için kaynaklar.
- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). 12 bölümden öğretici serisinin nasıl bir ASP.NET üyelik veritabanı dağıtılacağı gösterir.
- [App Services kullanan bir Web sitesi yapılandırma](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Web sitesi için projeleri ancak aynı zamanda web uygulama projeleri için ilgili.
- [Kullanıcıları ve rolleri üretim Web sitesindeki](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Web sitesi için projeleri ancak aynı zamanda web uygulama projeleri için ilgili.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Dağıtım için önceden derleme

- [ASP.NET Web uygulaması projesi ön derleme genel bakış](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Paketle/Yayımla Web sekmesi, proje özellikleri](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Gelişmiş ön derleme Ayarları iletişim kutusu](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Bir intranet web uygulaması dağıtma

- [Visual Studio 2013'te ASP.NET ile şirket içi Kurumsal kimlik doğrulama seçeneği (ADFS) kullanma](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog Vittorio Bertocci'nin tarafından.).
- [ASP.NET MVC kullanarak bir Intranet sitesi oluşturulacağını](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). Visual Studio 2010 için eski izlenecek writen intranet proje şablonları, Visual Studio 2013'te önemli değişikliklere yansıtmıyor.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Kullanıma hazır otomatik değil genel dağıtım görevleri otomatikleştirme

- [Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Web yayımlama üzerinde klasör izinlerini ayarlama](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (Sayed Hashimi'nın blogu).
- [Web proje paketi için kayıt defteri ayarları içerecek şekilde hedef dosya genişletme](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (Web geliştirme araçları blog).
- [XML (Web.config) dönüştürme genişletme](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (Sayed Hashimi'nın blogu). Özel XDT dönüşümler oluşturma işlemi gösterilmektedir.
- [Web Dağıtım Aracı (MSDeploy) özel sağlayıcı Al 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (Sayed Hashimi'nın blogu). Web dağıtımı özel bir sağlayıcı oluşturma işlemi gösterilmektedir.
- [Paket ve COM bileşenleri dağıtma](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (Web geliştirme araçları blog).
- [Paket .NET derlemeleri nasıl](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (Web geliştirme araçları blog). Derlemeleri GAC'ye dağıtma
- [Her şeyin - başlatma bilgisayarınızı Windows Azure VM için Web sunucunuzu IIS, Web dağıtımı ve diğer hizmetler ile betik](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (Tugberk Ugurlu'nın blogu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Geliştiriciler için Web dağıtımı kullanarak web uygulamalarını dağıtabilirsiniz böylece web sunucularını yapılandırma

- [Yönetici ve yönetici olmayan dağıtımları için yükleme ve yapılandırma Web dağıtımı](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.NET site).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Bir barındırma sağlayıcısı için sunucuları yapılandırma

- [Microsoft ASP.NET 4 barındırma Dağıtım Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=191365) (Microsoft İndirme Merkezi).
- [Profil XML dosyası oluşturmak](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.NET site).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Dağıtım sorunlarını giderme

- [Visual Studio'da Windows Azure Web Siteleri'nde sorun giderme](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure site).
- [Visual Studio kullanarak ASP.NET Web Dağıtımı: sorun giderme](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Web ile karşılaşılan sorun giderme dağıtma](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web dağıtımı hata kodları](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.NET site).
- [Web için Visual Studio ve ASP.NET dağıtım hakkında SSS](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Çekirdek IIS ile ASP.NET geliştirme sunucusu arasındaki farklılıklar](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Geliştirme ile üretim arasındaki yaygın yapılandırma farklılıkları](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Medium Trust ile de ASP.NET uygulamaları barındırma](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys Rolla siteden).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Belirli bir dağıtım soru ilgili Yardım alma

- [ASP.NET yapılandırma ve dağıtım forum](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Ek Kaynaklar

Bu bölümde, Visual Studio ve IIS dağıtım araçları kullanma hakkında daha fazla bilgi edinmek için yararlı olan ek kaynaklara bağlantılar sağlanmaktadır.

Aşağıdaki blogları sık Visual Studio web dağıtımı hakkında bilgi içerir:

- [Web geliştirme araçları Microsoft blog](https://blogs.msdn.com/b/webdevtools/).
- [Sayed Hashimi'nın blog](http://www.sedodream.com/).

Aşağıdaki kaynaklar, Web dağıtımı, web uygulama projesi dağıtım görevlerini gerçekleştirmek için Visual Studio kullanan IIS framework ile ilgili belgeler sağlar. Web dağıtımı içinde hakkında sorular sorabilirsiniz [Web dağıtım aracı Forumu](https://go.microsoft.com/fwlink/?LinkId=149411) IIS.NET web sitesinde.

- [Web giriş dağıtma](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Yükleme ve yapılandırma Web dağıtma](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Web otomatikleştirmek için PowerShell betiklerini dağıtmak Kurulum](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Web dağıtım aracı](https://go.microsoft.com/fwlink/?LinkId=151481). Web Dağıtımı belgelerini TechNet sitesindeki içeriği düğümünün en üst düzey tablo. Sayfaları yıl boyunca güncelleştirilmemiş TechNet birçok faydalı başvuru bilgileri içerir.
- [Microsoft.Web.Deployment Namespace](https://go.microsoft.com/fwlink/?LinkId=148630). API belgelerini değil güncelleştirildi 1.0 sürümünden itibaren.
- [Microsoft Web dağıtım ekibi blog](https://blogs.iis.net/msdeploy/default.aspx).
- [Sekme IIS.NET web sitesi yayımlama](https://www.iis.net/learn/publish).
