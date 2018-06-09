---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: klasör izinlerini ayarlama - 12 6 | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 573e75221a1c0018bded7544e584b0c75f47d607
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30887271"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: klasör izinlerini ayarlama - 12 6
====================
Tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, klasör izinlerini ayarlama *Elmah* klasörü dağıtılan Web sitesi uygulama günlük dosyaları bu klasörde oluşturabilirsiniz.

Visual Studio geliştirme Server (Cassini) kullanarak Visual Studio'da bir web uygulaması test ettiğinizde, uygulama kimliği altında çalışır. Geliştirme bilgisayarınızda büyük olasılıkla Yöneticiyseniz ve herhangi bir klasörde herhangi bir dosyaya herhangi bir şey yapmanızı tam yetkisine sahip. Ancak, bir uygulama IIS altında çalıştığında, site atandığı uygulama havuzu için tanımlanan kimlik altında çalışır. Bu, genellikle sınırlı izinleri olan sistem tarafından tanımlanan bir hesap olur. Varsayılan olarak okuma ve Yürütme izinleri, web uygulamanızın dosyalar ve klasörler üzerinde ancak yazma erişimi yok.

Bu, uygulamanızın oluşturduğu veya web uygulamalarında yaygın olan güncelleştirme dosyaları ihtiyacınız varsa bir sorun haline gelir. Contoso University uygulamada Elmah XML dosyaları oluşturur *Elmah* hatalarla ilgili ayrıntıları kaydetmek için klasör. Elmah şöyle kullanmayan olsa bile, sitenizi dosyaları karşıya yükleme veya sitenizdeki bir klasöre veri yazma diğer görevleri kullanıcı sağlayabilir.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Günlüğe kaydetme ve rapor hata test etme

(Visual Studio'da test edildiğinde olduğu rağmen) nasıl uygulama doğru IIS'de işe yaramazsa görmek için normalde Elmah tarafından günlüğe ve ayrıntıları görmek için Elmah hata günlüğü'nü açın bir hataya neden olabilir. Bir XML dosyası oluşturun ve hata ayrıntılarını depolamak Elmah işleyemezse, bir boş hata raporu bakın.

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ve ardından gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Sistem tarafından oluşturulan hata sayfası yerine bkz *GenericErrorPage.aspx* olduğundan sayfa `customErrors` ayarı Web.config dosyasında "RemoteOnly" ve IIS yerel olarak çalıştığını:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Şimdi Çalıştır *Elmah.axd* hata raporu görmek için. Elmah bir XML dosyası oluşturamadı çünkü boş hata günlüğü sayfasını görmek *Elmah* klasörü:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Elmah klasörü üzerinde yazma izni ayarı

Dağıtım işlemi otomatik bir parçası yapmak veya klasör izinlerini el ile ayarlayabilirsiniz. Otomatik hale getirme karmaşık MSBuild kodu gerektirir ve yalnızca bu ilk kez yapmak zorunda olduğundan, dağıtma, Bu öğretici yalnızca el ile yapmak nasıl gösterir. (Bu dağıtım işleminin parçası yapma hakkında daha fazla bilgi için bkz [Web yayımlama ayarı klasör izinleri](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi'nın blogunda.)

İçinde **Windows Explorer**, gitmek *C:\inetpub\wwwroot\ContosoUniversity*. Sağ *Elmah* klasöründe seçin **özellikleri**ve ardından **güvenlik** sekmesi.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Görmüyorsanız **DefaultAppPool** içinde **grup veya kullanıcı adları** listesi, büyük olasılıkla başka bir yöntem Bu öğreticide belirtilenden IIS ve ASP.NET 4 bilgisayarınızda ayarlamak için kullandığınız. Bu durumda, hangi kimlik Contoso University uygulama ve bu kimliğe yazma izni atanmış uygulama havuzu tarafından kullanılan bulun. Uygulama havuzu kimlikleri ilgili bağlantıları bu öğreticinin sonunda bakın.)


              **Düzenle**‘ye tıklayın. İçinde **Elmah izinlerini** iletişim kutusunda **DefaultAppPool**ve ardından **yazma** onay kutusuna **izin** sütun.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Tıklatın **Tamam** her iki iletişim kutularında.

## <a name="retesting-error-logging-and-reporting"></a>Günlüğe kaydetme ve rapor hata retesting

Test hata tekrar aynı şekilde (istek bozuk bir URL) neden olarak ve çalıştırma **hata günlüğü** sayfası. Bu süre hata sayfasında görüntülenir.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Ayrıca üzerinde yazma izni *uygulama\_veri* klasörü çünkü SQL Server Compact veritabanı dosyaları bu klasörde var ve bu veritabanlarındaki verileri güncelleştirmek kullanabilmek ister. Bu durumda, ancak, dağıtım işlemi otomatik olarak yazma izni ayarlar için fazladan bir şey yapmanız gerekmez *uygulama\_veri* klasör.

Contoso University almak gerekli görevlerin tümünü, yerel bilgisayarınızda IIS'de düzgün çalışmasını artık tamamladınız. Sonraki öğreticide, sitenin genel kullanıma açık bir barındırma sağlayıcısına dağıtarak hale getirir.

## <a name="more-information"></a>Daha fazla bilgi

Bu örnekte, neden Elmah günlük dosyalarını kaydedemedi nedeni çok açıktır. Sorunun nedenini göreceksiniz olduğu durumlarda IIS izleme kullanabilirsiniz; bkz: [sorun giderme başarısız istekleri kullanarak izleme IIS 7'de](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.NET sitesinde.

Uygulama havuzu kimlikleri izinleri hakkında daha fazla bilgi için bkz: [uygulama havuzu kimlikleri](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) ve [içerik dosya sistemi ACL'lerini aracılığıyla IIS güvenli](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.NET sitesinde.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
