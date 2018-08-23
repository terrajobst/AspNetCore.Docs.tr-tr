---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Web paketleri dağıtma | Microsoft Docs
author: jrjlee
description: Bu konu, uzak bir sunucuya Internet Information Services (IIS) Web Dağıtım Aracı'nı (Web... kullanarak web dağıtımı paketleri nasıl yayımlayabilirsiniz açıklar.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754648"
---
<a name="deploying-web-packages"></a>Web paketleri dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, uzak bir sunucuya (Web dağıtımı) Internet Information Services (IIS) Web Dağıtım Aracı'nı kullanarak web dağıtımı paketleri nasıl yayımlayabilirsiniz açıklar 2.0.
> 
> Uzak bir sunucuya web paketini dağıtabilmeniz için iki ana yolu vardır:
> 
> - MSDeploy.exe komut satırı yardımcı programını doğrudan kullanabilirsiniz.
> - Çalıştırabileceğiniz *[Proje adı].deploy.cmd* derleme işlemi oluşturan dosya.
> 
> Sonuç kullandığınız hangi yaklaşımın bağımsız olarak aynıdır. Esas olarak, tüm *. deploy.cmd* dosyasına MSDeploy.exe bazı önceden tanımlanmış değerlerle çalıştırmak için böylelikle paketi dağıtmak için kadar bilgi sağlamanız gerekmez. Bu dağıtım işlemini kolaylaştırır. Öte yandan, doğrudan MSDeploy.exe kullanarak, çok daha fazla esneklik tam olarak nasıl paketinizi dağıtıldığını üzerinden sağlar.
> 
> Hangi yaklaşımın kullandığınız bağımlı olan çeşitli Etkenler, üzerinde ne kadar denetim dahil olmak üzere dağıtım işlemini gerektirir ve Web dağıtımı Uzak Aracı hizmeti veya Web dağıtımı işleyicisi hedefleme. Bu konu, her bir yaklaşıma nasıl kullanıldığını açıklar ve her bir yaklaşıma uygun olduğunda tanımlar.
> 
> Görevler ve bu konudaki yönergeler, varsayın:
> 
> - Yerleşik ve açıklandığı gibi web uygulamanızın paketlenmiş [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
> - Değiştirdiğiniz *SetParameters.xml* açıklandığı gibi hedef ortamınız için doğru parametre değerlerini sağlamak için dosya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).


Çalıştırma [*proje adı*]*. deploy.cmd* dosyasıdır web paketini dağıtmak için en basit yolu. Özellikle, kullanarak *. deploy.cmd* dosyası kullanarak doğrudan MSDeploy.exe şu avantajları sunar:

- Web dağıtım paketinin konumu belirtmeniz gerekmez&#x2014; *. deploy.cmd* dosya aşina olduğu.
- Konumunu belirtmeniz gerekmez *SetParameters.xml* dosya&#x2014; *. deploy.cmd* dosya aşina olduğu.
- Kaynak ve hedef MSDeploy sağlayıcıları belirtmeniz gerekmez&#x2014; *. deploy.cmd* dosyasını kullanmak için hangi değerlerin zaten bilir.
- MSDeploy işlemi ayarlarını belirtmeniz gerekmez&#x2014; *. deploy.cmd* dosyası genellikle gerekli değerleri otomatik olarak MSDeploy.exe komutu ekler.

Kullanmadan önce *. deploy.cmd* bir web paketini dağıtmak için dosya emin olmanız:

- *. Deploy.cmd* dosyası [*proje adı*]. *SetParameters.xml* dosya ve web paketi ([*proje adı*]. *zip*) aynı klasörde bulunur.
- Web dağıtımı (MSDeploy.exe) çalıştıran bir bilgisayarda yüklü *. deploy.cmd* dosya.

*. Deploy.cmd* dosya çeşitli komut satırı seçeneklerini destekler. Bir komut istemi'nden dosyasını çalıştırdığınızda, bu temel sözdizimi aşağıdaki gibidir:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Belirtmeli bir **/T** bayrağı veya **/Y** bayrağı, bir deneme çalıştırmasındaki veya canlı bir dağıtım sırasıyla gerçekleştirmek isteyip istemediğinizi belirtmek için (her iki bayrakları aynı komutta kullanmayın). Bu tabloda, bu bayrakların her birinin amacı açıklanmaktadır.

| Bayrağı | Açıklama |
| --- | --- |
| **/T** | Msdeploy.exe'yi çağırır **– whatIf** bir deneme çalıştırmasındaki gösteren bayrak. Paket dağıtımı yerine paket dağıtırsanız ne olacağını bir rapor oluşturur. |
| **/Y** | Olmadan Msdeploy.exe'yi çağırır **– whatIf** bayrağı. Bu paket yerel bilgisayarın veya belirtilen hedef sunucuya dağıtır. |
| **/M** | Hedef sunucuyu belirtir ya da hizmet URL'si. Burada sağlayabilir değerleri hakkında daha fazla bilgi için bkz. **uç nokta konuları** bölümüne bakın. Atlarsanız **/M** bayrağı, paketin yerel bilgisayara dağıtılacak. |
| **/A** | MSDeploy.exe dağıtım gerçekleştirmek için kullanması gereken kimlik doğrulama türünü belirtir. Olası değerler **NTLM** ve **temel**. Atlarsanız **/A** bayrağı, kimlik doğrulama türü varsayılan olarak **NTLM** dağıtımı Web dağıtma uzak aracı hizmetine ve çok **temel** dağıtım için Web dağıtımı İşleyici. |
| **/U** | Kullanıcı adını belirtir. Bu, yalnızca temel kimlik doğrulaması kullanıyorsanız geçerlidir. |
| **/P** | Parolayı belirtir. Bu, yalnızca temel kimlik doğrulaması kullanıyorsanız geçerlidir. |
| **/ L** | Paket yerel IIS Express örneğine dağıtılması gerektiğini belirtir. |
| **/G** | Paket kullanılarak dağıtılır belirtir [tempAgent sağlayıcısı ayarı](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Atlarsanız **/G** bayrağı, değer varsayılan olarak **false**. |

> [!NOTE]
> Ayrıca yapı işleminin bir web paketi oluşturur. her zaman adlı bir dosya oluşturur *[Proje adı] .deploy-readme.txt* , bu dağıtım seçenekleri açıklanmaktadır.


Bu bayraklar ek olarak, Web Dağıtımı işlemi ayarları ek olarak belirtebilirsiniz *. deploy.cmd* parametreleri. Belirttiğiniz herhangi bir ek ayarı yalnızca temel alınan MSDeploy.exe komutu geçirilecek. Bu ayarlar hakkında daha fazla bilgi için bkz. [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Çalıştırarak ContactManager.Mvc web uygulama projesini bir test ortamına dağıtmak istediğiniz varsayalım *. deploy.cmd* dosya. Sınama ortamınızda Web dağıtma uzak aracı hizmetini kullanmayı açıklandığı yapılandırılmış [bir Web sunucusunu Web dağıtımı yayımlama için (Uzak Aracı) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Web uygulamasına dağıtmak için sonraki adımları tamamlamanız gerekir.

**Bir web uygulaması kullanarak dağıtılacak. deploy.cmd dosyası**

1. Derleme ve web uygulaması projesi açıklandığı paketini [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
2. Değiştirme *ContactManager.Mvc.SetParameters.xml* bölümünde anlatıldığı gibi test ortamınız için doğru parametre değerlerini içeren dosyaya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
3. Bir komut istemi penceresi açın ve dosyasının konumuna gidin *ContactManager.Mvc.deploy.cmd* dosya.
4. Bu komutu yazın ve Enter tuşuna basın:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Bu örnekte:

- **/Y** bayrağı, paketi gerçekten dağıtmak istediğiniz çalıştırması kullanmak yerine, bir deneme yapmak gösterir.
- **/M** bayrağı TESTWEB1 adlı sunucuya paketini dağıtmak istediğiniz gösterir. Web dağıtımı Uzak Aracı hizmeti paketi dağıtmak bu değerden MSDeploy.exe deneyecek http://TESTWEB1/MSDeployAgentService.
- **/A** bayrağı, NTLM kimlik doğrulaması kullanmak istediğiniz gösterir. Bu nedenle, bir kullanıcı adı ve parola belirtmeniz gerekmez.

Göstermek için kullanıldığında nasıl *. deploy.cmd* dosya dağıtım işlemini basitleştirir, ve oluşturulan çalıştırdığınızda yürütülen MSDeploy.exe komutu göz atın *ContactManager.Mvc.deploy.cmd* Yukarıda gösterilen seçenekler kullanma.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Kullanma hakkında daha fazla bilgi için *. deploy.cmd* web paketini dağıtmak için bkz. dosya [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>MSDeploy.exe kullanma

Kullanarak rağmen *. deploy.cmd* dosyası genellikle dağıtım işlemini basitleştirir, bazı durumlar vardır MSDeploy.exe doğrudan kullanılması tercih olduğunda. Örneğin:

- Web dağıtımı işleyicisi yönetici olmayan kullanıcı olarak dağıtmak istiyorsanız, kullanamazsınız *. deploy.cmd* dosya. Web dağıtımı 2.0, bir hata nedeniyle altında açıklandığı gibi budur **uç nokta konuları**.
- El ile farklı arasında geçiş yapmak istiyorsanız *SetParameters.xml* dosyaları farklı konumlarda tercih ettiğiniz MSDeploy.exe doğrudan kullanılacak.
- Birkaç MSDeploy.exe komut satırı bağımsız değişkenleri geçersiz kılmak istiyorsanız, doğrudan MSDeploy.exe kullanmayı tercih edebilirsiniz.

MSDeploy.exe kullandığınızda, üç temel bilgiler sağlamanız gerekir:

- A **– kaynak** gelen verilerinizi nereden geldiğini belirten bir parametre.
- A **– hedef** için verilerinize nerede bulunacağını gösteren parametresi.
- A **– fiil** belirten bir parametre [işlemi](https://technet.microsoft.com/library/dd568989(WS.10).aspx) gerçekleştirmek istediğiniz.

MSDeploy.exe dayanır [Web dağıtımı sağlayıcıları](https://technet.microsoft.com/library/dd569040(WS.10).aspx) kaynak ve hedef verileri işlemek için. Web dağıtımı, çok sayıda uygulama ve veri kaynakları ile çalışabilir aralığını temsil eden sağlayıcıları içerir&#x2014;Örneğin, SQL Server veritabanları, IIS web sunucuları, sertifika, Genel Derleme Önbelleği (GAC) derlemeleri için sağlayıcıları vardır çeşitli farklı yapılandırma dosyaları ve birçok diğer veri türleri. Her iki **– kaynak** parametresi ve **– hedef** parametresi biçiminde bir sağlayıcı belirtmelisiniz **– kaynak**: [*providerName*] [=*konumu*]. Bir IIS Web sitesine bir web paketi dağıtımı yapıyorsanız, bu değerleri kullanmanız gerekir:

- **– Kaynak** sağlayıcısıdır her zaman [paket](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Örneğin:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- **– Hedef** sağlayıcısıdır her zaman [otomatik](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Örneğin:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- **– Fiil** her zaman **eşitleme**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Ayrıca, çeşitli belirtmek üzere diğer ihtiyacınız [sağlayıcıya özgü ayarları](https://technet.microsoft.com/library/dd569001(WS.10).aspx) ve genel [işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Örneğin, bir hazırlama ortamına ContactManager.Mvc web uygulamasına dağıtmak istediğinizi varsayalım. Dağıtım, Web dağıtımı işleyicisi hedeflediğini ve temel kimlik doğrulaması kullanmanız gerekir. Web uygulamasına dağıtmak için sonraki adımları tamamlamanız gerekir.

**MSDeploy.exe kullanarak bir web uygulaması dağıtma**

1. Derleme ve web uygulaması projesi açıklandığı paketini [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).
2. Değiştirme *ContactManager.Mvc.SetParameters.xml* açıklandığı hazırlama ortamınız için doğru parametre değerlerini içeren dosyaya [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
3. Bir komut istemi penceresi açın ve MSDeploy.exe konumuna göz atın. Web dağıtımı V2\msdeploy.exe %PROGRAMFILES%\IIS\Microsoft genellikle budur.
4. Bu komutu yazın ve Enter tuşuna basın (dikkate satır sonları):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Bu örnekte:

- **– Kaynak** parametresinin belirttiği **paket** sağlayıcısı ve web paketi konumu belirtir.
- **– Hedef** parametresinin belirttiği **otomatik** sağlayıcısı. **ComputerName** ayarı, hedef sunucuda Web dağıtımı işleyicisi hizmeti URL'sini sağlar. **Authtype** ayarını temel kimlik doğrulaması kullanmak istediğiniz ve bu nedenle girmeniz gerektiğini gösterir bir **kullanıcıadı** ve **parola**. Son olarak, **includeAcls = "False"** ayarını hedef sunucuya kaynak web uygulamanızda erişim denetim listelerini (ACL'ler) dosyaları kopyalamak istemediğiniz gösterir.
- **– Fiil: eşitleme** bağımsız değişkeni, hedef sunucuda kaynak İçerik çoğaltmak istediğiniz gösterir.
- **– DisableLink** bağımsız değişkenleri belirtmek uygulama havuzları, sanal dizin yapılandırması ya da Güvenli Yuva Katmanı (SSL) sertifikalarını hedef sunucuda çoğaltmak istediğiniz yok. Daha fazla bilgi için [Web dağıtma bağlantı uzantıları](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- **– SetParamFile** parametresini sağlayan konumunu *SetParameters.xml* dosya.
- **– AllowUntrusted** anahtar gösteren Web dağıtımı bir güvenilen sertifika yetkilisi tarafından verilmemiş SSL sertifikalarını kabul etmelidir. Web dağıtımı işleyicisi için dağıtıyorsanız ve otomatik olarak imzalanan bir sertifika hizmeti URL'sini güvenliğini sağlamak için kullandınız, bu anahtarı'nı eklemeniz gerekir.

## <a name="automating-web-package-deployment"></a>Web paketi dağıtımı otomatikleştirme

İçinde çok sayıda kuruluş senaryoları, web paketlerinizi büyük tek adımlı veya otomatik dağıtım parçası olarak dağıtmak isteyebilirsiniz. Web paketlerinizi çalıştırarak dağıtmak seçtiğiniz bakılmaksızın *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak komutlarınızı Parametreleştirme ve bir Microsoft Build Engine (MSBuild) hedefte çağırmanıza Proje dosyası.

Kişi Yöneticisi örnek çözümde göz atın **PublishWebPackages** hedeflemek *Publish.proj* dosya. Bu hedef her biri için bir kez çalışır *. deploy.cmd* adlı bir öğe listesi tarafından tanımlanan dosyaya **PublishPackages**. Hedef için bağımsız değişken değerlerini tam bir dizi oluşturmak için özellikleri ve öğe meta verileri kullanan *. deploy.cmd* dosya ve kullandığı **Exec** komutu çalıştırmak için görev.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Örnek çözüm, genel özel proje dosyalarında giriş proje dosyası modeli daha geniş bir genel bakış için bkz: [proje dosyasını anlama](understanding-the-project-file.md) ve [derleme işlemini anlama](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Uç nokta konuları

Olup çalıştırarak web paketinizi dağıtmak bakılmaksızın *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak, bir bilgisayar adı veya dağıtımınız için hizmet uç noktası belirtmeniz gerekir.

Hedef web sunucusuna Web dağıtma uzak aracı hizmetini kullanarak dağıtım için yapılandırılmışsa, hedef olarak hedef hizmet URL'si belirtin.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Alternatif olarak, tek başına sunucu adını hedef olarak belirtebilirsiniz ve Web dağıtımı aracı uzak hizmet URL'si çıkarımlar.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Hedef web sunucusunu Web dağıtımı işleyicisi kullanarak dağıtım için yapılandırılmışsa, hedef olarak IIS Web Yönetim Hizmeti'nin (WMSvc) uç nokta adresini belirtmeniz gerekir. Varsayılan olarak, bu biçimi alır:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Herhangi birini kullanarak bu uç noktaları hedef *. deploy.cmd* dosya ya da doğrudan MSDeploy.exe. Ancak, açıklandığı gibi yönetici olmayan kullanıcı olarak, Web dağıtımı işleyicisi için dağıtmak istiyorsanız, [bir Web sunucusunu Web dağıtımı yayımlama için (Web dağıtımı işleyicisi) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), bir sorgu dizesi için hizmet uç noktası adresi eklemeniz gerekir.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Bu durum, yönetici olmayan kullanıcı IIS sunucu düzeyinde erişim yoktur çünkü; yalnızca belirli bir IIS Web sitesine erişim sahip. Zaman içinde Web yayımlama işlem hattı (Usewpp_copywebapplication), bir hata nedeniyle yazma, çalıştıramazsınız *. deploy.cmd* sorgu dizesi içeren bir uç nokta adresi kullanarak dosya. Bu senaryoda, web paketinizi doğrudan MSDeploy.exe kullanarak dağıtmanız gerekir.

> [!NOTE]
> Web dağıtımı Uzak Aracı hizmeti ve Web dağıtımı işleyicisi hakkında daha fazla bilgi için bkz. [Web dağıtımı için doğru yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Ortama özgü proje dosyalarınızı dağıtmak için bu uç noktaları için yapılandırma hakkında yönergeler için bkz. [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Kimlik doğrulama konuları

Olup çalıştırarak web paketinizi dağıtmak bakılmaksızın *. deploy.cmd* dosya veya MSDeploy.exe doğrudan kullanarak bir kimlik doğrulama türünü belirtmeniz gerekir. Web dağıtımı kabul eder, iki olası değerler: **NTLM** veya **temel**. Temel kimlik doğrulaması belirtirseniz, aynı zamanda bir kullanıcı adı ve parola sağlamanız gerekir. Bir kimlik doğrulaması türünü seçtiğinizde dikkat etmeniz gereken çeşitli faktörler vardır:

- Web dağıtımı uzak aracısının hizmete dağıtıyorsanız, NTLM kimlik doğrulaması kullanmanız gerekir. Uzak Aracı hizmeti, temel kimlik doğrulaması kimlik bilgileri kabul etmez.
- Web dağıtımı işleyicisi için dağıtıyorsanız, NTLM veya temel kimlik doğrulaması kullanabilirsiniz. Temel kimlik doğrulaması varsayılan ayardır. Temel kimlik doğrulaması kullanıcı adları ve parolalar düz metin olarak aktarılan kullanır ancak Web dağıtımı işleyicisi her zaman SSL şifrelemesi kullanır gibi kimlik bilgilerinizi korunur.
- Bir veritabanını web paketinizi içerir ve ayrı makineler web sunucusu ve veritabanı sunucusu olan, NTLM kimlik doğrulaması için kullanılan veritabanını dağıtma mümkün olmayacaktır [NTLM "çift atlama" sınırlama](https://go.microsoft.com/?linkid=9805120). SQL Server kimlik bilgileri dağıtım bağlantı dizenizi kullanabilir veya Web dağıtımı için temel kimlik doğrulaması kimlik bilgilerini sağlamanız gerekir. Bu sorunu daha ayrıntılı olarak açıklanan [Kurumsal ortamlara üyelik veritabanları dağıtma](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Sonuç

Web paketi çalıştırarak ya da dağıtımı bu konuda açıklanan *. deploy.cmd* dosya veya doğrudan MSDeploy.exe kullanarak. Bu, her bir yaklaşıma uygun olabilir ve nasıl Parametreleştirme ve daha büyük bir tek adımlı ya da otomatikleştirilmiş yapı işleminin bir parçası bir dağıtım komutu Çalıştır açıklanan açıklanmıştır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Nasıl oluşturulacağı ve bir web dağıtım paketi Parametreleştirme ilgili yönergeler için bkz [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) ve [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md). Derleme ve Team Foundation Server (TFS) örneğinden web paketleri dağıtma konusunda rehberlik için bkz [otomatik Web dağıtımı için Team Foundation Server yapılandırma](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Özelleştirme ve dağıtım işlemi sorun giderme hakkında daha fazla bilgi için bkz: [hariç dosya ve klasörleri dağıtımdan](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Önceki](configuring-parameters-for-web-package-deployment.md)
> [İleri](deploying-database-projects.md)
