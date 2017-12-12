---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: "Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtım | Microsoft Docs"
author: tdykstra
description: "Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 8446b3fc05e3ef4a5a30c753c989252fd7f1a56f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtım
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, Visual Studio web çağırmak nasıl gösterilir ardışık düzen komut satırından yayımlama. Bu, istediğiniz senaryolar için kullanışlıdır [dağıtım işlemini otomatikleştirmek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) el ile Visual Studio'da yapmakta yerine, genellikle kullanarak bir [kaynak kodu sürüm denetimi sistemi](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Dağıtmak için bir değişiklik yapın

Şu anda hakkında sayfası şablonu kodu görüntüler.

![Şablon kodu ile sayfası hakkında](command-line-deployment/_static/image1.png)

Öğrenci kayıt özetini görüntüler koduyla değiştirirsiniz.

Açık *About.aspx* sayfasında, tüm biçimlendirme içinde silmek `MainContent` `Content` öğesini ve onun yerine aşağıdaki biçimlendirmede Ekle:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Projeyi çalıştırmak ve seçmek **hakkında** sayfası.

![Sayfa hakkında](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Komut satırını kullanarak test için dağıtma

Başka bir veritabanı değişikliği, aspnet ContosoUniversity veritabanı için bunu devre dışı bırakma dbDacFx veritabanı dağıtımı dağıtma olmaz. Açık **Web'i Yayımla** sihirbazında ve her üç yayımlama profillerini, Temizle **güncelleştirme veritabanı** onay kutusunu **ayarları** sekmesi.

Windows 8 Başlat Sayfası arama **VS2012 için geliştirici komut istemi**.

Simgesine sağ tıklayın **VS2012 için geliştirici komut istemi** tıklatıp **yönetici olarak çalıştır**.

Çözüm dosyası yolu, çözüm dosya yolunu değiştirerek komut istemine aşağıdaki komutu girin:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild çözüm oluşturur ve test ortamı dağıtır.

![Komut satırı çıkışı](command-line-deployment/_static/image3.png)

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ardından **hakkında** dağıtımının başarılı olduğunu doğrulamak için sayfa.

Testinde herhangi Öğrenciler oluşturmadıysanız, altında boş bir sayfa görürsünüz **Öğrenci gövde istatistikleri** başlığı. Git **Öğrenciler** sayfasında, **eklemek Öğrenci**, bazı Öğrenciler ekleyin ve sonra geri dönüp **hakkında** Öğrenci istatistikleri görmek için sayfayı.

![Test ortamında sayfası hakkında](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Anahtar komut satırı seçenekleri

Çözüm dosyası yolu ve iki özellik için MSBuild geçirilen girdiğiniz komutu:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Tek tek projeler dağıtma karşı çözümü dağıtma

Çözüm dosyası belirtilmesi oluşturulacak Çözümdeki tüm projeleri neden olur. Çözümde birden çok web projeleri varsa, aşağıdaki MSBuild davranış geçerlidir:

- Komut satırında belirttiğiniz özellikleri her projeye geçirilir. Bu nedenle, her web projesi Belirttiğiniz ada sahip bir yayımlama profili olması gerekir. Belirtirseniz `/p:PublishProfile=Test`, her web projesi adlı bir yayımlama profili olmalıdır *Test*.
- Başka bir bile derlerken olmayan bir proje başarılı şekilde yayımlamak. Daha fazla bilgi için bkz: stackoverflow iş parçacığı [MSBuild başarısız iki paketlerle](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Bir çözüm yerine her bir proje belirtirseniz, Visual Studio sürümünü belirten bir parametre eklemeniz gerekir. Visual Studio 2012 kullanıyorsanız, komut satırında aşağıdaki örneğe benzer olacaktır:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 için sürüm numarasını 10.0 olur. Daha fazla bilgi için bkz: [Visual Studio Proje uyumluluk ve VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi'nın blogunda.

### <a name="specifying-the-publish-profile"></a>Yayımlama profili belirtme

Yayımlama profili adı veya tam yolunu belirtebilirsiniz *.pubxml* , aşağıdaki örnekte gösterildiği gibi dosya:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web yayımlama komut satırı yayımlama için desteklenen yöntemler

Üç yöntem yayımlamak için komut satırı yayımlama desteklenir:

- `MSDeploy`-Web dağıtımı kullanarak yayımlayın.
- `Package`-Web dağıtım paketi oluşturarak yayımlayın. Paket oluşturduğu MSBuild komutu ayrı olarak yüklemeniz gerekir.
- `FileSystem`-Dosyaları belirtilen klasöre kopyalayarak yayımlayın.

### <a name="specifying-the-build-configuration-and-platform"></a>Yapı yapılandırması ve platformu belirtme

Yapı yapılandırması ve platformu Visual Studio'da veya komut satırında ayarlamanız gerekir. Yayımlama profilleri adlandırıldığı özellikler dahil `LastUsedBuildConfiguration` ve `LastUsedPlatform`, ancak proje nasıl yapılandırıldığını belirlemek için bu özellikleri ayarlanamıyor. Daha fazla bilgi için bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi'nın blogunda.

## <a name="deploy-to-staging"></a>Hazırlama için dağıtma

Azure'a dağıtmak için komut satırına parola eklemeniz gerekir. Visual Studio'da yayımlama profilinde parola kaydettiyseniz, şifrelenmiş biçimde depolandıktan, *. pubxml.user* dosya. Bir komut satırı parametresi Parolada geçirmek zorunda biçimde bir komut satırı dağıtım yaparken bu dosyayı MSBuild tarafından erişilebilir değil.

1. Size gereken parolayı kopyalayın *.publishsettings* hazırlama web sitesi için daha önce indirdiğiniz dosya. Parola değeri `userPWD` özniteliği Web dağıtımı için `publishProfile` öğesi.

    ![Web dağıtımı parola](command-line-deployment/_static/image5.png)
2. Windows 8 Başlat Sayfası arama **VS2012 için geliştirici komut istemi**, komut istemini açmak için simgesini tıklatın. (Yerel bilgisayarda IIS dağıtma değil çünkü bu saat yönetici olarak açın gerekmez.)
3. Çözüm dosyası yolunu çözüm dosyanızı ve parolanızı parolayla yolunu değiştirerek komut istemine aşağıdaki komutu girin:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Bu komut satırı fazladan bir parametre içeren dikkat edin: `/p:AllowUntrustedCertificate=true`. Bu öğretici yazıldığı şekilde `AllowUntrustedCertificate` komut satırından Azure'da yayımlarken özelliği ayarlanmalıdır. Bu hata için düzeltme yayınlandığında, bu parametre gerekmez.
4. Bir tarayıcı açın ve hazırlama sitenizi URL'sine gidin ve ardından **hakkında** dağıtımının başarılı olduğunu doğrulamak için sayfa.

    Test ortamı için daha önce anlatıldığı gibi istatistikleri görmek için bazı Öğrenciler oluşturmanız gerekebilir **hakkında** sayfası.

## <a name="deploy-to-production"></a>Üretime dağıtma

Üretime dağıtma işlemi için hazırlama işlemine benzer.

1. Size gereken parolayı kopyalayın *.publishsettings* üretim web sitesi için daha önce indirdiğiniz dosya.
2. Açık **VS2012 için geliştirici komut istemi**.
3. Çözüm dosyası yolunu çözüm dosyanızı ve parolanızı parolayla yolunu değiştirerek komut istemine aşağıdaki komutu girin:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Gerçek üretim bir site için de bir veritabanı değişiklik olursa, genellikle kopyalamanız *uygulama\_offline.htm* dosya dağıtmadan önce siteye ve başarılı dağıtım sonrasında silin.
4. Bir tarayıcı açın ve hazırlama sitenizi URL'sine gidin ve ardından **hakkında** dağıtımının başarılı olduğunu doğrulamak için sayfa.

## <a name="summary"></a>Özet

Komut satırını kullanarak bir uygulama güncelleştirmesi şimdi dağıtmış.

![Test ortamında sayfası hakkında](command-line-deployment/_static/image6.png)

Sonraki öğreticide web genişletmek nasıl bir örnek görürsünüz yayımlama kanalı. Örnek projeye dahil edilmeyen dosyaları dağıtmak nasıl yapacağınızı gösterir.

>[!div class="step-by-step"]
[Önceki](deploying-a-database-update.md)
[sonraki](deploying-extra-files.md)
