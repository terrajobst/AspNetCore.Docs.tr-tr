---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Web paketi dağıtımı için parametreleri yapılandırma | Microsoft Docs
author: jrjlee
description: Bu konuda, Internet Information Services (IIS) web uygulama adları, bağlantı dizeleri ve hizmet uç noktaları gibi parametre değerlerini nasıl ayarlanacağı açıklanır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386555"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Web paketi dağıtımı için parametreleri yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, uzak bir IIS web sunucusu için web paketi dağıtırken hizmet uç noktaları, Internet Information Services (IIS) web uygulama adları ve bağlantı dizeleri gibi parametre değerlerini ayarlama işlemi açıklanmaktadır.


Bir web uygulaması projesi, derleme ve paketleme işlemi oluşturduğunuzda üç anahtar dosyalarını oluşturur:

- A *[Proje adı] .zip* dosya. Web dağıtım paketi için web uygulaması projenize budur. Bu paket, tüm derlemeleri, dosyalar, veritabanı betikleri ve uzak bir IIS web sunucusunda web uygulamanızı yeniden oluşturmak için gereken kaynakları içerir.
- A *[Proje adı].deploy.cmd* dosya. Bu, uzak bir IIS web sunucusuna, web dağıtım paketi yayımlama, Web dağıtımı (MSDeploy.exe) parametreli komutlar kümesi içerir.
- A *[Proje adı]. SetParameters.xml* dosya. Bu parametre değerlerini MSDeploy.exe komut kümesi sağlar. Bu dosyadaki değerleri güncelleştirin ve web paketinizi dağıttığınızda Web dağıtımı için komut satırı parametresi geçirin.

> [!NOTE]
> Derleme ve paketleme işlemi hakkında daha fazla bilgi için bkz. [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).


*SetParameters.xml* dosyası web uygulaması proje dosyasına ve tüm yapılandırma dosyaları projenize içinde dinamik olarak oluşturulur. Ne zaman oluşturun ve projenizi Web yayımlama işlem hattı (WPP) paketini veritabanı bağlantı dizelerini ve IIS web uygulamasını hedef gibi dağıtım ortamları arasında değişmesi olasılığı olan değişkenlere çok sayıda otomatik olarak algılar. Bu değerleri otomatik olarak web dağıtım paketinin parametreli ve eklenen *SetParameters.xml* dosya. Örneğin, bir bağlantı dizesi eklerseniz *web.config* dosyası web uygulaması projenize yapı işlemi, bu değişikliği algılar ve giriş ekler *SetParameters.xml* dosyası Buna göre.

İçinde çok sayıda durumlarda, bu otomatik Parametreleştirme yeterli olacaktır. Kullanıcılarınızın diğer ayarları uygulama ayarları veya Hizmeti uç nokta URL'leri gibi dağıtım ortamları arasında değişen ihtiyaçları varsa ancak Usewpp_copywebapplication içinkarşılıkgelengirişlerekleyinvebudeğerleridağıtımpaketindekiParametreleştirmesöylemenizgerekir*SetParameters.xml* dosya. Aşağıdaki bölümlerde bunun nasıl yapılacağı açıklanmaktadır.

### <a name="automatic-parameterization"></a>Otomatik Parametreleştirme

Derleme ve bir web uygulaması paketi WPP otomatik olarak bunları Parametreleştirme:

- Hedef IIS web uygulaması yolu ve adı.
- Tüm bağlantı dizeleri de, *web.config* dosya.
- Eklediğiniz tüm veritabanları için bağlantı dizelerini **SQL Paketle/Yayımla** proje özelliği sayfalarından sekmesindedir.

Örneğin, derlemek ve paketlemek için oluşturduysanız [Kişi Yöneticisi](the-contact-manager-solution.md) Parametreleştirme işlemi hiçbir şekilde WPP dokunmadan örnek çözüm bu oluşturmak *ContactManager.Mvc.SetParameters.xml* dosyası:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


Bu durumda:

- **IIS Web uygulaması adı** istediğiniz web uygulamasına dağıtmak için IIS yol parametresidir. Varsayılan değer alınır **Paketle/Yayımla Web** projenin özellik sayfalarındaki sayfa.
- **ApplicationServices Web.config bağlantı dizesidir** parametre oluşturulan bir **connectionStrings ve ekleme** öğesinde *web.config* dosya. Bu uygulama, üyelik veritabanının başvurun için kullanması gereken bağlantı dizesini temsil eder. Burada sağladığınız değerin dağıtılan yerine *web.config* dosya. Varsayılan değer, dağıtım öncesi alınır *web.config* dosya.

WPP de bu ürettiği dağıtım paketi özelliklerinde parametreleştiren. Dağıtım paketini yüklediğinizde bu özellikler için değerleri sağlayabilirsiniz. ' % S'paketi el ile IIS Yöneticisi'ni bölümünde anlatılan şekilde yükleyin, [Web paketlerini el ile yükleme](manually-installing-web-packages.md), Yükleme Sihirbazı'nı tüm parametrelerin değerlerini sağlamasını ister. Uzaktan kullanarak paketi yüklerseniz *. deploy.cmd* açıklandığı gibi dosya [Web paketleri dağıtma](deploying-web-packages.md), Web dağıtımı görünür için *SetParameters.xml* dosya parametre değerlerini sağlayın. Değerleri düzenleyebilirsiniz *SetParameters.xml* el ile dosya veya bir otomatik derleme ve dağıtım işleminin bir parçası dosya özelleştirebilirsiniz. Bu işlem, bu konunun ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır.

### <a name="custom-parameterization"></a>Özel Parametreleştirme

Daha karmaşık dağıtım senaryolarında genellikle projenizi dağıtmadan önce ek özellikler Parametreleştirme isteyebilirsiniz. Genel olarak bakıldığında, özellikleri ve hedef ortamlar arasında farklılık gösterir ayarları parametreleştirin. Bunlar şunları içerebilir:

- Hizmet uç noktalarını *web.config* dosya.
- Uygulama ayarlarında *web.config* dosya.
- İstediğiniz herhangi bir bildirim temelli özellikler belirtmek için kullanıcılara sor.

Eklemek için bu özellikleri parametre haline getirmek için en kolay yolu olan bir *parameters.xml* web uygulaması projenizin kök klasörüne bir dosya. Örneğin, kişi yöneticisi çözümde ContactManager.Mvc proje içeren bir *parameters.xml* kök klasöründe bir dosya.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Bu dosyayı açmak, tek bir içerdiğini göreceksiniz **parametre** girişi. Giriş bulup ContactService Windows Communication Foundation (WCF) hizmetinin uç noktası URL'yi Parametreleştirme XML Path Language (XPath) sorgusu kullanır. *web.config* dosya.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Uç nokta URL'sini dağıtım paketinde kümesini parametreleştirme yanı sıra WPP de karşılık gelen bir giriş ekler *SetParameters.xml* yanı sıra dağıtım paketi oluşturulan dosya.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Dağıtım paketini el ile yüklerseniz, IIS Yöneticisi'ni otomatik olarak parametreli özellikleri birlikte hizmet uç noktası adresi ister. Dağıtım paketi yüklerseniz çalıştırarak *. deploy.cmd* dosyasını düzenleyebilirsiniz *SetParameters.xml* değerleri ile birlikte hizmet uç noktası adresi için bir değer sağlamak için dosya otomatik olarak parametreli özellikleri.

Nasıl oluşturulacağı hakkında tüm ayrıntılar için bir *parameters.xml* bkz [nasıl yapılır: yapılandırma dağıtım ayarları, bir paketi kullanın parametreleri yüklü](https://msdn.microsoft.com/library/ff398068.aspx). Adlı yordamı **dağıtım parametrelerini Web.config dosyası ayarlarını kullanmak için** adım adım yönergeler sağlar.

## <a name="modifying-the-setparametersxml-file"></a>SetParameters.xml dosyasını değiştirme

Web uygulaması paketi el ile dağıtmak plan&#x2014;çalıştırarak ya da *. deploy.cmd* dosya veya komut satırından MSDeploy.exe çalıştırılıyor&#x2014;şey, el ile düzenleme durdurmak için  *SetParameters.xml* dağıtımdan önce dosya. Ancak, bir kurumsal ölçekli çözüm üzerinde çalışıyorsanız daha büyük, otomatik derleme ve dağıtım işleminin bir parçası bir web uygulaması paketi dağıtma gerekebilir. Bu senaryoda, Microsoft Build Engine (MSBuild) değiştirileceğini ihtiyacınız *SetParameters.xml* dosyayı. MSBuild kullanarak bunu yapabilirsiniz **XmlPoke** görev.

[Kişi Yöneticisi örnek çözüm](the-contact-manager-solution.md) bu işlemi göstermektedir. Bu örnek için ilgili ayrıntıları göstermek için düzenlenmiş izleyen kod örnekleri.

> [!NOTE]
> Örnek çözüm, genel özel proje dosyalarında giriş proje dosyası modeli daha geniş bir genel bakış için bkz: [proje dosyasını anlama](understanding-the-project-file.md) ve [derleme işlemini anlama](understanding-the-build-process.md).


İlk olarak, ilgilenilen parametre değerlerini ortama özgü proje dosyasındaki özellikleri olarak tanımlanır (örneğin, *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Kendi server ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz. [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Ardından, *Publish.proj* dosyası bu özellikleri alır. Çünkü her *SetParameters.xml* dosyası ile ilişkili bir *. deploy.cmd* dosya ve sonuçta her çağırmak için proje dosyasını istediğiniz *. deploy.cmd* proje dosyası dosyası oluşturur bir MSBuild *öğesi* her *. deploy.cmd* ilgilendiğiniz özelliklerini tanımlar ve dosya *öğe meta verileri*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


Bu durumda:

- **ParametersXml** meta veri değeri konumunu belirtir *SetParameters.xml* dosya.
- **IisWebAppName** değeri istediğiniz web uygulamasına dağıtmak IIS yoludur.
- **MembershipDBConnectionString** değerdir üyelik veritabanının bağlantı dizesini ve **MembershipDBConnectionName** değer **adı** özniteliği karşılık gelen parametre, *SetParameters.xml* dosya.
- **ServiceEndpointValue** değerdir, hedef sunucuda, WCF hizmeti için uç nokta adresini ve **ServiceEndpointParamName** karşılık gelen parametre ad özniteliğinin değeridir *SetParameters.xml* dosya.

Son olarak *Publish.proj* dosyası **PublishWebPackages** kullandığı hedef **XmlPoke** bu değerleri değiştirmek için görev *SetParameters.xml* dosya.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Her fark edeceksiniz **XmlPoke** görevi, dört öznitelik değerlerini belirtir:

- **XmlInputPath** özniteliği, değiştirmek istediğiniz dosyayı bulmak nereye görev söyler.
- **Sorgu** özniteliği, değiştirmek istediğiniz XML düğümü tanımlayan bir XPath sorgusu.
- **Değer** seçili XML düğümü eklemek istediğiniz yeni değeri bir özniteliktir.
- **Koşul** özniteliktir ölçütleri üzerinde görevin çalıştırın veya çalışmıyor. Bu durumlarda, bir null veya boş değer olarak eklemeye çalışmayın koşul sağlar *SetParameters.xml* dosya.

## <a name="conclusion"></a>Sonuç

Bu konuda açıklanan rolünü *SetParameters.xml* dosyası ve bir web uygulaması projesi oluşturduğunuzda nasıl oluşturulduğu açıklanmaktadır. Ekleyerek ek ayarlar nasıl parametreleştirebilirsiniz açıklandığı bir *parameters.xml* projenize bir dosya. Ayrıca nasıl değiştirebileceğiniz açıklanan *SetParameters.xml* dosyası kullanarak ek olarak, daha büyük, otomatik derleme işleminin bir parçası olarak **XmlPoke** proje dosyalarınızı görevde.

Bir sonraki konu [Web paketleri dağıtma](deploying-web-packages.md), web paketi çalıştırarak ya da dağıtımı açıklayan *. deploy.cmd* dosya veya MSDeploy.exe kullanarak doğrudan komutları. Her iki durumda da belirtebilirsiniz, *SetParameters.xml* dağıtım parametresi olarak dosya.

## <a name="further-reading"></a>Daha Fazla Bilgi

Web paketleri oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md). Aslında bir web paketi dağıtma hakkında yönergeler için bkz. [Web paketleri dağıtma](deploying-web-packages.md). Oluşturma konusunda adım adım kılavuz bir *parameters.xml* bkz [nasıl yapılır: yapılandırma dağıtım ayarları, bir paketi kullanın parametreleri yüklü](https://msdn.microsoft.com/library/ff398068.aspx).

Parametreleştirme Web dağıtımı hakkında daha fazla genel bilgi için bkz. [Web dağıtma Parametreleştirme eylem](https://go.microsoft.com/?linkid=9805119) (blog gönderisi).

> [!div class="step-by-step"]
> [Önceki](building-and-packaging-web-application-projects.md)
> [İleri](deploying-web-packages.md)
