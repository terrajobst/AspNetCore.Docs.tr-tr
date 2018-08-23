---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: klasör izinlerini ayarlama - 12 6 | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 81d5107c7e25a10218d13175c47913c94e9ab3fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755856"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: klasör izinlerini ayarlama - 12 6
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, klasör izinlerini ayarlama *Elmah* klasöründe dağıtılan web sitesi uygulama günlük dosyalarını bu klasöre oluşturabilirsiniz.

Visual Studio geliştirme sunucusu (Cassini) kullanarak Visual Studio'da bir web uygulamasını test ettiğinizde, uygulama kimliğinizi altında çalışır. Geliştirme bilgisayarınızda büyük olasılıkla Yöneticiyseniz ve tam bir klasördeki herhangi bir dosyaya bir şey yetkisine sahip. Ancak, bir uygulama IIS altında çalışırken, atanan site uygulama havuzu için tanımlanan kimlik altında çalışır. Genellikle, izinleri sınırlı olan sistem tarafından tanımlanan bir hesap budur. Varsayılan olarak okuma ve Yürütme izinleri, web uygulamanızın dosyalar ve klasörler üzerinde ancak yazma erişimi yok.

Bu uygulamanızın oluşturduğu veya web uygulamalarında yaygın olan güncelleştirme dosyaları gereksiniminiz varsa bir sorun haline gelir. Contoso University uygulamada, XML dosyalarında Elmah oluşturur *Elmah* hatalarıyla ilgili ayrıntıları kaydetmek için klasör. Elmah benzer bir şey bile kullanmazsanız, sitenizi dosyaları karşıya yükleme veya sitenizdeki bir klasöre veri yazma diğer görevleri kullanıcı sağlayabilir.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Günlüğe kaydetme ve Raporlama hata testi

(Visual Studio'da Test zamankiyle rağmen) nasıl uygulama doğru IIS'de işe yaramazsa görmek için normalde Elmah tarafından günlüğe ve ayrıntıları görmek için Elmah hata günlüğünü açın hatasıdır neden olabilir. Elmah bir XML dosyası oluşturun ve hata ayrıntılarını depolamak boş hata raporu görürsünüz.

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ve ardından gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Bir sistem tarafından oluşturulan hata sayfası yerine gördüğünüz *GenericErrorPage.aspx* çünkü sayfa `customErrors` ayarı Web.config dosyasında "RemoteOnly" olduğunu ve IIS yerel olarak çalıştırıyorsanız:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Şimdi Çalıştır *Elmah.axd* hata raporu görmek için. Elmah bir XML dosyasını oluşturamadı, çünkü bir boş hata günlüğü sayfasında bakın *Elmah* klasörü:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Elmah klasörde yazma izni ayarı

Klasör izinlerini el ile ayarlayabilirsiniz veya dağıtım işleminin otomatik bir parçası olun. Otomatik hale getirme karmaşık MSBuild kod gerektirir ve yalnızca bu ilk kez yapmanız gereken bu yana dağıttığınızda, Bu öğretici yalnızca el ile yapmak nasıl gösterir. (Bu dağıtım işleminin bir parçası yapma hakkında daha fazla bilgi için bkz: [Web yayımlama üzerinde klasör izinlerini ayarlama](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi'nın blogunda.)

İçinde **Windows Explorer**, gitmek *C:\inetpub\wwwroot\ContosoUniversity*. Sağ *Elmah* klasörüne **özellikleri**ve ardından **güvenlik** sekmesi.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Görmüyorsanız **DefaultAppPool** içinde **grup veya kullanıcı adları** listesinde, büyük olasılıkla kullanılan başka bir yöntem Bu öğreticide belirtilenden bilgisayarınızda IIS ve ASP.NET 4 ayarlamak için. Bu durumda, hangi kimlik Contoso University uygulama ve bu kimlik için yazma izni atanmış uygulama havuzu tarafından kullanıldığını öğrenin. Uygulama havuzu kimlikleri hakkında bağlantılara Bu öğreticinin sonunda bakın.)


              **Düzenle**‘ye tıklayın. İçinde **Elmah izinlerini** iletişim kutusunda **DefaultAppPool**ve ardından **yazma** onay kutusuna **izin** sütun.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Tıklayın **Tamam** iki iletişim kutularında.

## <a name="retesting-error-logging-and-reporting"></a>Günlüğe kaydetme ve Raporlama hata çözülüp

Test hata tekrar aynı şekilde (istek hatalı URL) neden olarak ve çalıştırma **hata günlüğü** sayfası. Bu süre, hata sayfasında görünür.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Ayrıca üzerinde yazma izni *uygulama\_veri* klasör çünkü SQL Server Compact veritabanı dosyalarını bu klasöre sahip ve bu veritabanlarında verileri güncelleştirme yönetebilmek istiyorsunuz. Bu durumda, ancak, dağıtım işlemi üzerinde yazma izni otomatik olarak ayarlar. çünkü ekstra bir şey yapmanız gerekmez *uygulama\_veri* klasör.

Artık tüm Contoso University almak gerekli görevleri IIS'de yerel bilgisayarınızda düzgün çalışmasını tamamladınız. Sonraki öğreticide, site genel kullanıma açık bir barındırma sağlayıcısına dağıtarak hale getirir.

## <a name="more-information"></a>Daha fazla bilgi

Bu örnekte, neden Elmah günlük dosyalarını kaydedemedi nedeni çok açıktır. Sorunun nedenini göreceksiniz olduğu durumlarda IIS izleme kullanabilirsiniz; bkz: [sorun giderme başarısız istekleri kullanarak izleme IIS 7'de](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.NET sitesinde.

Uygulama havuzu kimlikleri için izinleri verme konusunda daha fazla bilgi için bkz. [uygulama havuzu kimlikleri](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) ve [güvenli içerik, dosya sistemi ACL'ler üzerinden IIS](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.NET sitesinde.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [İleri](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
