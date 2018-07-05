---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtımı | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390880"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: komut satırı dağıtımı
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide Visual Studio web çağrılacak gösterilmektedir işlem hattı komut satırından yayımlama. Bu, istediğiniz senaryolar için kullanışlıdır [dağıtım işlemini otomatik hale getirmek](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) el ile Visual Studio'da yapmakta yerine, genellikle kullanarak bir [kaynak kod sürüm denetimi sisteminden](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Dağıtmak için bir değişiklik yapın

Şu anda hakkında sayfa şablon kodunu görüntüler.

![Şablon kod sayfasıyla hakkında](command-line-deployment/_static/image1.png)

Öğrenci kayıt özetini görüntüler koduyla değiştireceksiniz.

Açık *About.aspx* sayfasında, tüm biçimlendirme içinde silmek `MainContent` `Content` öğesi ve onun yerine aşağıdaki biçimlendirmede Ekle:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Projeyi çalıştırmak ve seçmek **hakkında** sayfası.

![Sayfa hakkında](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Test için komut satırı kullanarak dağıtma

Başka bir veritabanı değişikliği, bunu devre dışı bırakma dbDacFx veritabanı dağıtımı aspnet ContosoUniversity veritabanı dağıtılıyor olmaz. Açık **Web'i Yayımla** sihirbazında ve her üç yayımlama profilleri, NET **veritabanını Güncelleştir** onay kutusunu **ayarları** sekmesi.

Windows 8 Başlangıç sayfasındaki arama **VS2012 için geliştirici komut istemi**.

Simgesine sağ tıklayın **VS2012 için geliştirici komut istemi** tıklatıp **yönetici olarak çalıştır**.

Çözüm dosyasının yolu, çözüm dosyasının yolu ile değiştirerek komut isteminde aşağıdaki komutu girin:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild, çözüm derlenir ve test ortamı dağıtır.

![Komut satırı çıkışı](command-line-deployment/_static/image3.png)

Bir tarayıcı açın ve gidin `http://localhost/ContosoUniversity`, ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.

Tüm Öğrenciler testinde oluşturmadıysanız, altında boş bir sayfa görürsünüz **Öğrenci gövdesi istatistikleri** başlığı. Git **Öğrenciler** sayfasında **ekleme Öğrenci**, bazı Öğrenciler ekleyin ve ardından geri dönün **hakkında** Öğrenci istatistikleri görmek için sayfayı.

![Test ortamında sayfası hakkında](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Anahtar komut satırı seçenekleri

Çözüm dosyası yolu ve iki özellik MSBuild'e geçirilen girdiğiniz komut:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Tek tek projeleri dağıtma ve çözümü dağıtma

Çözüm dosyasını belirtme oluşturulacak Çözümdeki tüm projeleri neden olur. Çözümde birden çok web projeniz varsa, MSBuild aşağıdaki davranış geçerlidir:

- Her proje için komut satırında belirttiğiniz özellikleri geçirilir. Bu nedenle, her bir web projesi, belirttiğiniz ada sahip bir yayımlama profili olması gerekir. Belirtirseniz `/p:PublishProfile=Test`, her bir web projesi adlı bir yayımlama profili olmalıdır *Test*.
- Başka bir bile oluşturduğunuzda değil başarıyla bir proje yayımlayabilirsiniz. Daha fazla bilgi için bkz: stackoverflow iş parçacığı [MSBuild başarısız iki paketlerle](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Bir çözüm yerine tek bir projenin belirtirseniz, Visual Studio sürümünü belirten bir parametresi eklemeniz gerekir. Visual Studio 2012 kullanıyorsanız, komut satırında aşağıdaki örneğe benzer olacaktır:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Visual Studio 2010 sürüm 10.0 numarasıdır. Daha fazla bilgi için [Visual Studio Proje uyumluluğu ve VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi'nın blogunda.

### <a name="specifying-the-publish-profile"></a>Yayımlama profili belirtme

Yayımlama profili adı veya tam yolunu belirtebilirsiniz *.pubxml* aşağıdaki örnekte gösterildiği gibi dosya:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Web yayımlama komut satırı yayımlama için desteklenen yöntem

Üç yöntem yayımlamak için komut satırı yayınlama desteklenir:

- `MSDeploy` -Web dağıtımı kullanarak yayımlayın.
- `Package` -Bir Web dağıtım paketi oluşturarak yayımlayın. Paket oluşturduğu MSBuild komutu ayrı olarak yüklemeniz gerekir.
- `FileSystem` -Dosyaları belirtilen klasöre kopyalamak yayımlayın.

### <a name="specifying-the-build-configuration-and-platform"></a>Derleme yapılandırması ve platformu belirtme

Visual Studio'da veya komut satırında derleme yapılandırması ve platformu ayarlanmalıdır. Yayımlama profillerine adlandırılmış özellikleri içeren `LastUsedBuildConfiguration` ve `LastUsedPlatform`, ancak projenin nasıl oluşturulduğunu belirlemek için bu özellikleri ayarlanamıyor. Daha fazla bilgi için [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi'nın blogunda.

## <a name="deploy-to-staging"></a>Hazırlık ortamına dağıtma

Azure'a dağıtmak için komut satırına bir parola eklemeniz gerekir. Visual Studio yayımlama profilinde parolayı kaydettiyseniz, şifrelenmiş biçimde depolanmış, *. pubxml.user* dosya. Bir komut satırı parametresi parolayı geçirin zorunda bir komut satırı dağıtımı yaptığınızda bu dosyayı MSBuild tarafından erişilebilir değil.

1. Gelen ihtiyacınız parolasını kopyalayın *.publishsettings* hazırlama web sitesi için daha önce indirdiğiniz dosyayı. Parola değeri `userPWD` Web dağıtımı için öznitelik `publishProfile` öğesi.

    ![Web Deploy parolası](command-line-deployment/_static/image5.png)
2. Windows 8 Başlangıç sayfasındaki arama **VS2012 için geliştirici komut istemi**, komut istemi açmak için simgeye tıklayın. (Yerel bilgisayarda IIS'ye dağıtma değildir çünkü bu saat yönetici olarak açın gerekmez.)
3. Çözüm dosyasının yolu, çözüm dosyanızı ve parolanızı parolayla yoluyla değiştirerek komut isteminde aşağıdaki komutu girin:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Bu komut satırı fazladan bir parametre içerdiğine dikkat edin: `/p:AllowUntrustedCertificate=true`. Bu öğretici yazıldığı gibi `AllowUntrustedCertificate` komut satırından Azure'da yayımlarken özelliği ayarlanmalıdır. Bu hata için bir düzeltme kullanıma sunulduğunda, bu parametre gerekmez.
4. Bir tarayıcı açın ve hazırlama sitenizin URL'sine gidin ve ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.

    Test ortamı için daha önce bahsettiğim gibi istatistikleri görmek için bazı Öğrenciler oluşturmanız gerekebilir **hakkında** sayfası.

## <a name="deploy-to-production"></a>Üretime dağıtma

Üretim dağıtımı işlemi için hazırlama işlemine benzerdir.

1. Gelen ihtiyacınız parolasını kopyalayın *.publishsettings* üretim web sitesi için daha önce indirdiğiniz dosya.
2. Açık **VS2012 için geliştirici komut istemi**.
3. Çözüm dosyasının yolu, çözüm dosyanızı ve parolanızı parolayla yoluyla değiştirerek komut isteminde aşağıdaki komutu girin:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Gerçek üretim sitesi için aynı zamanda bir veritabanı değişiklik olduysa, genellikle kopyalar *uygulama\_offline.htm* dağıtmadan önce siteye dosya ve başarılı dağıtımdan sonra silin.
4. Bir tarayıcı açın ve hazırlama sitenizin URL'sine gidin ve ardından **hakkında** dağıtımın başarılı olduğunu doğrulamak için sayfa.

## <a name="summary"></a>Özet

Şimdi, komut satırını kullanarak bir uygulama güncelleştirmesi dağıttım.

![Test ortamında sayfası hakkında](command-line-deployment/_static/image6.png)

Sonraki öğreticide, web genişletmek nasıl bir örnek görürsünüz yayımlama kanalı. Örneğin, projede bulunmayan dosyaları dağıtma gösterilmektedir.

> [!div class="step-by-step"]
> [Önceki](deploying-a-database-update.md)
> [İleri](deploying-extra-files.md)
