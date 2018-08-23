---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: klasör izinlerini ayarlama | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 930f46c0ddb0b77525098291393e526107a542d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754918"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: klasör izinlerini ayarlama
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, klasör izinlerini ayarlama *Elmah* klasöründe dağıtılan web sitesi uygulama günlük dosyalarını bu klasöre oluşturabilirsiniz.

Visual Studio geliştirme sunucusu (Cassini) veya IIS Express kullanarak Visual Studio'da bir web uygulamasını test ettiğinizde, uygulama kimliğinizi altında çalışır. Geliştirme bilgisayarınızda büyük olasılıkla Yöneticiyseniz ve tam bir klasördeki herhangi bir dosyaya bir şey yetkisine sahip. Ancak, bir uygulama IIS altında çalışırken, atanan site uygulama havuzu için tanımlanan kimlik altında çalışır. Genellikle, izinleri sınırlı olan sistem tarafından tanımlanan bir hesap budur. Varsayılan olarak okuma ve Yürütme izinleri, web uygulamanızın dosyalar ve klasörler üzerinde ancak yazma erişimi yok.

Bu uygulamanızın oluşturduğu veya web uygulamalarında yaygın olan güncelleştirme dosyaları gereksiniminiz varsa bir sorun haline gelir. Contoso University uygulamada, XML dosyalarında Elmah oluşturur *Elmah* hatalarıyla ilgili ayrıntıları kaydetmek için klasör. Elmah benzer bir şey bile kullanmazsanız, sitenizi dosyaları karşıya yükleme veya sitenizdeki bir klasöre veri yazma diğer görevleri kullanıcı sağlayabilir.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test hata günlüğünü ve Raporlama

(Visual Studio'da Test zamankiyle rağmen) nasıl uygulama doğru IIS'de işe yaramazsa görmek için normalde Elmah tarafından günlüğe ve ayrıntıları görmek için Elmah hata günlüğünü açın hatasıdır neden olabilir. Elmah bir XML dosyası oluşturun ve hata ayrıntılarını depolamak boş hata raporu görürsünüz.

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ve ardından gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Bir sistem tarafından oluşturulan hata sayfası yerine gördüğünüz *GenericErrorPage.aspx* çünkü sayfa `customErrors` ayarı Web.config dosyasında "RemoteOnly" olduğunu ve IIS yerel olarak çalıştırıyorsanız:

![HTTP 404 hata sayfası](setting-folder-permissions/_static/image1.png)

Şimdi Çalıştır *Elmah.axd* hata raporu görmek için. Yönetici hesabı kimlik bilgilerinizle oturum açtıktan sonra (&quot;yönetici&quot; ve &quot;devpwd&quot;), Elmah bir XML dosyasını oluşturamadı, çünkü bir boş hata günlüğü sayfasında bakın *Elmah*klasörü:

![Hata günlüğü boş](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Elmah klasörde yazma izni ayarlayın

Klasör izinlerini el ile ayarlayabilirsiniz veya dağıtım işleminin otomatik bir parçası olun. Otomatik hale getirme karmaşık MSBuild kod gerektirir ve yalnızca bu dağıtım ilk kez yapmak zorunda olduğundan, nasıl el ile yapmak aşağıdaki adımları. (Bu dağıtım işleminin bir parçası yapma hakkında daha fazla bilgi için bkz: [Web yayımlama üzerinde klasör izinlerini ayarlama](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi'nın blogunda.)

1. İçinde **dosya Gezgini**, gitmek *C:\inetpub\wwwroot\ContosoUniversity*. Sağ *Elmah* klasörüne **özellikleri**ve ardından **güvenlik** sekmesi.
2. 
              **Düzenle**‘ye tıklayın.
3. İçinde **Elmah izinlerini** iletişim kutusunda **DefaultAppPool**ve ardından **yazma** onay kutusuna **izin** sütun.

    ![ELMAH klasörün izinlerini](setting-folder-permissions/_static/image3.png)

    (Görmüyorsanız **DefaultAppPool** içinde **grup veya kullanıcı adları** listesinde, büyük olasılıkla kullanılan başka bir yöntem Bu öğreticide belirtilenden bilgisayarınızda IIS ve ASP.NET 4 ayarlamak için. Bu durumda, hangi kimlik Contoso University uygulama ve bu kimlik için yazma izni atanmış uygulama havuzu tarafından kullanıldığını öğrenin. Uygulama havuzu kimlikleri hakkında bağlantılara Bu öğreticinin sonunda bakın.) Tıklayın **Tamam** iki iletişim kutularında.

## <a name="retest-error-logging-and-reporting"></a>Hata günlüğü ve raporlama yeniden test et

Test hata tekrar aynı şekilde (istek hatalı URL) neden olarak ve çalıştırma **hata günlüğü** sayfası. Bu süre, hata sayfasında görünür.

![ELMAH hata günlüğü sayfası](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Özet

Artık tüm Contoso University almak gerekli görevleri IIS'de yerel bilgisayarınızda düzgün çalışmasını tamamladınız. Sonraki öğreticide, sitenin genel kullanıma açık Azure'a dağıtarak hale getirir.

## <a name="more-information"></a>Daha fazla bilgi

Bu örnekte, neden Elmah günlük dosyalarını kaydedemedi nedeni çok açıktır. Sorunun nedenini göreceksiniz olduğu durumlarda IIS izleme kullanabilirsiniz; bkz: [sorun giderme başarısız istekleri kullanarak izleme IIS 7'de](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.NET sitesinde.

Uygulama havuzu kimlikleri için izinleri verme konusunda daha fazla bilgi için bkz. [uygulama havuzu kimlikleri](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) ve [güvenli içerik, dosya sistemi ACL'ler üzerinden IIS](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.NET sitesinde.

> [!div class="step-by-step"]
> [Önceki](deploying-to-iis.md)
> [İleri](deploying-to-production.md)
