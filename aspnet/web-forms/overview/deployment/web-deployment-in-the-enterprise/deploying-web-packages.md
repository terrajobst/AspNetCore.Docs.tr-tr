---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Web paketleri dağıtma | Microsoft Docs
author: jrjlee
description: Bu konu, uzak bir sunucuya Internet Information Services (IIS) Web Dağıtım Aracı (Web... kullanarak web dağıtımı paketleri nasıl yayımlayabilirsiniz açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 5d3af0fdcc6e7ae20194ba658e0cf72ad22c1234
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-web-packages"></a>Web paketleri dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, uzak bir sunucuya Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak web dağıtımı paketleri nasıl yayımlayabilirsiniz açıklar 2.0.
> 
> Uzak bir sunucuya web paketini dağıtabilmeniz için iki ana yolu vardır:
> 
> - Doğrudan MSDeploy.exe komut satırı yardımcı programını kullanabilirsiniz.
> - Çalıştırabilirsiniz *[Proje adı].deploy.cmd* dosya oluşturma işlemi oluşturur.
> 
> Sonuç hangi yaklaşımın bağımsız olarak kullandığınız aynıdır. Esas olarak, tüm *. deploy.cmd* dosyasına bazı önceden tanımlanmış değerlerle MSDeploy.exe çalıştırmak için böylelikle paketi dağıtmak için çok fazla bilgi sağlamanız gerekmez. Dağıtım işlemi basitleştirir. Diğer taraftan, MSDeploy.exe kullanarak doğrudan, çok daha fazla esneklik tam olarak nasıl paketinizi dağıtıldığını üzerinden sağlar.
> 
> Kullandığınız hangi yaklaşımın bağımlı olan çeşitli etkenlere dahil olmak üzere ne kadar denetim dağıtım işlemi gerektiren ve Web dağıtmak uzak Aracısı hizmeti veya Web dağıtımı işleyicisi hedefleme. Bu konu, her iki yaklaşımın kullanılmasını açıklar ve her bir yaklaşım uygun olduğunda tanımlar.
> 
> Görevleri ve bu konudaki yönergeler, varsayın:
> 
> - Yerleşik ve açıklandığı gibi web uygulamanızın paketlenmiş [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
> - Değişiklik *SetParameters.xml* açıklandığı gibi hedef ortamınız için doğru parametre değerlerini sağlamak için dosya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).


Çalıştıran [*proje adı*]*. deploy.cmd* web paketini dağıtmak için en basit yolu bir dosyadır. Özellikle, kullanarak *. deploy.cmd* dosyası MSDeploy.exe kullanarak doğrudan bu avantajları sunar:

- Web dağıtım paketi konumunu belirtmek gerekmeyen&#x2014; *. deploy.cmd* dosya zaten bilir olduğu.
- Konumunu belirtmeniz gerekmez *SetParameters.xml* dosya&#x2014; *. deploy.cmd* dosya zaten bilir olduğu.
- Kaynak ve hedef MSDeploy sağlayıcıları belirtmenize gerek yoktur&#x2014; *. deploy.cmd* dosyasını kullanmak için hangi değerlerin zaten bilir.
- MSDeploy işlemi ayarları belirtmenize gerek yoktur&#x2014; *. deploy.cmd* dosya sık gereken değerleri otomatik olarak MSDeploy.exe komutu ekler.

Kullanmadan önce *. deploy.cmd* web paketini dağıtmak için dosya emin olmanız:

- *. Deploy.cmd* dosyası, [*proje adı*]. *SetParameters.xml* dosya ve web paketini ([*proje adı*]. *zip*) aynı klasörde bulunur.
- Web dağıtımı (MSDeploy.exe) çalıştıran bilgisayarda yüklü *. deploy.cmd* dosya.

*. Deploy.cmd* dosyası çeşitli komut satırı seçeneklerini destekler. Bir komut isteminden dosyasını çalıştırdığınızda, bu temel sözdizimi şöyledir:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Ya da belirtmeniz gerekir bir **/T** bayrağı veya **/Y** deneme Çalıştır veya dinamik bir dağıtım sırasıyla gerçekleştirmek isteyip istemediğinizi belirtmek için bayrak (her iki bayrakları aynı komutta kullanmayın). Bu tabloda, bu bayrakların her amacı açıklanmaktadır.

| Bayrağı | Açıklama |
| --- | --- |
| **/T** | Msdeploy.exe'yi çağırır **– whatIf** deneme çalıştırması gösteren bayrak. Paketi dağıtmak yerine, paketi dağıtırsanız ne olacağını bir rapor oluşturur. |
| **/Y** | Olmadan Msdeploy.exe'yi çağırır **– whatIf** bayrağı. Bu paketin yerel bilgisayara veya belirtilen hedef sunucuya dağıtır. |
| **/M** | Hedef sunucuyu belirtir adı veya hizmet URL'si. Burada sağlayabilir değerleri hakkında daha fazla bilgi için bkz: **Endpoint konuları** bölümüne bakın. Atlarsanız **/M** bayrağı, paketi yerel bilgisayara dağıtılacak. |
| **/A** | MSDeploy.exe dağıtım gerçekleştirmek için kullanması gereken kimlik doğrulama türünü belirtir. Olası değerler şunlardır: **NTLM** ve **temel**. Atlarsanız **/A** bayrağı, kimlik doğrulama türü varsayılan olarak **NTLM** Web dağıtımı uzak aracı hizmetini ve sistemi dağıtımı için **temel** dağıtım için Web dağıtımı İşleyici. |
| **/U** | Kullanıcı adını belirtir. Bu, yalnızca temel kimlik doğrulaması kullanıyorsanız geçerlidir. |
| **/P** | Parolayı belirtir. Bu, yalnızca temel kimlik doğrulaması kullanıyorsanız geçerlidir. |
| **/L** | Paket yerel IIS Express örneğine dağıtılmalıdır gösterir. |
| **/G** | Paket kullanılarak dağıtılır belirtir [tempAgent sağlayıcı ayarı](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Atlarsanız **/G** bayrağı, değer varsayılan olarak **false**. |

> [!NOTE]
> Ayrıca her zaman bir web paketi oluşturma işlemi oluşturur, adında bir dosya oluşturur *[Proje adı] .deploy-readme.txt* bu dağıtım seçenekleri açıklanmaktadır.


Bu bayrakların ek olarak, Web Dağıtımı işlemi ayarları ek olarak belirtebilirsiniz *. deploy.cmd* parametreleri. Belirttiğiniz ek ayarları yalnızca temel alınan MSDeploy.exe komutu geçirilecek. Bu ayarlar hakkında daha fazla bilgi için bkz: [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Çalıştırarak ContactManager.Mvc web uygulama projesi için bir test ortamı dağıtmak istediğinizi varsayalım *. deploy.cmd* dosya. Test ortamınızı Web dağıtımı uzak aracı hizmetini kullanmak için açıklandığı şekilde yapılandırılmış [bir Web sunucusu Web dağıtımı yayımlama için (Uzak Aracı) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Web uygulamasını dağıtmak için sonraki adımları tamamlamanız gerekir.

**Bir web uygulaması kullanarak dağıtmak için. deploy.cmd dosyası**

1. Derleme ve web uygulama projesi açıklandığı gibi paket [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
2. Değiştirme *ContactManager.Mvc.SetParameters.xml* açıklandığı gibi sınama ortamınız için doğru parametre değerlerini içeren dosyaya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
3. Bir komut istemi penceresi açın ve dosyanın konumuna gidin *ContactManager.Mvc.deploy.cmd* dosya.
4. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Bu örnekte:

- **/Y** bayrağı gösterir gerçekte paketini dağıtmak istediğiniz yerine bir deneme yapılması çalıştırın.
- **/M** bayrağı, paketi TESTWEB1 adlı sunucuya dağıtmak istediğiniz gösterir. Bu değerden adresindeki Web dağıtımı uzak aracısının hizmet paketi dağıtmak MSDeploy.exe deneyecek http://TESTWEB1/MSDeployAgentService.
- **/A** bayrağı NTLM kimlik doğrulaması kullanmak istediğiniz gösterir. Bu nedenle, bir kullanıcı adı ve parola belirtmeniz gerekmez.

Göstermek için nasıl *. deploy.cmd* dosya dağıtım işlemini basitleştirir, ve oluşturulan çalıştırdığınızda yürütülen MSDeploy.exe komutu bakalım *ContactManager.Mvc.deploy.cmd* Yukarıda gösterilen seçenekleri kullanarak.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Kullanma hakkında daha fazla bilgi için *. deploy.cmd* web paketini dağıtmak için bkz: dosya [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>MSDeploy.exe kullanma

Kullanarak rağmen *. deploy.cmd* dosyası genellikle dağıtım işlemini basitleştirir, bazı durumlar vardır MSDeploy.exe doğrudan kullanılması tercih olduğunda. Örneğin:

- Web dağıtımı işleyicisi yönetici olmayan bir kullanıcı olarak dağıtmak istiyorsanız, kullanamazsınız *. deploy.cmd* dosya. Web dağıtımı 2.0, bir hata nedeniyle altında açıklandığı gibi budur **Endpoint konuları**.
- El ile farklı arasında geçiş yapmak istiyorsanız, *SetParameters.xml* dosyaları farklı konumlarda tercih ettiğiniz MSDeploy.exe doğrudan kullanmak.
- Birkaç MSDeploy.exe komut satırı bağımsız değişkenleri geçersiz kılmak istiyorsanız, MSDeploy.exe doğrudan kullanmayı tercih edebilirsiniz.

MSDeploy.exe kullandığınızda, üç temel bilgiler sağlamanız gerekir:

- A **– kaynak** verilerinizi burada geldiğini belirten parametresi.
- A **– taşınmaya** parametresi için verilerinizin nerede bulunacağını belirtir.
- A **– fiil** gösterir parametresi [işlemi](https://technet.microsoft.com/library/dd568989(WS.10).aspx) gerçekleştirmek istediğiniz.

MSDeploy.exe kullanır [Web dağıtımı sağlayıcıları](https://technet.microsoft.com/library/dd569040(WS.10).aspx) kaynak ve hedef verileri işlemek için. Web dağıtımı uygulamaları ve veri kaynakları ile çalışabilir aralığı temsil eden sağlayıcılarının çok içeren&#x2014;Örneğin, SQL Server veritabanları, IIS web sunucuları, sertifika, Genel Derleme Önbelleği (GAC) derlemeler için sağlayıcıları vardır çeşitli farklı yapılandırma dosyalarını ve diğer veri türleri çok sayıda. Her iki **– kaynak** parametre ve **– taşınmaya** parametresi biçiminde bir sağlayıcı belirtmelisiniz **– kaynak**: [*providerName*] [=*konumu*]. Bir IIS Web sitesine bir web paketi dağıtıyorsanız, bu değerleri kullanmanız gerekir:

- **– Kaynak** sağlayıcısıdır her zaman [paket](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Örneğin:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Taşınmaya** sağlayıcısıdır her zaman [otomatik](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Örneğin:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– Fiil** her zaman **eşitleme**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Ayrıca, çeşitli belirtmek diğer ihtiyacınız vardır [sağlayıcıya özgü ayarlar](https://technet.microsoft.com/library/dd569001(WS.10).aspx) ve genel [işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Örneğin, bir hazırlama ortamında ContactManager.Mvc web uygulamasına dağıtmak istediğinizi varsayalım. Dağıtım dağıtma Web işleyicisi hedeflediğini ve temel kimlik doğrulaması kullanmanız gerekir. Web uygulamasını dağıtmak için sonraki adımları tamamlamanız gerekir.

**MSDeploy.exe kullanan bir web uygulamasına dağıtmak için**

1. Derleme ve web uygulama projesi açıklandığı gibi paket [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
2. Değiştirme *ContactManager.Mvc.SetParameters.xml* açıklandığı gibi hazırlama ortamınız için doğru parametre değerlerini içeren dosyaya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
3. Bir komut istemi penceresi açın ve MSDeploy.exe konumuna göz atın. Bu genellikle Web dağıtımı V2\msdeploy.exe %PROGRAMFILES%\IIS\Microsoft olur.
4. Bu komutu yazın ve Enter tuşuna basın (satır sonları göz ardı):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Bu örnekte:

- **– Kaynak** parametresi belirtir **paket** sağlayıcı ve web paketinin konumu gösterir.
- **– Taşınmaya** parametresi belirtir **otomatik** sağlayıcısı. **ComputerName** ayar, hedef sunucuda Web dağıtımı işleyicisi hizmeti URL'sini sağlar. **Kimlik** ayarını gösterir, temel kimlik doğrulaması kullanmak istediğiniz ve bu nedenle sağlamanız gerekir bir **kullanıcıadı** ve **parola**. Son olarak, **includeAcls = "False"** ayarı, hedef sunucuya kaynak web uygulamanızda dosyaların erişim denetim listelerini (ACL'ler) kopyalamak istemediğiniz belirtir.
- **– Fiil: eşitleme** bağımsız değişkeni, hedef sunucuda kaynak İçerik çoğaltmak istediğiniz gösterir.
- **– DisableLink** bağımsız değişkenlerini belirtmek uygulama havuzları, sanal dizin yapılandırmasını ya da hedef sunucuda Güvenli Yuva Katmanı (SSL) sertifikalarını çoğaltmak istediğiniz yok. Daha fazla bilgi için bkz: [Web dağıtımı bağlantı uzantıları](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **– SetParamFile** parametresini konumunu sağlar *SetParameters.xml* dosya.
- **– AllowUntrusted** anahtarı gösterir Web dağıtımı bir güvenilen sertifika yetkilisi tarafından verilmemiş SSL sertifikalarını kabul etmelisiniz. Web dağıtımı işleyicisine dağıtıyorsanız ve kendinden imzalı bir sertifika hizmeti URL'si güvenli hale getirmek için kullandığınız varsa, bu anahtarı eklemeniz gerekir.

## <a name="automating-web-package-deployment"></a>Web paketi dağıtımı otomatikleştirme

Bir kuruluş senaryoları lot içinde daha büyük tek adımlı veya otomatik dağıtım parçası olarak, web paketleri dağıtmak istersiniz. Web paketleri çalıştırarak dağıtmak seçtiğinizden bağımsız olarak *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak, komutlarınızı Parametreleştirme ve bir Microsoft Build Engine (MSBuild) bir hedef çağırmanıza Proje dosyası.

Contact Manager örnek çözümü göz atın **PublishWebPackages** de hedeflemek *Publish.proj* dosya. Bu hedef her biri için bir kez çalışır *. deploy.cmd* adlı bir öğe listesi tarafından tanımlanan dosyaya **PublishPackages**. Hedef her biri için değişken değerleri tam bir dizi oluşturmak için özellikleri ve öğe meta verileri kullanır *. deploy.cmd* dosya ve kullandığı **Exec** komutu çalıştırmak için görev.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Proje dosyası modelinde örnek çözümü ve genel özel proje dosyalarında giriş daha geniş bir genel bakış için bkz: [proje dosyası anlama](understanding-the-project-file.md) ve [oluşturma işlemini anlama](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Uç nokta dikkat edilecek noktalar

Olup çalıştırarak web paketini dağıtmak bağımsız olarak *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak, bir bilgisayar adı veya dağıtımınız için hizmet uç noktası belirtmeniz gerekir.

Hedef web sunucusuna Web dağıtımı uzak aracı hizmetini kullanarak dağıtım için yapılandırılmışsa, hedef olarak hedef hizmeti URL'si belirtin.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternatif olarak, tek başına sunucu adı, hedef olarak belirtebilirsiniz ve Web dağıtımı Uzak Aracı hizmeti URL'si Infer.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Hedef web sunucusuna dağıtmak Web işleyicisi kullanarak dağıtım için yapılandırılmışsa, IIS Web Yönetim Hizmeti (WMSvc) uç noktası adresi, hedef olarak belirtmeniz gerekir. Varsayılan olarak, bu formun alır:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Herhangi birini kullanarak bu uç noktaları hedef *. deploy.cmd* dosya ya da doğrudan MSDeploy.exe. Ancak, açıklandığı gibi yönetici olmayan bir kullanıcı olarak dağıtma Web işleyicisi dağıtmak istiyorsanız, [bir Web sunucusu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırmak](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), hizmet uç noktası adresi için bir sorgu dizesi eklemeniz gerekir.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Bu durum, yönetici olmayan bir kullanıcı için IIS sunucu düzeyinde erişime sahip değil çünkü; çözemiyorsa yalnızca belirli bir IIS Web erişimi vardır. Zaman içinde Web yayımlama ardışık düzen (WPP), bir hata nedeniyle yazma, çalıştıramazsınız *. deploy.cmd* bir sorgu dizesi içeren bir uç nokta adresi kullanarak dosya. Bu senaryoda, MSDeploy.exe kullanarak doğrudan web paketinizi dağıtmanız gerekir.

> [!NOTE]
> Web dağıtımı Uzak Aracı hizmeti ve Web dağıtımı işleyicisi konusunda daha fazla bilgi için bkz: [Web dağıtımı sağ yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Bu uç noktalar ile dağıtmak için ortama özgü proje dosyalarınıza yapılandırma hakkında yönergeler için bkz [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Kimlik doğrulama konuları

Olup çalıştırarak web paketini dağıtmak bağımsız olarak *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak, bir kimlik doğrulama türünü belirtmeniz gerekir. Web dağıtımı olası iki değeri kabul eder: **NTLM** veya **temel**. Temel kimlik doğrulaması belirtirseniz, ayrıca bir kullanıcı adı ve parola sağlaması gerekir. Bir kimlik doğrulama türünü seçtiğinizde bilmeniz gereken çeşitli Etkenler vardır:

- Web dağıtımı uzak aracısının hizmete dağıtıyorsanız, NTLM kimlik doğrulaması kullanmanız gerekir. Uzak Aracı hizmetini temel kimlik doğrulama kimlik bilgileri kabul etmez.
- Web dağıtımı işleyicisine dağıtıyorsanız, NTLM veya temel kimlik doğrulaması kullanabilirsiniz. Temel kimlik doğrulaması varsayılan ayardır. Kullanıcı adları ve parolalar düz metin olarak aktarılan temel kimlik doğrulaması kullanır ancak Web dağıtımı işleyicisi her zaman SSL şifrelemesi kullanır gibi kimlik bilgileriniz korunur.
- Bir veritabanı web paketinizi içerir ve web sunucusu ve veritabanı sunucusu ayrı makinelerdir, nedeniyle NTLM kimlik doğrulaması kullanarak veritabanını dağıtamaz olmayacaktır [NTLM "çift atlamalı" sınırlaması](https://go.microsoft.com/?linkid=9805120). SQL Server kimlik bilgilerinin dağıtım bağlantı dizenizi kullanabilir veya Web dağıtımı için temel kimlik doğrulaması kimlik bilgilerini sağlamanız gerekir. Bu sorunu daha ayrıntılı olarak açıklanmıştır [üyelik veritabanına dağıtma kurumsal ortamlarda](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Sonuç

Bir web paketi çalıştırarak ya da dağıtımı bu konuda açıklanan *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak. Ne zaman her bir yaklaşım uygun olabilir ve nasıl Parametreleştirme ve daha büyük bir tek bir adımı veya otomatikleştirilmiş derleme işleminin bir parçası bir dağıtım komutu çalıştırmak açıklanan açıklanmıştır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Oluşturma ve bir web dağıtım paketi Parametreleştirme hakkında yönergeler için bkz [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) ve [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md). Derleme ve Team Foundation Server (TFS) örneğinden web paketlerini dağıtma hakkında yönergeler için bkz [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Özelleştirme ve dağıtım işlemini sorun giderme hakkında daha fazla bilgi için bkz: [hariç dosya ve klasörleri dağıtımından](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Önceki](configuring-parameters-for-web-package-deployment.md)
> [sonraki](deploying-database-projects.md)
