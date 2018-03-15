---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Dağıtım özellikleri için bir hedef ortamını yapılandırma | Microsoft Docs"
author: jrjlee
description: "Bu konuda, belirli bir hedef ortam için örnek Contact Manager çözümü dağıtmak için ortama özgü özelliklerini yapılandırmak açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Hedef ortam için dağıtım özellikleri yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda ortama özgü özelliklerinin belirli bir hedef ortam için örnek Contact Manager çözümü dağıtmak için nasıl yapılandırılacağı açıklanmaktadır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanan [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm & #x 2014; Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="process-overview"></a>İşlemine genel bakış

Derleme ve Contact Manager çözümü dağıtmak için kullanacağınız proje dosyası iki fiziksel dosyalarıyla ayrılır:

- Evrensel içeren bir derleme ayarları ve yönergeleri ( *Publish.proj* dosyası).
- Ortama özgü içeren bir derleme ayarları (*Env Dev.proj*, *Env Stage.proj*, vb.).

Derleme zamanında uygun ortama özgü proje dosyası Evrensel birleştirilir *Publish.proj* derleme yönergeleri eksiksiz bir kümesini oluşturmak için dosya. Belirli bir hedef ortamlara dağıtım oluşturarak veya kendi dağıtım senaryosu açıklanmaktadır ayarlarla ortama özgü proje dosyalarını özelleştirme yapılandırabilirsiniz.

Bu değerleri çok sayıda hedef ortamınızı nasıl yapılandırıldığını & #x 2014 tarafından belirlenir; özellikle, olup, hedef web sunucunuzun Web dağıtım aracı hizmetini (Uzak Aracı) veya Web dağıtımı işleyicisi kullanacak şekilde yapılandırılmış. Bu yaklaşımlar hakkında daha fazla bilgi ve kendi ortamınız için doğru yaklaşım seçme konusunda yönergeler için bkz: [Web dağıtımı sağ yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md).

[Kişi Yöneticisi Senaryo](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) iki ortama özgü proje dosyalarını gerektirir:

- Bir geliştirici test ortamı dağıtımına (*Env Dev.proj*). Geliştirici test ortamı açıklandığı gibi uzak aracı kullanarak uzak dağıtımları kabul edecek şekilde yapılandırılmış [senaryo: bir Test ortamı için Web dağıtımı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Bu dosyayı uzak aracı bağlantı dizeleri ve hizmet uç noktaları gibi konuma özgü ayarları yanı sıra uç noktası adresi sağlaması gerekir.
- Hazırlama ortamında dağıtımına (*Env Stage.proj*). Hazırlama ortamında açıklandığı gibi Web dağıtımı işleyicisi kullanarak uzak dağıtımları kabul edecek şekilde yapılandırılmış [senaryo: Web dağıtımı için bir hazırlama ortamını yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md). Bu dosya Web dağıtımı işleyicisi uç noktası adresi yanı sıra bağlantı dizeleri gibi konuma özgü ayarları sağlayın ve uç noktaları hizmet gerekiyor.

Ortama özgü proje dosyasında yapılandırma ayarlarını etkilemez dikkat çekmek üzere web içeriğini kendisini & #x 2014; paketi bunun yerine, bunlar paket nasıl dağıtıldığını ve ne zaman sağlanan parametre değerleri kontrol önemlidir Paket ayıklanır. Web paketinin üretim ortamına açıklandığı gibi el ile aldığınız [senaryo: bir üretim ortamında Web dağıtım için yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md) ve [Web paketlerini el ile yükleme](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), Hangi ayarların önemli değildir şekilde paketi oluşturulan olduğunda ortama özgü proje dosyasında kullanılır. Paketi içe aktardığınızda Internet Information Services (IIS) Yöneticisi için bağlantı dizeleri ve hizmet uç noktaları gibi herhangi bir parametreli değeri ister.

Kendi hedef ortam Contact Manager çözümü dağıtmak için bu dosyayı özelleştirmek veya bir şablon olarak kullanmak ve kendi dosyanızı oluşturun.

**Contact Manager çözüm ortama özgü dağıtım ayarlarını yapılandırmak için**

1. ContactManager WCF çözümü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresinde genişletin **Yayımla** klasörünü genişletin **EnvConfig** klasörü ve çift tıklatarak **Env Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Özellik değerleri değiştirin *Env Dev.proj* kendi test ortamınız için doğru değerlerle dosya.

    > [!NOTE]
    > Bu yordamdan sonraki tablo bu özelliklerin her biri hakkında daha fazla bilgi sağlar.
4. Çalışmanızı kaydedin ve kapatın *Env Dev.proj* dosya.

## <a name="choosing-the-right-deployment-properties"></a>Doğru dağıtım özellikleri seçme

Bu tablo her bir özellik örnek ortama özgü proje dosyasında amacını açıklayan *Env Dev.proj*ve temin etmelidir değerlerine bazı yönelik rehberlik sağlar.

| Özellik adı | Ayrıntılar |
| --- | --- |
| **MSDeployComputerName** hedef web sunucusu veya Hizmeti uç noktanın adı. | Hedef web sunucusuna uzak aracı hizmetine dağıtıyorsanız, hedef bilgisayar adı belirtin (örneğin, **TESTWEB1** veya **TESTWEB1.fabrikam.net**), veya uzaktan belirtebilirsiniz. aracı uç noktası (örneğin, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Dağıtım her durumda aynı şekilde çalışır. Web dağıtımı işleyicisine hedef web sunucusunda dağıtıyorsanız, hizmet uç noktası belirtin ve bir sorgu dizesi parametresi olarak IIS Web sitesinin adını dahil gerekir (örneğin, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`). |
| **MSDeployAuth** Web dağıtımı uzak bilgisayarda kimlik doğrulaması için kullanması gereken yöntemi. | Bu ayarlanmalı **NTLM** veya **temel**. Genellikle, kullanacağınız **NTLM** uzak aracının dağıtıyorsanız ve **temel** dağıtmak Web işleyicisi dağıtıyorsanız. Temel kimlik doğrulamasını kullanıyorsanız, ayrıca kullanıcı adı ve IIS Web Dağıtım Aracı (Web dağıtımı) dağıtım gerçekleştirmek için bürüneceği parola belirtmeniz gerekir. Bu örnekte, bu değerleri aracılığıyla sağlanan **MSDeployUsername** ve **MSDeployPassword** özellikleri. NTLM kimlik doğrulaması kullanırsanız, bu özellikleri atlayın ya da boş bırakın. |
| **MSDeployUsername** temel kimlik doğrulamasını kullanıyorsanız, Web dağıtımı bu hesabın uzak bilgisayarda kullanır. | Bu form zamanınızı *etki alanı*\*kullanıcıadı * (örneğin, **FABRIKAM\matt**). Temel kimlik doğrulaması belirtirseniz, bu değer yalnızca kullanılır. NTLM kimlik doğrulamasını kullanıyorsanız, özellik atlanabilir. Bir değer belirtilirse, göz ardı edilir. |
| **MSDeployPassword** temel kimlik doğrulamasını kullanıyorsanız, Web dağıtımı bu parolayı uzak bilgisayarda kullanır. | Bu, belirtilen kullanıcı hesabının parolası değil **MSDeployUsername** özelliği. Temel kimlik doğrulaması belirtirseniz, bu değer yalnızca kullanılır. NTLM kimlik doğrulamasını kullanıyorsanız, özellik atlanabilir. Bir değer belirtilirse, göz ardı edilir. |
| **ContactManagerIisPath** istediğiniz kişinin Yöneticisi MVC uygulamasını dağıtmak IIS yolu. | Formda IIS Yöneticisi'nde göründüğü gibi bu yolun olmalıdır [*IIS Web sitesi adı*] / [*web ** uygulama adı*]. IIS Web uygulamanızı dağıtmadan önce mevcut olması gerektiğini unutmayın. Örneğin, DemoSite adlı bir IIS Web sitesini oluşturduysanız, MVC uygulaması için IIS yolu DemoSite/ContactManager belirtebilirsiniz. |
| **ContactManagerServiceIisPath** istediğiniz kişinin Yöneticisi WCF Hizmeti dağıtmak IIS yolu. | Örneğin, WCF hizmeti olarak IIS yolunu belirtin DemoSite adlı bir IIS Web sitesini oluşturduysanız, **DemoSite/ContactManagerService**. |
| **ContactManagerTargetUrl** , WCF Hizmeti ulaşılabilir URL. | Bu form sürecek [*IIS Web sitesi kök URL'si*] / [*hizmet uygulaması adı*] / [*Hizmeti uç noktası*]. Örneğin, bir IIS Web sitesi bağlantı noktası 85 oluşturduysanız, formu URL götürecek `http://localhost:85/ContactManagerService/ContactService.svc`. MVC uygulama ve WCF hizmeti aynı sunucuya dağıtılan unutmayın. Sonuç olarak, bu URL her zaman sadece yüklü olduğu makinede erişilir. Bu nedenle, localhost IP adresi yerine makine adını veya bir ana bilgisayar üstbilgisi URL'de kullanmak en iyisidir. Makine adı veya bir ana bilgisayar üstbilgisi kullanıyorsanız [geri döngü onay](https://go.microsoft.com/?linkid=9805131) güvenlik özelliği IIS'de URL engelleme ve dönüş bir **HTTP 401.1 - yetkisiz** hata. |
| **CmDatabaseConnectionString** veritabanı sunucusu için bağlantı dizesi. | Bağlantı dizesi VSDBCMD veritabanı sunucusu ile iletişim kurmak ve veritabanı ve web sunucusu uygulama havuzu veritabanı sunucusu ile iletişim kurmak ve veritabanıyla etkileşim kurmak için kullanacağı kimlik bilgilerini oluşturmak için kullanacağı kimlik bilgileri belirler. Temelde iki seçeneğiniz burada vardır. Belirtebilirsiniz **Integrated Security = true**, tümleşik Windows kimlik doğrulaması kullanılır; bu durumda: **veri kaynağı TESTDB1; = Integrated Security = true** bu durumda, veritabanı kullanılarak oluşturulacak yürütülebilir VSDBCMD çalıştıran kullanıcının ve uygulama kimlik bilgilerini kullanarak web sunucusu makine hesabının kimliğini veritabanına erişecek. Alternatif olarak, kullanıcı adını ve parolasını bir SQL Server hesabı belirtebilirsiniz. Bu durumda, SQL Server kimlik bilgileri hem veritabanı oluşturmak için VSDBCMD ve veritabanıyla etkileşim için uygulama havuzu tarafından kullanılır: **veri kaynağı TESTDB1; = Kullanıcı Kimliği = ASqlUser; Parola Pa$ $w0rd =** tümleşik Windows kimlik doğrulaması kullanacağınız bu konudaki izlenecek varsayalım. |
| **CmTargetDatabase** veritabanı sunucusunda oluşturacaksınız veritabanı vermek istediğiniz adı. | Burada sağladığınız değer VSDBCMD komut için parametre olarak eklenir. Ayrıca, web sunucusundaki uygulama havuzu veritabanıyla etkileşim için kullanabileceğiniz tam bağlantı dizesi oluşturmak için kullanılır. |
  

Bu örnekler, belirli dağıtım senaryoları için bu özellikleri nasıl yapılandırabileceğiniz gösterir.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Örnek 1 & #x 2014; Uzak Aracı hizmeti için dağıtımı

Bu örnekte:

- Uzak Aracı hizmetine TESTWEB1 dağıtıyorsunuz.
- NTLM kimlik doğrulaması kullanmak için Web dağıtımı bilgilendirerek. Web dağıtımı Microsoft Build Engine (MSBuild) çağırmak için kullanılan kimlik bilgilerini kullanarak çalışacaktır.
- Tümleşik kimlik doğrulaması dağıtmak için kullandığınız **ContactManager** TESTDB1 veritabanına. Veritabanı MSBuild çağırmak için kullanılan kimlik bilgileri kullanılarak dağıtılır.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Örnek 2 & #x 2014; Web dağıtım işleyicisi Endpoint dağıtma

Bu örnekte:

- Web dağıtımı işleyicisi hizmet uç noktası STAGEWEB1 üzerinde dağıtıyorsunuz.
- Temel kimlik doğrulaması kullanmak için Web dağıtımı bilgilendirerek.
- Web dağıtımı uzak bilgisayarda FABRIKAM\stagingdeployer hesabı bürüneceği belirlediniz.
- SQL Server kimlik doğrulaması dağıtmak için kullandığınız **ContactManager** STAGEDB1 veritabanına.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Sonuç

Bu noktada, proje dosyalarınızı oluşturmak ve bir veya daha fazla hedef ortamlara Contact Manager çözümü dağıtmak için tam olarak yapılandırılır.

Bu proje dosyalarını tek adımlı, tekrarlanabilir dağıtım işleminin bir parçası kullanmak için yürütme gerekir *Publish.proj* MSBuild kullanarak dosya ve ortama özgü proje dosyası konumu bir parametre olarak geçirin. Çeşitli yollarla bunu yapabilirsiniz:

- MSBuild ve giriş özel proje dosyalarına genel bakış için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Özel proje dosyalarınıza yürüten bir MSBuild komut formüle hakkında daha fazla bilgi için bkz: [dağıtma Web paketleri](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- MSBuild komutlarınızı tek adımlı, tekrarlanabilir dağıtımlar için bir komut dosyası içine dahil hakkında daha fazla bilgi için bkz: [oluşturma ve bir dağıtım komut dosyası çalıştırma](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Takım yapısı özel Proje dosyalarınızı yürütün hakkında daha fazla bilgi için bkz: [bu destekleyen dağıtım yapı tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

>[!div class="step-by-step"]
[Önceki](creating-a-server-farm-with-the-web-farm-framework.md)
