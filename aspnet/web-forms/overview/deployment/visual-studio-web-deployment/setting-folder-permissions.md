---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: "Visual Studio kullanarak ASP.NET Web Dağıtımı: klasör izinlerini ayarlama | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 19bef5ff97fd5b79135df8ca9bd6bd316594cc5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: klasör izinlerini ayarlama
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, klasör izinlerini ayarlama *Elmah* klasörü dağıtılan Web sitesi uygulama günlük dosyaları bu klasörde oluşturabilirsiniz.

Bir web uygulaması Visual Studio geliştirme Server (Cassini) veya IIS Express kullanarak Visual Studio'da test ettiğinizde, uygulama kimliği altında çalışır. Geliştirme bilgisayarınızda büyük olasılıkla Yöneticiyseniz ve herhangi bir klasörde herhangi bir dosyaya herhangi bir şey yapmanızı tam yetkisine sahip. Ancak, bir uygulama IIS altında çalıştığında, site atandığı uygulama havuzu için tanımlanan kimlik altında çalışır. Bu, genellikle sınırlı izinleri olan sistem tarafından tanımlanan bir hesap olur. Varsayılan olarak okuma ve Yürütme izinleri, web uygulamanızın dosyalar ve klasörler üzerinde ancak yazma erişimi yok.

Bu, uygulamanızın oluşturduğu veya web uygulamalarında yaygın olan güncelleştirme dosyaları ihtiyacınız varsa bir sorun haline gelir. Contoso University uygulamada Elmah XML dosyaları oluşturur *Elmah* hatalarla ilgili ayrıntıları kaydetmek için klasör. Elmah şöyle kullanmayan olsa bile, sitenizi dosyaları karşıya yükleme veya sitenizdeki bir klasöre veri yazma diğer görevleri kullanıcı sağlayabilir.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test hata günlüğünü ve Raporlama

(Visual Studio'da test edildiğinde olduğu rağmen) nasıl uygulama doğru IIS'de işe yaramazsa görmek için normalde Elmah tarafından günlüğe ve ayrıntıları görmek için Elmah hata günlüğü'nü açın bir hataya neden olabilir. Bir XML dosyası oluşturun ve hata ayrıntılarını depolamak Elmah işleyemezse, bir boş hata raporu bakın.

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ve ardından gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Sistem tarafından oluşturulan hata sayfası yerine bkz *GenericErrorPage.aspx* olduğundan sayfa `customErrors` ayarı Web.config dosyasında "RemoteOnly" ve IIS yerel olarak çalıştığını:

![HTTP 404 hata sayfası](setting-folder-permissions/_static/image1.png)

Şimdi Çalıştır *Elmah.axd* hata raporu görmek için. Yönetici hesabı kimlik bilgilerinizle oturum sonra (&quot;yönetici&quot; ve &quot;devpwd&quot;), Elmah bir XML dosyası oluşturamadı çünkü boş hata günlüğü sayfasını görmek *Elmah*klasörü:

![Hata günlüğü boş](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Elmah klasörü üzerinde yazma izni Ayarla

Dağıtım işlemi otomatik bir parçası yapmak veya klasör izinlerini el ile ayarlayabilirsiniz. Yalnızca bu dağıttığınız ilk kez yapmak zorunda olduğundan, el ile yapmak nasıl aşağıdaki adımlar ve otomatik hale getirme karmaşık MSBuild kodu gerektirir. (Bu dağıtım işleminin parçası yapma hakkında daha fazla bilgi için bkz [Web yayımlama ayarı klasör izinleri](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi'nın blogunda.)

1. İçinde **dosya Gezgini**, gitmek *C:\inetpub\wwwroot\ContosoUniversity*. Sağ *Elmah* klasöründe seçin **özellikleri**ve ardından **güvenlik** sekmesi.
2. 
              **Düzenle**‘ye tıklayın.
3. İçinde **Elmah izinlerini** iletişim kutusunda **DefaultAppPool**ve ardından **yazma** onay kutusuna **izin** sütun.

    ![ELMAH klasör izinlerini](setting-folder-permissions/_static/image3.png)

    (Görmüyorsanız **DefaultAppPool** içinde **grup veya kullanıcı adları** listesi, büyük olasılıkla başka bir yöntem Bu öğreticide belirtilenden IIS ve ASP.NET 4 bilgisayarınızda ayarlamak için kullandığınız. Bu durumda, hangi kimlik Contoso University uygulama ve bu kimliğe yazma izni atanmış uygulama havuzu tarafından kullanılan bulun. Uygulama havuzu kimlikleri ilgili bağlantıları bu öğreticinin sonunda bakın.) Tıklatın **Tamam** her iki iletişim kutularında.

## <a name="retest-error-logging-and-reporting"></a>Hata günlüğü ve raporlama yeniden sınayın

Test hata tekrar aynı şekilde (istek bozuk bir URL) neden olarak ve çalıştırma **hata günlüğü** sayfası. Bu süre hata sayfasında görüntülenir.

![ELMAH hata günlüğü sayfası](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Özet

Contoso University almak gerekli görevlerin tümünü, yerel bilgisayarınızda IIS'de düzgün çalışmasını artık tamamladınız. Sonraki öğreticide, sitenin genel kullanıma açık Azure'a dağıtarak hale getirir.

## <a name="more-information"></a>Daha fazla bilgi

Bu örnekte, neden Elmah günlük dosyalarını kaydedemedi nedeni çok açıktır. Sorunun nedenini göreceksiniz olduğu durumlarda IIS izleme kullanabilirsiniz; bkz: [sorun giderme başarısız istekleri kullanarak izleme IIS 7'de](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) IIS.NET sitesinde.

Uygulama havuzu kimlikleri izinleri hakkında daha fazla bilgi için bkz: [uygulama havuzu kimlikleri](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) ve [içerik dosya sistemi ACL'lerini aracılığıyla IIS güvenli](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.NET sitesinde.

>[!div class="step-by-step"]
[Önceki](deploying-to-iis.md)
[sonraki](deploying-to-production.md)
