---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: Hedef ortam için dağıtım özelliklerini yapılandırma | Microsoft Docs
author: jrjlee
description: Bu konu, belirli bir hedef ortam için örnek Kişi Yöneticisi çözümü dağıtmak için ortama özgü özelliklerini yapılandırmayı açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: 1486cc7d8f25e823481dfab109d18c407c2d8063
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756211"
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Hedef ortam için dağıtım özelliklerini yapılandırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, belirli bir hedef ortam için örnek Kişi Yöneticisi çözümü dağıtmak için ortama özgü özelliklerini yapılandırmak açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) çözüm&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="process-overview"></a>İşleme genel bakış

Kişi Yöneticisi çözümü oluşturup dağıtmaya için kullanacağınız proje dosyası, iki fiziksel dosyalarına bölünür:

- Evrensel içeren bir derleme ayarları ve yönergeleri ( *Publish.proj* dosyası).
- Ortama özgü içeren bir derleme ayarları (*Env Dev.proj*, *Env Stage.proj*, vb.).

Derleme sırasında uygun ortama özgü proje dosyası Evrensel birleştirilir *Publish.proj* derleme yönergeleri eksiksiz bir kümesini oluşturmak için dosya. Belirli hedef ortamlara dağıtım oluşturarak veya kendi dağıtım senaryosu açıklayan ayarları ortama özgü proje dosyalarını özelleştirme yapılandırabilirsiniz.

Bu değerleri çok sayıda hedef ortamınızı nasıl yapılandırıldığına göre belirlenir&#x2014;özellikle olup, hedef web sunucusuna Web dağıtım aracı hizmetini (Uzak Aracı) veya Web dağıtımı işleyicisi kullanacak şekilde yapılandırıldı. Bu yaklaşımlar hakkında daha fazla bilgi ve ortamınızı doğru yaklaşımı seçme konusunda yönergeler için bkz. [Web dağıtımı için doğru yaklaşımı seçme](choosing-the-right-approach-to-web-deployment.md).

[Kişi yöneticisi senaryosu](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) iki ortama özgü proje dosyalarını gerektirir:

- Bir geliştirici test ortamına dağıtım (*Env Dev.proj*). Geliştirici test ortamı açıklandığı gibi uzak Aracı'yı kullanarak uzak dağıtımları kabul edecek şekilde yapılandırılmış [senaryo: Web dağıtımı için bir Test Ortamı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md). Bu dosyayı uzak aracı bağlantı dizelerini ve hizmet uç noktaları gibi konuma özgü ayarları yanı sıra uç nokta adresi sağlamanız gerekir.
- Hazırlık ortamına dağıtım (*Env Stage.proj*). Web dağıtımı işleyicisi kullanarak uzak dağıtımları açıklandığı kabul etmek için hazırlık ortamı yapılandırılmış [senaryo: Web dağıtımı için bir hazırlık ortamı yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md). Bu dosya Web dağıtımı işleyicisi uç nokta adresi yanı sıra bağlantı dizeleri gibi konuma özgü ayarları sağlayın ve hizmet uç noktaları gerekir.

Ortama özgü proje dosyasında yapılandırdığınız ayarları web paketinin içeriği etkilemeyen dikkat edin önemlidir&#x2014;bunun yerine, bunlar paket nasıl dağıtıldığını ve paket olduğunda hangi parametre değerlerini sağlanan denetleme Ayıklanan. Web paketi içinde açıklandığı şekilde el ile üretim ortamına aldığınız [senaryo: Web dağıtımı için bir üretim ortamı yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md) ve [Web paketlerini el ile yükleme](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), hangi ayarları farketmez şekilde paket oluşturduğunda ortama özgü proje dosyasında kullanılır. Paketi içeri aktarırken Internet Information Services (IIS) Yöneticisi için bağlantı dizelerini ve hizmet uç noktaları, gibi herhangi bir parametreli değeri ister.

Kişi Yöneticisi çözümü kendi hedef ortama dağıtmak için bu dosyayı özelleştirmek veya şablon olarak kullanmak ve kendi dosyanızı oluşturun.

**Kişi Yöneticisi çözümünü ortama özel dağıtım ayarlarını yapılandırmak için**

1. Visual Studio 2010'da ContactManager WCF çözümü açın.
2. İçinde **Çözüm Gezgini** penceresini genişletin **Yayımla** klasörünü genişletin **EnvConfig** klasörüne gittikten sonra çift tıklayarak **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Özellik değerleri değiştirin *Env Dev.proj* kendi test ortamı için doğru değerlerle dosya.

    > [!NOTE]
    > Bu yordam aşağıdaki tablo bu özelliklerden her biri hakkında daha fazla bilgi sağlar.
4. Çalışmanızı kaydedin ve kapatın *Env Dev.proj* dosya.

## <a name="choosing-the-right-deployment-properties"></a>Doğru dağıtım özellikleri seçme

Bu tabloda örnek ortama özgü proje dosyasındaki her bir özellik amacını açıklayan *Env Dev.proj*, sağlamanız değerlerin rehberlik sağlar.


|                                                        Özellik adı                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        Ayrıntılar                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              <strong>MSDeployComputerName</strong> hedef web sunucusu veya Hizmeti uç noktası adı.               |                                                                                                                                                                                                                                              Hedef web sunucusuna uzak aracı hizmetine dağıtıyorsanız, hedef bilgisayar adı belirtin (örneğin, <strong>TESTWEB1</strong> veya <strong>TESTWEB1.fabrikam.net</strong>), veya uzak belirtebilirsiniz. aracı uç noktası (örneğin, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). Dağıtımın her durumda aynı şekilde çalışır. Hedef web sunucusunda Web dağıtımı işleyicisi için dağıtıyorsanız, hizmet uç noktası belirtin ve bir sorgu dizesi parametresi olarak bir IIS Web sitesinin adını içerir (örneğin, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`).                                                                                                                                                                                                                                              |
|         <strong>MSDeployAuth</strong> Web dağıtımı uzak bilgisayarın kimliğini doğrulamak için kullanması gereken yöntemini.          |                                                                                                                                                                                                                          Bu ayarlanmalıdır <strong>NTLM</strong> veya <strong>temel</strong>. Genellikle, kullanacağınız <strong>NTLM</strong> Uzak Aracı hizmeti için dağıtıyorsanız ve <strong>temel</strong> için Web dağıtımı işleyicisi dağıtıyorsanız. Temel kimlik doğrulaması kullanıyorsanız, ayrıca IIS Web Dağıtım Aracı (Web dağıtımı) dağıtım gerçekleştirmek için bürüneceği parola ve kullanıcı adını belirtmeniz gerekir. Bu örnekte, bu değerleri aracılığıyla sağlanan <strong>MSDeployUsername</strong> ve <strong>MSDeployPassword</strong> özellikleri. NTLM kimlik doğrulaması kullanıyorsanız, bu özellikleri atlamak ya da boş bırakabilirsiniz.                                                                                                                                                                                                                          |
| <strong>MSDeployUsername</strong> temel kimlik doğrulaması kullanıyorsanız, Web dağıtımı bu hesabı uzak bilgisayar kullanır.  |                                                                                                                                                                                                                                                                                                                                                                                                                       Bu biçimini almalıdır <em>etki alanı</em>\*username * (örneğin, <strong>FABRIKAM\matt</strong>). Temel kimlik doğrulaması belirtirseniz, bu değer yalnızca kullanılır. NTLM kimlik doğrulaması kullanıyorsanız, özelliği atlanmış olabilir. Bir değer sağlanmazsa, göz ardı edilir.                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <strong>MSDeployPassword</strong> temel kimlik doğrulaması kullanıyorsanız, Web dağıtımı bu parolayı uzak bilgisayar kullanır. |                                                                                                                                                                                                                                                                                                                                                                                                                    Bu, belirttiğiniz kullanıcı hesabının parolasını, <strong>MSDeployUsername</strong> özelliği. Temel kimlik doğrulaması belirtirseniz, bu değer yalnızca kullanılır. NTLM kimlik doğrulaması kullanıyorsanız, özelliği atlanmış olabilir. Bir değer sağlanmazsa, göz ardı edilir.                                                                                                                                                                                                                                                                                                                                                                                                                    |
|     <strong>ContactManagerIisPath</strong> istediğiniz kişi yöneticisi MVC uygulaması dağıtmak IIS yolu.     |                                                                                                                                                                                                                                                                                                                                                                        Formda IIS Yöneticisi'nde göründüğü gibi bu yolun olmalıdır [<em>IIS Web sitesi adı</em>] / [<em>web</em><em>uygulama adı</em>]. IIS Web sitesi Uygulamanızı dağıtmadan önce mevcut olması gerektiğini unutmayın. Örneğin, DemoSite adlı bir IIS Web sitesi oluşturulduktan sonra MVC uygulaması için IIS yolunu DemoSite/ContactManager belirtebilirsiniz.                                                                                                                                                                                                                                                                                                                                                                        |
|   <strong>ContactManagerServiceIisPath</strong> istediğiniz kişi yöneticisi WCF Hizmeti dağıtmak IIS yolu.    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          Örneğin, WCF hizmeti olarak IIS yolunu belirtin DemoSite adlı bir IIS Web oluşturduysanız <strong>DemoSite/ContactManagerService</strong>.                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                  <strong>ContactManagerTargetUrl</strong> URL'si, WCF Hizmeti ulaşılabilir.                   |                                                                                                                                                     Bu form sürecek [<em>IIS Web sitesi kök URL'si</em>] / [<em>hizmet uygulaması adı</em>] / [<em>hizmet uç noktası</em>]. Örneğin, bir IIS Web sitesi bağlantı noktası 85 oluşturduysanız, form URL götürecek `http://localhost:85/ContactManagerService/ContactService.svc`. MVC uygulama ve WCF hizmeti aynı sunucuya dağıtılmasını unutmayın. Sonuç olarak, bu URL, yüklü olduğu makinede her zaman sadece erişilir. Bu nedenle, bu URL'de localhost IP adresi yerine makine adını veya bir konak üstbilgisi kullanmak en iyisidir. Makine adı veya bir konak üstbilgisi kullanırsanız [geri döngü denetimi](https://go.microsoft.com/?linkid=9805131) güvenlik özelliği IIS'de URL engelleyin ve dönüş bir <strong>HTTP 401.1 - yetkisiz</strong> hata.                                                                                                                                                     |
|                  <strong>CmDatabaseConnectionString</strong> veritabanı sunucusu için bağlantı dizesi.                  | Bağlantı dizesi VSDBCMD veritabanı oluşturup web server uygulama havuzu, veritabanı sunucusu ile iletişim kurmak ve veritabanıyla etkileşim kurmak için kullanacağı kimlik bilgilerini ve veritabanı sunucusu ile iletişim kurmak için kullanacağı kimlik bilgileri belirler. Temelde iki seçeneğiniz burada vardır. Belirtebilirsiniz <strong>Integrated Security = true</strong>, tümleşik Windows kimlik doğrulaması bu durumda kullanılır: <strong>veri kaynağı TESTDB1; = Integrated Security = true</strong> bu durumda, veritabanı kullanılarak oluşturulur yürütülebilir VSDBCMD çalıştıran kullanıcının masaüstündeki ve uygulamanın kimlik bilgileri, web sunucusu makine hesabının kimliğini kullanarak veritabanına erişir. Alternatif olarak, kullanıcı adını ve bir SQL Server hesabın parolasını belirtebilirsiniz. Bu durumda, SQL Server kimlik bilgileri VSDBCMD veritabanını oluşturmak için hem veritabanı ile etkileşim kurmak için uygulama havuzu tarafından kullanılır: <strong>veri kaynağı TESTDB1; = Kullanıcı Kimliği ASqlUser; = Parola Pa$ $w0rd =</strong> bu konudaki yönergeler tümleşik Windows kimlik doğrulaması kullanacağınız varsayılır. |
|        <strong>CmTargetDatabase</strong> veritabanı sunucusunda oluşturacaksınız veritabanı vermek istediğiniz adı.        |                                                                                                                                                                                                                                                                                                                                                                                                                                                     Burada sağladığınız değerin VSDBCMD komuta bir parametre olarak eklenir. Ayrıca, web sunucusundaki uygulama havuzu veritabanıyla etkileşim kurmak için kullanabileceğiniz tam bağlantı dizesi oluşturmak için kullanılır.                                                                                                                                                                                                                                                                                                                                                                                                                                                     |

Bu örnekler, belirli dağıtım senaryoları için bu özellikleri nasıl yapılandırabileceğiniz gösterir.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Örnek 1&#x2014;Uzak Aracı hizmeti dağıtımı

Bu örnekte:

- Uzak Aracı hizmetine TESTWEB1 dağıtıyorsunuz.
- NTLM kimlik doğrulaması kullanmak için Web dağıtımı söyleyen. Web dağıtımı, Microsoft Build Engine (MSBuild) çağırmak için kullanılan kimlik bilgileri kullanılarak çalıştırılır.
- Tümleşik kimlik doğrulaması dağıtmak için kullandığınız **ContactManager** TESTDB1 veritabanına. MSBuild'ı çağırmak için kullanılan kimlik bilgilerini kullanarak veritabanı dağıtılır.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Örnek 2&#x2014;Web dağıtımı işleyicisi uç nokta dağıtma

Bu örnekte:

- Web dağıtımı işleyicisi Hizmeti uç noktaya STAGEWEB1 dağıtıyorsunuz.
- Temel kimlik doğrulaması kullanmak için Web dağıtımı söyleyen.
- Web dağıtımı FABRIKAM\stagingdeployer hesabın uzak bilgisayarda bürüneceği belirlediniz.
- SQL Server kimlik doğrulaması dağıtmak için kullandığınız **ContactManager** STAGEDB1 veritabanına.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Sonuç

Bu noktada, proje dosyalarını oluşturmak ve bir veya daha fazla hedef ortamlara Kişi Yöneticisi çözümünü dağıtmak için tam olarak yapılandırılır.

Bu proje dosyalarının tek adımlı ve tekrarlanabilir dağıtım işleminin bir parçası kullanmak için yürütmek gereken *Publish.proj* MSBuild kullanarak dosya ve ortama özgü proje dosyasının konumunu bir parametre olarak geçirirsiniz. Bunu çeşitli şekillerde yapabilirsiniz:

- MSBuild ve giriş özel proje dosyalarına genel bakış için bkz: [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Özel proje dosyalarınızı yürüten bir MSBuild komut düzenleme hakkında daha fazla bilgi için bkz: [Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- MSBuild komutlarınızı tek adımlı, yinelenebilir dağıtımlar için bir komut dosyasına eklemek hakkında daha fazla bilgi için bkz: [oluşturma ve dağıtım komut dosyası çalıştırma](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Ekip özel Proje dosyalarınızı çalıştırmak hakkında daha fazla bilgi için bkz: [bu destekleyen dağıtım derleme tanımı oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

> [!div class="step-by-step"]
> [Önceki](creating-a-server-farm-with-the-web-farm-framework.md)
