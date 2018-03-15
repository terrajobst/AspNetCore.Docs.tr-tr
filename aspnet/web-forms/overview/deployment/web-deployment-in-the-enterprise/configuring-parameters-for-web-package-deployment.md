---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: "Parametreler Web paketi dağıtım için yapılandırma | Microsoft Docs"
author: jrjlee
description: "Bu konuda Internet Information Services (IIS) web uygulama adları, bağlantı dizeleri ve hizmet uç noktaları gibi parametre değerlerini nasıl ayarlanacağını açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 12a4ba8ad30df43e7192500ad4514dfa9679f899
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="configuring-parameters-for-web-package-deployment"></a>Parametreler Web paketi dağıtım için yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, uzak bir IIS web sunucusuna bir web paketini dağıtırken Internet Information Services (IIS) web uygulama adları, bağlantı dizeleri ve hizmet uç noktaları gibi parametre değerlerini ayarlamak açıklar.


Bir web uygulaması projesi, yapı ve Paketleme işlemini oluşturduğunuzda üç anahtar dosyalarını oluşturur:

- A *[Proje adı] .zip* dosyası. Web uygulama projesi için web dağıtım paketi budur. Bu paket, tüm derlemeler, dosyalar, veritabanı komut dosyalarında ve bir uzak IIS web sunucusunda web uygulamanızı yeniden oluşturmak için gereken kaynakları içerir.
- A *[Proje adı].deploy.cmd* dosya. Bu web dağıtım paketi için uzak bir IIS web sunucusuna yayımlama, Web dağıtımı (MSDeploy.exe) parametreli komutlar kümesi içerir.
- A *[Proje adı]. SetParameters.xml* dosya. Bu parametre değerleri MSDeploy.exe komut kümesi sağlar. Bu dosyadaki değerleri güncelleştirin ve web paketinizi dağıttığınızda, Web dağıtımı için komut satırı parametre olarak geçirin.

> [!NOTE]
> Derleme ve paket oluşturma işlemi hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).


*SetParameters.xml* dosyası, web uygulama projesi dosyanızı ve tüm yapılandırma dosyalarını, projenizin içindeki dinamik olarak oluşturulur. Ne zaman yapı ve Web yayımlama ardışık düzen (WPP) projenizi paket değişkenleri, hedef IIS web uygulaması gibi dağıtım ortamları ve herhangi bir veritabanı bağlantı dizesi arasında değişmesi olasılığı olan çok sayıda otomatik olarak algılar. Bu değerleri otomatik olarak web dağıtım paketi parametreli ve eklenen *SetParameters.xml* dosya. Örneğin, bir bağlantı dizesi eklerseniz *web.config* dosyası, web uygulaması projesi oluşturma işlemi, bu değişikliği algılar ve giriş ekleyeceksiniz *SetParameters.xml* dosya Buna göre.

İçinde çok sayıda durumlarda, bu otomatik parametrelemeyi yeterli olacaktır. Kullanıcılarınızın diğer ayarları uygulama ayarları veya Hizmeti uç nokta URL'leri gibi dağıtım ortamlar arasında farklılık gerekiyorsa ancak, bu değerleri dağıtım paketindeki Parametreleştirme ve karşılık gelen girdilere eklemekiçinWPPsöylemenizgerekir*SetParameters.xml* dosya. Aşağıdaki bölümlerde, bunu yapmak açıklanmaktadır.

### <a name="automatic-parameterization"></a>Otomatik Parametreleştirme

Derleme ve bir web uygulaması paketini WPP otomatik olarak bunlardan Parametreleştirme:

- Hedef IIS web uygulama yolu ve adı.
- Herhangi bir bağlantı dizeleri, *web.config* dosya.
- Eklediğiniz tüm veritabanları için bağlantı dizelerini **SQL Paketle/Yayımla** proje özellik sayfalarını sekmesindedir.

Örneğin, yapı ve paket olsaydınız [Contact Manager](the-contact-manager-solution.md) herhangi bir şekilde WPP parametrelemeyi işleminde temas olmadan örnek çözümü bu oluşturmak *ContactManager.Mvc.SetParameters.xml* dosyası:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


Bu durumda:

- **IIS Web uygulaması adı** parametresi, web uygulaması dağıtmak istediğiniz IIS yoludur. Varsayılan değer alınırlar **Paketle/Yayımla Web** proje özellik sayfalarını sayfasında.
- **ApplicationServices Web.config bağlantı dizesi** parametre oluşturulan bir **connectionStrings ve ekleme** öğesinde *web.config* dosya. Uygulama üyeliği veritabanı başvurun için kullanması gereken bağlantı dizesini temsil eder. Sağladığınız burada olacaktır Değer dağıtılan yerine *web.config* dosya. Varsayılan değer öncesi dağıtımından alınır *web.config* dosya.

WPP de bu ürettiği dağıtım paketi özelliklerinde parameterizes. Dağıtım paketini yüklediğinizde değerleri için bu özellikleri sağlar. Bölümünde açıklandığı gibi el ile IIS Yöneticisi aracılığıyla, paketi yüklerseniz [Web paketlerini el ile yükleme](manually-installing-web-packages.md), Yükleme Sihirbazı'nı tüm parametrelerin değerlerini sağlamasını ister. Kullanarak uzaktan paketi yüklerseniz *. deploy.cmd* açıklandığı gibi dosya [dağıtma Web paketleri](deploying-web-packages.md), Web dağıtımı görünür için *SetParameters.xml* dosya parametre değerlerini sağlayın. Değerleri düzenleyebilirsiniz *SetParameters.xml* el ile dosya veya dosya otomatik derleme ve dağıtım işleminin bir parçası özelleştirebilirsiniz. Bu işlem, bu konunun ilerleyen bölümlerinde daha ayrıntılı açıklanmıştır.

### <a name="custom-parameterization"></a>Özel Parametreleştirme

Daha karmaşık dağıtım senaryolarında genellikle projenizi dağıtmadan önce ek özellikler Parametreleştirme istersiniz. Genel olarak bakıldığında, tüm özellikler ve hedef ortamlar arasında değişir ayarları Parametreleştirme. Bunlar içerebilir:

- Hizmet uç noktalarını *web.config* dosya.
- Uygulama ayarları *web.config* dosya.
- İstediğiniz herhangi bir bildirim temelli özelliklerini belirtmek için kullanıcılara sor.

Eklemek için bu özellikleri Parametreleştirme yapmanın en kolay yolu olan bir *parameters.xml* web uygulaması projenizin kök klasöre dosya. Örneğin, kişinin Yöneticisi çözümde ContactManager.Mvc proje içerir bir *parameters.xml* dosyası kök klasöründe.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Bu dosya açarsanız, tek bir içerdiği görürsünüz **parametresi** girişi. Girişi bulun ve uç nokta URL'sini ContactService Windows Communication Foundation (WCF) hizmetinin Parametreleştirme için bir XML Path dili (XPath) sorgusu kullanır *web.config* dosya.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Uç nokta URL'sini dağıtım paketindeki kümesini parametreleştirme yanı sıra WPP de karşılık gelen bir giriş ekler *SetParameters.xml* dağıtım paketi oluşturulan dosyası.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Dağıtım paketini el ile yüklerseniz, IIS Yöneticisi'ni otomatik olarak parametreli özellikleri yanında hizmet uç noktası adresi için ister. Çalıştırarak dağıtım paketi yüklerseniz *. deploy.cmd* dosyasını düzenleyebilirsiniz *SetParameters.xml* dosya değerlerini birlikte hizmet uç noktası adresi için bir değer girin otomatik olarak parametreli özellikler.

Nasıl oluşturulacağı hakkında ayrıntılar için bir *parameters.xml* dosya için bkz: [nasıl yapılır: kullanım parametreleri yapılandırmak dağıtım ayarları, bir paketin yüklü](https://msdn.microsoft.com/library/ff398068.aspx). Adlı yordamı **Web.config dosyası ayarları için dağıtım parametrelerini kullanmak için** adım adım yönergeler sağlar.

## <a name="modifying-the-setparametersxml-file"></a>SetParameters.xml dosyasını değiştirme

Varsa web uygulama paketini el ile dağıtmak için plan & #x 2014; çalıştırarak ya da *. deploy.cmd* dosya veya komut satırı & #x 2014; MSDeploy.exe çalıştırarak bir şey yok, el ile düzenlemedurdurmakiçin*SetParameters.xml* dağıtımından önce dosya. Ancak, bir kurumsal ölçekte çözüm üzerinde çalışıyorsanız, daha büyük ve otomatik derleme ve dağıtım işleminin bir parçası bir web uygulaması paketi dağıtmak gerekebilir. Bu senaryoda, Microsoft Build Engine (MSBuild) değiştirmek için gereksinim duyduğunuz *SetParameters.xml* dosyayı. MSBuild kullanarak bunu yapabilirsiniz **XmlPoke** görev.

[Contact Manager örnek çözümü](the-contact-manager-solution.md) bu işlemi gösterilmektedir. Aşağıdaki kod örnekleri, bu örnek için ilgili ayrıntıları göstermek için düzenlendi.

> [!NOTE]
> Proje dosyası modelinde örnek çözümü ve genel özel proje dosyalarında giriş daha geniş bir genel bakış için bkz: [proje dosyası anlama](understanding-the-project-file.md) ve [oluşturma işlemini anlama](understanding-the-build-process.md).


İlk olarak, parametre değerlerini gösteren öğelerdir ortama özgü proje dosyası özellikleri olarak tanımlanır (örneğin, *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Kendi sunucu ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Ardından, *Publish.proj* dosyası bu özellikleri alır. Çünkü her *SetParameters.xml* dosya ile ilişkili bir *. deploy.cmd* dosya ve biz sonuçta her çağırmak için proje dosyası istediğiniz *. deploy.cmd* proje dosyası bir dosya oluşturur bir MSBuild *öğesi* her *. deploy.cmd* dosya ve ilgi özelliklerini tanımlar *öğe meta verisi*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


Bu durumda:

- **ParametersXml** meta veri değeri konumunu belirten *SetParameters.xml* dosya.
- **IisWebAppName** web uygulamasına dağıtmak istediğiniz IIS yolu bir değerdir.
- **MembershipDBConnectionString** üyelik veritabanı için bağlantı dizesi bir değerdir ve **MembershipDBConnectionName** değer **adı** özniteliği karşılık gelen parametrenin *SetParameters.xml* dosya.
- **ServiceEndpointValue** hedef sunucuda WCF Hizmeti uç noktası adresi bir değerdir ve **ServiceEndpointParamName** değerdir ilgili parametrenin adı özniteliği *SetParameters.xml* dosya.

Son olarak, içinde *Publish.proj* dosyası **PublishWebPackages** hedef kullanır **XmlPoke** bu değerleri değiştirmek için görev *SetParameters.xml* dosya.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Her fark edeceksiniz **XmlPoke** görevi, dört öznitelik değerlerini belirtir:

- **XmlInputPath** özniteliği değiştirmek istediğiniz dosya nerede bulacağını görev söyler.
- **Sorgu** değiştirmek istediğiniz XML düğümü tanımlayan bir XPath sorgusu bir özniteliktir.
- **Değeri** seçili XML düğümü eklemek istediğiniz yeni değer bir özniteliktir.
- **Koşulu** üzerinde görev çalıştırın ya da çalışmıyor ölçüt bir özniteliktir. Bu durumlarda, bir null veya boş değer olarak eklemek denemeyin koşul sağlar *SetParameters.xml* dosya.

## <a name="conclusion"></a>Sonuç

Rolü, bu konuda açıklanan *SetParameters.xml* dosyası ve bir web uygulaması projesi oluşturduğunuzda nasıl oluşturulduğu açıklanmaktadır. Ekleyerek ek ayarları nasıl Parametreleştirme açıklandığı bir *parameters.xml* projenize dosya. De nasıl değiştirileceği açıklanmaktadır *SetParameters.xml* dosyası kullanarak ek olarak, daha büyük ve otomatik derleme işleminin bir parçası olarak **XmlPoke** proje dosyalarınıza görev.

Sonraki konuyu [dağıtma Web paketleri](deploying-web-packages.md), nasıl bir web paketi çalıştırarak ya da dağıtımı açıklanmakta *. deploy.cmd* MSDeploy.exe kullanarak doğrudan komutları veya dosya. Her iki durumda da, belirttiğiniz, *SetParameters.xml* dosyası dağıtım parametresi olarak.

## <a name="further-reading"></a>Daha Fazla Bilgi

Web paketleri oluşturma hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md). Gerçekte bir web paketi dağıtma hakkında yönergeler için bkz: [dağıtma Web paketleri](deploying-web-packages.md). Nasıl oluşturulacağı hakkında adım adım kılavuz bir *parameters.xml* dosya için bkz: [nasıl yapılır: kullanım parametreleri yapılandırmak dağıtım ayarları, bir paketin yüklü](https://msdn.microsoft.com/library/ff398068.aspx).

Web dağıtımı içinde parametrelemeyi hakkında daha fazla genel bilgi için bkz: [Web dağıtımı parametrelemeyi eylem](https://go.microsoft.com/?linkid=9805119) (blog yayını).

>[!div class="step-by-step"]
[Önceki](building-and-packaging-web-application-projects.md)
[sonraki](deploying-web-packages.md)
